---
title: Неизменность ключа и настройки параметров в ASP.NET Core
author: rick-anderson
description: Узнайте подробности реализации неизменность ключа защиты данных в ASP.NET Core API-интерфейсы.
ms.author: riande
ms.date: 10/14/2016
uid: security/data-protection/implementation/key-immutability
ms.openlocfilehash: 7796cb102c0f6f03809704016fd36b28c7a82438
ms.sourcegitcommit: 8f8924ce4eb9effeaf489f177fb01b66867da16f
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 07/24/2018
ms.locfileid: "39219307"
---
# <a name="key-immutability-and-key-settings-in-aspnet-core"></a><span data-ttu-id="b9e49-103">Неизменность ключа и настройки параметров в ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="b9e49-103">Key immutability and key settings in ASP.NET Core</span></span>

<span data-ttu-id="b9e49-104">Когда объект сохраняется в резервное хранилище, его представление навсегда является фиксированным.</span><span class="sxs-lookup"><span data-stu-id="b9e49-104">Once an object is persisted to the backing store, its representation is forever fixed.</span></span> <span data-ttu-id="b9e49-105">Новые данные добавляются в хранилище, но существующие данные никогда не может быть изменено.</span><span class="sxs-lookup"><span data-stu-id="b9e49-105">New data can be added to the backing store, but existing data can never be mutated.</span></span> <span data-ttu-id="b9e49-106">Основное назначение такого поведения является избежание повреждения данных.</span><span class="sxs-lookup"><span data-stu-id="b9e49-106">The primary purpose of this behavior is to prevent data corruption.</span></span>

<span data-ttu-id="b9e49-107">Следствием этого поведения является то, что после ключ записывается в резервное хранилище, он станет неизменяемым.</span><span class="sxs-lookup"><span data-stu-id="b9e49-107">One consequence of this behavior is that once a key is written to the backing store, it's immutable.</span></span> <span data-ttu-id="b9e49-108">Даты создания, активации и истечения срока действия не может быть изменено, хотя он может быть отменено с помощью `IKeyManager`.</span><span class="sxs-lookup"><span data-stu-id="b9e49-108">Its creation, activation, and expiration dates can never be changed, though it can revoked by using `IKeyManager`.</span></span> <span data-ttu-id="b9e49-109">Кроме того его сведений о базовом алгоритмической, материала основного и шифрование на свойства rest также являются неизменяемыми.</span><span class="sxs-lookup"><span data-stu-id="b9e49-109">Additionally, its underlying algorithmic information, master keying material, and encryption at rest properties are also immutable.</span></span>

<span data-ttu-id="b9e49-110">Если разработчик изменяет параметров, которые влияют на постоянство ключа, эти изменения не вступают в силу до следующего создается ключ, либо с помощью явного вызова `IKeyManager.CreateNewKey` или с помощью система защиты данных в собственном [ключ автоматической Создание](xref:security/data-protection/implementation/key-management#data-protection-implementation-key-management) поведение.</span><span class="sxs-lookup"><span data-stu-id="b9e49-110">If the developer changes any setting that affects key persistence, those changes won't go into effect until the next time a key is generated, either via an explicit call to `IKeyManager.CreateNewKey` or via the data protection system's own [automatic key generation](xref:security/data-protection/implementation/key-management#data-protection-implementation-key-management) behavior.</span></span> <span data-ttu-id="b9e49-111">Ниже приведены параметры, которые влияют на постоянство ключа.</span><span class="sxs-lookup"><span data-stu-id="b9e49-111">The settings that affect key persistence are as follows:</span></span>

* [<span data-ttu-id="b9e49-112">Время существования ключа по умолчанию</span><span class="sxs-lookup"><span data-stu-id="b9e49-112">The default key lifetime</span></span>](xref:security/data-protection/implementation/key-management#data-protection-implementation-key-management)

* [<span data-ttu-id="b9e49-113">Шифрование ключей при механизм rest</span><span class="sxs-lookup"><span data-stu-id="b9e49-113">The key encryption at rest mechanism</span></span>](xref:security/data-protection/implementation/key-encryption-at-rest)

* [<span data-ttu-id="b9e49-114">Алгоритмического сведения, содержащиеся в ключ</span><span class="sxs-lookup"><span data-stu-id="b9e49-114">The algorithmic information contained within the key</span></span>](xref:security/data-protection/configuration/overview#changing-algorithms-with-usecryptographicalgorithms)

<span data-ttu-id="b9e49-115">Если требуется, чтобы эти параметры, чтобы запустить более ранних, чем следующий ключ автоматическое скользящее время, имеет смысл сделать явный вызов `IKeyManager.CreateNewKey` для принудительного создания нового ключа.</span><span class="sxs-lookup"><span data-stu-id="b9e49-115">If you need these settings to kick in earlier than the next automatic key rolling time, consider making an explicit call to `IKeyManager.CreateNewKey` to force the creation of a new key.</span></span> <span data-ttu-id="b9e49-116">Необходимо указать дату явной активации ({теперь + 2 дня} — это хорошее проверенное правило для выделить время на распространение изменения) и дату окончания срока действия в вызове.</span><span class="sxs-lookup"><span data-stu-id="b9e49-116">Remember to provide an explicit activation date ({ now + 2 days } is a good rule of thumb to allow time for the change to propagate) and expiration date in the call.</span></span>

>[!TIP]
> <span data-ttu-id="b9e49-117">Все приложения, касаясь репозитория следует указать те же параметры, с помощью `IDataProtectionBuilder` методы расширения.</span><span class="sxs-lookup"><span data-stu-id="b9e49-117">All applications touching the repository should specify the same settings with the `IDataProtectionBuilder` extension methods.</span></span> <span data-ttu-id="b9e49-118">В противном случае свойства постоянный ключ будет зависеть от конкретного приложения, вызвавший процедуры создания ключей.</span><span class="sxs-lookup"><span data-stu-id="b9e49-118">Otherwise, the properties of the persisted key will be dependent on the particular application that invoked the key generation routines.</span></span>
