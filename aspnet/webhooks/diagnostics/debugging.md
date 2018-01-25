---
uid: webhooks/diagnostics/debugging
title: "Веб-перехватчиков ASP.NET Отладка | Документы Microsoft"
author: rick-anderson
description: "Способы отладки ASP.NET веб-привязок."
ms.author: aspnetcontent
manager: wpickett
ms.date: 01/17/2012
ms.topic: article
ms.assetid: 467da78b-3c35-4c51-8b08-77a32379e4a8
ms.technology: 
ms.prod: .net-framework
ms.openlocfilehash: 524cdf0246eda9ef213414923cd23a92a01f211e
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/24/2018
---
# <a name="aspnet-webhooks-debugging"></a>Веб-перехватчиков ASP.NET Отладка  

## <a name="debugging-in-azure"></a>Отладка в Azure

Отладка веб-приложения во время выполнения в Azure, см. в разделе учебника [Устранение неполадок веб-приложения в службе приложений Azure с помощью Visual Studio](https://azure.microsoft.com/documentation/articles/web-sites-dotnet-troubleshoot-visual-studio/#webserverlogs).

## <a name="debugging-with-source-and-symbols"></a>Отладка с помощью исходного кода и символов

В дополнение к отладке кода, можно отлаживать непосредственно в веб-перехватчиков Microsoft ASP.NET и фактически все .NET. Это работает независимо от того, отладочная локально или удаленно. Сначала необходимо настроить Visual Studio, чтобы найти источник и символы в **отладки** и затем **параметры и настройки**. Необходимо задайте параметры следующим образом:

![Параметры и настройки](_static/SourceSymbols.png)

Затем добавьте ссылку на [symbolsource.org](http://symbolsource.org) для загрузки источника и символы. Последовательно выберите пункты **символы** вкладка меню выше и добавьте следующее расположение символов:

```
http://srv.symbolsource.org/pdb/Public
```

Кроме того убедитесь, что каталог кэша имеет короткое имя; в противном случае имена файлов можно получить слишком длинное что приведет к символы, не загружаются. Образец пути является:

```
C:\SymCache
```

Параметры должны выглядеть следующим образом:

![Пример расположения файла параметры символов](_static/SymSource.png)
