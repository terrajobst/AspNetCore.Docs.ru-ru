---
title: Написание пользовательского ПО промежуточного слоя ASP.NET Core
author: rick-anderson
description: Узнайте, как написать пользовательское ПО промежуточного слоя ASP.NET Core.
ms.author: riande
ms.custom: mvc
ms.date: 02/14/2019
uid: fundamentals/middleware/write
ms.openlocfilehash: 2c5577394a10370d92c8a83f9d806b63f3245c8b
ms.sourcegitcommit: b3894b65e313570e97a2ab78b8addd22f427cac8
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 02/23/2019
ms.locfileid: "56744922"
---
# <a name="write-custom-aspnet-core-middleware"></a>Написание пользовательского ПО промежуточного слоя ASP.NET Core

Авторы: [Рик Андерсон](https://twitter.com/RickAndMSFT) (Rick Anderson) и [Стив Смит](https://ardalis.com/) (Steve Smith)

ПО промежуточного слоя — это программное обеспечение, выстраиваемое в виде конвейера приложения для обработки запросов и откликов. ASP.NET Core предоставляет широкий набор встроенных компонентов ПО промежуточного слоя, но в некоторых случаях может потребоваться написать пользовательское ПО промежуточного слоя.

## <a name="middleware-class"></a>Класс ПО промежуточного слоя

ПО промежуточного слоя обычно инкапсулируется в класс и предоставляется с помощью метода расширения. Рассмотрим следующее ПО промежуточного слоя, которое задает язык и региональные параметры для текущего запроса из строки запроса.

[!code-csharp[](index/snapshot/Culture/StartupCulture.cs?name=snippet1)]

Приведенный выше пример кода демонстрирует создание компонента промежуточного слоя. Дополнительные сведения о встроенной поддержке локализации в ASP.NET Core см. в разделе <xref:fundamentals/localization>.

Это ПО промежуточного слоя можно проверить, передав язык и региональные параметры. Например, `http://localhost:7997/?culture=no`.

Следующий код перемещает делегат ПО промежуточного слоя в класс.

[!code-csharp[](index/snapshot/Culture/RequestCultureMiddleware.cs)]

::: moniker range="< aspnetcore-2.0"

Метод ПО промежуточного слоя `Task` должен иметь имя `Invoke`. В ASP.NET Core 2.0 и более поздних версиях имя может быть `Invoke` или `InvokeAsync`.

::: moniker-end

## <a name="middleware-extension-method"></a>Метод расширения ПО промежуточного слоя

Следующий метод расширения предоставляет ПО промежуточного слоя посредством <xref:Microsoft.AspNetCore.Builder.IApplicationBuilder>:

[!code-csharp[](index/snapshot/Culture/RequestCultureMiddlewareExtensions.cs)]

Следующий код вызывает ПО промежуточного слоя из `Startup.Configure`.

[!code-csharp[](index/snapshot/Culture/Startup.cs?name=snippet1&highlight=5)]

ПО промежуточного слоя должно соответствовать [принципу явных зависимостей](/dotnet/standard/modern-web-apps-azure-architecture/architectural-principles#explicit-dependencies), предоставляя свои зависимости в своем конструкторе. ПО промежуточного слоя создается один раз за *время существования приложения*. В разделе [Зависимости отдельных запросов](#per-request-dependencies) приведены сведения о том, как использовать службы совместно с ПО промежуточного слоя внутри запроса.

Компоненты промежуточного слоя могут разрешать свои зависимости, возникшие в результате [внедрения зависимостей](xref:fundamentals/dependency-injection), за счет параметров конструктора. [UseMiddleware&lt;T&gt;](/dotnet/api/microsoft.aspnetcore.builder.usemiddlewareextensions.usemiddleware#Microsoft_AspNetCore_Builder_UseMiddlewareExtensions_UseMiddleware_Microsoft_AspNetCore_Builder_IApplicationBuilder_System_Type_System_Object___) также может принимать дополнительные параметры напрямую.

## <a name="per-request-dependencies"></a>Зависимости отдельных запросов

Так как ПО промежуточного слоя создается при запуске приложения, а не для отдельных запросов, службы времени существования *scoped*, используемые конструкторами ПО промежуточного слоя, не являются общими с другими типами, возникшими в результате внедрения зависимостей, в каждом из запросов. Если необходимо предоставить службу *scoped* для совместного использования ПО промежуточного слоя и другими типами, добавьте ее в сигнатуру метода `Invoke`. Метод `Invoke` может принимать дополнительные параметры, заполняемые при внедрении зависимостей.

```csharp
public class CustomMiddleware
{
    private readonly RequestDelegate _next;

    public CustomMiddleware(RequestDelegate next)
    {
        _next = next;
    }

    // IMyScopedService is injected into Invoke
    public async Task Invoke(HttpContext httpContext, IMyScopedService svc)
    {
        svc.MyProperty = 1000;
        await _next(httpContext);
    }
}
```

## <a name="additional-resources"></a>Дополнительные ресурсы

* <xref:fundamentals/middleware/index>
* <xref:migration/http-modules>
* <xref:fundamentals/startup>
* <xref:fundamentals/request-features>
* <xref:fundamentals/middleware/extensibility>
* <xref:fundamentals/middleware/extensibility-third-party-container>
