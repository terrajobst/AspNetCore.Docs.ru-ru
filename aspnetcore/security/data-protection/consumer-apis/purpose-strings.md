---
title: "Назначение строк"
author: rick-anderson
description: 
keywords: ASP.NET Core
ms.author: riande
manager: wpickett
ms.date: 10/14/2016
ms.topic: article
ms.assetid: c96ed361-c382-4980-8933-800e740cfc38
ms.technology: aspnet
ms.prod: asp.net-core
uid: security/data-protection/consumer-apis/purpose-strings
ms.openlocfilehash: cc33bcfab4945e6d6f9ca7e61edeff4d1837661a
ms.sourcegitcommit: 0b6c8e6d81d2b3c161cd375036eecbace46a9707
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 08/11/2017
---
# <a name="purpose-strings"></a><span data-ttu-id="142a7-103">Назначение строк</span><span class="sxs-lookup"><span data-stu-id="142a7-103">Purpose Strings</span></span>

<a name=data-protection-consumer-apis-purposes></a>

<span data-ttu-id="142a7-104">Компоненты, которые занимают IDataProtectionProvider необходимо передать уникальный *целей* параметр метода CreateProtector.</span><span class="sxs-lookup"><span data-stu-id="142a7-104">Components which consume IDataProtectionProvider must pass a unique *purposes* parameter to the CreateProtector method.</span></span> <span data-ttu-id="142a7-105">Целей *параметр* обеспечивается самой безопасность системы защиты данных, как она обеспечивает изоляцию между шифрования потребителей, даже если корневой криптографические ключи совпадают.</span><span class="sxs-lookup"><span data-stu-id="142a7-105">The purposes *parameter* is inherent to the security of the data protection system, as it provides isolation between cryptographic consumers, even if the root cryptographic keys are the same.</span></span>

<span data-ttu-id="142a7-106">Если потребитель задает цели, строка назначения используется вместе с корневой ключи шифрования для формирования шифрования подразделов уникальным для этого клиента.</span><span class="sxs-lookup"><span data-stu-id="142a7-106">When a consumer specifies a purpose, the purpose string is used along with the root cryptographic keys to derive cryptographic subkeys unique to that consumer.</span></span> <span data-ttu-id="142a7-107">Это изолирует потребитель от других криптографических получателей в приложении: ни один другой компонент может читать его полезные данные, и он не может читать полезные данные любого другого компонента.</span><span class="sxs-lookup"><span data-stu-id="142a7-107">This isolates the consumer from all other cryptographic consumers in the application: no other component can read its payloads, and it cannot read any other component's payloads.</span></span> <span data-ttu-id="142a7-108">Эта изоляция также выводит затратна всей категории атаки компонента.</span><span class="sxs-lookup"><span data-stu-id="142a7-108">This isolation also renders infeasible entire categories of attack against the component.</span></span>

![Пример схемы назначения](purpose-strings/_static/purposes.png)

<span data-ttu-id="142a7-110">На диаграмме выше IDataProtector экземпляры A и B **нельзя** читать друг друга полезные данные, только свои собственные.</span><span class="sxs-lookup"><span data-stu-id="142a7-110">In the diagram above IDataProtector instances A and B **cannot** read each other's payloads, only their own.</span></span>

<span data-ttu-id="142a7-111">Строка цели не обязан быть секрета.</span><span class="sxs-lookup"><span data-stu-id="142a7-111">The purpose string doesn't have to be secret.</span></span> <span data-ttu-id="142a7-112">Он просто должно быть уникальным в том смысле, что ни один другой компонент правильно когда-либо будет предоставлять та же строка цели.</span><span class="sxs-lookup"><span data-stu-id="142a7-112">It should simply be unique in the sense that no other well-behaved component will ever provide the same purpose string.</span></span>

