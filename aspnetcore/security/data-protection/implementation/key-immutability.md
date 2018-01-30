---
title: "Неизменность ключа и изменение параметров"
author: rick-anderson
description: "В этом документе перечислены сведения о реализации неизменности защиты ключа API-интерфейсы данных ASP.NET Core."
manager: wpickett
ms.author: riande
ms.date: 10/14/2016
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: security/data-protection/implementation/key-immutability
ms.openlocfilehash: 98727c7a0c525edcda4fd8d004e0ac584cf0a5e5
ms.sourcegitcommit: a510f38930abc84c4b302029d019a34dfe76823b
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/30/2018
---
# <a name="key-immutability-and-changing-settings"></a><span data-ttu-id="79f86-103">Неизменность ключ и изменение параметров</span><span class="sxs-lookup"><span data-stu-id="79f86-103">Key Immutability and changing settings</span></span>

<span data-ttu-id="79f86-104">Когда объект сохраняется в резервное хранилище, его представление навсегда исправлена.</span><span class="sxs-lookup"><span data-stu-id="79f86-104">Once an object is persisted to the backing store, its representation is forever fixed.</span></span> <span data-ttu-id="79f86-105">Новые данные добавляются в резервное хранилище, но никогда не может изменить существующие данные.</span><span class="sxs-lookup"><span data-stu-id="79f86-105">New data can be added to the backing store, but existing data can never be mutated.</span></span> <span data-ttu-id="79f86-106">Основная цель такого поведения — во избежание повреждения данных.</span><span class="sxs-lookup"><span data-stu-id="79f86-106">The primary purpose of this behavior is to prevent data corruption.</span></span>

<span data-ttu-id="79f86-107">Одним из следствий этого поведения является что после написания резервное хранилище ключа является неизменяемым.</span><span class="sxs-lookup"><span data-stu-id="79f86-107">One consequence of this behavior is that once a key is written to the backing store, it's immutable.</span></span> <span data-ttu-id="79f86-108">Даты его создания, активации и истечения срока действия не может быть изменено, хотя он может быть отменено с помощью `IKeyManager`.</span><span class="sxs-lookup"><span data-stu-id="79f86-108">Its creation, activation, and expiration dates can never be changed, though it can revoked by using `IKeyManager`.</span></span> <span data-ttu-id="79f86-109">Кроме того его алгоритмической данных, материала основного и шифрование на остальные свойства также являются неизменяемыми.</span><span class="sxs-lookup"><span data-stu-id="79f86-109">Additionally, its underlying algorithmic information, master keying material, and encryption at rest properties are also immutable.</span></span>

<span data-ttu-id="79f86-110">Если разработчик изменяет параметров, которые затрагивает сохраняемость ключа, эти изменения не вступают в силу до следующего ключа формируется, либо через явный вызов `IKeyManager.CreateNewKey` или с помощью данных защиты системы собственные [автоматического ключ Создание](key-management.md#data-protection-implementation-key-management) поведение.</span><span class="sxs-lookup"><span data-stu-id="79f86-110">If the developer changes any setting that affects key persistence, those changes won't go into effect until the next time a key is generated, either via an explicit call to `IKeyManager.CreateNewKey` or via the data protection system's own [automatic key generation](key-management.md#data-protection-implementation-key-management) behavior.</span></span> <span data-ttu-id="79f86-111">Доступны следующие параметры, влияющие на сохраняемость ключа:</span><span class="sxs-lookup"><span data-stu-id="79f86-111">The settings that affect key persistence are as follows:</span></span>

* [<span data-ttu-id="79f86-112">Время жизни ключа по умолчанию</span><span class="sxs-lookup"><span data-stu-id="79f86-112">The default key lifetime</span></span>](key-management.md#data-protection-implementation-key-management)

* [<span data-ttu-id="79f86-113">Шифрование ключа в механизм rest</span><span class="sxs-lookup"><span data-stu-id="79f86-113">The key encryption at rest mechanism</span></span>](key-encryption-at-rest.md#data-protection-implementation-key-encryption-at-rest)

* [<span data-ttu-id="79f86-114">Алгоритмической сведений, содержащихся в ключ</span><span class="sxs-lookup"><span data-stu-id="79f86-114">The algorithmic information contained within the key</span></span>](xref:security/data-protection/configuration/overview#changing-algorithms-with-usecryptographicalgorithms)

<span data-ttu-id="79f86-115">Если необходимо, чтобы эти параметры, чтобы запустить более ранних, чем следующий ключ автоматического отката времени, попробуйте сделать явный вызов `IKeyManager.CreateNewKey` для принудительного создания нового ключа.</span><span class="sxs-lookup"><span data-stu-id="79f86-115">If you need these settings to kick in earlier than the next automatic key rolling time, consider making an explicit call to `IKeyManager.CreateNewKey` to force the creation of a new key.</span></span> <span data-ttu-id="79f86-116">Необходимо указать дату явная активация ({теперь + 2 дня} является хорошим правило пробуждается изменение распространилось) и Дата окончания срока действия в вызове.</span><span class="sxs-lookup"><span data-stu-id="79f86-116">Remember to provide an explicit activation date ({ now + 2 days } is a good rule of thumb to allow time for the change to propagate) and expiration date in the call.</span></span>

>[!TIP]
> <span data-ttu-id="79f86-117">Все приложения, коснувшись репозитория следует указать те же параметры с `IDataProtectionBuilder` методы расширения.</span><span class="sxs-lookup"><span data-stu-id="79f86-117">All applications touching the repository should specify the same settings with the `IDataProtectionBuilder` extension methods.</span></span> <span data-ttu-id="79f86-118">В противном случае свойства сохраненного ключа будет зависеть от конкретного приложения, который вызван процедуры создания ключей.</span><span class="sxs-lookup"><span data-stu-id="79f86-118">Otherwise, the properties of the persisted key will be dependent on the particular application that invoked the key generation routines.</span></span>
