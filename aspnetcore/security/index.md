---
title: Общие сведения о безопасности ASP.NET Core
author: rachelappel
description: Основные сведения о проверки подлинности, авторизации и безопасности в ASP.NET Core.
ms.author: rachelap
ms.date: 11/01/2017
uid: security/index
ms.openlocfilehash: a23d23cf1bf0503b59c6f5d962cecf89af37b4b3
ms.sourcegitcommit: a1afd04758e663d7062a5bfa8a0d4dca38f42afc
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 06/20/2018
ms.locfileid: "36278509"
---
# <a name="overview-of-aspnet-core-security"></a>Общие сведения о безопасности ASP.NET Core

ASP.NET Core позволяет разработчикам легко настраивать параметры безопасности для приложений и управлять ими. ASP.NET Core содержит функции для управления проверкой подлинности, авторизацией, защитой данных, применением SSL, секретами приложений, защитой от подделки запросов, а также для управления CORS. Эти функции обеспечения безопасности позволяют создавать надежные и защищенные приложения ASP.NET Core.

## <a name="aspnet-core-security-features"></a>Функции безопасности в ASP.NET Core

ASP.NET Core предоставляет множество средств и библиотек для защиты приложений, включая встроенные поставщики удостоверений, однако помимо них можно использовать сторонние службы удостоверений, такие как Facebook, LinkedIn и Twitter. В ASP.NET Core можно легко управлять секретами приложений, которые позволяют хранить и использовать конфиденциальные данные, не предоставляя их в коде.

## <a name="authentication-vs-authorization"></a>Проверка подлинности и Авторизация

Проверка подлинности — это процесс, когда пользователь вводит учетные данные, которые затем сравниваются с данными, хранящимися в операционной системе, базе данных, приложении или ресурсе. Если они совпадают, пользователи успешно проходят аутентификацию и во время авторизации могут выполнять разрешенные действия. Авторизация представляет собой процесс, определяющий, какие действия может выполнять пользователь.

Если взглянуть на проверку подлинности с другой стороны, ее можно считать способом входа в определенную область, например на сервер, в базу данных, приложение или ресурс, тогда как авторизация определяет, какие действия с какими объектами может выполнять пользователь в этой области (на сервере, в базе данных или приложении).

## <a name="common-vulnerabilities-in-software"></a>Распространенные уязвимости в программном обеспечении

ASP.NET Core и EF содержат средства, помогающие защитить приложения и предотвратить возникновение нарушений безопасности. Далее приводится список ссылок на документацию с описанием методов, позволяющих устранять наиболее распространенные уязвимости в веб-приложениях:

