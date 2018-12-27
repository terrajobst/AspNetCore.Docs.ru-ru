---
title: Настройка внешней учетной записи Facebook в ASP.NET Core
author: rick-anderson
description: В этом учебнике показано интеграцию Facebook учетной записи пользователя и проверки подлинности в существующее приложение ASP.NET Core.
ms.author: riande
ms.custom: mvc, seodec18
ms.date: 12/18/2018
uid: security/authentication/facebook-logins
ms.openlocfilehash: 66f895c7c8dcc00d991c0ea57535f2ed56431a77
ms.sourcegitcommit: 3e94d192b2ed9409fe72e3735e158b333354964c
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 12/21/2018
ms.locfileid: "53735782"
---
# <a name="facebook-external-login-setup-in-aspnet-core"></a>Настройка внешней учетной записи Facebook в ASP.NET Core

Авторы: [Валерий Новицкий](https://github.com/01binary) (Valeriy Novytskyy) и [Рик Андерсон](https://twitter.com/RickAndMSFT) (Rick Anderson)

Этом руководстве показано, как включить пользователей выполнить вход с использованием учетной записи Facebook, используя образец проекта ASP.NET Core 2.0, созданные на [предыдущую страницу](xref:security/authentication/social/index). Мы начнем с создания код приложения Facebook, следуя [официальный действия](https://developers.facebook.com).

## <a name="create-the-app-in-facebook"></a>Создать приложение в Facebook

* Перейдите к [приложений разработчиков для Facebook](https://developers.facebook.com/apps/) странице и войдите в систему. Если у вас нет учетной записи Facebook, используйте **зарегистрироваться для Facebook** ссылку на странице входа, чтобы создать его.

* Коснитесь **добавить новое приложение** кнопки в правом верхнем углу, чтобы создать новый идентификатор приложения.

   ![Facebook для портала разработчиков открыть в Microsoft Edge](index/_static/FBMyApps.png)

* Заполните форму и коснитесь **создать идентификатор приложения** кнопки.

  ![Создание формы новый идентификатор приложения](index/_static/FBNewAppId.png)

* На **выбрать продукт** щелкните **Настройка** на **имени для входа Facebook** карты.

  ![Страница установки продукта](index/_static/FBProductSetup.png)

* **Быстрого запуска** будет запущен мастер с **выберите платформу** первая страница. Сейчас пропустить мастера, нажав кнопку **параметры** ссылку в меню слева:

  ![Пропустить быстрый запуск](index/_static/FBSkipQuickStart.png)

* Появится **клиентские настройки OAuth** страницы:

  ![Параметры OAuth клиента](index/_static/FBOAuthSetup.png)

* Введите URI разработки с */signin-facebook* добавляется в **допустимый URI перенаправления OAuth** поле (например: `https://localhost:44320/signin-facebook`). Проверка подлинности Facebook, Настройка описывается далее в этом руководстве автоматически будет обрабатывать запросы на */signin-facebook* маршрут, чтобы реализовать поток OAuth.

> [!NOTE]
> URI */signin-facebook* задан в качестве обратного вызова по умолчанию поставщика проверки подлинности Facebook. URI обратного вызова по умолчанию можно изменить при настройке по промежуточного слоя проверки подлинности Facebook с помощью наследуемого [RemoteAuthenticationOptions.CallbackPath](/dotnet/api/microsoft.aspnetcore.authentication.remoteauthenticationoptions.callbackpath) свойство [FacebookOptions](/dotnet/api/microsoft.aspnetcore.authentication.facebook.facebookoptions) класс.

* Нажмите кнопку **сохранить изменения**.

* Нажмите кнопку **параметры** > **основные** ссылку в левой области навигации.

  На этой странице, запомните или запишите вашей `App ID` и `App Secret`. Оба сертификата в приложении ASP.NET Core будет добавлена в следующем разделе:

* При развертывании на сайте необходимо пересмотреть **имени для входа Facebook** установки: страница и зарегистрировать новый открытый универсальный код Ресурса.

## <a name="store-facebook-app-id-and-app-secret"></a>Идентификатор приложения Facebook Store и секрет приложения

Связать конфиденциальные параметры, такие как Facebook `App ID` и `App Secret` для конфигурации приложения с помощью [Secret Manager](xref:security/app-secrets). В целях этого учебника назовите токены `Authentication:Facebook:AppId` и `Authentication:Facebook:AppSecret`.

Выполните следующие команды, чтобы обеспечить безопасное хранение `App ID` и `App Secret` с помощью диспетчера секретов:

```console
dotnet user-secrets set Authentication:Facebook:AppId <app-id>
dotnet user-secrets set Authentication:Facebook:AppSecret <app-secret>
```

## <a name="configure-facebook-authentication"></a>Настройка проверки подлинности Facebook

::: moniker range=">= aspnetcore-2.0"

Добавьте службу Facebook в `ConfigureServices` метод в *Startup.cs* файла:

```csharp
services.AddDefaultIdentity<IdentityUser>()
        .AddDefaultUI(UIFramework.Bootstrap4)
        .AddEntityFrameworkStores<ApplicationDbContext>();

services.AddAuthentication().AddFacebook(facebookOptions =>
{
    facebookOptions.AppId = Configuration["Authentication:Facebook:AppId"];
    facebookOptions.AppSecret = Configuration["Authentication:Facebook:AppSecret"];
});
```

[!INCLUDE [default settings configuration](includes/default-settings.md)]

[!INCLUDE[](includes/chain-auth-providers.md)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

Установка [Microsoft.AspNetCore.Authentication.Facebook](https://www.nuget.org/packages/Microsoft.AspNetCore.Authentication.Facebook) пакета.

* Установить этот пакет с помощью Visual Studio 2017, щелкните правой кнопкой мыши проект и выберите **управление пакетами NuGet**.
* Чтобы установить с помощью интерфейса командной строки .NET Core, выполните следующую команду в каталоге проекта:

   `dotnet add package Microsoft.AspNetCore.Authentication.Facebook`

Добавьте по промежуточного слоя Facebook в `Configure` метод в *Startup.cs* файла:

```csharp
app.UseFacebookAuthentication(new FacebookOptions()
{
    AppId = Configuration["Authentication:Facebook:AppId"],
    AppSecret = Configuration["Authentication:Facebook:AppSecret"]
});
```

::: moniker-end

См. в разделе [FacebookOptions](/dotnet/api/microsoft.aspnetcore.builder.facebookoptions) Справочник по API, Дополнительные сведения о параметрах конфигурации, поддерживаемых проверкой подлинности Facebook. Параметры конфигурации может использоваться для:

* Запросить различные сведения о пользователе.
* Добавление аргументов строки запроса, настраивать параметры имени входа.

## <a name="sign-in-with-facebook"></a>Вход с помощью Facebook

Запустите приложение и нажмите кнопку **вход**. Отображается параметр выполнить вход с использованием Facebook.

![Веб-приложение: Пользователь не прошел проверку подлинности](index/_static/DoneFacebook.png)

Когда вы щелкаете **Facebook**, вы будете перенаправлены на Facebook для проверки подлинности:

![Страница проверки подлинности Facebook](index/_static/FBLogin.png)

Проверка подлинности Facebook запрашивает открытый профиль и адрес электронной почты по умолчанию:

![Экран согласия страницу проверки подлинности Facebook](index/_static/FBLoginDone.png)

После ввода учетных данных Facebook вы будете перенаправлены обратно на сайт, где вы можете задать свой адрес электронной почты.

Теперь вы вошли с использованием учетных данных Facebook:

![Веб-приложение: Пользователь прошел проверку подлинности](index/_static/Done.png)

[!INCLUDE[Forward request information when behind a proxy or load balancer section](includes/forwarded-headers-middleware.md)]

## <a name="troubleshooting"></a>Устранение неполадок

* **ASP.NET Core 2.x только:** Если удостоверение не настроена, вызвав `services.AddIdentity` в `ConfigureServices`, пытающиеся выполнить проверку подлинности приведет к *ArgumentException: Необходимо указать параметр «SignInScheme»*. Шаблон проекта, используемый в этом руководстве гарантирует, что это будет сделано.
* Если база данных сайта не был создан путем применения первоначальной миграции, вы получаете *сбой операции из базы данных при обработке запроса* ошибки. Коснитесь **применить миграции** для создания базы данных и обновить, чтобы продолжить выполнение после ошибки.

## <a name="next-steps"></a>Следующие шаги

* Добавить [Microsoft.AspNetCore.Authentication.Facebook](https://www.nuget.org/packages/Microsoft.AspNetCore.Authentication.Facebook) пакет NuGet в проект для более сложных сценариев проверки подлинности Facebook. Этот пакет не обязательно должна Интегрируйте функции Facebook внешней учетной записи с вашим приложением. 

* В этой статье объясняется, как можно выполнить проверку подлинности с помощью Facebook. Можно выполнить аналогичный подход для проверки подлинности с помощью других поставщиков, перечисленных на [предыдущую страницу](xref:security/authentication/social/index).

* После публикации веб-сайт веб-приложение Azure, необходимо сбросить `AppSecret` на портале разработчика Facebook.

* Задайте `Authentication:Facebook:AppId` и `Authentication:Facebook:AppSecret` как параметры приложения на портале Azure. Система конфигурации предназначена для чтения разделов из переменных среды.
