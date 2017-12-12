---
title: "Ограничение времени существования полезных данных, защищенных"
author: rick-anderson
description: "В этом документе объясняется, как ограничить время существования защищенных полезных данных с помощью интерфейсов API защиты данных ASP.NET Core."
keywords: "ASP.NET Core, защиты данных, время жизни полезных данных"
ms.author: riande
manager: wpickett
ms.date: 10/14/2016
ms.topic: article
ms.assetid: 000175e2-10fc-43dd-bfc2-51e004b97b44
ms.technology: aspnet
ms.prod: asp.net-core
uid: security/data-protection/consumer-apis/limited-lifetime-payloads
ms.openlocfilehash: 3fe2e97db118a92cf6fa003b9ce8a9dedf253136
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 11/10/2017
---
# <a name="limiting-the-lifetime-of-protected-payloads"></a><span data-ttu-id="fcdd3-104">Ограничение времени существования полезных данных, защищенных</span><span class="sxs-lookup"><span data-stu-id="fcdd3-104">Limiting the lifetime of protected payloads</span></span>

<span data-ttu-id="fcdd3-105">Существуют сценарии, где разработчик приложения хочет создать защищенный полезные данные, срок действия истекает через установленный период времени.</span><span class="sxs-lookup"><span data-stu-id="fcdd3-105">There are scenarios where the application developer wants to create a protected payload that expires after a set period of time.</span></span> <span data-ttu-id="fcdd3-106">Например защищенные полезных данных может представлять маркер сброса пароля, которая должна только действовать в течение одного часа.</span><span class="sxs-lookup"><span data-stu-id="fcdd3-106">For instance, the protected payload might represent a password reset token that should only be valid for one hour.</span></span> <span data-ttu-id="fcdd3-107">Возможна наверняка разработчикам создавать свои собственные формат полезных данных, который содержит внедренные срок действия, и дополнительно разработчикам может потребоваться это сделать, но для большинства разработчиков управление этих сроков действия может увеличиваться утомительным.</span><span class="sxs-lookup"><span data-stu-id="fcdd3-107">It is certainly possible for the developer to create their own payload format that contains an embedded expiration date, and advanced developers may wish to do this anyway, but for the majority of developers managing these expirations can grow tedious.</span></span>

