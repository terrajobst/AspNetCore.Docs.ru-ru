---
title: Статьи на основе проектов ASP.NET Core, созданных с учетными записями отдельных пользователей
author: rick-anderson
description: Ознакомьтесь с статьями, основанными на ASP.NET Core проектах, созданных с учетными записями отдельных пользователей.
ms.author: riande
ms.date: 11/30/2017
uid: security/authentication/individual
ms.openlocfilehash: 91c5665dc50124b3ba09bdcfbf3ba501f684c604
ms.sourcegitcommit: 9e85c2562df5e108d7933635c830297f484bb775
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 11/04/2019
ms.locfileid: "73463035"
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

См. [эту ошибку GitHub](https://github.com/aspnet/AspNetCore/issues/5833) для проверки подлинности веб-API.

<a name="no"></a>

## <a name="no-authentication"></a>Без проверки подлинности

Проверка подлинности указывается в .NET Core CLI с параметром `-au`. В Visual Studio диалоговое окно **Изменение проверки подлинности** доступно для новых веб-приложений. По умолчанию для новых веб-приложений в Visual Studio **Проверка подлинности не**выполняется.

Проекты, созданные без проверки подлинности:

* Не следует включать веб-страницы и пользовательский интерфейс для входа и выхода.
* Не содержать код проверки подлинности.

<a name="win"></a>

## <a name="windows-authentication"></a>Проверка подлинности Windows

Для новых веб-приложений в .NET Core CLI указана проверка подлинности Windows с параметром `-au Windows`. В Visual Studio диалоговое окно **Изменение проверки подлинности** предоставляет параметры **проверки подлинности Windows** .

Если выбрана проверка подлинности Windows, приложение настроено для использования [модуля IIS проверки подлинности Windows](xref:host-and-deploy/iis/modules). Проверка подлинности Windows предназначена для веб-сайтов интрасети.

## <a name="additional-resources"></a>Дополнительные ресурсы

В следующих статьях показано, как использовать код, созданный в шаблонах ASP.NET Core, использующих отдельные учетные записи пользователей.

* [Подтверждение учетной записи и восстановление пароля в ASP.NET Core](xref:security/authentication/accconfirm)
* [Создание приложения ASP.NET Core с данными пользователя, защищенными с помощью авторизации](xref:security/authorization/secure-data)
