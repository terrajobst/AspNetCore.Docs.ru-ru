---
title: "Общие сведения о безопасности ASP.NET Core | Документы Майкрософт"
author: rachelappel
description: "Сведения об основах проверки подлинности, авторизации и безопасности в ASP.NET Core"
ms.author: rachelap
manager: wpickett
ms.date: 11/01/2017
ms.topic: article
ms.assetid: a8fb7eb7-e0e5-4394-84f3-1f1dbe012345
ms.technology: aspnet
ms.prod: asp.net-core
uid: security/index
ms.openlocfilehash: 4f3a74d67ce3453499ea9785cc80bee183dc1aff
ms.sourcegitcommit: e4fb6b13be56a0fb2f2778623740a047d6489227
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 11/16/2017
---
# <a name="aspnet-core-security-overview"></a>Общие сведения о безопасности ASP.NET Core

ASP.NET Core позволяет разработчикам легко настраивать параметры безопасности для приложений и управлять ими. ASP.NET Core содержит функции для управления проверкой подлинности, авторизацией, защитой данных, применением SSL, секретами приложений, защитой от подделки запросов, а также для управления CORS. Эти функции обеспечения безопасности позволяют создавать надежные и защищенные приложения ASP.NET Core. 

## <a name="aspnet-core-security-features"></a>Функции безопасности в ASP.NET Core

ASP.NET Core предоставляет множество средств и библиотек для защиты приложений, включая встроенные поставщики удостоверений, однако помимо них можно использовать сторонние службы удостоверений, такие как Facebook, LinkedIn и Twitter. В ASP.NET Core можно легко управлять секретами приложений, которые позволяют хранить и использовать конфиденциальные данные, не предоставляя их в коде. 

## <a name="authentication-vs-authorization"></a>Проверка подлинности и Авторизация

Проверка подлинности — это процесс, когда пользователь вводит учетные данные, которые затем сравниваются с данными, хранящимися в операционной системе, базе данных, приложении или ресурсе. Если они совпадают, пользователи успешно проходят проверку подлинности и во время авторизации могут выполнять действия, для которых у них есть разрешения. Авторизация представляет собой процесс, определяющий, какие действия может выполнять пользователь. 

Если взглянуть на проверку подлинности с другой стороны, ее можно считать способом входа в определенную область, например на сервер, в базу данных, приложение или ресурс, тогда как авторизация определяет, какие действия с какими объектами может выполнять пользователь в этой области (на сервере, в базе данных или приложении).

## <a name="common-vulnerabilities-in-software"></a>Распространенные уязвимости в программном обеспечении

ASP.NET Core и EF содержат средства, помогающие защитить приложения и предотвратить возникновение нарушений безопасности. Далее приводится список ссылок на документацию с описанием методов, позволяющих устранять наиболее распространенные уязвимости в веб-приложениях:

