---
uid: web-forms/videos/aspnet-ajax/how-do-i-choose-between-methods-of-ajax-page-updates
title: "[Инструкции:] Выбрать один из методов AJAX для страницы обновления? | Документы Майкрософт"
author: JoeStagner
description: "В этом видеоролике Джо Стэгнер сравнивает два основных способа выполнения обновления страницы в стиле AJAX в приложениях ASP.NET. Первый метод является использование Upd..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/09/2007
ms.topic: article
ms.assetid: a5e33a7d-ccb2-483f-a955-3d39f72ba4ec
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/videos/aspnet-ajax/how-do-i-choose-between-methods-of-ajax-page-updates
msc.type: video
ms.openlocfilehash: cc3721c24c9fed0cb028d755330a5c6189b613b7
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 11/10/2017
---
<a name="how-do-i-choose-between-methods-of-ajax-page-updates"></a><span data-ttu-id="b80d6-105">[Инструкции:] Выбрать один из методов AJAX для страницы обновления?</span><span class="sxs-lookup"><span data-stu-id="b80d6-105">[How Do I:] Choose Between Methods of AJAX Page Updates?</span></span>
====================
<span data-ttu-id="b80d6-106">по [(Joe Stagner)](https://github.com/JoeStagner)</span><span class="sxs-lookup"><span data-stu-id="b80d6-106">by [Joe Stagner](https://github.com/JoeStagner)</span></span>

<span data-ttu-id="b80d6-107">В этом видеоролике Джо Стэгнер сравнивает два основных способа выполнения обновления страницы в стиле AJAX в приложениях ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="b80d6-107">In this video Joe Stagner compares the two primary methods of performing AJAX-style page updates in an ASP.NET application.</span></span> <span data-ttu-id="b80d6-108">Первый метод является использование UpdatePanel, где необходимо записать на стороне клиента, так и на сервере без дополнительного кода.</span><span class="sxs-lookup"><span data-stu-id="b80d6-108">The first method is to use an UpdatePanel, where no additional code needs to be written on either the client side or the server side.</span></span> <span data-ttu-id="b80d6-109">С помощью UpdatePanel преимущество заключается в том, что все работает автоматически.</span><span class="sxs-lookup"><span data-stu-id="b80d6-109">The benefit of using the UpdatePanel is that everything works automatically.</span></span> <span data-ttu-id="b80d6-110">Штраф —, на клиенте, он требует большого объема данных, которые должны быть включены в AJAX запроса и ответа, а также на сервере требуется полный жизненный цикл страницы для выполнения.</span><span class="sxs-lookup"><span data-stu-id="b80d6-110">The penalty is that at the client it requires a lot of data to be included in the AJAX request and response, and at the server it requires a full page lifecycle to be executed.</span></span> <span data-ttu-id="b80d6-111">Второй метод является использование обратных вызовов в сети, где необходимо записать на стороне клиента и стороне сервера дополнительный код.</span><span class="sxs-lookup"><span data-stu-id="b80d6-111">The second method is to use network callbacks, where additional code needs to be written on both the client side and the server side.</span></span> <span data-ttu-id="b80d6-112">Преимущество использования обратных вызовов сети — на стороне клиента требуется очень мало данных должны быть включены в AJAX запроса и ответа, что на сервере требуется только метод вызванная служба должна быть выполнена.</span><span class="sxs-lookup"><span data-stu-id="b80d6-112">The benefit of using network callbacks is that at the client it requires very little data to be included in the AJAX request and response, and at the server it requires only the called service method to be executed.</span></span> <span data-ttu-id="b80d6-113">Penality — это время и усилия, требуемые для записи необходимый код.</span><span class="sxs-lookup"><span data-stu-id="b80d6-113">The penality is the time and effort it takes to write the necessary code.</span></span> <span data-ttu-id="b80d6-114">Сергей заканчивается видео с описания того, что следует учитывать, при выборе между двумя основными способами обновления страницы в стиле AJAX.</span><span class="sxs-lookup"><span data-stu-id="b80d6-114">Joe concludes the video by discussing what you should consider when choosing between the two primary methods of AJAX-style page updates.</span></span> <span data-ttu-id="b80d6-115">(В этом видео будет использовать код из [как начало работы с ASP.NET AJAX](how-do-i-get-started-with-aspnet-ajax.md) видео и [инструкции сделать клиентские обратные вызовы по сети с помощью ASP.NET AJAX](how-do-i-make-client-side-network-callbacks-with-aspnet-ajax.md) видео.)</span><span class="sxs-lookup"><span data-stu-id="b80d6-115">(This video uses the code from the [How Do I Get Started with ASP.NET AJAX](how-do-i-get-started-with-aspnet-ajax.md) video and the [How Do I Make Client-Side Network Callbacks with ASP.NET AJAX](how-do-i-make-client-side-network-callbacks-with-aspnet-ajax.md) video.)</span></span>

[<span data-ttu-id="b80d6-116">&#9654; Посмотрите видео (в минутах 11)</span><span class="sxs-lookup"><span data-stu-id="b80d6-116">&#9654; Watch video (11 minutes)</span></span>](https://channel9.msdn.com/Blogs/ASP-NET-Site-Videos/how-do-i-choose-between-methods-of-ajax-page-updates)

>[!div class="step-by-step"]
<span data-ttu-id="b80d6-117">[Назад](how-do-i-update-multiple-regions-of-a-page-with-aspnet-ajax.md)
[Вперед](how-do-i-use-other-javascript-user-interface-libraries-with-aspnet-ajax.md)</span><span class="sxs-lookup"><span data-stu-id="b80d6-117">[Previous](how-do-i-update-multiple-regions-of-a-page-with-aspnet-ajax.md)
[Next](how-do-i-use-other-javascript-user-interface-libraries-with-aspnet-ajax.md)</span></span>
