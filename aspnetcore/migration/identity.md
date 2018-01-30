---
title: "Миграция проверку подлинности и удостоверение"
author: ardalis
description: 
manager: wpickett
ms.author: riande
ms.date: 10/14/2016
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: migration/identity
ms.openlocfilehash: f02d9472ea0aa1dceae3f53c812776aab85ab54e
ms.sourcegitcommit: a510f38930abc84c4b302029d019a34dfe76823b
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/30/2018
---
# <a name="migrating-authentication-and-identity"></a><span data-ttu-id="8121c-102">Миграция проверку подлинности и удостоверение</span><span class="sxs-lookup"><span data-stu-id="8121c-102">Migrating Authentication and Identity</span></span>

<a name="migration-identity"></a>

<span data-ttu-id="8121c-103">По [Стив Смит](https://ardalis.com/)</span><span class="sxs-lookup"><span data-stu-id="8121c-103">By [Steve Smith](https://ardalis.com/)</span></span>

<span data-ttu-id="8121c-104">В предыдущей статье мы [конфигурации, перенесенные из проекта ASP.NET MVC в ASP.NET Core MVC](configuration.md).</span><span class="sxs-lookup"><span data-stu-id="8121c-104">In the previous article we [migrated configuration from an ASP.NET MVC project to ASP.NET Core MVC](configuration.md).</span></span> <span data-ttu-id="8121c-105">В этой статье мы переносим функции управления регистрации, имя входа и пользователя.</span><span class="sxs-lookup"><span data-stu-id="8121c-105">In this article, we migrate the registration, login, and user management features.</span></span>

## <a name="configure-identity-and-membership"></a><span data-ttu-id="8121c-106">Настройте удостоверение и членства</span><span class="sxs-lookup"><span data-stu-id="8121c-106">Configure Identity and Membership</span></span>

<span data-ttu-id="8121c-107">В ASP.NET MVC компоненты проверки подлинности и удостоверение настраиваются с помощью ASP.NET Identity в Startup.Auth.cs и IdentityConfig.cs, расположенный в папке App_Start.</span><span class="sxs-lookup"><span data-stu-id="8121c-107">In ASP.NET MVC, authentication and identity features are configured using ASP.NET Identity in Startup.Auth.cs and IdentityConfig.cs, located in the App_Start folder.</span></span> <span data-ttu-id="8121c-108">В ASP.NET MVC основных компонентов, эти функции настраиваются в *файла Startup.cs*.</span><span class="sxs-lookup"><span data-stu-id="8121c-108">In ASP.NET Core MVC, these features are configured in *Startup.cs*.</span></span>

<span data-ttu-id="8121c-109">Установка `Microsoft.AspNetCore.Identity.EntityFrameworkCore` и `Microsoft.AspNetCore.Authentication.Cookies` пакеты NuGet.</span><span class="sxs-lookup"><span data-stu-id="8121c-109">Install the `Microsoft.AspNetCore.Identity.EntityFrameworkCore` and `Microsoft.AspNetCore.Authentication.Cookies` NuGet packages.</span></span>

<span data-ttu-id="8121c-110">Затем откройте файла Startup.cs и обновление `ConfigureServices()` метод для использования платформы Entity Framework и удостоверение службы:</span><span class="sxs-lookup"><span data-stu-id="8121c-110">Then, open Startup.cs and update the `ConfigureServices()` method to use Entity Framework and Identity services:</span></span>

```csharp
public void ConfigureServices(IServiceCollection services)
{
  // Add EF services to the services container.
  services.AddEntityFramework(Configuration)
    .AddSqlServer()
    .AddDbContext<ApplicationDbContext>();

  // Add Identity services to the services container.
  services.AddIdentity<ApplicationUser, IdentityRole>(Configuration)
    .AddEntityFrameworkStores<ApplicationDbContext>();

  services.AddMvc();
}
```

<span data-ttu-id="8121c-111">На этом этапе существует два типа, на который ссылается приведенный выше код, который мы еще не были перенесены из проекта ASP.NET MVC: `ApplicationDbContext` и `ApplicationUser`.</span><span class="sxs-lookup"><span data-stu-id="8121c-111">At this point, there are two types referenced in the above code that we haven't yet migrated from the ASP.NET MVC project: `ApplicationDbContext` and `ApplicationUser`.</span></span> <span data-ttu-id="8121c-112">Создайте новый *моделей* папки в ASP.NET Core проект и добавить в него соответствующий эти типы двух классов.</span><span class="sxs-lookup"><span data-stu-id="8121c-112">Create a new *Models* folder in the ASP.NET Core project, and add two classes to it corresponding to these types.</span></span> <span data-ttu-id="8121c-113">Вы найдете ASP.NET MVC версии этих классов в `/Models/IdentityModels.cs`, но мы будем использовать один файл для каждого класса в перенесенном проекте, так как это более четко.</span><span class="sxs-lookup"><span data-stu-id="8121c-113">You will find the ASP.NET MVC versions of these classes in `/Models/IdentityModels.cs`, but we will use one file per class in the migrated project since that's more clear.</span></span>

<span data-ttu-id="8121c-114">ApplicationUser.cs:</span><span class="sxs-lookup"><span data-stu-id="8121c-114">ApplicationUser.cs:</span></span>

```csharp
using Microsoft.AspNetCore.Identity.EntityFrameworkCore;

namespace NewMvc6Project.Models
{
  public class ApplicationUser : IdentityUser
  {
  }
}
```

<span data-ttu-id="8121c-115">ApplicationDbContext.cs:</span><span class="sxs-lookup"><span data-stu-id="8121c-115">ApplicationDbContext.cs:</span></span>

```csharp
using Microsoft.AspNetCore.Identity.EntityFramework;
using Microsoft.Data.Entity;

namespace NewMvc6Project.Models
{
  public class ApplicationDbContext : IdentityDbContext<ApplicationUser>
  {
    public ApplicationDbContext()
    {
      Database.EnsureCreated();
    }

    protected override void OnConfiguring(DbContextOptionsBuilder options)
    {
      options.UseSqlServer();
    }
  }
}
```

<span data-ttu-id="8121c-116">ASP.NET MVC Core Starter, веб-проекта не содержит много настройки пользователей или ApplicationDbContext.</span><span class="sxs-lookup"><span data-stu-id="8121c-116">The ASP.NET Core MVC Starter Web project doesn't include much customization of users, or the ApplicationDbContext.</span></span> <span data-ttu-id="8121c-117">При миграции в реальном приложении, также необходимо перенести все пользовательские свойства и методы пользователя вашего приложения и DbContext классов, а также любые другие классы модели, которую ваше приложение использует (например, если вашей DbContext DbSet<Album>, конечно, потребуется миграция альбом класса).</span><span class="sxs-lookup"><span data-stu-id="8121c-117">When migrating a real application, you will also need to migrate all of the custom properties and methods of your application's user and DbContext classes, as well as any other Model classes your application utilizes (for example, if your DbContext has a DbSet<Album>, you will of course need to migrate the Album class).</span></span>

<span data-ttu-id="8121c-118">С этими файлами на месте в файле Startup.cs файл можно сделать для компиляции, обновляя ее с помощью инструкций:</span><span class="sxs-lookup"><span data-stu-id="8121c-118">With these files in place, the Startup.cs file can be made to compile by updating its using statements:</span></span>

```csharp
using Microsoft.Framework.ConfigurationModel;
using Microsoft.AspNetCore.Hosting;
using NewMvc6Project.Models;
using Microsoft.AspNetCore.Identity;
```

<span data-ttu-id="8121c-119">Наше приложение готов для поддержки проверки подлинности и удостоверение службы — так же, их необходимо установить эти функции, предоставляемые пользователям.</span><span class="sxs-lookup"><span data-stu-id="8121c-119">Our application is now ready to support authentication and identity services - it just needs to have these features exposed to users.</span></span>

## <a name="migrate-registration-and-login-logic"></a><span data-ttu-id="8121c-120">Перенос регистрации и входа в систему логику</span><span class="sxs-lookup"><span data-stu-id="8121c-120">Migrate Registration and Login Logic</span></span>

<span data-ttu-id="8121c-121">С помощью службы удостоверений, настроенного для приложения и доступа к данным, настроенные с помощью платформы Entity Framework и SQL Server мы теперь готовы для добавления поддержки для регистрации и входа в приложение.</span><span class="sxs-lookup"><span data-stu-id="8121c-121">With identity services configured for the application and data access configured using Entity Framework and SQL Server, we are now ready to add support for registration and login to the application.</span></span> <span data-ttu-id="8121c-122">Помните, что [ранее в процессе миграции](mvc.md#migrate-layout-file) мы закомментирован ссылку на _LoginPartial в _Layout.cshtml.</span><span class="sxs-lookup"><span data-stu-id="8121c-122">Recall that [earlier in the migration process](mvc.md#migrate-layout-file) we commented out a reference to _LoginPartial in _Layout.cshtml.</span></span> <span data-ttu-id="8121c-123">Теперь пора вернуться к этому коду Раскомментировать ее, а затем добавьте необходимые контроллеры и представления для поддержки функции входа в систему.</span><span class="sxs-lookup"><span data-stu-id="8121c-123">Now it's time to return to that code, uncomment it, and add in the necessary controllers and views to support login functionality.</span></span>

<span data-ttu-id="8121c-124">_Layout.cshtml обновления; Раскомментируйте @Html.Partial строки:</span><span class="sxs-lookup"><span data-stu-id="8121c-124">Update _Layout.cshtml; uncomment the @Html.Partial line:</span></span>

```cshtml
      <li>@Html.ActionLink("Contact", "Contact", "Home")</li>
    </ul>
    @*@Html.Partial("_LoginPartial")*@
  </div>
</div>
```

<span data-ttu-id="8121c-125">Теперь добавьте страница представления MVC вызывается _LoginPartial для представления/общие папки.</span><span class="sxs-lookup"><span data-stu-id="8121c-125">Now, add a new MVC View Page called _LoginPartial to the Views/Shared folder:</span></span>

<span data-ttu-id="8121c-126">Обновить с помощью следующего кода _LoginPartial.cshtml (заменить все его содержимое):</span><span class="sxs-lookup"><span data-stu-id="8121c-126">Update _LoginPartial.cshtml with the following code (replace all of its contents):</span></span>

```cshtml
@inject SignInManager<User> SignInManager
@inject UserManager<User> UserManager

@if (SignInManager.IsSignedIn(User))
{
    <form asp-area="" asp-controller="Account" asp-action="LogOff" method="post" id="logoutForm" class="navbar-right">
        <ul class="nav navbar-nav navbar-right">
            <li>
                <a asp-area="" asp-controller="Manage" asp-action="Index" title="Manage">Hello @UserManager.GetUserName(User)!</a>
            </li>
            <li>
                <button type="submit" class="btn btn-link navbar-btn navbar-link">Log off</button>
            </li>
        </ul>
    </form>
}
else
{
    <ul class="nav navbar-nav navbar-right">
        <li><a asp-area="" asp-controller="Account" asp-action="Register">Register</a></li>
        <li><a asp-area="" asp-controller="Account" asp-action="Login">Log in</a></li>
    </ul>
}
```

<span data-ttu-id="8121c-127">На этом этапе можно будет обновить веб-сайт в браузере.</span><span class="sxs-lookup"><span data-stu-id="8121c-127">At this point, you should be able to refresh the site in your browser.</span></span>

## <a name="summary"></a><span data-ttu-id="8121c-128">Сводка</span><span class="sxs-lookup"><span data-stu-id="8121c-128">Summary</span></span>

<span data-ttu-id="8121c-129">ASP.NET Core вносит изменения в функциях ASP.NET Identity.</span><span class="sxs-lookup"><span data-stu-id="8121c-129">ASP.NET Core introduces changes to the ASP.NET Identity features.</span></span> <span data-ttu-id="8121c-130">В этой статье были рассмотрены вопросы миграции функции управления проверки подлинности и пользователь удостоверения ASP.NET в ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="8121c-130">In this article, you have seen how to migrate the authentication and user management features of an ASP.NET Identity to ASP.NET Core.</span></span>