* [Атаки с использованием межузловых сценариев](https://docs.microsoft.com/aspnet/core/security/cross-site-scripting)
* [Атаки путем внедрения кода SQL](https://docs.microsoft.com/ef/core/querying/raw-sql)
* [Подделки межсайтовых запросов (CSRF)](https://docs.microsoft.com/aspnet/core/security/anti-request-forgery)
* [Атаки с открытой переадресацией](https://docs.microsoft.com/aspnet/core/security/preventing-open-redirects)

Существует еще целый ряд уязвимостей, о которых следует знать. Дополнительные сведения см. в разделе этого документа, посвященном *документации по безопасности ASP.NET*. 

## <a name="aspnet-security-documentation"></a>Документация по безопасности ASP.NET

*   [Проверка подлинности](authentication/index.md)
    *   [Общие сведения об Identity](authentication/identity.md)
    *   [Включение проверки подлинности с помощью Facebook, Google и других внешних поставщиков](authentication/social/index.md)
    * [Настройка проверки подлинности Windows](authentication/windowsauth.md)
    *   [Подтверждение учетной записи и восстановление пароля](authentication/accconfirm.md)
    *   [Двухфакторная проверка подлинности с помощью SMS](authentication/2fa.md) 
    *   [Использование проверки подлинности с помощью файлов cookie без ASP.NET Core Identity](authentication/cookie.md)
    *   [Azure Active Directory](authentication/azure-active-directory/index.md)
        *   [Интеграция Azure AD в веб-приложение ASP.NET Core](https://azure.microsoft.com/documentation/samples/active-directory-dotnet-webapp-openidconnect-aspnetcore/)
        *   [Вызов веб-API ASP.NET Core из приложения WPF с помощью Azure AD](https://azure.microsoft.com/documentation/samples/active-directory-dotnet-native-aspnetcore/)
        *   [Вызов веб-API в веб-приложении ASP.NET Core с помощью Azure AD](https://azure.microsoft.com/documentation/samples/active-directory-dotnet-webapp-webapi-openidconnect-aspnetcore/)
        *   [ASP.NET Core с Azure AD B2C](https://azure.microsoft.com/resources/samples/active-directory-b2c-dotnetcore-webapp/)
    *   [Защита приложений ASP.NET Core с помощью IdentityServer4](https://identityserver4.readthedocs.io)
*   [Авторизация](authorization/index.md)
    *   [Введение](authorization/introduction.md)
    *   [Создание приложения с защитой данных пользователя с помощью авторизации](xref:security/authorization/secure-data)
    *   [Простая авторизация](authorization/simple.md)
    *   [Авторизация на основе ролей](authorization/roles.md)
    *   [Авторизация на основе утверждений](authorization/claims.md)
    *   [Пользовательская авторизация на основе политик](authorization/policies.md)
    *   [Внедрение зависимостей в обработчики требований](authorization/dependencyinjection.md)
    *   [Авторизация на основе ресурсов](authorization/resourcebased.md)
    *   [Авторизация на основе представлений](authorization/views.md)
    *   [Ограничение идентификаторов по схеме](authorization/limitingidentitybyscheme.md)
*   [Защита данных](data-protection/index.md)
    *   [Общие сведения о защите данных](data-protection/introduction.md)
    *   [Начало работы с API защиты данных](data-protection/using-data-protection.md)
    *   [Потребительские API](data-protection/consumer-apis/index.md)
        *   [Обзор потребительских API](data-protection/consumer-apis/overview.md)
        *   [Строки назначений](data-protection/consumer-apis/purpose-strings.md)
        *   [Иерархия назначений и мультитенантность](data-protection/consumer-apis/purpose-strings-multitenancy.md)
        *   [Хэширование паролей](data-protection/consumer-apis/password-hashing.md)
        *   [Ограничение времени существования защищенных полезных данных](data-protection/consumer-apis/limited-lifetime-payloads.md)
        *   [Снятие защиты полезных данных, ключи которых были отозваны](data-protection/consumer-apis/dangerous-unprotect.md)
    *   [Конфигурация](data-protection/configuration/index.md)
        *   [Настройка защиты данных](data-protection/configuration/overview.md)
        *   [Параметры по умолчанию](data-protection/configuration/default-settings.md)
        *   [Политика на уровне компьютера](data-protection/configuration/machine-wide-policy.md)
        *   [Сценарии, не поддерживающие DI](data-protection/configuration/non-di-scenarios.md)
    *   [API расширяемости](data-protection/extensibility/index.md)
        *   [Расширяемость базового шифрования](data-protection/extensibility/core-crypto.md)
        *   [Расширяемость управления ключами](data-protection/extensibility/key-management.md)
        *   [Различные API](data-protection/extensibility/misc-apis.md)
    *   [Реализация](data-protection/implementation/index.md)
        *   [Сведения о шифровании с проверкой подлинности](data-protection/implementation/authenticated-encryption-details.md)
        *   [Формирование подключа и шифрование с проверкой подлинности](data-protection/implementation/subkeyderivation.md)
        *   [Заголовки контекста](data-protection/implementation/context-headers.md)
        *   [Управление ключами](data-protection/implementation/key-management.md)
        *   [Поставщики хранилищ ключей](data-protection/implementation/key-storage-providers.md)
        *   [Шифрование ключей при хранении](data-protection/implementation/key-encryption-at-rest.md)
        *   [Неизменность ключа и изменение параметров](data-protection/implementation/key-immutability.md)
        *   [Формат хранилища ключей](data-protection/implementation/key-storage-format.md)
        *   [Временные поставщики защиты данных](data-protection/implementation/key-storage-ephemeral.md)
    *   [Совместимость](data-protection/compatibility/index.md)
        *   [Совместное использование файлов cookie в приложениях](data-protection/compatibility/cookie-sharing.md)
        *   [Замена <machineKey> в ASP.NET](data-protection/compatibility/replacing-machinekey.md)
*   [Создание приложения с защитой данных пользователя с помощью авторизации](xref:security/authorization/secure-data)
*   [Безопасное хранение секретов приложений во время разработки](app-secrets.md)
*   [Поставщик конфигурации Azure Key Vault](key-vault-configuration.md)
*   [Применение SSL](enforcing-ssl.md)
*   [Защита от подделки запросов](anti-request-forgery.md)
*   [Предотвращение атак с открытой переадресацией](preventing-open-redirects.md)
*   [Предотвращение использования межузловых сценариев](cross-site-scripting.md)
*   [Включение запросов о происхождении (CORS)](cors.md)
