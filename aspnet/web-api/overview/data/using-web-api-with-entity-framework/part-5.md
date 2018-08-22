---
uid: web-api/overview/data/using-web-api-with-entity-framework/part-5
title: Создание объектов передачи данных (DTO) | Документация Майкрософт
author: MikeWasson
description: ''
ms.author: riande
ms.date: 06/16/2014
ms.assetid: 0fd07176-b74b-48f0-9fac-0f02e3ffa213
msc.legacyurl: /web-api/overview/data/using-web-api-with-entity-framework/part-5
msc.type: authoredcontent
ms.openlocfilehash: 95075dd748f0fe4eb6d1c52d6bfe4a4576653b4c
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 08/16/2018
ms.locfileid: "41836994"
---
<a name="create-data-transfer-objects-dtos"></a>Создание объектов передачи данных (DTO)
====================
по [Майк Уоссон](https://github.com/MikeWasson)

[Скачать завершенный проект](https://github.com/MikeWasson/BookService)

Справа теперь наши веб-API предоставляет сущности базы данных клиенту. Клиент получает данные, прямо таблиц базы данных. Тем не менее это не всегда хорошая идея. Иногда требуется изменить форму данных, отправляемых клиенту. Например, можно сделать следующее:

- Удалите циклические ссылки, (см. выше).
- Скрыть определенных свойств, которые клиенты не должны просматривать.
- Исключить некоторые свойства, чтобы уменьшить размер полезных данных.
- Обработка графов объектов, содержащих вложенные объекты, чтобы сделать их более удобным для клиентов.
- Избегайте «уязвимости типа overposting». (См. в разделе [проверки модели](../../formats-and-model-binding/model-validation-in-aspnet-web-api.md) обсуждение от чрезмерной передачи данных.)
- Отделить уровень службы из вашего уровня базы данных.

Для этого можно определить *объект передачи данных* (DTO). DTO — это объект, который определяет, каким образом данные будут отправляться по сети. Давайте посмотрим, как это работает с сущностью книги. В папке Models добавьте два классы объектов передачи данных:

[!code-csharp[Main](part-5/samples/sample1.cs)]

`BookDetailDTO` Класс включает все свойства из модели книги, за исключением случаев, `AuthorName` представляет собой строку, которая будет содержать имя автора. `BookDTO` Класс содержит подмножество свойств из `BookDetailDTO`.

Затем замените два метода GET в `BooksController` класса с версиями, которые возвращают DTO. Мы будем использовать LINQ **выберите** инструкции для преобразования из сущностей книг в DTO.

[!code-csharp[Main](part-5/samples/sample2.cs)]

Ниже приведен код SQL, созданный по новому `GetBooks` метод. Вы увидите, что EF преобразует LINQ **выберите** в инструкцию SQL SELECT.

[!code-sql[Main](part-5/samples/sample3.sql)]

Наконец, измените `PostBook` метод вернет объект DTO.

[!code-csharp[Main](part-5/samples/sample4.cs)]

> [!NOTE]
> В этом руководстве мы преобразовали в DTO вручную в коде. Другой вариант — использовать библиотеку как [AutoMapper](http://automapper.org/) , обрабатывает преобразование автоматически.
> 
> [!div class="step-by-step"]
> [Назад](part-4.md)
> [Вперед](part-6.md)
