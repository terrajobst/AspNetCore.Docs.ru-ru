---
title: Использование шаблона проекта React и Redux с ASP.NET Core
author: SteveSandersonMS
description: Сведения о начале работы с шаблоном проекта одностраничного приложения (SPA) ASP.NET Core для React с Redux и create-react-app.
monikerRange: '>= aspnetcore-2.0'
ms.author: scaddie
ms.custom: mvc
ms.date: 02/21/2018
uid: spa/react-with-redux
ms.openlocfilehash: c1aedcf1e14a9e7b339b60dd02c4267cd5945a49
ms.sourcegitcommit: 42a8164b8aba21f322ffefacb92301bdfb4d3c2d
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/16/2019
ms.locfileid: "54341624"
---
# <a name="use-the-react-with-redux-project-template-with-aspnet-core"></a>Использование шаблона проекта React и Redux с ASP.NET Core

::: moniker range="= aspnetcore-2.0"

> [!NOTE]
> Эта документация не относятся к шаблоне проекта React с Redux, включенные в ASP.NET Core 2.0. Это имеет отношение к новой React с Redux шаблон, который можно обновить вручную. Шаблон ASP.NET Core 2.1 или более поздних версий.

::: moniker-end

Обновленный шаблон проекта React и Redux служит удобной отправной точкой для создания приложений ASP.NET Core на основе конвенций React, Redux и [create-react-app](https://github.com/facebookincubator/create-react-app) (CRA), позволяющих реализовать полноценный пользовательский интерфейс на стороне клиента.

За исключением команд создания проекта, все сведения о шаблоне React и Redux совпадают с данными о шаблоне React. Чтобы создать проект такого типа, выполните `dotnet new reactredux` вместо `dotnet new react`. Дополнительные сведения о функциональности обоих шаблонов на основе React вы найдете в [документации по шаблону React](xref:spa/react).

Сведения о настройке React с Redux составные части приложения в IIS, см. в разделе [2.1 ReactRedux шаблона: Не удается использовать SPA в службах IIS (aspnet/Templating &num;555)](https://github.com/aspnet/Templating/issues/555).
