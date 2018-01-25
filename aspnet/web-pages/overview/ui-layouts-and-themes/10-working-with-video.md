---
uid: web-pages/overview/ui-layouts-and-themes/10-working-with-video
title: "Отображение видео в ASP.NET Web Pages (Razor) сайта | Документы Microsoft"
author: tfitzmac
description: "В этой главе описывается отображение видео в веб-страниц ASP.NET со страницей синтаксиса Razor."
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/20/2014
ms.topic: article
ms.assetid: 332fb3da-e2a5-460d-bb90-dd911e1e2c95
ms.technology: dotnet-webpages
ms.prod: .net-framework
msc.legacyurl: /web-pages/overview/ui-layouts-and-themes/10-working-with-video
msc.type: authoredcontent
ms.openlocfilehash: a14659997d86d1b5cf5381e21e997c1a03a3f57c
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/24/2018
---
<a name="displaying-video-in-an-aspnet-web-pages-razor-site"></a><span data-ttu-id="b244c-103">Отображение видео в веб-страницы (Razor) узла ASP.NET</span><span class="sxs-lookup"><span data-stu-id="b244c-103">Displaying Video in an ASP.NET Web Pages (Razor) Site</span></span>
====================
<span data-ttu-id="b244c-104">по [Tom FitzMacken](https://github.com/tfitzmac)</span><span class="sxs-lookup"><span data-stu-id="b244c-104">by [Tom FitzMacken](https://github.com/tfitzmac)</span></span>

> <span data-ttu-id="b244c-105">В этой статье описывается использование видеопроигрывателя (носителей) в веб-сайта ASP.NET Web Pages (Razor) для просмотра видео, которые хранятся на сайте.</span><span class="sxs-lookup"><span data-stu-id="b244c-105">This article explains how to use a video (media) player in an ASP.NET Web Pages (Razor) website to let users view videos that are stored on the site.</span></span> <span data-ttu-id="b244c-106">Веб-страниц ASP.NET с синтаксисом Razor позволяет воспроизводить Flash (*.swf*), Media Player (*.wmv*) и Silverlight (*.xap*) видео.</span><span class="sxs-lookup"><span data-stu-id="b244c-106">ASP.NET Web Pages with Razor syntax lets you play Flash (*.swf*), Media Player (*.wmv*), and Silverlight (*.xap*) videos.</span></span>
> 
> <span data-ttu-id="b244c-107">Что вы узнаете следующее.</span><span class="sxs-lookup"><span data-stu-id="b244c-107">What you'll learn:</span></span>
> 
> - <span data-ttu-id="b244c-108">Как выбрать видеопроигрыватель.</span><span class="sxs-lookup"><span data-stu-id="b244c-108">How to choose a video player.</span></span>
> - <span data-ttu-id="b244c-109">Инструкции по добавлению видео на веб-страницу.</span><span class="sxs-lookup"><span data-stu-id="b244c-109">How to add video to a web page.</span></span>
> - <span data-ttu-id="b244c-110">Как задать атрибуты видеопроигрыватель.</span><span class="sxs-lookup"><span data-stu-id="b244c-110">How to set video player attributes.</span></span>
> 
> <span data-ttu-id="b244c-111">Это ASP.NET Razor pages функции, представленные в статье:</span><span class="sxs-lookup"><span data-stu-id="b244c-111">These are the ASP.NET Razor pages features introduced in the article:</span></span>
> 
> - <span data-ttu-id="b244c-112">`Video` Вспомогательные.</span><span class="sxs-lookup"><span data-stu-id="b244c-112">The `Video` helper.</span></span>
>   
> 
> ## <a name="software-versions-used-in-the-tutorial"></a><span data-ttu-id="b244c-113">Версии программного обеспечения, используемая в этом учебнике</span><span class="sxs-lookup"><span data-stu-id="b244c-113">Software versions used in the tutorial</span></span>
> 
> 
> - <span data-ttu-id="b244c-114">Веб-страниц ASP.NET (Razor) 2</span><span class="sxs-lookup"><span data-stu-id="b244c-114">ASP.NET Web Pages (Razor) 2</span></span>
> - <span data-ttu-id="b244c-115">WebMatrix 2</span><span class="sxs-lookup"><span data-stu-id="b244c-115">WebMatrix 2</span></span>
>   
> 
> <span data-ttu-id="b244c-116">Этот учебник также работает с WebMatrix 3.</span><span class="sxs-lookup"><span data-stu-id="b244c-116">This tutorial also works with WebMatrix 3.</span></span>


## <a name="introduction"></a><span data-ttu-id="b244c-117">Вступление</span><span class="sxs-lookup"><span data-stu-id="b244c-117">Introduction</span></span>

<span data-ttu-id="b244c-118">Может потребоваться отобразить видео на сайте.</span><span class="sxs-lookup"><span data-stu-id="b244c-118">You might want to display a video on your site.</span></span> <span data-ttu-id="b244c-119">Один из способов сделать это — для связи с узлом, уже имеет видео, например YouTube.</span><span class="sxs-lookup"><span data-stu-id="b244c-119">One way to do that is to link to a site that already has the video, like YouTube.</span></span> <span data-ttu-id="b244c-120">Если вы хотите внедрить видео из этих сайтов непосредственно в свои собственные страницы, можно обычно получить разметки HTML с сайта и скопировать его на страницу.</span><span class="sxs-lookup"><span data-stu-id="b244c-120">If you want to embed a video from these sites directly in your own pages, you can usually get HTML markup from the site and then copy it into your page.</span></span> <span data-ttu-id="b244c-121">Например приведенный ниже показано, как внедрить YouTube видео:</span><span class="sxs-lookup"><span data-stu-id="b244c-121">For example, the following example shows how to embed a YouTube video:</span></span>

[!code-html[Main](10-working-with-video/samples/sample1.html?highlight=10-14)]

<span data-ttu-id="b244c-122">Если вы хотите воспроизвести видео на собственных веб-сайта (не на общедоступном сайте общий доступ к видео), нельзя связать непосредственно к ней с помощью внедренных разметки следующим образом.</span><span class="sxs-lookup"><span data-stu-id="b244c-122">If you want to play a video that's on your own website (not on a public video-sharing site), you can't directly link to it using embedded markup like this.</span></span> <span data-ttu-id="b244c-123">Тем не менее, воспроизведение видео с веб-узла с помощью `Video` вспомогательного приложения, который отображает проигрыватель непосредственно в страницу.</span><span class="sxs-lookup"><span data-stu-id="b244c-123">However, you can play videos from your site by using the `Video` helper, which renders a media player directly in a page.</span></span>

<a id="Choosing_a_Video_Player"></a>
## <a name="choosing-a-video-player"></a><span data-ttu-id="b244c-124">Выбрав видеопроигрывателем</span><span class="sxs-lookup"><span data-stu-id="b244c-124">Choosing a Video Player</span></span>

<span data-ttu-id="b244c-125">Существует множество форматов видеофайлов, и каждый формат обычно требует разных проигрывателя и другой способ настройки проигрывателя.</span><span class="sxs-lookup"><span data-stu-id="b244c-125">There are lots of formats for video files, and each format typically requires a different player and a different way to configure the player.</span></span> <span data-ttu-id="b244c-126">На страницах ASP.NET Razor можно воспроизводить видео в веб-страницы с помощью `Video` поддержки.</span><span class="sxs-lookup"><span data-stu-id="b244c-126">In ASP.NET Razor pages, you can play a video in a web page using the `Video` helper.</span></span> <span data-ttu-id="b244c-127">`Video` Вспомогательный упрощает процесс внедрения видео на веб-странице, так как Visual Studio автоматически создает `object` и `embed` HTML-элементов, которые обычно используются для добавления видео на страницу.</span><span class="sxs-lookup"><span data-stu-id="b244c-127">The `Video` helper simplifies the process of embedding videos in a web page because it automatically generates the `object` and `embed` HTML elements that are normally used to add video to the page.</span></span>

<span data-ttu-id="b244c-128">`Video` Вспомогательные поддерживает следующие универсальные проигрыватели:</span><span class="sxs-lookup"><span data-stu-id="b244c-128">The `Video` helper supports the following media players:</span></span>

- <span data-ttu-id="b244c-129">Adobe Flash</span><span class="sxs-lookup"><span data-stu-id="b244c-129">Adobe Flash</span></span>
- <span data-ttu-id="b244c-130">Проигрывателя Windows Media</span><span class="sxs-lookup"><span data-stu-id="b244c-130">Windows MediaPlayer</span></span>
- <span data-ttu-id="b244c-131">Microsoft Silverlight</span><span class="sxs-lookup"><span data-stu-id="b244c-131">Microsoft Silverlight</span></span>

### <a name="the-flash-player"></a><span data-ttu-id="b244c-132">Проигрыватель Flash Player</span><span class="sxs-lookup"><span data-stu-id="b244c-132">The Flash Player</span></span>

<span data-ttu-id="b244c-133">`Flash` Игрока `Video` вспомогательный позволяют воспроизводить видео флэш-памяти (*.swf* файлы) на веб-странице.</span><span class="sxs-lookup"><span data-stu-id="b244c-133">The `Flash` player of the `Video` helper let you play Flash videos (*.swf* files) in a web page.</span></span> <span data-ttu-id="b244c-134">Как минимум необходимо указать путь к файлу изображения.</span><span class="sxs-lookup"><span data-stu-id="b244c-134">At a minimum, you have to provide a path to the video file.</span></span> <span data-ttu-id="b244c-135">Если указать только путь проигрыватель использует значения по умолчанию, установленные с текущей версией Flash.</span><span class="sxs-lookup"><span data-stu-id="b244c-135">If you specify nothing but the path, the player uses default values that are set by the current version of Flash.</span></span> <span data-ttu-id="b244c-136">Типичные значения приведены ниже.</span><span class="sxs-lookup"><span data-stu-id="b244c-136">Typical default settings are:</span></span>

- <span data-ttu-id="b244c-137">Видео отображается с использованием высоту и ширину по умолчанию и без цвета фона.</span><span class="sxs-lookup"><span data-stu-id="b244c-137">The video is displayed using its default width and height and without a background color.</span></span>
- <span data-ttu-id="b244c-138">Видео воспроизводится автоматически при загрузке страницы.</span><span class="sxs-lookup"><span data-stu-id="b244c-138">The video plays automatically when the page loads.</span></span>
- <span data-ttu-id="b244c-139">Видео циклов непрерывно, пока не был явно остановлен.</span><span class="sxs-lookup"><span data-stu-id="b244c-139">The video loops continuously until it's explicitly stopped.</span></span>
- <span data-ttu-id="b244c-140">Видео масштабируется для отображения всех видео, а не обрезки видео в соответствии с указанными размерами.</span><span class="sxs-lookup"><span data-stu-id="b244c-140">The video is scaled to show all of the video, rather than cropping the video to fit a specific size.</span></span>
- <span data-ttu-id="b244c-141">Видео воспроизводится в окне.</span><span class="sxs-lookup"><span data-stu-id="b244c-141">The video plays in a window.</span></span>

### <a name="the-mediaplayer-player"></a><span data-ttu-id="b244c-142">Проигрыватель MediaPlayer</span><span class="sxs-lookup"><span data-stu-id="b244c-142">The MediaPlayer Player</span></span>

<span data-ttu-id="b244c-143">`MediaPlayer` Игрока `Video` вспомогательный позволяет воспроизводить видео Windows Media (*.wmv* файлы), Windows Media audio (*.wma* файлы) и MP3 (*.mp3* файлы) на веб-странице.</span><span class="sxs-lookup"><span data-stu-id="b244c-143">The `MediaPlayer` player of the `Video` helper lets you play Windows Media videos (*.wmv* files), Windows Media audio (*.wma* files), and MP3 (*.mp3* files) in a web page.</span></span> <span data-ttu-id="b244c-144">Необходимо включить путь к файлу мультимедиа для воспроизведения; все остальные параметры являются необязательными.</span><span class="sxs-lookup"><span data-stu-id="b244c-144">You must include path of the media file to play; all other parameters are optional.</span></span> <span data-ttu-id="b244c-145">Если указать только путь проигрыватель использует параметры по умолчанию, задать с помощью текущей версии MediaPlayer, например:</span><span class="sxs-lookup"><span data-stu-id="b244c-145">If you specify only a path, the player uses default settings set by the current version of MediaPlayer, such as:</span></span>

- <span data-ttu-id="b244c-146">Видео отображается с использованием высоту и ширину по умолчанию.</span><span class="sxs-lookup"><span data-stu-id="b244c-146">The video is displayed using its default width and height.</span></span>
- <span data-ttu-id="b244c-147">Видео воспроизводится автоматически при загрузке страницы.</span><span class="sxs-lookup"><span data-stu-id="b244c-147">The video plays automatically when the page loads.</span></span>
- <span data-ttu-id="b244c-148">Оно воспроизводится один раз (он не цикла).</span><span class="sxs-lookup"><span data-stu-id="b244c-148">The video plays once (it doesn't loop).</span></span>
- <span data-ttu-id="b244c-149">Проигрыватель отображает полный набор элементов управления в пользовательском интерфейсе.</span><span class="sxs-lookup"><span data-stu-id="b244c-149">The player displays the full set of controls in the user interface.</span></span>
- <span data-ttu-id="b244c-150">Оно воспроизводится в окне.</span><span class="sxs-lookup"><span data-stu-id="b244c-150">The video plays in in a window.</span></span>

### <a name="the-silverlight-player"></a><span data-ttu-id="b244c-151">Проигрыватель Silverlight</span><span class="sxs-lookup"><span data-stu-id="b244c-151">The Silverlight Player</span></span>

<span data-ttu-id="b244c-152">`Silverlight` Игрока `Video` вспомогательный позволяет воспроизводить Windows Media Video (*.wmv* файлы), Windows Media Audio (*.wma* файлы) и MP3 (*.mp3* файлы).</span><span class="sxs-lookup"><span data-stu-id="b244c-152">The `Silverlight` player of the `Video` helper lets you play Windows Media Video (*.wmv* files), Windows Media Audio (*.wma* files), and MP3 (*.mp3* files).</span></span> <span data-ttu-id="b244c-153">Необходимо задать параметр path для указания пакета приложения на основе Silverlight (*.xap* файла).</span><span class="sxs-lookup"><span data-stu-id="b244c-153">You must set the path parameter to point to a Silverlight-based application package (*.xap* file).</span></span> <span data-ttu-id="b244c-154">Также необходимо задать параметры width и height.</span><span class="sxs-lookup"><span data-stu-id="b244c-154">You also must set the width and height parameters.</span></span> <span data-ttu-id="b244c-155">Все остальные параметры являются необязательными.</span><span class="sxs-lookup"><span data-stu-id="b244c-155">All other parameters are optional.</span></span> <span data-ttu-id="b244c-156">При использовании проигрывателя Silverlight для видео, если устанавливается только для обязательных параметров проигрывателя Silverlight отображает видео без цвета фона.</span><span class="sxs-lookup"><span data-stu-id="b244c-156">When you use the Silverlight player for video, if you set only the required parameters, the Silverlight player displays the video without a background color.</span></span>

> [!NOTE]
> <span data-ttu-id="b244c-157">В случае, если вы еще не знаете Silverlight: *.xap* файл представляет собой сжатый файл, содержащий инструкции макета в *.xaml* файла управляемого кода в сборках и дополнительные ресурсы.</span><span class="sxs-lookup"><span data-stu-id="b244c-157">In case you don't already know Silverlight: the *.xap* file is a compressed file that contains layout instructions in a *.xaml* file, managed code in assemblies, and optional resources.</span></span> <span data-ttu-id="b244c-158">Можно создать *.xap* файл в Visual Studio, как проект приложения Silverlight.</span><span class="sxs-lookup"><span data-stu-id="b244c-158">You can create a *.xap* file in Visual Studio as a Silverlight application project.</span></span>


<span data-ttu-id="b244c-159">`Silverlight` Видеопроигрыватель использует как параметры, указываемые для проигрывателя и параметры, которые содержатся в *.xap* файла.</span><span class="sxs-lookup"><span data-stu-id="b244c-159">The `Silverlight` video player uses both the settings that you provide for the player and the settings that are provided in the *.xap* file.</span></span>

> [!TIP] 
> 
> <a id="SB_MimeTypes"></a>
> ### <a name="mime-types"></a><span data-ttu-id="b244c-160">Типы MIME</span><span class="sxs-lookup"><span data-stu-id="b244c-160">MIME Types</span></span>
> 
> <span data-ttu-id="b244c-161">Когда браузер загружает файл, браузер обеспечивает соответствие MIME-тип, указанный для документа, который готовится к просмотру тип файлов.</span><span class="sxs-lookup"><span data-stu-id="b244c-161">When a browser downloads a file, the browser makes sure that the file type matches the MIME type that's specified for the document that's being rendered.</span></span> <span data-ttu-id="b244c-162">MIME-тип является типом или носителя тип содержимого файла.</span><span class="sxs-lookup"><span data-stu-id="b244c-162">The MIME type is the content type or media type of a file.</span></span> <span data-ttu-id="b244c-163">`Video` Вспомогательные используются следующие типы MIME:</span><span class="sxs-lookup"><span data-stu-id="b244c-163">The `Video` helper uses the following MIME types:</span></span>
> 
> - `application/x-shockwave-flash`
> - `application/x-mplayer2`
> - `application/x-silverlight-2`


<a id="Playing_Flash"></a>
## <a name="playing-flash-swf-videos"></a><span data-ttu-id="b244c-164">Воспроизведение видео Flash (.swf)</span><span class="sxs-lookup"><span data-stu-id="b244c-164">Playing Flash (.swf) Videos</span></span>

<span data-ttu-id="b244c-165">Эта процедура показано, как для воспроизведения видео флэш-памяти с именем *sample.swf*.</span><span class="sxs-lookup"><span data-stu-id="b244c-165">This procedure shows you how to play a Flash video named *sample.swf*.</span></span> <span data-ttu-id="b244c-166">В процедуре предполагается, что у вас есть папка с именем *мультимедиа* на веб-узла и что *.swf* файл находится в этой папке.</span><span class="sxs-lookup"><span data-stu-id="b244c-166">The procedure assumes that you've got a folder named *Media* on your site and that the *.swf* file is in that folder.</span></span>

1. <span data-ttu-id="b244c-167">Добавить библиотека вспомогательных методов ASP.NET Web веб-сайта, как описано в [Установка помощников в узел веб-страниц ASP.NET](https://go.microsoft.com/fwlink/?LinkId=252372), если не было добавлено.</span><span class="sxs-lookup"><span data-stu-id="b244c-167">Add the ASP.NET Web Helpers Library to your website as described in [Installing Helpers in an ASP.NET Web Pages Site](https://go.microsoft.com/fwlink/?LinkId=252372), if you haven't already added it.</span></span>
2. <span data-ttu-id="b244c-168">Веб-сайта, добавьте страницы и назовите его *FlashVideo.cshtml*.</span><span class="sxs-lookup"><span data-stu-id="b244c-168">In the website, add a page and name it *FlashVideo.cshtml*.</span></span>
3. <span data-ttu-id="b244c-169">Добавьте следующую разметку страницы:</span><span class="sxs-lookup"><span data-stu-id="b244c-169">Add the following markup to the page:</span></span> 

    [!code-cshtml[Main](10-working-with-video/samples/sample2.cshtml)]
4. <span data-ttu-id="b244c-170">Запустите страницу в браузере.</span><span class="sxs-lookup"><span data-stu-id="b244c-170">Run the page in a browser.</span></span> <span data-ttu-id="b244c-171">(Убедитесь, что выбран страницы **файлы** рабочей области, прежде чем запускать его.) Эта страница отображается, видео воспроизводится автоматически.</span><span class="sxs-lookup"><span data-stu-id="b244c-171">(Make sure the page is selected in the **Files** workspace before you run it.) The page is displayed and the video plays automatically.</span></span> 

    <span data-ttu-id="b244c-172">![[image]](10-working-with-video/_static/image1.jpg "ch08_video-1.jpg")</span><span class="sxs-lookup"><span data-stu-id="b244c-172">![[image]](10-working-with-video/_static/image1.jpg "ch08_video-1.jpg")</span></span>

<span data-ttu-id="b244c-173">Можно задать `quality` для Flash-видео, чтобы параметр `low`, `autolow`, `autohigh`, `medium`, `high`, и `best`:</span><span class="sxs-lookup"><span data-stu-id="b244c-173">You can set the `quality` parameter for a Flash video to `low`, `autolow`, `autohigh`, `medium`, `high`, and `best`:</span></span>

[!code-cshtml[Main](10-working-with-video/samples/sample3.cshtml)]

<span data-ttu-id="b244c-174">Вы можете изменить Flash-видео для воспроизведения на определенного размера с помощью `scale` параметр, который можно задать следующее:</span><span class="sxs-lookup"><span data-stu-id="b244c-174">You can change the Flash video to play at a specific size using the `scale` parameter, which you can set to the following:</span></span>

- <span data-ttu-id="b244c-175">`showall`.</span><span class="sxs-lookup"><span data-stu-id="b244c-175">`showall`.</span></span> <span data-ttu-id="b244c-176">Это делает видимыми видео целиком сохранением первоначальных пропорций.</span><span class="sxs-lookup"><span data-stu-id="b244c-176">This makes the entire video visible while maintaining the original aspect ratio.</span></span> <span data-ttu-id="b244c-177">Тем не менее вы может оказаться границы для каждой стороны.</span><span class="sxs-lookup"><span data-stu-id="b244c-177">However, you might end up with borders on each side.</span></span>
- <span data-ttu-id="b244c-178">`noorder`.</span><span class="sxs-lookup"><span data-stu-id="b244c-178">`noorder`.</span></span> <span data-ttu-id="b244c-179">При этом размер видео, сохраняя исходные пропорции, но может обрезаться.</span><span class="sxs-lookup"><span data-stu-id="b244c-179">This scales the video while maintaining the original aspect ratio, but it might be cropped.</span></span>
- <span data-ttu-id="b244c-180">`exactfit`.</span><span class="sxs-lookup"><span data-stu-id="b244c-180">`exactfit`.</span></span> <span data-ttu-id="b244c-181">Это делает все видео видимыми не сохраняя исходные пропорции, но может происходить искажение.</span><span class="sxs-lookup"><span data-stu-id="b244c-181">This makes the entire video visible without preserving the original aspect ratio, but distortion may occur.</span></span>

<span data-ttu-id="b244c-182">Если не указать `scale` параметр, все видео будут отображаться и будет поддерживать исходные пропорции без всякой обрезки.</span><span class="sxs-lookup"><span data-stu-id="b244c-182">If you don't specify a `scale` parameter, the entire video will be visible and the original aspect ratio will be maintained without any cropping.</span></span> <span data-ttu-id="b244c-183">В следующем примере показано, как использовать `scale` параметр:</span><span class="sxs-lookup"><span data-stu-id="b244c-183">The following example shows how to use the `scale` parameter:</span></span>

[!code-cshtml[Main](10-working-with-video/samples/sample4.cshtml)]

<span data-ttu-id="b244c-184">Проигрыватель Flash player поддерживает видео режим параметра `windowMode`.</span><span class="sxs-lookup"><span data-stu-id="b244c-184">The Flash player supports a video mode setting named `windowMode`.</span></span> <span data-ttu-id="b244c-185">Вы можете задать `window`, `opaque`, и `transparent`.</span><span class="sxs-lookup"><span data-stu-id="b244c-185">You can set this to `window`, `opaque`, and `transparent`.</span></span> <span data-ttu-id="b244c-186">По умолчанию `windowMode` равно `window`, отображающий видео в отдельном окне на веб-странице.</span><span class="sxs-lookup"><span data-stu-id="b244c-186">By default, the `windowMode` is set to `window`, which displays the video in a separate window on the web page.</span></span> <span data-ttu-id="b244c-187">`opaque` Параметр скрывает все за видео на веб-странице.</span><span class="sxs-lookup"><span data-stu-id="b244c-187">The `opaque` setting hides everything behind the video on the web page.</span></span> <span data-ttu-id="b244c-188">`transparent` Параметр позволяет фона веб-страницы видны сквозь видео, при условии, что какой-либо части видео является прозрачным.</span><span class="sxs-lookup"><span data-stu-id="b244c-188">The `transparent` setting lets the background of the web page show through the video, assuming any part of the video is transparent.</span></span>

<a id="Playing_MediaPlayer"></a>
## <a name="playing-mediaplayer-wmv-videos"></a><span data-ttu-id="b244c-189">Воспроизведение MediaPlayer (*.wmv*) видео</span><span class="sxs-lookup"><span data-stu-id="b244c-189">Playing MediaPlayer (*.wmv*) Videos</span></span>

<span data-ttu-id="b244c-190">Ниже показано, как для воспроизведения видео окна носителя с именем *sample.wmv* , находящийся в *Media* папки.</span><span class="sxs-lookup"><span data-stu-id="b244c-190">The following procedure shows you how to play a Window Media video named *sample.wmv* that's in the *Media* folder.</span></span>

1. <span data-ttu-id="b244c-191">Добавить библиотека вспомогательных методов ASP.NET Web веб-сайта, как описано в [Установка помощников в узел веб-страниц ASP.NET](https://go.microsoft.com/fwlink/?LinkId=252372), если это еще не сделано.</span><span class="sxs-lookup"><span data-stu-id="b244c-191">Add the ASP.NET Web Helpers Library to your website as described in [Installing Helpers in an ASP.NET Web Pages Site](https://go.microsoft.com/fwlink/?LinkId=252372), if you haven't already.</span></span>
2. <span data-ttu-id="b244c-192">Создание новой страницы с именем *MediaPlayerVideo.cshtml*.</span><span class="sxs-lookup"><span data-stu-id="b244c-192">Create a new page named *MediaPlayerVideo.cshtml*.</span></span>
3. <span data-ttu-id="b244c-193">Добавьте следующую разметку страницы:</span><span class="sxs-lookup"><span data-stu-id="b244c-193">Add the following markup to the page:</span></span> 

    [!code-cshtml[Main](10-working-with-video/samples/sample5.cshtml)]
4. <span data-ttu-id="b244c-194">Запустите страницу в браузере.</span><span class="sxs-lookup"><span data-stu-id="b244c-194">Run the page in a browser.</span></span> <span data-ttu-id="b244c-195">Видео загружает и воспроизводит автоматически.</span><span class="sxs-lookup"><span data-stu-id="b244c-195">The video loads and plays automatically.</span></span> 

    <span data-ttu-id="b244c-196">![[image]](10-working-with-video/_static/image2.jpg "ch08_video-2.jpg")</span><span class="sxs-lookup"><span data-stu-id="b244c-196">![[image]](10-working-with-video/_static/image2.jpg "ch08_video-2.jpg")</span></span>

<span data-ttu-id="b244c-197">Можно задать `playCount` в целое число, указывающее, сколько раз для автоматического воспроизведения видео:</span><span class="sxs-lookup"><span data-stu-id="b244c-197">You can set `playCount` to an integer that indicates how many times to play the video automatically:</span></span>

[!code-cshtml[Main](10-working-with-video/samples/sample6.cshtml)]

<span data-ttu-id="b244c-198">`uiMode` Позволяет указать, какие элементы управления отображаются в пользовательском интерфейсе.</span><span class="sxs-lookup"><span data-stu-id="b244c-198">The `uiMode` parameter lets you specify which controls show up in the user interface.</span></span> <span data-ttu-id="b244c-199">Можно задать `uiMode` для `invisible`, `none`, `mini`, или `full`.</span><span class="sxs-lookup"><span data-stu-id="b244c-199">You can set `uiMode` to `invisible`, `none`, `mini`, or `full`.</span></span> <span data-ttu-id="b244c-200">Если не указать `uiMode` параметра, будет видео отображается в окне состояния, поиска панели управления кнопок и громкости Помимо окна видео.</span><span class="sxs-lookup"><span data-stu-id="b244c-200">If you don't specify a `uiMode` parameter, the video will be displayed with the status window, seek bar, control buttons, and volume controls in addition to the video window.</span></span> <span data-ttu-id="b244c-201">Эти элементы управления будет отображаться также в том случае, если использовать проигрыватель для воспроизведения звукового файла.</span><span class="sxs-lookup"><span data-stu-id="b244c-201">These controls will also be displayed if you use the player to play an audio file.</span></span> <span data-ttu-id="b244c-202">Ниже приведен пример использования `uiMode` параметр:</span><span class="sxs-lookup"><span data-stu-id="b244c-202">Here's an example of how to use the `uiMode` parameter:</span></span>

[!code-cshtml[Main](10-working-with-video/samples/sample7.cshtml)]

<span data-ttu-id="b244c-203">По умолчанию аудио включено при воспроизведении видео.</span><span class="sxs-lookup"><span data-stu-id="b244c-203">By default, audio is on when the video plays.</span></span> <span data-ttu-id="b244c-204">Отключение звука аудио, задав `mute` параметр в значение true:</span><span class="sxs-lookup"><span data-stu-id="b244c-204">You can mute the audio by setting the `mute` parameter to true:</span></span>

[!code-cshtml[Main](10-working-with-video/samples/sample8.cshtml)]

<span data-ttu-id="b244c-205">Можно задать уровень аудио, видео MediaPlayer установив `volume` параметр значение в диапазоне от 0 до 100.</span><span class="sxs-lookup"><span data-stu-id="b244c-205">You can control the audio level of the MediaPlayer video by setting the `volume` parameter to a value between 0 and 100.</span></span> <span data-ttu-id="b244c-206">Значение по умолчанию — 50.</span><span class="sxs-lookup"><span data-stu-id="b244c-206">The default value is 50.</span></span> <span data-ttu-id="b244c-207">Ниже приведен пример:</span><span class="sxs-lookup"><span data-stu-id="b244c-207">Here's an example:</span></span>

[!code-cshtml[Main](10-working-with-video/samples/sample9.cshtml)]

<a id="Playing_Silverlight"></a>
## <a name="playing-silverlight-videos"></a><span data-ttu-id="b244c-208">Воспроизведение видео Silverlight</span><span class="sxs-lookup"><span data-stu-id="b244c-208">Playing Silverlight Videos</span></span>

<span data-ttu-id="b244c-209">Эта процедура показано, как воспроизвести видео, содержащихся в Silverlight *.xap* страницу, которая находится в папке с именем *носителя*.</span><span class="sxs-lookup"><span data-stu-id="b244c-209">This procedure shows you how to play video contained in a Silverlight *.xap* page that's in a folder named *Media*.</span></span>

1. <span data-ttu-id="b244c-210">Добавить библиотека вспомогательных методов ASP.NET Web веб-сайта, как описано в [Установка помощников в узел веб-страниц ASP.NET](https://go.microsoft.com/fwlink/?LinkId=252372), если это еще не сделано.</span><span class="sxs-lookup"><span data-stu-id="b244c-210">Add the ASP.NET Web Helpers Library to your website as described in [Installing Helpers in an ASP.NET Web Pages Site](https://go.microsoft.com/fwlink/?LinkId=252372), if you haven't already .</span></span>
2. <span data-ttu-id="b244c-211">Создание новой страницы с именем *SilverlightVideo.cshtml*.</span><span class="sxs-lookup"><span data-stu-id="b244c-211">Create a new page named *SilverlightVideo.cshtml*.</span></span>
3. <span data-ttu-id="b244c-212">Добавьте следующую разметку страницы:</span><span class="sxs-lookup"><span data-stu-id="b244c-212">Add the following markup to the page:</span></span> 

    [!code-cshtml[Main](10-working-with-video/samples/sample10.cshtml)]
4. <span data-ttu-id="b244c-213">Запустите страницу в браузере.</span><span class="sxs-lookup"><span data-stu-id="b244c-213">Run the page in a browser.</span></span> 

    <span data-ttu-id="b244c-214">![[image]](10-working-with-video/_static/image3.jpg "ch08_video-3.jpg")</span><span class="sxs-lookup"><span data-stu-id="b244c-214">![[image]](10-working-with-video/_static/image3.jpg "ch08_video-3.jpg")</span></span>

<a id="Additional_Resources"></a>
## <a name="additional-resources"></a><span data-ttu-id="b244c-215">Дополнительные ресурсы</span><span class="sxs-lookup"><span data-stu-id="b244c-215">Additional Resources</span></span>


<span data-ttu-id="b244c-216">[Общие сведения об Silverlight](https://msdn.microsoft.com/library/bb404700(VS.95).aspx)</span><span class="sxs-lookup"><span data-stu-id="b244c-216">[Silverlight Overview](https://msdn.microsoft.com/library/bb404700(VS.95).aspx)</span></span>

[<span data-ttu-id="b244c-217">Атрибуты тега флэш-памяти ОБЪЕКТА и ВНЕДРЕНИЯ</span><span class="sxs-lookup"><span data-stu-id="b244c-217">Flash OBJECT and EMBED tag attributes</span></span>](http://kb2.adobe.com/cps/127/tn_12701.html)

<span data-ttu-id="b244c-218">[Windows Media Player 11 SDK PARAM теги](https://msdn.microsoft.com/library/aa392321(VS.85).aspx)</span><span class="sxs-lookup"><span data-stu-id="b244c-218">[Windows Media Player 11 SDK PARAM Tags](https://msdn.microsoft.com/library/aa392321(VS.85).aspx)</span></span>
