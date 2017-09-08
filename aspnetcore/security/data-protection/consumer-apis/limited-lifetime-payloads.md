---
title: "Ограничение времени существования полезных данных, защищенных"
author: rick-anderson
description: 
keywords: ASP.NET Core
ms.author: riande
manager: wpickett
ms.date: 10/14/2016
ms.topic: article
ms.assetid: 000175e2-10fc-43dd-bfc2-51e004b97b44
ms.technology: aspnet
ms.prod: asp.net-core
uid: security/data-protection/consumer-apis/limited-lifetime-payloads
ms.openlocfilehash: 4ff13803b328c1e9dd2934c38c88b43f5798de03
ms.sourcegitcommit: 0b6c8e6d81d2b3c161cd375036eecbace46a9707
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 08/11/2017
---
# <a name="limiting-the-lifetime-of-protected-payloads"></a><span data-ttu-id="4cca0-103">Ограничение времени существования полезных данных, защищенных</span><span class="sxs-lookup"><span data-stu-id="4cca0-103">Limiting the lifetime of protected payloads</span></span>

<span data-ttu-id="4cca0-104">Существуют сценарии, где разработчик приложения хочет создать защищенный полезные данные, срок действия истекает через установленный период времени.</span><span class="sxs-lookup"><span data-stu-id="4cca0-104">There are scenarios where the application developer wants to create a protected payload that expires after a set period of time.</span></span> <span data-ttu-id="4cca0-105">Например защищенные полезных данных может представлять маркер сброса пароля, которая должна только действовать в течение одного часа.</span><span class="sxs-lookup"><span data-stu-id="4cca0-105">For instance, the protected payload might represent a password reset token that should only be valid for one hour.</span></span> <span data-ttu-id="4cca0-106">Возможна наверняка разработчикам создавать свои собственные формат полезных данных, который содержит внедренные срок действия, и дополнительно разработчикам может потребоваться это сделать, но для большинства разработчиков управление этих сроков действия может увеличиваться утомительным.</span><span class="sxs-lookup"><span data-stu-id="4cca0-106">It is certainly possible for the developer to create their own payload format that contains an embedded expiration date, and advanced developers may wish to do this anyway, but for the majority of developers managing these expirations can grow tedious.</span></span>

<span data-ttu-id="4cca0-107">Чтобы облегчить эту задачу для наших аудитория разработчиков, Microsoft.AspNetCore.DataProtection.Extensions пакет содержит API служебной программы для создания полезных данных, которые автоматически истекает после заданного периода времени.</span><span class="sxs-lookup"><span data-stu-id="4cca0-107">To make this easier for our developer audience, the package Microsoft.AspNetCore.DataProtection.Extensions contains utility APIs for creating payloads that automatically expire after a set period of time.</span></span> <span data-ttu-id="4cca0-108">Эти API-интерфейсы находятся по ITimeLimitedDataProtector типа.</span><span class="sxs-lookup"><span data-stu-id="4cca0-108">These APIs hang off of the ITimeLimitedDataProtector type.</span></span>

## <a name="api-usage"></a><span data-ttu-id="4cca0-109">Использование API</span><span class="sxs-lookup"><span data-stu-id="4cca0-109">API usage</span></span>

<span data-ttu-id="4cca0-110">Интерфейс ITimeLimitedDataProtector является основной интерфейс для защиты и снятие защиты с ограниченным сроком / самостоятельно истечение срока действия полезных данных.</span><span class="sxs-lookup"><span data-stu-id="4cca0-110">The ITimeLimitedDataProtector interface is the core interface for protecting and unprotecting time-limited / self-expiring payloads.</span></span> <span data-ttu-id="4cca0-111">Чтобы создать экземпляр ITimeLimitedDataProtector, сначала потребуется экземпляр обычной [IDataProtector](overview.md) создан с определенной цели.</span><span class="sxs-lookup"><span data-stu-id="4cca0-111">To create an instance of an ITimeLimitedDataProtector, you'll first need an instance of a regular [IDataProtector](overview.md) constructed with a specific purpose.</span></span> <span data-ttu-id="4cca0-112">Как только экземпляр IDataProtector доступен, вызов метода расширения IDataProtector.ToTimeLimitedDataProtector для получения предохранитель с возможностями встроенных истечение срока действия.</span><span class="sxs-lookup"><span data-stu-id="4cca0-112">Once the IDataProtector instance is available, call the IDataProtector.ToTimeLimitedDataProtector extension method to get back a protector with built-in expiration capabilities.</span></span>

