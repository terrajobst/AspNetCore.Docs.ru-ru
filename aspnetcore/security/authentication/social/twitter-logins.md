---
title: "Программа установки внешнее имя входа Twitter"
author: rick-anderson
description: 
keywords: ASP.NET Core
ms.author: riande
manager: wpickett
ms.date: 11/01/2016
ms.topic: article
ms.assetid: E5931607-31C0-4B20-B416-85E3550F5EA8
ms.technology: aspnet
ms.prod: asp.net-core
uid: security/authentication/twitter-logins
ms.openlocfilehash: 87be0bdd4637cff609a4908b303a13272656e2a4
ms.sourcegitcommit: 6e83c55eb0450a3073ef2b95fa5f5bcb20dbbf89
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 09/28/2017
---
# <a name="configuring-twitter-authentication"></a>Настройка проверки подлинности Twitter

<a name=security-authentication-twitter-logins></a>

Авторы: [Валерий Новицкий](https://github.com/01binary) (Valeriy Novytskyy) и [Рик Андерсон](https://twitter.com/RickAndMSFT) (Rick Anderson)

Этого учебника показано, как предоставить пользователям возможность [войти в свою учетную запись Twitter](https://dev.twitter.com/web/sign-in/desktop-browser) с помощью проекта ASP.NET 2.0 основной пример создан на [предыдущую страницу](index.md).

## <a name="create-the-app-in-twitter"></a>Создать приложение на Twitter

* Перейдите к [https://apps.twitter.com/](https://apps.twitter.com/) и выполните вход. Если у вас еще нет учетной записи Twitter, используйте  **[Зарегистрируйтесь сейчас](https://twitter.com/signup)**  ссылку, чтобы создать его. После входа в, **управление приложениями** показана страница:

![Управление приложениями Twitter открыть в Microsoft Edge](index/_static/TwitterAppManage.png)

* Коснитесь **создать новое приложение** и заполнить приложения **имя**, **описание** и открытые **веб-сайт** URI (это может быть временной пока не будет Зарегистрируйте доменное имя):

![Создание страницы приложения](index/_static/TwitterCreate.png)

* Введите URI разработки с */signin-twitter* добавляется в **допустимый URI перенаправления OAuth** поля (например: `https://localhost:44320/signin-twitter`). Схема проверки подлинности Twitter настроен далее в этом учебнике автоматически будет обрабатывать запросы на */signin-twitter* маршрута для реализации потока OAuth.

* Заполните другие формы и коснитесь **создания приложения Twitter**. Отображаются сведения о новом приложение.

![Вкладка "Подробности" на странице приложения](index/_static/TwitterAppDetails.png)

* При развертывании узла необходимо будет повторно **управление приложениями** страницы и зарегистрировать новый открытый универсальный код Ресурса.

## <a name="storing-twitter-consumerkey-and-consumersecret"></a>Хранение Twitter ConsumerKey и ConsumerSecret

Связать конфиденциальные параметры, такие как Twitter `Consumer Key` и `Consumer Secret` в конфигурации приложения с помощью [секрет диспетчер](../../app-secrets.md). В целях этого учебника назовите токены `Authentication:Twitter:ConsumerKey` и `Authentication:Twitter:ConsumerSecret`.

Эти токены можно найти на **ключей и маркеры доступа** вкладку после создания нового приложения Twitter:

![Вкладка ключей и маркеры доступа](index/_static/TwitterKeys.png)

## <a name="configure-twitter-authentication"></a>Настройка проверки подлинности Twitter

Шаблон проекта, используемые в этом учебнике гарантирует, что [Microsoft.AspNetCore.Authentication.Twitter](https://www.nuget.org/packages/Microsoft.AspNetCore.Authentication.Twitter) пакет уже установлен.

* Чтобы установить этот пакет с 2017 г. для Visual Studio, щелкните правой кнопкой мыши проект и выберите пункт **управление пакетами NuGet**.
* Чтобы установить с .NET Core CLI, выполните следующую команду в каталоге проекта:

   `dotnet add package Microsoft.AspNetCore.Authentication.Twitter`

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

Добавление службы Twitter в `ConfigureServices` метод в *файла Startup.cs* файла:

```csharp
services.AddIdentity<ApplicationUser, IdentityRole>()
        .AddEntityFrameworkStores<ApplicationDbContext>()
        .AddDefaultTokenProviders();

services.AddAuthentication().AddTwitter(twitterOptions =>
{
    twitterOptions.ConsumerKey = Configuration["Authentication:Twitter:ConsumerKey"];
    twitterOptions.ConsumerSecret = Configuration["Authentication:Twitter:ConsumerSecret"];
});
```

[!INCLUDE[default settings configuration](includes/default-settings.md)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

Добавить по промежуточного слоя Twitter в `Configure` метод в *файла Startup.cs* файла:

```csharp
app.UseTwitterAuthentication(new TwitterOptions()
{
    ConsumerKey = Configuration["Authentication:Twitter:ConsumerKey"],
    ConsumerSecret = Configuration["Authentication:Twitter:ConsumerSecret"]
});
```

---

В разделе [TwitterOptions](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.builder.twitteroptions) Справочник по API для дополнительных сведений о параметрах конфигурации, поддерживаемых проверкой подлинности Twitter. Это можно использовать для запроса различные сведения о пользователе.

## <a name="sign-in-with-twitter"></a>Войдите с помощью Twitter

Запустите приложение и нажмите кнопку **входа**. Появляется возможность войти в Twitter:

![Веб-приложения: пользователь не прошел проверку подлинности](index/_static/DoneTwitter.png)

Щелкнув **Twitter** перенаправляет Twitter для проверки подлинности:

![Страница проверки подлинности Twitter](index/_static/TwitterLogin.png)

После ввода учетных данных Twitter, вы будете перенаправлены к веб-сайта, где вы можете задать ваш адрес электронной почты.

Будет выполнен вход с помощью учетных данных Twitter:

![Веб-приложения: пользователь прошел проверку подлинности](index/_static/Done.png)

## <a name="troubleshooting"></a>Устранение неполадок

* **ASP.NET Core 2.x только:** Если удостоверение не настроена, вызвав `services.AddIdentity` в `ConfigureServices`, пытающиеся выполнить проверку подлинности приведет к *ArgumentException: необходимо указать параметр «SignInScheme»*. Шаблон проекта, используемые в этом учебнике гарантирует, что происходит.
* Если не был создан путем применения первоначальной миграции базы данных сайта, вы получите *не удалось выполнить операцию базы данных при обработке запроса* ошибки. Коснитесь **применить миграции** для создания базы данных и обновить, чтобы продолжить выполнение после ошибки.

## <a name="next-steps"></a>Следующие шаги

* В этой статье показано, как можно выполнить проверку подлинности с Twitter. Можно выполнить подобный подход для проверки подлинности для других поставщиков на [предыдущую страницу](index.md).

* После публикации веб-сайте Azure веб-приложения, необходимо переустановить `ConsumerSecret` на портале разработчика Twitter.

* Задать `Authentication:Twitter:ConsumerKey` и `Authentication:Twitter:ConsumerSecret` как параметры приложения на портале Azure. Система конфигурации настраивается для чтения ключи из переменных среды.
