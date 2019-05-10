---
title: Рекомендации по проектированию SignalR API
author: anurse
description: Дополнительные сведения о разработке SignalR API-интерфейсы для обеспечения совместимости между версиями приложения.
monikerRange: '>= aspnetcore-2.1'
ms.author: anurse
ms.custom: mvc
ms.date: 11/06/2018
uid: signalr/api-design
ms.openlocfilehash: 3f17bf055b793e8fc91fbcc15f668928ca261f77
ms.sourcegitcommit: 5b0eca8c21550f95de3bb21096bd4fd4d9098026
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/27/2019
ms.locfileid: "64897811"
---
# <a name="signalr-api-design-considerations"></a><span data-ttu-id="54dbe-103">Рекомендации по проектированию SignalR API</span><span class="sxs-lookup"><span data-stu-id="54dbe-103">SignalR API design considerations</span></span>

<span data-ttu-id="54dbe-104">По [Andrew Stanton медсестра](https://twitter.com/anurse)</span><span class="sxs-lookup"><span data-stu-id="54dbe-104">By [Andrew Stanton-Nurse](https://twitter.com/anurse)</span></span>

<span data-ttu-id="54dbe-105">Эта статья содержит рекомендации по созданию API на основе SignalR.</span><span class="sxs-lookup"><span data-stu-id="54dbe-105">This article provides guidance for building SignalR-based APIs.</span></span>

## <a name="use-custom-object-parameters-to-ensure-backwards-compatibility"></a><span data-ttu-id="54dbe-106">Использование параметров пользовательского объекта для обеспечения обратной совместимости</span><span class="sxs-lookup"><span data-stu-id="54dbe-106">Use custom object parameters to ensure backwards-compatibility</span></span>

<span data-ttu-id="54dbe-107">Добавление параметров в метод концентратора SignalR (на клиенте или сервере) является *критическое изменение*.</span><span class="sxs-lookup"><span data-stu-id="54dbe-107">Adding parameters to a SignalR hub method (on either the client or the server) is a *breaking change*.</span></span> <span data-ttu-id="54dbe-108">Это означает, что старые клиенты и серверы ошибки возникают при попытке вызвать метод без параметров, соответствующее числу.</span><span class="sxs-lookup"><span data-stu-id="54dbe-108">This means older clients/servers will get errors when they try to invoke the method without the appropriate number of parameters.</span></span> <span data-ttu-id="54dbe-109">Однако добавление свойств к параметру пользовательского объекта является **не** является критическим изменением.</span><span class="sxs-lookup"><span data-stu-id="54dbe-109">However, adding properties to a custom object parameter is **not** a breaking change.</span></span> <span data-ttu-id="54dbe-110">Это можно использовать для разработки совместимые интерфейсы API, которые устойчивы к изменениям на стороне клиента или сервера.</span><span class="sxs-lookup"><span data-stu-id="54dbe-110">This can be used to design compatible APIs that are resilient to changes on the client or the server.</span></span>

<span data-ttu-id="54dbe-111">Например рассмотрим API серверной части следующим образом:</span><span class="sxs-lookup"><span data-stu-id="54dbe-111">For example, consider a server-side API like the following:</span></span>

[!code-csharp[ParameterBasedOldVersion](api-design/sample/Samples.cs?name=ParameterBasedOldVersion)]

<span data-ttu-id="54dbe-112">Клиент JavaScript вызывает этот метод с помощью `invoke` следующим образом:</span><span class="sxs-lookup"><span data-stu-id="54dbe-112">The JavaScript client calls this method using `invoke` as follows:</span></span>

[!code-typescript[CallWithOneParameter](api-design/sample/Samples.ts?name=CallWithOneParameter)]

<span data-ttu-id="54dbe-113">Если вы позже добавить второй параметр метода сервера, старые клиенты не предоставляют значение этого параметра.</span><span class="sxs-lookup"><span data-stu-id="54dbe-113">If you later add a second parameter to the server method, older clients won't provide this parameter value.</span></span> <span data-ttu-id="54dbe-114">Пример:</span><span class="sxs-lookup"><span data-stu-id="54dbe-114">For example:</span></span>

[!code-csharp[ParameterBasedNewVersion](api-design/sample/Samples.cs?name=ParameterBasedNewVersion)]

<span data-ttu-id="54dbe-115">Когда старый клиент пытается вызвать этот метод, он получит такую ошибку:</span><span class="sxs-lookup"><span data-stu-id="54dbe-115">When the old client tries to invoke this method, it will get an error like this:</span></span>

```
Microsoft.AspNetCore.SignalR.HubException: Failed to invoke 'GetTotalLength' due to an error on the server.
```

<span data-ttu-id="54dbe-116">На сервере вы увидите примерно следующее сообщение журнала:</span><span class="sxs-lookup"><span data-stu-id="54dbe-116">On the server, you'll see a log message like this:</span></span>

```
System.IO.InvalidDataException: Invocation provides 1 argument(s) but target expects 2.
```

<span data-ttu-id="54dbe-117">Старый клиент отправил только один параметр, но более новые API сервера требуется два параметра.</span><span class="sxs-lookup"><span data-stu-id="54dbe-117">The old client only sent one parameter, but the newer server API required two parameters.</span></span> <span data-ttu-id="54dbe-118">Использование пользовательских объектов в качестве параметров обеспечивает большую гибкость.</span><span class="sxs-lookup"><span data-stu-id="54dbe-118">Using custom objects as parameters gives you more flexibility.</span></span> <span data-ttu-id="54dbe-119">Давайте перепроектировать исходного API-интерфейса для пользовательского объекта:</span><span class="sxs-lookup"><span data-stu-id="54dbe-119">Let's redesign the original API to use a custom object:</span></span>

[!code-csharp[ObjectBasedOldVersion](api-design/sample/Samples.cs?name=ObjectBasedOldVersion)]

<span data-ttu-id="54dbe-120">Теперь клиент использует объект для вызова метода:</span><span class="sxs-lookup"><span data-stu-id="54dbe-120">Now, the client uses an object to call the method:</span></span>

[!code-typescript[CallWithObject](api-design/sample/Samples.ts?name=CallWithObject)]

<span data-ttu-id="54dbe-121">Вместо добавления параметра, добавьте свойство, чтобы `TotalLengthRequest` объекта:</span><span class="sxs-lookup"><span data-stu-id="54dbe-121">Instead of adding a parameter, add a property to the `TotalLengthRequest` object:</span></span>

[!code-csharp[ObjectBasedNewVersion](api-design/sample/Samples.cs?name=ObjectBasedNewVersion&highlight=4,9-13)]

<span data-ttu-id="54dbe-122">Когда старый клиент отправляет один параметр, лишние `Param2` остается свойство `null`.</span><span class="sxs-lookup"><span data-stu-id="54dbe-122">When the old client sends a single parameter, the extra `Param2` property will be left `null`.</span></span> <span data-ttu-id="54dbe-123">Вы можете обнаружить сообщение, отправленное более старого клиента путем проверки `Param2` для `null` и применить значение по умолчанию.</span><span class="sxs-lookup"><span data-stu-id="54dbe-123">You can detect a message sent by an older client by checking the `Param2` for `null` and apply a default value.</span></span> <span data-ttu-id="54dbe-124">Новый клиент может отправлять оба параметра.</span><span class="sxs-lookup"><span data-stu-id="54dbe-124">A new client can send both parameters.</span></span>

[!code-typescript[CallWithObjectNew](api-design/sample/Samples.ts?name=CallWithObjectNew)]

<span data-ttu-id="54dbe-125">Тот же метод работает для методов, определенные на клиенте.</span><span class="sxs-lookup"><span data-stu-id="54dbe-125">The same technique works for methods defined on the client.</span></span> <span data-ttu-id="54dbe-126">Пользовательский объект можно отправлять на стороне сервера:</span><span class="sxs-lookup"><span data-stu-id="54dbe-126">You can send a custom object from the server side:</span></span>

[!code-csharp[ClientSideObjectBasedOld](api-design/sample/Samples.cs?name=ClientSideObjectBasedOld)]

<span data-ttu-id="54dbe-127">На стороне клиента, доступ к `Message` свойство, а не с помощью параметра:</span><span class="sxs-lookup"><span data-stu-id="54dbe-127">On the client side, you access the `Message` property rather than using a parameter:</span></span>

[!code-typescript[OnWithObjectOld](api-design/sample/Samples.ts?name=OnWithObjectOld)]

<span data-ttu-id="54dbe-128">Если позднее вы решите добавить отправителя сообщения в полезных данных, добавьте свойство к объекту:</span><span class="sxs-lookup"><span data-stu-id="54dbe-128">If you later decide to add the sender of the message to the payload, add a property to the object:</span></span>

[!code-csharp[ClientSideObjectBasedNew](api-design/sample/Samples.cs?name=ClientSideObjectBasedNew&highlight=5)]

<span data-ttu-id="54dbe-129">Старые клиенты не будут ожидать `Sender` значение, поэтому они не будет обрабатывать его.</span><span class="sxs-lookup"><span data-stu-id="54dbe-129">The older clients won't be expecting the `Sender` value, so they'll ignore it.</span></span> <span data-ttu-id="54dbe-130">Новый клиент сможет принять его, обновив для чтения свойство:</span><span class="sxs-lookup"><span data-stu-id="54dbe-130">A new client can accept it by updating to read the new property:</span></span>

[!code-typescript[OnWithObjectNew](api-design/sample/Samples.ts?name=OnWithObjectNew&highlight=2-5)]

<span data-ttu-id="54dbe-131">В этом случае новый клиент также устойчива к старый сервер, который не предоставляет `Sender` значение.</span><span class="sxs-lookup"><span data-stu-id="54dbe-131">In this case, the new client is also tolerant of an old server that doesn't provide the `Sender` value.</span></span> <span data-ttu-id="54dbe-132">Поскольку старый сервер не предоставляют `Sender` значение, клиент проверяет, существует ли он перед доступом к нему.</span><span class="sxs-lookup"><span data-stu-id="54dbe-132">Since the old server won't provide the `Sender` value, the client checks to see if it exists before accessing it.</span></span>
