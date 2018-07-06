---
uid: mvc/overview/older-versions-1/models-data/validating-with-the-idataerrorinfo-interface-vb
title: Проверка с помощью интерфейса IDataErrorInfo (VB) | Документация Майкрософт
author: StephenWalther
description: Стивен Вальтер показано, как для отображения пользовательской проверки ошибок путем реализации интерфейса IDataErrorInfo в классе модели.
ms.author: aspnetcontent
ms.date: 03/02/2009
ms.assetid: 3a8a9d9f-82dd-4959-b7c6-960e9ce95df1
msc.legacyurl: /mvc/overview/older-versions-1/models-data/validating-with-the-idataerrorinfo-interface-vb
msc.type: authoredcontent
ms.openlocfilehash: 55716b48a7991424a4d03bbbc7d96f8f12f8dabb
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 07/05/2018
ms.locfileid: "37834023"
---
<a name="validating-with-the-idataerrorinfo-interface-vb"></a>Проверка с помощью интерфейса IDataErrorInfo (VB)
====================
по [Стивен Вальтер](https://github.com/StephenWalther)

> Стивен Вальтер показано, как для отображения пользовательской проверки ошибок путем реализации интерфейса IDataErrorInfo в классе модели.


Этот учебник призван объяснить одним из подходов к проверке в ASP.NET MVC-приложениях. Вы узнаете, как запретить кто-то отправка формы HTML без указания значения для обязательных полей формы. В этом руководстве вы узнаете, как выполнять проверку с помощью интерфейса IErrorDataInfo.

## <a name="assumptions"></a>Допущения

В данном случае я использую MoviesDB базы данных и таблицы базы данных фильмов. Эта таблица содержит следующие столбцы:

<a id="0.6_table01"></a>


| **Имя столбца** | **Тип данных** | **Разрешить значения NULL** |
| --- | --- | --- |
| Идентификатор | Int | False |
| Заголовок | Nvarchar(100) | False |
| Директор | Nvarchar(100) | False |
| DateReleased | DateTime | False |


В данном случае я использую Microsoft Entity Framework для создания Мои классы модели базы данных. Класс Movie, созданным Entity Framework отображается на рис. 1.


[![Сущность фильма](validating-with-the-idataerrorinfo-interface-vb/_static/image1.jpg)](validating-with-the-idataerrorinfo-interface-vb/_static/image1.png)

**Рис 01**: сущности фильма ([Просмотр полноразмерного изображения](validating-with-the-idataerrorinfo-interface-vb/_static/image2.png))


> [!NOTE] 
> 
> Дополнительные сведения об использовании платформы Entity Framework для создания классов модели базы данных, см. Мой руководстве Creating Model Classes with Entity Framework.


## <a name="the-controller-class"></a>Класс контроллера

Мы используем контроллера Home список фильмов и создание новых фильмов. В листинге 1 содержится код для этого класса.

**В листинге 1 - Controllers\HomeController.vb**

[!code-vb[Main](validating-with-the-idataerrorinfo-interface-vb/samples/sample1.vb)]

Класс контроллера Home в листинге 1 содержит два действия Create(). Первое действие отображает HTML-форма для создания новый фильм. Второе действие Create() выполняет фактическое Вставка новый фильм в базе данных. Второе действие Create() вызывается, когда форму с первое действие Create() отправляется на сервер.

Обратите внимание на то, что второе действие Create() содержит следующие строки кода:

[!code-vb[Main](validating-with-the-idataerrorinfo-interface-vb/samples/sample2.vb)]

Свойство IsValid возвращает значение false, если возникает ошибка проверки. В этом случае следует создать представление, содержащее HTML-форма для создания фильма отобразится.

## <a name="creating-a-partial-class"></a>Создание разделяемого класса

Класс Movie создается платформой Entity Framework. Вы увидите код для класса фильма, если развернуть файл MoviesDBModel.edmx в окне обозревателя решений и откройте файл MoviesDBModel.Designer.vb в редакторе кода (см. рис. 2).


[![Код для сущности фильма](validating-with-the-idataerrorinfo-interface-vb/_static/image2.jpg)](validating-with-the-idataerrorinfo-interface-vb/_static/image3.png)

**Рис. 02**: код для сущности фильма ([Просмотр полноразмерного изображения](validating-with-the-idataerrorinfo-interface-vb/_static/image4.png))


Класс Movie — это разделяемый класс. Это означает, что мы можем добавить еще один разделяемый класс с тем же именем, чтобы расширить функциональные возможности класса Movie. Мы добавим логику проверки в новый разделяемый класс.

Добавление класса в листинге 2 папку Models.

**В листинге 2 - Models\Movie.vb**

[!code-vb[Main](validating-with-the-idataerrorinfo-interface-vb/samples/sample3.vb)]

Обратите внимание, что класс в листинге 2 включает *частичного* модификатор. Все методы и свойства, добавляемые к этому классу становятся частью класс Movie, созданным Entity Framework.

## <a name="adding-onchanging-and-onchanged-partial-methods"></a>Добавление OnChanging и OnChanged частичных методов

Когда платформа Entity Framework создает класс сущностей, Entity Framework автоматически добавляет разделяемые методы в класс. Платформа Entity Framework создает OnChanging и OnChanged разделяемые методы, соответствующие каждому свойству класса.

В случае класс Movie Entity Framework создает следующие методы:

- OnIdChanging
- OnIdChanged
- OnTitleChanging
- OnTitleChanged
- OnDirectorChanging
- OnDirectorChanged
- OnDateReleasedChanging
- OnDateReleasedChanged

Метод OnChanging вызывается правой перед изменением соответствующего свойства. Метод OnChanged вызывается справа после изменения свойства.

Можно воспользоваться преимуществами эти разделяемые методы для добавления логики проверки класс Movie. Обновление класс Movie в листинге 3 проверяет, что в свойствах Title и директор назначаются непустых значений.

> [!NOTE] 
> 
> Разделяемый метод — это метод, определенный в классе, который не является обязательным для реализации. Если не реализовать разделяемый метод, компилятор удаляет сигнатуру метода, и все вызовы метода таким образом являются без затрат времени выполнения, связанных с разделяемого метода. В редакторе кода Visual Studio, можно добавить разделяемый метод, введя ключевое слово *частичного* за которыми следует пробел, чтобы просмотреть список частичных представлений для реализации.


**Листинг 3 - Models\Movie.vb**

[!code-vb[Main](validating-with-the-idataerrorinfo-interface-vb/samples/sample4.vb)]

Например, если вы попытаетесь назначить пустую строку в свойстве Title, тогда сообщение об ошибке назначается словарь с именем \_ошибки.

На этом этапе ничего не произойдет при пустая строка присвоить свойству заголовка и ошибка добавляется к закрытому \_полей ошибок. Нам нужно реализовать интерфейс IDataErrorInfo для предоставления этих ошибок проверки в ASP.NET MVC Framework.

## <a name="implementing-the-idataerrorinfo-interface"></a>Вы реализуете интерфейс IDataErrorInfo

Интерфейс IDataErrorInfo было частью .NET framework с первой версии. Этот интерфейс является очень простой интерфейс:

[!code-vb[Main](validating-with-the-idataerrorinfo-interface-vb/samples/sample5.vb)]

Если класс реализует интерфейс IDataErrorInfo, платформа ASP.NET MVC будет использовать этот интерфейс при создании экземпляра класса. Например контроллер Home Create() действие принимает экземпляр класса Movie:

[!code-vb[Main](validating-with-the-idataerrorinfo-interface-vb/samples/sample6.vb)]

Платформа ASP.NET MVC создает экземпляр фильма, передается в действие Create() с помощью связывателя модели (DefaultModelBinder). Связыватель модели отвечает за создание экземпляра объекта фильма, привязывая поля формы HTML для экземпляра объекта фильма.

DefaultModelBinder обнаруживает ли класс реализует интерфейс IDataErrorInfo. Если класс реализует этот интерфейс связывателя модели вызывает IDataErrorInfo.this индексатор для каждого свойства класса. Если индексатор возвращает сообщение об ошибке это сообщение об ошибке для моделирования состояние автоматически добавит связывателя модели.

DefaultModelBinder также проверяет свойство IDataErrorInfo.Error. Это свойство предназначено для представления ошибок проверки отличных от свойств, связанный с классом. Например может потребоваться применить правило проверки, который зависит от значения нескольких свойств класса фильма. В этом случае возвратит ошибку проверки из свойства ошибки.

Обновленный класс Movie в листинге 4 реализует интерфейс IDataErrorInfo.

**Листинг 4 - Models\Movie.vb (реализует IDataErrorInfo)**

[!code-vb[Main](validating-with-the-idataerrorinfo-interface-vb/samples/sample7.vb)]

В листинге 4 проверяет свойство индексатора \_коллекцию ошибок, чтобы увидеть, если он содержит ключ, который соответствует имени свойства, передаваемое индексатора. При возникновении ошибки проверки, связанные со свойством, возвращается пустая строка.

Не нужно каким-либо образом использовать измененный класс Movie контроллер Home. Страницы, отображаемой на рис. 3 показано, что произойдет, если значение не указано для заголовка или Директор полей формы.


[![Автоматическое создание методов действий](validating-with-the-idataerrorinfo-interface-vb/_static/image3.jpg)](validating-with-the-idataerrorinfo-interface-vb/_static/image5.png)

**Рис 03**: форма с отсутствующими значениями ([Просмотр полноразмерного изображения](validating-with-the-idataerrorinfo-interface-vb/_static/image6.png))


Обратите внимание на то, что значение DateReleased проверяется автоматически. Так как свойство DateReleased не допускает значения NULL, DefaultModelBinder выдает ошибку проверки для этого свойства автоматически при его не имеет значения. Если вы хотите изменить сообщение об ошибке для свойства DateReleased необходимо создать настраиваемый связыватель модели.

## <a name="summary"></a>Сводка

В этом руководстве вы узнали, как использовать интерфейс IDataErrorInfo для формирования сообщений об ошибках проверки. Во-первых мы создали разделяемый класс Movie, который расширяет функциональность разделяемый класс Movie, созданным Entity Framework. Затем мы добавили логику проверки в фильма OnTitleChanging() и OnDirectorChanging() частичные методы класса. Наконец мы реализовали интерфейс IDataErrorInfo, чтобы предоставить эти сообщения проверки в ASP.NET MVC Framework.

> [!div class="step-by-step"]
> [Назад](performing-simple-validation-vb.md)
> [Вперед](validating-with-a-service-layer-vb.md)
