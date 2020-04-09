---
title: Добавление, загрузка и удаление пользовательских данных в Identity в проекте ASP.NET Core
author: rick-anderson
description: Узнайте, как добавить пользовательские пользовательские данные в Identity в проекте ASP.NET Core. Удаление данных на GDPR.
ms.author: riande
ms.date: 03/26/2020
ms.custom: mvc, seodec18
uid: security/authentication/add-user-data
ms.openlocfilehash: 76b83df22381429feab80056c36dbdac1e5f20c7
ms.sourcegitcommit: 1d8f1396ccc66a0c3fcb5e5f36ea29b50db6d92a
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/01/2020
ms.locfileid: "80501229"
---
# <a name="add-download-and-delete-custom-user-data-to-identity-in-an-aspnet-core-project"></a>Добавление, загрузка и удаление пользовательских пользовательских данных пользователей в Identity в проекте ASP.NET Core

Автор: [Рик Андерсон (Rick Anderson)](https://twitter.com/RickAndMSFT)

В этой статье показано, как сделать следующее:

* Добавьте пользовательские пользовательские данные в веб-приложение ASP.NET Core.
* Отметьте пользовательскую модель <xref:Microsoft.AspNetCore.Identity.PersonalDataAttribute> пользовательских данных с помощью атрибута, чтобы она была автоматически доступна для скачивания и удаления. Возможность загрузки и удаления данных помогает удовлетворить требования [GDPR.](xref:security/gdpr)

Образец проекта создается из веб-приложения Razor Pages, но инструкции аналогичны для веб-приложения core MVC ASP.NET Core.

[Просмотр или загрузка образца кода](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/security/authentication/add-user-data) [(как скачать)](xref:index#how-to-download-a-sample)

## <a name="prerequisites"></a>Предварительные требования

::: moniker range=">= aspnetcore-3.0"

[!INCLUDE [](~/includes/3.0-SDK.md)]

::: moniker-end

::: moniker range="< aspnetcore-3.0"

[!INCLUDE [](~/includes/2.2-SDK.md)]

::: moniker-end

## <a name="create-a-razor-web-app"></a>Создание веб-приложения Razor

# <a name="visual-studio"></a>[Visual Studio](#tab/visual-studio)

::: moniker range=">= aspnetcore-3.0"

* В Visual Studio в меню **Файл** щелкните **Создать** > **Проект**. Назовите проект **WebApp1,** если хотите, чтобы он соответствовал названию [кода скачать образец.](https://github.com/dotnet/AspNetCore.Docs/tree/live/aspnetcore/security/authentication/add-user-data)
* Выберите **ASP.NET основных веб-приложений** > **OK**
* Выберите **ASP.NET Core 3.0** в выпадении
* Выберите **WEB-приложение** > **OK**
* Постройте и запустите проект.

::: moniker-end

::: moniker range="< aspnetcore-3.0"

* В Visual Studio в меню **Файл** щелкните **Создать** > **Проект**. Назовите проект **WebApp1,** если хотите, чтобы он соответствовал названию [кода скачать образец.](https://github.com/dotnet/AspNetCore.Docs/tree/live/aspnetcore/security/authentication/add-user-data)
* Выберите **ASP.NET основных веб-приложений** > **OK**
* Выберите **ASP.NET Core 2.2** в выпадении
* Выберите **WEB-приложение** > **OK**
* Постройте и запустите проект.

::: moniker-end


# <a name="net-core-cli"></a>[Интерфейс командной строки .NET Core](#tab/netcore-cli)

```dotnetcli
dotnet new webapp -o WebApp1
```

---

## <a name="run-the-identity-scaffolder"></a>Выполнить идентичность леса

# <a name="visual-studio"></a>[Visual Studio](#tab/visual-studio)

* От **решения Explorer**, право нажмите на проект > **Добавить** > **новые Scaffolded пункт**.
* Из левой панели диалога **Добавить Scaffold** выберите **Identity** > **Add.**
* В диалоге **Add Identity** следующие варианты:
  * Выберите существующий файл *макета : Страницы/Общие/_Layout.cshtml*
  * Выберите следующие файлы для переопределения:
    * **Учетная запись/регистрация**
    * **Учетная запись/Управление/Индекс**
  * Выберите **+** кнопку для создания нового **класса контекста данных.** Примите тип (**WebApp1.Models.WebApp1Контекст,** если проект называется **WebApp1**).
  * Выберите **+** кнопку для создания нового **класса пользователя.** Примите тип **(WebApp1User,** если проект называется **WebApp1)**> **Добавить**.
* Выберите **Добавить**.

# <a name="net-core-cli"></a>[Интерфейс командной строки .NET Core](#tab/netcore-cli)

Если ранее вы не установили эшафот ASP.NET Core, установите его сейчас:

```dotnetcli
dotnet tool install -g dotnet-aspnet-codegenerator
```

Добавьте ссылку на пакет к файлу [проекта](https://www.nuget.org/packages/Microsoft.VisualStudio.Web.CodeGeneration.Design/) (.csproj). Выполнить следующую команду в каталоге проекта:

```dotnetcli
dotnet add package Microsoft.VisualStudio.Web.CodeGeneration.Design
dotnet restore
```

Выполнить следующую команду, чтобы перечислить параметры леса Identity:

```dotnetcli
dotnet aspnet-codegenerator identity -h
```

В папке проекта запустите эшафот Identity:

```dotnetcli
dotnet aspnet-codegenerator identity -u WebApp1User -fi Account.Register;Account.Manage.Index
```

---

Следуйте инструкциям в [Миграция, UseAuthentication, и макет](xref:security/authentication/scaffold-identity#efm) для выполнения следующих шагов:

* Создайте миграцию и обновите базу данных.
* Добавлен `UseAuthentication` в `Startup.Configure`.
* Добавьте `<partial name="_LoginPartial" />` в файл макета.
* Проверьте работу приложения:
  * Регистрация пользователя
  * Выберите новое имя пользователя (рядом со ссылкой **На Logout).** Возможно, потребуется расширить окно или выбрать значок панели навигации, чтобы показать имя пользователя и другие ссылки.
  * Выберите вкладку **«Личные данные».**
  * Выберите кнопку **Загрузка** и изучили файл *PersonalData.json.*
  * Проверьте кнопку **«Удалить»,** которая удаляет зарегистрированное на пользователя.

## <a name="add-custom-user-data-to-the-identity-db"></a>Добавление пользовательских пользовательских данных в DB Identity

Обновление `IdentityUser` производного класса с пользовательскими свойствами. Если вы назвали проект WebApp1, файл называется *области / Identity/Data/WebApp1User.cs*. Обновление файла со следующим кодом:

::: moniker range=">= aspnetcore-3.0"

[!code-csharp[](add-user-data/samples/3.x/SampleApp/Areas/Identity/Data/WebApp1User.cs)]

::: moniker-end

::: moniker range="< aspnetcore-3.0"

[!code-csharp[](add-user-data/samples/2.x/SampleApp/Areas/Identity/Data/WebApp1User.cs)]

::: moniker-end

Свойства с атрибутом [PersonalData:](/dotnet/api/microsoft.aspnetcore.identity.personaldataattribute)

* Удален, когда *области / идентичность / Страницы / учетная запись / Управление / DeletePersonalData.cshtml* Razor Page звонки `UserManager.Delete`.
* Включено в загруженные данные *по странице Areas/Identity/Pages/Account/Manage/DownloadPersonalData.cshtml* Razor Page.

### <a name="update-the-accountmanageindexcshtml-page"></a>Обновление страницы счета/Управления/Индекса.cshtml

Обновление `InputModel` в *областях / Идентичность / Страницы / Счет / Управление / Index.cshtml.cs* со следующим выделенным кодом:

::: moniker range=">= aspnetcore-3.0"

[!code-csharp[](add-user-data/samples/3.x/SampleApp/Areas/Identity/Pages/Account/Manage/Index.cshtml.cs?name=snippet&highlight=24-32,48-49,96-104,106)]

Обновление *зон/идентификации/Страницы/Счет/Управление/Индекс.cshtml* со следующей выделенной разметкой:

[!code-cshtml[](add-user-data/samples/3.x/SampleApp/Areas/Identity/Pages/Account/Manage/Index.cshtml?highlight=18-25)]

::: moniker-end

::: moniker range="< aspnetcore-3.0"

[!code-csharp[](add-user-data/samples/2.x/SampleApp/Areas/Identity/Pages/Account/Manage/Index.cshtml.cs?name=snippet&highlight=28-36,63-64,98-106,119)]

Обновление *зон/идентификации/Страницы/Счет/Управление/Индекс.cshtml* со следующей выделенной разметкой:

[!code-cshtml[](add-user-data/samples/2.x/SampleApp/Areas/Identity/Pages/Account/Manage/Index.cshtml?highlight=35-42)]

::: moniker-end

### <a name="update-the-accountregistercshtml-page"></a>Обновление страницы Счета/Register.cshtml

Обновление `InputModel` в *области / идентичность / Страницы / Счета / Register.cshtml.cs* со следующим выделенным кодом:

::: moniker range=">= aspnetcore-3.0"

[!code-csharp[](add-user-data/samples/3.x/SampleApp/Areas/Identity/Pages/Account/Register.cshtml.cs?name=snippet&highlight=30-38,70-71)]

Обновление *зон/идентификации/Страницы/Счет/Register.cshtml* со следующей выделенной разметкой:

[!code-cshtml[](add-user-data/samples/3.x/SampleApp/Areas/Identity/Pages/Account/Register.cshtml?highlight=16-25)]

::: moniker-end

::: moniker range="< aspnetcore-3.0"

[!code-csharp[](add-user-data/samples/2.x/SampleApp/Areas/Identity/Pages/Account/Register.cshtml.cs?name=snippet&highlight=28-36,67,66)]

Обновление *зон/идентификации/Страницы/Счет/Register.cshtml* со следующей выделенной разметкой:

[!code-cshtml[](add-user-data/samples/2.x/SampleApp/Areas/Identity/Pages/Account/Register.cshtml?highlight=16-25)]

::: moniker-end


Создайте проект.

### <a name="add-a-migration-for-the-custom-user-data"></a>Добавление миграции для пользовательских пользовательских данных пользователя

# <a name="visual-studio"></a>[Visual Studio](#tab/visual-studio)

В визуальной студии **пакет менеджер консоли**:

```powershell
Add-Migration CustomUserData
Update-Database
```

# <a name="net-core-cli"></a>[Интерфейс командной строки .NET Core](#tab/netcore-cli)

```dotnetcli
dotnet ef migrations add CustomUserData
dotnet ef database update
```

---

## <a name="test-create-view-download-delete-custom-user-data"></a>Тест создать, просмотреть, скачать, удалить пользовательские данные пользователя

Проверьте работу приложения:

* Зарегистрируйте нового пользователя.
* Просмотр пользовательских пользовательских `/Identity/Account/Manage` данных пользователей на странице.
* Загрузка и просмотр персональных `/Identity/Account/Manage/PersonalData` данных пользователей со страницы.

## <a name="add-claims-to-identity-using-iuserclaimsprincipalfactoryapplicationuser"></a>Добавление претензий к идентификации с помощью IUserClaimsPrincipalFactory<ApplicationUser>

Дополнительные требования могут быть добавлены `IUserClaimsPrincipalFactory<T>` к ASP.NET Core Identity с помощью интерфейса. Этот класс можно добавить в `Startup.ConfigureServices` приложение в методе. Добавьте пользовательскую реализацию класса следующим образом:

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddIdentity<ApplicationUser, IdentityRole>()
        .AddEntityFrameworkStores<ApplicationDbContext>()
        .AddDefaultTokenProviders();

    services.AddScoped<IUserClaimsPrincipalFactory<ApplicationUser>, 
        AdditionalUserClaimsPrincipalFactory>();
```

Демо-код `ApplicationUser` использует класс. Этот класс `IsAdmin` добавляет свойство, которое используется для добавления дополнительной претензии.

```csharp
public class ApplicationUser : IdentityUser
{
    public bool IsAdmin { get; set; }
}
```

Класс `AdditionalUserClaimsPrincipalFactory` реализует интерфейс `UserClaimsPrincipalFactory`. Новая претензия роли добавляется к `ClaimsPrincipal`.

```csharp
public class AdditionalUserClaimsPrincipalFactory 
        : UserClaimsPrincipalFactory<ApplicationUser, IdentityRole>
{
    public AdditionalUserClaimsPrincipalFactory( 
        UserManager<ApplicationUser> userManager,
        RoleManager<IdentityRole> roleManager, 
        IOptions<IdentityOptions> optionsAccessor) 
        : base(userManager, roleManager, optionsAccessor)
    {}

    public async override Task<ClaimsPrincipal> CreateAsync(ApplicationUser user)
    {
        var principal = await base.CreateAsync(user);
        var identity = (ClaimsIdentity)principal.Identity;

        var claims = new List<Claim>();
        if (user.IsAdmin)
        {
            claims.Add(new Claim(JwtClaimTypes.Role, "admin"));
        }
        else
        {
            claims.Add(new Claim(JwtClaimTypes.Role, "user"));
        }

        identity.AddClaims(claims);
        return principal;
    }
}
```

Дополнительная претензия может быть использована в приложении. На странице Razor `IAuthorizationService` экземпляр может использоваться для доступа к значению претензии.

```cshtml
@using Microsoft.AspNetCore.Authorization
@inject IAuthorizationService AuthorizationService

@if ((await AuthorizationService.AuthorizeAsync(User, "IsAdmin")).Succeeded)
{
    <ul class="mr-auto navbar-nav">
        <li class="nav-item">
            <a class="nav-link" asp-controller="Admin" asp-action="Index">ADMIN</a>
        </li>
    </ul>
}
```