<span data-ttu-id="4cca0-113">ITimeLimitedDataProtector предоставляет следующие методы API контактную зону и расширения:</span><span class="sxs-lookup"><span data-stu-id="4cca0-113">ITimeLimitedDataProtector exposes the following API surface and extension methods:</span></span>

* <span data-ttu-id="4cca0-114">CreateProtector (строка назначение): этот интерфейс API ITimeLimitedDataProtector похож на существующих IDataProtectionProvider.CreateProtector в том, что он может использоваться для создания [цели цепочки](purpose-strings.md) из предохранителя корневой ограниченной по времени.</span><span class="sxs-lookup"><span data-stu-id="4cca0-114">CreateProtector(string purpose) : ITimeLimitedDataProtector This API is similar to the existing IDataProtectionProvider.CreateProtector in that it can be used to create [purpose chains](purpose-strings.md) from a root time-limited protector.</span></span>

* <span data-ttu-id="4cca0-115">Защита (byte [] открытого текста, истечение срока действия DateTimeOffset): byte]</span><span class="sxs-lookup"><span data-stu-id="4cca0-115">Protect(byte[] plaintext, DateTimeOffset expiration) : byte[]</span></span>

* <span data-ttu-id="4cca0-116">Защита (открытого текста byte [], время существования TimeSpan): byte]</span><span class="sxs-lookup"><span data-stu-id="4cca0-116">Protect(byte[] plaintext, TimeSpan lifetime) : byte[]</span></span>

* <span data-ttu-id="4cca0-117">Защита (byte [] текстовая): byte]</span><span class="sxs-lookup"><span data-stu-id="4cca0-117">Protect(byte[] plaintext) : byte[]</span></span>

* <span data-ttu-id="4cca0-118">Защита (текстовая строка, истечение срока действия DateTimeOffset): строка</span><span class="sxs-lookup"><span data-stu-id="4cca0-118">Protect(string plaintext, DateTimeOffset expiration) : string</span></span>

* <span data-ttu-id="4cca0-119">Защита (обычный текст строки, время существования TimeSpan): строка</span><span class="sxs-lookup"><span data-stu-id="4cca0-119">Protect(string plaintext, TimeSpan lifetime) : string</span></span>

* <span data-ttu-id="4cca0-120">Защита (текстовая строка): строка</span><span class="sxs-lookup"><span data-stu-id="4cca0-120">Protect(string plaintext) : string</span></span>

<span data-ttu-id="4cca0-121">В дополнение к методам защитить core, принимающих только обычный текст существуют новые перегрузки, которые разрешается указывать срок действия полезных данных.</span><span class="sxs-lookup"><span data-stu-id="4cca0-121">In addition to the core Protect methods which take only the plaintext, there are new overloads which allow specifying the payload's expiration date.</span></span> <span data-ttu-id="4cca0-122">Можно указать дату истечения срока действия, как абсолютная Дата (с помощью DateTimeOffset) или как относительное время (от текущего системного времени, через интервал времени).</span><span class="sxs-lookup"><span data-stu-id="4cca0-122">The expiration date can be specified as an absolute date (via a DateTimeOffset) or as a relative time (from the current system time, via a TimeSpan).</span></span> <span data-ttu-id="4cca0-123">При вызове перегрузки, которая не принимает срока действия полезных данных предполагается никогда не истекает.</span><span class="sxs-lookup"><span data-stu-id="4cca0-123">If an overload which doesn't take an expiration is called, the payload is assumed never to expire.</span></span>

