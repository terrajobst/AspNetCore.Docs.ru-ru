---
title: Статьи на основе проектов ASP.NET Core, созданных с помощью отдельных учетных записей пользователей
author: rick-anderson
description: Ознакомьтесь со статьями, на основе проектов ASP.NET Core, созданных с помощью отдельных учетных записей пользователей.
ms.author: riande
ms.date: 11/30/2017
uid: security/authentication/individual
ms.openlocfilehash: ac843342ffc73632fbf9f6359c6c1a5878dcef0d
ms.sourcegitcommit: c12ebdab65853f27fbb418204646baf6ce69515e
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 09/21/2018
ms.locfileid: "46523068"
---
# <a name="articles-based-on-aspnet-core-projects-created-with-individual-user-accounts"></a>Статьи на основе проектов ASP.NET Core, созданных с помощью отдельных учетных записей пользователей

Удостоверение ASP.NET Core включается в шаблоны проектов в Visual Studio с параметром «Учетные записи отдельных пользователей».

Шаблоны проверки подлинности доступны в .NET Core CLI с помощью `-au Individual`:

::: moniker range=">= aspnetcore-2.1"

```console
dotnet new mvc -au Individual
dotnet new webapi -au Individual
dotnet new webapp -au Individual
```

::: moniker-end

::: moniker range="= aspnetcore-2.0"

```console
dotnet new mvc -au Individual
dotnet new webapi -au Individual
dotnet new razor -au Individual
```

::: moniker-end

<a name="no"></a>
## <a name="no-authentication"></a>Без проверки подлинности

Указана проверка подлинности в .NET Core CLI с `-au` параметр. В Visual Studio **изменить способ проверки подлинности** диалоговое окно доступно для новых веб-приложений. Значение по умолчанию для новых веб-приложений в Visual Studio — **без проверки подлинности**.

Проекты, созданные без проверки подлинности:

* Не содержат веб-страницы и пользовательский Интерфейс для входа систему и выхода.
* Не содержат код проверки подлинности.

<a name="win"></a>
## <a name="windows-authentication"></a>Проверка подлинности Windows

Для новых веб-приложений в .NET Core CLI с задана проверка подлинности Windows `-au Windows` параметр. В Visual Studio **изменить способ проверки подлинности** диалоговое окно предоставляет **проверки подлинности Windows** параметры.

Если выбран метод проверки подлинности Windows, приложение настроено для использования [модуль IIS для Windows](xref:host-and-deploy/iis/modules). Проверка подлинности Windows предназначен для веб-сайтов интрасети.

## <a name="additional-resources"></a>Дополнительные ресурсы

В следующих статьях описывается, как использовать код, созданный в шаблонах ASP.NET Core, использующих отдельных учетных записей пользователей:

* [Двухфакторная проверка подлинности с помощью SMS](xref:security/authentication/2fa)
* [Подтверждение учетной записи и восстановление пароля в ASP.NET Core](xref:security/authentication/accconfirm)
* [Создание приложения ASP.NET Core с помощью данных пользователя с помощью авторизации](xref:security/authorization/secure-data)