* [Атаки с использованием межузловых сценариев](xref:security/cross-site-scripting)
* [Атаки путем внедрения кода SQL](https://docs.microsoft.com/ef/core/querying/raw-sql)
* [Подделки межсайтовых запросов (CSRF)](xref:security/anti-request-forgery)
* [Атаки с открытой переадресацией](xref:security/preventing-open-redirects)

Существует еще целый ряд уязвимостей, о которых следует знать. Дополнительные сведения см. в разделе этого документа, посвященном *документации по безопасности ASP.NET*.

## <a name="aspnet-security-documentation"></a>Документация по безопасности ASP.NET

*   [Проверка подлинности](xref:security/authentication/index)
    *   [Общие сведения об Identity](xref:security/authentication/identity)
    *   [Включение проверки подлинности с помощью Facebook, Google и других внешних поставщиков](xref:security/authentication/social/index)
    *   [Включение проверки подлинности с помощью WS-Federation](xref:security/authentication/ws-federation)
    * [Настройка проверки подлинности Windows](xref:security/authentication/windowsauth)
    *   [Подтверждение учетной записи и восстановление пароля](xref:security/authentication/accconfirm)
    *   [Двухфакторная проверка подлинности с помощью SMS](xref:security/authentication/2fa)
    *   [Использование проверки подлинности с помощью файлов cookie без Identity](xref:security/authentication/cookie)
    *   [Azure Active Directory](xref:security/authentication/azure-active-directory/index)
        *   [Интеграция Azure AD в веб-приложение ASP.NET Core](https://azure.microsoft.com/documentation/samples/active-directory-dotnet-webapp-openidconnect-aspnetcore/)
        *   [Вызов веб-API ASP.NET Core из приложения WPF с помощью Azure AD](https://azure.microsoft.com/documentation/samples/active-directory-dotnet-native-aspnetcore/)
        *   [Вызов веб-API в веб-приложении ASP.NET Core с помощью Azure AD](https://azure.microsoft.com/documentation/samples/active-directory-dotnet-webapp-webapi-openidconnect-aspnetcore/)
        *   [ASP.NET Core с Azure AD B2C](https://azure.microsoft.com/resources/samples/active-directory-b2c-dotnetcore-webapp/)
    *   [Защита приложений ASP.NET Core с помощью IdentityServer4](https://identityserver4.readthedocs.io)
*   [Авторизация](xref:security/authorization/index)
    *   [Введение](xref:security/authorization/introduction)
    *   [Создание приложения с защитой данных пользователя с помощью авторизации](xref:security/authorization/secure-data)
    *   [Простая авторизация](xref:security/authorization/simple)
    *   [Авторизация на основе ролей](xref:security/authorization/roles)
    *   [Авторизация на основе утверждений](xref:security/authorization/claims)
    *   [Авторизация на основе политик](xref:security/authorization/policies)
    *   [Внедрение зависимостей в обработчики требований](xref:security/authorization/dependencyinjection)
    *   [Авторизация на основе ресурсов](xref:security/authorization/resourcebased)
    *   [Авторизация на основе представлений](xref:security/authorization/views)
    *   [Ограничение идентификаторов по схеме](xref:security/authorization/limitingidentitybyscheme)
*   [Защита данных](xref:security/data-protection/index)
    *   [Общие сведения о защите данных](xref:security/data-protection/introduction)
    *   [Начало работы с API защиты данных](xref:security/data-protection/using-data-protection)
    *   [Потребительские API](xref:security/data-protection/consumer-apis/index)
        *   [Обзор потребительских API](xref:security/data-protection/consumer-apis/overview)
        *   [Строки назначений](xref:security/data-protection/consumer-apis/purpose-strings)
        *   [Иерархия назначений и мультитенантность](xref:security/data-protection/consumer-apis/purpose-strings-multitenancy)
        *   [Хэшированные пароли](xref:security/data-protection/consumer-apis/password-hashing)
        *   [Ограничение времени существования защищенных полезных данных](xref:security/data-protection/consumer-apis/limited-lifetime-payloads)
        *   [Снятие защиты полезных данных, ключи которых были отозваны](xref:security/data-protection/consumer-apis/dangerous-unprotect)
    *   [Конфигурация](xref:security/data-protection/configuration/index)
        *   [Настройка защиты данных](xref:security/data-protection/configuration/overview)
        *   [Параметры по умолчанию](xref:security/data-protection/configuration/default-settings)
        *   [Политики уровня компьютера](xref:security/data-protection/configuration/machine-wide-policy)
        *   [Сценарии, не поддерживающие внедрение зависимостей](xref:security/data-protection/configuration/non-di-scenarios)
    *   [API расширяемости](xref:security/data-protection/extensibility/index)
        *   [Расширяемость базового шифрования](xref:security/data-protection/extensibility/core-crypto)
        *   [Расширяемость управления ключами](xref:security/data-protection/extensibility/key-management)
        *   [Различные API](xref:security/data-protection/extensibility/misc-apis)
    *   [Реализация](xref:security/data-protection/implementation/index)
        *   [Сведения о шифровании с проверкой подлинности](xref:security/data-protection/implementation/authenticated-encryption-details)
        *   [Формирование подключа и шифрование с проверкой подлинности](xref:security/data-protection/implementation/subkeyderivation)
        *   [Заголовки контекста](xref:security/data-protection/implementation/context-headers)
        *   [Управление ключами](xref:security/data-protection/implementation/key-management)
        *   [Поставщики хранилищ ключей](xref:security/data-protection/implementation/key-storage-providers)
        *   [Шифрование ключей при хранении](xref:security/data-protection/implementation/key-encryption-at-rest)
        *   [Неизменность и параметры ключа](xref:security/data-protection/implementation/key-immutability)
        *   [Формат хранилища ключей](xref:security/data-protection/implementation/key-storage-format)
        *   [Временные поставщики защиты данных](xref:security/data-protection/implementation/key-storage-ephemeral)
    *   [Совместимость](xref:security/data-protection/compatibility/index)
        *   [Замена <machineKey> в ASP.NET](xref:security/data-protection/compatibility/replacing-machinekey)
*   [Создание приложения с защитой данных пользователя с помощью авторизации](xref:security/authorization/secure-data)
*   [Безопасное хранение секретов приложений во время разработки](xref:security/app-secrets)
*   [Поставщик конфигурации Azure Key Vault](xref:security/key-vault-configuration)
*   [Применение SSL](xref:security/enforcing-ssl)
*   [Защита от подделки запросов](xref:security/anti-request-forgery)
*   [Предотвращение атак с открытой переадресацией](xref:security/preventing-open-redirects)
*   [Предотвращайте использование межузловых сценариев](xref:security/cross-site-scripting)
*   [Включение запросов о происхождении (CORS)](xref:security/cors)
*   [Совместное использование приложениями файлов cookie](xref:security/cookie-sharing)
