---
title: "Приступая к работе с API защиты данных"
author: rick-anderson
description: 
keywords: ASP.NET Core
ms.author: riande
manager: wpickett
ms.date: 10/14/2016
ms.topic: article
ms.assetid: 39b7a73c-29d4-4137-b311-49529adcf3cb
ms.technology: aspnet
ms.prod: asp.net-core
uid: security/data-protection/using-data-protection
ms.openlocfilehash: 9489b55b1de626b77bcbe21cb9678e561b559099
ms.sourcegitcommit: 8f4d4fad1ca27adf9e396f5c205c9875a3963664
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/13/2017
---
# <a name="getting-started-with-the-data-protection-apis"></a><span data-ttu-id="a39a4-103">Приступая к работе с API защиты данных</span><span class="sxs-lookup"><span data-stu-id="a39a4-103">Getting Started with the Data Protection APIs</span></span>

<a name="security-data-protection-getting-started"></a>

<span data-ttu-id="a39a4-104">В его простейшей защиту данных состоит из следующих шагов:</span><span class="sxs-lookup"><span data-stu-id="a39a4-104">At its simplest protecting data consists of the following steps:</span></span>

1. <span data-ttu-id="a39a4-105">Создание данных предохранитель из поставщик защиты данных.</span><span class="sxs-lookup"><span data-stu-id="a39a4-105">Create a data protector from a data protection provider.</span></span>

2. <span data-ttu-id="a39a4-106">Вызовите метод защитить с данными, которые необходимо защитить.</span><span class="sxs-lookup"><span data-stu-id="a39a4-106">Call the Protect method with the data you want to protect.</span></span>

3. <span data-ttu-id="a39a4-107">Вызовите метод снять защиту с данными, которые вы хотите преобразовать обратно в обычный текст.</span><span class="sxs-lookup"><span data-stu-id="a39a4-107">Call the Unprotect method with the data you want to turn back into plain text.</span></span>

<span data-ttu-id="a39a4-108">Большинство платформ, таких как ASP.NET или SignalR уже настройки системы защиты данных и добавить его в контейнер службы, который доступен через внедрения зависимостей.</span><span class="sxs-lookup"><span data-stu-id="a39a4-108">Most frameworks such as ASP.NET or SignalR already configure the data protection system and add it to a service container you access via dependency injection.</span></span> <span data-ttu-id="a39a4-109">Следующий пример демонстрирует настройку контейнер службы для внедрения зависимостей и регистрации стека защиты данных, получения поставщик защиты данных через DI, создания предохранителя и защиту и снятие защиты данных</span><span class="sxs-lookup"><span data-stu-id="a39a4-109">The following sample demonstrates configuring a service container for dependency injection and registering the data protection stack, receiving the data protection provider via DI, creating a protector and protecting then unprotecting data</span></span>

[!code-csharp[Main](../../security/data-protection/using-data-protection/samples/protectunprotect.cs?highlight=26,34,35,36,37,38,39,40)]

<span data-ttu-id="a39a4-110">При создании предохранителя необходимо указать один или несколько [строки цели](consumer-apis/purpose-strings.md).</span><span class="sxs-lookup"><span data-stu-id="a39a4-110">When you create a protector you must provide one or more [Purpose Strings](consumer-apis/purpose-strings.md).</span></span> <span data-ttu-id="a39a4-111">Строка цели обеспечивает изоляцию между потребителей, например предохранителя, созданные с целью строку «зеленый» не сможет снять защиту данных, предоставленных предохранитель с целью «фиолетовый».</span><span class="sxs-lookup"><span data-stu-id="a39a4-111">A purpose string provides isolation between consumers, for example a protector created with a purpose string of "green" would not be able to unprotect data provided by a protector with a purpose of "purple".</span></span>

>[!TIP]
> <span data-ttu-id="a39a4-112">Экземпляры IDataProtectionProvider и IDataProtector являются потокобезопасными для нескольких клиентов.</span><span class="sxs-lookup"><span data-stu-id="a39a4-112">Instances of IDataProtectionProvider and IDataProtector are thread-safe for multiple callers.</span></span> <span data-ttu-id="a39a4-113">Предполагается, что после компонент получает ссылку на IDataProtector через вызов CreateProtector, он будет использовать эту ссылку для несколько вызовов Protect и Unprotect.</span><span class="sxs-lookup"><span data-stu-id="a39a4-113">It is intended that once a component gets a reference to an IDataProtector via a call to CreateProtector, it will use that reference for multiple calls to Protect and Unprotect.</span></span>
>
><span data-ttu-id="a39a4-114">Вызов Unprotect вызовет CryptographicException, если защищенный полезных данных не может быть проверена или расшифровать.</span><span class="sxs-lookup"><span data-stu-id="a39a4-114">A call to Unprotect will throw CryptographicException if the protected payload cannot be verified or deciphered.</span></span> <span data-ttu-id="a39a4-115">Некоторые компоненты нужна возможность пропускать ошибки во время снятия защиты операций; компонент, который считывает файлы cookie проверки подлинности может обработать эту ошибку и обрабатывать запрос, как если бы объекты cookie не на всех, а не ошибкой запроса сразу.</span><span class="sxs-lookup"><span data-stu-id="a39a4-115">Some components may wish to ignore errors during unprotect operations; a component which reads authentication cookies might handle this error and treat the request as if it had no cookie at all rather than fail the request outright.</span></span> <span data-ttu-id="a39a4-116">В частности, компонентами, которые хотите такого поведения следует перехватывать CryptographicException вместо подавление всех исключений.</span><span class="sxs-lookup"><span data-stu-id="a39a4-116">Components which want this behavior should specifically catch CryptographicException instead of swallowing all exceptions.</span></span>
