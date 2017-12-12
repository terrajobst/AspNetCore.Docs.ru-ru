---
title: "Приступая к работе с API защиты данных"
author: rick-anderson
description: "В этом документе описывается использование интерфейсов API защиты данных ASP.NET Core для защиты и снятие защиты данных в приложении."
keywords: "ASP.NET Core, защиты данных"
ms.author: riande
manager: wpickett
ms.date: 10/14/2016
ms.topic: article
ms.assetid: 39b7a73c-29d4-4137-b311-49529adcf3cb
ms.technology: aspnet
ms.prod: asp.net-core
uid: security/data-protection/using-data-protection
ms.openlocfilehash: 535bfaf2077cda91c27e7d0d68b9804e8596070e
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 11/10/2017
---
# <a name="getting-started-with-the-data-protection-apis"></a><span data-ttu-id="4379b-104">Приступая к работе с API защиты данных</span><span class="sxs-lookup"><span data-stu-id="4379b-104">Getting Started with the Data Protection APIs</span></span>

<a name="security-data-protection-getting-started"></a>

<span data-ttu-id="4379b-105">На простой защиту данных состоит из следующих шагов:</span><span class="sxs-lookup"><span data-stu-id="4379b-105">At its simplest, protecting data consists of the following steps:</span></span>

1. <span data-ttu-id="4379b-106">Создание данных предохранитель из поставщик защиты данных.</span><span class="sxs-lookup"><span data-stu-id="4379b-106">Create a data protector from a data protection provider.</span></span>

2. <span data-ttu-id="4379b-107">Вызовите `Protect` метод с данными, которую необходимо защитить.</span><span class="sxs-lookup"><span data-stu-id="4379b-107">Call the `Protect` method with the data you want to protect.</span></span>

3. <span data-ttu-id="4379b-108">Вызовите `Unprotect` метод с данными требуется вернулась в виде обычного текста.</span><span class="sxs-lookup"><span data-stu-id="4379b-108">Call the `Unprotect` method with the data you want to turn back into plain text.</span></span>

<span data-ttu-id="4379b-109">Большинство платформ и модели приложения, такие как ASP.NET или SignalR, уже настройки системы защиты данных и добавьте его в контейнер службы, который доступен через внедрения зависимостей.</span><span class="sxs-lookup"><span data-stu-id="4379b-109">Most frameworks and app models, such as ASP.NET or SignalR, already configure the data protection system and add it to a service container you access via dependency injection.</span></span> <span data-ttu-id="4379b-110">Следующий пример демонстрирует настройку контейнер службы для внедрения зависимостей и регистрации стека защиты данных, получения поставщик защиты данных через DI, создания предохранителя и защиту и снятие защиты данных</span><span class="sxs-lookup"><span data-stu-id="4379b-110">The following sample demonstrates configuring a service container for dependency injection and registering the data protection stack, receiving the data protection provider via DI, creating a protector and protecting then unprotecting data</span></span>

[!code-csharp[Main](../../security/data-protection/using-data-protection/samples/protectunprotect.cs?highlight=26,34,35,36,37,38,39,40)]

<span data-ttu-id="4379b-111">При создании предохранителя необходимо указать один или несколько [строки цели](consumer-apis/purpose-strings.md).</span><span class="sxs-lookup"><span data-stu-id="4379b-111">When you create a protector you must provide one or more [Purpose Strings](consumer-apis/purpose-strings.md).</span></span> <span data-ttu-id="4379b-112">Строка цели обеспечивает изоляцию между потребителей.</span><span class="sxs-lookup"><span data-stu-id="4379b-112">A purpose string provides isolation between consumers.</span></span> <span data-ttu-id="4379b-113">Например предохранителя, созданные с целью строку «зеленый» не сможет снять защиту данных, предоставленных предохранитель с целью «фиолетовый».</span><span class="sxs-lookup"><span data-stu-id="4379b-113">For example, a protector created with a purpose string of "green" would not be able to unprotect data provided by a protector with a purpose of "purple".</span></span>

>[!TIP]
> <span data-ttu-id="4379b-114">Экземпляры `IDataProtectionProvider` и `IDataProtector` потокобезопасны для нескольких клиентов.</span><span class="sxs-lookup"><span data-stu-id="4379b-114">Instances of `IDataProtectionProvider` and `IDataProtector` are thread-safe for multiple callers.</span></span> <span data-ttu-id="4379b-115">Он предназначен, когда компонент получает ссылку на `IDataProtector` через вызов `CreateProtector`, он будет использовать эту ссылку для нескольких вызовов `Protect` и `Unprotect`.</span><span class="sxs-lookup"><span data-stu-id="4379b-115">It is intended that once a component gets a reference to an `IDataProtector` via a call to `CreateProtector`, it will use that reference for multiple calls to `Protect` and `Unprotect`.</span></span>
>
><span data-ttu-id="4379b-116">Вызов `Unprotect` CryptographicException вызывает исключение, если защищенный полезных данных не может быть проверена или расшифровать.</span><span class="sxs-lookup"><span data-stu-id="4379b-116">A call to `Unprotect` will throw CryptographicException if the protected payload cannot be verified or deciphered.</span></span> <span data-ttu-id="4379b-117">Некоторые компоненты нужна возможность пропускать ошибки во время снятия защиты операций; компонент, который считывает файлы cookie проверки подлинности может обработать эту ошибку и обрабатывать запрос, как если бы объекты cookie не на всех, а не ошибкой запроса сразу.</span><span class="sxs-lookup"><span data-stu-id="4379b-117">Some components may wish to ignore errors during unprotect operations; a component which reads authentication cookies might handle this error and treat the request as if it had no cookie at all rather than fail the request outright.</span></span> <span data-ttu-id="4379b-118">В частности, компонентами, которые хотите такого поведения следует перехватывать CryptographicException вместо подавление всех исключений.</span><span class="sxs-lookup"><span data-stu-id="4379b-118">Components which want this behavior should specifically catch CryptographicException instead of swallowing all exceptions.</span></span>
