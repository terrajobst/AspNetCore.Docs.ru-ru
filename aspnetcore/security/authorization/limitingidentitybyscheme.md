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
# <a name="limiting-identity-by-scheme"></a><span data-ttu-id="bf090-103">Ограничение удостоверения по схеме</span><span class="sxs-lookup"><span data-stu-id="bf090-103">Limiting identity by scheme</span></span>

<a name=security-authorization-limiting-by-scheme></a>

<span data-ttu-id="bf090-104">В некоторых сценариях например приложений на одной странице возможна в конечном итоге несколько методов проверки подлинности.</span><span class="sxs-lookup"><span data-stu-id="bf090-104">In some scenarios, such as Single Page Applications it is possible to end up with multiple authentication methods.</span></span> <span data-ttu-id="bf090-105">Например приложение может использовать проверку подлинности на основе файлов cookie для входа и проверки подлинности носителя для запросов на JavaScript.</span><span class="sxs-lookup"><span data-stu-id="bf090-105">For example, your application may use cookie-based authentication to log in and bearer authentication for JavaScript requests.</span></span> <span data-ttu-id="bf090-106">В некоторых случаях может иметь несколько экземпляров промежуточного по проверки подлинности.</span><span class="sxs-lookup"><span data-stu-id="bf090-106">In some cases you may have multiple instances of an authentication middleware.</span></span> <span data-ttu-id="bf090-107">Например два файла cookie middlewares где один содержит основные идентификаторов и один создается при многофакторной проверки подлинности активируются, так как операция, которой требуется повысить уровень защиты по запросу пользователя.</span><span class="sxs-lookup"><span data-stu-id="bf090-107">For example, two cookie middlewares where one contains a basic identity and one is created when a multi-factor authentication has triggered because the user requested an operation that requires extra security.</span></span>

<span data-ttu-id="bf090-108">Схемы проверки подлинности имен при промежуточного по проверки подлинности настраивается во время проверки подлинности, например</span><span class="sxs-lookup"><span data-stu-id="bf090-108">Authentication schemes are named when authentication middleware is configured during authentication, for example</span></span>

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

<span data-ttu-id="bf090-109">В этой конфигурации двух middlewares проверки подлинности будут добавлены, один для файлов cookie и один для носителя.</span><span class="sxs-lookup"><span data-stu-id="bf090-109">In this configuration two authentication middlewares have been added, one for cookies and one for bearer.</span></span>

>[!NOTE]
><span data-ttu-id="bf090-110">При добавлении нескольких промежуточного по проверки подлинности следует убедиться, что не по промежуточного слоя настроена на автоматический запуск.</span><span class="sxs-lookup"><span data-stu-id="bf090-110">When adding multiple authentication middleware you should ensure that no middleware is configured to run automatically.</span></span> <span data-ttu-id="bf090-111">Это можно сделать, задав `AutomaticAuthenticate` параметры свойству значение false.</span><span class="sxs-lookup"><span data-stu-id="bf090-111">You do this by setting the `AutomaticAuthenticate` options property to false.</span></span> <span data-ttu-id="bf090-112">Если этого не сделать Такая фильтрация по схеме, не будет работать.</span><span class="sxs-lookup"><span data-stu-id="bf090-112">If you fail to do this filtering by scheme will not work.</span></span>

## <a name="selecting-the-scheme-with-the-authorize-attribute"></a><span data-ttu-id="bf090-113">При выборе схемы с атрибутом авторизовать</span><span class="sxs-lookup"><span data-stu-id="bf090-113">Selecting the scheme with the Authorize attribute</span></span>

<span data-ttu-id="bf090-114">Как без промежуточного по проверки подлинности настроен для автоматического запуска и создания удостоверения вы должны, точке авторизации выберите, какое по промежуточного слоя будет использоваться.</span><span class="sxs-lookup"><span data-stu-id="bf090-114">As no authentication middleware is configured to automatically run and create an identity you must, at the point of authorization choose which middleware will be used.</span></span> <span data-ttu-id="bf090-115">Установите по промежуточного слоя, необходимо авторизовать с проще всего использовать `ActiveAuthenticationSchemes` свойства.</span><span class="sxs-lookup"><span data-stu-id="bf090-115">The simplest way to select the middleware you wish to authorize with is to use the `ActiveAuthenticationSchemes` property.</span></span> <span data-ttu-id="bf090-116">Это свойство принимает разделенный запятыми список схем проверки подлинности для использования.</span><span class="sxs-lookup"><span data-stu-id="bf090-116">This property accepts a comma delimited list of Authentication Schemes to use.</span></span> <span data-ttu-id="bf090-117">Например,</span><span class="sxs-lookup"><span data-stu-id="bf090-117">For example;</span></span>

```csharp
[Authorize(ActiveAuthenticationSchemes = "Cookie,Bearer")]
public class MixedController : Controller
```

<span data-ttu-id="bf090-118">В примере выше носителя и файл cookie middlewares запускается и иметь возможность создания и добавления удостоверение для текущего пользователя.</span><span class="sxs-lookup"><span data-stu-id="bf090-118">In the example above both the cookie and bearer middlewares will run and have a chance to create and append an identity for the current user.</span></span> <span data-ttu-id="bf090-119">Указав единственную схему только указанный по промежуточного слоя будет выполняться;</span><span class="sxs-lookup"><span data-stu-id="bf090-119">By specifying a single scheme only the specified middleware will run;</span></span>

```csharp
[Authorize(ActiveAuthenticationSchemes = "Bearer")]
```

<span data-ttu-id="bf090-120">В этом случае будет выполняться только по промежуточного слоя с схемы носителя и будет проигнорировано всех удостоверений, основан на файле cookie.</span><span class="sxs-lookup"><span data-stu-id="bf090-120">In this case only the middleware with the Bearer scheme would run, and any cookie based identities would be ignored.</span></span>

## <a name="selecting-the-scheme-with-policies"></a><span data-ttu-id="bf090-121">При выборе схемы с помощью политик</span><span class="sxs-lookup"><span data-stu-id="bf090-121">Selecting the scheme with policies</span></span>

<span data-ttu-id="bf090-122">Чтобы задать нужный схем в [политики](policies.md#security-authorization-policies-based) можно задать `AuthenticationSchemes` коллекции при добавлении политики.</span><span class="sxs-lookup"><span data-stu-id="bf090-122">If you prefer to specify the desired schemes in [policy](policies.md#security-authorization-policies-based) you can set the `AuthenticationSchemes` collection when adding your policy.</span></span>

```csharp
options.AddPolicy("Over18", policy =>
{
    policy.AuthenticationSchemes.Add("Bearer");
    policy.RequireAuthenticatedUser();
    policy.Requirements.Add(new Over18Requirement());
});
```

<span data-ttu-id="bf090-123">В этом примере Over18 политики будет выполняться только для идентификаторов, созданные `Bearer` по промежуточного слоя.</span><span class="sxs-lookup"><span data-stu-id="bf090-123">In this example the Over18 policy will only run against the identity created by the `Bearer` middleware.</span></span>
