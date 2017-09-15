---
title: "Встроенные вспомогательные функции тегов ASP.NET Core"
author: pkellner
description: "Встроенные вспомогательные функции тегов ASP.NET Core"
keywords: "ASP.NET Core, вспомогательная функция тегов"
ms.author: riande
manager: wpickett
ms.date: 7/11/2017
ms.topic: article
ms.technology: aspnet
ms.prod: aspnet-core
uid: mvc/views/tag-helpers/builtin-th/Index
ms.openlocfilehash: 3f47cc571eff0c522aaf6543de58f158835384d4
ms.sourcegitcommit: 0b6c8e6d81d2b3c161cd375036eecbace46a9707
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 08/11/2017
---
# <a name="aspnet-core-built-in-tag-helpers"></a><span data-ttu-id="4c74f-104">Встроенные вспомогательные функции тегов ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="4c74f-104">ASP.NET Core built-in Tag Helpers</span></span>

<span data-ttu-id="4c74f-105">Автор: [Питер Кельнер (Peter Kellner)](http://peterkellner.net)</span><span class="sxs-lookup"><span data-stu-id="4c74f-105">By [Peter Kellner](http://peterkellner.net)</span></span> 

<span data-ttu-id="4c74f-106">Платформа ASP.NET Core включает в себя множество вспомогательных функций тегов, которые могут помочь повысить производительность при написании надежного кода.</span><span class="sxs-lookup"><span data-stu-id="4c74f-106">The ASP.NET Core framework includes many Tag Helpers that can help you be more productive in writing robust code.</span></span> <span data-ttu-id="4c74f-107">В этом разделе приводится обзор всех встроенных вспомогательных функций тегов.</span><span class="sxs-lookup"><span data-stu-id="4c74f-107">This section provides an overview of all the built-in Tag Helpers.</span></span>

> [!NOTE]
> <span data-ttu-id="4c74f-108">Существуют встроенные вспомогательные функции тегов, которые не рассматриваются, так как обработчик представлений [Razor](xref:mvc/views/razor) использует их для внутренних целей.</span><span class="sxs-lookup"><span data-stu-id="4c74f-108">There are built-in Tag Helpers which are not discussed, since they are used internally by the [Razor](xref:mvc/views/razor) view engine.</span></span> <span data-ttu-id="4c74f-109">В их число входит вспомогательная функция тегов для символа ~, которая расширяется до корневого пути к веб-сайту.</span><span class="sxs-lookup"><span data-stu-id="4c74f-109">This includes a Tag Helper for the ~ character which expands to the root path of the web site.</span></span>

## <a name="built-in-aspnet-core-tag-helpers"></a><span data-ttu-id="4c74f-110">Встроенные вспомогательные функции тегов ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="4c74f-110">Built-in ASP.NET Core Tag Helpers</span></span>

<span data-ttu-id="4c74f-111">**[Вспомогательная функция тега привязки](xref:mvc/views/tag-helpers/builtin-th/AnchorTagHelper)**</span><span class="sxs-lookup"><span data-stu-id="4c74f-111">**[Anchor Tag Helper](xref:mvc/views/tag-helpers/builtin-th/AnchorTagHelper)**</span></span>

<span data-ttu-id="4c74f-112">**[Вспомогательная функция тега кэша](xref:mvc/views/tag-helpers/builtin-th/CacheTagHelper)**</span><span class="sxs-lookup"><span data-stu-id="4c74f-112">**[Cache Tag Helper](xref:mvc/views/tag-helpers/builtin-th/CacheTagHelper)**</span></span>

<span data-ttu-id="4c74f-113">**[Вспомогательная функция тега распределенного кэша](xref:mvc/views/tag-helpers/builtin-th/DistributedCacheTagHelper)**</span><span class="sxs-lookup"><span data-stu-id="4c74f-113">**[Distributed Cache Tag Helper](xref:mvc/views/tag-helpers/builtin-th/DistributedCacheTagHelper)**</span></span>

<span data-ttu-id="4c74f-114">**[Вспомогательная функция тега среды](xref:mvc/views/tag-helpers/builtin-th/EnvironmentTagHelper)**</span><span class="sxs-lookup"><span data-stu-id="4c74f-114">**[Environment Tag Helper](xref:mvc/views/tag-helpers/builtin-th/EnvironmentTagHelper)**</span></span>

[comment]: **[FormActionTagHelper](xref:mvc/views/tag-helpers/builtin-th/FormActionTagHelper)**

[comment]: **[FormTagTagHelper](xref:mvc/views/tag-helpers/builtin-th/FormTagHelper)**

<span data-ttu-id="4c74f-115">**[Вспомогательная функция тега образа](xref:mvc/views/tag-helpers/builtin-th/ImageTagHelper)**</span><span class="sxs-lookup"><span data-stu-id="4c74f-115">**[Image Tag Helper](xref:mvc/views/tag-helpers/builtin-th/ImageTagHelper)**</span></span>

[comment]: **[InputTagHelper](xref:mvc/views/tag-helpers/builtin-th/InputTagHelper)**

[comment]: **[LabelTagHelper](xref:mvc/views/tag-helpers/builtin-th/LabelTagHelper)**

[comment]: **[LinkTagHelper](xref:mvc/views/tag-helpers/builtin-th/LinkTagHelper)**

[comment]: **[OptionTagHelper](xref:mvc/views/tag-helpers/builtin-th/OptionTagHelper)**

[comment]: **[ScriptTagHelper](xref:mvc/views/tag-helpers/builtin-th/ScriptTagTagHelper)**

[comment]: **[SelectTagHelper](xref:mvc/views/tag-helpers/builtin-th/SelectTagTagHelper)**

[comment]: **[TextAreaTagHelper](xref:mvc/views/tag-helpers/builtin-th/TextAreaTagHelper)**

[comment]: **[ValidationMessageTagHelper](xref:mvc/views/tag-helpers/builtin-th/ValidationMessageTagHelper)**

[comment]: **[ValidationSummaryTagHelper](xref:mvc/views/tag-helpers/builtin-th/ValidationSummaryTagHelper)**  
  
  
<!--

## Additional Resources

REQUIRED These must be xref links, not relative, that is ../../
* [Client-Side Development](../../../client-side/index.md)

* [Tag Helpers](xref:mvc/views/tag-helpers/intro)
-->
