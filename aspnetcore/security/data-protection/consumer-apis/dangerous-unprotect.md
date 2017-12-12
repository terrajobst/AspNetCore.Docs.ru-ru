---
title: "Снятие защиты полезных данных, ключи которых были отозваны"
author: rick-anderson
description: "В этом документе объясняется, как следует снять защиту данных, защищенные с помощью ключей, поскольку были отозваны, в приложении ASP.NET Core."
keywords: "Ключи, IPersistedDataProtector отозван ASP.NET Core, защиты данных"
ms.author: riande
manager: wpickett
ms.date: 10/14/2016
ms.topic: article
ms.assetid: 6c4e6591-45d2-4d25-855e-062ad352d648
ms.technology: aspnet
ms.prod: asp.net-core
uid: security/data-protection/consumer-apis/dangerous-unprotect
ms.openlocfilehash: 5d431f0bbe7152525c9a360a6e90bccbd26be93d
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 11/10/2017
---
# <a name="unprotecting-payloads-whose-keys-have-been-revoked"></a><span data-ttu-id="415a0-104">Снятие защиты полезных данных, ключи которых были отозваны</span><span class="sxs-lookup"><span data-stu-id="415a0-104">Unprotecting payloads whose keys have been revoked</span></span>

<a name="data-protection-consumer-apis-dangerous-unprotect"></a>

