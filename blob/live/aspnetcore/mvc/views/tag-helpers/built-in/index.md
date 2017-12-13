---
title: "Встроенные вспомогательные функции тегов ASP.NET Core"
author: pkellner
description: "Встроенные вспомогательные функции тегов ASP.NET Core"
keywords: "ASP.NET Core, вспомогательная функция тегов"
ms.author: riande
manager: wpickett
ms.date: 09/13/2017
ms.topic: article
ms.technology: aspnet
ms.prod: aspnet-core
uid: mvc/views/tag-helpers/builtin-th/Index
ms.openlocfilehash: dd732822a715df19c0ee4b6accad3455ad6537da
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 11/10/2017
---
# <a name="aspnet-core-built-in-tag-helpers"></a><span data-ttu-id="8880a-104">Встроенные вспомогательные функции тегов ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="8880a-104">ASP.NET Core built-in Tag Helpers</span></span>

<span data-ttu-id="8880a-105">Автор: [Питер Кельнер (Peter Kellner)](http://peterkellner.net)</span><span class="sxs-lookup"><span data-stu-id="8880a-105">By [Peter Kellner](http://peterkellner.net)</span></span> 

<span data-ttu-id="8880a-106">ASP.NET Core включает множество встроенных вспомогательных функций для эффективной работы с тегами.</span><span class="sxs-lookup"><span data-stu-id="8880a-106">ASP.NET Core includes many built-in Tag Helpers to boost your productivity.</span></span> <span data-ttu-id="8880a-107">В этом разделе приводится их обзор.</span><span class="sxs-lookup"><span data-stu-id="8880a-107">This section provides an overview of the built-in Tag Helpers.</span></span>

> [!NOTE]
> <span data-ttu-id="8880a-108">Некоторые встроенные вспомогательные функции тегов здесь не рассматриваются, так как они используются для внутренних задач обработчика представлений [Razor](xref:mvc/views/razor).</span><span class="sxs-lookup"><span data-stu-id="8880a-108">There are built-in Tag Helpers which aren't discussed, since they're used internally by the [Razor](xref:mvc/views/razor) view engine.</span></span> <span data-ttu-id="8880a-109">В их число входит вспомогательная функция для знака "~", который разворачивается к корневому пути веб-сайта.</span><span class="sxs-lookup"><span data-stu-id="8880a-109">This includes a Tag Helper for the ~ character, which expands to the root path of the website.</span></span>

## <a name="built-in-aspnet-core-tag-helpers"></a><span data-ttu-id="8880a-110">Встроенные вспомогательные функции тегов ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="8880a-110">Built-in ASP.NET Core Tag Helpers</span></span>

<span data-ttu-id="8880a-111">**[Вспомогательная функция тега привязки](xref:mvc/views/tag-helpers/builtin-th/anchor-tag-helper)**</span><span class="sxs-lookup"><span data-stu-id="8880a-111">**[Anchor Tag Helper](xref:mvc/views/tag-helpers/builtin-th/anchor-tag-helper)**</span></span>

<span data-ttu-id="8880a-112">**[Вспомогательная функция тега кэша](xref:mvc/views/tag-helpers/builtin-th/cache-tag-helper)**</span><span class="sxs-lookup"><span data-stu-id="8880a-112">**[Cache Tag Helper](xref:mvc/views/tag-helpers/builtin-th/cache-tag-helper)**</span></span>

<span data-ttu-id="8880a-113">**[Вспомогательная функция тега распределенного кэша](xref:mvc/views/tag-helpers/builtin-th/distributed-cache-tag-helper)**</span><span class="sxs-lookup"><span data-stu-id="8880a-113">**[Distributed Cache Tag Helper](xref:mvc/views/tag-helpers/builtin-th/distributed-cache-tag-helper)**</span></span>

<span data-ttu-id="8880a-114">**[Вспомогательная функция тега среды](xref:mvc/views/tag-helpers/builtin-th/environment-tag-helper)**</span><span class="sxs-lookup"><span data-stu-id="8880a-114">**[Environment Tag Helper](xref:mvc/views/tag-helpers/builtin-th/environment-tag-helper)**</span></span>

[comment]: **[FormActionTagHelper](xref:mvc/views/tag-helpers/builtin-th/form-action-tag-helper)**

<span data-ttu-id="8880a-115">**[Вспомогательная функция тега форм](xref:mvc/views/working-with-forms#the-form-tag-helper)**</span><span class="sxs-lookup"><span data-stu-id="8880a-115">**[Form Tag Helper](xref:mvc/views/working-with-forms#the-form-tag-helper)**</span></span>

<span data-ttu-id="8880a-116">**[Вспомогательная функция тега образа](xref:mvc/views/tag-helpers/builtin-th/image-tag-helper)**</span><span class="sxs-lookup"><span data-stu-id="8880a-116">**[Image Tag Helper](xref:mvc/views/tag-helpers/builtin-th/image-tag-helper)**</span></span>

<span data-ttu-id="8880a-117">**[Вспомогательная функция тега Input](xref:mvc/views/working-with-forms#the-input-tag-helper)**</span><span class="sxs-lookup"><span data-stu-id="8880a-117">**[Input Tag Helper](xref:mvc/views/working-with-forms#the-input-tag-helper)**</span></span>

<span data-ttu-id="8880a-118">**[Вспомогательная функция тега метки](xref:mvc/views/working-with-forms#the-label-tag-helper)**</span><span class="sxs-lookup"><span data-stu-id="8880a-118">**[Label Tag Helper](xref:mvc/views/working-with-forms#the-label-tag-helper)**</span></span>

[comment]: **[LinkTagHelper](xref:mvc/views/tag-helpers/builtin-th/link-tag-helper)**

[comment]: **[OptionTagHelper](xref:mvc/views/tag-helpers/builtin-th/option-tag-helper)**

[comment]: **[ScriptTagHelper](xref:mvc/views/tag-helpers/builtin-th/script-tag-helper)**

<span data-ttu-id="8880a-119">**[Вспомогательная функция тега Select](xref:mvc/views/working-with-forms#the-select-tag-helper)**</span><span class="sxs-lookup"><span data-stu-id="8880a-119">**[Select Tag Helper](xref:mvc/views/working-with-forms#the-select-tag-helper)**</span></span>

<span data-ttu-id="8880a-120">**[Вспомогательная функция тега Textarea](xref:mvc/views/working-with-forms#the-textarea-tag-helper)**</span><span class="sxs-lookup"><span data-stu-id="8880a-120">**[Textarea Tag Helper](xref:mvc/views/working-with-forms#the-textarea-tag-helper)**</span></span>

<span data-ttu-id="8880a-121">**[Вспомогательная функция тега сообщения о проверке](xref:mvc/views/working-with-forms#the-validation-message-tag-helper)**</span><span class="sxs-lookup"><span data-stu-id="8880a-121">**[Validation Message Tag Helper](xref:mvc/views/working-with-forms#the-validation-message-tag-helper)**</span></span>

<span data-ttu-id="8880a-122">**[Вспомогательная функция тега сводки по проверке](xref:mvc/views/working-with-forms#the-validation-summary-tag-helper)**</span><span class="sxs-lookup"><span data-stu-id="8880a-122">**[Validation Summary Tag Helper](xref:mvc/views/working-with-forms#the-validation-summary-tag-helper)**</span></span>

## <a name="additional-resources"></a><span data-ttu-id="8880a-123">Дополнительные ресурсы</span><span class="sxs-lookup"><span data-stu-id="8880a-123">Additional resources</span></span>

* [<span data-ttu-id="8880a-124">Клиентская разработка</span><span class="sxs-lookup"><span data-stu-id="8880a-124">Client-Side Development</span></span>](xref:client-side/index)
* [<span data-ttu-id="8880a-125">Вспомогательные функции тегов</span><span class="sxs-lookup"><span data-stu-id="8880a-125">Tag Helpers</span></span>](xref:mvc/views/tag-helpers/intro)
