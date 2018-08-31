---
title: Получение библиотеки на стороне клиента в ASP.NET Core с LibMan
author: scottaddie
description: Узнайте, как установить ресурсы библиотеки на стороне клиента в проекте ASP.NET Core с помощью диспетчера библиотек (LibMan).
ms.author: scaddie
ms.custom: mvc
ms.date: 08/14/2018
uid: client-side/libman/index
ms.openlocfilehash: a6ff0cc3342cfac74739387aa17046ed5050232f
ms.sourcegitcommit: ecf2cd4e0613569025b28e12de3baa21d86d4258
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 08/30/2018
ms.locfileid: "43312363"
---
# <a name="client-side-library-acquisition-in-aspnet-core-with-libman"></a><span data-ttu-id="b3643-103">Получение библиотеки на стороне клиента в ASP.NET Core с LibMan</span><span class="sxs-lookup"><span data-stu-id="b3643-103">Client-side library acquisition in ASP.NET Core with LibMan</span></span>

<span data-ttu-id="b3643-104">Автор: [Скотт Адди](https://twitter.com/Scott_Addie) (Scott Addie)</span><span class="sxs-lookup"><span data-stu-id="b3643-104">By [Scott Addie](https://twitter.com/Scott_Addie)</span></span>

<span data-ttu-id="b3643-105">Диспетчер библиотек (LibMan) — это облегченное средство получения библиотек на стороне клиента.</span><span class="sxs-lookup"><span data-stu-id="b3643-105">Library Manager (LibMan) is a lightweight, client-side library acquisition tool.</span></span> <span data-ttu-id="b3643-106">LibMan загружает популярные библиотеки и платформы из файловой системы или из [сети доставки содержимого (CDN)](https://wikipedia.org/wiki/Content_delivery_network).</span><span class="sxs-lookup"><span data-stu-id="b3643-106">LibMan downloads popular libraries and frameworks from the file system or from a [content delivery network (CDN)](https://wikipedia.org/wiki/Content_delivery_network).</span></span> <span data-ttu-id="b3643-107">Поддерживаются сети доставки содержимого [CDNJS](https://cdnjs.com/) и [unpkg](https://unpkg.com/#/).</span><span class="sxs-lookup"><span data-stu-id="b3643-107">The supported CDNs include [CDNJS](https://cdnjs.com/) and [unpkg](https://unpkg.com/#/).</span></span> <span data-ttu-id="b3643-108">Выбранные файлы библиотеки извлекаются и размещаются в соответствующем месте в проекте ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="b3643-108">The selected library files are fetched and placed in the appropriate location within the ASP.NET Core project.</span></span>

## <a name="libman-use-cases"></a><span data-ttu-id="b3643-109">Варианты использования LibMan</span><span class="sxs-lookup"><span data-stu-id="b3643-109">LibMan use cases</span></span>

<span data-ttu-id="b3643-110">LibMan предоставляет следующие преимущества:</span><span class="sxs-lookup"><span data-stu-id="b3643-110">LibMan offers the following benefits:</span></span>

* <span data-ttu-id="b3643-111">Загружаются только необходимые файлы библиотеки.</span><span class="sxs-lookup"><span data-stu-id="b3643-111">Only the library files you need are downloaded.</span></span>
* <span data-ttu-id="b3643-112">Дополнительные средства, такие как [Node.js](https://nodejs.org), [npm](https://www.npmjs.com) и [WebPack](https://webpack.js.org), не обязательны для получения подмножества файлов в библиотеке.</span><span class="sxs-lookup"><span data-stu-id="b3643-112">Additional tooling, such as [Node.js](https://nodejs.org), [npm](https://www.npmjs.com), and [WebPack](https://webpack.js.org), isn't necessary to acquire a subset of files in a library.</span></span>
* <span data-ttu-id="b3643-113">Файлы можно разместить в определенном расположении, не прибегая к задачам сборки или копированию файлов вручную.</span><span class="sxs-lookup"><span data-stu-id="b3643-113">Files can be placed in a specific location without resorting to build tasks or manual file copying.</span></span>

<span data-ttu-id="b3643-114">Дополнительные сведения о преимуществах LibMan смотрите в видео [Разработка современного веб-интерфейса в Visual Studio 2017: сегмент LibMan](https://channel9.msdn.com/Events/Build/2017/B8073#time=43m34s).</span><span class="sxs-lookup"><span data-stu-id="b3643-114">For more information about LibMan's benefits, watch [Modern front-end web development in Visual Studio 2017: LibMan segment](https://channel9.msdn.com/Events/Build/2017/B8073#time=43m34s).</span></span>

<span data-ttu-id="b3643-115">LibMan не является системой управления пакетами.</span><span class="sxs-lookup"><span data-stu-id="b3643-115">LibMan isn't a package management system.</span></span> <span data-ttu-id="b3643-116">Если вы уже используете диспетчер пакетов, например npm или [yarn](https://yarnpkg.com), используйте его и дальше.</span><span class="sxs-lookup"><span data-stu-id="b3643-116">If you're already using a package manager, such as npm or [yarn](https://yarnpkg.com), continue doing so.</span></span> <span data-ttu-id="b3643-117">LibMan не предназначен для замены этих средств.</span><span class="sxs-lookup"><span data-stu-id="b3643-117">LibMan wasn't developed to replace those tools.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="b3643-118">Дополнительные ресурсы</span><span class="sxs-lookup"><span data-stu-id="b3643-118">Additional resources</span></span>

* <xref:client-side/libman/libman-vs>
* <xref:client-side/libman/libman-cli>
* [<span data-ttu-id="b3643-119">Репозиторий LibMan на GitHub</span><span class="sxs-lookup"><span data-stu-id="b3643-119">LibMan GitHub repository</span></span>](https://github.com/aspnet/LibraryManager)
