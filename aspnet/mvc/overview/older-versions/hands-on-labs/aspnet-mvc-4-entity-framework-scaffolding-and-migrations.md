---
uid: mvc/overview/older-versions/hands-on-labs/aspnet-mvc-4-entity-framework-scaffolding-and-migrations
title: "Перенос и формирование шаблонов ASP.NET MVC 4 Entity Framework | Документы Microsoft"
author: rick-anderson
description: "Знакомы с методами контроллера ASP.NET MVC 4, и завершена &quot;вспомогательные методы, форм и проверки&quot; практическая работа следует иметь в виду..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/18/2013
ms.topic: article
ms.assetid: 093c1362-f10b-407c-a708-be370f4b62b0
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions/hands-on-labs/aspnet-mvc-4-entity-framework-scaffolding-and-migrations
msc.type: authoredcontent
ms.openlocfilehash: 15db1589eb90739458b430c35cea38e93e3dec5a
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 11/10/2017
---
<a name="aspnet-mvc-4-entity-framework-scaffolding-and-migrations"></a>Перенос и формирование шаблонов ASP.NET MVC 4 Entity Framework
====================
по [Web лагеря команды](https://twitter.com/webcamps)

> Знакомы с методами контроллера ASP.NET MVC 4, и завершена &quot;вспомогательные методы, форм и проверки&quot; практическая работа следует помнить, что многие логику для создания, обновления, перечисления и удаления любой сущности данных, он будет повторяться среди приложение. Считается, что если модель содержит несколько классов для управления, можно скорее всего, тратят много времени, записи, POST и GET методы действий для каждой сущности операции, а также каждое из представлений.
> 
> В этой лаборатории вы узнаете, как использовать формирование шаблонов ASP.NET MVC 4 для автоматического создания базовых показателей приложения CRUD (Создание, чтение, обновление и удаление). Начиная с простой класс модели и без единой строки кода, вы создадите контроллера, который будет содержать все операции CRUD, а также все необходимые представления. После построения и запуска простого решения, необходимо будет создан вместе с логику MVC и представления для работы с данными базы данных приложения.
> 
> Кроме того вы узнаете, как просто можно использовать Entity Framework миграции для обновления модели на протяжении всего приложения. Entity Framework миграции позволяют изменить базу данных, после изменения модели с простых шагов. С помощью всех этих помните можно для создания и обслуживания веб-приложений более эффективно преимуществами с новейшими функциями ASP.NET MVC 4.


<a id="Objectives"></a>

<a id="Objectives"></a>
### <a name="objectives"></a>Цели

В этой лаборатории практических вы узнаете, как:

- Используйте формирование шаблонов ASP.NET для операций CRUD в контроллерах.
- Измените модель базы данных, с помощью миграций Entity Framework.

<a id="Prerequisites"></a>

<a id="Prerequisites"></a>
### <a name="prerequisites"></a>Предварительные требования

Необходимо иметь следующие элементы для этой лаборатории.

- [Microsoft Visual Studio Express 2012 для Web](https://www.microsoft.com/visualstudio/eng/products/visual-studio-express-for-web) или выше (чтение [приложение A](#AppendixA) инструкции по его установке).

<a id="Setup"></a>

<a id="Setup"></a>
### <a name="setup"></a>Установка

**Установка фрагменты кода**

Для удобства большая часть кода, который вы планируете управлять вдоль этой лаборатории доступна как фрагменты кода Visual Studio. Чтобы установить фрагменты кода запуска **.\Source\Setup\CodeSnippets.vsi** файла.

Если вы не знакомы с фрагментов кода Visual Studio и хотите узнать о способах их использования, можно ссылаться в приложение из этого документа &quot; [с помощью фрагментов кода в приложении б.](#AppendixB)&quot;.

* * *

<a id="Exercises"></a>

<a id="Exercises"></a>
## <a name="exercises"></a>Упражнения

Следующее упражнение составляют это Практическое лабораторное занятие:

1. [С помощью формирования шаблонов ASP.NET MVC 4 с Entity Framework миграции](#Exercise1)

> [!NOTE]
> В этом упражнении сопровождается **окончания** папку, содержащую полученное в результате решение, должен быть получен после завершения выполнения упражнения. Это решение можно использовать как руководство, если вам нужна дополнительная помощь, работа в упражнении.


Предполагаемое время для выполнения этого занятия: **30 минут**

<a id="Exercise1"></a>

<a id="Exercise_1_Using_ASPNET_MVC_4_Scaffolding_with_Entity_Framework_Migrations"></a>
### <a name="exercise-1-using-aspnet-mvc-4-scaffolding-with-entity-framework-migrations"></a>Упражнение 1: Использование формирование шаблонов ASP.NET MVC 4 с Entity Framework миграции

Формирование шаблонов ASP.NET MVC предоставляет быстрый способ создания операций CRUD стандартизованного, создание необходимую логику, которая позволяет приложению взаимодействовать с уровня базы данных.

В этом упражнении вы узнаете, как использовать формирование шаблонов ASP.NET MVC 4 с кодом, сначала создайте методы CRUD. Затем вы узнаете, как обновлять модель внесения изменений в базе данных с помощью миграций Entity Framework.

<a id="Ex1Task1"></a>

<a id="Task_1-_Creating_a_new_ASPNET_MVC_4_project_using_Scaffolding"></a>
#### <a name="task-1--creating-a-new-aspnet-mvc-4-project-using-scaffolding"></a>Проект 1 — Создание нового ASP.NET MVC 4 задачи с помощью формирования шаблонов

1. Если это еще не открыто, запустите **Visual Studio 2012**.
2. Выберите **файл | Новый проект**. В командлет New Project диалоговое окно, в разделе **Visual C# | Web** выберите **веб-приложение ASP.NET MVC 4**. Имя проекта для **MVC4andEFMigrations** и укажите расположение **Source\Ex1 UsingMVC4ScaffoldingEFMigrations** папку части этой лаборатории. Задать **имя решения** для **начать** и обеспечить **создать каталог для решения** проверяется. Нажмите кнопку **ОК**.

    ![Диалоговое окно "новый проект ASP.NET MVC 4"](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image1.png "новое диалоговое окно проекта ASP.NET MVC 4")

    *Диалоговое окно "новый проект ASP.NET MVC 4"*
3. В **нового проекта ASP.NET MVC 4** диалогового окна выберите **веб-приложение** шаблона и убедитесь, что **Razor** нажата **обработчик представлений**. Нажмите кнопку **ОК** для создания проекта.

    ![Новый Интернет-приложения ASP.NET MVC 4](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image2.png "новых Интернет-приложения ASP.NET MVC 4")

    *Новый Интернет-приложения ASP.NET MVC 4*
4. В обозревателе решений щелкните правой кнопкой мыши **моделей** и выберите **добавить | Класс** создание простого класса человека (POCO). Назовите его **лицо** и нажмите кнопку **ОК**.
5. Откройте класс Person и вставьте следующие свойства.

    (Фрагмент - кода *ASP.NET MVC 4 и Entity Framework миграции - свойства пользователя сервера Ex1*)


    [!code-csharp[Main](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/samples/sample1.cs)]
6. Нажмите кнопку **сборки | Построение решения** для сохранения изменений и постройте проект.

    ![Сборка приложения](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image3.png "построение приложения")

    *Построение приложения*
7. В обозревателе решений щелкните правой кнопкой мыши папку controllers и выбрать **добавить | Контроллер**.
8. Назовите контроллер *PersonController* и завершить **параметры формирования шаблонов** со следующими значениями.

    1. В **шаблона** раскрывающемся списке выберите **контроллер MVC с действиями чтения и записи и представлениями, использующий Entity Framework** параметр.
    2. В **класс модели** раскрывающемся списке выберите **лицо** класса.
    3. В **класс контекста данных** выберите  **&lt;новый контекст данных... &gt;**. Выберите любое имя и нажмите кнопку **ОК**.
    4. В **представления** раскрывающемся списке, убедитесь, что **Razor** выбран.

    ![Добавление пользователя контроллера с помощью формирования шаблонов](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image4.png "Добавление пользователя контроллера с помощью формирования шаблонов")

    *Добавление пользователя контроллера с помощью формирования шаблонов*
9. Нажмите кнопку **добавить** Создание нового контроллера для пользователя с помощью формирования шаблонов. Вы создали действия контроллера, а также представления.

    ![После создания пользователя контроллера с помощью формирования шаблонов](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image5.png "после создания пользователя контроллера с помощью формирования шаблонов")

    *После создания пользователя контроллера с помощью формирования шаблонов*
10. Откройте **PersonController** класса. Обратите внимание, что полный CRUD методы действия были созданы автоматически.

    ![Внутри контроллера лица](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image6.png "внутри пользователя контроллера")

    *Внутри контроллера Person*

<a id="Ex1Task2"></a>

<a id="Task_2-_Running_the_application"></a>
#### <a name="task-2--running-the-application"></a>Задача 2 выполняемого приложения

На этом этапе базе данных еще не создан. В этой задаче будет запустите приложение в первый раз и протестировать операции CRUD. Базы данных создается с помощью Code First.

1. Нажмите клавишу **F5** для запуска приложения.
2. В браузере, добавьте **/Person** URL-адрес, чтобы открыть страницу пользователя.

    ![Первый запуск приложения](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image7.png "первый запуск приложения")

    *Приложение: сначала запустите*
3. Вы лица страниц и протестировать операции CRUD.

    1. Нажмите кнопку **создать новый** для добавления нового пользователя. Введите имя и фамилию и нажмите кнопку **создать**.

        ![Добавление нового лица](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image8.png "Добавление нового пользователя")

        *Добавление нового пользователя*
    2. В списке его можно удалить, изменить или добавить элементы.

        ![список людей](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image9.png "список людей")

        *Список людей*
    3. Нажмите кнопку **сведения** открыть сведения пользователя.

        ![Сведения о его](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image10.png "сведений пользователя")

        *Подробные сведения для пользователя*
4. Закройте окно браузера и вернитесь в Visual Studio. Обратите внимание, что создан всей CRUD для сущности person во всем приложении — из модели для представления - без необходимости написания единой строки кода!

<a id="Ex1Task3"></a>

<a id="Task_3-_Updating_the_database_using_Entity_Framework_Migrations"></a>
#### <a name="task-3--updating-the-database-using-entity-framework-migrations"></a>Задача 3-обновления базы данных, с помощью Entity Framework миграции

В этой задаче будет обновлять базу данных, с помощью миграций Entity Framework. Вы поймете, как просто можно изменить модель и отражение изменений в базах данных, используя функцию миграции Entity Framework.

1. Откройте консоль диспетчера пакетов. Выберите **инструменты | Диспетчер пакетов библиотеки | Консоль диспетчера пакетов**.
2. В консоли диспетчера пакетов введите следующую команду:

    PMC

    [!code-powershell[Main](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/samples/sample2.ps1)]

    ![Включение миграции](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image11.png "включение миграции")

    *Включение миграции*

    Включить миграцию команда создает **миграций** папке, которая содержит скрипт для инициализации базы данных.

    ![Миграция папку](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image12.png "папку миграции")

    *Миграция папки*
3. Откройте **Configuration.cs** файл в папке миграции. Найдите конструктор класса и измените **AutomaticMigrationsEnabled** значение *true*.


    [!code-csharp[Main](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/samples/sample3.cs)]
4. Откройте класс Person и добавьте атрибут для отчества данного лица. Модель изменяется с помощью этого нового атрибута.


    [!code-csharp[Main](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/samples/sample4.cs)]
5. Выберите **сборки | Построение решения** меню для построения приложения.

    ![Сборка приложения](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image13.png "построение приложения")

    *Сборка приложения*
6. В консоли диспетчера пакетов введите следующую команду:

    PMC

    [!code-powershell[Main](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/samples/sample5.ps1)]

    Эта команда будет выглядеть изменения в объекты данных, и затем добавьте необходимые команды соответствующим образом изменить базу данных.

    ![Добавление отчество](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image14.png "Добавление отчество")

    *Добавление отчество*
7. (Необязательно) Можно выполнить следующую команду для создания скрипта SQL разностное обновление. Это позволит обновить базу данных вручную (в этом случае необязательно), или применить изменения в других базах данных:

    PMC

    [!code-powershell[Main](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/samples/sample6.ps1)]

    ![Формирование скрипта SQL](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image15.png "формирование скрипта SQL")

    *Формирование скрипта SQL*

    ![Скрипт SQL update](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image16.png "обновление скрипта SQL")

    *Скрипт SQL update*
8. В консоли диспетчера пакетов введите следующую команду, чтобы обновить базу данных:

    PMC

    [!code-powershell[Main](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/samples/sample7.ps1)]

    ![Обновление базы данных](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image17.png "обновление базы данных")

    *Обновление базы данных*

    При этом будет добавлено **MiddleName** столбца в **людей** таблицы для сопоставления используется текущее определение **лицо** класса.
9. После обновления базы данных, щелкните правой кнопкой мыши папку контроллера и выберите **добавить | Контроллер** Добавление пользователя контроллера снова (полная с теми же значениями). Это обновляет существующие методы и представления, Добавление нового атрибута.

    ![Добавление, обновление контроллера](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image18.png "Добавление обновление контроллера")

    *Обновление контроллера*
10. Нажмите кнопку **Добавить**. Выберите значения **перезаписать PersonController.cs** и **перезаписать связанные представления** и нажмите кнопку **ОК**.

    ![Добавление перезаписи контроллера](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image19.png)

    *Обновление контроллера*

<a id="Ex1Task4"></a>

<a id="Task4-_Running_the_application"></a>
#### <a name="task4--running-the-application"></a>Task4 - запуск приложения

1. Нажмите клавишу **F5** для запуска приложения.
2. Откройте **/Person**. Обратите внимание, что данные была сохранена во время отчество столбец был добавлен.

    ![Отчество добавлены](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image20.png "отчество добавлен")

    *Отчество добавлен*
3. Если щелкнуть **изменить**, можно будет добавить второе имя текущего пользователя.

    ![Отчество edition](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image21.png "отчество выпуска")

* * *

<a id="Summary"></a>

<a id="Summary"></a>
## <a name="summary"></a>Сводка

В этой практической было изучено простых шагов для создания операций CRUD с формирование шаблонов ASP.NET MVC 4 с помощью любой класс модели. Затем вы узнали, как для выполнения обновлений сквозного в приложении - из базы данных для просмотра - с помощью миграций Entity Framework.

<a id="AppendixA"></a>

<a id="Appendix_A_Installing_Visual_Studio_Express_2012_for_Web"></a>
## <a name="appendix-a-installing-visual-studio-express-2012-for-web"></a>Приложение а. Установка Visual Studio Express 2012 для Web

Можно установить **Microsoft Visual Studio Express 2012 для Web** или другой &quot;Express&quot; версию, используя  **[Microsoft Web Platform Installer](https://www.microsoft.com/web/downloads/platform.aspx)** . Следующие инструкции описывают действия, необходимые для установки *Visual studio Express 2012 для Web* с помощью *Microsoft Web Platform Installer*.

1. Последовательно выберите пункты [ [https://go.microsoft.com/? linkid = 9810169](https://go.microsoft.com/?linkid=9810169)](https://go.microsoft.com/?linkid=9810169). Кроме того, если вы уже установили установщика веб-платформы, можно открыть его и выполните поиск продукта &quot; *Visual Studio Express 2012 для Web с пакетом Windows Azure SDK*&quot;.
2. Щелкните **установить сейчас**. Если у вас **Web Platform Installer** вы будете перенаправлены, чтобы загрузить и установить ее сначала.
3. Один раз **Web Platform Installer** открыто, нажмите кнопку **установить** для запуска программы установки.

    ![Установка Visual Studio Express](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image22.png "установка Visual Studio Express")

    *Установка Visual Studio Express*
4. Чтение всех продуктов лицензий и условия и нажмите кнопку **принимаю** для продолжения.

    ![Принятие условий лицензионного соглашения](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image23.png)

    *Принятие условий лицензионного соглашения*
5. Дождитесь завершения процесса загрузки и установки.

    ![Ход установки](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image24.png)

    *Ход выполнения установки*
6. По завершении установки нажмите кнопку **Готово**.

    ![Установка завершена](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image25.png)

    *Установка завершена*
7. Нажмите кнопку **выхода** закрыть установщик веб-платформы.
8. Чтобы открыть Visual Studio Express для Web, перейдите к **запустить** экрана и начинается запись &quot; **VS Express**&quot;, выберите команду **VS Express для Web** Плитка.

    ![VS Express для Web плитки](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image26.png)

    *VS Express для Web плитки*

<a id="AppendixB"></a>

<a id="Appendix_B_Using_Code_Snippets"></a>
## <a name="appendix-b-using-code-snippets"></a>Приложение б. фрагменты кода

С помощью фрагментов кода у вас есть весь код, который требуется под рукой. Документ лаборатории сообщает только при их использовании, как показано на следующем рисунке.

![Фрагменты кода Visual Studio для вставки кода в проекте](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image27.png "фрагменты кода с помощью Visual Studio вставьте код в проект")

*Фрагменты кода Visual Studio для вставки кода в проекте*

***Добавление фрагмента кода, с помощью клавиатуры (только C#)***

1. Поместите курсор в место вставки кода.
2. Начните вводить имя фрагмента (без пробелов и дефисы).
3. Смотрите, как IntelliSense отображает, совпадающие с именами фрагменты.
4. Выберите правильный фрагмент (или продолжите ввод, пока не будет выделен весь фрагмент имени).
5. Нажмите клавишу Tab дважды, чтобы вставить фрагмент в положении курсора.

![Начните вводить имя фрагмента](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image28.png "начните вводить имя фрагмента")

*Начните вводить имя фрагмента*

![Нажмите клавишу Tab, чтобы выделить выделенный фрагмент](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image29.png "нажмите клавишу Tab, чтобы выделить выделенный фрагмент")

*Нажмите клавишу Tab, чтобы выделить выделенный фрагмент*

![Снова нажмите клавишу Tab и фрагмент будет расширен](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image30.png "будет расширен, снова нажмите клавишу Tab и фрагмента кода")

*Снова нажмите клавишу Tab и фрагмент будет расширен*

***Добавление фрагмента кода, с помощью мыши (C#, Visual Basic и XML)*** 1. Щелкните правой кнопкой мыши место вставки фрагмента кода.

1. Выберите **вставить фрагмент** следуют **Мои фрагменты кода**.
2. Выберите соответствующий фрагмент из списка, щелкнув по ней.

![Щелкните правой кнопкой мыши место для вставки фрагмента кода и выбора вставить фрагмент](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image31.png "место для вставки фрагмента кода и выбора вставить фрагмент кода щелкните правой кнопкой мыши")

*Щелкните правой кнопкой мыши в место вставки фрагмента кода и выберите Вставить фрагмент*

![Выберите из списка, соответствующего фрагмента, щелкнув его](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image32.png "выбрать соответствующий фрагмент из списка, щелкнув по ней")

*Выберите соответствующий фрагмент из списка, щелкнув по ней*
