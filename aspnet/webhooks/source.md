---
uid: webhooks/source
title: Исходный код ASP.NET веб-перехватчики и пакетов NuGet | Документация Майкрософт
author: rick-anderson
description: Ссылки на исходный код ASP.NET веб-перехватчики и пакеты NuGet
ms.author: riande
ms.date: 01/17/2012
ms.assetid: 91a62bfa-ea3a-41f9-a2e1-e90d2c8fc8ca
ms.openlocfilehash: ff716b476f7dc69b6071d3febd5b5871e4f02689
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 08/16/2018
ms.locfileid: "41829045"
---
# <a name="aspnet-webhooks-source-code-and-nuget-packages"></a>Исходный код ASP.NET веб-перехватчики и пакеты NuGet

Веб-перехватчики Microsoft ASP.NET является частью семейства Microsoft ASP.NET, модулей и размещен как управляющий [открытым проектом источник на GitHub](https://github.com/aspnet/WebHooks). Это означает, что мы принимаем работы, но см. в [рекомендациями по участию](https://github.com/aspnet/Home/blob/master/CONTRIBUTING.md) перед отправкой запроса на включение внесенных изменений.

Теперь этот электронной документации, который вы читаете также размещен как управляющий [открытым исходным кодом на GitHub](http://docs.asp.net/en/latest/contribute/style-guide.html#style-guide) , а также принимает вклад.

## <a name="nuget-packages"></a>Пакеты NuGet

[Пакеты NuGet](https://nuget.org/packages?q=Microsoft.AspNet.WebHooks) можно разделить на три части:

* [Распространенные](https://www.nuget.org/packages?q=Microsoft.AspNet.WebHooks.Common): общие пакета, который разделяется между отправителями и получателями.

* [Отправитель](https://www.nuget.org/packages?q=Microsoft.AspNet.WebHooks.Custom): набор пакетов, поддержкой собственных веб-перехватчики пересылки другим пользователям. Функциональные возможности для отправки веб-перехватчики описан более подробно в [отправки веб-перехватчики](sending/index.md).

* [Приемники](https://www.nuget.org/packages?q=Microsoft.AspNet.WebHooks.Receivers): набор пакетов, поддерживающих получение веб-перехватчики от других. Функциональность для получения веб-перехватчики описан более подробно в [получения веб-перехватчики](receiving/index.md).
