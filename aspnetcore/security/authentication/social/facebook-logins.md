---
title: Настройка внешней учетной записи Facebook в ASP.NET Core
author: rick-anderson
description: Руководство с примерами кода, демонстрирующими интеграцию аутентификации пользователя с учетной записью Facebook с существующим ASP.NET Core приложением.
ms.author: riande
ms.custom: seoapril2019, mvc, seodec18
ms.date: 03/19/2020
monikerRange: '>= aspnetcore-3.0'
uid: security/authentication/facebook-logins
ms.openlocfilehash: bb26a27f026e744c7d4925aa2281bf0625fff8a2
ms.sourcegitcommit: 9b6e7f421c243963d5e419bdcfc5c4bde71499aa
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/21/2020
ms.locfileid: "79989784"
---
# <a name="facebook-external-login-setup-in-aspnet-core"></a>Настройка внешней учетной записи Facebook в ASP.NET Core

Авторы: [Валерий Новицкий](https://github.com/01binary) (Valeriy Novytskyy) и [Рик Андерсон](https://twitter.com/RickAndMSFT) (Rick Anderson)

В этом учебнике с примерами кода показано, как включить вход пользователей с помощью учетной записи Facebook, используя пример проекта ASP.NET Core 3,0, созданный на [предыдущей странице](xref:security/authentication/social/index). Начнем с создания идентификатора приложения Facebook, следуя [официальным действиям](https://developers.facebook.com).

## <a name="create-the-app-in-facebook"></a>Создать приложение в Facebook

* Добавьте в проект пакет NuGet [Microsoft. AspNetCore. Authentication. Facebook](https://www.nuget.org/packages/Microsoft.AspNetCore.Authentication.Facebook) .

* Перейдите на страницу [приложения Facebook Developers](https://developers.facebook.com/apps/) и выполните вход. Если у вас еще нет учетной записи Facebook, используйте ссылку **зарегистрироваться для Facebook** на странице входа, чтобы создать ее.  После получения учетной записи Facebook следуйте инструкциям по регистрации в качестве разработчика Facebook.

* В меню **Мои приложения** выберите пункт **создать приложение** , чтобы создать новый идентификатор приложения.

   ![Facebook для портала разработчиков открыть в Microsoft Edge](index/_static/FBMyApps.png)

* Заполните форму и нажмите кнопку **CREATE App ID (создать идентификатор приложения** ).

  ![Создание формы новый идентификатор приложения](index/_static/FBNewAppId.png)

* В карточке нового приложения выберите **Добавить продукт**.  На карточке **входа Facebook** щелкните **настроить** . 

  ![Страница установки продукта](index/_static/FBProductSetup.png)

* Мастер **быстрого** запуска запускается с параметром **выбрать платформу** в качестве первой страницы. Пропустите мастер, щелкнув ссылку **Параметры** **входа Facebook** в меню в левом нижнем углу экрана:

  ![Пропустить быстрый запуск](index/_static/FBSkipQuickStart.png)

* Отобразится страница **параметров OAuth клиента** :

  ![Параметры OAuth клиента](index/_static/FBOAuthSetup.png)

* Введите универсальный код ресурса (URI) для разработки с */сигнин-фацебук* , добавленным в **допустимое поле URI перенаправления OAuth** (например: `https://localhost:44320/signin-facebook`). Проверка подлинности Facebook, настроенная далее в этом руководстве, автоматически обрабатывает запросы по маршруту */сигнин-фацебук* для реализации потока OAuth.

> [!NOTE]
> URI */сигнин-фацебук* задается в качестве обратного вызова по умолчанию для поставщика проверки подлинности Facebook. URI обратного вызова по умолчанию можно изменить при настройке по промежуточного слоя проверки подлинности Facebook с помощью унаследованного свойства [ремотеаусентикатионоптионс. каллбаккпас](/dotnet/api/microsoft.aspnetcore.authentication.remoteauthenticationoptions.callbackpath) класса [фацебукоптионс](/dotnet/api/microsoft.aspnetcore.authentication.facebook.facebookoptions) .

* Нажмите кнопку **Сохранить изменения**.

* В левой области навигации щелкните **параметры** > **Обычная** ссылка.

  На этой странице запишите `App ID` и `App Secret`. Оба сертификата в приложении ASP.NET Core будет добавлена в следующем разделе:

* При развертывании сайта необходимо повторно посетить страницу настройки **имени входа Facebook** и зарегистрировать новый общедоступный URI.

## <a name="store-the-facebook-app-id-and-secret"></a>Хранение идентификатора и секрета приложения Facebook

Храните конфиденциальные параметры, такие как идентификатор приложения Facebook и значения секрета, с помощью [диспетчера секретов](xref:security/app-secrets). Для этого примера выполните следующие действия.

1. Инициализируйте проект для хранения секретных данных согласно инструкциям в разделе [Включение секретного хранилища](xref:security/app-secrets#enable-secret-storage).
1. Храните конфиденциальные параметры в локальном хранилище секретов с секретными ключами `Authentication:Facebook:AppId` и `Authentication:Facebook:AppSecret`:

    ```dotnetcli
    dotnet user-secrets set "Authentication:Facebook:AppId" "<app-id>"
    dotnet user-secrets set "Authentication:Facebook:AppSecret" "<app-secret>"
    ```

[!INCLUDE[](~/includes/environmentVarableColon.md)]

## <a name="configure-facebook-authentication"></a>Настройка проверки подлинности Facebook

Добавьте службу Facebook в метод `ConfigureServices` в файле *Startup.CS* :

```csharp
services.AddAuthentication().AddFacebook(facebookOptions =>
{
    facebookOptions.AppId = Configuration["Authentication:Facebook:AppId"];
    facebookOptions.AppSecret = Configuration["Authentication:Facebook:AppSecret"];
});
```

[!INCLUDE [default settings configuration](includes/default-settings.md)]

[!INCLUDE[](includes/chain-auth-providers.md)]

Дополнительные сведения о параметрах конфигурации, поддерживаемых проверкой подлинности Facebook, см. в справочнике по API [фацебукоптионс](/dotnet/api/microsoft.aspnetcore.builder.facebookoptions) . Параметры конфигурации может использоваться для:

* Запросить различные сведения о пользователе.
* Добавление аргументов строки запроса, настраивать параметры имени входа.

## <a name="sign-in-with-facebook"></a>Вход с помощью Facebook

Запустите приложение и нажмите кнопку **войти**. Отображается параметр выполнить вход с использованием Facebook.

![Веб-приложения: пользователь не прошел проверку подлинности](index/_static/DoneFacebook.png)

Если щелкнуть **Facebook**, вы будете перенаправлены на Facebook для проверки подлинности:

![Страница проверки подлинности Facebook](index/_static/FBLogin.png)

Проверка подлинности Facebook запрашивает открытый профиль и адрес электронной почты по умолчанию:

![Экран согласия страницу проверки подлинности Facebook](index/_static/FBLoginDone.png)

После ввода учетных данных Facebook вы будете перенаправлены обратно на сайт, где вы можете задать свой адрес электронной почты.

Теперь вы вошли с использованием учетных данных Facebook:

![Веб-приложения: пользователь прошел проверку подлинности](index/_static/Done.png)

[!INCLUDE[Forward request information when behind a proxy or load balancer section](includes/forwarded-headers-middleware.md)]

## <a name="troubleshooting"></a>Диагностика

* **Только ASP.NET Core 2. x:** Если удостоверение не настроено путем вызова `services.AddIdentity` в `ConfigureServices`, попытка проверки подлинности приведет к появлению *исключения ArgumentException: необходимо указать параметр "сигнинсчеме"* . Шаблон проекта, используемый в этом руководстве гарантирует, что это будет сделано.
* Если база данных сайта не была создана путем применения первоначальной миграции, то при обработке ошибки запроса возникнет *Ошибка операции с базой данных* . Выберите **Применить миграции** , чтобы создать базу данных и обновить, чтобы продолжить выполнение после ошибки.

## <a name="next-steps"></a>Следующие шаги

* В этой статье объясняется, как можно выполнить проверку подлинности с помощью Facebook. Аналогичный подход можно использовать для проверки подлинности с другими поставщиками, перечисленными на [предыдущей странице](xref:security/authentication/social/index).

* После публикации веб-сайта в веб-приложение Azure необходимо сбросить `AppSecret` на портале разработчика Facebook.

* Задайте `Authentication:Facebook:AppId` и `Authentication:Facebook:AppSecret` в качестве параметров приложения в портал Azure. Система конфигурации предназначена для чтения разделов из переменных среды.
