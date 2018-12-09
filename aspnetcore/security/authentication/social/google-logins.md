---
title: Настройка внешней учетной записи Google в ASP.NET Core
author: rick-anderson
description: В этом учебнике показано интеграцию Google учетной записи пользователя и проверки подлинности в существующее приложение ASP.NET Core.
ms.author: riande
ms.custom: mvc, seodec18
ms.date: 11/11/2018
uid: security/authentication/google-logins
ms.openlocfilehash: e5deda5d521643e3155be00f4630a86c6a82575c
ms.sourcegitcommit: 49faca2644590fc081d86db46ea5e29edfc28b7b
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 12/09/2018
ms.locfileid: "53121535"
---
# <a name="google-external-login-setup-in-aspnet-core"></a>Настройка внешней учетной записи Google в ASP.NET Core

Авторы: [Валерий Новицкий](https://github.com/01binary) (Valeriy Novytskyy) и [Рик Андерсон](https://twitter.com/RickAndMSFT) (Rick Anderson)

Этом руководстве показано, как разрешить пользователям вход с помощью своей учетной записи Google + используя образец проекта ASP.NET Core 2.0, созданные на [предыдущую страницу](xref:security/authentication/social/index). Мы начнем, следуя [официальный действия](https://developers.google.com/identity/sign-in/web/devconsole-project) для создания нового приложения в консоли Google API.

## <a name="create-the-app-in-google-api-console"></a>Создание приложения в консоли Google API

* Перейдите к [ https://console.developers.google.com/projectselector/apis/library ](https://console.developers.google.com/projectselector/apis/library) и войдите в систему. Если у вас нет учетной записи Google, используйте **Дополнительные параметры** > **[создать учетную запись](https://accounts.google.com/SignUpWithoutGmail?service=cloudconsole&continue=https%3A%2F%2Fconsole.developers.google.com%2Fprojectselector%2Fapis%2Flibrary&ltmpl=api)**  ссылку, чтобы создать его:

![Консоли Google API](index/_static/GoogleConsoleLogin.png)

* Вы будете перенаправлены на **библиотека API диспетчера** страницы:

![Целевая на странице библиотеки API диспетчера](index/_static/GoogleConsoleSwitchboard.png)

* Коснитесь **создать** и введите ваш **имя_проекта**:

![Диалоговое окно создания нового проекта](index/_static/GoogleConsoleNewProj.png)

* После принятия диалогового окна, вы будете перенаправлены на страницу библиотеки, в которой можно выбрать функции для нового приложения. Найти **Google + API** в списке и щелкните ссылку на нее, чтобы добавить компонент API:

![Поиск «Google + API» на странице библиотеки API диспетчера](index/_static/GoogleConsoleChooseApi.png)

* Откроется страница для вновь добавленного API. Коснитесь **включить** Добавление Google + знак в компонент приложения:

![Целевая на странице диспетчера API Google + API](index/_static/GoogleConsoleEnableApi.png)

* После включения API, коснитесь **Создание учетных данных** Настройка секреты:

![Создание кнопки учетные данные на странице диспетчера API Google + API](index/_static/GoogleConsoleGoCredentials.png)

* Выберите:
  * **Google + API**
  * **Веб-сервера (например node.js, Tomcat)**, и
  * **Данные пользователя**:

![Страница API диспетчера учетных данных: Узнайте также довольно учетные данные, вы должны панели](index/_static/GoogleConsoleChooseCred.png)

* Коснитесь **какие учетные данные нужны?** , чтобы перейти на втором шаге Конфигурация приложения **создать идентификатор клиента OAuth 2.0**:

![Страница API диспетчера учетных данных: создайте идентификатор клиента OAuth 2.0](index/_static/GoogleConsoleCreateClient.png)

* Поскольку мы создаем проект Google + со всего одной функцией (вход), мы можете ввести тот же **имя** для идентификатора клиента OAuth 2.0, который мы использовали для проекта.

* Введите URI разработки с `/signin-google` добавляется в **авторизованные URI перенаправления** поле (например: `https://localhost:44320/signin-google`). Проверка подлинности Google настроена далее в этом руководстве автоматически будет обрабатывать запросы на `/signin-google` маршрута, чтобы реализовать поток OAuth.

> [!NOTE]
> Сегмент URI `/signin-google` задан в качестве обратного вызова по умолчанию поставщик проверки подлинности Google. URI обратного вызова по умолчанию можно изменить при настройке по промежуточного слоя проверки подлинности Google с помощью наследуемого [RemoteAuthenticationOptions.CallbackPath](/dotnet/api/microsoft.aspnetcore.authentication.remoteauthenticationoptions.callbackpath) свойство [GoogleOptions](/dotnet/api/microsoft.aspnetcore.authentication.google.googleoptions) класса.

* Нажмите клавишу TAB, чтобы добавить **авторизованные URI перенаправления** запись.

* Коснитесь **создать идентификатор клиента**, которой осуществляется переход к третьему этапу — **настроить экран согласия OAuth 2.0**:

![Страница API диспетчера учетных данных: настроить экран согласия OAuth 2.0](index/_static/GoogleConsoleAddCred.png)

* Введите ваш общедоступны **адрес электронной почты** и **название продукта** показано для вашего приложения, когда Google + предлагает пользователю войти в систему. Доступны дополнительные параметры в разделе **возможностей настройки**.

* Коснитесь **Продолжить** для перейдите к последнему шагу, **скачать учетные данные**:

![Страница API диспетчера учетных данных: скачивание учетных данных](index/_static/GoogleConsoleFinish.png)

* Коснитесь **загрузить** сохранить файл JSON с помощью секретов приложения и **сделать** чтобы завершить создание нового приложения.

* При развертывании на сайте необходимо будет пересмотреть **консоли Google** и зарегистрировать новый общедоступный URL-адрес.

## <a name="store-google-clientid-and-clientsecret"></a>Store Google ClientID и ClientSecret

Связать конфиденциальные параметры, такие как Google `Client ID` и `Client Secret` для конфигурации приложения с помощью [Secret Manager](xref:security/app-secrets). В целях этого учебника назовите токены `Authentication:Google:ClientId` и `Authentication:Google:ClientSecret`.

Значения для этих маркеров можно найти в JSON-файл, Скачанный на предыдущем шаге, в `web.client_id` и `web.client_secret`.

## <a name="configure-google-authentication"></a>Настройка проверки подлинности Google

::: moniker range=">= aspnetcore-2.0"

Добавить службу Google в `ConfigureServices` метод в *Startup.cs* файла:

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

[!INCLUDE [default settings configuration](includes/default-settings.md)]

[!INCLUDE[](includes/chain-auth-providers.md)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

Шаблон проекта, используемый в этом руководстве гарантирует, что [Microsoft.AspNetCore.Authentication.Google](https://www.nuget.org/packages/Microsoft.AspNetCore.Authentication.Google) установки пакета.

* Установить этот пакет с помощью Visual Studio 2017, щелкните правой кнопкой мыши проект и выберите **управление пакетами NuGet**.
* Чтобы установить с помощью интерфейса командной строки .NET Core, выполните следующую команду в каталоге проекта:

`dotnet add package Microsoft.AspNetCore.Authentication.Google`

Добавьте по промежуточного слоя Google в `Configure` метод в *Startup.cs* файла:

```csharp
app.UseGoogleAuthentication(new GoogleOptions()
{
    ClientId = Configuration["Authentication:Google:ClientId"],
    ClientSecret = Configuration["Authentication:Google:ClientSecret"]
});
```

::: moniker-end

См. в разделе [GoogleOptions](/dotnet/api/microsoft.aspnetcore.builder.googleoptions) Справочник по API, Дополнительные сведения о параметрах конфигурации, поддерживается проверка подлинности Google. Это может использоваться для запроса различные сведения о пользователе.

## <a name="sign-in-with-google"></a>Войдите с помощью Google

Запустите приложение и нажмите кнопку **вход**. Появится возможность войти с помощью Google:

![Веб-приложение работает в Microsoft Edge: пользователь не прошел проверку подлинности](index/_static/DoneGoogle.png)

При выборе Google, вы будете перенаправлены на Google для проверки подлинности:

![Диалоговое окно проверки подлинности Google](index/_static/GoogleLogin.png)

После ввода учетных данных Google, затем вы будете перенаправлены обратно на веб-узел, где вы можете задать свой адрес электронной почты.

Теперь вы вошли с использованием учетных данных Google:

![Веб-приложение работает в Microsoft Edge: пользователь прошел проверку подлинности](index/_static/Done.png)

[!INCLUDE[Forward request information when behind a proxy or load balancer section](includes/forwarded-headers-middleware.md)]

## <a name="troubleshooting"></a>Устранение неполадок

* Если появится `403 (Forbidden)` страницу ошибки из собственного приложения, когда работает в режиме разработки (или приостановки выполнения в отладчике с той же ошибкой), убедитесь, что **Google + API** была включена в **библиотека API диспетчера** , выполнив действия, описанные [более ранних версий на этой странице](#create-the-app-in-google-api-console). Если вход не работает, а не получают все ошибки, переключитесь в режим разработки для упрощения процесса отладки проблемы.
* **ASP.NET Core 2.x только:** Если удостоверение не настроена, вызвав `services.AddIdentity` в `ConfigureServices`, пытающиеся выполнить проверку подлинности приведет к *ArgumentException: необходимо указать параметр «SignInScheme»*. Шаблон проекта, используемый в этом руководстве гарантирует, что это будет сделано.
* Если база данных сайта не был создан путем применения первоначальной миграции, вы получите *сбой операции из базы данных при обработке запроса* ошибки. Коснитесь **применить миграции** для создания базы данных и обновить, чтобы продолжить выполнение после ошибки.

## <a name="next-steps"></a>Следующие шаги

* В этой статье объясняется, как можно выполнить проверку подлинности с помощью Google. Можно выполнить аналогичный подход для проверки подлинности с помощью других поставщиков, перечисленных на [предыдущую страницу](xref:security/authentication/social/index).

* После публикации веб-сайт веб-приложение Azure, необходимо сбросить `ClientSecret` в консоли Google API.

* Задайте `Authentication:Google:ClientId` и `Authentication:Google:ClientSecret` как параметры приложения на портале Azure. Система конфигурации предназначена для чтения разделов из переменных среды.
