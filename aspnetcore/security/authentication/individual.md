---
title: Статьи на основе проектов ASP.NET Core, созданных с учетными записями отдельных пользователей
author: rick-anderson
description: Ознакомьтесь с статьями, основанными на ASP.NET Core проектах, созданных с учетными записями отдельных пользователей.
ms.author: riande
ms.date: 11/30/2017
uid: security/authentication/individual
ms.openlocfilehash: cf548417268a8587787471b9ed91c0ed109fbee9
ms.sourcegitcommit: 215954a638d24124f791024c66fd4fb9109fd380
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 09/18/2019
ms.locfileid: "71080706"
---
# <a name="articles-based-on-aspnet-core-projects-created-with-individual-user-accounts"></a>Статьи на основе проектов ASP.NET Core, созданных с учетными записями отдельных пользователей

ASP.NET Core удостоверение включается в шаблоны проектов Visual Studio с параметром "индивидуальные учетные записи пользователей".

Шаблоны проверки подлинности доступны в .NET Core CLI `-au Individual`с помощью:

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

Проверка подлинности указывается в .NET Core CLI с `-au` параметром. В Visual Studio диалоговое окно **Изменение проверки подлинности** доступно для новых веб-приложений. По умолчанию для новых веб-приложений в Visual Studio **Проверка подлинности не**выполняется.

Проекты, созданные без проверки подлинности:

* Не следует включать веб-страницы и пользовательский интерфейс для входа и выхода.
* Не содержать код проверки подлинности.

<a name="win"></a>

## <a name="windows-authentication"></a>Проверка подлинности Windows

Проверка подлинности Windows указана для новых веб-приложений в `-au Windows` .NET Core CLI с параметром. В Visual Studio диалоговое окно **Изменение проверки подлинности** предоставляет параметры **проверки подлинности Windows** .

Если выбрана проверка подлинности Windows, приложение настроено для использования [модуля IIS проверки подлинности Windows](xref:host-and-deploy/iis/modules). Проверка подлинности Windows предназначена для веб-сайтов интрасети.

## <a name="additional-resources"></a>Дополнительные ресурсы

В следующих статьях показано, как использовать код, созданный в шаблонах ASP.NET Core, использующих отдельные учетные записи пользователей.

* [Двухфакторная проверка подлинности с помощью SMS](xref:security/authentication/2fa)
* [Подтверждение учетной записи и восстановление пароля в ASP.NET Core](xref:security/authentication/accconfirm)
* [Создание приложения ASP.NET Core с данными пользователя, защищенными с помощью авторизации](xref:security/authorization/secure-data)
