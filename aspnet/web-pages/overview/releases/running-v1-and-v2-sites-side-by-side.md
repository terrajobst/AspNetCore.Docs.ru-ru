---
uid: web-pages/overview/releases/running-v1-and-v2-sites-side-by-side
title: Под управлением разных версий веб-страниц ASP.NET (Razor) параллельно | Документация Майкрософт
author: tfitzmac
description: В этой статье объясняется, как запуск веб-сайтов ASP.NET Web Pages (Razor) на том же компьютере или сервере, когда веб-сайты настроены для использования разных версий...
ms.author: riande
ms.date: 02/10/2014
ms.assetid: a861409b-4ae6-4868-9e09-87edfac3535f
msc.legacyurl: /web-pages/overview/releases/running-v1-and-v2-sites-side-by-side
msc.type: authoredcontent
ms.openlocfilehash: 9021f9b7a68b8b20f7f2fbcd5649cc7226401a1b
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 08/16/2018
ms.locfileid: "41836888"
---
<a name="running-different-versions-of-aspnet-web-pages-razor-side-by-side"></a><span data-ttu-id="c1540-103">Рядом друг с другом под управлением разных версий веб-страниц ASP.NET (Razor)</span><span class="sxs-lookup"><span data-stu-id="c1540-103">Running Different Versions of ASP.NET Web Pages (Razor) Side by Side</span></span>
====================
<span data-ttu-id="c1540-104">по [Tom FitzMacken](https://github.com/tfitzmac)</span><span class="sxs-lookup"><span data-stu-id="c1540-104">by [Tom FitzMacken](https://github.com/tfitzmac)</span></span>

> <span data-ttu-id="c1540-105">В этой статье объясняется, как запустить веб-сайтов ASP.NET Web Pages (Razor) на том же компьютере или сервере, когда веб-сайты настроены для использования разных версий веб-страниц ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="c1540-105">This article explains how to run ASP.NET Web Pages (Razor) websites on the same computer or server when the websites are configured to use different versions of ASP.NET Web Pages.</span></span>
> 
> <span data-ttu-id="c1540-106">Вы узнаете, как:</span><span class="sxs-lookup"><span data-stu-id="c1540-106">What you'll learn:</span></span>
> 
> - <span data-ttu-id="c1540-107">Что по умолчанию выполняется в ASP.NET при наличии сайтов, созданных с помощью веб-страниц ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="c1540-107">What the default behavior is in ASP.NET when you have sites built with ASP.NET Web Pages.</span></span>
> - <span data-ttu-id="c1540-108">Сведения о настройке нового сайта для запуска с более старой версии веб-страниц ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="c1540-108">How to configure a new site to run with an older version of ASP.NET Web Pages.</span></span>
>   
> 
> <span data-ttu-id="c1540-109">Это функция ASP.NET, впервые представленная в этой статье:</span><span class="sxs-lookup"><span data-stu-id="c1540-109">This is the ASP.NET feature introduced in the article:</span></span>
> 
> - <span data-ttu-id="c1540-110">`webPages:Version` Параметр конфигурации.</span><span class="sxs-lookup"><span data-stu-id="c1540-110">The `webPages:Version` configuration setting.</span></span>
>   
> 
> ## <a name="software-versions"></a><span data-ttu-id="c1540-111">Версии программного обеспечения</span><span class="sxs-lookup"><span data-stu-id="c1540-111">Software versions</span></span>
> 
> 
> - <span data-ttu-id="c1540-112">Веб-страниц ASP.NET (Razor) 3</span><span class="sxs-lookup"><span data-stu-id="c1540-112">ASP.NET Web Pages (Razor) 3</span></span>
>   
> 
> <span data-ttu-id="c1540-113">Этот учебник также работает с 2 веб-страниц ASP.NET и веб-страниц ASP.NET 1.0.</span><span class="sxs-lookup"><span data-stu-id="c1540-113">This tutorial also works with ASP.NET Web Pages 2 and ASP.NET Web Pages 1.0.</span></span>


<span data-ttu-id="c1540-114">Веб-страницы ASP.NET поддерживает возможность запуска веб-сайтов рядом друг с другом.</span><span class="sxs-lookup"><span data-stu-id="c1540-114">ASP.NET Web Pages supports the ability to run websites side by side.</span></span> <span data-ttu-id="c1540-115">Это позволяет продолжать использовать имеющиеся приложения веб-страниц ASP.NET, создания новых приложений, веб-страниц ASP.NET и запустить все из них на одном компьютере.</span><span class="sxs-lookup"><span data-stu-id="c1540-115">This lets you continue to run your older ASP.NET Web Pages applications, build new ASP.NET Web Pages applications, and run all of them on the same computer.</span></span>

<span data-ttu-id="c1540-116">Ниже приведены некоторые рекомендации по установке веб-страниц с помощью WebMatrix.</span><span class="sxs-lookup"><span data-stu-id="c1540-116">Here are some things to remember when you install the Web Pages with WebMatrix:</span></span>

- <span data-ttu-id="c1540-117">По умолчанию существующие веб-страницы приложения будут работать как последнюю версию на компьютере.</span><span class="sxs-lookup"><span data-stu-id="c1540-117">By default, existing Web Pages applications will run as the latest version on your computer.</span></span> <span data-ttu-id="c1540-118">(Сборки устанавливаются в глобальный кэш сборок (GAC) и используются автоматически.)</span><span class="sxs-lookup"><span data-stu-id="c1540-118">(The assemblies are installed in the global assembly cache (GAC) and are used automatically.)</span></span>
- <span data-ttu-id="c1540-119">Если вы хотите запустить сайт с помощью другой версии веб-страниц ASP.NET, можно настроить для этого сайта.</span><span class="sxs-lookup"><span data-stu-id="c1540-119">If you want to run a site using a different version of ASP.NET Web Pages, you can configure the site to do that.</span></span> <span data-ttu-id="c1540-120">Если узел уже не содержит *web.config* файл в корневом каталоге сайта, создайте его и скопируйте следующий код XML, перезаписав существующее содержимое.</span><span class="sxs-lookup"><span data-stu-id="c1540-120">If your site doesn't already have a *web.config* file in the root of the site, create a new one and copy the following XML into it, overwriting the existing content.</span></span> <span data-ttu-id="c1540-121">Если сайт уже содержит *web.config* добавьте `<appSettings>` элемент, аналогичный приведенному ниже, чтобы `<configuration>` раздел.</span><span class="sxs-lookup"><span data-stu-id="c1540-121">If the site already contains a *web.config* file, add an `<appSettings>` element like the following one to the `<configuration>` section.</span></span>

    [!code-xml[Main](running-v1-and-v2-sites-side-by-side/samples/sample1.xml)]
  <span data-ttu-id="c1540-122">"— Если не указать версию в *web.config* файл, сайт развертывается в качестве последней версии.</span><span class="sxs-lookup"><span data-stu-id="c1540-122">\`- If you do not specify a version in the *web.config* file, a site is deployed as the latest version.</span></span> <span data-ttu-id="c1540-123">(Сборки копируются в *bin* папку развернутого веб-сайта.)</span><span class="sxs-lookup"><span data-stu-id="c1540-123">(The assemblies are copied to the *bin* folder in the deployed site.)</span></span>
- <span data-ttu-id="c1540-124">Новые приложения, создаваемые с помощью шаблонов узлов в Web Matrix включают сборки версии веб-страниц на сайте *bin* папки.</span><span class="sxs-lookup"><span data-stu-id="c1540-124">New applications that you create using the site templates in Web Matrix include the Web Pages version assemblies in the site's *bin* folder.</span></span>

<span data-ttu-id="c1540-125">Как правило, всегда можно управлять какую версию веб-страниц для использования с веб-узла с помощью NuGet для установки на соответствующие сборки на веб-узел *bin* папки.</span><span class="sxs-lookup"><span data-stu-id="c1540-125">In general, you can always control which version of Web Pages to use with your site by using NuGet to install the appropriate assemblies into the site's *bin* folder.</span></span> <span data-ttu-id="c1540-126">Чтобы найти пакеты, посетите [NuGet.org](http://NuGet.org).</span><span class="sxs-lookup"><span data-stu-id="c1540-126">To find packages, visit [NuGet.org](http://NuGet.org).</span></span>

## <a name="additional-resources"></a><span data-ttu-id="c1540-127">Дополнительные ресурсы</span><span class="sxs-lookup"><span data-stu-id="c1540-127">Additional Resources</span></span>

[<span data-ttu-id="c1540-128">Лучшие компоненты веб-страниц ASP.NET 2</span><span class="sxs-lookup"><span data-stu-id="c1540-128">The Top Features in ASP.NET Web Pages 2</span></span>](top-features-in-web-pages-2.md)
