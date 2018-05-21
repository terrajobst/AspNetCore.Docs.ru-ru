---
title: Авторизация на основе утверждений в ASP.NET Core
author: rick-anderson
description: Дополнительные сведения о добавлении проверки утверждений для авторизации в приложении ASP.NET Core.
manager: wpickett
ms.author: riande
ms.date: 10/14/2016
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: security/authorization/claims
ms.openlocfilehash: 2464f8cac720dcf5de02f2679e9450e8b77de3ee
ms.sourcegitcommit: 24c32648ab0c6f0be15333d7c23c1bf680858c43
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 05/20/2018
---
# <a name="claims-based-authorization-in-aspnet-core"></a>Авторизация на основе утверждений в ASP.NET Core

<a name="security-authorization-claims-based"></a>

При создании удостоверение может быть присвоено одно или несколько утверждений, выданных доверенной стороной. Утверждения, является пара имя-значение, представляющее какие субъекта, можно сделать, не какие субъекта. Например имеется водительское удостоверение, выданный центром локального определяющим лицензии. Ваше водительское удостоверение есть дата рождения. В этом случае будет имя утверждения `DateOfBirth`, значение утверждения будет дата рождения, например `8th June 1970` и издатель будет определяющим центра лицензии. Авторизация на основе утверждений, в самом простом случае проверяет значение утверждения и разрешает доступ к ресурсу на основе этого значения. Например, если требуется доступ к сообщество ночь процесс авторизации может быть:

Сотрудник отдела безопасности дверца будет возвращаться значение даты рождения утверждения и ли они доверять издателя (определяющим центра лицензии) перед предоставлением доступа.

Удостоверение может содержать несколько утверждений с несколькими значениями и может содержать несколько утверждений того же типа.

## <a name="adding-claims-checks"></a>Добавление утверждений проверки

Утверждения проверки авторизации на основе декларативного — разработчик внедряет их в свой код от контроллера или действия в контроллере, указав утверждения, которые текущий пользователь должен обладать и при необходимости выполнять значение утверждения для доступа к Запрошенный ресурс. Утверждения, что требования, на основе политики, разработчику необходимо построить и зарегистрировать программные требования утверждений политики.

Простейший тип утверждения политики выглядит на наличие утверждения и не проверяет значение.

Сначала необходимо для создания и регистрации политики. Это происходит в процессе настройки службы авторизации, которой обычно принимает участие в `ConfigureServices()` в ваш *файла Startup.cs* файл.

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

В этом случае `EmployeeOnly` политика проверяет наличие `EmployeeNumber` утверждения для текущего удостоверения.

Затем примените политику с помощью `Policy` свойство `AuthorizeAttribute` атрибута, чтобы указать имя политики;

```csharp
[Authorize(Policy = "EmployeeOnly")]
public IActionResult VacationBalance()
{
    return View();
}
```

`AuthorizeAttribute` Атрибут может применяться к контроллеру целиком в данном экземпляре только политика сопоставления удостоверений будет разрешен доступ к любому действию на контроллере.

```csharp
[Authorize(Policy = "EmployeeOnly")]
public class VacationController : Controller
{
    public ActionResult VacationBalance()
    {
    }
}
```

При наличии контроллера, который защищен службой `AuthorizeAttribute` атрибута, но хотите разрешить анонимный доступ для применения каких-либо действий `AllowAnonymousAttribute` атрибута.

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

Большинство утверждений имеют значение. При создании политики можно указать список допустимых значений. Следующий пример бы только успешно для сотрудников, чей номер сотрудника был 1, 2, 3, 4 или 5.

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

### <a name="add-a-generic-claim-check"></a>Добавьте проверку универсального утверждения

Если значение утверждения не одно значение или преобразование не требуется, используйте [RequireAssertion](/dotnet/api/microsoft.aspnetcore.authorization.authorizationpolicybuilder.requireassertion). Дополнительные сведения см. в разделе [с помощью func для выполнения политики](xref:security/authorization/policies#using-a-func-to-fulfill-a-policy).

## <a name="multiple-policy-evaluation"></a>Несколько оценки политики

Если применяется несколько политик контроллер или действие, все политики необходимо передать перед предоставлением доступа. Пример:

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

В приведенном выше примере все удостоверения, что удовлетворяет `EmployeeOnly` политики можно получить доступ к `Payslip` действия, что эта политика применяется на контроллере. Однако для вызова `UpdateSalary` действия должны выполняться удостоверение *оба* `EmployeeOnly` политики и `HumanResources` политики.

Если требуется более сложных политик, например переводить Дата рождения утверждения, вычисление age от него, а затем проверка возраст 21 или более ранних версий, а затем нужно написать [обработчики настраиваемой политики](xref:security/authorization/policies).
