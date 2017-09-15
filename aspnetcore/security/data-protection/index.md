---
title: "Защита данных в ASP.NET Core"
author: rick-anderson
description: 
keywords: ASP.NET Core
ms.author: riande
manager: wpickett
ms.date: 10/14/2016
ms.topic: article
ms.assetid: 1f402da8-1052-4970-9835-9f9f16a02dbc
ms.technology: aspnet
ms.prod: asp.net-core
uid: security/data-protection/index
ms.openlocfilehash: 39f2ca96f8542de033274ea957b5c7736948c981
ms.sourcegitcommit: 0b6c8e6d81d2b3c161cd375036eecbace46a9707
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 08/11/2017
---
# <a name="data-protection-in-aspnet-core-consumer-apis-configuration-extensibility-apis-and-implementation"></a><span data-ttu-id="9f52f-103">Защита данных в ASP.NET Core: потребительские API, настройка, API расширяемости и реализация</span><span class="sxs-lookup"><span data-stu-id="9f52f-103">Data Protection in ASP.NET Core: Consumer APIs, configuration, extensibility APIs and implementation</span></span>

* [<span data-ttu-id="9f52f-104">Общие сведения о защите данных</span><span class="sxs-lookup"><span data-stu-id="9f52f-104">Introduction to Data Protection</span></span>](introduction.md)

* [<span data-ttu-id="9f52f-105">Начало работы с API защиты данных</span><span class="sxs-lookup"><span data-stu-id="9f52f-105">Getting Started with the Data Protection APIs</span></span>](using-data-protection.md)

* [<span data-ttu-id="9f52f-106">Потребительские API</span><span class="sxs-lookup"><span data-stu-id="9f52f-106">Consumer APIs</span></span>](consumer-apis/index.md)

  * [<span data-ttu-id="9f52f-107">Обзор потребительских API</span><span class="sxs-lookup"><span data-stu-id="9f52f-107">Consumer APIs Overview</span></span>](consumer-apis/overview.md)

  * [<span data-ttu-id="9f52f-108">Строки назначений</span><span class="sxs-lookup"><span data-stu-id="9f52f-108">Purpose Strings</span></span>](consumer-apis/purpose-strings.md)

  * [<span data-ttu-id="9f52f-109">Иерархия назначений и мультитенантность</span><span class="sxs-lookup"><span data-stu-id="9f52f-109">Purpose hierarchy and multi-tenancy</span></span>](consumer-apis/purpose-strings-multitenancy.md)

  * [<span data-ttu-id="9f52f-110">Хэширование паролей</span><span class="sxs-lookup"><span data-stu-id="9f52f-110">Password Hashing</span></span>](consumer-apis/password-hashing.md)

  * [<span data-ttu-id="9f52f-111">Ограничение времени существования защищенных полезных данных</span><span class="sxs-lookup"><span data-stu-id="9f52f-111">Limiting the lifetime of protected payloads</span></span>](consumer-apis/limited-lifetime-payloads.md)

  * [<span data-ttu-id="9f52f-112">Снятие защиты полезных данных, ключи которых были отозваны</span><span class="sxs-lookup"><span data-stu-id="9f52f-112">Unprotecting payloads whose keys have been revoked</span></span>](consumer-apis/dangerous-unprotect.md)

* [<span data-ttu-id="9f52f-113">Конфигурация</span><span class="sxs-lookup"><span data-stu-id="9f52f-113">Configuration</span></span>](configuration/index.md)

  * [<span data-ttu-id="9f52f-114">Настройка защиты данных</span><span class="sxs-lookup"><span data-stu-id="9f52f-114">Configuring Data Protection</span></span>](configuration/overview.md)

  * [<span data-ttu-id="9f52f-115">Параметры по умолчанию</span><span class="sxs-lookup"><span data-stu-id="9f52f-115">Default Settings</span></span>](configuration/default-settings.md)

  * [<span data-ttu-id="9f52f-116">Политика на уровне компьютера</span><span class="sxs-lookup"><span data-stu-id="9f52f-116">Machine Wide Policy</span></span>](configuration/machine-wide-policy.md)

  * [<span data-ttu-id="9f52f-117">Сценарии, не поддерживающие DI</span><span class="sxs-lookup"><span data-stu-id="9f52f-117">Non DI Aware Scenarios</span></span>](configuration/non-di-scenarios.md)

