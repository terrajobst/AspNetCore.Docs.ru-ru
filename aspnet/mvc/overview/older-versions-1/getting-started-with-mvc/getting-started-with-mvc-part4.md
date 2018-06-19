---
uid: mvc/overview/older-versions-1/getting-started-with-mvc/getting-started-with-mvc-part4
title: Создание базы данных | Документы Microsoft
author: shanselman
description: Это руководство для новичков, в котором представлены основные сведения по ASP.NET MVC. Создание простого веб-приложения, чтение и запись из базы данных.
ms.author: aspnetcontent
manager: wpickett
ms.date: 08/14/2010
ms.topic: article
ms.assetid: 742df67f-484d-4ef3-af6b-8c791e556b43
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions-1/getting-started-with-mvc/getting-started-with-mvc-part4
msc.type: authoredcontent
ms.openlocfilehash: ff2a41803cd31ce50bbf79e630d827b6de441ba3
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/06/2018
ms.locfileid: "30868078"
---
<a name="creating-a-database"></a>Создание базы данных
====================
по [Скотт Хансельман](https://github.com/shanselman)

> Это руководство для новичков, в котором представлены основные сведения по ASP.NET MVC. Вы создадите простое веб-приложение выполняет чтение и запись из базы данных. Посетите [центр обучения ASP.NET MVC](../../../index.md) для поиска других ASP.NET MVC, учебники и примеры.


В этом разделе мы собираемся создать новую БД SQL Express, мы будем использовать для хранения и извлечения данных фильма. В СРЕДЕ Visual Web Developer выберите представление | Обозреватель серверов. Щелкните правой кнопкой мыши подключения данных и выберите команду Добавить подключение...

![AddConnection](getting-started-with-mvc-part4/_static/image1.png)

В диалоговом окне Выбор источника данных выберите Microsoft SQL Server и нажмите кнопку Продолжить.

![](getting-started-with-mvc-part4/_static/image2.png)

В диалоговом окне Добавление соединения, введите «. \SQLEXPRESS» для имени сервера и введите «Фильмы» в качестве имени новой базы данных.

[![Добавить диалоговое окно соединения](getting-started-with-mvc-part4/_static/image4.png)](getting-started-with-mvc-part4/_static/image3.png)

Нажмите "ОК" и запрашивается Если вы хотите создать эту базу данных. Выберите "Да".

[![Создать фильмы?](getting-started-with-mvc-part4/_static/image6.png)](getting-started-with-mvc-part4/_static/image5.png)

Итак, пустую базу данных в обозревателе серверов.

![Добавить новую таблицу](getting-started-with-mvc-part4/_static/image7.png)

Щелкните правой кнопкой мыши на таблицы и нажмите кнопку Добавить таблицу. Появится конструктор таблиц. Добавьте столбцы для идентификатора, заголовок, ReleaseDate, Жанр и Price. Щелкните правой кнопкой мыши столбец ID и нажмите кнопку Задать первичный ключ. Вот какие my области конструктора, имеет следующий вид.

[![Редактор таблиц базы данных](getting-started-with-mvc-part4/_static/image9.png)](getting-started-with-mvc-part4/_static/image8.png)

Кроме того выберите столбец, идентификатор и в разделе ниже свойства столбца измените «Спецификация идентификации» на «Да».

[![IsIdentity - свойства столбца](getting-started-with-mvc-part4/_static/image11.png)](getting-started-with-mvc-part4/_static/image10.png)

Если у вас есть задачи, щелкните значок «сохранить» на панели инструментов или выберите файл | Сохранить в меню, а имя таблицы «**фильма**» (единственного числа). У нас есть базы данных и таблицы.

[![Выберите имя](getting-started-with-mvc-part4/_static/image13.png)](getting-started-with-mvc-part4/_static/image12.png)

Вернитесь в обозревателе серверов щелкните правой кнопкой мыши таблицу фильма, затем выберите «Показать таблицу данных». Введите несколько фильмы, базе данных имеет некоторые данные.

[![Редактирование таблиц базы данных](getting-started-with-mvc-part4/_static/image15.png)](getting-started-with-mvc-part4/_static/image14.png)

## <a name="creating-a-model"></a>Создание модели

Теперь вернитесь в обозревателе решений в правой части интегрированной среды разработки и правой кнопкой мыши папку модели и выберите пункт Добавить | Новый элемент.

[![addnewmodelitem](getting-started-with-mvc-part4/_static/image17.png)](getting-started-with-mvc-part4/_static/image16.png)

Мы собираемся создать модель EDM из нашей новой базы данных. Набор классов, добавляются в свой проект, который позволяет с легкостью нам для запросов и управления данными в базе данных. Выберите узел данных в левой части диалогового окна, а затем выберите шаблона элемента модели EDM ADO.NET. Присвойте ему имя Movies.edmx.

[![AddNewDataModel](getting-started-with-mvc-part4/_static/image19.png)](getting-started-with-mvc-part4/_static/image18.png)

Нажмите кнопку «Добавить». Затем откроется «мастер моделей EDM».

В появившемся окне Новый выберите Создать из базы данных. Поскольку мы только что внесли базы данных, мы только необходимо сообщить платформе Entity Framework о нашей новой базы данных и соответствующей таблицей. Нажмите рядом с полем, сохранить соединение базы данных в конфигурации наш веб-приложения. Теперь проверьте таблицы и фильма флажок и нажмите кнопку Готово.

[![Мастер моделей EDM](getting-started-with-mvc-part4/_static/image21.png)](getting-started-with-mvc-part4/_static/image20.png)

Теперь мы нашей новая таблица фильма в конструкторе Entity Framework и доступ к нему из кода.

[![Фильмы - Microsoft Visual Web Developer 2010 Express](getting-started-with-mvc-part4/_static/image23.png)](getting-started-with-mvc-part4/_static/image22.png)

В области конструктора появится класса «Фильма». Этот класс сопоставляется с таблицей «Фильма» в базе данных, а каждое свойство внутри он сопоставляется со столбцом в таблице. Каждый экземпляр класса «Фильма» будут соответствовать строки в таблице «Фильма».

Если вас не устраивает именования по умолчанию и сопоставления соглашения, используемые платформой Entity Framework, можно использовать конструктор Entity Framework, изменить или настроить их. Для этого приложения будет использовать значения по умолчанию и сохраните файл как-является.

Теперь давайте работать с некоторыми реальными данными.

> [!div class="step-by-step"]
> [Назад](getting-started-with-mvc-part3.md)
> [Вперед](getting-started-with-mvc-part5.md)
