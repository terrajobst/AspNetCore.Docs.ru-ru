---
title: "Миграция проверку подлинности и удостоверение"
author: ardalis
description: 
ms.author: riande
manager: wpickett
ms.date: 10/14/2016
ms.topic: article
ms.technology: aspnet
ms.prod: asp.net-core
uid: migration/identity
ms.openlocfilehash: a3aa976578d8db089c69bf888f492f9cd7df824b
ms.sourcegitcommit: 3e303620a125325bb9abd4b2d315c106fb8c47fd
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/19/2018
---
# <a name="migrating-authentication-and-identity"></a>Миграция проверку подлинности и удостоверение

<a name="migration-identity"></a>

По [Стив Смит](https://ardalis.com/)

В предыдущей статье мы [конфигурации, перенесенные из проекта ASP.NET MVC в ASP.NET Core MVC](configuration.md). В этой статье мы переносим функции управления регистрации, имя входа и пользователя.

## <a name="configure-identity-and-membership"></a>Настройте удостоверение и членства

В ASP.NET MVC компоненты проверки подлинности и удостоверение настраиваются с помощью ASP.NET Identity в Startup.Auth.cs и IdentityConfig.cs, расположенный в папке App_Start. В ASP.NET MVC основных компонентов, эти функции настраиваются в *файла Startup.cs*.

Установка `Microsoft.AspNetCore.Identity.EntityFrameworkCore` и `Microsoft.AspNetCore.Authentication.Cookies` пакеты NuGet.

Затем откройте файла Startup.cs и обновление `ConfigureServices()` метод для использования платформы Entity Framework и удостоверение службы:

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

На этом этапе существует два типа, на который ссылается приведенный выше код, который мы еще не были перенесены из проекта ASP.NET MVC: `ApplicationDbContext` и `ApplicationUser`. Создайте новый *моделей* папки в ASP.NET Core проект и добавить в него соответствующий эти типы двух классов. Вы найдете ASP.NET MVC версии этих классов в `/Models/IdentityModels.cs`, но мы будем использовать один файл для каждого класса в перенесенном проекте, так как это более четко.

ApplicationUser.cs:

```csharp
using Microsoft.AspNetCore.Identity.EntityFrameworkCore;

namespace NewMvc6Project.Models
{
  public class ApplicationUser : IdentityUser
  {
  }
}
```

ApplicationDbContext.cs:

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

ASP.NET MVC Core Starter, веб-проекта не содержит много настройки пользователей или ApplicationDbContext. При миграции в реальном приложении, также необходимо перенести все пользовательские свойства и методы пользователя вашего приложения и DbContext классов, а также любые другие классы модели, которую ваше приложение использует (например, если вашей DbContext DbSet<Album>, конечно, потребуется миграция альбом класса).

С этими файлами на месте в файле Startup.cs файл можно сделать для компиляции, обновляя ее с помощью инструкций:

```csharp
using Microsoft.Framework.ConfigurationModel;
using Microsoft.AspNetCore.Hosting;
using NewMvc6Project.Models;
using Microsoft.AspNetCore.Identity;
```

Наше приложение готов для поддержки проверки подлинности и удостоверение службы — так же, их необходимо установить эти функции, предоставляемые пользователям.

## <a name="migrate-registration-and-login-logic"></a>Перенос регистрации и входа в систему логику

С помощью службы удостоверений, настроенного для приложения и доступа к данным, настроенные с помощью платформы Entity Framework и SQL Server мы теперь готовы для добавления поддержки для регистрации и входа в приложение. Помните, что [ранее в процессе миграции](mvc.md#migrate-layout-file) мы закомментирован ссылку на _LoginPartial в _Layout.cshtml. Теперь пора вернуться к этому коду Раскомментировать ее, а затем добавьте необходимые контроллеры и представления для поддержки функции входа в систему.

_Layout.cshtml обновления; Раскомментируйте @Html.Partial строки:

```cshtml
      <li>@Html.ActionLink("Contact", "Contact", "Home")</li>
    </ul>
    @*@Html.Partial("_LoginPartial")*@
  </div>
</div>
```

Теперь добавьте страница представления MVC вызывается _LoginPartial для представления/общие папки.

Обновить с помощью следующего кода _LoginPartial.cshtml (заменить все его содержимое):

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

На этом этапе можно будет обновить веб-сайт в браузере.

## <a name="summary"></a>Сводка

ASP.NET Core вносит изменения в функциях ASP.NET Identity. В этой статье были рассмотрены вопросы миграции функции управления проверки подлинности и пользователь удостоверения ASP.NET в ASP.NET Core.
