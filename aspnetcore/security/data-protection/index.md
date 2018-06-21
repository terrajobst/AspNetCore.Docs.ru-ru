---
title: Защита данных в ASP.NET Core
author: rick-anderson
description: В этом документе приводится перечень различных разделов о защите данных в ASP.NET Core.
ms.author: riande
ms.date: 10/14/2016
uid: security/data-protection/index
ms.openlocfilehash: 1c38c587956dd99078fa4386c2f4423d784abc60
ms.sourcegitcommit: a1afd04758e663d7062a5bfa8a0d4dca38f42afc
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 06/20/2018
ms.locfileid: "36277128"
---
# <a name="data-protection-in-aspnet-core"></a><span data-ttu-id="66710-103">Защита данных в ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="66710-103">Data Protection in ASP.NET Core</span></span>

* [<span data-ttu-id="66710-104">Общие сведения о защите данных</span><span class="sxs-lookup"><span data-stu-id="66710-104">Introduction to data protection</span></span>](xref:security/data-protection/introduction)

* [<span data-ttu-id="66710-105">Начало работы с API защиты данных</span><span class="sxs-lookup"><span data-stu-id="66710-105">Get started with the Data Protection APIs</span></span>](xref:security/data-protection/using-data-protection)

* [<span data-ttu-id="66710-106">Потребительские API</span><span class="sxs-lookup"><span data-stu-id="66710-106">Consumer APIs</span></span>](xref:security/data-protection/consumer-apis/index)

  * [<span data-ttu-id="66710-107">Обзор потребительских API</span><span class="sxs-lookup"><span data-stu-id="66710-107">Consumer APIs overview</span></span>](xref:security/data-protection/consumer-apis/overview)

  * [<span data-ttu-id="66710-108">Строки назначений</span><span class="sxs-lookup"><span data-stu-id="66710-108">Purpose strings</span></span>](xref:security/data-protection/consumer-apis/purpose-strings)

  * [<span data-ttu-id="66710-109">Иерархия назначений и мультитенантность</span><span class="sxs-lookup"><span data-stu-id="66710-109">Purpose hierarchy and multi-tenancy</span></span>](xref:security/data-protection/consumer-apis/purpose-strings-multitenancy)

  * [<span data-ttu-id="66710-110">Хэшированные пароли</span><span class="sxs-lookup"><span data-stu-id="66710-110">Hash passwords</span></span>](xref:security/data-protection/consumer-apis/password-hashing)

  * [<span data-ttu-id="66710-111">Ограничение времени существования защищенных полезных данных</span><span class="sxs-lookup"><span data-stu-id="66710-111">Limit the lifetime of protected payloads</span></span>](xref:security/data-protection/consumer-apis/limited-lifetime-payloads)

  * [<span data-ttu-id="66710-112">Снятие защиты полезных данных, ключи которых были отозваны</span><span class="sxs-lookup"><span data-stu-id="66710-112">Unprotect payloads whose keys have been revoked</span></span>](xref:security/data-protection/consumer-apis/dangerous-unprotect)

* [<span data-ttu-id="66710-113">Конфигурация</span><span class="sxs-lookup"><span data-stu-id="66710-113">Configuration</span></span>](xref:security/data-protection/configuration/index)

  * [<span data-ttu-id="66710-114">Настройка защиты данных в ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="66710-114">Configure ASP.NET Core Data Protection</span></span>](xref:security/data-protection/configuration/overview)

  * [<span data-ttu-id="66710-115">Параметры по умолчанию</span><span class="sxs-lookup"><span data-stu-id="66710-115">Default settings</span></span>](xref:security/data-protection/configuration/default-settings)

  * [<span data-ttu-id="66710-116">Политики уровня компьютера</span><span class="sxs-lookup"><span data-stu-id="66710-116">Machine-wide policy</span></span>](xref:security/data-protection/configuration/machine-wide-policy)

  * [<span data-ttu-id="66710-117">Сценарии, не поддерживающие внедрение зависимостей</span><span class="sxs-lookup"><span data-stu-id="66710-117">Non DI-aware scenarios</span></span>](xref:security/data-protection/configuration/non-di-scenarios)

