---
title: Авторизация на основе утверждений в ASP.NET Core
author: rick-anderson
description: Узнайте, как добавить проверки утверждений для авторизации в ASP.NET Core приложении.
ms.author: riande
ms.date: 10/14/2016
uid: security/authorization/claims
ms.openlocfilehash: e289851aafcbc7e3b3f60ab9fbe4b182a78bdf8a
ms.sourcegitcommit: 9a129f5f3e31cc449742b164d5004894bfca90aa
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/06/2020
ms.locfileid: "78652972"
---
# <a name="claims-based-authorization-in-aspnet-core"></a>Авторизация на основе утверждений в ASP.NET Core

<a name="security-authorization-claims-based"></a>

При создании удостоверения ему может быть назначено одно или несколько утверждений, выданных доверенной стороной. Утверждение — это пара "имя-значение", которая представляет предметную тему, а не то, что может делать субъект. Например, у вас может быть лицензия драйвера, выданная локальным центром лицензий. У лицензии вашего драйвера есть Дата рождения. В этом случае имя утверждения будет `DateOfBirth`, значение утверждения — это Дата рождения, например `8th June 1970` и издатель будет центром сертификации. Авторизация на основе утверждений, в самом простом, проверяет значение утверждения и разрешает доступ к ресурсу на основе этого значения. Например, если требуется доступ к ночному клубу, процесс авторизации может быть следующим:

Директор по безопасности дверцы вычислит значение даты утверждения о рождении и получит ли он доверие к издателю (Управление центром лицензий) перед предоставлением доступа.

Удостоверение может содержать несколько утверждений с несколькими значениями и может содержать несколько утверждений одного типа.

## <a name="adding-claims-checks"></a>Добавление проверок утверждений

Проверки авторизации на основе утверждений являются декларативными — разработчик внедряет их в код для контроллера или действия в пределах контроллера, указывая утверждения, которыми должен обладать текущий пользователь, и, при необходимости, значение, которое должно храниться в заявке для доступа к запрошенный ресурс. Требования к утверждениям основаны на политиках. разработчик должен создать и зарегистрировать политику, которая выражает требования к утверждениям.

Простейший тип политики утверждений ищет наличие утверждения и не проверяет его.

Сначала необходимо создать и зарегистрировать политику. Это происходит как часть конфигурации службы авторизации, которая обычно принимает участие в `ConfigureServices()` в файле *Startup.CS* .

::: moniker range=">= aspnetcore-3.0"

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddControllersWithViews();
    services.AddRazorPages();

    services.AddAuthorization(options =>
    {
        options.AddPolicy("EmployeeOnly", policy => policy.RequireClaim("EmployeeNumber"));
    });
}
```

::: moniker-end

::: moniker range="< aspnetcore-3.0"

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddMvc();

    services.AddAuthorization(options =>
    {
        options.AddPolicy("EmployeeOnly", policy => policy.RequireClaim("EmployeeNumber"));
    });
}
```

::: moniker-end

В этом случае `EmployeeOnly` политика проверяет наличие `EmployeeNumber` утверждения для текущего удостоверения.

Затем примените политику, используя свойство `Policy` атрибута `AuthorizeAttribute`, чтобы указать имя политики.

```csharp
[Authorize(Policy = "EmployeeOnly")]
public IActionResult VacationBalance()
{
    return View();
}
```

Атрибут `AuthorizeAttribute` можно применить ко всему контроллеру. в этом экземпляре будет разрешен доступ к любому действию на контроллере только в удостоверениях, соответствующих политике.

```csharp
[Authorize(Policy = "EmployeeOnly")]
public class VacationController : Controller
{
    public ActionResult VacationBalance()
    {
    }
}
```

Если у вас есть контроллер, защищенный атрибутом `AuthorizeAttribute`, но хотите разрешить анонимный доступ к определенным действиям, примените атрибут `AllowAnonymousAttribute`.

```csharp
[Authorize(Policy = "EmployeeOnly")]
public class VacationController : Controller
{
    public ActionResult VacationBalance()
    {
    }

    [AllowAnonymous]
    public ActionResult VacationPolicy()
    {
    }
}
```

Большинство заявок поставляются со значением. При создании политики можно указать список допустимых значений. Следующий пример будет выполнен только для сотрудников, чей номер сотрудника был 1, 2, 3, 4 или 5.

::: moniker range=">= aspnetcore-3.0"

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddControllersWithViews();
    services.AddRazorPages();

    services.AddAuthorization(options =>
    {
        options.AddPolicy("Founders", policy =>
                          policy.RequireClaim("EmployeeNumber", "1", "2", "3", "4", "5"));
    });
}
```

::: moniker-end

::: moniker range="< aspnetcore-3.0"

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddMvc();

    services.AddAuthorization(options =>
    {
        options.AddPolicy("Founders", policy =>
                          policy.RequireClaim("EmployeeNumber", "1", "2", "3", "4", "5"));
    });
}
```

::: moniker-end
### <a name="add-a-generic-claim-check"></a>Добавление проверки универсального утверждения

Если значение утверждения не является одним значением или требуется преобразование, используйте [рекуиреассертион](/dotnet/api/microsoft.aspnetcore.authorization.authorizationpolicybuilder.requireassertion). Дополнительные сведения см. в разделе [Использование функции func для выполнения политики](xref:security/authorization/policies#using-a-func-to-fulfill-a-policy).

## <a name="multiple-policy-evaluation"></a>Оценка нескольких политик

При применении нескольких политик к контроллеру или действию все политики должны пройти до предоставления доступа. Пример:

```csharp
[Authorize(Policy = "EmployeeOnly")]
public class SalaryController : Controller
{
    public ActionResult Payslip()
    {
    }

    [Authorize(Policy = "HumanResources")]
    public ActionResult UpdateSalary()
    {
    }
}
```

В приведенном выше примере любое удостоверение, удовлетворяющее политике `EmployeeOnly`, может получить доступ к действию `Payslip`, так как эта политика применяется на контроллере. Однако для вызова действия `UpdateSalary` удостоверение должно выполнять как политику `EmployeeOnly`, *так* и политику `HumanResources`.

Если вы хотите использовать более сложные политики, например, выполнив утверждение о рождении, вычисляя возраст из него, а затем проверив возраст на 21 или более раннюю версию, необходимо написать [пользовательские обработчики политик](xref:security/authorization/policies).
