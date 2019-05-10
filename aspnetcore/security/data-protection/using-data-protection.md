---
title: Начало работы с API защиты данных в ASP.NET Core
author: rick-anderson
description: Узнайте, как использовать интерфейсы API защиты данных ASP.NET Core для защиты и снятии с него защиты данных в приложении.
ms.author: riande
ms.date: 10/14/2016
uid: security/data-protection/using-data-protection
ms.openlocfilehash: 25bf099a3d9edd7e6e0872725cbc3707750314e6
ms.sourcegitcommit: dd9c73db7853d87b566eef136d2162f648a43b85
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 05/06/2019
ms.locfileid: "65087636"
---
# <a name="get-started-with-the-data-protection-apis-in-aspnet-core"></a><span data-ttu-id="8287c-103">Начало работы с API защиты данных в ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="8287c-103">Get started with the Data Protection APIs in ASP.NET Core</span></span>

<a name="security-data-protection-getting-started"></a>

<span data-ttu-id="8287c-104">На его самый простой и защиту данных состоит из следующих действий:</span><span class="sxs-lookup"><span data-stu-id="8287c-104">At its simplest, protecting data consists of the following steps:</span></span>

1. <span data-ttu-id="8287c-105">Создание средства защиты данных от поставщика защиты данных.</span><span class="sxs-lookup"><span data-stu-id="8287c-105">Create a data protector from a data protection provider.</span></span>

2. <span data-ttu-id="8287c-106">Вызовите `Protect` метод с данными, которые необходимо защитить.</span><span class="sxs-lookup"><span data-stu-id="8287c-106">Call the `Protect` method with the data you want to protect.</span></span>

3. <span data-ttu-id="8287c-107">Вызовите `Unprotect` метод с данными, вы хотите включить обратно в виде обычного текста.</span><span class="sxs-lookup"><span data-stu-id="8287c-107">Call the `Unprotect` method with the data you want to turn back into plain text.</span></span>

<span data-ttu-id="8287c-108">Большинство платформ и модели приложения, такие как ASP.NET Core или SignalR, уже настроить систему защиты данных и добавьте его в контейнер службы, к которым доступ посредством внедрения зависимостей.</span><span class="sxs-lookup"><span data-stu-id="8287c-108">Most frameworks and app models, such as ASP.NET Core or SignalR, already configure the data protection system and add it to a service container you access via dependency injection.</span></span> <span data-ttu-id="8287c-109">Следующий пример демонстрирует настройку контейнер службы для внедрения зависимостей и регистрации в стеке защиты данных, получения поставщик защиты данных с помощью внедрения Зависимостей, Создание средства защиты и защиту, а затем снятие защиты данных.</span><span class="sxs-lookup"><span data-stu-id="8287c-109">The following sample demonstrates configuring a service container for dependency injection and registering the data protection stack, receiving the data protection provider via DI, creating a protector and protecting then unprotecting data.</span></span>

[!code-csharp[](../../security/data-protection/using-data-protection/samples/protectunprotect.cs?highlight=26,34,35,36,37,38,39,40)]

<span data-ttu-id="8287c-110">При создании предохранитель необходимо указать один или несколько [строки назначений](xref:security/data-protection/consumer-apis/purpose-strings).</span><span class="sxs-lookup"><span data-stu-id="8287c-110">When you create a protector you must provide one or more [Purpose Strings](xref:security/data-protection/consumer-apis/purpose-strings).</span></span> <span data-ttu-id="8287c-111">Строка назначения обеспечивает изоляцию между потребителей.</span><span class="sxs-lookup"><span data-stu-id="8287c-111">A purpose string provides isolation between consumers.</span></span> <span data-ttu-id="8287c-112">Например средства защиты, созданные с помощью назначения строку «green» невозможно снять защиту данных, предоставленных предохранитель с целью «фиолетовый».</span><span class="sxs-lookup"><span data-stu-id="8287c-112">For example, a protector created with a purpose string of "green" wouldn't be able to unprotect data provided by a protector with a purpose of "purple".</span></span>

>[!TIP]
> <span data-ttu-id="8287c-113">Экземпляры `IDataProtectionProvider` и `IDataProtector` потокобезопасны для нескольких клиентов.</span><span class="sxs-lookup"><span data-stu-id="8287c-113">Instances of `IDataProtectionProvider` and `IDataProtector` are thread-safe for multiple callers.</span></span> <span data-ttu-id="8287c-114">Она предназначена, после того как компонент возвращает ссылку на `IDataProtector` через вызов `CreateProtector`, он будет использовать эту ссылку для нескольких вызовов `Protect` и `Unprotect`.</span><span class="sxs-lookup"><span data-stu-id="8287c-114">It's intended that once a component gets a reference to an `IDataProtector` via a call to `CreateProtector`, it will use that reference for multiple calls to `Protect` and `Unprotect`.</span></span>
>
><span data-ttu-id="8287c-115">Вызов `Unprotect` вызовет CryptographicException в случае защищенных полезных данных не может проверить или расшифровать.</span><span class="sxs-lookup"><span data-stu-id="8287c-115">A call to `Unprotect` will throw CryptographicException if the protected payload cannot be verified or deciphered.</span></span> <span data-ttu-id="8287c-116">Некоторые компоненты можно пропускать ошибки во время снятия операций; компонент, который считывает файлы cookie проверки подлинности может обрабатывать эту ошибку и обрабатывать запрос, как если бы она имела cookie не вообще, а не отклонят запрос прямо сейчас.</span><span class="sxs-lookup"><span data-stu-id="8287c-116">Some components may wish to ignore errors during unprotect operations; a component which reads authentication cookies might handle this error and treat the request as if it had no cookie at all rather than fail the request outright.</span></span> <span data-ttu-id="8287c-117">Компоненты, которые хотите такого поведения в частности следует перехватывать CryptographicException вместо подавление всех исключений.</span><span class="sxs-lookup"><span data-stu-id="8287c-117">Components which want this behavior should specifically catch CryptographicException instead of swallowing all exceptions.</span></span>
