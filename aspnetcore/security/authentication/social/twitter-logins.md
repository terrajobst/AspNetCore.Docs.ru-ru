---
title: Настройка внешней учетной записи Twitter с помощью ASP.NET Core
author: rick-anderson
description: В этом учебнике показано интеграции проверки подлинности пользователя учетной записи Twitter в существующее приложение ASP.NET Core.
ms.author: riande
ms.custom: mvc
ms.date: 11/11/2018
uid: security/authentication/twitter-logins
ms.openlocfilehash: 49db8b921fde169380ca284f46e535786b2b8a30
ms.sourcegitcommit: 3e94d192b2ed9409fe72e3735e158b333354964c
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 12/21/2018
ms.locfileid: "53735808"
---
# <a name="twitter-external-login-setup-with-aspnet-core"></a>Настройка внешней учетной записи Twitter с помощью ASP.NET Core

Авторы: [Валерий Новицкий](https://github.com/01binary) (Valeriy Novytskyy) и [Рик Андерсон](https://twitter.com/RickAndMSFT) (Rick Anderson)

Этом руководстве показано, как разрешить пользователям [войдите с помощью своей учетной записью Twitter](https://dev.twitter.com/web/sign-in/desktop-browser) используя образец проекта ASP.NET Core 2.0 создан на [предыдущую страницу](xref:security/authentication/social/index).

## <a name="create-the-app-in-twitter"></a>Создайте приложение в Twitter

* Перейдите к [ https://apps.twitter.com/ ](https://apps.twitter.com/) и войдите в систему. Если у вас нет учетной записи Twitter, используйте **[Зарегистрируйтесь сейчас](https://twitter.com/signup)** ссылку, чтобы создать его. После входа, **управление приложениями** отобразится страница с:

  ![Управление приложениями Twitter можно открыть в Microsoft Edge](index/_static/TwitterAppManage.png)

* Коснитесь **создать новое приложение** и заполните приложения **имя**, **описание** и общедоступных **веб-сайт** URI (это могут быть временными пока не будут Зарегистрируйте доменное имя):

  ![Создание страницы приложения](index/_static/TwitterCreate.png)

* Введите URI разработки с `/signin-twitter` добавляется в **допустимый URI перенаправления OAuth** поле (например: `https://localhost:44320/signin-twitter`). Схема проверки подлинности Twitter, Настройка описывается далее в этом руководстве автоматически будет обрабатывать запросы на `/signin-twitter` маршрута, чтобы реализовать поток OAuth.

  > [!NOTE]
  > Сегмент URI `/signin-twitter` задан в качестве обратного вызова по умолчанию поставщика проверки подлинности Twitter. URI обратного вызова по умолчанию можно изменить при настройке по промежуточного слоя проверки подлинности Twitter с помощью наследуемого [RemoteAuthenticationOptions.CallbackPath](/dotnet/api/microsoft.aspnetcore.authentication.remoteauthenticationoptions.callbackpath) свойство [TwitterOptions](/dotnet/api/microsoft.aspnetcore.authentication.twitter.twitteroptions) класс.

* Заполните оставшиеся поля формы и коснитесь **создать приложение Twitter**. Отображаются сведения о новом приложения:

  ![Вкладке "Сведения" на странице "приложения"](index/_static/TwitterAppDetails.png)

* При развертывании на сайте необходимо будет пересмотреть **управление приложениями** странице и зарегистрировать новый открытый универсальный код Ресурса.

## <a name="storing-twitter-consumerkey-and-consumersecret"></a>Хранение Twitter ConsumerKey и ConsumerSecret

Связать конфиденциальные параметры, такие как Twitter `Consumer Key` и `Consumer Secret` для конфигурации приложения с помощью [Secret Manager](xref:security/app-secrets). В целях этого учебника назовите токены `Authentication:Twitter:ConsumerKey` и `Authentication:Twitter:ConsumerSecret`.

Эти маркеры можно найти на **ключи и токены доступа** вкладке после создания нового приложения Twitter:

![Вкладка "ключи и маркеры доступа"](index/_static/TwitterKeys.png)

## <a name="configure-twitter-authentication"></a>Настройка проверки подлинности Twitter

Шаблон проекта, используемый в этом руководстве гарантирует, что [Microsoft.AspNetCore.Authentication.Twitter](https://www.nuget.org/packages/Microsoft.AspNetCore.Authentication.Twitter) пакет уже установлен.

* Установить этот пакет с помощью Visual Studio 2017, щелкните правой кнопкой мыши проект и выберите **управление пакетами NuGet**.
* Чтобы установить с помощью интерфейса командной строки .NET Core, выполните следующую команду в каталоге проекта:

  `dotnet add package Microsoft.AspNetCore.Authentication.Twitter`

::: moniker range=">= aspnetcore-2.0"

Добавление службы Twitter в `ConfigureServices` метод в *Startup.cs* файла:

```csharp
services.AddDefaultIdentity<IdentityUser>()
        .AddDefaultUI(UIFramework.Bootstrap4)
        .AddEntityFrameworkStores<ApplicationDbContext>();

services.AddAuthentication().AddTwitter(twitterOptions =>
{
    twitterOptions.ConsumerKey = Configuration["Authentication:Twitter:ConsumerKey"];
    twitterOptions.ConsumerSecret = Configuration["Authentication:Twitter:ConsumerSecret"];
});
```

[!INCLUDE [default settings configuration](includes/default-settings.md)]

[!INCLUDE[](includes/chain-auth-providers.md)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

Добавьте по промежуточного слоя Twitter в `Configure` метод в *Startup.cs* файла:

```csharp
app.UseTwitterAuthentication(new TwitterOptions()
{
    ConsumerKey = Configuration["Authentication:Twitter:ConsumerKey"],
    ConsumerSecret = Configuration["Authentication:Twitter:ConsumerSecret"]
});
```

::: moniker-end

См. в разделе [TwitterOptions](/dotnet/api/microsoft.aspnetcore.builder.twitteroptions) Справочник по API, Дополнительные сведения о параметрах конфигурации, поддерживаемых проверкой подлинности Twitter. Это может использоваться для запроса различные сведения о пользователе.

## <a name="sign-in-with-twitter"></a>Войдите с помощью Twitter

Запустите приложение и нажмите кнопку **вход**. Появится возможность войти с помощью Twitter:

![Веб-приложение: Пользователь не прошел проверку подлинности](index/_static/DoneTwitter.png)

Щелкнув **Twitter** перенаправляет Twitter для проверки подлинности:

![Страница проверки подлинности Twitter](index/_static/TwitterLogin.png)

После ввода учетных данных Twitter, вы будете перенаправлены обратно на веб-узел, где вы можете задать свой адрес электронной почты.

Теперь вы вошли с использованием учетных данных Twitter:

![Веб-приложение: Пользователь прошел проверку подлинности](index/_static/Done.png)

[!INCLUDE[Forward request information when behind a proxy or load balancer section](includes/forwarded-headers-middleware.md)]

## <a name="troubleshooting"></a>Устранение неполадок

* **ASP.NET Core 2.x только:** Если удостоверение не настроена, вызвав `services.AddIdentity` в `ConfigureServices`, пытающиеся выполнить проверку подлинности приведет к *ArgumentException: Необходимо указать параметр «SignInScheme»*. Шаблон проекта, используемый в этом руководстве гарантирует, что это будет сделано.
* Если база данных сайта не был создан путем применения первоначальной миграции, вы получите *сбой операции из базы данных при обработке запроса* ошибки. Коснитесь **применить миграции** для создания базы данных и обновить, чтобы продолжить выполнение после ошибки.

## <a name="next-steps"></a>Следующие шаги

* В этой статье объясняется, как можно выполнить проверку подлинности с помощью Twitter. Можно выполнить аналогичный подход для проверки подлинности с помощью других поставщиков, перечисленных на [предыдущую страницу](xref:security/authentication/social/index).

* После публикации веб-сайт веб-приложение Azure, необходимо сбросить `ConsumerSecret` на портале разработчика Twitter.

* Задайте `Authentication:Twitter:ConsumerKey` и `Authentication:Twitter:ConsumerSecret` как параметры приложения на портале Azure. Система конфигурации предназначена для чтения разделов из переменных среды.
