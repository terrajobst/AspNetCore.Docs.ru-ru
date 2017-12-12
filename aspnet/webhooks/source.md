---
uid: webhooks/source
title: "Веб-перехватчиков ASP.NET исходный код и пакеты NuGet | Документы Microsoft"
author: rick-anderson
description: "Ссылки на веб-перехватчиков ASP.NET исходный код и пакеты NuGet"
ms.author: aspnetcontent
manager: wpickett
ms.date: 01/17/2012
ms.topic: article
ms.assetid: 91a62bfa-ea3a-41f9-a2e1-e90d2c8fc8ca
ms.technology: 
ms.prod: .net-framework
ms.openlocfilehash: ab5eaaa32a8f678b2aa4e2a0b30b674dbd6d6e9c
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 11/10/2017
---
# <a name="aspnet-webhooks-source-code-and-nuget-packages"></a>Веб-перехватчиков ASP.NET исходный код и пакеты NuGet

Веб-перехватчиков Microsoft ASP.NET является частью семейства Microsoft ASP.NET модулей и размещается в виде [Открытие исходного проекта на GitHub](https://github.com/aspnet/WebHooks). Это означает, что мы принимаем работы, но см. в [вклад рекомендации](https://github.com/aspnet/Home/blob/master/CONTRIBUTING.md) перед отправкой запроса на включение.

В следующей документации по документации, которую вы читаете теперь также размещается в виде [открытым исходным кодом на сайте GitHub](http://docs.asp.net/en/latest/contribute/style-guide.html#style-guide) , а также принимает публикаций.

## <a name="nuget-packages"></a>Пакеты NuGet

Веб-перехватчиков Microsoft ASP.NET можно получить, как просмотреть пакеты Nuget, это означает, что необходимо выбрать флаг предварительного просмотра в Visual Studio для отображения этих.

[Пакетов Nuget](https://nuget.org/packages?q=Microsoft.AspNet.WebHooks) devided на три части:

* [Общие](https://www.nuget.org/packages?q=Microsoft.AspNet.WebHooks.Common): общие пакета, который является общим для отправителей и получателей.

* [Отправитель](https://www.nuget.org/packages?q=Microsoft.AspNet.WebHooks.Custom): набор пакетов, поддержка отправки собственных веб-перехватчиков для других пользователей. Данная функциональная возможность для отправки веб-перехватчиков описан более подробно в [отправки веб-перехватчиков](sending/index.md).

* [Приемники](https://www.nuget.org/packages?q=Microsoft.AspNet.WebHooks.Receivers): набор пакетов, поддерживающий получение веб-перехватчиков от других. Более подробно описана функциональность для получения веб-перехватчиков [получения веб-перехватчиков](receiving/index.md).