>[!TIP]
> <span data-ttu-id="142a7-113">Использование пространств имен и типов имя компонента, использует интерфейсы API защиты данных — хорошее проверенное правило, как и практик эти данные никогда не приведет к конфликту.</span><span class="sxs-lookup"><span data-stu-id="142a7-113">Using the namespace and type name of the component consuming the data protection APIs is a good rule of thumb, as in practice this information will never conflict.</span></span>
>
><span data-ttu-id="142a7-114">Созданную пользователем Contoso компонент, который отвечает за minting маркеров носителя может использовать Contoso.Security.BearerToken в качестве его назначение строки.</span><span class="sxs-lookup"><span data-stu-id="142a7-114">A Contoso-authored component which is responsible for minting bearer tokens might use Contoso.Security.BearerToken as its purpose string.</span></span> <span data-ttu-id="142a7-115">Или - более - Contoso.Security.BearerToken.v1 его использовать как его назначение строки.</span><span class="sxs-lookup"><span data-stu-id="142a7-115">Or - even better - it might use Contoso.Security.BearerToken.v1 as its purpose string.</span></span> <span data-ttu-id="142a7-116">Добавление номер версии позволяет будущей версии для использования в качестве его назначение Contoso.Security.BearerToken.v2 и разные версии будет полностью изолированы друг от друга, как перейти полезных данных.</span><span class="sxs-lookup"><span data-stu-id="142a7-116">Appending the version number allows a future version to use Contoso.Security.BearerToken.v2 as its purpose, and the different versions would be completely isolated from one another as far as payloads go.</span></span>

<span data-ttu-id="142a7-117">Так как параметр целей CreateProtector, массив строк, указанных выше может вместо указаны как [«Contoso.Security.BearerToken», «v1»].</span><span class="sxs-lookup"><span data-stu-id="142a7-117">Since the purposes parameter to CreateProtector is a string array, the above could have been instead specified as [ "Contoso.Security.BearerToken", "v1" ].</span></span> <span data-ttu-id="142a7-118">Это позволяет установление иерархии целей и открывает возможность многопользовательский режим сценариев с системой защиты данных.</span><span class="sxs-lookup"><span data-stu-id="142a7-118">This allows establishing a hierarchy of purposes and opens up the possibility of multi-tenancy scenarios with the data protection system.</span></span>

<a name=data-protection-contoso-purpose></a>

