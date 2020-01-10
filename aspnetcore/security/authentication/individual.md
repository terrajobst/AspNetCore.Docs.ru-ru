---
title: Статьи на основе проектов ASP.NET Core, созданных с учетными записями отдельных пользователей
author: rick-anderson
description: Ознакомьтесь с статьями, основанными на ASP.NET Core проектах, созданных с учетными записями отдельных пользователей.
ms.author: riande
ms.date: 12/11/2019
uid: security/authentication/individual
ms.openlocfilehash: 7ef0d5eabded61d04fb9fe7be384a663ad7ea5f4
ms.sourcegitcommit: 7dfe6cc8408ac6a4549c29ca57b0c67ec4baa8de
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/09/2020
ms.locfileid: "75828715"
---
# <a name="articles-based-on-aspnet-core-projects-created-with-individual-user-accounts"></a>Статьи на основе проектов ASP.NET Core, созданных с учетными записями отдельных пользователей

ASP.NET Core удостоверение включается в шаблоны проектов Visual Studio с параметром "индивидуальные учетные записи пользователей".

Шаблоны проверки подлинности доступны в .NET Core CLI с `-au Individual`.

::: moniker range=">= aspnetcore-2.1"

```dotnetcli
dotnet new mvc -au Individual
dotnet new webapp -au Individual
```

::: moniker-end

::: moniker range="= aspnetcore-2.0"

```dotnetcli
dotnet new mvc -au Individual
dotnet new razor -au Individual
```

::: moniker-end

См. [эту ошибку GitHub](https://github.com/dotnet/AspNetCore/issues/5833) для проверки подлинности веб-API.

<a name="no"></a>

## <a name="no-authentication"></a>Без проверки подлинности

Проверка подлинности указывается в .NET Core CLI с параметром `-au`. В Visual Studio диалоговое окно **Изменение проверки подлинности** доступно для новых веб-приложений. По умолчанию для новых веб-приложений в Visual Studio **Проверка подлинности не**выполняется.

Проекты, созданные без проверки подлинности:

* Не следует включать веб-страницы и пользовательский интерфейс для входа и выхода.
* Не содержать код проверки подлинности.

<a name="win"></a>

## <a name="windows-authentication"></a>Аутентификация Windows

Для новых веб-приложений в .NET Core CLI указана проверка подлинности Windows с параметром `-au Windows`. В Visual Studio диалоговое окно **Изменение проверки подлинности** предоставляет параметры **проверки подлинности Windows** .

Если выбрана проверка подлинности Windows, приложение настроено для использования [модуля IIS проверки подлинности Windows](xref:host-and-deploy/iis/modules). Проверка подлинности Windows предназначена для веб-сайтов интрасети.

## <a name="dotnet-new-webapp-authentication-options"></a>Параметры проверки подлинности DotNet New webapp

В следующей таблице приведены параметры проверки подлинности, доступные для новых веб-приложений.

| Параметр | Тип проверки подлинности | Ссылка на дополнительные сведения |
 | ----------------- | ------------ | ---------- |
| Нет            |  Нет проверки подлинности | | 
| Индивидуальное лицо      |  Индивидуальная проверка подлинности | <xref:security/authentication/identity>
| IndividualB2C   |  Индивидуальная проверка подлинности, размещенная в облаке, с Azure AD B2C | [Azure AD B2C](/azure/active-directory-b2c/) |
| синглеорг       |  Аутентификация в Организации для одного клиента | [Azure AD](/azure/active-directory/develop/quickstart-v2-aspnet-core-webapp) |
| мултиорг        |  Проверка подлинности организации для нескольких клиентов | [Azure AD](/azure/active-directory/develop/quickstart-v2-aspnet-core-webapp) |
| Портал         |  Аутентификация Windows | [Проверка подлинности Windows](xref:security/authentication/windowsauth)

## <a name="visual-studio-new-webapp-authentication-options"></a>Параметры проверки подлинности New webapp в Visual Studio

В следующей таблице показаны параметры проверки подлинности, доступные при создании нового веб-приложения с помощью Visual Studio.

| Параметр | Тип проверки подлинности | Ссылка на дополнительные сведения |
 | ----------------- | ------------ | ---------- |
| Нет            |  Нет проверки подлинности | | 
| Индивидуальные учетные записи пользователей и хранение учетных записей пользователей в приложении |  Индивидуальная проверка подлинности | <xref:security/authentication/identity> |
| Учетные записи отдельных пользователей и подключение к существующему хранилищу пользователей в облаке |  Индивидуальная проверка подлинности, размещенная в облаке, с Azure AD B2C | [Azure AD B2C](/azure/active-directory-b2c/) |
| Рабочее или учебное облако/Единая Организация  |  Аутентификация в Организации для одного клиента | [Azure AD](/azure/active-directory/develop/quickstart-v2-aspnet-core-webapp) |
| Рабочее или учебное облако/несколько Организации |  Проверка подлинности организации для нескольких клиентов | [Azure AD](/azure/active-directory/develop/quickstart-v2-aspnet-core-webapp) |
| Портал         |  Аутентификация Windows | [Проверка подлинности Windows](xref:security/authentication/windowsauth)

## <a name="additional-resources"></a>Дополнительные ресурсы

В следующих статьях показано, как использовать код, созданный в шаблонах ASP.NET Core, использующих отдельные учетные записи пользователей.

* [Подтверждение учетной записи и восстановление пароля в ASP.NET Core](xref:security/authentication/accconfirm)
* [Создание приложения ASP.NET Core с данными пользователя, защищенными с помощью авторизации](xref:security/authorization/secure-data)
