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
ms.openlocfilehash: 8b42e65bb6121355120a6f4fbe8cd4d1fea153de
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 11/10/2017
---
# <a name="data-protection-in-aspnet-core-consumer-apis-configuration-extensibility-apis-and-implementation"></a><span data-ttu-id="a914f-104">Защита данных в ASP.NET Core: потребительские API, настройка, API расширяемости и реализация</span><span class="sxs-lookup"><span data-stu-id="a914f-104">Data Protection in ASP.NET Core: Consumer APIs, configuration, extensibility APIs and implementation</span></span>

* [<span data-ttu-id="a914f-105">Общие сведения о защите данных</span><span class="sxs-lookup"><span data-stu-id="a914f-105">Introduction to Data Protection</span></span>](introduction.md)

* [<span data-ttu-id="a914f-106">Начало работы с API защиты данных</span><span class="sxs-lookup"><span data-stu-id="a914f-106">Getting Started with the Data Protection APIs</span></span>](using-data-protection.md)

* [<span data-ttu-id="a914f-107">Потребительские API</span><span class="sxs-lookup"><span data-stu-id="a914f-107">Consumer APIs</span></span>](consumer-apis/index.md)

  * [<span data-ttu-id="a914f-108">Обзор потребительских API</span><span class="sxs-lookup"><span data-stu-id="a914f-108">Consumer APIs Overview</span></span>](consumer-apis/overview.md)

  * [<span data-ttu-id="a914f-109">Строки назначений</span><span class="sxs-lookup"><span data-stu-id="a914f-109">Purpose Strings</span></span>](consumer-apis/purpose-strings.md)

  * [<span data-ttu-id="a914f-110">Иерархия назначений и мультитенантность</span><span class="sxs-lookup"><span data-stu-id="a914f-110">Purpose hierarchy and multi-tenancy</span></span>](consumer-apis/purpose-strings-multitenancy.md)

  * [<span data-ttu-id="a914f-111">Хэширование паролей</span><span class="sxs-lookup"><span data-stu-id="a914f-111">Password Hashing</span></span>](consumer-apis/password-hashing.md)

  * [<span data-ttu-id="a914f-112">Ограничение времени существования защищенных полезных данных</span><span class="sxs-lookup"><span data-stu-id="a914f-112">Limiting the lifetime of protected payloads</span></span>](consumer-apis/limited-lifetime-payloads.md)

  * [<span data-ttu-id="a914f-113">Снятие защиты полезных данных, ключи которых были отозваны</span><span class="sxs-lookup"><span data-stu-id="a914f-113">Unprotecting payloads whose keys have been revoked</span></span>](consumer-apis/dangerous-unprotect.md)

* [<span data-ttu-id="a914f-114">Конфигурация</span><span class="sxs-lookup"><span data-stu-id="a914f-114">Configuration</span></span>](configuration/index.md)

  * [<span data-ttu-id="a914f-115">Настройка защиты данных</span><span class="sxs-lookup"><span data-stu-id="a914f-115">Configuring Data Protection</span></span>](configuration/overview.md)

  * [<span data-ttu-id="a914f-116">Параметры по умолчанию</span><span class="sxs-lookup"><span data-stu-id="a914f-116">Default Settings</span></span>](configuration/default-settings.md)

  * [<span data-ttu-id="a914f-117">Политика на уровне компьютера</span><span class="sxs-lookup"><span data-stu-id="a914f-117">Machine Wide Policy</span></span>](configuration/machine-wide-policy.md)

  * [<span data-ttu-id="a914f-118">Сценарии, не поддерживающие DI</span><span class="sxs-lookup"><span data-stu-id="a914f-118">Non DI Aware Scenarios</span></span>](configuration/non-di-scenarios.md)

* [<span data-ttu-id="a914f-119">API расширяемости</span><span class="sxs-lookup"><span data-stu-id="a914f-119">Extensibility APIs</span></span>](extensibility/index.md)

  * [<span data-ttu-id="a914f-120">Расширяемость базового шифрования</span><span class="sxs-lookup"><span data-stu-id="a914f-120">Core cryptography extensibility</span></span>](extensibility/core-crypto.md)

  * [<span data-ttu-id="a914f-121">Расширяемость управления ключами</span><span class="sxs-lookup"><span data-stu-id="a914f-121">Key management extensibility</span></span>](extensibility/key-management.md)

  * [<span data-ttu-id="a914f-122">Различные API</span><span class="sxs-lookup"><span data-stu-id="a914f-122">Miscellaneous APIs</span></span>](extensibility/misc-apis.md)

* [<span data-ttu-id="a914f-123">Реализация</span><span class="sxs-lookup"><span data-stu-id="a914f-123">Implementation</span></span>](implementation/index.md)

  * [<span data-ttu-id="a914f-124">Сведения о шифровании с проверкой подлинности</span><span class="sxs-lookup"><span data-stu-id="a914f-124">Authenticated encryption details</span></span>](implementation/authenticated-encryption-details.md)

  * [<span data-ttu-id="a914f-125">Формирование подключа и шифрование с проверкой подлинности</span><span class="sxs-lookup"><span data-stu-id="a914f-125">Subkey Derivation and Authenticated Encryption</span></span>](implementation/subkeyderivation.md)

  * [<span data-ttu-id="a914f-126">Заголовки контекста</span><span class="sxs-lookup"><span data-stu-id="a914f-126">Context headers</span></span>](implementation/context-headers.md)

  * [<span data-ttu-id="a914f-127">Управление ключами</span><span class="sxs-lookup"><span data-stu-id="a914f-127">Key Management</span></span>](implementation/key-management.md)

  * [<span data-ttu-id="a914f-128">Поставщики хранилищ ключей</span><span class="sxs-lookup"><span data-stu-id="a914f-128">Key Storage Providers</span></span>](implementation/key-storage-providers.md)

  * [<span data-ttu-id="a914f-129">Шифрование ключей при хранении</span><span class="sxs-lookup"><span data-stu-id="a914f-129">Key Encryption At Rest</span></span>](implementation/key-encryption-at-rest.md)

  * [<span data-ttu-id="a914f-130">Неизменность ключа и изменение параметров</span><span class="sxs-lookup"><span data-stu-id="a914f-130">Key Immutability and Changing Settings</span></span>](implementation/key-immutability.md)

  * [<span data-ttu-id="a914f-131">Формат хранилища ключей</span><span class="sxs-lookup"><span data-stu-id="a914f-131">Key Storage Format</span></span>](implementation/key-storage-format.md)

  * [<span data-ttu-id="a914f-132">Временные поставщики защиты данных</span><span class="sxs-lookup"><span data-stu-id="a914f-132">Ephemeral data protection providers</span></span>](implementation/key-storage-ephemeral.md)

* [<span data-ttu-id="a914f-133">Совместимость</span><span class="sxs-lookup"><span data-stu-id="a914f-133">Compatibility</span></span>](compatibility/index.md)

  * [<span data-ttu-id="a914f-134">Совместное использование файлов cookie в приложениях</span><span class="sxs-lookup"><span data-stu-id="a914f-134">Sharing cookies between applications</span></span>](compatibility/cookie-sharing.md)

  * [<span data-ttu-id="a914f-135">Замена <machineKey> в ASP.NET</span><span class="sxs-lookup"><span data-stu-id="a914f-135">Replacing <machineKey> in ASP.NET</span></span>](compatibility/replacing-machinekey.md)
