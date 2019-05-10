---
title: Авторизация на основе заявок в ASP.NET Core
author: rick-anderson
description: Сведения о добавлении проверки утверждений для авторизации в приложении ASP.NET Core.
ms.author: riande
ms.date: 10/14/2016
uid: security/authorization/claims
ms.openlocfilehash: 6b60ae5515819b017ab577f655ed91ee4d8ed0dd
ms.sourcegitcommit: dd9c73db7853d87b566eef136d2162f648a43b85
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 05/06/2019
ms.locfileid: "65086153"
---
# <a name="claims-based-authorization-in-aspnet-core"></a>Авторизация на основе заявок в ASP.NET Core

<a name="security-authorization-claims-based"></a>

При создании удостоверение может быть присвоено одно или несколько утверждений, выданных доверенным лицом. Утверждения — пары имя/значение, представляющее какие субъекта, не какие субъекта можно сделать. Например возможно, водительские, выданный локальной вождения центром лицензии. Удостоверения имеет Дата рождения нужна на нем. В этом случае утверждение будет носить имя `DateOfBirth`, значение утверждения будет дата рождения нужна, например `8th June 1970` издателя будет вождения центра лицензии. Авторизация на основе утверждений, в самом простом случае проверяет значение утверждения и позволяет получить доступ к ресурсу на основе этого значения. Например, если требуется доступ к ночной клуб процесс авторизации могут быть:

Выдано директору по безопасности дверцу оценит значение Дата рождения утверждения и ли они доверять издателю (вождения центр лицензии) перед предоставлением доступа.

Удостоверение может содержать несколько утверждений с несколькими значениями и может содержать несколько утверждений того же типа.

## <a name="adding-claims-checks"></a>Добавление утверждений проверок

Утверждение авторизации на основе проверки являются декларативными — разработчик внедряет их в свой код от контроллера или действия в контроллере, указав утверждения, которые текущий пользователь должен обладать и при необходимости выполнять значение утверждения для доступа Запрошенный ресурс. Утверждения, что требования на основе политик, разработчик должен создать и зарегистрировать выражения требования утверждения политики.

Простейший тип утверждения политики выглядит на наличие утверждения и не проверяет значение.

Сначала необходимо создать и зарегистрировать политики. Это происходит так, как часть конфигурации службы авторизации, который обычно принимает участие в `ConfigureServices()` в вашей *Startup.cs* файла.

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

В этом случае `EmployeeOnly` политика проверяет наличие `EmployeeNumber` утверждения от текущего удостоверения.

Затем применить политику с использованием `Policy` свойство `AuthorizeAttribute` атрибут, чтобы указать имя политики;

```csharp
[Authorize(Policy = "EmployeeOnly")]
public IActionResult VacationBalance()
{
    return View();
}
```

`AuthorizeAttribute` Атрибут может применяться для всего контроллера, в данном экземпляре, только для удостоверения, политика сопоставления будет разрешен доступ к любому действию контроллера.

```csharp
[Authorize(Policy = "EmployeeOnly")]
public class VacationController : Controller
{
    public ActionResult VacationBalance()
    {
    }
}
```

Если у вас есть контроллер, который защищен службой `AuthorizeAttribute` атрибута, но, чтобы разрешить анонимный доступ для определенных действий, можно применить `AllowAnonymousAttribute` атрибута.

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

Большинство утверждения поступают со значением. При создании политики можно указать список допустимых значений. Следующий пример бы только успешно для сотрудников employee, номер которого был 1, 2, 3, 4 или 5.

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

### <a name="add-a-generic-claim-check"></a>Добавить проверку универсального утверждения

Если значение утверждения не одно значение или преобразование является обязательным, используйте [RequireAssertion](/dotnet/api/microsoft.aspnetcore.authorization.authorizationpolicybuilder.requireassertion). Дополнительные сведения см. в разделе [с помощью func для выполнения политики](xref:security/authorization/policies#using-a-func-to-fulfill-a-policy).

## <a name="multiple-policy-evaluation"></a>Несколько оценки политики

Если применить несколько политик к контроллеру или действию, все политики должен пройти перед предоставлением доступа. Пример:

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

В приведенном выше примере любое удостоверение, что удовлетворяет `EmployeeOnly` политики могут иметь доступ к `Payslip` действия, как эта политика применяется на контроллере. Тем не менее для вызова `UpdateSalary` удостоверение необходимо выполнить действие *оба* `EmployeeOnly` политики и `HumanResources` политики.

Если требуется более сложные политики, например переводить дату рождения утверждения, вычисление возраст от него, а затем проверка возраст 21 или более ранней версии, а затем вам нужно написать [обработчики настраиваемой политики](xref:security/authorization/policies).
