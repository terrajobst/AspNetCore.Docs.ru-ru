---
title: "Безопасность"
author: rick-anderson
description: 
keywords: ASP.NET Core
ms.author: riande
manager: wpickett
ms.date: 10/14/2016
ms.topic: article
ms.assetid: a8fb7eb7-e0e5-4394-84f3-1f1dbe012345
ms.technology: aspnet
ms.prod: asp.net-core
uid: security/index
ms.openlocfilehash: e8045b8804d1e7915cd81d697d43a173e33a7119
ms.sourcegitcommit: 9cdbfd0d670d70b9c354216aabee260c52dad5ee
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 09/12/2017
---
# <a name="security"></a><span data-ttu-id="9dd95-103">Безопасность</span><span class="sxs-lookup"><span data-stu-id="9dd95-103">Security</span></span>

*   [<span data-ttu-id="9dd95-104">Проверка подлинности</span><span class="sxs-lookup"><span data-stu-id="9dd95-104">Authentication</span></span>](authentication/index.md)
    *   [<span data-ttu-id="9dd95-105">Общие сведения об Identity</span><span class="sxs-lookup"><span data-stu-id="9dd95-105">Introduction to Identity</span></span>](authentication/identity.md)
    *   [<span data-ttu-id="9dd95-106">Включение проверки подлинности с помощью Facebook, Google и других внешних поставщиков</span><span class="sxs-lookup"><span data-stu-id="9dd95-106">Enabling authentication using Facebook, Google and other external providers</span></span>](authentication/social/index.md)
    * [<span data-ttu-id="9dd95-107">Настройка проверки подлинности Windows</span><span class="sxs-lookup"><span data-stu-id="9dd95-107">Configure Windows Authentication</span></span>](authentication/windowsauth.md)
    *   [<span data-ttu-id="9dd95-108">Подтверждение учетной записи и восстановление пароля</span><span class="sxs-lookup"><span data-stu-id="9dd95-108">Account Confirmation and Password Recovery</span></span>](authentication/accconfirm.md)
    *   [<span data-ttu-id="9dd95-109">Двухфакторная проверка подлинности с помощью SMS</span><span class="sxs-lookup"><span data-stu-id="9dd95-109">Two-factor authentication with SMS</span></span>](authentication/2fa.md) 
    *   [<span data-ttu-id="9dd95-110">Использование проверки подлинности с помощью файлов cookie без ASP.NET Core Identity</span><span class="sxs-lookup"><span data-stu-id="9dd95-110">Using Cookie Authentication without ASP.NET Core Identity</span></span>](authentication/cookie.md)
    *   [<span data-ttu-id="9dd95-111">Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="9dd95-111">Azure Active Directory</span></span>](authentication/azure-active-directory/index.md)
        *   [<span data-ttu-id="9dd95-112">Интеграция Azure AD в веб-приложение ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="9dd95-112">Integrating Azure AD Into an ASP.NET Core Web App</span></span>](https://azure.microsoft.com/documentation/samples/active-directory-dotnet-webapp-openidconnect-aspnetcore/)
        *   [<span data-ttu-id="9dd95-113">Вызов веб-API ASP.NET Core из приложения WPF с помощью Azure AD</span><span class="sxs-lookup"><span data-stu-id="9dd95-113">Calling a ASP.NET Core Web API From a WPF Application Using Azure AD</span></span>](https://azure.microsoft.com/documentation/samples/active-directory-dotnet-native-aspnetcore/)
        *   [<span data-ttu-id="9dd95-114">Вызов веб-API в веб-приложении ASP.NET Core с помощью Azure AD</span><span class="sxs-lookup"><span data-stu-id="9dd95-114">Calling a Web API in an ASP.NET Core Web Application Using Azure AD</span></span>](https://azure.microsoft.com/documentation/samples/active-directory-dotnet-webapp-webapi-openidconnect-aspnetcore/)
        *   [<span data-ttu-id="9dd95-115">ASP.NET Core с Azure AD B2C</span><span class="sxs-lookup"><span data-stu-id="9dd95-115">An ASP.NET Core web app with Azure AD B2C</span></span>](https://azure.microsoft.com/resources/samples/active-directory-b2c-dotnetcore-webapp/)
    *   [<span data-ttu-id="9dd95-116">Защита приложений ASP.NET Core с помощью IdentityServer4</span><span class="sxs-lookup"><span data-stu-id="9dd95-116">Securing ASP.NET Core apps with IdentityServer4</span></span>](https://identityserver4.readthedocs.io)
*   [<span data-ttu-id="9dd95-117">Авторизация</span><span class="sxs-lookup"><span data-stu-id="9dd95-117">Authorization</span></span>](authorization/index.md)
    *   [<span data-ttu-id="9dd95-118">Введение</span><span class="sxs-lookup"><span data-stu-id="9dd95-118">Introduction</span></span>](authorization/introduction.md)
    *   [<span data-ttu-id="9dd95-119">Создание приложения с защитой данных пользователя с помощью авторизации</span><span class="sxs-lookup"><span data-stu-id="9dd95-119">Create an app with user data protected by authorization</span></span>](xref:security/authorization/secure-data)
    *   [<span data-ttu-id="9dd95-120">Простая авторизация</span><span class="sxs-lookup"><span data-stu-id="9dd95-120">Simple Authorization</span></span>](authorization/simple.md)
    *   [<span data-ttu-id="9dd95-121">Авторизация на основе ролей</span><span class="sxs-lookup"><span data-stu-id="9dd95-121">Role based Authorization</span></span>](authorization/roles.md)
    *   [<span data-ttu-id="9dd95-122">Авторизация на основе утверждений</span><span class="sxs-lookup"><span data-stu-id="9dd95-122">Claims-Based Authorization</span></span>](authorization/claims.md)
    *   [<span data-ttu-id="9dd95-123">Пользовательская авторизация на основе политик</span><span class="sxs-lookup"><span data-stu-id="9dd95-123">Custom Policy-Based Authorization</span></span>](authorization/policies.md)
    *   [<span data-ttu-id="9dd95-124">Внедрение зависимостей в обработчики требований</span><span class="sxs-lookup"><span data-stu-id="9dd95-124">Dependency Injection in requirement handlers</span></span>](authorization/dependencyinjection.md)
    *   [<span data-ttu-id="9dd95-125">Авторизация на основе ресурсов</span><span class="sxs-lookup"><span data-stu-id="9dd95-125">Resource Based Authorization</span></span>](authorization/resourcebased.md)
    *   [<span data-ttu-id="9dd95-126">Авторизация на основе представлений</span><span class="sxs-lookup"><span data-stu-id="9dd95-126">View Based Authorization</span></span>](authorization/views.md)
    *   [<span data-ttu-id="9dd95-127">Ограничение идентификаторов по схеме</span><span class="sxs-lookup"><span data-stu-id="9dd95-127">Limiting identity by scheme</span></span>](authorization/limitingidentitybyscheme.md)
*   [<span data-ttu-id="9dd95-128">Защита данных</span><span class="sxs-lookup"><span data-stu-id="9dd95-128">Data Protection</span></span>](data-protection/index.md)
    *   [<span data-ttu-id="9dd95-129">Общие сведения о защите данных</span><span class="sxs-lookup"><span data-stu-id="9dd95-129">Introduction to Data Protection</span></span>](data-protection/introduction.md)
    *   [<span data-ttu-id="9dd95-130">Начало работы с API защиты данных</span><span class="sxs-lookup"><span data-stu-id="9dd95-130">Getting Started with the Data Protection APIs</span></span>](data-protection/using-data-protection.md)
    *   [<span data-ttu-id="9dd95-131">Потребительские API</span><span class="sxs-lookup"><span data-stu-id="9dd95-131">Consumer APIs</span></span>](data-protection/consumer-apis/index.md)
        *   [<span data-ttu-id="9dd95-132">Обзор потребительских API</span><span class="sxs-lookup"><span data-stu-id="9dd95-132">Consumer APIs Overview</span></span>](data-protection/consumer-apis/overview.md)
        *   [<span data-ttu-id="9dd95-133">Строки назначений</span><span class="sxs-lookup"><span data-stu-id="9dd95-133">Purpose Strings</span></span>](data-protection/consumer-apis/purpose-strings.md)
        *   [<span data-ttu-id="9dd95-134">Иерархия назначений и мультитенантность</span><span class="sxs-lookup"><span data-stu-id="9dd95-134">Purpose hierarchy and multi-tenancy</span></span>](data-protection/consumer-apis/purpose-strings-multitenancy.md)
        *   [<span data-ttu-id="9dd95-135">Хэширование паролей</span><span class="sxs-lookup"><span data-stu-id="9dd95-135">Password Hashing</span></span>](data-protection/consumer-apis/password-hashing.md)
        *   [<span data-ttu-id="9dd95-136">Ограничение времени существования защищенных полезных данных</span><span class="sxs-lookup"><span data-stu-id="9dd95-136">Limiting the lifetime of protected payloads</span></span>](data-protection/consumer-apis/limited-lifetime-payloads.md)
        *   [<span data-ttu-id="9dd95-137">Снятие защиты полезных данных, ключи которых были отозваны</span><span class="sxs-lookup"><span data-stu-id="9dd95-137">Unprotecting payloads whose keys have been revoked</span></span>](data-protection/consumer-apis/dangerous-unprotect.md)
    *   [<span data-ttu-id="9dd95-138">Конфигурация</span><span class="sxs-lookup"><span data-stu-id="9dd95-138">Configuration</span></span>](data-protection/configuration/index.md)
        *   [<span data-ttu-id="9dd95-139">Настройка защиты данных</span><span class="sxs-lookup"><span data-stu-id="9dd95-139">Configuring Data Protection</span></span>](data-protection/configuration/overview.md)
        *   [<span data-ttu-id="9dd95-140">Параметры по умолчанию</span><span class="sxs-lookup"><span data-stu-id="9dd95-140">Default Settings</span></span>](data-protection/configuration/default-settings.md)
        *   [<span data-ttu-id="9dd95-141">Политика на уровне компьютера</span><span class="sxs-lookup"><span data-stu-id="9dd95-141">Machine Wide Policy</span></span>](data-protection/configuration/machine-wide-policy.md)
        *   [<span data-ttu-id="9dd95-142">Сценарии, не поддерживающие DI</span><span class="sxs-lookup"><span data-stu-id="9dd95-142">Non DI Aware Scenarios</span></span>](data-protection/configuration/non-di-scenarios.md)
    *   [<span data-ttu-id="9dd95-143">API расширяемости</span><span class="sxs-lookup"><span data-stu-id="9dd95-143">Extensibility APIs</span></span>](data-protection/extensibility/index.md)
        *   [<span data-ttu-id="9dd95-144">Расширяемость базового шифрования</span><span class="sxs-lookup"><span data-stu-id="9dd95-144">Core cryptography extensibility</span></span>](data-protection/extensibility/core-crypto.md)
        *   [<span data-ttu-id="9dd95-145">Расширяемость управления ключами</span><span class="sxs-lookup"><span data-stu-id="9dd95-145">Key management extensibility</span></span>](data-protection/extensibility/key-management.md)
        *   [<span data-ttu-id="9dd95-146">Различные API</span><span class="sxs-lookup"><span data-stu-id="9dd95-146">Miscellaneous APIs</span></span>](data-protection/extensibility/misc-apis.md)
    *   [<span data-ttu-id="9dd95-147">Реализация</span><span class="sxs-lookup"><span data-stu-id="9dd95-147">Implementation</span></span>](data-protection/implementation/index.md)
        *   [<span data-ttu-id="9dd95-148">Сведения о шифровании с проверкой подлинности.</span><span class="sxs-lookup"><span data-stu-id="9dd95-148">Authenticated encryption details.</span></span>](data-protection/implementation/authenticated-encryption-details.md)
        *   [<span data-ttu-id="9dd95-149">Формирование подключа и шифрование с проверкой подлинности</span><span class="sxs-lookup"><span data-stu-id="9dd95-149">Subkey Derivation and Authenticated Encryption</span></span>](data-protection/implementation/subkeyderivation.md)
        *   [<span data-ttu-id="9dd95-150">Заголовки контекста</span><span class="sxs-lookup"><span data-stu-id="9dd95-150">Context headers</span></span>](data-protection/implementation/context-headers.md)
        *   [<span data-ttu-id="9dd95-151">Управление ключами</span><span class="sxs-lookup"><span data-stu-id="9dd95-151">Key Management</span></span>](data-protection/implementation/key-management.md)
        *   [<span data-ttu-id="9dd95-152">Поставщики хранилищ ключей</span><span class="sxs-lookup"><span data-stu-id="9dd95-152">Key Storage Providers</span></span>](data-protection/implementation/key-storage-providers.md)
        *   [<span data-ttu-id="9dd95-153">Шифрование ключей при хранении</span><span class="sxs-lookup"><span data-stu-id="9dd95-153">Key Encryption At Rest</span></span>](data-protection/implementation/key-encryption-at-rest.md)
        *   [<span data-ttu-id="9dd95-154">Неизменность ключа и изменение параметров</span><span class="sxs-lookup"><span data-stu-id="9dd95-154">Key Immutability and Changing Settings</span></span>](data-protection/implementation/key-immutability.md)
        *   [<span data-ttu-id="9dd95-155">Формат хранилища ключей</span><span class="sxs-lookup"><span data-stu-id="9dd95-155">Key Storage Format</span></span>](data-protection/implementation/key-storage-format.md)
        *   [<span data-ttu-id="9dd95-156">Временные поставщики защиты данных</span><span class="sxs-lookup"><span data-stu-id="9dd95-156">Ephemeral data protection providers</span></span>](data-protection/implementation/key-storage-ephemeral.md)
    *   [<span data-ttu-id="9dd95-157">Совместимость</span><span class="sxs-lookup"><span data-stu-id="9dd95-157">Compatibility</span></span>](data-protection/compatibility/index.md)
        *   [<span data-ttu-id="9dd95-158">Совместное использование файлов cookie в приложениях</span><span class="sxs-lookup"><span data-stu-id="9dd95-158">Sharing cookies between applications</span></span>](data-protection/compatibility/cookie-sharing.md)
        *   [<span data-ttu-id="9dd95-159">Замена <machineKey> в ASP.NET</span><span class="sxs-lookup"><span data-stu-id="9dd95-159">Replacing <machineKey> in ASP.NET</span></span>](data-protection/compatibility/replacing-machinekey.md)
*   [<span data-ttu-id="9dd95-160">Создание приложения с защитой данных пользователя с помощью авторизации</span><span class="sxs-lookup"><span data-stu-id="9dd95-160">Create an app with user data protected by authorization</span></span>](xref:security/authorization/secure-data)
*   [<span data-ttu-id="9dd95-161">Безопасное хранение секретов приложений во время разработки</span><span class="sxs-lookup"><span data-stu-id="9dd95-161">Safe storage of app secrets during development</span></span>](app-secrets.md)
*   [<span data-ttu-id="9dd95-162">Поставщик конфигурации Azure Key Vault</span><span class="sxs-lookup"><span data-stu-id="9dd95-162">Azure Key Vault configuration provider</span></span>](key-vault-configuration.md)
*   [<span data-ttu-id="9dd95-163">Применение SSL</span><span class="sxs-lookup"><span data-stu-id="9dd95-163">Enforcing SSL</span></span>](enforcing-ssl.md)
*   [<span data-ttu-id="9dd95-164">Настройка HTTPS для разработки</span><span class="sxs-lookup"><span data-stu-id="9dd95-164">Setting up HTTPS for development</span></span>](https.md)
*   [<span data-ttu-id="9dd95-165">Защита от подделки запросов</span><span class="sxs-lookup"><span data-stu-id="9dd95-165">Anti-Request Forgery</span></span>](anti-request-forgery.md)
*   [<span data-ttu-id="9dd95-166">Предотвращение атак с открытой переадресацией</span><span class="sxs-lookup"><span data-stu-id="9dd95-166">Preventing Open Redirect Attacks</span></span>](preventing-open-redirects.md)
*   [<span data-ttu-id="9dd95-167">Предотвращение использования межузловых сценариев</span><span class="sxs-lookup"><span data-stu-id="9dd95-167">Preventing Cross-Site Scripting</span></span>](cross-site-scripting.md)
*   [<span data-ttu-id="9dd95-168">Включение запросов о происхождении (CORS)</span><span class="sxs-lookup"><span data-stu-id="9dd95-168">Enabling Cross-Origin Requests (CORS)</span></span>](cors.md)
