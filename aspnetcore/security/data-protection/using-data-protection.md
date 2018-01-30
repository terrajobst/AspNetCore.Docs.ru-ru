---
title: "Приступая к работе с API защиты данных"
author: rick-anderson
description: "В этом документе описывается использование интерфейсов API защиты данных ASP.NET Core для защиты и снятие защиты данных в приложении."
manager: wpickett
ms.author: riande
ms.date: 10/14/2016
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: security/data-protection/using-data-protection
ms.openlocfilehash: e8c4183f5c47d8ffec8edf163eb1e4d6f2757d9d
ms.sourcegitcommit: a510f38930abc84c4b302029d019a34dfe76823b
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/30/2018
---
# <a name="getting-started-with-the-data-protection-apis"></a><span data-ttu-id="6698d-103">Приступая к работе с API защиты данных</span><span class="sxs-lookup"><span data-stu-id="6698d-103">Getting Started with the Data Protection APIs</span></span>

<a name="security-data-protection-getting-started"></a>

<span data-ttu-id="6698d-104">На простой защиту данных состоит из следующих шагов:</span><span class="sxs-lookup"><span data-stu-id="6698d-104">At its simplest, protecting data consists of the following steps:</span></span>

1. <span data-ttu-id="6698d-105">Создание данных предохранитель из поставщик защиты данных.</span><span class="sxs-lookup"><span data-stu-id="6698d-105">Create a data protector from a data protection provider.</span></span>

2. <span data-ttu-id="6698d-106">Вызовите `Protect` метод с данными, которую необходимо защитить.</span><span class="sxs-lookup"><span data-stu-id="6698d-106">Call the `Protect` method with the data you want to protect.</span></span>

3. <span data-ttu-id="6698d-107">Вызовите `Unprotect` метод с данными требуется вернулась в виде обычного текста.</span><span class="sxs-lookup"><span data-stu-id="6698d-107">Call the `Unprotect` method with the data you want to turn back into plain text.</span></span>

<span data-ttu-id="6698d-108">Большинство платформ и модели приложения, такие как ASP.NET или SignalR, уже настройки системы защиты данных и добавьте его в контейнер службы, который доступен через внедрения зависимостей.</span><span class="sxs-lookup"><span data-stu-id="6698d-108">Most frameworks and app models, such as ASP.NET or SignalR, already configure the data protection system and add it to a service container you access via dependency injection.</span></span> <span data-ttu-id="6698d-109">Следующий пример демонстрирует настройку контейнер службы для внедрения зависимостей и регистрации стека защиты данных, получения поставщик защиты данных через DI, создания предохранителя и защиту и снятие защиты данных</span><span class="sxs-lookup"><span data-stu-id="6698d-109">The following sample demonstrates configuring a service container for dependency injection and registering the data protection stack, receiving the data protection provider via DI, creating a protector and protecting then unprotecting data</span></span>

[!code-csharp[Main](../../security/data-protection/using-data-protection/samples/protectunprotect.cs?highlight=26,34,35,36,37,38,39,40)]

<span data-ttu-id="6698d-110">При создании предохранителя необходимо указать один или несколько [строки цели](consumer-apis/purpose-strings.md).</span><span class="sxs-lookup"><span data-stu-id="6698d-110">When you create a protector you must provide one or more [Purpose Strings](consumer-apis/purpose-strings.md).</span></span> <span data-ttu-id="6698d-111">Строка цели обеспечивает изоляцию между потребителей.</span><span class="sxs-lookup"><span data-stu-id="6698d-111">A purpose string provides isolation between consumers.</span></span> <span data-ttu-id="6698d-112">Например предохранителя, созданные с целью строку «зеленый» не смогут снять защиту данных, предоставленных предохранитель с целью «фиолетовый».</span><span class="sxs-lookup"><span data-stu-id="6698d-112">For example, a protector created with a purpose string of "green" wouldn't be able to unprotect data provided by a protector with a purpose of "purple".</span></span>

>[!TIP]
> <span data-ttu-id="6698d-113">Экземпляры `IDataProtectionProvider` и `IDataProtector` потокобезопасны для нескольких клиентов.</span><span class="sxs-lookup"><span data-stu-id="6698d-113">Instances of `IDataProtectionProvider` and `IDataProtector` are thread-safe for multiple callers.</span></span> <span data-ttu-id="6698d-114">Он предназначен, когда компонент получает ссылку на `IDataProtector` через вызов `CreateProtector`, он будет использовать эту ссылку для нескольких вызовов `Protect` и `Unprotect`.</span><span class="sxs-lookup"><span data-stu-id="6698d-114">It's intended that once a component gets a reference to an `IDataProtector` via a call to `CreateProtector`, it will use that reference for multiple calls to `Protect` and `Unprotect`.</span></span>
>
><span data-ttu-id="6698d-115">Вызов `Unprotect` CryptographicException вызывает исключение, если защищенный полезных данных не может быть проверена или расшифровать.</span><span class="sxs-lookup"><span data-stu-id="6698d-115">A call to `Unprotect` will throw CryptographicException if the protected payload cannot be verified or deciphered.</span></span> <span data-ttu-id="6698d-116">Некоторые компоненты нужна возможность пропускать ошибки во время снятия защиты операций; компонент, который считывает файлы cookie проверки подлинности может обработать эту ошибку и обрабатывать запрос, как если бы объекты cookie не на всех, а не ошибкой запроса сразу.</span><span class="sxs-lookup"><span data-stu-id="6698d-116">Some components may wish to ignore errors during unprotect operations; a component which reads authentication cookies might handle this error and treat the request as if it had no cookie at all rather than fail the request outright.</span></span> <span data-ttu-id="6698d-117">В частности, компонентами, которые хотите такого поведения следует перехватывать CryptographicException вместо подавление всех исключений.</span><span class="sxs-lookup"><span data-stu-id="6698d-117">Components which want this behavior should specifically catch CryptographicException instead of swallowing all exceptions.</span></span>