* [<span data-ttu-id="9f52f-118">API расширяемости</span><span class="sxs-lookup"><span data-stu-id="9f52f-118">Extensibility APIs</span></span>](extensibility/index.md)

  * [<span data-ttu-id="9f52f-119">Расширяемость базового шифрования</span><span class="sxs-lookup"><span data-stu-id="9f52f-119">Core cryptography extensibility</span></span>](extensibility/core-crypto.md)

  * [<span data-ttu-id="9f52f-120">Расширяемость управления ключами</span><span class="sxs-lookup"><span data-stu-id="9f52f-120">Key management extensibility</span></span>](extensibility/key-management.md)

  * [<span data-ttu-id="9f52f-121">Различные API</span><span class="sxs-lookup"><span data-stu-id="9f52f-121">Miscellaneous APIs</span></span>](extensibility/misc-apis.md)

* [<span data-ttu-id="9f52f-122">Реализация</span><span class="sxs-lookup"><span data-stu-id="9f52f-122">Implementation</span></span>](implementation/index.md)

  * [<span data-ttu-id="9f52f-123">Сведения о шифровании с проверкой подлинности.</span><span class="sxs-lookup"><span data-stu-id="9f52f-123">Authenticated encryption details.</span></span>](implementation/authenticated-encryption-details.md)

  * [<span data-ttu-id="9f52f-124">Формирование подключа и шифрование с проверкой подлинности</span><span class="sxs-lookup"><span data-stu-id="9f52f-124">Subkey Derivation and Authenticated Encryption</span></span>](implementation/subkeyderivation.md)

  * [<span data-ttu-id="9f52f-125">Заголовки контекста</span><span class="sxs-lookup"><span data-stu-id="9f52f-125">Context headers</span></span>](implementation/context-headers.md)

  * [<span data-ttu-id="9f52f-126">Управление ключами</span><span class="sxs-lookup"><span data-stu-id="9f52f-126">Key Management</span></span>](implementation/key-management.md)

  * [<span data-ttu-id="9f52f-127">Поставщики хранилищ ключей</span><span class="sxs-lookup"><span data-stu-id="9f52f-127">Key Storage Providers</span></span>](implementation/key-storage-providers.md)

  * [<span data-ttu-id="9f52f-128">Шифрование ключей при хранении</span><span class="sxs-lookup"><span data-stu-id="9f52f-128">Key Encryption At Rest</span></span>](implementation/key-encryption-at-rest.md)

  * [<span data-ttu-id="9f52f-129">Неизменность ключа и изменение параметров</span><span class="sxs-lookup"><span data-stu-id="9f52f-129">Key Immutability and Changing Settings</span></span>](implementation/key-immutability.md)

  * [<span data-ttu-id="9f52f-130">Формат хранилища ключей</span><span class="sxs-lookup"><span data-stu-id="9f52f-130">Key Storage Format</span></span>](implementation/key-storage-format.md)

  * [<span data-ttu-id="9f52f-131">Временные поставщики защиты данных</span><span class="sxs-lookup"><span data-stu-id="9f52f-131">Ephemeral data protection providers</span></span>](implementation/key-storage-ephemeral.md)

* [<span data-ttu-id="9f52f-132">Совместимость</span><span class="sxs-lookup"><span data-stu-id="9f52f-132">Compatibility</span></span>](compatibility/index.md)

  * [<span data-ttu-id="9f52f-133">Совместное использование файлов cookie в приложениях</span><span class="sxs-lookup"><span data-stu-id="9f52f-133">Sharing cookies between applications</span></span>](compatibility/cookie-sharing.md)

  * [<span data-ttu-id="9f52f-134">Замена <machineKey> в ASP.NET</span><span class="sxs-lookup"><span data-stu-id="9f52f-134">Replacing <machineKey> in ASP.NET</span></span>](compatibility/replacing-machinekey.md)
