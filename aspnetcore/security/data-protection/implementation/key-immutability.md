---
title: "Неизменность ключа и изменение параметров"
author: rick-anderson
description: "В этом документе перечислены сведения о реализации неизменности защиты ключа API-интерфейсы данных ASP.NET Core."
keywords: "Неизменность ключа ASP.NET Core, защиты данных"
ms.author: riande
manager: wpickett
ms.date: 10/14/2016
ms.topic: article
ms.assetid: fc911ae3-feca-4ffe-8b43-362bc632fe04
ms.technology: aspnet
ms.prod: asp.net-core
uid: security/data-protection/implementation/key-immutability
ms.openlocfilehash: 96860b44b64f241a1bbff2ac8366e0863b1ac10c
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 11/10/2017
---
# <a name="key-immutability-and-changing-settings"></a><span data-ttu-id="d0f59-104">Неизменность ключ и изменение параметров</span><span class="sxs-lookup"><span data-stu-id="d0f59-104">Key Immutability and changing settings</span></span>

<span data-ttu-id="d0f59-105">Когда объект сохраняется в резервное хранилище, его представление навсегда исправлена.</span><span class="sxs-lookup"><span data-stu-id="d0f59-105">Once an object is persisted to the backing store, its representation is forever fixed.</span></span> <span data-ttu-id="d0f59-106">Новые данные добавляются в резервное хранилище, но никогда не может изменить существующие данные.</span><span class="sxs-lookup"><span data-stu-id="d0f59-106">New data can be added to the backing store, but existing data can never be mutated.</span></span> <span data-ttu-id="d0f59-107">Основная цель такого поведения — во избежание повреждения данных.</span><span class="sxs-lookup"><span data-stu-id="d0f59-107">The primary purpose of this behavior is to prevent data corruption.</span></span>

<span data-ttu-id="d0f59-108">Одним из следствий этого поведения является что после написания резервное хранилище ключа является неизменяемым.</span><span class="sxs-lookup"><span data-stu-id="d0f59-108">One consequence of this behavior is that once a key is written to the backing store, it is immutable.</span></span> <span data-ttu-id="d0f59-109">Даты его создания, активации и истечения срока действия не может быть изменено, хотя он может быть отменено с помощью `IKeyManager`.</span><span class="sxs-lookup"><span data-stu-id="d0f59-109">Its creation, activation, and expiration dates can never be changed, though it can revoked by using `IKeyManager`.</span></span> <span data-ttu-id="d0f59-110">Кроме того его алгоритмической данных, материала основного и шифрование на остальные свойства также являются неизменяемыми.</span><span class="sxs-lookup"><span data-stu-id="d0f59-110">Additionally, its underlying algorithmic information, master keying material, and encryption at rest properties are also immutable.</span></span>

<span data-ttu-id="d0f59-111">Если разработчик изменяет параметров, которые затрагивает сохраняемость ключа, эти изменения не вступают в силу до следующего ключа формируется, либо через явный вызов `IKeyManager.CreateNewKey` или с помощью данных защиты системы собственные [автоматического ключ Создание](key-management.md#data-protection-implementation-key-management) поведение.</span><span class="sxs-lookup"><span data-stu-id="d0f59-111">If the developer changes any setting that affects key persistence, those changes will not go into effect until the next time a key is generated, either via an explicit call to `IKeyManager.CreateNewKey` or via the data protection system's own [automatic key generation](key-management.md#data-protection-implementation-key-management) behavior.</span></span> <span data-ttu-id="d0f59-112">Доступны следующие параметры, влияющие на сохраняемость ключа:</span><span class="sxs-lookup"><span data-stu-id="d0f59-112">The settings that affect key persistence are as follows:</span></span>

* [<span data-ttu-id="d0f59-113">Время жизни ключа по умолчанию</span><span class="sxs-lookup"><span data-stu-id="d0f59-113">The default key lifetime</span></span>](key-management.md#data-protection-implementation-key-management)

* [<span data-ttu-id="d0f59-114">Шифрование ключа в механизм rest</span><span class="sxs-lookup"><span data-stu-id="d0f59-114">The key encryption at rest mechanism</span></span>](key-encryption-at-rest.md#data-protection-implementation-key-encryption-at-rest)

* [<span data-ttu-id="d0f59-115">Алгоритмической сведений, содержащихся в ключ</span><span class="sxs-lookup"><span data-stu-id="d0f59-115">The algorithmic information contained within the key</span></span>](xref:security/data-protection/configuration/overview#changing-algorithms-with-usecryptographicalgorithms)

<span data-ttu-id="d0f59-116">Если необходимо, чтобы эти параметры, чтобы запустить более ранних, чем следующий ключ автоматического отката времени, попробуйте сделать явный вызов `IKeyManager.CreateNewKey` для принудительного создания нового ключа.</span><span class="sxs-lookup"><span data-stu-id="d0f59-116">If you need these settings to kick in earlier than the next automatic key rolling time, consider making an explicit call to `IKeyManager.CreateNewKey` to force the creation of a new key.</span></span> <span data-ttu-id="d0f59-117">Необходимо указать дату явная активация ({теперь + 2 дня} является хорошим правило пробуждается изменение распространилось) и Дата окончания срока действия в вызове.</span><span class="sxs-lookup"><span data-stu-id="d0f59-117">Remember to provide an explicit activation date ({ now + 2 days } is a good rule of thumb to allow time for the change to propagate) and expiration date in the call.</span></span>

>[!TIP]
> <span data-ttu-id="d0f59-118">Все приложения, коснувшись репозитория следует указать те же параметры с `IDataProtectionBuilder` методы расширения.</span><span class="sxs-lookup"><span data-stu-id="d0f59-118">All applications touching the repository should specify the same settings with the `IDataProtectionBuilder` extension methods.</span></span> <span data-ttu-id="d0f59-119">В противном случае свойства сохраненного ключа будет зависеть от конкретного приложения, который вызван процедуры создания ключей.</span><span class="sxs-lookup"><span data-stu-id="d0f59-119">Otherwise, the properties of the persisted key will be dependent on the particular application that invoked the key generation routines.</span></span>
