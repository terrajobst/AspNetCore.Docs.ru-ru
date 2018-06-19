---
uid: mvc/overview/older-versions-1/models-data/validating-with-the-idataerrorinfo-interface-cs
title: Проверка с помощью IDataErrorInfo-интерфейс (C#) | Документы Microsoft
author: StephenWalther
description: Стивен Вальтер показано, как для отображения пользовательской проверки ошибок, реализовав интерфейс IDataErrorInfo в классе модели.
ms.author: aspnetcontent
manager: wpickett
ms.date: 03/02/2009
ms.topic: article
ms.assetid: 4733b9f1-9999-48fb-8b73-6038fbcc5ecb
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions-1/models-data/validating-with-the-idataerrorinfo-interface-cs
msc.type: authoredcontent
ms.openlocfilehash: b5028b2e07c4144efa59824885ce96cd8b037dff
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/06/2018
ms.locfileid: "30870522"
---
<a name="validating-with-the-idataerrorinfo-interface-c"></a>Проверка с помощью IDataErrorInfo-интерфейс (C#)
====================
по [Стивен Вальтер](https://github.com/StephenWalther)

> Стивен Вальтер показано, как для отображения пользовательской проверки ошибок, реализовав интерфейс IDataErrorInfo в классе модели.


Целью данного учебника является объяснить одним из подходов к проверке в приложении ASP.NET MVC. Вы научитесь допустить Отправка HTML-формы без указания значения для обязательных полей формы. В этом учебнике вы узнаете, как выполнять проверку с помощью интерфейса IErrorDataInfo.

## <a name="assumptions"></a>Допущения

В этом учебнике используется MoviesDB базы данных и таблицы базы данных фильмов. Эта таблица содержит следующие столбцы:

<a id="0.5_table01"></a>


| **Имя столбца** | **Тип данных** | **Разрешить значения NULL** |
| --- | --- | --- |
| Идентификатор | Int | False |
| Заголовок | Nvarchar(100) | False |
| Директор | Nvarchar(100) | False |
| DateReleased | DateTime | False |


В этом учебнике. я использую Microsoft Entity Framework для создания классов модели моей базы данных. На рисунке 1 отображается фильма класс, созданный платформой Entity Framework.


[![Сущность фильма](validating-with-the-idataerrorinfo-interface-cs/_static/image1.jpg)](validating-with-the-idataerrorinfo-interface-cs/_static/image1.png)

**На рисунке 01**: фильма сущности ([Просмотр полноразмерное изображение](validating-with-the-idataerrorinfo-interface-cs/_static/image2.png))


> [!NOTE] 
> 
> Дополнительные сведения об использовании платформы Entity Framework для создания классов модели базы данных, видят Мои учебника под названием Creating Model Classes с платформой Entity Framework.


## <a name="the-controller-class"></a>Класс контроллера

Мы используем контроллера Home в список фильмов и создание новых фильмов. В список 1 содержится код для этого класса.

**Листинг 1 - Controllers\HomeController.cs**

[!code-csharp[Main](validating-with-the-idataerrorinfo-interface-cs/samples/sample1.cs)]

Класс контроллера Home в листинге 1 содержит два действия Create(). Первое действие отображает HTML-формы для создания новых фильмов. Второе действие Create() выполняет фактическое вставки новых фильма в базу данных. Второе действие Create() вызывается при отправке формы, отображаемого элементом первое действие Create() на сервер.

Обратите внимание, что второе действие Create() содержит следующие строки кода:

[!code-csharp[Main](validating-with-the-idataerrorinfo-interface-cs/samples/sample2.cs)]

Свойство IsValid возвращает значение false, если имеется ошибка проверки. В этом случае отображается повторно создать представление, содержащее форма HTML для создания фильма.

## <a name="creating-a-partial-class"></a>Создание разделяемого класса

Класс фильма создается платформой Entity Framework. Можно просмотреть код для класса фильм, если расширение файла MoviesDBModel.edmx в окне обозревателя решений и откройте в редакторе кода файл MoviesDBModel.Designer.cs (см. рис. 2).


[![Код для сущности фильма](validating-with-the-idataerrorinfo-interface-cs/_static/image2.jpg)](validating-with-the-idataerrorinfo-interface-cs/_static/image3.png)

**На рисунке 02**: код для сущности фильма ([Просмотр полноразмерное изображение](validating-with-the-idataerrorinfo-interface-cs/_static/image4.png))


Класс фильма является частичным классом. Это означает, что можно добавить еще один разделяемый класс с тем же именем, расширяющие функциональные возможности класса фильма. Мы добавим логику проверки в новый разделяемый класс.

Добавьте класс в списке 2 в папке «модели».

**Листинг 2 - Models\Movie.cs**

[!code-csharp[Main](validating-with-the-idataerrorinfo-interface-cs/samples/sample3.cs)]

Обратите внимание, что класс в списке 2 включает *частичного* модификатор. Любые методы или свойства, добавленные к этому классу становятся частью фильма класс, созданный платформой Entity Framework.

## <a name="adding-onchanging-and-onchanged-partial-methods"></a>Добавление OnChanging и OnChanged частичных методов

Если Entity Framework создает класс сущностей, Entity Framework автоматически добавляет разделяемые методы в класс. Entity Framework создает OnChanging и OnChanged разделяемые методы, которые соответствуют каждому свойству класса.

В случае класса фильма Entity Framework создает следующие методы:

- OnIdChanging
- OnIdChanged
- OnTitleChanging
- OnTitleChanged
- OnDirectorChanging
- OnDirectorChanged
- OnDateReleasedChanging
- OnDateReleasedChanged

Метод OnChanging вызывается правой перед изменением соответствующего свойства. Метод OnChanged вызывается справа после изменения свойства.

Можно воспользоваться преимуществами этих разделяемые методы для добавления логики проверки класс фильма. Обновления класса фильма в списке 3 проверяет, что свойства Title и директора назначены непустых значений.

> [!NOTE] 
> 
> Разделяемый метод — это метод, определенный в классе, который не является обязательным для реализации. Если не реализовать частичный метод компилятор удаляет сигнатуру метода, и все вызовы метода, поэтому являются не во время выполнения затраты, связанные с разделяемого метода. В редакторе кода Visual Studio можно добавить разделяемого метода, введя ключевое слово *частичного* за которыми следует пробел, чтобы просмотреть список частичными репликами для реализации.


**Листинг 3 - Models\Movie.cs**

[!code-csharp[Main](validating-with-the-idataerrorinfo-interface-cs/samples/sample4.cs)]

Например, при попытке присвоить свойству заголовка пустая строка, то сообщение об ошибке присваивается словарь с именем \_ошибок.

На этом этапе ничего не фактически произойдет, если назначить пустую строку для свойства Title и коллекцию ошибок добавляется к закрытому \_ошибки поля. Нам нужно реализовать интерфейс IDataErrorInfo для предоставления этих ошибок проверки для платформы ASP.NET MVC.

## <a name="implementing-the-idataerrorinfo-interface"></a>Реализация IDataErrorInfo-интерфейс

IDataErrorInfo-интерфейс был частью .NET framework, начиная с первой версии. Этот интерфейс является очень простым:

[!code-csharp[Main](validating-with-the-idataerrorinfo-interface-cs/samples/sample5.cs)]

Если класс реализует интерфейс IDataErrorInfo, платформа ASP.NET MVC будет использовать этот интерфейс, при создании экземпляра класса. Например контроллер Home действие Create() принимает экземпляр класса фильма:

[!code-csharp[Main](validating-with-the-idataerrorinfo-interface-cs/samples/sample6.cs)]

Платформа ASP.NET MVC создает экземпляр фильм, переданные в действие Create() с помощью связывателя модели (DefaultModelBinder). Связыватель модели отвечает за создание экземпляра объекта фильма путем привязки к экземпляру объекта фильма поля формы HTML.

DefaultModelBinder обнаруживает ли класс реализует интерфейс IDataErrorInfo. Если класс реализует этот интерфейс связывателя модели вызывает IDataErrorInfo.this индексатора для каждого свойства класса. Если индексатор возвращает сообщение об ошибке связывателя модели добавляет это сообщение об ошибке для моделирования состояние автоматически.

DefaultModelBinder также проверяет свойство IDataErrorInfo.Error. Это свойство предназначено для представления ошибки проверки конкретных не свойства, связанные с классом. Например можно применить правило проверки, зависит от значений нескольких свойств класса фильма. В этом случае вернет ошибку проверки из свойства ошибки.

Обновленный класс фильма в листинге 4 реализует интерфейс IDataErrorInfo.

**Листинг 4 - Models\Movie.cs (реализует IDataErrorInfo)**

[!code-csharp[Main](validating-with-the-idataerrorinfo-interface-cs/samples/sample7.cs)]

В листинге 4 проверяет свойство индексатора \_коллекцию ошибок, содержит ли ключ, который соответствует имени свойства, переданный индексатору. Если ошибки проверки, не связанный со свойством, возвращается пустая строка.

Не требуется каким-либо образом с помощью измененного класса фильма контроллер Home. Страницы, отображаемой на рис. 3 показано, что произойдет, если значение не указано для заголовка или директора поля формы.


[![Автоматическое создание методов действий](validating-with-the-idataerrorinfo-interface-cs/_static/image3.jpg)](validating-with-the-idataerrorinfo-interface-cs/_static/image5.png)

**На рисунке 03**: форма с отсутствующими значениями ([Просмотр полноразмерное изображение](validating-with-the-idataerrorinfo-interface-cs/_static/image6.png))


Обратите внимание, что DateReleased проверить автоматически. Так как свойство DateReleased не может принимать значение NULL, DefaultModelBinder приводит к возникновению ошибки проверки для этого свойства автоматически при не имеет значение. Если вы хотите изменить сообщение об ошибке для свойства DateReleased необходимо создать настраиваемый связыватель модели.

## <a name="summary"></a>Сводка

В этом учебнике вы узнали, как использовать интерфейс IDataErrorInfo для формирования сообщений об ошибках проверки. Во-первых мы создали разделяемый класс фильм, который расширяет функциональность в разделяемый класс фильм, созданные платформой Entity Framework. Далее мы добавили логику проверки в фильма класса OnTitleChanging() и OnDirectorChanging() разделяемые методы. Наконец мы реализовали IDataErrorInfo-интерфейс для предоставления этих сообщений проверки с платформой ASP.NET MVC.

> [!div class="step-by-step"]
> [Назад](performing-simple-validation-cs.md)
> [Вперед](validating-with-a-service-layer-cs.md)
