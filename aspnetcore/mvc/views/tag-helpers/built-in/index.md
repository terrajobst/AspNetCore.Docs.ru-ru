---
title: Встроенные вспомогательные функции тегов ASP.NET Core
author: pkellner
description: Узнайте, как использовать встроенные вспомогательные функции тегов ASP.NET Core для более эффективной работы.
ms.author: riande
ms.custom: mvc
ms.date: 10/10/2018
uid: mvc/views/tag-helpers/builtin-th/Index
ms.openlocfilehash: 58840d6ecd09bd2ae7f96c046a0cb93c018f9645
ms.sourcegitcommit: 4bdf7703aed86ebd56b9b4bae9ad5700002af32d
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/15/2018
ms.locfileid: "49325488"
---
# <a name="aspnet-core-built-in-tag-helpers"></a><span data-ttu-id="67ffc-103">Встроенные вспомогательные функции тегов ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="67ffc-103">ASP.NET Core built-in Tag Helpers</span></span>

<span data-ttu-id="67ffc-104">Автор: [Питер Кельнер (Peter Kellner)](http://peterkellner.net)</span><span class="sxs-lookup"><span data-stu-id="67ffc-104">By [Peter Kellner](http://peterkellner.net)</span></span>

<span data-ttu-id="67ffc-105">Общие сведения о вспомогательных функциях тегов см. здесь: <xref:mvc/views/tag-helpers/intro>.</span><span class="sxs-lookup"><span data-stu-id="67ffc-105">For an overview of Tag Helpers, see <xref:mvc/views/tag-helpers/intro>.</span></span>

> [!NOTE]
> <span data-ttu-id="67ffc-106">Есть и другие встроенные вспомогательные функции тегов, которые здесь не описаны.</span><span class="sxs-lookup"><span data-stu-id="67ffc-106">There are built-in Tag Helpers which aren't described in the documentation.</span></span> <span data-ttu-id="67ffc-107">Эти вспомогательные функции используются внутренними механизмами системой просмотра [Razor](xref:mvc/views/razor).</span><span class="sxs-lookup"><span data-stu-id="67ffc-107">These Tag Helpers are used internally by the [Razor](xref:mvc/views/razor) view engine.</span></span> <span data-ttu-id="67ffc-108">В их число входит вспомогательная функция для знака `~` (тильда), который указывает на корневой путь веб-сайта.</span><span class="sxs-lookup"><span data-stu-id="67ffc-108">This includes a Tag Helper for the `~` (tilde) character, which expands to the root path of the website.</span></span>

## <a name="built-in-aspnet-core-tag-helpers"></a><span data-ttu-id="67ffc-109">Встроенные вспомогательные функции тегов ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="67ffc-109">Built-in ASP.NET Core Tag Helpers</span></span>

<span data-ttu-id="67ffc-110">**[Вспомогательная функция тега привязки](xref:mvc/views/tag-helpers/builtin-th/anchor-tag-helper)**</span><span class="sxs-lookup"><span data-stu-id="67ffc-110">**[Anchor Tag Helper](xref:mvc/views/tag-helpers/builtin-th/anchor-tag-helper)**</span></span>

<span data-ttu-id="67ffc-111">**[Вспомогательная функция тега кэша](xref:mvc/views/tag-helpers/builtin-th/cache-tag-helper)**</span><span class="sxs-lookup"><span data-stu-id="67ffc-111">**[Cache Tag Helper](xref:mvc/views/tag-helpers/builtin-th/cache-tag-helper)**</span></span>

<span data-ttu-id="67ffc-112">**[Вспомогательная функция тега распределенного кэша](xref:mvc/views/tag-helpers/builtin-th/distributed-cache-tag-helper)**</span><span class="sxs-lookup"><span data-stu-id="67ffc-112">**[Distributed Cache Tag Helper](xref:mvc/views/tag-helpers/builtin-th/distributed-cache-tag-helper)**</span></span>

<span data-ttu-id="67ffc-113">**[Вспомогательная функция тега среды](xref:mvc/views/tag-helpers/builtin-th/environment-tag-helper)**</span><span class="sxs-lookup"><span data-stu-id="67ffc-113">**[Environment Tag Helper](xref:mvc/views/tag-helpers/builtin-th/environment-tag-helper)**</span></span>

[comment]: **[FormActionTagHelper](xref:mvc/views/tag-helpers/builtin-th/form-action-tag-helper)**

<span data-ttu-id="67ffc-114">**[Вспомогательная функция тега форм](xref:mvc/views/working-with-forms#the-form-tag-helper)**</span><span class="sxs-lookup"><span data-stu-id="67ffc-114">**[Form Tag Helper](xref:mvc/views/working-with-forms#the-form-tag-helper)**</span></span>

<span data-ttu-id="67ffc-115">**[Вспомогательная функция тега образа](xref:mvc/views/tag-helpers/builtin-th/image-tag-helper)**</span><span class="sxs-lookup"><span data-stu-id="67ffc-115">**[Image Tag Helper](xref:mvc/views/tag-helpers/builtin-th/image-tag-helper)**</span></span>

<span data-ttu-id="67ffc-116">**[Вспомогательная функция тега Input](xref:mvc/views/working-with-forms#the-input-tag-helper)**</span><span class="sxs-lookup"><span data-stu-id="67ffc-116">**[Input Tag Helper](xref:mvc/views/working-with-forms#the-input-tag-helper)**</span></span>

<span data-ttu-id="67ffc-117">**[Вспомогательная функция тега метки](xref:mvc/views/working-with-forms#the-label-tag-helper)**</span><span class="sxs-lookup"><span data-stu-id="67ffc-117">**[Label Tag Helper](xref:mvc/views/working-with-forms#the-label-tag-helper)**</span></span>

[comment]: **[LinkTagHelper](xref:mvc/views/tag-helpers/builtin-th/link-tag-helper)**

[comment]: **[OptionTagHelper](xref:mvc/views/tag-helpers/builtin-th/option-tag-helper)**

[comment]: **[ScriptTagHelper](xref:mvc/views/tag-helpers/builtin-th/script-tag-helper)**

<span data-ttu-id="67ffc-118">**[Вспомогательная функция тега частичного представления](xref:mvc/views/tag-helpers/builtin-th/partial-tag-helper)**</span><span class="sxs-lookup"><span data-stu-id="67ffc-118">**[Partial Tag Helper](xref:mvc/views/tag-helpers/builtin-th/partial-tag-helper)**</span></span>

<span data-ttu-id="67ffc-119">**[Вспомогательная функция тега Select](xref:mvc/views/working-with-forms#the-select-tag-helper)**</span><span class="sxs-lookup"><span data-stu-id="67ffc-119">**[Select Tag Helper](xref:mvc/views/working-with-forms#the-select-tag-helper)**</span></span>

<span data-ttu-id="67ffc-120">**[Вспомогательная функция тега Textarea](xref:mvc/views/working-with-forms#the-textarea-tag-helper)**</span><span class="sxs-lookup"><span data-stu-id="67ffc-120">**[Textarea Tag Helper](xref:mvc/views/working-with-forms#the-textarea-tag-helper)**</span></span>

<span data-ttu-id="67ffc-121">**[Вспомогательная функция тега сообщения о проверке](xref:mvc/views/working-with-forms#the-validation-message-tag-helper)**</span><span class="sxs-lookup"><span data-stu-id="67ffc-121">**[Validation Message Tag Helper](xref:mvc/views/working-with-forms#the-validation-message-tag-helper)**</span></span>

<span data-ttu-id="67ffc-122">**[Вспомогательная функция тега сводки по проверке](xref:mvc/views/working-with-forms#the-validation-summary-tag-helper)**</span><span class="sxs-lookup"><span data-stu-id="67ffc-122">**[Validation Summary Tag Helper](xref:mvc/views/working-with-forms#the-validation-summary-tag-helper)**</span></span>

## <a name="additional-resources"></a><span data-ttu-id="67ffc-123">Дополнительные ресурсы</span><span class="sxs-lookup"><span data-stu-id="67ffc-123">Additional resources</span></span>

* <xref:mvc/views/tag-helpers/intro>
* <xref:mvc/views/tag-helpers/th-components>
