---
title: Неизменность ключа и параметры ключа в ASP.NET Core
author: rick-anderson
description: Узнайте подробности реализации ключа неизменность ASP.NET Core Data Protection API-интерфейсы.
ms.author: riande
ms.date: 10/14/2016
uid: security/data-protection/implementation/key-immutability
ms.openlocfilehash: 45460e0bdf6ad0a978f984d55b64f562c13fb575
ms.sourcegitcommit: a1afd04758e663d7062a5bfa8a0d4dca38f42afc
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 06/20/2018
ms.locfileid: "36279458"
---
# <a name="key-immutability-and-key-settings-in-aspnet-core"></a><span data-ttu-id="72244-103">Неизменность ключа и параметры ключа в ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="72244-103">Key immutability and key settings in ASP.NET Core</span></span>

<span data-ttu-id="72244-104">Когда объект сохраняется в резервное хранилище, его представление навсегда исправлена.</span><span class="sxs-lookup"><span data-stu-id="72244-104">Once an object is persisted to the backing store, its representation is forever fixed.</span></span> <span data-ttu-id="72244-105">Новые данные добавляются в резервное хранилище, но никогда не может изменить существующие данные.</span><span class="sxs-lookup"><span data-stu-id="72244-105">New data can be added to the backing store, but existing data can never be mutated.</span></span> <span data-ttu-id="72244-106">Основная цель такого поведения — во избежание повреждения данных.</span><span class="sxs-lookup"><span data-stu-id="72244-106">The primary purpose of this behavior is to prevent data corruption.</span></span>

<span data-ttu-id="72244-107">Одним из следствий этого поведения является что после написания резервное хранилище ключа является неизменяемым.</span><span class="sxs-lookup"><span data-stu-id="72244-107">One consequence of this behavior is that once a key is written to the backing store, it's immutable.</span></span> <span data-ttu-id="72244-108">Даты его создания, активации и истечения срока действия не может быть изменено, хотя он может быть отменено с помощью `IKeyManager`.</span><span class="sxs-lookup"><span data-stu-id="72244-108">Its creation, activation, and expiration dates can never be changed, though it can revoked by using `IKeyManager`.</span></span> <span data-ttu-id="72244-109">Кроме того его алгоритмической данных, материала основного и шифрование на остальные свойства также являются неизменяемыми.</span><span class="sxs-lookup"><span data-stu-id="72244-109">Additionally, its underlying algorithmic information, master keying material, and encryption at rest properties are also immutable.</span></span>

<span data-ttu-id="72244-110">Если разработчик изменяет параметров, которые затрагивает сохраняемость ключа, эти изменения не вступают в силу до следующего ключа формируется, либо через явный вызов `IKeyManager.CreateNewKey` или с помощью данных защиты системы собственные [автоматического ключ Создание](xref:security/data-protection/implementation/key-management#data-protection-implementation-key-management) поведение.</span><span class="sxs-lookup"><span data-stu-id="72244-110">If the developer changes any setting that affects key persistence, those changes won't go into effect until the next time a key is generated, either via an explicit call to `IKeyManager.CreateNewKey` or via the data protection system's own [automatic key generation](xref:security/data-protection/implementation/key-management#data-protection-implementation-key-management) behavior.</span></span> <span data-ttu-id="72244-111">Доступны следующие параметры, влияющие на сохраняемость ключа:</span><span class="sxs-lookup"><span data-stu-id="72244-111">The settings that affect key persistence are as follows:</span></span>

* [<span data-ttu-id="72244-112">Время жизни ключа по умолчанию</span><span class="sxs-lookup"><span data-stu-id="72244-112">The default key lifetime</span></span>](xref:security/data-protection/implementation/key-management#data-protection-implementation-key-management)

* [<span data-ttu-id="72244-113">Шифрование ключа в механизм rest</span><span class="sxs-lookup"><span data-stu-id="72244-113">The key encryption at rest mechanism</span></span>](xref:security/data-protection/implementation/key-encryption-at-rest#data-protection-implementation-key-encryption-at-rest)

* [<span data-ttu-id="72244-114">Алгоритмической сведений, содержащихся в ключ</span><span class="sxs-lookup"><span data-stu-id="72244-114">The algorithmic information contained within the key</span></span>](xref:security/data-protection/configuration/overview#changing-algorithms-with-usecryptographicalgorithms)

<span data-ttu-id="72244-115">Если необходимо, чтобы эти параметры, чтобы запустить более ранних, чем следующий ключ автоматического отката времени, попробуйте сделать явный вызов `IKeyManager.CreateNewKey` для принудительного создания нового ключа.</span><span class="sxs-lookup"><span data-stu-id="72244-115">If you need these settings to kick in earlier than the next automatic key rolling time, consider making an explicit call to `IKeyManager.CreateNewKey` to force the creation of a new key.</span></span> <span data-ttu-id="72244-116">Необходимо указать дату явная активация ({теперь + 2 дня} является хорошим правило пробуждается изменение распространилось) и Дата окончания срока действия в вызове.</span><span class="sxs-lookup"><span data-stu-id="72244-116">Remember to provide an explicit activation date ({ now + 2 days } is a good rule of thumb to allow time for the change to propagate) and expiration date in the call.</span></span>

>[!TIP]
> <span data-ttu-id="72244-117">Все приложения, коснувшись репозитория следует указать те же параметры с `IDataProtectionBuilder` методы расширения.</span><span class="sxs-lookup"><span data-stu-id="72244-117">All applications touching the repository should specify the same settings with the `IDataProtectionBuilder` extension methods.</span></span> <span data-ttu-id="72244-118">В противном случае свойства сохраненного ключа будет зависеть от конкретного приложения, который вызван процедуры создания ключей.</span><span class="sxs-lookup"><span data-stu-id="72244-118">Otherwise, the properties of the persisted key will be dependent on the particular application that invoked the key generation routines.</span></span>
