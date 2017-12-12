---
uid: web-api/overview/data/using-web-api-with-entity-framework/part-5
title: "Создать объекты передачи данных (DTO) | Документы Microsoft"
author: MikeWasson
description: 
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/16/2014
ms.topic: article
ms.assetid: 0fd07176-b74b-48f0-9fac-0f02e3ffa213
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/data/using-web-api-with-entity-framework/part-5
msc.type: authoredcontent
ms.openlocfilehash: e179dcd52200734a45c84b3c7c3069a4727904c1
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 11/10/2017
---
<a name="create-data-transfer-objects-dtos"></a>Создать объекты передачи данных (DTO)
====================
по [Mike Wasson](https://github.com/MikeWasson)

[Загрузка завершенного проекта](https://github.com/MikeWasson/BookService)

Справа теперь наших веб-API предоставляет сущностей базы данных на клиент. Клиент получает данные, которые сопоставляются непосредственно в таблицах базы данных. Однако это не всегда рекомендуется. Иногда требуется изменить форму данных, отправляемых клиенту. Например, можно сделать следующее:

- Удалите циклические ссылки (см. выше).
- Скрыть определенным свойствам, которые клиенты не могут просмотреть.
- Пропустите некоторые свойства, чтобы уменьшить размер полезных данных.
- Сведение графов объектов, содержащих вложенные объекты, чтобы сделать их более удобным для клиентов.
- Избегайте «чрезмерно учета» уязвимостей. (См. [проверка модели](../../formats-and-model-binding/model-validation-in-aspnet-web-api.md) обсуждение чрезмерное учета.)
- Отделяете на уровне службы из вашего уровня базы данных.

Чтобы сделать это, можно определить *объект передачи данных* (DTO). Объект DTO — это объект, который определяет, каким образом данные будут отправлены по сети. Давайте посмотрим, как это работает с сущностью книги. В папке Models находится добавьте два класса DTO:

[!code-csharp[Main](part-5/samples/sample1.cs)]

`BookDetailDTO` Класс включает все свойства из книги модели, за исключением того, что `AuthorName` является строкой, которая будет содержать имя автора. `BookDTO` Класс содержит подмножество свойств из `BookDetailDTO`.

Затем замените два метода GET в `BooksController` класса с версиями, которые возвращают DTO. Мы будем использовать LINQ **выберите** инструкции для преобразования из сущностей книг в DTO.

[!code-csharp[Main](part-5/samples/sample2.cs)]

Ниже приведен код SQL, созданный новый `GetBooks` метод. Вы увидите, что EF преобразует LINQ **выберите** в инструкцию SQL SELECT.

[!code-sql[Main](part-5/samples/sample3.sql)]

Наконец, измените `PostBook` метод для возврата DTO.

[!code-csharp[Main](part-5/samples/sample4.cs)]

> [!NOTE]
> В этом учебнике мы преобразование DTO вручную в коде. Другой вариант — использовать библиотеку как [AutoMapper](http://automapper.org/) , обрабатывает преобразование автоматически.

>[!div class="step-by-step"]
[Назад](part-4.md)
[Вперед](part-6.md)