* <span data-ttu-id="4cca0-124">Снять защиту (byte [] protectedData, ожидания истечения срока действия DateTimeOffset): byte]</span><span class="sxs-lookup"><span data-stu-id="4cca0-124">Unprotect(byte[] protectedData, out DateTimeOffset expiration) : byte[]</span></span>

* <span data-ttu-id="4cca0-125">Снять защиту (protectedData byte []): byte]</span><span class="sxs-lookup"><span data-stu-id="4cca0-125">Unprotect(byte[] protectedData) : byte[]</span></span>

* <span data-ttu-id="4cca0-126">Снять защиту (ожидания истечения срока действия DateTimeOffset, protectedData строка): строка</span><span class="sxs-lookup"><span data-stu-id="4cca0-126">Unprotect(string protectedData, out DateTimeOffset expiration) : string</span></span>

* <span data-ttu-id="4cca0-127">Снять защиту (строка protectedData): строка</span><span class="sxs-lookup"><span data-stu-id="4cca0-127">Unprotect(string protectedData) : string</span></span>

<span data-ttu-id="4cca0-128">Методы Unprotect возвращают незащищенных исходных данных.</span><span class="sxs-lookup"><span data-stu-id="4cca0-128">The Unprotect methods return the original unprotected data.</span></span> <span data-ttu-id="4cca0-129">Если полезные данные еще не истек, абсолютный срок действия возвращается как Необязательный выходной параметр вместе с исходной незащищенные данные.</span><span class="sxs-lookup"><span data-stu-id="4cca0-129">If the payload hasn't yet expired, the absolute expiration is returned as an optional out parameter along with the original unprotected data.</span></span> <span data-ttu-id="4cca0-130">Если истек срок действия полезных данных всех перегруженных версий метода Unprotect вызовет CryptographicException.</span><span class="sxs-lookup"><span data-stu-id="4cca0-130">If the payload is expired, all overloads of the Unprotect method will throw CryptographicException.</span></span>

>[!WARNING]
> <span data-ttu-id="4cca0-131">Не рекомендуется использовать эти API-интерфейсы для защиты полезных нагрузок, требующих сохраняемости долгосрочной или не определено.</span><span class="sxs-lookup"><span data-stu-id="4cca0-131">It is not advised to use these APIs to protect payloads which require long-term or indefinite persistence.</span></span> <span data-ttu-id="4cca0-132">«Я допускают для защищенных нагрузок быть невозможности восстановить после месяца?»</span><span class="sxs-lookup"><span data-stu-id="4cca0-132">"Can I afford for the protected payloads to be permanently unrecoverable after a month?"</span></span> <span data-ttu-id="4cca0-133">может служить хорошим правило; Если ответ не затем разработчики следует рассмотреть альтернативные API-интерфейсы.</span><span class="sxs-lookup"><span data-stu-id="4cca0-133">can serve as a good rule of thumb; if the answer is no then developers should consider alternative APIs.</span></span>

<span data-ttu-id="4cca0-134">Пример ниже, используется [пути кода не DI](../configuration/non-di-scenarios.md) для экземпляров системы защиты данных.</span><span class="sxs-lookup"><span data-stu-id="4cca0-134">The sample below uses the [non-DI code paths](../configuration/non-di-scenarios.md) for instantiating the data protection system.</span></span> <span data-ttu-id="4cca0-135">Чтобы запустить этот образец, убедитесь, сначала добавили ссылку на пакет Microsoft.AspNetCore.DataProtection.Extensions.</span><span class="sxs-lookup"><span data-stu-id="4cca0-135">To run this sample, ensure that you have first added a reference to the Microsoft.AspNetCore.DataProtection.Extensions package.</span></span>

<span data-ttu-id="4cca0-136">[!code-none[Main](limited-lifetime-payloads/samples/limitedlifetimepayloads.cs)]</span><span class="sxs-lookup"><span data-stu-id="4cca0-136">[!code-none[Main](limited-lifetime-payloads/samples/limitedlifetimepayloads.cs)]</span></span>
