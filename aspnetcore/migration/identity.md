---
title: Миграция проверки подлинности и удостоверения в ASP.NET Core
author: ardalis
description: Узнайте, как перенести проверку подлинности и удостоверение из проекта ASP.NET MVC в проекте ASP.NET Core MVC.
ms.author: riande
ms.date: 10/14/2016
uid: migration/identity
ms.openlocfilehash: 72e62e78e37325ec47d54abbc11a875ae87fb63a
ms.sourcegitcommit: 5b0eca8c21550f95de3bb21096bd4fd4d9098026
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/27/2019
ms.locfileid: "64891881"
---
# <a name="migrate-authentication-and-identity-to-aspnet-core"></a><span data-ttu-id="1377a-103">Миграция проверки подлинности и удостоверения в ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="1377a-103">Migrate Authentication and Identity to ASP.NET Core</span></span>

<span data-ttu-id="1377a-104">Автор: [Стив Смит](https://ardalis.com/) (Steve Smith)</span><span class="sxs-lookup"><span data-stu-id="1377a-104">By [Steve Smith](https://ardalis.com/)</span></span>

<span data-ttu-id="1377a-105">В предыдущей статье мы [конфигурации, перенесенные из проекта ASP.NET MVC в ASP.NET Core MVC](xref:migration/configuration).</span><span class="sxs-lookup"><span data-stu-id="1377a-105">In the previous article, we [migrated configuration from an ASP.NET MVC project to ASP.NET Core MVC](xref:migration/configuration).</span></span> <span data-ttu-id="1377a-106">В этой статье мы переносим функции управления регистрации, входа и пользователя.</span><span class="sxs-lookup"><span data-stu-id="1377a-106">In this article, we migrate the registration, login, and user management features.</span></span>

## <a name="configure-identity-and-membership"></a><span data-ttu-id="1377a-107">Настройка удостоверений и членства</span><span class="sxs-lookup"><span data-stu-id="1377a-107">Configure Identity and Membership</span></span>

<span data-ttu-id="1377a-108">В ASP.NET MVC, настройки проверки подлинности и идентификации компонентов с помощью ASP.NET Identity в *Startup.Auth.cs* и *IdentityConfig.cs*, который находится в *App_Start* папка.</span><span class="sxs-lookup"><span data-stu-id="1377a-108">In ASP.NET MVC, authentication and identity features are configured using ASP.NET Identity in *Startup.Auth.cs* and *IdentityConfig.cs*, located in the *App_Start* folder.</span></span> <span data-ttu-id="1377a-109">В ASP.NET Core MVC, эти функции настраиваются в *Startup.cs*.</span><span class="sxs-lookup"><span data-stu-id="1377a-109">In ASP.NET Core MVC, these features are configured in *Startup.cs*.</span></span>

<span data-ttu-id="1377a-110">Установка `Microsoft.AspNetCore.Identity.EntityFrameworkCore` и `Microsoft.AspNetCore.Authentication.Cookies` пакеты NuGet.</span><span class="sxs-lookup"><span data-stu-id="1377a-110">Install the `Microsoft.AspNetCore.Identity.EntityFrameworkCore` and `Microsoft.AspNetCore.Authentication.Cookies` NuGet packages.</span></span>

<span data-ttu-id="1377a-111">Затем откройте *Startup.cs* и обновить `Startup.ConfigureServices` метод использовать Entity Framework и удостоверение службы:</span><span class="sxs-lookup"><span data-stu-id="1377a-111">Then, open *Startup.cs* and update the `Startup.ConfigureServices` method to use Entity Framework and Identity services:</span></span>

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

<span data-ttu-id="1377a-112">На этом этапе существует два типа, на которые ссылается приведенный выше код, который мы еще не перенесены из проекта ASP.NET MVC: `ApplicationDbContext` и `ApplicationUser`.</span><span class="sxs-lookup"><span data-stu-id="1377a-112">At this point, there are two types referenced in the above code that we haven't yet migrated from the ASP.NET MVC project: `ApplicationDbContext` and `ApplicationUser`.</span></span> <span data-ttu-id="1377a-113">Создайте новый *моделей* папку в ASP.NET Core проекта, а также добавить в него соответствующие этим типам двух классов.</span><span class="sxs-lookup"><span data-stu-id="1377a-113">Create a new *Models* folder in the ASP.NET Core project, and add two classes to it corresponding to these types.</span></span> <span data-ttu-id="1377a-114">Вы найдете ASP.NET MVC версии этих классов в */Models/IdentityModels.cs*, однако мы будем использовать один файл для каждого класса переноса проекта, так как это более четко.</span><span class="sxs-lookup"><span data-stu-id="1377a-114">You will find the ASP.NET MVC versions of these classes in */Models/IdentityModels.cs*, but we will use one file per class in the migrated project since that's more clear.</span></span>

<span data-ttu-id="1377a-115">*ApplicationUser.cs*:</span><span class="sxs-lookup"><span data-stu-id="1377a-115">*ApplicationUser.cs*:</span></span>

```csharp
using Microsoft.AspNetCore.Identity.EntityFrameworkCore;

namespace NewMvcProject.Models
{
  public class ApplicationUser : IdentityUser
  {
  }
}
```

<span data-ttu-id="1377a-116">*ApplicationDbContext.cs*:</span><span class="sxs-lookup"><span data-stu-id="1377a-116">*ApplicationDbContext.cs*:</span></span>

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
            // Customize the ASP.NET Core Identity model and override the defaults if needed.
            // For example, you can rename the ASP.NET Core Identity table names and more.
            // Add your customizations after calling base.OnModelCreating(builder);
        }
    }
}
```

<span data-ttu-id="1377a-117">Проект ASP.NET Core MVC Starter Web не включать требующих многих настроек пользователей, или `ApplicationDbContext`.</span><span class="sxs-lookup"><span data-stu-id="1377a-117">The ASP.NET Core MVC Starter Web project doesn't include much customization of users, or the `ApplicationDbContext`.</span></span> <span data-ttu-id="1377a-118">При миграции в реальном приложении, нужно перенести все пользовательские свойства и методы пользователя вашего приложения и `DbContext` классов, а также любые другие классы модели, использует приложение.</span><span class="sxs-lookup"><span data-stu-id="1377a-118">When migrating a real app, you also need to migrate all of the custom properties and methods of your app's user and `DbContext` classes, as well as any other Model classes your app utilizes.</span></span> <span data-ttu-id="1377a-119">Например если ваш `DbContext` имеет `DbSet<Album>`, вам потребуется выполнять миграцию `Album` класса.</span><span class="sxs-lookup"><span data-stu-id="1377a-119">For example, if your `DbContext` has a `DbSet<Album>`, you need to migrate the `Album` class.</span></span>

<span data-ttu-id="1377a-120">С этими файлами на месте *Startup.cs* файл может быть сделан для компиляции, обновив его `using` инструкции:</span><span class="sxs-lookup"><span data-stu-id="1377a-120">With these files in place, the *Startup.cs* file can be made to compile by updating its `using` statements:</span></span>

```csharp
using Microsoft.AspNetCore.Builder;
using Microsoft.AspNetCore.Identity;
using Microsoft.AspNetCore.Hosting;
using Microsoft.EntityFrameworkCore;
using Microsoft.Extensions.Configuration;
using Microsoft.Extensions.DependencyInjection;
```

<span data-ttu-id="1377a-121">Наше приложение теперь готов для поддержки проверки подлинности и службы идентификации.</span><span class="sxs-lookup"><span data-stu-id="1377a-121">Our app is now ready to support authentication and Identity services.</span></span> <span data-ttu-id="1377a-122">Он просто должен иметь эти функции, предоставляемые пользователям.</span><span class="sxs-lookup"><span data-stu-id="1377a-122">It just needs to have these features exposed to users.</span></span>

## <a name="migrate-registration-and-login-logic"></a><span data-ttu-id="1377a-123">Перенести логику регистрации и входа в систему</span><span class="sxs-lookup"><span data-stu-id="1377a-123">Migrate registration and login logic</span></span>

<span data-ttu-id="1377a-124">Службы удостоверений, настроенного для приложения, и доступ к данным, настроенные с помощью Entity Framework и SQL Server мы готовы добавить поддержку для регистрации и входа в приложение.</span><span class="sxs-lookup"><span data-stu-id="1377a-124">With Identity services configured for the app and data access configured using Entity Framework and SQL Server, we're ready to add support for registration and login to the app.</span></span> <span data-ttu-id="1377a-125">Помните, что [ранее в процессе миграции](xref:migration/mvc#migrate-the-layout-file) мы закомментирован ссылку *_LoginPartial* в *_Layout.cshtml*.</span><span class="sxs-lookup"><span data-stu-id="1377a-125">Recall that [earlier in the migration process](xref:migration/mvc#migrate-the-layout-file) we commented out a reference to *_LoginPartial* in *_Layout.cshtml*.</span></span> <span data-ttu-id="1377a-126">Теперь пора вернуться в этот код, раскомментируйте его, а также добавить необходимые контроллеры и представления для поддержки функции входа в систему.</span><span class="sxs-lookup"><span data-stu-id="1377a-126">Now it's time to return to that code, uncomment it, and add in the necessary controllers and views to support login functionality.</span></span>

<span data-ttu-id="1377a-127">Раскомментируйте `@Html.Partial` строку в *_Layout.cshtml*:</span><span class="sxs-lookup"><span data-stu-id="1377a-127">Uncomment the `@Html.Partial` line in *_Layout.cshtml*:</span></span>

```cshtml
      <li>@Html.ActionLink("Contact", "Contact", "Home")</li>
    </ul>
    @*@Html.Partial("_LoginPartial")*@
  </div>
