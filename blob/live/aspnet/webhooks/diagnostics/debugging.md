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
# <a name="aspnet-webhooks-debugging"></a><span data-ttu-id="db2dc-103">Веб-перехватчиков ASP.NET Отладка</span><span class="sxs-lookup"><span data-stu-id="db2dc-103">ASP.NET WebHooks debugging</span></span>  

## <a name="debugging-in-azure"></a><span data-ttu-id="db2dc-104">Отладка в Azure</span><span class="sxs-lookup"><span data-stu-id="db2dc-104">Debugging in Azure</span></span>

<span data-ttu-id="db2dc-105">Отладка веб-приложения во время выполнения в Azure, см. в разделе учебника [Устранение неполадок веб-приложения в службе приложений Azure с помощью Visual Studio](https://azure.microsoft.com/en-us/documentation/articles/web-sites-dotnet-troubleshoot-visual-studio/#webserverlogs).</span><span class="sxs-lookup"><span data-stu-id="db2dc-105">To debug your Web Application while running in Azure, please see the tutorial [Troubleshoot a web app in Azure App Service using Visual Studio](https://azure.microsoft.com/en-us/documentation/articles/web-sites-dotnet-troubleshoot-visual-studio/#webserverlogs).</span></span>

## <a name="debugging-with-source-and-symbols"></a><span data-ttu-id="db2dc-106">Отладка с помощью исходного кода и символов</span><span class="sxs-lookup"><span data-stu-id="db2dc-106">Debugging with Source and Symbols</span></span>

<span data-ttu-id="db2dc-107">В дополнение к отладке кода, можно отлаживать непосредственно в веб-перехватчиков Microsoft ASP.NET и фактически все .NET.</span><span class="sxs-lookup"><span data-stu-id="db2dc-107">In addition to debugging your own code, it is possible to debug directly into Microsoft ASP.NET WebHooks, and in fact all of .NET.</span></span> <span data-ttu-id="db2dc-108">Это работает независимо от того, отладочная локально или удаленно.</span><span class="sxs-lookup"><span data-stu-id="db2dc-108">This works regardless of whether you debug locally or remotely.</span></span> <span data-ttu-id="db2dc-109">Сначала необходимо настроить Visual Studio, чтобы найти источник и символы в **отладки** и затем **параметры и настройки**.</span><span class="sxs-lookup"><span data-stu-id="db2dc-109">First, configure Visual Studio to find the source and symbols by going to **Debug** and then **Options and Settings**.</span></span> <span data-ttu-id="db2dc-110">Необходимо задайте параметры следующим образом:</span><span class="sxs-lookup"><span data-stu-id="db2dc-110">Set the options like this:</span></span>

![Параметры и настройки](_static/SourceSymbols.png)

<span data-ttu-id="db2dc-112">Затем добавьте ссылку на [symbolsource.org](http://symbolsource.org) для загрузки источника и символы.</span><span class="sxs-lookup"><span data-stu-id="db2dc-112">Then add a link to [symbolsource.org](http://symbolsource.org) for downloading the source and symbols.</span></span> <span data-ttu-id="db2dc-113">Последовательно выберите пункты **символы** вкладка меню выше и добавьте следующее расположение символов:</span><span class="sxs-lookup"><span data-stu-id="db2dc-113">Go to the **Symbols** tab of the menu above and add the following as a symbol location:</span></span>

```
http://srv.symbolsource.org/pdb/Public
```

<span data-ttu-id="db2dc-114">Кроме того убедитесь, что каталог кэша имеет короткое имя; в противном случае имена файлов можно получить слишком длинное что приведет к символы, не загружаются.</span><span class="sxs-lookup"><span data-stu-id="db2dc-114">In addition, make sure that the cache directory has a short name; otherwise the file names can get too long which will cause the symbols to not load.</span></span> <span data-ttu-id="db2dc-115">Образец пути является:</span><span class="sxs-lookup"><span data-stu-id="db2dc-115">A sample path is:</span></span>

```
C:\SymCache
```

<span data-ttu-id="db2dc-116">Параметры должны выглядеть следующим образом:</span><span class="sxs-lookup"><span data-stu-id="db2dc-116">The settings should look similar to this:</span></span>

![Пример расположения файла параметры символов](_static/SymSource.png)
