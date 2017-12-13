---
uid: web-api/overview/data/using-web-api-with-entity-framework/part-9
title: "Добавление нового элемента в базе данных | Документы Microsoft"
author: MikeWasson
description: 
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/16/2014
ms.topic: article
ms.assetid: 0967c29e-e124-4db0-a788-c45d0ff5aff2
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/data/using-web-api-with-entity-framework/part-9
msc.type: authoredcontent
ms.openlocfilehash: d33355b1bd286513958f71ce5521942a6cbb584f
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 11/10/2017
---
<a name="add-a-new-item-to-the-database"></a><span data-ttu-id="7466a-102">Добавление нового элемента в базе данных</span><span class="sxs-lookup"><span data-stu-id="7466a-102">Add a New Item to the Database</span></span>
====================
<span data-ttu-id="7466a-103">по [Mike Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="7466a-103">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

[<span data-ttu-id="7466a-104">Загрузка завершенного проекта</span><span class="sxs-lookup"><span data-stu-id="7466a-104">Download Completed Project</span></span>](https://github.com/MikeWasson/BookService)

<span data-ttu-id="7466a-105">В этом разделе будет добавлена возможность для пользователей создать новую книгу.</span><span class="sxs-lookup"><span data-stu-id="7466a-105">In this section, you will add the ability for users to create a new book.</span></span> <span data-ttu-id="7466a-106">В файле app.js добавьте следующий код для модели представления:</span><span class="sxs-lookup"><span data-stu-id="7466a-106">In app.js, add the following code to the view model:</span></span>

[!code-javascript[Main](part-9/samples/sample1.js)]

<span data-ttu-id="7466a-107">В Index.cshtml Замените следующую разметку:</span><span class="sxs-lookup"><span data-stu-id="7466a-107">In Index.cshtml, replace the following markup:</span></span>

[!code-html[Main](part-9/samples/sample2.html)]

<span data-ttu-id="7466a-108">с:</span><span class="sxs-lookup"><span data-stu-id="7466a-108">With:</span></span>

[!code-html[Main](part-9/samples/sample3.html)]

<span data-ttu-id="7466a-109">Эта разметка создает формы для отправки нового автора.</span><span class="sxs-lookup"><span data-stu-id="7466a-109">This markup creates a form for submitting a new author.</span></span> <span data-ttu-id="7466a-110">Значения для списка автор привязаны к `authors` наблюдаемый в модели представления.</span><span class="sxs-lookup"><span data-stu-id="7466a-110">The values for the author drop-down list are data-bound to the `authors` observable in the view model.</span></span> <span data-ttu-id="7466a-111">Форма входов значения привязаны к `newBook` свойство модели представления.</span><span class="sxs-lookup"><span data-stu-id="7466a-111">For the other form inputs, the values are data-bound to the `newBook` property of the view model.</span></span>

<span data-ttu-id="7466a-112">Обработчик отправки формы привязан к `addBook` функции:</span><span class="sxs-lookup"><span data-stu-id="7466a-112">The submit handler on the form is bound to the `addBook` function:</span></span>

[!code-html[Main](part-9/samples/sample4.html)]

<span data-ttu-id="7466a-113">`addBook` Функция считывает текущие значения входных данных формы с привязкой к данным для создания объекта JSON.</span><span class="sxs-lookup"><span data-stu-id="7466a-113">The `addBook` function reads the current values of the data-bound form inputs to create a JSON object.</span></span> <span data-ttu-id="7466a-114">Затем он отправляет объект JSON для `/api/books`.</span><span class="sxs-lookup"><span data-stu-id="7466a-114">Then it POSTs the JSON object to `/api/books`.</span></span>

>[!div class="step-by-step"]
<span data-ttu-id="7466a-115">[Назад](part-8.md)
[Вперед](part-10.md)</span><span class="sxs-lookup"><span data-stu-id="7466a-115">[Previous](part-8.md)
[Next](part-10.md)</span></span>