<span data-ttu-id="fcdd3-108">Чтобы облегчить эту задачу для наших аудитория разработчиков, пакет [Microsoft.AspNetCore.DataProtection.Extensions](https://www.nuget.org/packages/Microsoft.AspNetCore.DataProtection.Extensions/) содержит API служебной программы для создания полезных данных, которые автоматически истекает после заданного периода времени.</span><span class="sxs-lookup"><span data-stu-id="fcdd3-108">To make this easier for our developer audience, the package [Microsoft.AspNetCore.DataProtection.Extensions](https://www.nuget.org/packages/Microsoft.AspNetCore.DataProtection.Extensions/) contains utility APIs for creating payloads that automatically expire after a set period of time.</span></span> <span data-ttu-id="fcdd3-109">Эти API-интерфейсы зависания пределами `ITimeLimitedDataProtector` типа.</span><span class="sxs-lookup"><span data-stu-id="fcdd3-109">These APIs hang off of the `ITimeLimitedDataProtector` type.</span></span>

## <a name="api-usage"></a><span data-ttu-id="fcdd3-110">Использование API</span><span class="sxs-lookup"><span data-stu-id="fcdd3-110">API usage</span></span>

<span data-ttu-id="fcdd3-111">`ITimeLimitedDataProtector` Интерфейс является основной интерфейс для защиты и снятие защиты полезных данных ограниченной по времени и автоматическим истечением срока действия.</span><span class="sxs-lookup"><span data-stu-id="fcdd3-111">The `ITimeLimitedDataProtector` interface is the core interface for protecting and unprotecting time-limited / self-expiring payloads.</span></span> <span data-ttu-id="fcdd3-112">Для создания экземпляра `ITimeLimitedDataProtector`, вам потребуется сначала экземпляр обычной [IDataProtector](overview.md) создан с определенной цели.</span><span class="sxs-lookup"><span data-stu-id="fcdd3-112">To create an instance of an `ITimeLimitedDataProtector`, you'll first need an instance of a regular [IDataProtector](overview.md) constructed with a specific purpose.</span></span> <span data-ttu-id="fcdd3-113">Один раз `IDataProtector` экземпляр доступен, вызовите `IDataProtector.ToTimeLimitedDataProtector` метод расширения для получения предохранитель с возможностями встроенных истечение срока действия.</span><span class="sxs-lookup"><span data-stu-id="fcdd3-113">Once the `IDataProtector` instance is available, call the `IDataProtector.ToTimeLimitedDataProtector` extension method to get back a protector with built-in expiration capabilities.</span></span>

<span data-ttu-id="fcdd3-114">`ITimeLimitedDataProtector`предоставляет следующие методы API контактную зону и расширения:</span><span class="sxs-lookup"><span data-stu-id="fcdd3-114">`ITimeLimitedDataProtector` exposes the following API surface and extension methods:</span></span>

* <span data-ttu-id="fcdd3-115">CreateProtector (строка назначение): ITimeLimitedDataProtector - этот API, похожие на существующие `IDataProtectionProvider.CreateProtector` в том, что он может использоваться для создания [цели цепочки](purpose-strings.md) из предохранителя корневой ограниченной по времени.</span><span class="sxs-lookup"><span data-stu-id="fcdd3-115">CreateProtector(string purpose) : ITimeLimitedDataProtector - This API is similar to the existing `IDataProtectionProvider.CreateProtector` in that it can be used to create [purpose chains](purpose-strings.md) from a root time-limited protector.</span></span>

* <span data-ttu-id="fcdd3-116">Защита (byte [] открытого текста, истечение срока действия DateTimeOffset): byte]</span><span class="sxs-lookup"><span data-stu-id="fcdd3-116">Protect(byte[] plaintext, DateTimeOffset expiration) : byte[]</span></span>

* <span data-ttu-id="fcdd3-117">Защита (открытого текста byte [], время существования TimeSpan): byte]</span><span class="sxs-lookup"><span data-stu-id="fcdd3-117">Protect(byte[] plaintext, TimeSpan lifetime) : byte[]</span></span>

* <span data-ttu-id="fcdd3-118">Защита (byte [] текстовая): byte]</span><span class="sxs-lookup"><span data-stu-id="fcdd3-118">Protect(byte[] plaintext) : byte[]</span></span>

* <span data-ttu-id="fcdd3-119">Защита (текстовая строка, истечение срока действия DateTimeOffset): строка</span><span class="sxs-lookup"><span data-stu-id="fcdd3-119">Protect(string plaintext, DateTimeOffset expiration) : string</span></span>

* <span data-ttu-id="fcdd3-120">Защита (обычный текст строки, время существования TimeSpan): строка</span><span class="sxs-lookup"><span data-stu-id="fcdd3-120">Protect(string plaintext, TimeSpan lifetime) : string</span></span>

* <span data-ttu-id="fcdd3-121">Защита (текстовая строка): строка</span><span class="sxs-lookup"><span data-stu-id="fcdd3-121">Protect(string plaintext) : string</span></span>

<span data-ttu-id="fcdd3-122">Помимо базовых `Protect` методов, принимающих только обычный текст, существуют новые перегрузки, которые разрешается указывать срок действия полезных данных.</span><span class="sxs-lookup"><span data-stu-id="fcdd3-122">In addition to the core `Protect` methods which take only the plaintext, there are new overloads which allow specifying the payload's expiration date.</span></span> <span data-ttu-id="fcdd3-123">Дата окончания срока действия может быть указан как абсолютная Дата (через `DateTimeOffset`) или как относительное время (из текущей системы времени через `TimeSpan`).</span><span class="sxs-lookup"><span data-stu-id="fcdd3-123">The expiration date can be specified as an absolute date (via a `DateTimeOffset`) or as a relative time (from the current system time, via a `TimeSpan`).</span></span> <span data-ttu-id="fcdd3-124">При вызове перегрузки, которая не принимает срока действия полезных данных предполагается никогда не истекает.</span><span class="sxs-lookup"><span data-stu-id="fcdd3-124">If an overload which doesn't take an expiration is called, the payload is assumed never to expire.</span></span>

