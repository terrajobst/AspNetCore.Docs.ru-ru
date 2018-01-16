---
title: "Защита данных в ASP.NET Core"
author: rick-anderson
description: "В этом документе приводится перечень различных разделов о защите данных в ASP.NET Core."
keywords: "ASP.NET Core, защита данных"
ms.author: riande
manager: wpickett
ms.date: 10/14/2016
ms.topic: article
ms.assetid: 1f402da8-1052-4970-9835-9f9f16a02dbc
ms.technology: aspnet
ms.prod: asp.net-core
uid: security/data-protection/index
ms.openlocfilehash: cbf18c6ec867fefec22980f3e3493562594ef72d
ms.sourcegitcommit: 12e5194936b7e820efc5505a2d5d4f84e88eb5ef
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/11/2018
---
# <a name="data-protection-in-aspnet-core-consumer-apis-configuration-extensibility-apis-and-implementation"></a><span data-ttu-id="2ea56-104">Защита данных в ASP.NET Core: потребительские API, настройка, API расширяемости и реализация</span><span class="sxs-lookup"><span data-stu-id="2ea56-104">Data Protection in ASP.NET Core: Consumer APIs, configuration, extensibility APIs and implementation</span></span>

* [<span data-ttu-id="2ea56-105">Общие сведения о защите данных</span><span class="sxs-lookup"><span data-stu-id="2ea56-105">Introduction to data protection</span></span>](introduction.md)

* [<span data-ttu-id="2ea56-106">Начало работы с API защиты данных</span><span class="sxs-lookup"><span data-stu-id="2ea56-106">Get started with the Data Protection APIs</span></span>](using-data-protection.md)

* [<span data-ttu-id="2ea56-107">Потребительские API</span><span class="sxs-lookup"><span data-stu-id="2ea56-107">Consumer APIs</span></span>](consumer-apis/index.md)

  * [<span data-ttu-id="2ea56-108">Обзор потребительских API</span><span class="sxs-lookup"><span data-stu-id="2ea56-108">Consumer APIs overview</span></span>](consumer-apis/overview.md)

  * [<span data-ttu-id="2ea56-109">Строки назначений</span><span class="sxs-lookup"><span data-stu-id="2ea56-109">Purpose strings</span></span>](consumer-apis/purpose-strings.md)

  * [<span data-ttu-id="2ea56-110">Иерархия назначений и мультитенантность</span><span class="sxs-lookup"><span data-stu-id="2ea56-110">Purpose hierarchy and multi-tenancy</span></span>](consumer-apis/purpose-strings-multitenancy.md)

  * [<span data-ttu-id="2ea56-111">Хэширование паролей</span><span class="sxs-lookup"><span data-stu-id="2ea56-111">Password hashing</span></span>](consumer-apis/password-hashing.md)

  * [<span data-ttu-id="2ea56-112">Ограничение времени существования защищенных полезных данных</span><span class="sxs-lookup"><span data-stu-id="2ea56-112">Limiting the lifetime of protected payloads</span></span>](consumer-apis/limited-lifetime-payloads.md)

  * [<span data-ttu-id="2ea56-113">Снятие защиты полезных данных, ключи которых были отозваны</span><span class="sxs-lookup"><span data-stu-id="2ea56-113">Unprotecting payloads whose keys have been revoked</span></span>](consumer-apis/dangerous-unprotect.md)

* [<span data-ttu-id="2ea56-114">Конфигурация</span><span class="sxs-lookup"><span data-stu-id="2ea56-114">Configuration</span></span>](configuration/index.md)

  * [<span data-ttu-id="2ea56-115">Настройка защиты данных</span><span class="sxs-lookup"><span data-stu-id="2ea56-115">Configuring data protection</span></span>](configuration/overview.md)

  * [<span data-ttu-id="2ea56-116">Параметры по умолчанию</span><span class="sxs-lookup"><span data-stu-id="2ea56-116">Default settings</span></span>](configuration/default-settings.md)

  * [<span data-ttu-id="2ea56-117">Политики уровня компьютера</span><span class="sxs-lookup"><span data-stu-id="2ea56-117">Machine-wide policy</span></span>](configuration/machine-wide-policy.md)

  * [<span data-ttu-id="2ea56-118">Сценарии, не поддерживающие внедрение зависимостей</span><span class="sxs-lookup"><span data-stu-id="2ea56-118">Non DI-aware scenarios</span></span>](configuration/non-di-scenarios.md)

