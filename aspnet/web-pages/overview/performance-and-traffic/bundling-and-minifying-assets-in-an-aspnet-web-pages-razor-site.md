---
uid: web-pages/overview/performance-and-traffic/bundling-and-minifying-assets-in-an-aspnet-web-pages-razor-site
title: Объединение и Минификация активов в ASP.NET Web Pages (Razor) сайта | Документация Майкрософт
author: microsoft
description: Объединение и Минификация способами для ускорения веб-узла. Объединение позволяет объединить несколько файлов JavaScript (JS) или несколько каскадных таблиц стилей (...)
ms.author: aspnetcontent
ms.date: 06/21/2012
ms.assetid: 8906f1e9-4b66-4a03-8e8a-9e9debf8ed91
msc.legacyurl: /web-pages/overview/performance-and-traffic/bundling-and-minifying-assets-in-an-aspnet-web-pages-razor-site
msc.type: authoredcontent
ms.openlocfilehash: 5799c4a3ae1c36636e0e485361f0f55e62ec7ec7
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 07/05/2018
ms.locfileid: "37813447"
---
<a name="bundling-and-minifying-assets-in-an-aspnet-web-pages-razor-site"></a><span data-ttu-id="ce394-104">Объединение и Минификация активов на сайте ASP.NET Web Pages (Razor)</span><span class="sxs-lookup"><span data-stu-id="ce394-104">Bundling and Minifying Assets in an ASP.NET Web Pages (Razor) Site</span></span>
====================
<span data-ttu-id="ce394-105">по [Microsoft](https://github.com/microsoft)</span><span class="sxs-lookup"><span data-stu-id="ce394-105">by [Microsoft](https://github.com/microsoft)</span></span>

> <span data-ttu-id="ce394-106">Объединение и Минификация способами для ускорения веб-узла.</span><span class="sxs-lookup"><span data-stu-id="ce394-106">Bundling and minification are ways to make your site faster.</span></span> <span data-ttu-id="ce394-107">Объединение позволяет объединить несколько JavaScript (*.js*) файлов или несколько каскадных таблиц стилей (*.css*) файлах таким образом, чтобы их можно загрузить как единое целое, а не по одному.</span><span class="sxs-lookup"><span data-stu-id="ce394-107">Bundling lets you combine multiple JavaScript (*.js*) files or multiple cascading style sheet (*.css*) files so that they can be downloaded as a unit, rather than one at a time.</span></span> <span data-ttu-id="ce394-108">Минификация сжимает out пробелы и выполняет другие типы сжатия, чтобы загружены файлы как небольшой возможного.</span><span class="sxs-lookup"><span data-stu-id="ce394-108">Minification squeezes out whitespace and performs other types of compression to make the downloaded files as small a possible.</span></span>
> 
> > [!NOTE]
> > <span data-ttu-id="ce394-109">RC выпуска ASP.NET Web Pages 2 не поддерживает объединение и Минификация, так как пакет, который содержит требуемые элементы еще не доступны в Microsoft WebMatrix.</span><span class="sxs-lookup"><span data-stu-id="ce394-109">The RC release of ASP.NET Web Pages 2 does not support bundling and minification because the package that contains the required elements is not yet available in Microsoft WebMatrix.</span></span> <span data-ttu-id="ce394-110">Приносим извинения за неудобства.</span><span class="sxs-lookup"><span data-stu-id="ce394-110">We apologize for this inconvenience.</span></span> <span data-ttu-id="ce394-111">Пакет должен быть доступен в окончательной версии ASP.NET Web Pages 2 и WebMatrix 2.</span><span class="sxs-lookup"><span data-stu-id="ce394-111">The package is expected to be available in the final release of ASP.NET Web Pages 2 and WebMatrix 2.</span></span>
