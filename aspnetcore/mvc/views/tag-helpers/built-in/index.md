---
title: "Встроенные вспомогательные функции тегов ASP.NET Core"
author: pkellner
description: "Встроенные вспомогательные функции тегов ASP.NET Core"
manager: wpickett
ms.author: riande
ms.date: 09/13/2017
ms.prod: aspnet-core
ms.technology: aspnet
ms.topic: article
uid: mvc/views/tag-helpers/builtin-th/Index
ms.openlocfilehash: 09024bf0d6c87fce9eba9b70bebefa11d2ff0a44
ms.sourcegitcommit: a510f38930abc84c4b302029d019a34dfe76823b
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/30/2018
---
# <a name="aspnet-core-built-in-tag-helpers"></a><span data-ttu-id="40b1c-103">Встроенные вспомогательные функции тегов ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="40b1c-103">ASP.NET Core built-in Tag Helpers</span></span>

<span data-ttu-id="40b1c-104">Автор: [Питер Кельнер (Peter Kellner)](http://peterkellner.net)</span><span class="sxs-lookup"><span data-stu-id="40b1c-104">By [Peter Kellner](http://peterkellner.net)</span></span> 

<span data-ttu-id="40b1c-105">ASP.NET Core включает множество встроенных вспомогательных функций для эффективной работы с тегами.</span><span class="sxs-lookup"><span data-stu-id="40b1c-105">ASP.NET Core includes many built-in Tag Helpers to boost your productivity.</span></span> <span data-ttu-id="40b1c-106">В этом разделе приводится их обзор.</span><span class="sxs-lookup"><span data-stu-id="40b1c-106">This section provides an overview of the built-in Tag Helpers.</span></span>

> [!NOTE]
> <span data-ttu-id="40b1c-107">Некоторые встроенные вспомогательные функции тегов здесь не рассматриваются, так как они используются для внутренних задач обработчика представлений [Razor](xref:mvc/views/razor).</span><span class="sxs-lookup"><span data-stu-id="40b1c-107">There are built-in Tag Helpers which aren't discussed, since they're used internally by the [Razor](xref:mvc/views/razor) view engine.</span></span> <span data-ttu-id="40b1c-108">В их число входит вспомогательная функция для знака "~", который разворачивается к корневому пути веб-сайта.</span><span class="sxs-lookup"><span data-stu-id="40b1c-108">This includes a Tag Helper for the ~ character, which expands to the root path of the website.</span></span>

## <a name="built-in-aspnet-core-tag-helpers"></a><span data-ttu-id="40b1c-109">Встроенные вспомогательные функции тегов ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="40b1c-109">Built-in ASP.NET Core Tag Helpers</span></span>

<span data-ttu-id="40b1c-110">**[Вспомогательная функция тега привязки](xref:mvc/views/tag-helpers/builtin-th/anchor-tag-helper)**</span><span class="sxs-lookup"><span data-stu-id="40b1c-110">**[Anchor Tag Helper](xref:mvc/views/tag-helpers/builtin-th/anchor-tag-helper)**</span></span>

<span data-ttu-id="40b1c-111">**[Вспомогательная функция тега кэша](xref:mvc/views/tag-helpers/builtin-th/cache-tag-helper)**</span><span class="sxs-lookup"><span data-stu-id="40b1c-111">**[Cache Tag Helper](xref:mvc/views/tag-helpers/builtin-th/cache-tag-helper)**</span></span>

<span data-ttu-id="40b1c-112">**[Вспомогательная функция тега распределенного кэша](xref:mvc/views/tag-helpers/builtin-th/distributed-cache-tag-helper)**</span><span class="sxs-lookup"><span data-stu-id="40b1c-112">**[Distributed Cache Tag Helper](xref:mvc/views/tag-helpers/builtin-th/distributed-cache-tag-helper)**</span></span>

<span data-ttu-id="40b1c-113">**[Вспомогательная функция тега среды](xref:mvc/views/tag-helpers/builtin-th/environment-tag-helper)**</span><span class="sxs-lookup"><span data-stu-id="40b1c-113">**[Environment Tag Helper](xref:mvc/views/tag-helpers/builtin-th/environment-tag-helper)**</span></span>

[comment]: **[FormActionTagHelper](xref:mvc/views/tag-helpers/builtin-th/form-action-tag-helper)**

<span data-ttu-id="40b1c-114">**[Вспомогательная функция тега форм](xref:mvc/views/working-with-forms#the-form-tag-helper)**</span><span class="sxs-lookup"><span data-stu-id="40b1c-114">**[Form Tag Helper](xref:mvc/views/working-with-forms#the-form-tag-helper)**</span></span>

<span data-ttu-id="40b1c-115">**[Вспомогательная функция тега образа](xref:mvc/views/tag-helpers/builtin-th/image-tag-helper)**</span><span class="sxs-lookup"><span data-stu-id="40b1c-115">**[Image Tag Helper](xref:mvc/views/tag-helpers/builtin-th/image-tag-helper)**</span></span>

<span data-ttu-id="40b1c-116">**[Вспомогательная функция тега Input](xref:mvc/views/working-with-forms#the-input-tag-helper)**</span><span class="sxs-lookup"><span data-stu-id="40b1c-116">**[Input Tag Helper](xref:mvc/views/working-with-forms#the-input-tag-helper)**</span></span>

<span data-ttu-id="40b1c-117">**[Вспомогательная функция тега метки](xref:mvc/views/working-with-forms#the-label-tag-helper)**</span><span class="sxs-lookup"><span data-stu-id="40b1c-117">**[Label Tag Helper](xref:mvc/views/working-with-forms#the-label-tag-helper)**</span></span>

[comment]: **[LinkTagHelper](xref:mvc/views/tag-helpers/builtin-th/link-tag-helper)**

[comment]: **[OptionTagHelper](xref:mvc/views/tag-helpers/builtin-th/option-tag-helper)**

[comment]: **[ScriptTagHelper](xref:mvc/views/tag-helpers/builtin-th/script-tag-helper)**

<span data-ttu-id="40b1c-118">**[Вспомогательная функция тега Select](xref:mvc/views/working-with-forms#the-select-tag-helper)**</span><span class="sxs-lookup"><span data-stu-id="40b1c-118">**[Select Tag Helper](xref:mvc/views/working-with-forms#the-select-tag-helper)**</span></span>

<span data-ttu-id="40b1c-119">**[Вспомогательная функция тега Textarea](xref:mvc/views/working-with-forms#the-textarea-tag-helper)**</span><span class="sxs-lookup"><span data-stu-id="40b1c-119">**[Textarea Tag Helper](xref:mvc/views/working-with-forms#the-textarea-tag-helper)**</span></span>

<span data-ttu-id="40b1c-120">**[Вспомогательная функция тега сообщения о проверке](xref:mvc/views/working-with-forms#the-validation-message-tag-helper)**</span><span class="sxs-lookup"><span data-stu-id="40b1c-120">**[Validation Message Tag Helper](xref:mvc/views/working-with-forms#the-validation-message-tag-helper)**</span></span>

<span data-ttu-id="40b1c-121">**[Вспомогательная функция тега сводки по проверке](xref:mvc/views/working-with-forms#the-validation-summary-tag-helper)**</span><span class="sxs-lookup"><span data-stu-id="40b1c-121">**[Validation Summary Tag Helper](xref:mvc/views/working-with-forms#the-validation-summary-tag-helper)**</span></span>

## <a name="additional-resources"></a><span data-ttu-id="40b1c-122">Дополнительные ресурсы</span><span class="sxs-lookup"><span data-stu-id="40b1c-122">Additional resources</span></span>

* [<span data-ttu-id="40b1c-123">Клиентская разработка</span><span class="sxs-lookup"><span data-stu-id="40b1c-123">Client-Side Development</span></span>](xref:client-side/index)
* [<span data-ttu-id="40b1c-124">Вспомогательные функции тегов</span><span class="sxs-lookup"><span data-stu-id="40b1c-124">Tag Helpers</span></span>](xref:mvc/views/tag-helpers/intro)