<span data-ttu-id="415a0-105">В основном защиты данных ASP.NET Core API-интерфейсы не предназначены для неопределенного сохраняемости Конфиденциально полезных данных.</span><span class="sxs-lookup"><span data-stu-id="415a0-105">The ASP.NET Core data protection APIs are not primarily intended for indefinite persistence of confidential payloads.</span></span> <span data-ttu-id="415a0-106">Другие технологии, такие как [CNG Windows DPAPI](https://msdn.microsoft.com/library/windows/desktop/hh706794%28v=vs.85%29.aspx) и [Azure Rights Management](https://docs.microsoft.com/rights-management/) больше подходят для сценария хранилища не ограничена, и они обладают возможностями соответственно строгого управления ключами.</span><span class="sxs-lookup"><span data-stu-id="415a0-106">Other technologies like [Windows CNG DPAPI](https://msdn.microsoft.com/library/windows/desktop/hh706794%28v=vs.85%29.aspx) and [Azure Rights Management](https://docs.microsoft.com/rights-management/) are more suited to the scenario of indefinite storage, and they have correspondingly strong key management capabilities.</span></span> <span data-ttu-id="415a0-107">С другой стороны, нет ничего запретить разработчик с помощью интерфейсов API защиты данных ASP.NET Core для долгосрочной защиты конфиденциальных данных.</span><span class="sxs-lookup"><span data-stu-id="415a0-107">That said, there is nothing prohibiting a developer from using the ASP.NET Core data protection APIs for long-term protection of confidential data.</span></span> <span data-ttu-id="415a0-108">Ключи, никогда не удаляются из ключей, поэтому `IDataProtector.Unprotect` всегда можно восстановить существующий полезных данных, при условии, что ключи доступны и допустимым.</span><span class="sxs-lookup"><span data-stu-id="415a0-108">Keys are never removed from the key ring, so `IDataProtector.Unprotect` can always recover existing payloads as long as the keys are available and valid.</span></span>

<span data-ttu-id="415a0-109">Однако проблема возникает при попытке снять защиту данных, защищенных отозванных ключом, как разработчик `IDataProtector.Unprotect` в этом случае будет вызвано исключение.</span><span class="sxs-lookup"><span data-stu-id="415a0-109">However, an issue arises when the developer tries to unprotect data that has been protected with a revoked key, as `IDataProtector.Unprotect` will throw an exception in this case.</span></span> <span data-ttu-id="415a0-110">Это может быть удобно при кратковременных или временных полезных данных (например, токены проверки подлинности), как эти виды полезных данных можно легко воссоздать в системе и в худшем случае посетитель узла возможно, потребуется снова войти в систему.</span><span class="sxs-lookup"><span data-stu-id="415a0-110">This might be fine for short-lived or transient payloads (like authentication tokens), as these kinds of payloads can easily be recreated by the system, and at worst the site visitor might be required to log in again.</span></span> <span data-ttu-id="415a0-111">Но для материализованных полезных данных, имеющих `Unprotect` throw может привести к потере недопустимых значений.</span><span class="sxs-lookup"><span data-stu-id="415a0-111">But for persisted payloads, having `Unprotect` throw could lead to unacceptable data loss.</span></span>

## <a name="ipersisteddataprotector"></a><span data-ttu-id="415a0-112">IPersistedDataProtector</span><span class="sxs-lookup"><span data-stu-id="415a0-112">IPersistedDataProtector</span></span>

<span data-ttu-id="415a0-113">Для поддержки сценариев, что полезные данные, необходимо снять защиту даже в случае отозванных ключи система защиты данных содержит `IPersistedDataProtector` типа.</span><span class="sxs-lookup"><span data-stu-id="415a0-113">To support the scenario of allowing payloads to be unprotected even in the face of revoked keys, the data protection system contains an `IPersistedDataProtector` type.</span></span> <span data-ttu-id="415a0-114">Чтобы получить экземпляр `IPersistedDataProtector`, просто получить экземпляр `IDataProtector` в обычном и приведение try `IDataProtector` для `IPersistedDataProtector`.</span><span class="sxs-lookup"><span data-stu-id="415a0-114">To get an instance of `IPersistedDataProtector`, simply get an instance of `IDataProtector` in the normal fashion and try casting the `IDataProtector` to `IPersistedDataProtector`.</span></span>

> [!NOTE]
> <span data-ttu-id="415a0-115">Не все `IDataProtector` экземпляры могут быть приведены к `IPersistedDataProtector`.</span><span class="sxs-lookup"><span data-stu-id="415a0-115">Not all `IDataProtector` instances can be cast to `IPersistedDataProtector`.</span></span> <span data-ttu-id="415a0-116">Разработчикам следует использовать в C# как оператор или аналогичные, чтобы избежать исключений среды выполнения причиной недопустимые приведения, они должны быть подготовлены к соответствующим образом обрабатывать случаи сбоя.</span><span class="sxs-lookup"><span data-stu-id="415a0-116">Developers should use the C# as operator or similar to avoid runtime exceptions caused by invalid casts, and they should be prepared to handle the failure case appropriately.</span></span>

<span data-ttu-id="415a0-117">`IPersistedDataProtector`предоставляет поверхность следующие API:</span><span class="sxs-lookup"><span data-stu-id="415a0-117">`IPersistedDataProtector` exposes the following API surface:</span></span>

```csharp
DangerousUnprotect(byte[] protectedData, bool ignoreRevocationErrors,
     out bool requiresMigration, out bool wasRevoked) : byte[]
```

<span data-ttu-id="415a0-118">Этот API принимает защищенных полезных данных (в виде массива байтов) и возвращает незащищенных полезных данных.</span><span class="sxs-lookup"><span data-stu-id="415a0-118">This API takes the protected payload (as a byte array) and returns the unprotected payload.</span></span> <span data-ttu-id="415a0-119">Нет ни одна перегрузка на основе строк.</span><span class="sxs-lookup"><span data-stu-id="415a0-119">There is no string-based overload.</span></span> <span data-ttu-id="415a0-120">Ниже приведены два выходных параметров.</span><span class="sxs-lookup"><span data-stu-id="415a0-120">The two out parameters are as follows.</span></span>

* <span data-ttu-id="415a0-121">`requiresMigration`: установлено значение true, если ключ, используемый для защиты этого полезные данные больше не по умолчанию активного ключа, например, старый ключ, используемый для защиты этого полезные данные и ключ операции отката с момента откроется месте.</span><span class="sxs-lookup"><span data-stu-id="415a0-121">`requiresMigration`: will be set to true if the key used to protect this payload is no longer the active default key, e.g., the key used to protect this payload is old and a key rolling operation has since taken place.</span></span> <span data-ttu-id="415a0-122">Вызывающий объект можете рассмотреть возможность размещения полезных данных в зависимости от их бизнес-требованиям.</span><span class="sxs-lookup"><span data-stu-id="415a0-122">The caller may wish to consider reprotecting the payload depending on their business needs.</span></span>

* <span data-ttu-id="415a0-123">`wasRevoked`: будет присвоено значение true, если ключ, используемый для защиты этого полезных данных был отозван.</span><span class="sxs-lookup"><span data-stu-id="415a0-123">`wasRevoked`: will be set to true if the key used to protect this payload was revoked.</span></span>

>[!WARNING]
> <span data-ttu-id="415a0-124">Соблюдайте осторожность при передаче `ignoreRevocationErrors: true` для `DangerousUnprotect` метод.</span><span class="sxs-lookup"><span data-stu-id="415a0-124">Exercise extreme caution when passing `ignoreRevocationErrors: true` to the `DangerousUnprotect` method.</span></span> <span data-ttu-id="415a0-125">Если после вызова этого метода `wasRevoked` имеет значение true, то ключ, используемый для защиты этого полезных данных был отозван и подлинность полезных данных, которые должны рассматриваться как подозрительная.</span><span class="sxs-lookup"><span data-stu-id="415a0-125">If after calling this method the `wasRevoked` value is true, then the key used to protect this payload was revoked, and the payload's authenticity should be treated as suspect.</span></span> <span data-ttu-id="415a0-126">В этом случае только продолжают работать в незащищенном полезных данных при наличии некоторую уверенность в отдельном что его подлинность, например, что он поступающих от защищенную базу данных, а не отправки клиентом ненадежных веб.</span><span class="sxs-lookup"><span data-stu-id="415a0-126">In this case, only continue operating on the unprotected payload if you have some separate assurance that it is authentic, e.g. that it's coming from a secure database rather than being sent by an untrusted web client.</span></span>

[!code-csharp[Main](dangerous-unprotect/samples/dangerous-unprotect.cs)]
