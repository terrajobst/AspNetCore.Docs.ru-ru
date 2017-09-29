---
title: "Программа установки внешней учетной записи Google в ASP.NET Core"
author: rick-anderson
description: "Программа установки внешней учетной записи Google в ASP.NET Core"
keywords: ASP.NET Core
ms.author: riande
manager: wpickett
ms.date: 08/02/2017
ms.topic: article
ms.technology: aspnet
ms.prod: asp.net-core
uid: security/authentication/google-logins
ms.openlocfilehash: 8723a74250ff1b0a63139057bfc17fdd31dd169e
ms.sourcegitcommit: 6e83c55eb0450a3073ef2b95fa5f5bcb20dbbf89
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 09/28/2017
---
# <a name="configuring-google-authentication-in-aspnet-core"></a>Настройка проверки подлинности Google в ASP.NET Core

<a name=security-authentication-google-logins></a>

Авторы: [Валерий Новицкий](https://github.com/01binary) (Valeriy Novytskyy) и [Рик Андерсон](https://twitter.com/RickAndMSFT) (Rick Anderson)

Этого учебника показано, как предоставить пользователям возможность войти в их Google + учетную запись с помощью ASP.NET 2.0 основной пример проекта создан на [предыдущую страницу](index.md). Мы начать со следующих [официальный действия](https://developers.google.com/identity/sign-in/web/devconsole-project) для создания нового приложения в консоли Google API.

## <a name="create-the-app-in-google-api-console"></a>Создание приложения в консоли Google API

* Перейдите к [https://console.developers.google.com/projectselector/apis/library](https://console.developers.google.com/projectselector/apis/library) и выполните вход. Если у вас еще нет учетной записи Google, используйте **Дополнительные параметры** > **[создать учетную запись](https://accounts.google.com/SignUpWithoutGmail?service=cloudconsole&continue=https%3A%2F%2Fconsole.developers.google.com%2Fprojectselector%2Fapis%2Flibrary&ltmpl=api)**  ссылку, чтобы создать один:

![Консоли Google API](index/_static/GoogleConsoleLogin.png)

* Вы будете перенаправлены на **библиотека API диспетчера** страницы:

![Библиотека API диспетчера страницы](index/_static/GoogleConsoleSwitchboard.png)

* Коснитесь **создать** и введите ваш **имя проекта**:

![Диалоговое окно создания нового проекта](index/_static/GoogleConsoleNewProj.png)

* После принятия диалоговое окно, вы будете перенаправлены на страницу библиотеки, в которой можно выбрать компоненты для нового приложения. Найти **API Google +** в списке и щелкните ссылку на нее, чтобы добавить компонент API:

![Библиотека API диспетчера страницы](index/_static/GoogleConsoleChooseApi.png)

* Откроется страница для вновь добавленного API. Коснитесь **включить** Добавление приложения Google + знак в функции:

![Страница диспетчера API Google + API](index/_static/GoogleConsoleEnableApi.png)

* После включения API, коснитесь **создать учетные данные** Настройка секреты:

![Страница диспетчера API Google + API](index/_static/GoogleConsoleGoCredentials.png)

* Выберите:
   * **Google + API**
   * **Веб-сервера (например node.js, Tomcat)**, и
   * **Данные пользователя**:

![Страница API диспетчера учетных данных: Узнайте, что тип учетных данных вы должны панели](index/_static/GoogleConsoleChooseCred.png)

* Коснитесь **какие учетные данные нужны?** которой осуществляется переход ко второму шагу Конфигурация приложения **создать идентификатор клиента OAuth 2.0**:

![Страница API диспетчера учетных данных: создать идентификатор клиента OAuth 2.0](index/_static/GoogleConsoleCreateClient.png)

* Поскольку мы создаем с какой-либо функции (вход), мы можете ввести тот же проект Google + **имя** для идентификатор клиента OAuth 2.0, что мы использовали для проекта.

* Введите URI разработки с */signin-google* добавляется в **авторизованные URI перенаправления** поля (например: `https://localhost:44320/signin-google`). Проверка подлинности Google далее в этом учебнике автоматически будет обрабатывать запросы на */signin-google* маршрута для реализации потока OAuth.

* Нажмите клавишу TAB, чтобы добавить **авторизованные URI перенаправления** входа.

* Коснитесь **создать идентификатор клиента**, которой осуществляется переход к третьему этапу **настроить экран согласия OAuth 2.0**:

![Страница API диспетчера учетных данных: настроить экран согласия OAuth 2.0](index/_static/GoogleConsoleAddCred.png)

* Введите ваш общедоступным **адрес электронной почты** и **название продукта** показано для своего приложения Google + пользователю войти в систему по запросу. Дополнительные параметры доступны в разделе **Дополнительные параметры настройки**.

* Коснитесь **Продолжить** для перейдите к последнему шагу, **загрузить учетные данные**:

![Страница API диспетчера учетных данных: загрузить учетные данные](index/_static/GoogleConsoleFinish.png)

* Коснитесь **загрузки** для сохранения файла JSON с секретами приложения и **сделать** для завершения создания нового приложения.

* При развертывании узла необходимо будет повторно **консоли Google** и зарегистрировать новый общедоступный URL-адрес.

## <a name="store-google-clientid-and-clientsecret"></a>Магазин Google ClientID и ClientSecret

Связать конфиденциальные параметры, например Google `Client ID` и `Client Secret` в конфигурации приложения с помощью [секрет диспетчер](../../app-secrets.md). В целях этого учебника назовите токены `Authentication:Google:ClientId` и `Authentication:Google:ClientSecret`.

Значения для этих маркеров можно найти в JSON-файл, загруженный в предыдущем шаге, в `web.client_id` и `web.client_secret`.

## <a name="configure-google-authentication"></a>Настройка проверки подлинности Google

Шаблон проекта, используемые в этом учебнике гарантирует, что [Microsoft.AspNetCore.Authentication.Google](https://www.nuget.org/packages/Microsoft.AspNetCore.Authentication.Google) пакет установлен.

 * Чтобы установить этот пакет с 2017 г. для Visual Studio, щелкните правой кнопкой мыши проект и выберите пункт **управление пакетами NuGet**.
 * Чтобы установить с .NET Core CLI, выполните следующую команду в каталоге проекта:

   `dotnet add package Microsoft.AspNetCore.Authentication.Google`

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

Добавление службы Google в `ConfigureServices` метод в *файла Startup.cs* файла:

```csharp
services.AddIdentity<ApplicationUser, IdentityRole>()
        .AddEntityFrameworkStores<ApplicationDbContext>()
        .AddDefaultTokenProviders();

services.AddAuthentication().AddGoogle(googleOptions =>
{
    googleOptions.ClientId = Configuration["Authentication:Google:ClientId"];
    googleOptions.ClientSecret = Configuration["Authentication:Google:ClientSecret"];
});
```

[!INCLUDE[default settings configuration](includes/default-settings.md)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

Добавить по промежуточного слоя Google в `Configure` метод в *файла Startup.cs* файла:

```csharp
app.UseGoogleAuthentication(new GoogleOptions()
{
    ClientId = Configuration["Authentication:Google:ClientId"],
    ClientSecret = Configuration["Authentication:Google:ClientSecret"]
});
```

---

В разделе [GoogleOptions](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.builder.googleoptions) Справочник по API для дополнительных сведений о параметрах конфигурации, поддерживаемых проверкой подлинности Google. Это можно использовать для запроса различные сведения о пользователе.

## <a name="sign-in-with-google"></a>Вход Google

Запустите приложение и нажмите кнопку **входа**. Появляется возможность войти с помощью Google:

![Веб-приложение работает в Microsoft Edge: пользователь не прошел проверку подлинности](index/_static/DoneGoogle.png)

При нажатии кнопки на Google, вы будете перенаправлены на Google для проверки подлинности:

![Диалоговое окно проверки подлинности Google](index/_static/GoogleLogin.png)

После ввода учетных данных Google, затем вы будете перенаправлены к веб-сайта, где вы можете задать ваш адрес электронной почты.

Будет выполнен вход с помощью учетных данных Google:

![Веб-приложение работает в Microsoft Edge: пользователь прошел проверку подлинности](index/_static/Done.png)

## <a name="troubleshooting"></a>Устранение неполадок

* При получении `403 (Forbidden)` страницы ошибки из вашего собственного приложения, когда работает в режиме разработки (или приостановки выполнения в отладчике с той же ошибки), убедитесь, что **API Google +** была включена в **библиотека API диспетчера** , выполнив следующие действия [ранее на этой странице](#create-the-app-in-google-api-console). Если вход не работает, а не получают все ошибки, переключитесь в режим разработки для упрощения процесса отладки проблему.
* **ASP.NET Core 2.x только:** Если удостоверение не настроена, вызвав `services.AddIdentity` в `ConfigureServices`, пытающиеся выполнить проверку подлинности приведет к *ArgumentException: необходимо указать параметр «SignInScheme»*. Шаблон проекта, используемые в этом учебнике гарантирует, что происходит.
* Если не был создан путем применения первоначальной миграции базы данных сайта, вы получите *не удалось выполнить операцию базы данных при обработке запроса* ошибки. Коснитесь **применить миграции** для создания базы данных и обновить, чтобы продолжить выполнение после ошибки.

## <a name="next-steps"></a>Следующие шаги

* В этой статье показано, как можно выполнить проверку подлинности с помощью Google. Можно выполнить подобный подход для проверки подлинности для других поставщиков на [предыдущую страницу](index.md).

* После публикации веб-сайте Azure веб-приложения, необходимо переустановить `ClientSecret` в консоли Google API.

* Задать `Authentication:Google:ClientId` и `Authentication:Google:ClientSecret` как параметры приложения на портале Azure. Система конфигурации настраивается для чтения ключи из переменных среды.
