---
title: "Защита данных в ASP.NET Core"
author: rick-anderson
description: "В этом документе приводится перечень различных разделов о защите данных в ASP.NET Core."
ms.author: riande
manager: wpickett
ms.date: 10/14/2016
ms.topic: article
ms.technology: aspnet
ms.prod: asp.net-core
uid: security/data-protection/index
ms.openlocfilehash: 7bbd203a67b32032ba2ab82448a5fc9a495b52aa
ms.sourcegitcommit: 3e303620a125325bb9abd4b2d315c106fb8c47fd
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/19/2018
---
# <a name="data-protection-in-aspnet-core-consumer-apis-configuration-extensibility-apis-and-implementation"></a><span data-ttu-id="61121-103">Защита данных в ASP.NET Core: потребительские API, настройка, API расширяемости и реализация</span><span class="sxs-lookup"><span data-stu-id="61121-103">Data Protection in ASP.NET Core: Consumer APIs, configuration, extensibility APIs and implementation</span></span>

* [<span data-ttu-id="61121-104">Общие сведения о защите данных</span><span class="sxs-lookup"><span data-stu-id="61121-104">Introduction to data protection</span></span>](introduction.md)

* [<span data-ttu-id="61121-105">Начало работы с API защиты данных</span><span class="sxs-lookup"><span data-stu-id="61121-105">Get started with the Data Protection APIs</span></span>](using-data-protection.md)

* [<span data-ttu-id="61121-106">Потребительские API</span><span class="sxs-lookup"><span data-stu-id="61121-106">Consumer APIs</span></span>](consumer-apis/index.md)

  * [<span data-ttu-id="61121-107">Обзор потребительских API</span><span class="sxs-lookup"><span data-stu-id="61121-107">Consumer APIs overview</span></span>](consumer-apis/overview.md)

  * [<span data-ttu-id="61121-108">Строки назначений</span><span class="sxs-lookup"><span data-stu-id="61121-108">Purpose strings</span></span>](consumer-apis/purpose-strings.md)

  * [<span data-ttu-id="61121-109">Иерархия назначений и мультитенантность</span><span class="sxs-lookup"><span data-stu-id="61121-109">Purpose hierarchy and multi-tenancy</span></span>](consumer-apis/purpose-strings-multitenancy.md)

  * [<span data-ttu-id="61121-110">Хэширование паролей</span><span class="sxs-lookup"><span data-stu-id="61121-110">Password hashing</span></span>](consumer-apis/password-hashing.md)

  * [<span data-ttu-id="61121-111">Ограничение времени существования защищенных полезных данных</span><span class="sxs-lookup"><span data-stu-id="61121-111">Limiting the lifetime of protected payloads</span></span>](consumer-apis/limited-lifetime-payloads.md)

  * [<span data-ttu-id="61121-112">Снятие защиты полезных данных, ключи которых были отозваны</span><span class="sxs-lookup"><span data-stu-id="61121-112">Unprotecting payloads whose keys have been revoked</span></span>](consumer-apis/dangerous-unprotect.md)

* [<span data-ttu-id="61121-113">Конфигурация</span><span class="sxs-lookup"><span data-stu-id="61121-113">Configuration</span></span>](configuration/index.md)

  * [<span data-ttu-id="61121-114">Настройка защиты данных</span><span class="sxs-lookup"><span data-stu-id="61121-114">Configuring data protection</span></span>](configuration/overview.md)

  * [<span data-ttu-id="61121-115">Параметры по умолчанию</span><span class="sxs-lookup"><span data-stu-id="61121-115">Default settings</span></span>](configuration/default-settings.md)

  * [<span data-ttu-id="61121-116">Политики уровня компьютера</span><span class="sxs-lookup"><span data-stu-id="61121-116">Machine-wide policy</span></span>](configuration/machine-wide-policy.md)

  * [<span data-ttu-id="61121-117">Сценарии, не поддерживающие внедрение зависимостей</span><span class="sxs-lookup"><span data-stu-id="61121-117">Non DI-aware scenarios</span></span>](configuration/non-di-scenarios.md)

* [<span data-ttu-id="61121-118">API расширяемости</span><span class="sxs-lookup"><span data-stu-id="61121-118">Extensibility APIs</span></span>](extensibility/index.md)

  * [<span data-ttu-id="61121-119">Расширяемость базового шифрования</span><span class="sxs-lookup"><span data-stu-id="61121-119">Core cryptography extensibility</span></span>](extensibility/core-crypto.md)

  * [<span data-ttu-id="61121-120">Расширяемость управления ключами</span><span class="sxs-lookup"><span data-stu-id="61121-120">Key management extensibility</span></span>](extensibility/key-management.md)

  * [<span data-ttu-id="61121-121">Различные API</span><span class="sxs-lookup"><span data-stu-id="61121-121">Miscellaneous APIs</span></span>](extensibility/misc-apis.md)

* [<span data-ttu-id="61121-122">Реализация</span><span class="sxs-lookup"><span data-stu-id="61121-122">Implementation</span></span>](implementation/index.md)

  * [<span data-ttu-id="61121-123">Сведения о шифровании с проверкой подлинности</span><span class="sxs-lookup"><span data-stu-id="61121-123">Authenticated encryption details</span></span>](implementation/authenticated-encryption-details.md)

  * [<span data-ttu-id="61121-124">Формирование подключа и шифрование с проверкой подлинности</span><span class="sxs-lookup"><span data-stu-id="61121-124">Subkey derivation and authenticated encryption</span></span>](implementation/subkeyderivation.md)

  * [<span data-ttu-id="61121-125">Заголовки контекста</span><span class="sxs-lookup"><span data-stu-id="61121-125">Context headers</span></span>](implementation/context-headers.md)

  * [<span data-ttu-id="61121-126">Управление ключами</span><span class="sxs-lookup"><span data-stu-id="61121-126">Key management</span></span>](implementation/key-management.md)

  * [<span data-ttu-id="61121-127">Поставщики хранилищ ключей</span><span class="sxs-lookup"><span data-stu-id="61121-127">Key storage providers</span></span>](implementation/key-storage-providers.md)

  * [<span data-ttu-id="61121-128">Шифрование ключей при хранении</span><span class="sxs-lookup"><span data-stu-id="61121-128">Key encryption at rest</span></span>](implementation/key-encryption-at-rest.md)

  * [<span data-ttu-id="61121-129">Неизменность ключа и изменение параметров</span><span class="sxs-lookup"><span data-stu-id="61121-129">Key immutability and changing settings</span></span>](implementation/key-immutability.md)

  * [<span data-ttu-id="61121-130">Формат хранилища ключей</span><span class="sxs-lookup"><span data-stu-id="61121-130">Key storage format</span></span>](implementation/key-storage-format.md)

  * [<span data-ttu-id="61121-131">Временные поставщики защиты данных</span><span class="sxs-lookup"><span data-stu-id="61121-131">Ephemeral data protection providers</span></span>](implementation/key-storage-ephemeral.md)

* [<span data-ttu-id="61121-132">Совместимость</span><span class="sxs-lookup"><span data-stu-id="61121-132">Compatibility</span></span>](compatibility/index.md)

  * [<span data-ttu-id="61121-133">Совместное использование файлов cookie в приложениях</span><span class="sxs-lookup"><span data-stu-id="61121-133">Sharing cookies between apps</span></span>](compatibility/cookie-sharing.md)

  * [<span data-ttu-id="61121-134">Замена <machineKey> в ASP.NET</span><span class="sxs-lookup"><span data-stu-id="61121-134">Replacing <machineKey> in ASP.NET</span></span>](compatibility/replacing-machinekey.md)