* [<span data-ttu-id="2ea56-119">API расширяемости</span><span class="sxs-lookup"><span data-stu-id="2ea56-119">Extensibility APIs</span></span>](extensibility/index.md)

  * [<span data-ttu-id="2ea56-120">Расширяемость базового шифрования</span><span class="sxs-lookup"><span data-stu-id="2ea56-120">Core cryptography extensibility</span></span>](extensibility/core-crypto.md)

  * [<span data-ttu-id="2ea56-121">Расширяемость управления ключами</span><span class="sxs-lookup"><span data-stu-id="2ea56-121">Key management extensibility</span></span>](extensibility/key-management.md)

  * [<span data-ttu-id="2ea56-122">Различные API</span><span class="sxs-lookup"><span data-stu-id="2ea56-122">Miscellaneous APIs</span></span>](extensibility/misc-apis.md)

* [<span data-ttu-id="2ea56-123">Реализация</span><span class="sxs-lookup"><span data-stu-id="2ea56-123">Implementation</span></span>](implementation/index.md)

  * [<span data-ttu-id="2ea56-124">Сведения о шифровании с проверкой подлинности</span><span class="sxs-lookup"><span data-stu-id="2ea56-124">Authenticated encryption details</span></span>](implementation/authenticated-encryption-details.md)

  * [<span data-ttu-id="2ea56-125">Формирование подключа и шифрование с проверкой подлинности</span><span class="sxs-lookup"><span data-stu-id="2ea56-125">Subkey derivation and authenticated encryption</span></span>](implementation/subkeyderivation.md)

  * [<span data-ttu-id="2ea56-126">Заголовки контекста</span><span class="sxs-lookup"><span data-stu-id="2ea56-126">Context headers</span></span>](implementation/context-headers.md)

  * [<span data-ttu-id="2ea56-127">Управление ключами</span><span class="sxs-lookup"><span data-stu-id="2ea56-127">Key management</span></span>](implementation/key-management.md)

  * [<span data-ttu-id="2ea56-128">Поставщики хранилищ ключей</span><span class="sxs-lookup"><span data-stu-id="2ea56-128">Key storage providers</span></span>](implementation/key-storage-providers.md)

  * [<span data-ttu-id="2ea56-129">Шифрование ключей при хранении</span><span class="sxs-lookup"><span data-stu-id="2ea56-129">Key encryption at rest</span></span>](implementation/key-encryption-at-rest.md)

  * [<span data-ttu-id="2ea56-130">Неизменность ключа и изменение параметров</span><span class="sxs-lookup"><span data-stu-id="2ea56-130">Key immutability and changing settings</span></span>](implementation/key-immutability.md)

  * [<span data-ttu-id="2ea56-131">Формат хранилища ключей</span><span class="sxs-lookup"><span data-stu-id="2ea56-131">Key storage format</span></span>](implementation/key-storage-format.md)

  * [<span data-ttu-id="2ea56-132">Временные поставщики защиты данных</span><span class="sxs-lookup"><span data-stu-id="2ea56-132">Ephemeral data protection providers</span></span>](implementation/key-storage-ephemeral.md)

* [<span data-ttu-id="2ea56-133">Совместимость</span><span class="sxs-lookup"><span data-stu-id="2ea56-133">Compatibility</span></span>](compatibility/index.md)

  * [<span data-ttu-id="2ea56-134">Совместное использование файлов cookie в приложениях</span><span class="sxs-lookup"><span data-stu-id="2ea56-134">Sharing cookies between apps</span></span>](compatibility/cookie-sharing.md)

  * [<span data-ttu-id="2ea56-135">Замена <machineKey> в ASP.NET</span><span class="sxs-lookup"><span data-stu-id="2ea56-135">Replacing <machineKey> in ASP.NET</span></span>](compatibility/replacing-machinekey.md)
