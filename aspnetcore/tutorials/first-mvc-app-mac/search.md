---
title: Добавление поиска в приложение MVC ASP.NET Core
author: rick-anderson
description: Инструкции по добавлению поиска в простое приложение ASP.NET Core MVC
ms.author: riande
ms.date: 04/07/2017
uid: tutorials/first-mvc-app-mac/search
ms.openlocfilehash: aca0835340977605cc84fad1970ac30fa1a9872a
ms.sourcegitcommit: 356c8d394aaf384c834e9c90cabab43bfe36e063
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 06/26/2018
ms.locfileid: "36961546"
---
[!INCLUDE [adding-model](../../includes/mvc-intro/search1.md)]

Примечание. SQLlite зависит от регистра, поэтому условием поиска должно быть "Ghost", а не "ghost".

[!INCLUDE [adding-model](../../includes/mvc-intro/search2.md)]

Измените тег `<form>` в представлении Razor *Views\movie\Index.cshtml* для указания `method="get"`:

```html
<form asp-controller="Movies" asp-action="Index" method="get">
```

[!INCLUDE [adding-model](../../includes/mvc-intro/search3.md)]

> [!div class="step-by-step"]
> [Предыдущая статья — "Методы и представления контроллера"](controller-methods-views.md)
> [Следующая статья — "Добавление поля"](new-field.md)