</div>
```

<span data-ttu-id="1377a-128">Теперь добавьте новое представление Razor вызывается *_LoginPartial* для *Views/Shared* папку:</span><span class="sxs-lookup"><span data-stu-id="1377a-128">Now, add a new Razor view called *_LoginPartial* to the *Views/Shared* folder:</span></span>

<span data-ttu-id="1377a-129">Обновление *_LoginPartial.cshtml* следующим кодом (Замените все его содержимое):</span><span class="sxs-lookup"><span data-stu-id="1377a-129">Update *_LoginPartial.cshtml* with the following code (replace all of its contents):</span></span>

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

<span data-ttu-id="1377a-130">На этом этапе можно будет обновить веб-сайт в браузере.</span><span class="sxs-lookup"><span data-stu-id="1377a-130">At this point, you should be able to refresh the site in your browser.</span></span>

## <a name="summary"></a><span data-ttu-id="1377a-131">Сводка</span><span class="sxs-lookup"><span data-stu-id="1377a-131">Summary</span></span>

<span data-ttu-id="1377a-132">ASP.NET Core представлены изменения функций ASP.NET Identity.</span><span class="sxs-lookup"><span data-stu-id="1377a-132">ASP.NET Core introduces changes to the ASP.NET Identity features.</span></span> <span data-ttu-id="1377a-133">В этой статье было показано, как перенести функции управления проверки подлинности и пользователя в ASP.NET Identity в ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="1377a-133">In this article, you have seen how to migrate the authentication and user management features of ASP.NET Identity to ASP.NET Core.</span></span>
