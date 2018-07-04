---
uid: mvc/overview/older-versions-1/getting-started-with-mvc/getting-started-with-mvc-part4
title: Создание базы данных | Документация Майкрософт
author: shanselman
description: Это руководство для начинающих, в котором представлены основные сведения по ASP.NET MVC. Создание простого веб-приложения, которое считывает и записывает в базу данных.
ms.author: aspnetcontent
manager: wpickett
ms.date: 08/14/2010
ms.topic: article
ms.assetid: 742df67f-484d-4ef3-af6b-8c791e556b43
ms.technology: dotnet-mvc
msc.legacyurl: /mvc/overview/older-versions-1/getting-started-with-mvc/getting-started-with-mvc-part4
msc.type: authoredcontent
ms.openlocfilehash: 0e72359095e4c40ef7e56f1290a45ded257143c9
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 07/03/2018
ms.locfileid: "37362360"
---
<a name="creating-a-database"></a>Создание базы данных
====================
по [(Scott hanselman)](https://github.com/shanselman)

> Это руководство для начинающих, в котором представлены основные сведения по ASP.NET MVC. Вы создадите простое веб-приложение, которое считывает и записывает в базу данных. Посетите [центр обучения ASP.NET MVC](../../../index.md) для поиска других ASP.NET MVC, учебники и примеры.


В этом разделе мы собираемся создать новую SQL Express, мы будем использовать для хранения и извлечения данных фильмов. Из интегрированной среды разработки Visual Web Developer, выберите представление | Обозреватель серверов. Щелкните правой кнопкой подключения к данным и нажмите кнопку Добавить соединение...

![AddConnection](getting-started-with-mvc-part4/_static/image1.png)

В диалоговом окне Выбор источника данных выберите Microsoft SQL Server и нажмите кнопку Продолжить.

![](getting-started-with-mvc-part4/_static/image2.png)

В диалоговом окне "Добавить подключение", введите «. \SQLEXPRESS» для имени сервера и введите «Movies» в качестве имени новой базы данных.

[![Добавить диалоговое окно подключения](getting-started-with-mvc-part4/_static/image4.png)](getting-started-with-mvc-part4/_static/image3.png)

Нажмите «ОК», и появляется если вы хотите создать эту базу данных. Выберите "Да".

[![Создание фильмов?](getting-started-with-mvc-part4/_static/image6.png)](getting-started-with-mvc-part4/_static/image5.png)

Теперь у вас есть пустой базы данных в обозревателе серверов.

![Добавление новой таблицы](getting-started-with-mvc-part4/_static/image7.png)

Щелкните правой кнопкой мыши в таблицах и нажмите кнопку Добавить таблицу. Будет отображаться в конструкторе таблиц. Добавьте столбцы для Id, Title, ReleaseDate, Жанр и цена. Щелкните правой кнопкой столбец идентификатора, и нажмите кнопку Задать первичный ключ. Вот какие моей области конструктора, как выглядит.

[![Редактор таблицы базы данных](getting-started-with-mvc-part4/_static/image9.png)](getting-started-with-mvc-part4/_static/image8.png)

Кроме того выберите столбец, идентификатор и в группе свойств столбца ниже изменить «Спецификация идентификатора» значение «Да».

[![IsIdentity - свойства столбца](getting-started-with-mvc-part4/_static/image11.png)](getting-started-with-mvc-part4/_static/image10.png)

Если у вас есть задачи, щелкните значок "Сохранить" на панели инструментов или выберите файл | Сохраните в меню и назовите таблицу "**фильма**" (в единственном числе). У нас есть базы данных и таблицу!

[![Выберите имя](getting-started-with-mvc-part4/_static/image13.png)](getting-started-with-mvc-part4/_static/image12.png)

Вернитесь в обозреватель сервера и щелкните правой кнопкой мыши в таблице Movie, а затем выберите «Показать таблицу данных». Введите несколько фильмов, поэтому нашей базе данных имеет некоторые данные.

[![Редактирование таблиц базы данных](getting-started-with-mvc-part4/_static/image15.png)](getting-started-with-mvc-part4/_static/image14.png)

## <a name="creating-a-model"></a>Создание модели

Теперь вернитесь в обозреватель решений, в правой части интегрированной среды разработки и щелкните папку Models правой кнопкой мыши и выберите Add | Новый элемент.

[![addnewmodelitem](getting-started-with-mvc-part4/_static/image17.png)](getting-started-with-mvc-part4/_static/image16.png)

Мы собираемся создать сущностную модель из нашей новой базы данных. Это добавит набора классов в наш проект, который упрощает процесс для нас для запроса и управления данными в базе данных. Выберите узел данных в левой части диалогового окна и затем выберите шаблон элемента модели EDM ADO.NET. Назовите файл Movies.edmx.

[![AddNewDataModel](getting-started-with-mvc-part4/_static/image19.png)](getting-started-with-mvc-part4/_static/image18.png)

Нажмите кнопку «Добавить». Затем откроется «мастер моделей EDM».

В новое диалоговое окно, которое появляется выберите пункт Создать из базы данных. Так как мы только что внесли базу данных, необходимо только сообщить о новой базе данных и соответствующей таблицей Entity Framework. Нажмите кнопку рядом с полем, сохранить наши подключения к базе данных в конфигурации нашего веб-приложения. Теперь проверьте таблицы и фильмов флажок и нажмите кнопку Готово.

[![Мастер моделей EDM](getting-started-with-mvc-part4/_static/image21.png)](getting-started-with-mvc-part4/_static/image20.png)

Теперь мы см. в разделе нашей новой таблицы Movie в конструкторе Entity Framework и доступ к нему из кода.

[![Фильмы - Microsoft Visual Web Developer 2010 Express](getting-started-with-mvc-part4/_static/image23.png)](getting-started-with-mvc-part4/_static/image22.png)

В области конструктора отобразится класс «Фильма». Этот класс сопоставляется с таблицей «Фильма» в нашей базе данных, а каждое свойство в ней сопоставляет со столбцом с таблицей. Каждый экземпляр класса «Фильма» будет соответствовать строки в таблице «Фильма».

Если вам не нравятся именования по умолчанию и сопоставления соглашения, используемые платформой Entity Framework, можно использовать конструктор Entity Framework, изменить или настроить их. Для этого приложения будет использовать значения по умолчанию и сохраните файл как-является.

Теперь поработаем со некоторые реальные данные!

> [!div class="step-by-step"]
> [Назад](getting-started-with-mvc-part3.md)
> [Вперед](getting-started-with-mvc-part5.md)
