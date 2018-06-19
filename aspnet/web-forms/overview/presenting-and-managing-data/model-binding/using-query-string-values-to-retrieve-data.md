---
uid: web-forms/overview/presenting-and-managing-data/model-binding/using-query-string-values-to-retrieve-data
title: С помощью значения строки запроса для фильтрации данных с использованием привязки модели и веб-формы | Документы Microsoft
author: tfitzmac
description: Этот учебник ряд демонстрирует основные аспекты использования привязки модели в проекте веб-форм ASP.NET. Привязка модели позволяет взаимодействия с данными дополнительные прямые-...
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/27/2014
ms.topic: article
ms.assetid: b90978bd-795d-4871-9ade-1671caff5730
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/presenting-and-managing-data/model-binding/using-query-string-values-to-retrieve-data
msc.type: authoredcontent
ms.openlocfilehash: 03d20decf0eeff6062fbc6f8dd66f644b405c7cc
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/06/2018
ms.locfileid: "30886827"
---
<a name="using-query-string-values-to-filter-data-with-model-binding-and-web-forms"></a>С помощью значения строки запроса для фильтрации данных с привязкой модели и веб-форм
====================
по [Tom FitzMacken](https://github.com/tfitzmac)

> Этот учебник ряд демонстрирует основные аспекты использования привязки модели в проекте веб-форм ASP.NET. Привязка модели позволяет взаимодействия с данными более прост, чем работе с данными источника объектов (например, элемент управления ObjectDataSource или SqlDataSource). Эта серия начинается с вводный материал и перемещает на следующих занятиях рассматривается более сложных понятиях.
> 
> Этого учебника показано, как передать значение в строку запроса и использовать это значение для получения данных через привязки модели.
> 
> Этот учебник построен на проект, созданный в [более ранних](retrieving-data.md) части ряда.
> 
> Вы можете [загрузки](https://go.microsoft.com/fwlink/?LinkId=286116) весь проект C# или Visual Basic. Загружаемый код работает с Visual Studio 2012 или Visual Studio 2013. Она использует шаблон Visual Studio 2012, который немного отличается от шаблона Visual Studio 2013, показанными в данном руководстве.


## <a name="what-youll-build"></a>Мы создадим

В этом учебнике вы сможете:

1. Добавьте новую страницу, чтобы отобразить Зарегистрированные курсы для учащихся
2. Получить зарегистрированных курсов для выбранного учащегося на основе значения в строке запроса
3. Добавить гиперссылку со значением строки запроса из представления сетки на новую страницу

Действия, описанные в этом учебнике довольно аналогично тому, что вы делали в предыдущих [учебника](sorting-paging-and-filtering-data.md) для фильтрации отображаемых учащихся, основанному на выборе пользователя в раскрывающемся списке. В этом учебнике используется **управления** атрибута в методе select, чтобы указать, что значение параметра берется из элемента управления. В этом учебнике мы используем **QueryString** атрибута в методе select, чтобы указать, что значение параметра берется из строки запроса.

## <a name="add-new-page-for-displaying-a-students-courses"></a>Добавить новую страницу для отображения Стьюдента курсов

Добавление нового веб-формы, использующей главную страницу Site.master и назовите страницу **курсы**.

В **Courses.aspx** файл, добавить представление сетки для отображения курсов для выбранного учащегося.

[!code-aspx[Main](using-query-string-values-to-retrieve-data/samples/sample1.aspx)]

## <a name="define-the-select-method"></a>Определение метода select

В **Courses.aspx.cs**, вы добавите выберите метод с именем, указанным в представлении сетки **SelectMethod** свойство. В этом методе будет определить запрос для получения Стьюдента курсов, а также указать, что параметр берется из значение строки запроса с тем же именем в качестве параметра.

Во-первых, необходимо добавить следующие **с помощью** инструкции.

[!code-csharp[Main](using-query-string-values-to-retrieve-data/samples/sample2.cs)]

Затем добавьте следующий код Courses.aspx.cs:

[!code-csharp[Main](using-query-string-values-to-retrieve-data/samples/sample3.cs)]

Атрибута QueryString означает, что значение строки запроса с именем идентификатором StudentID автоматически назначается параметр этого метода.

## <a name="add-hyperlink-with-query-string-value"></a>Добавьте гиперссылку на значение строки запроса

В представлении сетки на Students.aspx добавляется поле гиперссылки, ссылающуюся на новую страницу курсов. Гиперссылка будет содержать значение строки запроса с идентификатором Стьюдента.

В Students.aspx добавьте следующее поле к столбцам представления сетки под поле для всего кредиты.

[!code-aspx[Main](using-query-string-values-to-retrieve-data/samples/sample4.aspx?highlight=7-8)]

Запустите приложение и обратите внимание, представления сетки теперь содержит курсы ссылку.

![Добавление гиперссылки](using-query-string-values-to-retrieve-data/_static/image1.png)

Если щелкнуть одну из ссылок, вы увидите этого студента зарегистрированных курсов.

![Показать курсы](using-query-string-values-to-retrieve-data/_static/image2.png)

## <a name="conclusion"></a>Заключение

В этом учебнике вы добавили ссылку со значением строки запроса. Используется, значение строки запроса для значения параметра в методе select.

В следующем [учебника](adding-business-logic-layer.md), будет переместить код из файлов кода в слой бизнес-логики и доступа к данным.

> [!div class="step-by-step"]
> [Назад](integrating-jquery-ui.md)
> [Вперед](adding-business-logic-layer.md)
