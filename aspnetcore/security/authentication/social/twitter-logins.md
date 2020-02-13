---
title: Настройка внешнего входа в Twitter с помощью ASP.NET Core
author: rick-anderson
description: В этом руководстве демонстрируется интеграция аутентификации пользователя учетной записи Twitter с существующим ASP.NET Core приложением.
ms.author: riande
ms.custom: mvc
ms.date: 12/06/2019
monikerRange: '>= aspnetcore-3.0'
uid: security/authentication/twitter-logins
ms.openlocfilehash: 4710c033018710ce3620f8d7221ae2253b2c0b69
ms.sourcegitcommit: 85564ee396c74c7651ac47dd45082f3f1803f7a2
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 02/12/2020
ms.locfileid: "77172523"
---
# <a name="twitter-external-sign-in-setup-with-aspnet-core"></a>Настройка внешнего входа в Twitter с помощью ASP.NET Core

Авторы: [Валерий Новицкий](https://github.com/01binary) (Valeriy Novytskyy) и [Рик Андерсон](https://twitter.com/RickAndMSFT) (Rick Anderson)

В этом примере показано, как разрешить пользователям [входить с помощью учетной записи Twitter](https://dev.twitter.com/web/sign-in/desktop-browser) , используя пример проекта ASP.NET Core 3,0, созданный на [предыдущей странице](xref:security/authentication/social/index).

## <a name="create-the-app-in-twitter"></a>Создание приложения в Twitter

* Добавьте в проект пакет NuGet [Microsoft. AspNetCore. Authentication. Twitter](https://www.nuget.org/packages/Microsoft.AspNetCore.Authentication.Twitter/3.0.0) .

* Перейдите к [https://apps.twitter.com/](https://apps.twitter.com/) и выполните вход. Если у вас еще нет учетной записи Twitter, используйте ссылку **[зарегистрироваться сейчас](https://twitter.com/signup)** , чтобы создать ее.

* Выберите **Create an app** (Создать приложение). Укажите **имя приложения**, **Описание приложения** и универсальный код ресурса (URI) общедоступного **веб-сайта** (это может быть временным, пока не будет зарегистрировано доменное имя):

* Установите флажок **включить вход с помощью Twitter** .

* Microsoft. AspNetCore. Identity требует, чтобы по умолчанию у пользователей был адрес электронной почты. Перейдите на вкладку **разрешения** , нажмите кнопку " **изменить** " и установите флажок " **запросить адрес электронной почты у пользователей**".

* Введите универсальный код ресурса (URI) для разработки `/signin-twitter` добавлен в поле **URL-адреса обратного вызова** (например: `https://webapp128.azurewebsites.net/signin-twitter`). Схема проверки подлинности Twitter, настроенная далее в этом примере, автоматически обрабатывает запросы на `/signin-twitter` маршруте для реализации потока OAuth.

  > [!NOTE]
  > Сегмент URI `/signin-twitter` задается в качестве обратного вызова по умолчанию для поставщика проверки подлинности Twitter. URI обратного вызова по умолчанию можно изменить при настройке по промежуточного слоя проверки подлинности Twitter с помощью унаследованного свойства [ремотеаусентикатионоптионс. каллбаккпас](/dotnet/api/microsoft.aspnetcore.authentication.remoteauthenticationoptions.callbackpath) класса [твиттероптионс](/dotnet/api/microsoft.aspnetcore.authentication.twitter.twitteroptions) .

* Заполните оставшуюся часть формы и выберите **создать**. Отобразятся сведения о новом приложении:

## <a name="storing-twitter-consumer-api-key-and-secret"></a>Хранение ключа и секрета API потребителя Twitter

Выполните следующие команды для безопасного хранения `ClientId` и `ClientSecret` с помощью [диспетчера секретов](xref:security/app-secrets):

```dotnetcli
dotnet user-secrets set Authentication:Twitter:ConsumerAPIKey <Key>
dotnet user-secrets set Authentication:Twitter:ConsumerSecret <Secret>
```

Свяжите конфиденциальные параметры, такие как Twitter `Consumer Key` и `Consumer Secret` в конфигурации приложения с помощью [диспетчера секретов](xref:security/app-secrets). Для целей этого примера назовите маркеры `Authentication:Twitter:ConsumerKey` и `Authentication:Twitter:ConsumerSecret`.

Эти маркеры можно найти на вкладке **ключи и маркеры доступа** после создания нового приложения Twitter.

## <a name="configure-twitter-authentication"></a>Настройка проверки подлинности Twitter

Добавьте службу Twitter в метод `ConfigureServices` в файле *Startup.CS* :

[!code-csharp[](~/security/authentication/social/social-code/3.x/StartupTwitter3x.cs?name=snippet&highlight=10-15)]

[!INCLUDE [default settings configuration](includes/default-settings.md)]

[!INCLUDE[](includes/chain-auth-providers.md)]

Дополнительные сведения о параметрах конфигурации, поддерживаемых проверкой подлинности в Twitter, см. в справочнике по API [твиттероптионс](/dotnet/api/microsoft.aspnetcore.builder.twitteroptions) . Это может использоваться для запроса различные сведения о пользователе.

## <a name="sign-in-with-twitter"></a>Вход с помощью Twitter

Запустите приложение и выберите **Вход**. Появится возможность входа с помощью Twitter:

При щелчке **Twitter** в Twitter перенаправления для проверки подлинности:

После ввода учетных данных Twitter вы будете перенаправлены на веб-сайт, на котором можно задать свой адрес электронной почты.

Теперь вы выполнили вход с использованием учетных данных Twitter:

[!INCLUDE[Forward request information when behind a proxy or load balancer section](includes/forwarded-headers-middleware.md)]

## <a name="troubleshooting"></a>Устранение неполадок

* **Только ASP.NET Core 2. x:** Если удостоверение не настроено путем вызова `services.AddIdentity` в `ConfigureServices`, попытка проверки подлинности приведет к появлению *исключения ArgumentException: необходимо указать параметр "сигнинсчеме"* . Шаблон проекта, используемый в этом образце, гарантирует, что это будет сделано.
* Если база данных сайта не была создана с применением первоначальной миграции, то при обработке ошибки запроса возникнет *Ошибка операции с базой данных* . Выберите **Применить миграции** , чтобы создать базу данных и обновить, чтобы продолжить выполнение после ошибки.

## <a name="next-steps"></a>Дальнейшие действия

* В этой статье показано, как можно пройти проверку подлинности в Twitter. Аналогичный подход можно использовать для проверки подлинности с другими поставщиками, перечисленными на [предыдущей странице](xref:security/authentication/social/index).

* После публикации веб-сайта в веб-приложение Azure необходимо сбросить `ConsumerSecret` на портале разработчика Twitter.

* Задайте `Authentication:Twitter:ConsumerKey` и `Authentication:Twitter:ConsumerSecret` в качестве параметров приложения в портал Azure. Система конфигурации предназначена для чтения разделов из переменных среды.
