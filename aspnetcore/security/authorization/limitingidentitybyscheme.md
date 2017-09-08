---
title: "Ограничение удостоверения по схеме"
author: rick-anderson
description: 
keywords: ASP.NET Core
ms.author: riande
manager: wpickett
ms.date: 10/14/2016
ms.topic: article
ms.assetid: d3d6ca1b-b4b5-4bf7-898e-dcd90ec1bf8c
ms.technology: aspnet
ms.prod: asp.net-core
uid: security/authorization/limitingidentitybyscheme
ms.openlocfilehash: 2483c441da317a5c29b611b3a4910eae3c01fd7a
ms.sourcegitcommit: 0b6c8e6d81d2b3c161cd375036eecbace46a9707
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 08/11/2017
---
# <a name="limiting-identity-by-scheme"></a>Ограничение удостоверения по схеме

<a name=security-authorization-limiting-by-scheme></a>

В некоторых сценариях например приложений на одной странице возможна в конечном итоге несколько методов проверки подлинности. Например приложение может использовать проверку подлинности на основе файлов cookie для входа и проверки подлинности носителя для запросов на JavaScript. В некоторых случаях может иметь несколько экземпляров промежуточного по проверки подлинности. Например два файла cookie middlewares где один содержит основные идентификаторов и один создается при многофакторной проверки подлинности активируются, так как операция, которой требуется повысить уровень защиты по запросу пользователя.

Схемы проверки подлинности имен при промежуточного по проверки подлинности настраивается во время проверки подлинности, например

```csharp
app.UseCookieAuthentication(new CookieAuthenticationOptions()
{
    AuthenticationScheme = "Cookie",
    LoginPath = new PathString("/Account/Unauthorized/"),
    AccessDeniedPath = new PathString("/Account/Forbidden/"),
    AutomaticAuthenticate = false
});

app.UseBearerAuthentication(options =>
{
    options.AuthenticationScheme = "Bearer";
    options.AutomaticAuthenticate = false;
});
```

В этой конфигурации двух middlewares проверки подлинности будут добавлены, один для файлов cookie и один для носителя.

>[!NOTE]
>При добавлении нескольких промежуточного по проверки подлинности следует убедиться, что не по промежуточного слоя настроена на автоматический запуск. Это можно сделать, задав `AutomaticAuthenticate` параметры свойству значение false. Если этого не сделать Такая фильтрация по схеме, не будет работать.

## <a name="selecting-the-scheme-with-the-authorize-attribute"></a>При выборе схемы с атрибутом авторизовать

Как без промежуточного по проверки подлинности настроен для автоматического запуска и создания удостоверения вы должны, точке авторизации выберите, какое по промежуточного слоя будет использоваться. Установите по промежуточного слоя, необходимо авторизовать с проще всего использовать `ActiveAuthenticationSchemes` свойства. Это свойство принимает разделенный запятыми список схем проверки подлинности для использования. Например,

```csharp
[Authorize(ActiveAuthenticationSchemes = "Cookie,Bearer")]
public class MixedController : Controller
```

В примере выше носителя и файл cookie middlewares запускается и иметь возможность создания и добавления удостоверение для текущего пользователя. Указав единственную схему только указанный по промежуточного слоя будет выполняться;

```csharp
[Authorize(ActiveAuthenticationSchemes = "Bearer")]
```

В этом случае будет выполняться только по промежуточного слоя с схемы носителя и будет проигнорировано всех удостоверений, основан на файле cookie.

## <a name="selecting-the-scheme-with-policies"></a>При выборе схемы с помощью политик

Чтобы задать нужный схем в [политики](policies.md#security-authorization-policies-based) можно задать `AuthenticationSchemes` коллекции при добавлении политики.

```csharp
options.AddPolicy("Over18", policy =>
{
    policy.AuthenticationSchemes.Add("Bearer");
    policy.RequireAuthenticatedUser();
    policy.Requirements.Add(new Over18Requirement());
});
```

В этом примере Over18 политики будет выполняться только для идентификаторов, созданные `Bearer` по промежуточного слоя.
