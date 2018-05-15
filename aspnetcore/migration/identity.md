---
title: Перенести проверку подлинности и удостоверение в ASP.NET Core
author: ardalis
description: Дополнительные сведения о миграции проверку подлинности и удостоверение из проекта ASP.NET MVC в проект ASP.NET Core MVC.
manager: wpickett
ms.author: riande
ms.date: 10/14/2016
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: migration/identity
ms.openlocfilehash: 2a80274e9056b41e370f199c7d41865db5fcedd7
ms.sourcegitcommit: 477d38e33530a305405eaf19faa29c6d805273aa
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 05/08/2018
---
# <a name="migrate-authentication-and-identity-to-aspnet-core"></a><span data-ttu-id="621b3-103">Перенести проверку подлинности и удостоверение в ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="621b3-103">Migrate Authentication and Identity to ASP.NET Core</span></span>

<span data-ttu-id="621b3-104">Автор: [Стив Смит](https://ardalis.com/) (Steve Smith)</span><span class="sxs-lookup"><span data-stu-id="621b3-104">By [Steve Smith](https://ardalis.com/)</span></span>

<span data-ttu-id="621b3-105">В предыдущей статье мы [конфигурации, перенесенные из проекта ASP.NET MVC в ASP.NET Core MVC](xref:migration/configuration).</span><span class="sxs-lookup"><span data-stu-id="621b3-105">In the previous article, we [migrated configuration from an ASP.NET MVC project to ASP.NET Core MVC](xref:migration/configuration).</span></span> <span data-ttu-id="621b3-106">В этой статье мы переносим функции управления регистрации, имя входа и пользователя.</span><span class="sxs-lookup"><span data-stu-id="621b3-106">In this article, we migrate the registration, login, and user management features.</span></span>

## <a name="configure-identity-and-membership"></a><span data-ttu-id="621b3-107">Настройте удостоверение и членства</span><span class="sxs-lookup"><span data-stu-id="621b3-107">Configure Identity and Membership</span></span>

<span data-ttu-id="621b3-108">В ASP.NET MVC компоненты проверки подлинности и удостоверение настроены с использованием ASP.NET Identity в *Startup.Auth.cs* и *IdentityConfig.cs*, расположенного в *App_Start* папка.</span><span class="sxs-lookup"><span data-stu-id="621b3-108">In ASP.NET MVC, authentication and identity features are configured using ASP.NET Identity in *Startup.Auth.cs* and *IdentityConfig.cs*, located in the *App_Start* folder.</span></span> <span data-ttu-id="621b3-109">В ASP.NET MVC основных компонентов, эти функции настраиваются в *файла Startup.cs*.</span><span class="sxs-lookup"><span data-stu-id="621b3-109">In ASP.NET Core MVC, these features are configured in *Startup.cs*.</span></span>

<span data-ttu-id="621b3-110">Установка `Microsoft.AspNetCore.Identity.EntityFrameworkCore` и `Microsoft.AspNetCore.Authentication.Cookies` пакеты NuGet.</span><span class="sxs-lookup"><span data-stu-id="621b3-110">Install the `Microsoft.AspNetCore.Identity.EntityFrameworkCore` and `Microsoft.AspNetCore.Authentication.Cookies` NuGet packages.</span></span>

<span data-ttu-id="621b3-111">Затем откройте *файла Startup.cs* и обновить `Startup.ConfigureServices` метод для использования платформы Entity Framework и удостоверение службы:</span><span class="sxs-lookup"><span data-stu-id="621b3-111">Then, open *Startup.cs* and update the `Startup.ConfigureServices` method to use Entity Framework and Identity services:</span></span>

```csharp
public void ConfigureServices(IServiceCollection services)
{
    // Add EF services to the services container.
    services.AddDbContext<ApplicationDbContext>(options =>
        options.UseSqlServer(Configuration.GetConnectionString("DefaultConnection")));

    services.AddIdentity<ApplicationUser, IdentityRole>()
        .AddEntityFrameworkStores<ApplicationDbContext>()
        .AddDefaultTokenProviders();

     services.AddMvc();
}
```

<span data-ttu-id="621b3-112">На этом этапе существует два типа, на который ссылается приведенный выше код, который мы еще не были перенесены из проекта ASP.NET MVC: `ApplicationDbContext` и `ApplicationUser`.</span><span class="sxs-lookup"><span data-stu-id="621b3-112">At this point, there are two types referenced in the above code that we haven't yet migrated from the ASP.NET MVC project: `ApplicationDbContext` and `ApplicationUser`.</span></span> <span data-ttu-id="621b3-113">Создайте новый *моделей* папки в ASP.NET Core проект и добавить в него соответствующий эти типы двух классов.</span><span class="sxs-lookup"><span data-stu-id="621b3-113">Create a new *Models* folder in the ASP.NET Core project, and add two classes to it corresponding to these types.</span></span> <span data-ttu-id="621b3-114">Вы найдете ASP.NET MVC версии этих классов в */Models/IdentityModels.cs*, но мы будем использовать один файл для каждого класса в перенесенном проекте, так как это более четко.</span><span class="sxs-lookup"><span data-stu-id="621b3-114">You will find the ASP.NET MVC versions of these classes in */Models/IdentityModels.cs*, but we will use one file per class in the migrated project since that's more clear.</span></span>

<span data-ttu-id="621b3-115">*ApplicationUser.cs*:</span><span class="sxs-lookup"><span data-stu-id="621b3-115">*ApplicationUser.cs*:</span></span>

```csharp
using Microsoft.AspNetCore.Identity.EntityFrameworkCore;

namespace NewMvcProject.Models
{
  public class ApplicationUser : IdentityUser
  {
  }
}
```

<span data-ttu-id="621b3-116">*ApplicationDbContext.cs*:</span><span class="sxs-lookup"><span data-stu-id="621b3-116">*ApplicationDbContext.cs*:</span></span>

```csharp
using Microsoft.AspNetCore.Identity.EntityFramework;
using Microsoft.Data.Entity;

namespace NewMvcProject.Models
{
    public class ApplicationDbContext : IdentityDbContext<ApplicationUser>
    {
        public ApplicationDbContext(DbContextOptions<ApplicationDbContext> options)
            : base(options)
        {
        }

        protected override void OnModelCreating(ModelBuilder builder)
        {
            base.OnModelCreating(builder);
            // Customize the ASP.NET Identity model and override the defaults if needed.
            // For example, you can rename the ASP.NET Identity table names and more.
            // Add your customizations after calling base.OnModelCreating(builder);
        }
    }
}
```

<span data-ttu-id="621b3-117">ASP.NET MVC Core Starter, веб-проекта не содержит много настройки пользователей, или `ApplicationDbContext`.</span><span class="sxs-lookup"><span data-stu-id="621b3-117">The ASP.NET Core MVC Starter Web project doesn't include much customization of users, or the `ApplicationDbContext`.</span></span> <span data-ttu-id="621b3-118">При миграции настоящее приложение, необходимо перенести все пользовательские свойства и методы пользователя приложения и `DbContext` классов, а также любые другие классы модели, использует приложение.</span><span class="sxs-lookup"><span data-stu-id="621b3-118">When migrating a real app, you also need to migrate all of the custom properties and methods of your app's user and `DbContext` classes, as well as any other Model classes your app utilizes.</span></span> <span data-ttu-id="621b3-119">Например если ваш `DbContext` имеет `DbSet<Album>`, необходимые для миграции `Album` класса.</span><span class="sxs-lookup"><span data-stu-id="621b3-119">For example, if your `DbContext` has a `DbSet<Album>`, you need to migrate the `Album` class.</span></span>

<span data-ttu-id="621b3-120">С этими файлами на месте *файла Startup.cs* файл можно сделать для компиляции, обновив его `using` инструкции:</span><span class="sxs-lookup"><span data-stu-id="621b3-120">With these files in place, the *Startup.cs* file can be made to compile by updating its `using` statements:</span></span>

```csharp
using Microsoft.AspNetCore.Builder;
using Microsoft.AspNetCore.Identity;
using Microsoft.AspNetCore.Hosting;
using Microsoft.EntityFrameworkCore;
using Microsoft.Extensions.Configuration;
using Microsoft.Extensions.DependencyInjection;
```

<span data-ttu-id="621b3-121">Нашего приложения готов для поддержки проверки подлинности и удостоверение службы.</span><span class="sxs-lookup"><span data-stu-id="621b3-121">Our app is now ready to support authentication and Identity services.</span></span> <span data-ttu-id="621b3-122">Необходимо только эти функции доступны для пользователей.</span><span class="sxs-lookup"><span data-stu-id="621b3-122">It just needs to have these features exposed to users.</span></span>

## <a name="migrate-registration-and-login-logic"></a><span data-ttu-id="621b3-123">Перенести логику регистрации и входа в систему</span><span class="sxs-lookup"><span data-stu-id="621b3-123">Migrate registration and login logic</span></span>

<span data-ttu-id="621b3-124">Настроено для приложения службы удостоверений и доступа к данным, настроенные с помощью платформы Entity Framework и SQL Server все готово для добавления поддержки для регистрации и входа в приложение.</span><span class="sxs-lookup"><span data-stu-id="621b3-124">With Identity services configured for the app and data access configured using Entity Framework and SQL Server, we're ready to add support for registration and login to the app.</span></span> <span data-ttu-id="621b3-125">Помните, что [ранее в процессе миграции](xref:migration/mvc#migrate-the-layout-file) мы закомментирован ссылку на *_LoginPartial* в *_Layout.cshtml*.</span><span class="sxs-lookup"><span data-stu-id="621b3-125">Recall that [earlier in the migration process](xref:migration/mvc#migrate-the-layout-file) we commented out a reference to *_LoginPartial* in *_Layout.cshtml*.</span></span> <span data-ttu-id="621b3-126">Теперь пора вернуться к этому коду Раскомментировать ее, а затем добавьте необходимые контроллеры и представления для поддержки функции входа в систему.</span><span class="sxs-lookup"><span data-stu-id="621b3-126">Now it's time to return to that code, uncomment it, and add in the necessary controllers and views to support login functionality.</span></span>

<span data-ttu-id="621b3-127">Раскомментируйте `@Html.Partial` строку в *_Layout.cshtml*:</span><span class="sxs-lookup"><span data-stu-id="621b3-127">Uncomment the `@Html.Partial` line in *_Layout.cshtml*:</span></span>

```cshtml
      <li>@Html.ActionLink("Contact", "Contact", "Home")</li>
    </ul>
    @*@Html.Partial("_LoginPartial")*@
  </div>
</div>
```

<span data-ttu-id="621b3-128">Теперь добавьте новый Razor представление с именем *_LoginPartial* для *представления/Общие* папки:</span><span class="sxs-lookup"><span data-stu-id="621b3-128">Now, add a new Razor view called *_LoginPartial* to the *Views/Shared* folder:</span></span>

<span data-ttu-id="621b3-129">Обновление *_LoginPartial.cshtml* с помощью следующего кода (заменить все его содержимое):</span><span class="sxs-lookup"><span data-stu-id="621b3-129">Update *_LoginPartial.cshtml* with the following code (replace all of its contents):</span></span>

```cshtml
@inject SignInManager<ApplicationUser> SignInManager
@inject UserManager<ApplicationUser> UserManager

@if (SignInManager.IsSignedIn(User))
{
    <form asp-area="" asp-controller="Account" asp-action="Logout" method="post" id="logoutForm" class="navbar-right">
        <ul class="nav navbar-nav navbar-right">
            <li>
                <a asp-area="" asp-controller="Manage" asp-action="Index" title="Manage">Hello @UserManager.GetUserName(User)!</a>
            </li>
            <li>
                <button type="submit" class="btn btn-link navbar-btn navbar-link">Log out</button>
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

<span data-ttu-id="621b3-130">На этом этапе можно будет обновить веб-сайт в браузере.</span><span class="sxs-lookup"><span data-stu-id="621b3-130">At this point, you should be able to refresh the site in your browser.</span></span>

## <a name="summary"></a><span data-ttu-id="621b3-131">Сводка</span><span class="sxs-lookup"><span data-stu-id="621b3-131">Summary</span></span>

<span data-ttu-id="621b3-132">ASP.NET Core вносит изменения в функциях ASP.NET Identity.</span><span class="sxs-lookup"><span data-stu-id="621b3-132">ASP.NET Core introduces changes to the ASP.NET Identity features.</span></span> <span data-ttu-id="621b3-133">В этой статье были рассмотрены вопросы миграции функции управления проверки подлинности и пользователь в ASP.NET Identity в ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="621b3-133">In this article, you have seen how to migrate the authentication and user management features of ASP.NET Identity to ASP.NET Core.</span></span>
