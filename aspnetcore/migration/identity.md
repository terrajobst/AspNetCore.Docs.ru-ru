---
title: Перенесите проверку подлинности и удостоверение в ASP.NET Core
author: ardalis
description: Узнайте, как перенести проверку подлинности и удостоверение из проекта ASP.NET MVC в проект ASP.NET Core MVC.
ms.author: riande
ms.date: 10/14/2016
uid: migration/identity
ms.openlocfilehash: f821930dbd36de18db31104cddf34c563009a506
ms.sourcegitcommit: 476ea5ad86a680b7b017c6f32098acd3414c0f6c
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 08/14/2019
ms.locfileid: "69022271"
---
# <a name="migrate-authentication-and-identity-to-aspnet-core"></a>Перенесите проверку подлинности и удостоверение в ASP.NET Core

Автор: [Стив Смит](https://ardalis.com/) (Steve Smith)

В предыдущей статье мы перенесли [конфигурацию из проекта ASP.NET MVC в ASP.NET Core MVC](xref:migration/configuration). В этой статье мы переносим функции регистрации, входа и управления пользователями.

## <a name="configure-identity-and-membership"></a>Настройка удостоверений и членства

В ASP.NET MVC функции проверки подлинности и идентификации настраиваются с помощью ASP.NET Identity в *Startup.auth.CS* и *IdentityConfig.CS*, расположенных в папке *App_Start* . В ASP.NET Core MVC эти функции настраиваются в *Startup.CS*.

Установите пакеты NuGet `Microsoft.AspNetCore.Authentication.Cookies`и. `Microsoft.AspNetCore.Identity.EntityFrameworkCore`

Затем откройте *Startup.CS* и обновите `Startup.ConfigureServices` метод, чтобы использовать Entity Framework и службы удостоверений:

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

На этом этапе существует два типа, упоминаемых в приведенном выше коде, которые еще не перенесены из проекта MVC ASP.NET `ApplicationDbContext` : `ApplicationUser`и. Создайте новую папку *Models* в проекте ASP.NET Core и добавьте в нее два класса, соответствующие этим типам. Версии ASP.NET MVC этих классов можно найти в */Моделс/идентитимоделс.КС*, но мы будем использовать по одному файлу для каждого класса в перенесенном проекте, так как это более ясно.

*ApplicationUser.CS*:

```csharp
using Microsoft.AspNetCore.Identity.EntityFrameworkCore;

namespace NewMvcProject.Models
{
  public class ApplicationUser : IdentityUser
  {
  }
}
```

*ApplicationDbContext.CS*:

```csharp
using Microsoft.AspNetCore.Identity.EntityFrameworkCore;
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

ASP.NET Coreный веб-проект MVC не включает в `ApplicationDbContext`себя значительную настройку пользователей или. При переносе реального приложения необходимо также перенести все пользовательские свойства и методы пользователя и `DbContext` классов приложения, а также любые другие классы модели, используемые вашим приложением. Например, если `DbSet<Album>`у вас `DbContext` есть, необходимо перенести `Album` класс.

Используя эти файлы, можно выполнить компиляцию файла *Startup.CS* , обновив `using` инструкции:

```csharp
using Microsoft.AspNetCore.Builder;
using Microsoft.AspNetCore.Identity;
using Microsoft.AspNetCore.Hosting;
using Microsoft.EntityFrameworkCore;
using Microsoft.Extensions.Configuration;
using Microsoft.Extensions.DependencyInjection;
```

Теперь наше приложение готово к поддержке проверки подлинности и служб удостоверений. Они просто должны предоставлять эти функции пользователям.

## <a name="migrate-registration-and-login-logic"></a>Миграция логики регистрации и входа в систему

Используя службы удостоверений, настроенные для доступа к приложениям и данным, настроенные с помощью Entity Framework и SQL Server, мы готовы добавить поддержку регистрации и входа в приложение. Вспомним, что [ранее в процессе миграции](xref:migration/mvc#migrate-the-layout-file) мы заносим ссылку на *_LoginPartial* в *_layout. cshtml*. Теперь пришло время вернуться к этому коду, раскомментировать его и добавить необходимые контроллеры и представления для поддержки функций входа.

Раскомментируйте `@Html.Partial` строку в *_layout. cshtml*:

```cshtml
      <li>@Html.ActionLink("Contact", "Contact", "Home")</li>
    </ul>
    @*@Html.Partial("_LoginPartial")*@
  </div>
</div>
```

Теперь добавьте новое представление Razor с именем *_LoginPartial* в папку *views/Shared* .

Обновите *_LoginPartial. cshtml* с помощью следующего кода (замените все его содержимое):

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

На этом этапе вы сможете обновить сайт в браузере.

## <a name="summary"></a>Сводка

ASP.NET Core вносит изменения в функции ASP.NET Identity. В этой статье вы узнали, как перенести функции проверки подлинности и управления пользователями ASP.NET Identity в ASP.NET Core.
