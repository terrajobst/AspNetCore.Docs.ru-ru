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
ms.openlocfilehash: 566ee353f6a947e3ef0efdfd0af3a81dff2147c7
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 11/10/2017
---
# <a name="aspnet-webhooks-debugging"></a>Веб-перехватчиков ASP.NET Отладка  

## <a name="debugging-in-azure"></a>Отладка в Azure

Отладка веб-приложения во время выполнения в Azure, см. в разделе учебника [Устранение неполадок веб-приложения в службе приложений Azure с помощью Visual Studio](https://azure.microsoft.com/en-us/documentation/articles/web-sites-dotnet-troubleshoot-visual-studio/#webserverlogs).

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
