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
# <a name="data-protection-in-aspnet-core-consumer-apis-configuration-extensibility-apis-and-implementation"></a>Защита данных в ASP.NET Core: потребительские API, настройка, API расширяемости и реализация

* [Общие сведения о защите данных](introduction.md)

* [Начало работы с API защиты данных](using-data-protection.md)

* [Потребительские API](consumer-apis/index.md)

  * [Обзор потребительских API](consumer-apis/overview.md)

  * [Строки назначений](consumer-apis/purpose-strings.md)

  * [Иерархия назначений и мультитенантность](consumer-apis/purpose-strings-multitenancy.md)

  * [Хэширование паролей](consumer-apis/password-hashing.md)

  * [Ограничение времени существования защищенных полезных данных](consumer-apis/limited-lifetime-payloads.md)

  * [Снятие защиты полезных данных, ключи которых были отозваны](consumer-apis/dangerous-unprotect.md)

* [Конфигурация](configuration/index.md)

  * [Настройка защиты данных](configuration/overview.md)

  * [Параметры по умолчанию](configuration/default-settings.md)

  * [Политика на уровне компьютера](configuration/machine-wide-policy.md)

  * [Сценарии, не поддерживающие DI](configuration/non-di-scenarios.md)

* [API расширяемости](extensibility/index.md)

  * [Расширяемость базового шифрования](extensibility/core-crypto.md)

  * [Расширяемость управления ключами](extensibility/key-management.md)

  * [Различные API](extensibility/misc-apis.md)

* [Реализация](implementation/index.md)

  * [Сведения о шифровании с проверкой подлинности](implementation/authenticated-encryption-details.md)

  * [Формирование подключа и шифрование с проверкой подлинности](implementation/subkeyderivation.md)

  * [Заголовки контекста](implementation/context-headers.md)

  * [Управление ключами](implementation/key-management.md)

  * [Поставщики хранилищ ключей](implementation/key-storage-providers.md)

  * [Шифрование ключей при хранении](implementation/key-encryption-at-rest.md)

  * [Неизменность ключа и изменение параметров](implementation/key-immutability.md)

  * [Формат хранилища ключей](implementation/key-storage-format.md)

  * [Временные поставщики защиты данных](implementation/key-storage-ephemeral.md)

* [Совместимость](compatibility/index.md)

  * [Совместное использование файлов cookie в приложениях](compatibility/cookie-sharing.md)

  * [Замена <machineKey> в ASP.NET](compatibility/replacing-machinekey.md)
