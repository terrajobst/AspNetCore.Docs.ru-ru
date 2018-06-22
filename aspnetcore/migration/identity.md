---
title: Перенести проверку подлинности и удостоверение в ASP.NET Core
author: ardalis
description: Дополнительные сведения о миграции проверку подлинности и удостоверение из проекта ASP.NET MVC в проект ASP.NET Core MVC.
ms.author: riande
ms.date: 10/14/2016
uid: migration/identity
ms.openlocfilehash: e05d72ca78c7b8191a47f78cda31ee40e04d0706
ms.sourcegitcommit: a1afd04758e663d7062a5bfa8a0d4dca38f42afc
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 06/20/2018
ms.locfileid: "36275701"
---
# <a name="migrate-authentication-and-identity-to-aspnet-core"></a>Перенести проверку подлинности и удостоверение в ASP.NET Core

Автор: [Стив Смит](https://ardalis.com/) (Steve Smith)

В предыдущей статье мы [конфигурации, перенесенные из проекта ASP.NET MVC в ASP.NET Core MVC](xref:migration/configuration). В этой статье мы переносим функции управления регистрации, имя входа и пользователя.

## <a name="configure-identity-and-membership"></a>Настройте удостоверение и членства

В ASP.NET MVC компоненты проверки подлинности и удостоверение настроены с использованием ASP.NET Identity в *Startup.Auth.cs* и *IdentityConfig.cs*, расположенного в *App_Start* папка. В ASP.NET MVC основных компонентов, эти функции настраиваются в *файла Startup.cs*.

Установка `Microsoft.AspNetCore.Identity.EntityFrameworkCore` и `Microsoft.AspNetCore.Authentication.Cookies` пакеты NuGet.

Затем откройте *файла Startup.cs* и обновить `Startup.ConfigureServices` метод для использования платформы Entity Framework и удостоверение службы:

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

На этом этапе существует два типа, на который ссылается приведенный выше код, который мы еще не были перенесены из проекта ASP.NET MVC: `ApplicationDbContext` и `ApplicationUser`. Создайте новый *моделей* папки в ASP.NET Core проект и добавить в него соответствующий эти типы двух классов. Вы найдете ASP.NET MVC версии этих классов в */Models/IdentityModels.cs*, но мы будем использовать один файл для каждого класса в перенесенном проекте, так как это более четко.

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
            // Customize the ASP.NET Identity model and override the defaults if needed.
            // For example, you can rename the ASP.NET Identity table names and more.
            // Add your customizations after calling base.OnModelCreating(builder);
        }
    }
}
```

ASP.NET MVC Core Starter, веб-проекта не содержит много настройки пользователей, или `ApplicationDbContext`. При миграции настоящее приложение, необходимо перенести все пользовательские свойства и методы пользователя приложения и `DbContext` классов, а также любые другие классы модели, использует приложение. Например если ваш `DbContext` имеет `DbSet<Album>`, необходимые для миграции `Album` класса.

С этими файлами на месте *файла Startup.cs* файл можно сделать для компиляции, обновив его `using` инструкции:

```csharp
using Microsoft.AspNetCore.Builder;
using Microsoft.AspNetCore.Identity;
using Microsoft.AspNetCore.Hosting;
using Microsoft.EntityFrameworkCore;
using Microsoft.Extensions.Configuration;
using Microsoft.Extensions.DependencyInjection;
```

Нашего приложения готов для поддержки проверки подлинности и удостоверение службы. Необходимо только эти функции доступны для пользователей.

## <a name="migrate-registration-and-login-logic"></a>Перенести логику регистрации и входа в систему

Настроено для приложения службы удостоверений и доступа к данным, настроенные с помощью платформы Entity Framework и SQL Server все готово для добавления поддержки для регистрации и входа в приложение. Помните, что [ранее в процессе миграции](xref:migration/mvc#migrate-the-layout-file) мы закомментирован ссылку на *_LoginPartial* в *_Layout.cshtml*. Теперь пора вернуться к этому коду Раскомментировать ее, а затем добавьте необходимые контроллеры и представления для поддержки функции входа в систему.

Раскомментируйте `@Html.Partial` строку в *_Layout.cshtml*:

```cshtml
      <li>@Html.ActionLink("Contact", "Contact", "Home")</li>
    </ul>
    @*@Html.Partial("_LoginPartial")*@
  </div>
</div>
```

Теперь добавьте новый Razor представление с именем *_LoginPartial* для *представления/Общие* папки:

Обновление *_LoginPartial.cshtml* с помощью следующего кода (заменить все его содержимое):

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

ASP.NET Core вносит изменения в функциях ASP.NET Identity. В этой статье были рассмотрены вопросы миграции функции управления проверки подлинности и пользователь в ASP.NET Identity в ASP.NET Core.
