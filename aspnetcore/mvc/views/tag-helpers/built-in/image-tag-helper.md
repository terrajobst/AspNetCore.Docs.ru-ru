---
title: Вспомогательная функция тега изображения в ASP.NET Core
author: pkellner
description: Сведения о работе со вспомогательной функцией тега изображения
manager: wpickett
ms.author: riande
ms.date: 02/14/2017
ms.prod: aspnet-core
ms.technology: aspnet
ms.topic: article
uid: mvc/views/tag-helpers/builtin-th/image-tag-helper
ms.openlocfilehash: 6aa9175f873c4ea62e0319c812e5312cd3331141
ms.sourcegitcommit: 48beecfe749ddac52bc79aa3eb246a2dcdaa1862
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/22/2018
ms.locfileid: "30072266"
---
# <a name="image-tag-helper-in-aspnet-core"></a><span data-ttu-id="cb986-103">Вспомогательная функция тега изображения в ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="cb986-103">Image Tag Helper in ASP.NET Core</span></span>

<span data-ttu-id="cb986-104">Автор: [Питер Кельнер (Peter Kellner)](http://peterkellner.net)</span><span class="sxs-lookup"><span data-stu-id="cb986-104">By [Peter Kellner](http://peterkellner.net)</span></span> 

<span data-ttu-id="cb986-105">Вспомогательная функция тега изображения расширяет возможности тега `img` (`<img>`).</span><span class="sxs-lookup"><span data-stu-id="cb986-105">The Image Tag Helper enhances the `img` (`<img>`) tag.</span></span> <span data-ttu-id="cb986-106">Для нее требуется тег `src`, а также атрибут `asp-append-version` типа `boolean`.</span><span class="sxs-lookup"><span data-stu-id="cb986-106">It requires a `src` tag as well as the `boolean` attribute `asp-append-version`.</span></span>

<span data-ttu-id="cb986-107">Если источник изображения (`src`) представляет собой статический файл на веб-сервере узла, к нему добавляется уникальная строка отключения кэширования в качестве параметра запроса.</span><span class="sxs-lookup"><span data-stu-id="cb986-107">If the image source (`src`) is a static file on the host web server, a unique cache busting string is appended as a query parameter to the image source.</span></span> <span data-ttu-id="cb986-108">Благодаря этому при изменении файла на веб-сервере узла создается уникальный URL-адрес запроса, включающий в себя обновленный параметр запроса.</span><span class="sxs-lookup"><span data-stu-id="cb986-108">This ensures that if the file on the host web server changes, a unique request URL is generated that includes the updated request parameter.</span></span> <span data-ttu-id="cb986-109">Строка отключения кэширования — это уникальное значение, представляющее хэш-код статического файла изображения.</span><span class="sxs-lookup"><span data-stu-id="cb986-109">The cache busting string is a unique value representing the hash of the static image file.</span></span>

<span data-ttu-id="cb986-110">Если источник изображения (`src`) не является статическим файлом (например, это удаленный URL-адрес, либо файла нет на сервере), создается атрибут `src` тега `<img>` без параметра запроса, представляющего строку отключения кэширования.</span><span class="sxs-lookup"><span data-stu-id="cb986-110">If the image source (`src`) isn't a static file (for example a remote URL or the file doesn't exist on the server), the `<img>` tag's `src` attribute is generated with no cache busting query string parameter.</span></span>

## <a name="image-tag-helper-attributes"></a><span data-ttu-id="cb986-111">Атрибуты вспомогательной функции тега изображения</span><span class="sxs-lookup"><span data-stu-id="cb986-111">Image Tag Helper Attributes</span></span>


### <a name="asp-append-version"></a><span data-ttu-id="cb986-112">asp-append-version</span><span class="sxs-lookup"><span data-stu-id="cb986-112">asp-append-version</span></span>

<span data-ttu-id="cb986-113">При указании вместе с атрибутом `src` вызывается вспомогательная функция тега изображения.</span><span class="sxs-lookup"><span data-stu-id="cb986-113">When specified along with a `src` attribute, the Image Tag Helper is invoked.</span></span>

<span data-ttu-id="cb986-114">Пример допустимой функции тега `img`:</span><span class="sxs-lookup"><span data-stu-id="cb986-114">An example of a valid `img` tag helper is:</span></span>

```cshtml
<img src="~/images/asplogo.png" 
    asp-append-version="true"  />
```

<span data-ttu-id="cb986-115">Если статический файл существует в каталоге *..wwwroot/images/asplogo.png*, создаваемый код HTML будет похож на следующий (хэш-код будет иным):</span><span class="sxs-lookup"><span data-stu-id="cb986-115">If the static file exists in the directory *..wwwroot/images/asplogo.png* the generated html is similar to the following (the hash will be different):</span></span>

```html
<img 
    src="/images/asplogo.png?v=Kl_dqr9NVtnMdsM2MUg4qthUnWZm5T1fCEimBPWDNgM"/>
```

<span data-ttu-id="cb986-116">В качестве значения параметру `v` присваивается значение хэша файла на диске.</span><span class="sxs-lookup"><span data-stu-id="cb986-116">The value assigned to the parameter `v` is the hash value of the file on disk.</span></span> <span data-ttu-id="cb986-117">Если веб-серверу не удается получить доступ для чтения к указанному статическому файлу, параметры `v` не добавляются к атрибуту `src`.</span><span class="sxs-lookup"><span data-stu-id="cb986-117">If the web server is unable to obtain read access to the static file referenced,  no `v` parameters is added to the `src` attribute.</span></span>

- - -

### <a name="src"></a><span data-ttu-id="cb986-118">src</span><span class="sxs-lookup"><span data-stu-id="cb986-118">src</span></span>

<span data-ttu-id="cb986-119">Для активации вспомогательной функции тега изображения требуется атрибут src в элементе `<img>`.</span><span class="sxs-lookup"><span data-stu-id="cb986-119">To activate the Image Tag Helper, the src attribute is required on the `<img>` element.</span></span> 

> [!NOTE]
> <span data-ttu-id="cb986-120">Вспомогательная функция тега изображения использует поставщик `Cache` на локальном веб-сервере для хранения значения `Sha512`, вычисленного для данного файла.</span><span class="sxs-lookup"><span data-stu-id="cb986-120">The Image Tag Helper uses the `Cache` provider on the local web server to store the calculated `Sha512` of a given file.</span></span> <span data-ttu-id="cb986-121">Если файл запрашивается снова, значение `Sha512` не нужно вычислять повторно.</span><span class="sxs-lookup"><span data-stu-id="cb986-121">If the file is requested again the `Sha512` doesn't need to be recalculated.</span></span> <span data-ttu-id="cb986-122">Кэш делает недействительным наблюдатель, который назначается файлу при вычислении его значения `Sha512`.</span><span class="sxs-lookup"><span data-stu-id="cb986-122">The Cache is invalidated by a file watcher that's attached to the file when the file's `Sha512` is calculated.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="cb986-123">Дополнительные ресурсы</span><span class="sxs-lookup"><span data-stu-id="cb986-123">Additional resources</span></span>

* <xref:performance/caching/memory>
