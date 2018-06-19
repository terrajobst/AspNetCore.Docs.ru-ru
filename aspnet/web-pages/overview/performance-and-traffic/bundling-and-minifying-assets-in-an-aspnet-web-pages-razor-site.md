---
uid: web-pages/overview/performance-and-traffic/bundling-and-minifying-assets-in-an-aspnet-web-pages-razor-site
title: Объединение и Минификация средств в ASP.NET Web Pages (Razor) сайта | Документы Microsoft
author: microsoft
description: Объединении и иными способами для ускорения веб-узла. Объединение позволяет объединять несколько файлов JavaScript (JS-) или несколько таблицы каскадных стилей (...
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/21/2012
ms.topic: article
ms.assetid: 8906f1e9-4b66-4a03-8e8a-9e9debf8ed91
ms.technology: dotnet-webpages
ms.prod: .net-framework
msc.legacyurl: /web-pages/overview/performance-and-traffic/bundling-and-minifying-assets-in-an-aspnet-web-pages-razor-site
msc.type: authoredcontent
ms.openlocfilehash: df00b5c9e741e429011143d04121df97c1930602
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 11/10/2017
ms.locfileid: "26528553"
---
<a name="bundling-and-minifying-assets-in-an-aspnet-web-pages-razor-site"></a><span data-ttu-id="258da-104">Объединение и Минификация активов на сайте ASP.NET Web Pages (Razor)</span><span class="sxs-lookup"><span data-stu-id="258da-104">Bundling and Minifying Assets in an ASP.NET Web Pages (Razor) Site</span></span>
====================
<span data-ttu-id="258da-105">по [Microsoft](https://github.com/microsoft)</span><span class="sxs-lookup"><span data-stu-id="258da-105">by [Microsoft](https://github.com/microsoft)</span></span>

> <span data-ttu-id="258da-106">Объединении и иными способами для ускорения веб-узла.</span><span class="sxs-lookup"><span data-stu-id="258da-106">Bundling and minification are ways to make your site faster.</span></span> <span data-ttu-id="258da-107">Объединение позволяет объединить несколько JavaScript (*.js*) файлов или нескольких таблицы каскадных стилей (*.css*) файлы, чтобы их можно загрузить как единое целое, а не по одному.</span><span class="sxs-lookup"><span data-stu-id="258da-107">Bundling lets you combine multiple JavaScript (*.js*) files or multiple cascading style sheet (*.css*) files so that they can be downloaded as a unit, rather than one at a time.</span></span> <span data-ttu-id="258da-108">Иными сжимает out пробелов и выполняет другие типы сжатия возможного делать как можно меньше загруженные файлы.</span><span class="sxs-lookup"><span data-stu-id="258da-108">Minification squeezes out whitespace and performs other types of compression to make the downloaded files as small a possible.</span></span>
> 
> > [!NOTE]
> > <span data-ttu-id="258da-109">В выпуске версии-КАНДИДАТА ASP.NET Web Pages 2 не поддерживает объединение и Минификация, так как пакет, содержащий необходимые элементы еще не доступен в Microsoft WebMatrix.</span><span class="sxs-lookup"><span data-stu-id="258da-109">The RC release of ASP.NET Web Pages 2 does not support bundling and minification because the package that contains the required elements is not yet available in Microsoft WebMatrix.</span></span> <span data-ttu-id="258da-110">Приносим извинения за неудобства.</span><span class="sxs-lookup"><span data-stu-id="258da-110">We apologize for this inconvenience.</span></span> <span data-ttu-id="258da-111">Предполагается, что пакет доступен в окончательном выпуске ASP.NET Web Pages 2 и WebMatrix 2.</span><span class="sxs-lookup"><span data-stu-id="258da-111">The package is expected to be available in the final release of ASP.NET Web Pages 2 and WebMatrix 2.</span></span>
