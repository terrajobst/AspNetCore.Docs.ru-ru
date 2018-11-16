---
title: Настройка внешней учетной записи учетной записи Майкрософт с помощью ASP.NET Core
author: rick-anderson
description: В этом учебнике показано интеграции проверки подлинности пользователя учетной записи Майкрософт в существующее приложение ASP.NET Core.
ms.author: riande
ms.custom: mvc
ms.date: 11/11/2018
uid: security/authentication/microsoft-logins
ms.openlocfilehash: 89969370cea66b7b6632f1b0be59e135767c831e
ms.sourcegitcommit: 09bcda59a58019fdf47b2db5259fe87acf19dd38
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 11/15/2018
ms.locfileid: "51708404"
---
# <a name="microsoft-account-external-login-setup-with-aspnet-core"></a>Настройка внешней учетной записи учетной записи Майкрософт с помощью ASP.NET Core

Авторы: [Валерий Новицкий](https://github.com/01binary) (Valeriy Novytskyy) и [Рик Андерсон](https://twitter.com/RickAndMSFT) (Rick Anderson)

Этом руководстве показано, как включить пользователей выполнить вход с использованием своей учетной записью Майкрософт, используя образец проекта ASP.NET Core 2.0, созданные на [предыдущую страницу](xref:security/authentication/social/index).

## <a name="create-the-app-in-microsoft-developer-portal"></a>Создание приложения на портале разработчика Майкрософт

* Перейдите к [ https://apps.dev.microsoft.com ](https://apps.dev.microsoft.com) и создать или войти в учетную запись Майкрософт:

![Диалоговое окно входа](index/_static/MicrosoftDevLogin.png)

Если у вас нет учетной записи Майкрософт, коснитесь  **[создайте ее!](https://signup.live.com/signup?wa=wsignin1.0&rpsnv=13&ct=1478151035&rver=6.7.6643.0&wp=SAPI_LONG&wreply=https%3a%2f%2fapps.dev.microsoft.com%2fLoginPostBack&id=293053&aadredir=1&contextid=D70D4F21246BAB50&bk=1478151036&uiflavor=web&uaid=f0c3de863a914c358b8dc01b1ff49e85&mkt=EN-US&lc=1033&lic=1)** После входа вы будете перенаправлены на **моих приложений** страницы:

![Портал разработчиков Microsoft открыть в Microsoft Edge](index/_static/MicrosoftDev.png)

* Коснитесь **Добавление веб-приложения** в верхнем правом углу, а затем введите ваш **имя_приложения** и **контактный адрес электронной почты**:

![Диалоговое окно создания регистрации приложения](index/_static/MicrosoftDevAppCreate.png)

* Для целей данного учебника, снимите **руководство по настройке** "флажок".

* Коснитесь **создать** продолжать **регистрации** страницы. Укажите **имя** и обратите внимание на значение **идентификатор приложения**, который можно использовать в качестве `ClientId` далее в этом руководстве:

![Страница регистрации](index/_static/MicrosoftDevAppReg.png)

* Коснитесь **Добавление платформы** в **платформ** и выберите **Web** платформы:

![Добавить диалоговое окно платформы](index/_static/MicrosoftDevAppPlatform.png)

* В новом **Web** платформы введите свой URL-адрес разработки с `/signin-microsoft` добавляется в **URL-адреса перенаправления** поле (например: `https://localhost:44320/signin-microsoft`). Схема проверки подлинности Майкрософт, Настройка описывается далее в этом руководстве автоматически будет обрабатывать запросы на `/signin-microsoft` маршрута, чтобы реализовать поток OAuth:

![Веб-платформы раздел](index/_static/MicrosoftRedirectUri.png)

> [!NOTE]
> Сегмент URI `/signin-microsoft` задан в качестве обратного вызова по умолчанию поставщика проверки подлинности Майкрософт. URI обратного вызова по умолчанию можно изменить при настройке по промежуточного слоя проверки подлинности Майкрософт с помощью наследуемого [RemoteAuthenticationOptions.CallbackPath](/dotnet/api/microsoft.aspnetcore.authentication.remoteauthenticationoptions.callbackpath) свойство [MicrosoftAccountOptions](/dotnet/api/microsoft.aspnetcore.authentication.microsoftaccount.microsoftaccountoptions) класса.

* Коснитесь **Добавление URL-адреса** чтобы URL-адрес был добавлен.

* Заполните все остальные параметры приложения, при необходимости и коснитесь **Сохранить** в нижней части страницы, чтобы сохранить изменения в конфигурации приложения.

* При развертывании на сайте необходимо будет пересмотреть **регистрации** странице и задайте новый общедоступный URL-адрес.

## <a name="store-microsoft-application-id-and-password"></a>Store Microsoft Application Id и пароль

* Примечание `Application Id` на **регистрации** страницы.

* Коснитесь **создать новый пароль** в **секретов приложения** раздел. Откроется окно, где можно скопировать пароль приложения:

![Диалоговое окно создания пароля, созданных](index/_static/MicrosoftDevPassword.png)

Связать конфиденциальные параметры, такие как Microsoft `Application ID` и `Password` для конфигурации приложения с помощью [Secret Manager](xref:security/app-secrets). В целях этого учебника назовите токены `Authentication:Microsoft:ApplicationId` и `Authentication:Microsoft:Password`.

## <a name="configure-microsoft-account-authentication"></a>Настройка проверки подлинности учетной записи Майкрософт

Шаблон проекта, используемый в этом руководстве гарантирует, что [Microsoft.AspNetCore.Authentication.MicrosoftAccount](https://www.nuget.org/packages/Microsoft.AspNetCore.Authentication.MicrosoftAccount) пакет уже установлен.

* Установить этот пакет с помощью Visual Studio 2017, щелкните правой кнопкой мыши проект и выберите **управление пакетами NuGet**.
* Чтобы установить с помощью интерфейса командной строки .NET Core, выполните следующую команду в каталоге проекта:

   `dotnet add package Microsoft.AspNetCore.Authentication.MicrosoftAccount`

::: moniker range=">= aspnetcore-2.0"

Добавление службы учетной записи Майкрософт в `ConfigureServices` метод в *Startup.cs* файла:

```csharp
services.AddIdentity<ApplicationUser, IdentityRole>()
        .AddEntityFrameworkStores<ApplicationDbContext>()
        .AddDefaultTokenProviders();

services.AddAuthentication().AddMicrosoftAccount(microsoftOptions =>
{
    microsoftOptions.ClientId = Configuration["Authentication:Microsoft:ApplicationId"];
    microsoftOptions.ClientSecret = Configuration["Authentication:Microsoft:Password"];
});
```

[!INCLUDE [default settings configuration](includes/default-settings.md)]

[!INCLUDE[](includes/chain-auth-providers.md)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

Добавьте по промежуточного слоя учетной записи Майкрософт в `Configure` метод в *Startup.cs* файла:

```csharp
app.UseMicrosoftAccountAuthentication(new MicrosoftAccountOptions()
{
    ClientId = Configuration["Authentication:Microsoft:ApplicationId"],
    ClientSecret = Configuration["Authentication:Microsoft:Password"]
});
```

::: moniker-end

Несмотря на то, что терминология, используемая на портале разработчиков Майкрософт называет эти маркеры `ApplicationId` и `Password`, они виде `ClientId` и `ClientSecret` в конфигурацию API.

См. в разделе [MicrosoftAccountOptions](/dotnet/api/microsoft.aspnetcore.builder.microsoftaccountoptions) Справочник по API, Дополнительные сведения о параметрах конфигурации, поддерживается проверка подлинности учетной записи Майкрософт. Это может использоваться для запроса различные сведения о пользователе.

## <a name="sign-in-with-microsoft-account"></a>Войдите с помощью учетной записи Майкрософт

Запустите приложение и нажмите кнопку **вход**. Появится возможность входа с учетной записью Майкрософт:

![Веб-приложение журнала на странице: пользователь не прошел проверку подлинности](index/_static/DoneMicrosoft.png)

Если щелкнуть on Microsoft, вы будете перенаправлены в корпорацию Майкрософт для проверки подлинности. После входа под учетной записью Майкрософт (если это не сделано) вам будет предложено разрешить приложению доступ к вашим сведениям:

![Диалоговое окно проверки подлинности Майкрософт](index/_static/MicrosoftLogin.png)

Коснитесь **Да** и вы будете перенаправлены обратно на веб-узел, где вы можете задать свой адрес электронной почты.

Теперь вы вошли с использованием учетных данных Майкрософт:

![Веб-приложения: пользователь прошел проверку подлинности](index/_static/Done.png)

[!INCLUDE[Forward request information when behind a proxy or load balancer section](includes/forwarded-headers-middleware.md)]

## <a name="troubleshooting"></a>Устранение неполадок

* Если поставщик учетной записи Майкрософт вы будете перенаправлены к странице ошибки входа, обратите внимание, ошибка заголовок и описание параметров строки запроса сразу после `#` (хэштег) в Uri.

  Несмотря на то, что сообщение об ошибке кажется указывают на проблему с использованием проверки подлинности, наиболее распространенной причиной является Uri, не соответствующих ни одному из приложения **URI перенаправления** для **Web** платформы .
* **ASP.NET Core 2.x только:** Если удостоверение не настроена, вызвав `services.AddIdentity` в `ConfigureServices`, пытающиеся выполнить проверку подлинности приведет к *ArgumentException: необходимо указать параметр «SignInScheme»*. Шаблон проекта, используемый в этом руководстве гарантирует, что это будет сделано.
* Если база данных сайта не был создан путем применения первоначальной миграции, вы получите *сбой операции из базы данных при обработке запроса* ошибки. Коснитесь **применить миграции** для создания базы данных и обновить, чтобы продолжить выполнение после ошибки.

## <a name="next-steps"></a>Следующие шаги

* В этой статье объясняется, как можно выполнить проверку подлинности с корпорацией Майкрософт. Можно выполнить аналогичный подход для проверки подлинности с помощью других поставщиков, перечисленных на [предыдущую страницу](xref:security/authentication/social/index).

* После публикации веб-сайт веб-приложение Azure, необходимо создать новый `Password` на портале разработчика Microsoft.

* Задайте `Authentication:Microsoft:ApplicationId` и `Authentication:Microsoft:Password` как параметры приложения на портале Azure. Система конфигурации предназначена для чтения разделов из переменных среды.
