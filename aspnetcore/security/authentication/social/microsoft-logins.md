---
title: "Программа установки внешнее имя входа учетной записи Майкрософт"
author: rick-anderson
description: 
keywords: ASP.NET Core
ms.author: riande
manager: wpickett
ms.date: 08/24/2017
ms.topic: article
ms.assetid: 66DB4B94-C78C-4005-BA03-3D982B87C268
ms.technology: aspnet
ms.prod: asp.net-core
uid: security/authentication/microsoft-logins
ms.openlocfilehash: cc2d119ae2f7cf06ed0ea4b8a91fd8fa15804468
ms.sourcegitcommit: fb518f856f31fe53c09196a13309eacb85b37a22
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 09/08/2017
---
# <a name="configuring-microsoft-account-authentication"></a>Настройка проверки подлинности учетной записи Майкрософт

<a name=security-authentication-microsoft-logins></a>

По [Valeriy Novytskyy](https://github.com/01binary) и [Рик Андерсон](https://twitter.com/RickAndMSFT)

Этого учебника показано, как предоставить пользователям возможность войти в учетную запись Майкрософт с помощью ASP.NET 2.0 основной пример проекта создан на [предыдущую страницу](index.md).

## <a name="create-the-app-in-microsoft-developer-portal"></a>Создать приложение на портале для разработчиков Microsoft

* Перейдите к [https://apps.dev.microsoft.com](https://apps.dev.microsoft.com) и создать или войти в учетную запись Майкрософт:

![В диалоговом окне входа](index/_static/MicrosoftDevLogin.png)

Если у вас нет учетной записи Майкрософт, коснитесь  **[создать!](https://signup.live.com/signup?wa=wsignin1.0&rpsnv=13&ct=1478151035&rver=6.7.6643.0&wp=SAPI_LONG&wreply=https%3a%2f%2fapps.dev.microsoft.com%2fLoginPostBack&id=293053&aadredir=1&contextid=D70D4F21246BAB50&bk=1478151036&uiflavor=web&uaid=f0c3de863a914c358b8dc01b1ff49e85&mkt=EN-US&lc=1033&lic=1)** После входа вы попадете на **Мои приложения** страницы:

![Портал разработчиков Microsoft открыть в Microsoft Edge](index/_static/MicrosoftDev.png)

* Коснитесь **Добавление приложения** в правом верхнем углу, а затем введите ваш **имя_приложения** и **адрес электронной почты**:

![Новое диалоговое окно регистрации приложения](index/_static/MicrosoftDevAppCreate.png)

* Для целей этого учебника, снимите **интерактивной установки** флажок.

* Коснитесь **создать** продолжать **регистрации** страницы:

![Страница регистрации](index/_static/MicrosoftDevAppReg.png)

* Коснитесь **добавить платформу** в **платформы** , выберите **Web** платформа:

![Добавить диалоговое окно платформы](index/_static/MicrosoftDevAppPlatform.png)

* В новом **Web** платформы введите URL-адрес разработки с */signin-microsoft* добавляется в **URL-адреса перенаправления** поля (например: `https://localhost:44320/signin-microsoft`). Схема проверки подлинности Microsoft настроен далее в этом учебнике автоматически будет обрабатывать запросы на */signin-microsoft* маршрута для реализации потока OAuth:

![Веб-платформы раздела](index/_static/MicrosoftRedirectUri.png)

* Коснитесь **Добавление URL-адреса** чтобы URL-адрес был добавлен.

* Заполните все прочие параметры приложения, при необходимости и коснитесь **Сохранить** в нижней части страницы, чтобы сохранить изменения в конфигурации приложения.

* При развертывании узла необходимо будет повторно **регистрации** и задайте новый общедоступный URL-адрес.

## <a name="store-microsoft-application-id-and-password"></a>Сохранить код приложения Майкрософт и пароль

* Примечание `Application Id` отображаются на **регистрации** страницы.

* Коснитесь **создать новый пароль** в **секреты приложения** раздела. Откроется окно, где можно скопировать пароль приложения:

![Новый пароль, созданный диалоговое окно](index/_static/MicrosoftDevPassword.png)

Связать конфиденциальные параметры, такие как Microsoft `Application ID` и `Password` в конфигурации приложения с помощью [секрет диспетчер](../../app-secrets.md). В целях этого учебника назовите токены `Authentication:Microsoft:ApplicationId` и `Authentication:Microsoft:Password`.

## <a name="configure-microsoft-account-authentication"></a>Настройка проверки подлинности учетной записи Майкрософт

Шаблон проекта, используемые в этом учебнике гарантирует, что [Microsoft.AspNetCore.Authentication.MicrosoftAccount](https://www.nuget.org/packages/Microsoft.AspNetCore.Authentication.MicrosoftAccount) пакет уже установлен.

* Чтобы установить этот пакет с 2017 г. для Visual Studio, щелкните правой кнопкой мыши проект и выберите пункт **управление пакетами NuGet**.
* Чтобы установить с .NET Core CLI, выполните следующую команду в каталоге проекта:

   `dotnet add package Microsoft.AspNetCore.Authentication.MicrosoftAccount`

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

Добавление службы учетной записи Майкрософт в `ConfigureServices` метод в *файла Startup.cs* файла:

```csharp
services.AddAuthentication().AddMicrosoftAccount(microsoftOptions =>
{
    microsoftOptions.ClientId = Configuration["Authentication:Microsoft:ApplicationId"];
    microsoftOptions.ClientSecret = Configuration["Authentication:Microsoft:Password"];
});
```

`AddAuthentication` Метод должен вызываться только один раз при добавлении нескольких поставщиков проверки подлинности. Все ранее настроенные переопределения могут быть последующие вызовы [AuthenticationOptions](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.builder.authenticationoptions) свойства.

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

Добавьте учетную запись Майкрософт по промежуточного слоя в `Configure` метод в *файла Startup.cs* файла:

```csharp
app.UseMicrosoftAccountAuthentication(new MicrosoftAccountOptions()
{
    ClientId = Configuration["Authentication:Microsoft:ApplicationId"],
    ClientSecret = Configuration["Authentication:Microsoft:Password"]
});
```

---

Несмотря на то, что эти токены имена терминологию, используемую на портал разработчиков Microsoft `ApplicationId` и `Password`, они представляются как `ClientId` и `ClientSecret` для API-Интерфейс настройки.

В разделе [MicrosoftAccountOptions](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.builder.microsoftaccountoptions) Справочник по API для дополнительных сведений о параметрах конфигурации, поддерживается проверка подлинности учетной записи Майкрософт. Это можно использовать для запроса различные сведения о пользователе.

## <a name="sign-in-with-microsoft-account"></a>Войдите с учетной записью Майкрософт

Запустите приложение и нажмите кнопку **входа**. Появляется возможность войти в Microsoft:

![Веб-приложение журнала страницы: пользователь не прошел проверку подлинности](index/_static/DoneMicrosoft.png)

При нажатии кнопки на Microsoft, вы будете перенаправлены в корпорацию Майкрософт для проверки подлинности. После входа под учетной записью Майкрософт (если он еще не выполнили вход) будет предложено разрешить приложению доступ к вашим данным:

![Диалоговое окно проверки подлинности Майкрософт](index/_static/MicrosoftLogin.png)

Коснитесь **Да** вы будете перенаправлены к веб-сайта, где вы можете задать ваш адрес электронной почты.

Вы вошли в, используя учетные данные Microsoft:

![Веб-приложения: пользователь прошел проверку подлинности](index/_static/Done.png)

## <a name="troubleshooting"></a>Устранение неполадок

* Если поставщик учетной записи Майкрософт вы будете перенаправлены на страницу ошибки входа, обратите внимание, ошибка заголовок и описание параметров строки запроса непосредственно за `#` (хэштегом) в Uri.

  Несмотря на то, что сообщение об ошибке, кажется указывают на проблему с использованием проверки подлинности, наиболее распространенной причиной является Uri, не соответствует ни одному из приложения **URI перенаправления** указано **Web** платформы .
* **ASP.NET Core 2.x только:** Если удостоверение не настроена, вызвав `services.AddIdentity` в `ConfigureServices`, пытающиеся выполнить проверку подлинности приведет к *ArgumentException: необходимо указать параметр «SignInScheme»*. Шаблон проекта, используемые в этом учебнике гарантирует, что происходит.
* Если не был создан путем применения первоначальной миграции базы данных сайта, вы получите *не удалось выполнить операцию базы данных при обработке запроса* ошибки. Коснитесь **применить миграции** для создания базы данных и обновить, чтобы продолжить выполнение после ошибки.

## <a name="next-steps"></a>Следующие шаги

* В этой статье показано, как можно выполнить проверку подлинности с помощью Microsoft. Можно выполнить подобный подход для проверки подлинности для других поставщиков на [предыдущую страницу](index.md).

* После публикации веб-узла веб-приложение Azure, следует создать новый `Password` на портале разработчиков Майкрософт.

* Задать `Authentication:Microsoft:ApplicationId` и `Authentication:Microsoft:Password` как параметры приложения на портале Azure. Система конфигурации настраивается для чтения ключи из переменных среды.
