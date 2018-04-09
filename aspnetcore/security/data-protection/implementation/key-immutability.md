---
title: Неизменность ключа и параметры ключа в ASP.NET Core
author: rick-anderson
description: Узнайте подробности реализации ключа неизменность ASP.NET Core Data Protection API-интерфейсы.
manager: wpickett
ms.author: riande
ms.date: 10/14/2016
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: security/data-protection/implementation/key-immutability
ms.openlocfilehash: e918b00562aca9821de87c38f10242177517d8a5
ms.sourcegitcommit: 48beecfe749ddac52bc79aa3eb246a2dcdaa1862
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/22/2018
---
# <a name="key-immutability-and-key-settings-in-aspnet-core"></a>Неизменность ключа и параметры ключа в ASP.NET Core

Когда объект сохраняется в резервное хранилище, его представление навсегда исправлена. Новые данные добавляются в резервное хранилище, но никогда не может изменить существующие данные. Основная цель такого поведения — во избежание повреждения данных.

Одним из следствий этого поведения является что после написания резервное хранилище ключа является неизменяемым. Даты его создания, активации и истечения срока действия не может быть изменено, хотя он может быть отменено с помощью `IKeyManager`. Кроме того его алгоритмической данных, материала основного и шифрование на остальные свойства также являются неизменяемыми.

Если разработчик изменяет параметров, которые затрагивает сохраняемость ключа, эти изменения не вступают в силу до следующего ключа формируется, либо через явный вызов `IKeyManager.CreateNewKey` или с помощью данных защиты системы собственные [автоматического ключ Создание](xref:security/data-protection/implementation/key-management#data-protection-implementation-key-management) поведение. Доступны следующие параметры, влияющие на сохраняемость ключа:

* [Время жизни ключа по умолчанию](xref:security/data-protection/implementation/key-management#data-protection-implementation-key-management)

* [Шифрование ключа в механизм rest](xref:security/data-protection/implementation/key-encryption-at-rest#data-protection-implementation-key-encryption-at-rest)

* [Алгоритмической сведений, содержащихся в ключ](xref:security/data-protection/configuration/overview#changing-algorithms-with-usecryptographicalgorithms)

Если необходимо, чтобы эти параметры, чтобы запустить более ранних, чем следующий ключ автоматического отката времени, попробуйте сделать явный вызов `IKeyManager.CreateNewKey` для принудительного создания нового ключа. Необходимо указать дату явная активация ({теперь + 2 дня} является хорошим правило пробуждается изменение распространилось) и Дата окончания срока действия в вызове.

>[!TIP]
> Все приложения, коснувшись репозитория следует указать те же параметры с `IDataProtectionBuilder` методы расширения. В противном случае свойства сохраненного ключа будет зависеть от конкретного приложения, который вызван процедуры создания ключей.
