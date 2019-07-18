---
title: Вспомогательная функция тега изображения в ASP.NET Core
author: pkellner
description: Сведения о работе со вспомогательной функцией тега изображения.
ms.author: riande
ms.custom: mvc
ms.date: 04/06/2019
uid: mvc/views/tag-helpers/builtin-th/image-tag-helper
ms.openlocfilehash: 964072ad276f7e3e411ee41cb03a2efb9d05c585
ms.sourcegitcommit: 7a40c56bf6a6aaa63a7ee83a2cac9b3a1d77555e
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 07/12/2019
ms.locfileid: "67856119"
---
# <a name="image-tag-helper-in-aspnet-core"></a><span data-ttu-id="12481-103">Вспомогательная функция тега изображения в ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="12481-103">Image Tag Helper in ASP.NET Core</span></span>

<span data-ttu-id="12481-104">Автор: [Питер Кельнер (Peter Kellner)](https://peterkellner.net)</span><span class="sxs-lookup"><span data-stu-id="12481-104">By [Peter Kellner](https://peterkellner.net)</span></span>

<span data-ttu-id="12481-105">Вспомогательная функция тега изображения расширяет возможности тега `<img>`, чтобы предоставлять поведение отключения кэширования для файлов статических изображений.</span><span class="sxs-lookup"><span data-stu-id="12481-105">The Image Tag Helper enhances the `<img>` tag to provide cache-busting behavior for static image files.</span></span>

<span data-ttu-id="12481-106">Строка отключения кэширования — это уникальное значение, представляющее хэш-код статического файла изображения, добавленный к URL-адресу ресурса.</span><span class="sxs-lookup"><span data-stu-id="12481-106">A cache-busting string is a unique value representing the hash of the static image file appended to the asset's URL.</span></span> <span data-ttu-id="12481-107">Уникальная строка требует у клиентов (и некоторых прокси-серверов) повторно загружать изображение с веб-сервера, а не из кэша клиента.</span><span class="sxs-lookup"><span data-stu-id="12481-107">The unique string prompts clients (and some proxies) to reload the image from the host web server and not from the client's cache.</span></span>

<span data-ttu-id="12481-108">Если источник изображения (`src`) представляет собой статический файл на веб-сервере:</span><span class="sxs-lookup"><span data-stu-id="12481-108">If the image source (`src`) is a static file on the host web server:</span></span>

* <span data-ttu-id="12481-109">Уникальная строка отключения кэширования добавляется в качестве параметра запроса к источнику изображения.</span><span class="sxs-lookup"><span data-stu-id="12481-109">A unique cache-busting string is appended as a query parameter to the image source.</span></span>
* <span data-ttu-id="12481-110">При изменении файла на веб-сервере узла создается уникальный URL-адрес запроса, включающий в себя обновленный параметр запроса.</span><span class="sxs-lookup"><span data-stu-id="12481-110">If the file on the host web server changes, a unique request URL is generated that includes the updated request parameter.</span></span>

<span data-ttu-id="12481-111">Общие сведения о вспомогательных функциях тегов см. здесь: <xref:mvc/views/tag-helpers/intro>.</span><span class="sxs-lookup"><span data-stu-id="12481-111">For an overview of Tag Helpers, see <xref:mvc/views/tag-helpers/intro>.</span></span>

## <a name="image-tag-helper-attributes"></a><span data-ttu-id="12481-112">Атрибуты вспомогательной функции тега изображения</span><span class="sxs-lookup"><span data-stu-id="12481-112">Image Tag Helper Attributes</span></span>

### <a name="src"></a><span data-ttu-id="12481-113">src</span><span class="sxs-lookup"><span data-stu-id="12481-113">src</span></span>

<span data-ttu-id="12481-114">Для активации вспомогательной функции тега изображения требуется атрибут `src` в элементе `<img>`.</span><span class="sxs-lookup"><span data-stu-id="12481-114">To activate the Image Tag Helper, the `src` attribute is required on the `<img>` element.</span></span>

<span data-ttu-id="12481-115">Источник изображения (`src`) должен указывать на физический статический файл на сервере.</span><span class="sxs-lookup"><span data-stu-id="12481-115">The image source (`src`) must point to a physical static file on the server.</span></span> <span data-ttu-id="12481-116">Если `src` — это удаленный URI, параметр строки запроса отключения кэширования не создается.</span><span class="sxs-lookup"><span data-stu-id="12481-116">If the `src` is a remote URI, the cache-busting query string parameter isn't generated.</span></span>

### <a name="asp-append-version"></a><span data-ttu-id="12481-117">asp-append-version</span><span class="sxs-lookup"><span data-stu-id="12481-117">asp-append-version</span></span>

<span data-ttu-id="12481-118">Если указан `asp-append-version` со значением `true` и атрибутом `src`, вызывается вспомогательная функция тега изображения.</span><span class="sxs-lookup"><span data-stu-id="12481-118">When `asp-append-version` is specified with a `true` value along with a `src` attribute, the Image Tag Helper is invoked.</span></span>

<span data-ttu-id="12481-119">В приведенном ниже примере используется вспомогательная функция тега изображения:</span><span class="sxs-lookup"><span data-stu-id="12481-119">The following example uses an Image Tag Helper:</span></span>

```cshtml
<img src="~/images/asplogo.png" asp-append-version="true">
```

<span data-ttu-id="12481-120">Если статический файл существует в каталоге */wwwroot/images/* , создаваемый код HTML будет похож на следующий (хэш-код будет иным):</span><span class="sxs-lookup"><span data-stu-id="12481-120">If the static file exists in the directory */wwwroot/images/*, the generated HTML is similar to the following (the hash will be different):</span></span>

```html
<img src="/images/asplogo.png?v=Kl_dqr9NVtnMdsM2MUg4qthUnWZm5T1fCEimBPWDNgM">
```

<span data-ttu-id="12481-121">В качестве значения параметру `v` присваивается значение хэша файла *asplogo.png* на диске.</span><span class="sxs-lookup"><span data-stu-id="12481-121">The value assigned to the parameter `v` is the hash value of the *asplogo.png* file on disk.</span></span> <span data-ttu-id="12481-122">Если веб-серверу не удается получить доступ для чтения к указанному статическому файлу, параметр `v` не добавляется к атрибуту `src` в преобразованной разметке.</span><span class="sxs-lookup"><span data-stu-id="12481-122">If the web server is unable to obtain read access to the static file, no `v` parameter is added to the `src` attribute in the rendered markup.</span></span>

## <a name="hash-caching-behavior"></a><span data-ttu-id="12481-123">Поведение кэширования хэша</span><span class="sxs-lookup"><span data-stu-id="12481-123">Hash caching behavior</span></span>

<span data-ttu-id="12481-124">Вспомогательная функция тега изображения использует поставщик кэша на локальном веб-сервере для хранения значения хэша `Sha512`, вычисленного для данного файла.</span><span class="sxs-lookup"><span data-stu-id="12481-124">The Image Tag Helper uses the cache provider on the local web server to store the calculated `Sha512` hash of a given file.</span></span> <span data-ttu-id="12481-125">Если файл запрашивается несколько раз, хэш не пересчитывается.</span><span class="sxs-lookup"><span data-stu-id="12481-125">If the file is requested multiple times, the hash isn't recalculated.</span></span> <span data-ttu-id="12481-126">Наблюдатель, который назначается файлу при вычислении его значения хэша `Sha512`, делает кэш недействительным.</span><span class="sxs-lookup"><span data-stu-id="12481-126">The cache is invalidated by a file watcher that's attached to the file when the file's `Sha512` hash is calculated.</span></span> <span data-ttu-id="12481-127">При изменении файла на диске вычисляется и кэшируется новый хэш.</span><span class="sxs-lookup"><span data-stu-id="12481-127">When the file changes on disk, a new hash is calculated and cached.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="12481-128">Дополнительные ресурсы</span><span class="sxs-lookup"><span data-stu-id="12481-128">Additional resources</span></span>

* <xref:performance/caching/memory>
