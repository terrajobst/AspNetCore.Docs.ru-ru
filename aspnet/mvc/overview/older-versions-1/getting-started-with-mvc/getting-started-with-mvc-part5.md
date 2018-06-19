---
uid: mvc/overview/older-versions-1/getting-started-with-mvc/getting-started-with-mvc-part5
title: Доступ к вашей модели данных от контроллера | Документы Microsoft
author: shanselman
description: Это руководство для новичков, в котором представлены основные сведения по ASP.NET MVC. Создание простого веб-приложения, чтение и запись из базы данных.
ms.author: aspnetcontent
manager: wpickett
ms.date: 08/14/2010
ms.topic: article
ms.assetid: 004703cd-e0e9-4ba7-9974-1b0475c71222
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions-1/getting-started-with-mvc/getting-started-with-mvc-part5
msc.type: authoredcontent
ms.openlocfilehash: 2ba1b73f40a920e27e4a03d9f703e62054d3f25c
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/10/2018
ms.locfileid: "30876440"
---
<a name="accessing-your-models-data-from-a-controller"></a>Доступ к вашей модели данных из контроллера
====================
по [Скотт Хансельман](https://github.com/shanselman)

> Это руководство для новичков, в котором представлены основные сведения по ASP.NET MVC. Вы создадите простое веб-приложение выполняет чтение и запись из базы данных. Посетите [центр обучения ASP.NET MVC](../../../index.md) для поиска других ASP.NET MVC, учебники и примеры.


В этом разделе мы собираемся создавать новый класс MoviesController и написать код, получающий наши данные фильма и отображает его обратно в браузер, с использованием шаблона представления.

Щелкните правой кнопкой папку Controllers и сделать новый MoviesController.

[![Добавление контроллера](getting-started-with-mvc-part5/_static/image2.png)](getting-started-with-mvc-part5/_static/image1.png)

Это создаст новый файл «MoviesController.cs» под нашей \Controllers папку в данном проекте. Давайте обновить MovieController, чтобы получить список фильмов из наших заполненными базы данных.

[!code-csharp[Main](getting-started-with-mvc-part5/samples/sample1.cs)]

Мы занимаемся запрос LINQ, чтобы только извлечение фильмы, выпущенным после лето 1984. Он понадобится Просмотр шаблона отрисовки этот список фильмов обратно, поэтому щелкните правой кнопкой мыши в методе и выберите Добавить представление для его создания.

В диалоговом окне Добавление представления мы будем указывать мы передаем список&lt;Movies.Models.Movie&gt; для наших шаблона представления. В отличие от ранее измеренным временем мы используется диалоговое окно Добавить представление и создать шаблон «Empty» это время мы будем указать, что мы хотим автоматически» на основе скаффолдинга» Просмотр шаблона нам с содержимое по умолчанию в Visual Studio. Это нужно сделать, выбрав элемент «Список» в «просмотр содержимого в раскрывающемся меню.

Помните, что когда был создан новый класс, необходимо скомпилировать приложение для его отображения в диалоговом окне Добавить представление.

![Добавить представление](getting-started-with-mvc-part5/_static/image3.png)

Нажмите кнопку Добавить, и система автоматически создаст код для представления для нас, отображает наш список фильмов. Это подходящий момент, чтобы изменить &lt;h2&gt; заголовок примерно в «Мой список фильмов», как мы делали ранее с помощью представления Hello World.

[![Фильмы - Microsoft Visual Web Developer 2010 Express](getting-started-with-mvc-part5/_static/image5.png)](getting-started-with-mvc-part5/_static/image4.png)

Запустите приложение и посетите /Movies в адресной строке. Теперь мы загрузили данные из базы данных, с помощью простого запроса в контроллере и возвращаются данные для представления, который знает о фильмах. Это представление, то вращается через список фильмов и создает таблицу данных для нас.

[![Список фильмов - Windows Internet Explorer](getting-started-with-mvc-part5/_static/image7.png)](getting-started-with-mvc-part5/_static/image6.png)

Мы не будет реализован функциональные возможности редактирования, подробные сведения и удалить с помощью этого приложения -, нам не нужен ссылки по умолчанию, шаблон формирования создан для нас. Откройте файл /Movies/Index.aspx и удалите их.

Ниже приведен исходный код для наших обновленный шаблон представления должны выглядеть после внесении этих изменений:

[!code-aspx[Main](getting-started-with-mvc-part5/samples/sample2.aspx)]

Он является создание ссылок, которые нам не требуется, поэтому мы удалим их в этом примере. Мы сохранит нашей Создание новой связи, как это Далее! Вот как выглядит нашего приложения с выбранным столбцом удалены.

[![Список фильмов - Windows Internet Explorer](getting-started-with-mvc-part5/_static/image9.png)](getting-started-with-mvc-part5/_static/image8.png)

Теперь у нас есть простого списка наши данные фильма. Тем не менее если щелкнуть ссылку «Создать», мы получите ошибку, не подключен! Давайте реализовать метод действия Create и дать пользователю возможность ввести новые фильмы в базе данных.

> [!div class="step-by-step"]
> [Назад](getting-started-with-mvc-part4.md)
> [Вперед](getting-started-with-mvc-part6.md)
