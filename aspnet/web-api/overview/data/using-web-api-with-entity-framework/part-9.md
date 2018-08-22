---
uid: web-api/overview/data/using-web-api-with-entity-framework/part-9
title: Добавить новый элемент в базу данных | Документация Майкрософт
author: MikeWasson
description: ''
ms.author: riande
ms.date: 06/16/2014
ms.assetid: 0967c29e-e124-4db0-a788-c45d0ff5aff2
msc.legacyurl: /web-api/overview/data/using-web-api-with-entity-framework/part-9
msc.type: authoredcontent
ms.openlocfilehash: 01586ef6eecdbe1acfadfcfcc0c9ca8b442de2d3
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 08/16/2018
ms.locfileid: "41824095"
---
<a name="add-a-new-item-to-the-database"></a><span data-ttu-id="06287-102">Добавить новый элемент в базу данных</span><span class="sxs-lookup"><span data-stu-id="06287-102">Add a New Item to the Database</span></span>
====================
<span data-ttu-id="06287-103">по [Майк Уоссон](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="06287-103">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

[<span data-ttu-id="06287-104">Скачать завершенный проект</span><span class="sxs-lookup"><span data-stu-id="06287-104">Download Completed Project</span></span>](https://github.com/MikeWasson/BookService)

<span data-ttu-id="06287-105">В этом разделе вы добавите возможность для пользователей создать новую книгу.</span><span class="sxs-lookup"><span data-stu-id="06287-105">In this section, you will add the ability for users to create a new book.</span></span> <span data-ttu-id="06287-106">В файле app.js добавьте следующий код для модели представления:</span><span class="sxs-lookup"><span data-stu-id="06287-106">In app.js, add the following code to the view model:</span></span>

[!code-javascript[Main](part-9/samples/sample1.js)]

<span data-ttu-id="06287-107">В файле Index.cshtml Замените следующую разметку:</span><span class="sxs-lookup"><span data-stu-id="06287-107">In Index.cshtml, replace the following markup:</span></span>

[!code-html[Main](part-9/samples/sample2.html)]

<span data-ttu-id="06287-108">На:</span><span class="sxs-lookup"><span data-stu-id="06287-108">With:</span></span>

[!code-html[Main](part-9/samples/sample3.html)]

<span data-ttu-id="06287-109">Эта разметка создает формы для отправки нового автора.</span><span class="sxs-lookup"><span data-stu-id="06287-109">This markup creates a form for submitting a new author.</span></span> <span data-ttu-id="06287-110">Значения для раскрывающегося списка автор привязаны к `authors` наблюдаемые в модели представления.</span><span class="sxs-lookup"><span data-stu-id="06287-110">The values for the author drop-down list are data-bound to the `authors` observable in the view model.</span></span> <span data-ttu-id="06287-111">Для других формы ввода данных, значения привязаны к `newBook` свойство модели представления.</span><span class="sxs-lookup"><span data-stu-id="06287-111">For the other form inputs, the values are data-bound to the `newBook` property of the view model.</span></span>

<span data-ttu-id="06287-112">Обработчик события submit в форме привязан к `addBook` функции:</span><span class="sxs-lookup"><span data-stu-id="06287-112">The submit handler on the form is bound to the `addBook` function:</span></span>

[!code-html[Main](part-9/samples/sample4.html)]

<span data-ttu-id="06287-113">`addBook` Функция считывает текущие значения входных параметров формы с привязкой к данным для создания объекта JSON.</span><span class="sxs-lookup"><span data-stu-id="06287-113">The `addBook` function reads the current values of the data-bound form inputs to create a JSON object.</span></span> <span data-ttu-id="06287-114">Затем она отправляет объект JSON для `/api/books`.</span><span class="sxs-lookup"><span data-stu-id="06287-114">Then it POSTs the JSON object to `/api/books`.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="06287-115">[Назад](part-8.md)
> [Вперед](part-10.md)</span><span class="sxs-lookup"><span data-stu-id="06287-115">[Previous](part-8.md)
[Next](part-10.md)</span></span>
