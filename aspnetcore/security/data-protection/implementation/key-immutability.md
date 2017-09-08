---
title: "Неизменность ключа и изменение параметров"
author: rick-anderson
description: 
keywords: ASP.NET Core
ms.author: riande
manager: wpickett
ms.date: 10/14/2016
ms.topic: article
ms.assetid: fc911ae3-feca-4ffe-8b43-362bc632fe04
ms.technology: aspnet
ms.prod: asp.net-core
uid: security/data-protection/implementation/key-immutability
ms.openlocfilehash: 3afce8f84ebe3b709ea169c7db27f99829f157ab
ms.sourcegitcommit: 0b6c8e6d81d2b3c161cd375036eecbace46a9707
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 08/11/2017
---
# <a name="key-immutability-and-changing-settings"></a><span data-ttu-id="f60b2-103">Неизменность ключ и изменение параметров</span><span class="sxs-lookup"><span data-stu-id="f60b2-103">Key Immutability and changing settings</span></span>

<span data-ttu-id="f60b2-104">Когда объект сохраняется в резервное хранилище, его представление навсегда исправлена.</span><span class="sxs-lookup"><span data-stu-id="f60b2-104">Once an object is persisted to the backing store, its representation is forever fixed.</span></span> <span data-ttu-id="f60b2-105">Новые данные добавляются в резервное хранилище, но никогда не может изменить существующие данные.</span><span class="sxs-lookup"><span data-stu-id="f60b2-105">New data can be added to the backing store, but existing data can never be mutated.</span></span> <span data-ttu-id="f60b2-106">Основная цель такого поведения — во избежание повреждения данных.</span><span class="sxs-lookup"><span data-stu-id="f60b2-106">The primary purpose of this behavior is to prevent data corruption.</span></span>

<span data-ttu-id="f60b2-107">Одним из следствий этого поведения является что после написания резервное хранилище ключа является неизменяемым.</span><span class="sxs-lookup"><span data-stu-id="f60b2-107">One consequence of this behavior is that once a key is written to the backing store, it is immutable.</span></span> <span data-ttu-id="f60b2-108">Даты его создания, активации и истечения срока действия не может быть изменено, хотя он может быть отменено с помощью IKeyManager.</span><span class="sxs-lookup"><span data-stu-id="f60b2-108">Its creation, activation, and expiration dates can never be changed, though it can revoked by using IKeyManager.</span></span> <span data-ttu-id="f60b2-109">Кроме того его алгоритмической данных, материала основного и шифрование на остальные свойства также являются неизменяемыми.</span><span class="sxs-lookup"><span data-stu-id="f60b2-109">Additionally, its underlying algorithmic information, master keying material, and encryption at rest properties are also immutable.</span></span>

<span data-ttu-id="f60b2-110">Если разработчик изменяет параметров, которые затрагивает сохраняемость ключа, эти изменения не вступают в силу до следующего ключа формируется через явный вызов IKeyManager.CreateNewKey или через данных защиты системы собственные [автоматический Создание ключа](key-management.md#data-protection-implementation-key-management) поведение.</span><span class="sxs-lookup"><span data-stu-id="f60b2-110">If the developer changes any setting that affects key persistence, those changes will not go into effect until the next time a key is generated, either via an explicit call to IKeyManager.CreateNewKey or via the data protection system's own [automatic key generation](key-management.md#data-protection-implementation-key-management) behavior.</span></span> <span data-ttu-id="f60b2-111">Доступны следующие параметры, влияющие на сохраняемость ключа:</span><span class="sxs-lookup"><span data-stu-id="f60b2-111">The settings that affect key persistence are as follows:</span></span>

* [<span data-ttu-id="f60b2-112">Время жизни ключа по умолчанию</span><span class="sxs-lookup"><span data-stu-id="f60b2-112">The default key lifetime</span></span>](key-management.md#data-protection-implementation-key-management)

* [<span data-ttu-id="f60b2-113">Шифрование ключа в механизм rest</span><span class="sxs-lookup"><span data-stu-id="f60b2-113">The key encryption at rest mechanism</span></span>](key-encryption-at-rest.md#data-protection-implementation-key-encryption-at-rest)

* [<span data-ttu-id="f60b2-114">Алгоритмической сведений, содержащихся в ключ</span><span class="sxs-lookup"><span data-stu-id="f60b2-114">The algorithmic information contained within the key</span></span>](../configuration/overview.md#data-protection-changing-algorithms)

<span data-ttu-id="f60b2-115">Если необходимо, чтобы эти параметры, чтобы запустить более ранних, чем следующий ключ автоматического отката времени, имеет смысл сделать явный вызов IKeyManager.CreateNewKey для принудительного создания нового ключа.</span><span class="sxs-lookup"><span data-stu-id="f60b2-115">If you need these settings to kick in earlier than the next automatic key rolling time, consider making an explicit call to IKeyManager.CreateNewKey to force the creation of a new key.</span></span> <span data-ttu-id="f60b2-116">Необходимо указать дату явная активация ({теперь + 2 дня} является хорошим правило пробуждается изменение распространилось) и Дата окончания срока действия в вызове.</span><span class="sxs-lookup"><span data-stu-id="f60b2-116">Remember to provide an explicit activation date ({ now + 2 days } is a good rule of thumb to allow time for the change to propagate) and expiration date in the call.</span></span>

>[!TIP]
> <span data-ttu-id="f60b2-117">Все приложения, коснувшись репозитория следует указать те же параметры, с помощью методов расширения IDataProtectionBuilder, в противном случае значение свойства сохраненного ключа будет зависеть от конкретного приложения, который вызван процедуры создания ключей .</span><span class="sxs-lookup"><span data-stu-id="f60b2-117">All applications touching the repository should specify the same settings with the IDataProtectionBuilder extension methods, otherwise the properties of the persisted key will be dependent on the particular application that invoked the key generation routines.</span></span>
