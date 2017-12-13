---
title: "Вспомогательный тег изображения | Документы Microsoft"
author: pkellner
description: "Показано, как работать с тега поддержки изображений"
keywords: "ASP.NET Core, вспомогательная функция тегов"
ms.author: riande
manager: wpickett
ms.date: 02/14/2017
ms.topic: article
ms.assetid: c045d485-d1dc-4cea-a675-46be83b7a013
ms.technology: aspnet
ms.prod: aspnet-core
uid: mvc/views/tag-helpers/builtin-th/image-tag-helper
ms.openlocfilehash: 0d55514508b963ce05031f89a20af696f5d4a670
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 11/10/2017
---
# <a name="imagetaghelper"></a><span data-ttu-id="73615-104">ImageTagHelper</span><span class="sxs-lookup"><span data-stu-id="73615-104">ImageTagHelper</span></span>

<span data-ttu-id="73615-105">Автор: [Питер Кельнер (Peter Kellner)](http://peterkellner.net)</span><span class="sxs-lookup"><span data-stu-id="73615-105">By [Peter Kellner](http://peterkellner.net)</span></span> 

<span data-ttu-id="73615-106">Тег поддержки изображений улучшает `img` (`<img>`) тега.</span><span class="sxs-lookup"><span data-stu-id="73615-106">The Image Tag Helper enhances the `img` (`<img>`) tag.</span></span> <span data-ttu-id="73615-107">Требуется `src` тег а также `boolean` атрибут `asp-append-version`.</span><span class="sxs-lookup"><span data-stu-id="73615-107">It requires a `src` tag as well as the `boolean` attribute `asp-append-version`.</span></span>

<span data-ttu-id="73615-108">Если источник изображения (`src`) представляет собой статический файл на сервере веб-узла добавляется уникальный кэша busting строку как параметр запроса для источника изображения.</span><span class="sxs-lookup"><span data-stu-id="73615-108">If the image source (`src`) is a static file on the host web server, a unique cache busting string is appended as a query parameter to the image source.</span></span> <span data-ttu-id="73615-109">Это гарантирует, что при изменении файла на сервере веб-узла URL-АДРЕСЕ запроса уникальный создается, содержит параметр обновленный запрос.</span><span class="sxs-lookup"><span data-stu-id="73615-109">This ensures that if the file on the host web server changes, a unique request URL is generated that includes the updated request parameter.</span></span> <span data-ttu-id="73615-110">Кэш busting строка имеет уникальное значение, представляющее хэш файла статическое изображение.</span><span class="sxs-lookup"><span data-stu-id="73615-110">The cache busting string is a unique value representing the hash of the static image file.</span></span>

<span data-ttu-id="73615-111">Если источник изображения (`src`) не является статическим файлом (например удаленный URL-адрес или файл не существует на сервере) `<img>` тега `src` без кэширования, busting строкового параметра запроса создается атрибут.</span><span class="sxs-lookup"><span data-stu-id="73615-111">If the image source (`src`) isn't a static file (for example a remote URL or the file doesn't exist on the server), the `<img>` tag's `src` attribute is generated with no cache busting query string parameter.</span></span>

## <a name="image-tag-helper-attributes"></a><span data-ttu-id="73615-112">Атрибуты вспомогательного тега Image</span><span class="sxs-lookup"><span data-stu-id="73615-112">Image Tag Helper Attributes</span></span>


### <a name="asp-append-version"></a><span data-ttu-id="73615-113">добавить версию ASP</span><span class="sxs-lookup"><span data-stu-id="73615-113">asp-append-version</span></span>

<span data-ttu-id="73615-114">При указании вместе с `src` атрибут вспомогательный тег изображения вызывается.</span><span class="sxs-lookup"><span data-stu-id="73615-114">When specified along with a `src` attribute, the Image Tag Helper is invoked.</span></span>

<span data-ttu-id="73615-115">Примером является допустимым `img` модуль тег:</span><span class="sxs-lookup"><span data-stu-id="73615-115">An example of a valid `img` tag helper is:</span></span>

```cshtml
<img src="~/images/asplogo.png" 
    asp-append-version="true"  />
```

<span data-ttu-id="73615-116">Если статический файл существует в каталоге *... wwwroot/Images/asplogo.PNG* созданного кода html (хэш-код будет отличаться) следующего вида:</span><span class="sxs-lookup"><span data-stu-id="73615-116">If the static file exists in the directory *..wwwroot/images/asplogo.png* the generated html is similar to the following (the hash will be different):</span></span>

```html
<img 
    src="/images/asplogo.png?v=Kl_dqr9NVtnMdsM2MUg4qthUnWZm5T1fCEimBPWDNgM"/>
```

<span data-ttu-id="73615-117">Значение, присваиваемое параметру `v` значение хэша файла на диске.</span><span class="sxs-lookup"><span data-stu-id="73615-117">The value assigned to the parameter `v` is the hash value of the file on disk.</span></span> <span data-ttu-id="73615-118">Если веб-сервер не может получить доступ на чтение к статические ссылки на файл, нет `v` добавляется параметры `src` атрибута.</span><span class="sxs-lookup"><span data-stu-id="73615-118">If the web server is unable to obtain read access to the static file referenced,  no `v` parameters is added to the `src` attribute.</span></span>

- - -

### <a name="src"></a><span data-ttu-id="73615-119">src</span><span class="sxs-lookup"><span data-stu-id="73615-119">src</span></span>

<span data-ttu-id="73615-120">Чтобы активировать вспомогательный тег изображения, атрибут src необходим для `<img>` элемента.</span><span class="sxs-lookup"><span data-stu-id="73615-120">To activate the Image Tag Helper, the src attribute is required on the `<img>` element.</span></span> 

> [!NOTE]
> <span data-ttu-id="73615-121">Использует вспомогательный тег изображения `Cache` поставщика на локальном веб-сервере для хранения вычисляемого `Sha512` для заданного файла.</span><span class="sxs-lookup"><span data-stu-id="73615-121">The Image Tag Helper uses the `Cache` provider on the local web server to store the calculated `Sha512` of a given file.</span></span> <span data-ttu-id="73615-122">Если снова запрошенный файл `Sha512` не требуется повторное вычисление.</span><span class="sxs-lookup"><span data-stu-id="73615-122">If the file is requested again the `Sha512` does not need to be recalculated.</span></span> <span data-ttu-id="73615-123">Кэш становится недействительным, файл наблюдатель, который присоединяется к файлу при его `Sha512` вычисляется.</span><span class="sxs-lookup"><span data-stu-id="73615-123">The Cache is invalidated by a file watcher that is attached to the file when the file's `Sha512` is calculated.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="73615-124">Дополнительные ресурсы</span><span class="sxs-lookup"><span data-stu-id="73615-124">Additional resources</span></span>

* <xref:performance/caching/memory>
