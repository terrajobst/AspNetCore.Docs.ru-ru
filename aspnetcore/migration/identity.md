---
title: Миграция проверки подлинности и удостоверения в ASP.NET Core
author: ardalis
description: Узнайте, как перенести проверку подлинности и удостоверение из проекта ASP.NET MVC в проекте ASP.NET Core MVC.
ms.author: riande
ms.date: 10/14/2016
uid: migration/identity
ms.openlocfilehash: 72e62e78e37325ec47d54abbc11a875ae87fb63a
ms.sourcegitcommit: d53e0cc71542b92de867bcce51575b054886f529
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 08/16/2018
ms.locfileid: "41838326"
---
# <a name="migrate-authentication-and-identity-to-aspnet-core"></a>Миграция проверки подлинности и удостоверения в ASP.NET Core

Автор: [Стив Смит](https://ardalis.com/) (Steve Smith)

В предыдущей статье мы [конфигурации, перенесенные из проекта ASP.NET MVC в ASP.NET Core MVC](xref:migration/configuration). В этой статье мы переносим функции управления регистрации, входа и пользователя.

## <a name="configure-identity-and-membership"></a>Настройка удостоверений и членства

В ASP.NET MVC, настройки проверки подлинности и идентификации компонентов с помощью ASP.NET Identity в *Startup.Auth.cs* и *IdentityConfig.cs*, который находится в *App_Start* папка. В ASP.NET Core MVC, эти функции настраиваются в *Startup.cs*.

Установка `Microsoft.AspNetCore.Identity.EntityFrameworkCore` и `Microsoft.AspNetCore.Authentication.Cookies` пакеты NuGet.

Затем откройте *Startup.cs* и обновить `Startup.ConfigureServices` метод использовать Entity Framework и удостоверение службы:

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

На этом этапе существует два типа, на которые ссылается приведенный выше код, который мы еще не перенесены из проекта ASP.NET MVC: `ApplicationDbContext` и `ApplicationUser`. Создайте новый *моделей* папку в ASP.NET Core проекта, а также добавить в него соответствующие этим типам двух классов. Вы найдете ASP.NET MVC версии этих классов в */Models/IdentityModels.cs*, однако мы будем использовать один файл для каждого класса переноса проекта, так как это более четко.

*ApplicationUser.cs*:

```csharp
using Microsoft.AspNetCore.Identity.EntityFrameworkCore;

namespace NewMvcProject.Models
{
  public class ApplicationUser : IdentityUser
  {
  }
}
```

*ApplicationDbContext.cs*:

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

Проект ASP.NET Core MVC Starter Web не включать требующих многих настроек пользователей, или `ApplicationDbContext`. При миграции в реальном приложении, нужно перенести все пользовательские свойства и методы пользователя вашего приложения и `DbContext` классов, а также любые другие классы модели, использует приложение. Например если ваш `DbContext` имеет `DbSet<Album>`, вам потребуется выполнять миграцию `Album` класса.

С этими файлами на месте *Startup.cs* файл может быть сделан для компиляции, обновив его `using` инструкции:

```csharp
using Microsoft.AspNetCore.Builder;
using Microsoft.AspNetCore.Identity;
using Microsoft.AspNetCore.Hosting;
using Microsoft.EntityFrameworkCore;
using Microsoft.Extensions.Configuration;
using Microsoft.Extensions.DependencyInjection;
```

Наше приложение теперь готов для поддержки проверки подлинности и службы идентификации. Он просто должен иметь эти функции, предоставляемые пользователям.

## <a name="migrate-registration-and-login-logic"></a>Перенести логику регистрации и входа в систему

Службы удостоверений, настроенного для приложения, и доступ к данным, настроенные с помощью Entity Framework и SQL Server мы готовы добавить поддержку для регистрации и входа в приложение. Помните, что [ранее в процессе миграции](xref:migration/mvc#migrate-the-layout-file) мы закомментирован ссылку *_LoginPartial* в *_Layout.cshtml*. Теперь пора вернуться в этот код, раскомментируйте его, а также добавить необходимые контроллеры и представления для поддержки функции входа в систему.

Раскомментируйте `@Html.Partial` строку в *_Layout.cshtml*:

```cshtml
      <li>@Html.ActionLink("Contact", "Contact", "Home")</li>
    </ul>
    @*@Html.Partial("_LoginPartial")*@
  </div>
</div>
```

Теперь добавьте новое представление Razor вызывается *_LoginPartial* для *Views/Shared* папку:

Обновление *_LoginPartial.cshtml* следующим кодом (Замените все его содержимое):

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

На этом этапе можно будет обновить веб-сайт в браузере.

## <a name="summary"></a>Сводка

ASP.NET Core представлены изменения функций ASP.NET Identity. В этой статье было показано, как перенести функции управления проверки подлинности и пользователя в ASP.NET Identity в ASP.NET Core.
