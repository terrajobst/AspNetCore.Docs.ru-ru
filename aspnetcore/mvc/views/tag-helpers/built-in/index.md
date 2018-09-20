---
title: Встроенные вспомогательные функции тегов ASP.NET Core
author: pkellner
description: Узнайте, как использовать встроенные вспомогательные функции тегов ASP.NET Core для более эффективной работы.
ms.author: riande
ms.date: 09/18/2018
uid: mvc/views/tag-helpers/builtin-th/Index
ms.openlocfilehash: 5d9425e0b68578c86a6f9a691322e0bb63a860fb
ms.sourcegitcommit: c684eb6c0999d11d19e15e65939e5c7f99ba47df
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 09/19/2018
ms.locfileid: "46292314"
---
# <a name="aspnet-core-built-in-tag-helpers"></a><span data-ttu-id="6c083-103">Встроенные вспомогательные функции тегов ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="6c083-103">ASP.NET Core built-in Tag Helpers</span></span>

<span data-ttu-id="6c083-104">Автор: [Питер Кельнер (Peter Kellner)](http://peterkellner.net)</span><span class="sxs-lookup"><span data-stu-id="6c083-104">By [Peter Kellner](http://peterkellner.net)</span></span>

<span data-ttu-id="6c083-105">ASP.NET Core включает множество встроенных вспомогательных функций для эффективной работы с тегами.</span><span class="sxs-lookup"><span data-stu-id="6c083-105">ASP.NET Core includes many built-in Tag Helpers to boost your productivity.</span></span> <span data-ttu-id="6c083-106">В этом разделе приводится их обзор.</span><span class="sxs-lookup"><span data-stu-id="6c083-106">This section provides an overview of the built-in Tag Helpers.</span></span>

> [!NOTE]
> <span data-ttu-id="6c083-107">Некоторые встроенные вспомогательные функции тегов здесь не рассматриваются, так как они используются для внутренних задач обработчика представлений [Razor](xref:mvc/views/razor).</span><span class="sxs-lookup"><span data-stu-id="6c083-107">There are built-in Tag Helpers which aren't discussed, since they're used internally by the [Razor](xref:mvc/views/razor) view engine.</span></span> <span data-ttu-id="6c083-108">В их число входит вспомогательная функция для знака "~", который разворачивается к корневому пути веб-сайта.</span><span class="sxs-lookup"><span data-stu-id="6c083-108">This includes a Tag Helper for the ~ character, which expands to the root path of the website.</span></span>

## <a name="built-in-aspnet-core-tag-helpers"></a><span data-ttu-id="6c083-109">Встроенные вспомогательные функции тегов ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="6c083-109">Built-in ASP.NET Core Tag Helpers</span></span>

<span data-ttu-id="6c083-110">**[Вспомогательная функция тега привязки](xref:mvc/views/tag-helpers/builtin-th/anchor-tag-helper)**</span><span class="sxs-lookup"><span data-stu-id="6c083-110">**[Anchor Tag Helper](xref:mvc/views/tag-helpers/builtin-th/anchor-tag-helper)**</span></span>

<span data-ttu-id="6c083-111">**[Вспомогательная функция тега кэша](xref:mvc/views/tag-helpers/builtin-th/cache-tag-helper)**</span><span class="sxs-lookup"><span data-stu-id="6c083-111">**[Cache Tag Helper](xref:mvc/views/tag-helpers/builtin-th/cache-tag-helper)**</span></span>

<span data-ttu-id="6c083-112">**[Вспомогательная функция тега распределенного кэша](xref:mvc/views/tag-helpers/builtin-th/distributed-cache-tag-helper)**</span><span class="sxs-lookup"><span data-stu-id="6c083-112">**[Distributed Cache Tag Helper](xref:mvc/views/tag-helpers/builtin-th/distributed-cache-tag-helper)**</span></span>

<span data-ttu-id="6c083-113">**[Вспомогательная функция тега среды](xref:mvc/views/tag-helpers/builtin-th/environment-tag-helper)**</span><span class="sxs-lookup"><span data-stu-id="6c083-113">**[Environment Tag Helper](xref:mvc/views/tag-helpers/builtin-th/environment-tag-helper)**</span></span>

[comment]: **[FormActionTagHelper](xref:mvc/views/tag-helpers/builtin-th/form-action-tag-helper)**

<span data-ttu-id="6c083-114">**[Вспомогательная функция тега форм](xref:mvc/views/working-with-forms#the-form-tag-helper)**</span><span class="sxs-lookup"><span data-stu-id="6c083-114">**[Form Tag Helper](xref:mvc/views/working-with-forms#the-form-tag-helper)**</span></span>

<span data-ttu-id="6c083-115">**[Вспомогательная функция тега образа](xref:mvc/views/tag-helpers/builtin-th/image-tag-helper)**</span><span class="sxs-lookup"><span data-stu-id="6c083-115">**[Image Tag Helper](xref:mvc/views/tag-helpers/builtin-th/image-tag-helper)**</span></span>

<span data-ttu-id="6c083-116">**[Вспомогательная функция тега Input](xref:mvc/views/working-with-forms#the-input-tag-helper)**</span><span class="sxs-lookup"><span data-stu-id="6c083-116">**[Input Tag Helper](xref:mvc/views/working-with-forms#the-input-tag-helper)**</span></span>

<span data-ttu-id="6c083-117">**[Вспомогательная функция тега метки](xref:mvc/views/working-with-forms#the-label-tag-helper)**</span><span class="sxs-lookup"><span data-stu-id="6c083-117">**[Label Tag Helper](xref:mvc/views/working-with-forms#the-label-tag-helper)**</span></span>

[comment]: **[LinkTagHelper](xref:mvc/views/tag-helpers/builtin-th/link-tag-helper)**

[comment]: **[OptionTagHelper](xref:mvc/views/tag-helpers/builtin-th/option-tag-helper)**

[comment]: **[ScriptTagHelper](xref:mvc/views/tag-helpers/builtin-th/script-tag-helper)**

<span data-ttu-id="6c083-118">**[Вспомогательная функция тега частичного представления](xref:mvc/views/tag-helpers/builtin-th/partial-tag-helper)**</span><span class="sxs-lookup"><span data-stu-id="6c083-118">**[Partial Tag Helper](xref:mvc/views/tag-helpers/builtin-th/partial-tag-helper)**</span></span>

<span data-ttu-id="6c083-119">**[Вспомогательная функция тега Select](xref:mvc/views/working-with-forms#the-select-tag-helper)**</span><span class="sxs-lookup"><span data-stu-id="6c083-119">**[Select Tag Helper](xref:mvc/views/working-with-forms#the-select-tag-helper)**</span></span>

<span data-ttu-id="6c083-120">**[Вспомогательная функция тега Textarea](xref:mvc/views/working-with-forms#the-textarea-tag-helper)**</span><span class="sxs-lookup"><span data-stu-id="6c083-120">**[Textarea Tag Helper](xref:mvc/views/working-with-forms#the-textarea-tag-helper)**</span></span>

<span data-ttu-id="6c083-121">**[Вспомогательная функция тега сообщения о проверке](xref:mvc/views/working-with-forms#the-validation-message-tag-helper)**</span><span class="sxs-lookup"><span data-stu-id="6c083-121">**[Validation Message Tag Helper](xref:mvc/views/working-with-forms#the-validation-message-tag-helper)**</span></span>

<span data-ttu-id="6c083-122">**[Вспомогательная функция тега сводки по проверке](xref:mvc/views/working-with-forms#the-validation-summary-tag-helper)**</span><span class="sxs-lookup"><span data-stu-id="6c083-122">**[Validation Summary Tag Helper](xref:mvc/views/working-with-forms#the-validation-summary-tag-helper)**</span></span>

## <a name="additional-resources"></a><span data-ttu-id="6c083-123">Дополнительные ресурсы</span><span class="sxs-lookup"><span data-stu-id="6c083-123">Additional resources</span></span>

* <xref:mvc/views/tag-helpers/intro>
* <xref:mvc/views/tag-helpers/th-components>
