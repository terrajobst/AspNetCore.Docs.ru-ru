---
uid: web-forms/videos/aspnet-ajax/how-do-i-choose-between-methods-of-ajax-page-updates
title: '[Инструкции] Выбор метода для AJAX обновлений страниц? | Документы Майкрософт'
author: JoeStagner
description: В этом видео (Joe Stagner) сравнивает два основных способа выполнения обновления страницы в стиле AJAX в приложениях ASP.NET. Первый метод заключается в том, чтобы использовать Upd...
ms.author: riande
ms.date: 07/09/2007
ms.assetid: a5e33a7d-ccb2-483f-a955-3d39f72ba4ec
msc.legacyurl: /web-forms/videos/aspnet-ajax/how-do-i-choose-between-methods-of-ajax-page-updates
msc.type: video
ms.openlocfilehash: 9f146c36ab2225e732a2a35d84bc4e6a616b22d5
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 08/16/2018
ms.locfileid: "41836260"
---
<a name="how-do-i-choose-between-methods-of-ajax-page-updates"></a><span data-ttu-id="24e1f-105">[Инструкции] Выбор метода для AJAX обновлений страниц?</span><span class="sxs-lookup"><span data-stu-id="24e1f-105">[How Do I:] Choose Between Methods of AJAX Page Updates?</span></span>
====================
<span data-ttu-id="24e1f-106">по [(Joe Stagner)](https://github.com/JoeStagner)</span><span class="sxs-lookup"><span data-stu-id="24e1f-106">by [Joe Stagner](https://github.com/JoeStagner)</span></span>

<span data-ttu-id="24e1f-107">В этом видео (Joe Stagner) сравнивает два основных способа выполнения обновления страницы в стиле AJAX в приложениях ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="24e1f-107">In this video Joe Stagner compares the two primary methods of performing AJAX-style page updates in an ASP.NET application.</span></span> <span data-ttu-id="24e1f-108">Первый способ — использовать UpdatePanel, где должна быть записана на стороне клиента, так и на сервере без дополнительного кода.</span><span class="sxs-lookup"><span data-stu-id="24e1f-108">The first method is to use an UpdatePanel, where no additional code needs to be written on either the client side or the server side.</span></span> <span data-ttu-id="24e1f-109">Преимущество использования UpdatePanel является то, что все работает автоматически.</span><span class="sxs-lookup"><span data-stu-id="24e1f-109">The benefit of using the UpdatePanel is that everything works automatically.</span></span> <span data-ttu-id="24e1f-110">Штраф —, на стороне клиента, он требует большого объема данных для включения в AJAX-запрос и ответ, а также на сервере, требуется полный жизненный цикл страницы для выполнения.</span><span class="sxs-lookup"><span data-stu-id="24e1f-110">The penalty is that at the client it requires a lot of data to be included in the AJAX request and response, and at the server it requires a full page lifecycle to be executed.</span></span> <span data-ttu-id="24e1f-111">Второй способ — использовать обратные вызовы по сети, где дополнительный код нужно писать на стороне клиента и на стороне сервера.</span><span class="sxs-lookup"><span data-stu-id="24e1f-111">The second method is to use network callbacks, where additional code needs to be written on both the client side and the server side.</span></span> <span data-ttu-id="24e1f-112">Преимущество использования обратных вызовов по сети — на стороне клиента требует очень мало данных для включения в AJAX-запрос и ответ, что на сервере требуется только метод вызванная служба должна быть выполнена.</span><span class="sxs-lookup"><span data-stu-id="24e1f-112">The benefit of using network callbacks is that at the client it requires very little data to be included in the AJAX request and response, and at the server it requires only the called service method to be executed.</span></span> <span data-ttu-id="24e1f-113">Снижать являются время и усилия, требуемые для записи необходимый код.</span><span class="sxs-lookup"><span data-stu-id="24e1f-113">The penality is the time and effort it takes to write the necessary code.</span></span> <span data-ttu-id="24e1f-114">Джо завершается видео с обсуждения, что следует учитывать, при выборе между два основных метода для обновления страницы в стиле AJAX.</span><span class="sxs-lookup"><span data-stu-id="24e1f-114">Joe concludes the video by discussing what you should consider when choosing between the two primary methods of AJAX-style page updates.</span></span> <span data-ttu-id="24e1f-115">(В этом видео используется код из [как начать работу с помощью ASP.NET AJAX](how-do-i-get-started-with-aspnet-ajax.md) видео и [инструкции сделать клиентские обратные вызовы по сети с помощью ASP.NET AJAX](how-do-i-make-client-side-network-callbacks-with-aspnet-ajax.md) видео.)</span><span class="sxs-lookup"><span data-stu-id="24e1f-115">(This video uses the code from the [How Do I Get Started with ASP.NET AJAX](how-do-i-get-started-with-aspnet-ajax.md) video and the [How Do I Make Client-Side Network Callbacks with ASP.NET AJAX](how-do-i-make-client-side-network-callbacks-with-aspnet-ajax.md) video.)</span></span>

[<span data-ttu-id="24e1f-116">&#9654;Просмотрите видео (11 минут)</span><span class="sxs-lookup"><span data-stu-id="24e1f-116">&#9654; Watch video (11 minutes)</span></span>](https://channel9.msdn.com/Blogs/ASP-NET-Site-Videos/how-do-i-choose-between-methods-of-ajax-page-updates)

> [!div class="step-by-step"]
> <span data-ttu-id="24e1f-117">[Назад](how-do-i-update-multiple-regions-of-a-page-with-aspnet-ajax.md)
> [Вперед](how-do-i-use-other-javascript-user-interface-libraries-with-aspnet-ajax.md)</span><span class="sxs-lookup"><span data-stu-id="24e1f-117">[Previous](how-do-i-update-multiple-regions-of-a-page-with-aspnet-ajax.md)
[Next](how-do-i-use-other-javascript-user-interface-libraries-with-aspnet-ajax.md)</span></span>