* [<span data-ttu-id="66710-118">API расширяемости</span><span class="sxs-lookup"><span data-stu-id="66710-118">Extensibility APIs</span></span>](xref:security/data-protection/extensibility/index)

  * [<span data-ttu-id="66710-119">Расширяемость базового шифрования</span><span class="sxs-lookup"><span data-stu-id="66710-119">Core cryptography extensibility</span></span>](xref:security/data-protection/extensibility/core-crypto)

  * [<span data-ttu-id="66710-120">Расширяемость управления ключами</span><span class="sxs-lookup"><span data-stu-id="66710-120">Key management extensibility</span></span>](xref:security/data-protection/extensibility/key-management)

  * [<span data-ttu-id="66710-121">Различные API</span><span class="sxs-lookup"><span data-stu-id="66710-121">Miscellaneous APIs</span></span>](xref:security/data-protection/extensibility/misc-apis)

* [<span data-ttu-id="66710-122">Реализация</span><span class="sxs-lookup"><span data-stu-id="66710-122">Implementation</span></span>](xref:security/data-protection/implementation/index)

  * [<span data-ttu-id="66710-123">Сведения о шифровании с проверкой подлинности</span><span class="sxs-lookup"><span data-stu-id="66710-123">Authenticated encryption details</span></span>](xref:security/data-protection/implementation/authenticated-encryption-details)

  * [<span data-ttu-id="66710-124">Формирование подключа и шифрование с проверкой подлинности</span><span class="sxs-lookup"><span data-stu-id="66710-124">Subkey derivation and authenticated encryption</span></span>](xref:security/data-protection/implementation/subkeyderivation)

  * [<span data-ttu-id="66710-125">Заголовки контекста</span><span class="sxs-lookup"><span data-stu-id="66710-125">Context headers</span></span>](xref:security/data-protection/implementation/context-headers)

  * [<span data-ttu-id="66710-126">Управление ключами</span><span class="sxs-lookup"><span data-stu-id="66710-126">Key management</span></span>](xref:security/data-protection/implementation/key-management)

  * [<span data-ttu-id="66710-127">Поставщики хранилищ ключей</span><span class="sxs-lookup"><span data-stu-id="66710-127">Key storage providers</span></span>](xref:security/data-protection/implementation/key-storage-providers)

  * [<span data-ttu-id="66710-128">Шифрование ключей при хранении</span><span class="sxs-lookup"><span data-stu-id="66710-128">Key encryption at rest</span></span>](xref:security/data-protection/implementation/key-encryption-at-rest)

  * [<span data-ttu-id="66710-129">Неизменность и параметры ключа</span><span class="sxs-lookup"><span data-stu-id="66710-129">Key immutability and settings</span></span>](xref:security/data-protection/implementation/key-immutability)

  * [<span data-ttu-id="66710-130">Формат хранилища ключей</span><span class="sxs-lookup"><span data-stu-id="66710-130">Key storage format</span></span>](xref:security/data-protection/implementation/key-storage-format)

  * [<span data-ttu-id="66710-131">Временные поставщики защиты данных</span><span class="sxs-lookup"><span data-stu-id="66710-131">Ephemeral data protection providers</span></span>](xref:security/data-protection/implementation/key-storage-ephemeral)

* [<span data-ttu-id="66710-132">Совместимость</span><span class="sxs-lookup"><span data-stu-id="66710-132">Compatibility</span></span>](xref:security/data-protection/compatibility/index)

  * [<span data-ttu-id="66710-133">Замена ASP.NET <machineKey> в ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="66710-133">Replacing ASP.NET <machineKey> in ASP.NET Core</span></span>](xref:security/data-protection/compatibility/replacing-machinekey)