* <span data-ttu-id="fcdd3-125">Снять защиту (byte [] protectedData, ожидания истечения срока действия DateTimeOffset): byte]</span><span class="sxs-lookup"><span data-stu-id="fcdd3-125">Unprotect(byte[] protectedData, out DateTimeOffset expiration) : byte[]</span></span>

* <span data-ttu-id="fcdd3-126">Снять защиту (protectedData byte []): byte]</span><span class="sxs-lookup"><span data-stu-id="fcdd3-126">Unprotect(byte[] protectedData) : byte[]</span></span>

* <span data-ttu-id="fcdd3-127">Снять защиту (ожидания истечения срока действия DateTimeOffset, protectedData строка): строка</span><span class="sxs-lookup"><span data-stu-id="fcdd3-127">Unprotect(string protectedData, out DateTimeOffset expiration) : string</span></span>

* <span data-ttu-id="fcdd3-128">Снять защиту (строка protectedData): строка</span><span class="sxs-lookup"><span data-stu-id="fcdd3-128">Unprotect(string protectedData) : string</span></span>

<span data-ttu-id="fcdd3-129">`Unprotect` Методы возвращают незащищенных исходных данных.</span><span class="sxs-lookup"><span data-stu-id="fcdd3-129">The `Unprotect` methods return the original unprotected data.</span></span> <span data-ttu-id="fcdd3-130">Если полезные данные еще не истек, абсолютный срок действия возвращается как Необязательный выходной параметр вместе с исходной незащищенные данные.</span><span class="sxs-lookup"><span data-stu-id="fcdd3-130">If the payload hasn't yet expired, the absolute expiration is returned as an optional out parameter along with the original unprotected data.</span></span> <span data-ttu-id="fcdd3-131">Если истек срок действия полезных данных всех перегруженных версий метода Unprotect вызовет CryptographicException.</span><span class="sxs-lookup"><span data-stu-id="fcdd3-131">If the payload is expired, all overloads of the Unprotect method will throw CryptographicException.</span></span>

>[!WARNING]
> <span data-ttu-id="fcdd3-132">Не рекомендуется использовать эти API-интерфейсы для защиты полезных нагрузок, требующих сохраняемости долгосрочной или не определено.</span><span class="sxs-lookup"><span data-stu-id="fcdd3-132">It is not advised to use these APIs to protect payloads which require long-term or indefinite persistence.</span></span> <span data-ttu-id="fcdd3-133">«Я допускают для защищенных нагрузок быть невозможности восстановить после месяца?»</span><span class="sxs-lookup"><span data-stu-id="fcdd3-133">"Can I afford for the protected payloads to be permanently unrecoverable after a month?"</span></span> <span data-ttu-id="fcdd3-134">может служить хорошим правило; Если ответ не затем разработчики следует рассмотреть альтернативные API-интерфейсы.</span><span class="sxs-lookup"><span data-stu-id="fcdd3-134">can serve as a good rule of thumb; if the answer is no then developers should consider alternative APIs.</span></span>

<span data-ttu-id="fcdd3-135">Пример ниже, используется [пути кода не DI](../configuration/non-di-scenarios.md) для экземпляров системы защиты данных.</span><span class="sxs-lookup"><span data-stu-id="fcdd3-135">The sample below uses the [non-DI code paths](../configuration/non-di-scenarios.md) for instantiating the data protection system.</span></span> <span data-ttu-id="fcdd3-136">Чтобы запустить этот образец, убедитесь, сначала добавили ссылку на пакет Microsoft.AspNetCore.DataProtection.Extensions.</span><span class="sxs-lookup"><span data-stu-id="fcdd3-136">To run this sample, ensure that you have first added a reference to the Microsoft.AspNetCore.DataProtection.Extensions package.</span></span>

[!code-csharp[Main](limited-lifetime-payloads/samples/limitedlifetimepayloads.cs)]