>[!WARNING]
> <span data-ttu-id="142a7-119">Компоненты, не следует разрешать недоверенному пользователю вход в качестве единственного источника входных данных для целей цепочки.</span><span class="sxs-lookup"><span data-stu-id="142a7-119">Components should not allow untrusted user input to be the sole source of input for the purposes chain.</span></span>
>
><span data-ttu-id="142a7-120">Например рассмотрим компонент Contoso.Messaging.SecureMessage, который отвечает за хранение защищенных сообщений.</span><span class="sxs-lookup"><span data-stu-id="142a7-120">For example, consider a component Contoso.Messaging.SecureMessage which is responsible for storing secure messages.</span></span> <span data-ttu-id="142a7-121">Если бы безопасного обмена сообщениями компонента вызвал CreateProtector ([username]), то злоумышленник может создать учетную запись с именем пользователя «Contoso.Security.BearerToken» при попытке получить компонент может вызвать CreateProtector([" Contoso.Security.BearerToken»]), таким образом непреднамеренно вследствие чего безопасного обмена сообщениями система mint полезных данных, может быть воспринято как маркеры проверки подлинности.</span><span class="sxs-lookup"><span data-stu-id="142a7-121">If the secure messaging component were to call CreateProtector([ username ]), then a malicious user might create an account with username "Contoso.Security.BearerToken" in an attempt to get the component to call CreateProtector([ "Contoso.Security.BearerToken" ]), thus inadvertently causing the secure messaging system to mint payloads that could be perceived as authentication tokens.</span></span>
>
><span data-ttu-id="142a7-122">Лучше цепочки целей для обмена сообщениями компонента будет CreateProtector ([«Contoso.Messaging.SecureMessage», «пользователя: имя пользователя»]), предоставляющее правильную изоляции.</span><span class="sxs-lookup"><span data-stu-id="142a7-122">A better purposes chain for the messaging component would be CreateProtector([ "Contoso.Messaging.SecureMessage", "User: username" ]), which provides proper isolation.</span></span>

<span data-ttu-id="142a7-123">Изоляция, предоставляемая и поведение IDataProtectionProvider, IDataProtector и целей таковы:</span><span class="sxs-lookup"><span data-stu-id="142a7-123">The isolation provided by and behaviors of IDataProtectionProvider, IDataProtector, and purposes are as follows:</span></span>

* <span data-ttu-id="142a7-124">Для данного объекта IDataProtectionProvider метод CreateProtector создает объект IDataProtector, уникальным образом привязаны к IDataProtectionProvider объекта, который его создал и параметр целей, который был передан в метод.</span><span class="sxs-lookup"><span data-stu-id="142a7-124">For a given IDataProtectionProvider object, the CreateProtector method will create an IDataProtector object uniquely tied to both the IDataProtectionProvider object which created it and the purposes parameter which was passed into the method.</span></span>

* <span data-ttu-id="142a7-125">Назначение параметра не может иметь значение null.</span><span class="sxs-lookup"><span data-stu-id="142a7-125">The purpose parameter must not be null.</span></span> <span data-ttu-id="142a7-126">(Если целей задан как массив, это означает, что массив не может быть нулевой длины, и все элементы массива должны быть отличным от null.) Пустая строка цели технически разрешено, но не рекомендуется.</span><span class="sxs-lookup"><span data-stu-id="142a7-126">(If purposes is specified as an array, this means that the array must not be of zero length and all elements of the array must be non-null.) An empty string purpose is technically allowed but is discouraged.</span></span>

* <span data-ttu-id="142a7-127">Аргументы для двух целей эквивалентны только в том случае, если они содержат одинаковые строки (используя компаратор проверки на порядковый номер) в том же порядке.</span><span class="sxs-lookup"><span data-stu-id="142a7-127">Two purposes arguments are equivalent if and only if they contain the same strings (using an ordinal comparer) in the same order.</span></span> <span data-ttu-id="142a7-128">Аргумент одноцелевой эквивалентно соответствующего целей одноэлементный массив.</span><span class="sxs-lookup"><span data-stu-id="142a7-128">A single purpose argument is equivalent to the corresponding single-element purposes array.</span></span>

* <span data-ttu-id="142a7-129">Два объекта IDataProtector эквивалентны только в том случае, если они создаются из эквивалентных объектов IDataProtectionProvider с параметрами эквивалентные целей.</span><span class="sxs-lookup"><span data-stu-id="142a7-129">Two IDataProtector objects are equivalent if and only if they are created from equivalent IDataProtectionProvider objects with equivalent purposes parameters.</span></span>

* <span data-ttu-id="142a7-130">Для данного объекта IDataProtector, вызов Unprotect(protectedData) возвращает исходный unprotectedData Если и только в том случае, если protectedData: = Protect(unprotectedData) для эквивалентный объект IDataProtector.</span><span class="sxs-lookup"><span data-stu-id="142a7-130">For a given IDataProtector object, a call to Unprotect(protectedData) will return the original unprotectedData if and only if protectedData := Protect(unprotectedData) for an equivalent IDataProtector object.</span></span>

> [!NOTE]
> <span data-ttu-id="142a7-131">Мы не рассматриваем случай, где компонент намеренно выбирает строки цели, известной в конфликт с другим компонентом.</span><span class="sxs-lookup"><span data-stu-id="142a7-131">We're not considering the case where some component intentionally chooses a purpose string which is known to conflict with another component.</span></span> <span data-ttu-id="142a7-132">Такой компонент будет по существу считаться вредоносных и эта система не предназначен для обеспечения гарантии безопасности, в том случае, если вредоносный код уже выполняется внутри рабочего процесса.</span><span class="sxs-lookup"><span data-stu-id="142a7-132">Such a component would essentially be considered malicious, and this system is not intended to provide security guarantees in the event that malicious code is already running inside of the worker process.</span></span>
