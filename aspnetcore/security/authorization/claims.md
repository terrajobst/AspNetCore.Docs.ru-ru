---
title: "Авторизация на основе утверждений"
author: rick-anderson
description: "В этом документе описывается добавление проверки утверждений для авторизации в приложении ASP.NET Core."
manager: wpickett
ms.author: riande
ms.date: 10/14/2016
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: security/authorization/claims
ms.openlocfilehash: 76b6566df4a427836eb5060f7d80e1039e479884
ms.sourcegitcommit: a510f38930abc84c4b302029d019a34dfe76823b
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/30/2018
---
# <a name="claims-based-authorization"></a>Авторизация на основе утверждений

<a name="security-authorization-claims-based"></a>

При создании удостоверение может быть присвоено одно или несколько утверждений, выданных доверенной стороной. Утверждение-это имя значение пара, которая представляет какие субъекта, можно сделать, не какие субъекта. Например имеется водительское удостоверение, выданный центром локального определяющим лицензии. Ваше водительское удостоверение есть дата рождения. В этом случае будет имя утверждения `DateOfBirth`, значение утверждения будет дата рождения, например `8th June 1970` и издатель будет определяющим центра лицензии. Авторизация на основе утверждений, в самом простом случае проверяет значение утверждения и разрешает доступ к ресурсу на основе этого значения. Например, если требуется доступ к сообщество ночь процесс авторизации может быть:

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

Если требуется более сложных политик, например переводить Дата рождения утверждения, вычисление age от него, а затем проверка возраст 21 или более ранних версий, а затем нужно написать [обработчики настраиваемой политики](policies.md).
