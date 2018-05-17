---
title: Настройка внешнего входа Facebook в ASP.NET Core
author: rick-anderson
description: В этом учебнике показано интеграции проверки подлинности пользователя учетной записи Facebook в существующее приложение ASP.NET Core.
manager: wpickett
ms.author: riande
ms.date: 08/01/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: security/authentication/facebook-logins
ms.openlocfilehash: a0b96f480aaa3941cf63b25780c5a1d9d4b2dbb0
ms.sourcegitcommit: 9bc34b8269d2a150b844c3b8646dcb30278a95ea
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 05/12/2018
---
# <a name="facebook-external-login-setup-in-aspnet-core"></a>Настройка внешнего входа Facebook в ASP.NET Core

Авторы: [Валерий Новицкий](https://github.com/01binary) (Valeriy Novytskyy) и [Рик Андерсон](https://twitter.com/RickAndMSFT) (Rick Anderson)

Этого учебника показано, как предоставить пользователям возможность войти в свою учетную запись Facebook с помощью ASP.NET 2.0 основной пример проекта создан на [предыдущую страницу](xref:security/authentication/social/index). Проверка подлинности Facebook требует [Microsoft.AspNetCore.Authentication.Facebook](https://www.nuget.org/packages/Microsoft.AspNetCore.Authentication.Facebook) пакет NuGet. Мы начнем с создания код приложения Facebook, следуя [официальный действия](https://developers.facebook.com).

## <a name="create-the-app-in-facebook"></a>Создать приложение на Facebook

*  Перейдите к [приложения Facebook разработчики](https://developers.facebook.com/apps/) страницы и выполните вход. Если у вас еще нет учетной записи Facebook, используйте **зарегистрироваться для Facebook** ссылку на страницу входа для ее создания.

* Коснитесь **добавить новое приложение** кнопку в правом верхнем углу, чтобы создать новый идентификатор приложения.

   ![Facebook для портала разработчиков открыть в Microsoft Edge](index/_static/FBMyApps.png)

* Заполните форму и коснитесь **создать идентификатор приложения** кнопки.

   ![Создайте форму новый идентификатор приложения](index/_static/FBNewAppId.png)

* На **выберите продукт** щелкните **Set Up** на **входа Facebook** карты.

   ![Страница установки продукта](index/_static/FBProductSetup.png)

* **Краткое руководство** запустит мастер **выберите платформу** первая страница. Сейчас пропустить мастер, нажав кнопку **параметры** ссылку в меню слева:

   ![Пропустить быстрый запуск](index/_static/FBSkipQuickStart.png)

* Появится **параметры клиента OAuth** страницы:

![Страница параметров OAuth клиента](index/_static/FBOAuthSetup.png)

* Введите URI разработки с */signin-facebook* добавляется в **допустимый URI перенаправления OAuth** поля (например: `https://localhost:44320/signin-facebook`). Проверка подлинности Facebook далее в этом учебнике автоматически будет обрабатывать запросы на */signin-facebook* маршрута для реализации потока OAuth.

* Нажмите кнопку **сохранить изменения**.

* Нажмите кнопку **мониторинга** ссылку в левой области навигации. 

    На этой странице, запишите вашей `App ID` и `App Secret`. В следующем разделе будет добавлен и в приложениях ASP.NET Core.

   ![Панели мониторинга разработчика Facebook](index/_static/FBDashboard.png)

* При развертывании сайта необходимо повторно **входа Facebook** страница установки и зарегистрировать новый открытый универсальный код Ресурса.

## <a name="store-facebook-app-id-and-app-secret"></a>Сохранить код приложения Facebook и секрет приложения

Связать конфиденциальные параметры, например Facebook `App ID` и `App Secret` в конфигурации приложения с помощью [секрет диспетчер](xref:security/app-secrets). В целях этого учебника назовите токены `Authentication:Facebook:AppId` и `Authentication:Facebook:AppSecret`.

Выполните следующие команды для безопасного хранения `App ID` и `App Secret` с помощью диспетчера секрет:

```console
dotnet user-secrets set Authentication:Facebook:AppId <app-id>
dotnet user-secrets set Authentication:Facebook:AppSecret <app-secret>
```

## <a name="configure-facebook-authentication"></a>Настройка проверки подлинности Facebook

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x/)

Добавьте службу Facebook в `ConfigureServices` метод в *файла Startup.cs* файла:

```csharp
services.AddIdentity<ApplicationUser, IdentityRole>()
        .AddEntityFrameworkStores<ApplicationDbContext>()
        .AddDefaultTokenProviders();

services.AddAuthentication().AddFacebook(facebookOptions =>
{
    facebookOptions.AppId = Configuration["Authentication:Facebook:AppId"];
    facebookOptions.AppSecret = Configuration["Authentication:Facebook:AppSecret"];
});
```

[!INCLUDE [default settings configuration](includes/default-settings.md)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x/)

Установка [Microsoft.AspNetCore.Authentication.Facebook](https://www.nuget.org/packages/Microsoft.AspNetCore.Authentication.Facebook) пакета.

* Чтобы установить этот пакет с 2017 г. для Visual Studio, щелкните правой кнопкой мыши проект и выберите пункт **управление пакетами NuGet**.
* Чтобы установить с .NET Core CLI, выполните следующую команду в каталоге проекта:

   `dotnet add package Microsoft.AspNetCore.Authentication.Facebook`

Добавить по промежуточного слоя Facebook в `Configure` метод в *файла Startup.cs* файла:

```csharp
app.UseFacebookAuthentication(new FacebookOptions()
{
    AppId = Configuration["Authentication:Facebook:AppId"],
    AppSecret = Configuration["Authentication:Facebook:AppSecret"]
});
```

---

В разделе [FacebookOptions](/dotnet/api/microsoft.aspnetcore.builder.facebookoptions) Справочник по API для дополнительных сведений о параметрах конфигурации, поддерживаемых проверкой подлинности Facebook. Параметры конфигурации можно использовать для:

* Запрос различные сведения о пользователе.
* Добавьте аргументы строки запроса, чтобы настроить имя входа в систему.

## <a name="sign-in-with-facebook"></a>Войдите с помощью Facebook

Запустите приложение и нажмите кнопку **входа**. Появится возможность войти в Facebook.

![Веб-приложения: пользователь не прошел проверку подлинности](index/_static/DoneFacebook.png)

При нажатии кнопки на **Facebook**, вы будете перенаправлены на Facebook для проверки подлинности:

![Страница проверки подлинности Facebook](index/_static/FBLogin.png)

Открытый профиль и адрес электронной почты запросы проверки подлинности Facebook по умолчанию:

![Страница проверки подлинности Facebook](index/_static/FBLoginDone.png)

После ввода учетных данных Facebook вы будете перенаправлены обратно на сайт, где вы можете задать ваш адрес электронной почты.

Вы вошли в Facebook учетные данные с помощью:

![Веб-приложения: пользователь прошел проверку подлинности](index/_static/Done.png)

## <a name="troubleshooting"></a>Устранение неполадок

* **ASP.NET Core 2.x только:** Если удостоверение не настроена, вызвав `services.AddIdentity` в `ConfigureServices`, пытающиеся выполнить проверку подлинности приведет к *ArgumentException: необходимо указать параметр «SignInScheme»*. Шаблон проекта, используемые в этом учебнике гарантирует, что происходит.
* Если не был создан путем применения первоначальной миграции базы данных сайта, вы получаете *не удалось выполнить операцию базы данных при обработке запроса* ошибки. Коснитесь **применить миграции** для создания базы данных и обновить, чтобы продолжить выполнение после ошибки.

## <a name="next-steps"></a>Следующие шаги

* В этой статье показано, как можно выполнить проверку подлинности с Facebook. Можно выполнить подобный подход для проверки подлинности для других поставщиков на [предыдущую страницу](xref:security/authentication/social/index).

* После публикации веб-сайте Azure веб-приложения, необходимо переустановить `AppSecret` на портале разработчика Facebook.

* Задать `Authentication:Facebook:AppId` и `Authentication:Facebook:AppSecret` как параметры приложения на портале Azure. Система конфигурации настраивается для чтения ключи из переменных среды.
