---
title: Добавление поиска
author: rick-anderson
description: Инструкции по добавлению поиска в простое приложение ASP.NET Core MVC
ms.author: riande
ms.date: 04/07/2017
uid: tutorials/first-mvc-app-xplat/search
ms.openlocfilehash: dc84eb38c0487d90451979ec9572bf1641571357
ms.sourcegitcommit: a1afd04758e663d7062a5bfa8a0d4dca38f42afc
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 06/20/2018
ms.locfileid: "36276010"
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
