---
uid: mvc/overview/older-versions/hands-on-labs/aspnet-mvc-4-entity-framework-scaffolding-and-migrations
title: Перенос и ASP.NET MVC 4 Entity Framework, формирование шаблонов | Документация Майкрософт
author: rick-anderson
description: Если вы знакомы с методов контроллера ASP.NET MVC 4 или завершили &quot;вспомогательные методы, формы и проверка&quot; Практическое лабораторное занятие, следует иметь в виду...
ms.author: aspnetcontent
ms.date: 02/18/2013
ms.assetid: 093c1362-f10b-407c-a708-be370f4b62b0
msc.legacyurl: /mvc/overview/older-versions/hands-on-labs/aspnet-mvc-4-entity-framework-scaffolding-and-migrations
msc.type: authoredcontent
ms.openlocfilehash: 31f593f294c4865f621a8556cb43d0d9c42f2660
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 07/05/2018
ms.locfileid: "37814126"
---
# <a name="aspnet-mvc-4-entity-framework-scaffolding-and-migrations"></a>Перенос и ASP.NET MVC 4 Entity Framework, формирование шаблонов

по [Web Слышатся Team](https://twitter.com/webcamps)

[Скачайте комплект учебных материалов по лагеря Web](https://aka.ms/webcamps-training-kit)

Если вы знакомы с методов контроллера ASP.NET MVC 4 или завершили &quot;вспомогательные методы, формы и проверка&quot; Практическое лабораторное занятие, следует иметь в виду, многие из логики, чтобы создать, обновить, отобразить и удалить любой сущности данных, он повторяется Среди приложений. Помимо того, что, если модель содержит несколько классов для управления, можно скорее всего, тратят значительное время написания методы действий POST и GET для каждой сущности операции, а также каждое из этих представлений.

В этом лабораторном занятии вы узнаете, как использовать формирование шаблонов ASP.NET MVC 4 для автоматического создания базового плана приложения CRUD (Create, Read, Update и Delete). Начиная от простой класс модели и, не написав ни строчки кода, вы создадите контроллер, который будет содержать все операции CRUD, а также все необходимые представления. После построения и запуска простого решения, вы получите базу данных приложения, созданный вместе с MVC логику и представления для манипуляций с данными.

Кроме того вы узнаете, насколько это просто использовать миграции Entity Framework для выполнения обновлений модели на протяжении всего приложения. Миграции Entity Framework позволяют изменить базу данных, после изменения модели с помощью простых шагов. С все это в виду можно для создания и поддержки веб-приложения более эффективно, используя преимущества новые возможности ASP.NET MVC 4.

> [!NOTE]
> Все примеры кода и фрагменты кода включены в Web лагеря комплект обучающих материалов, доступных из в [выпуски Microsoft Web/WebCampTrainingKit](https://aka.ms/webcamps-training-kit). Проект, относящиеся к этой лаборатории доступен в [механизма формирования шаблонов ASP.NET MVC 4 Entity Framework и миграции](https://github.com/Microsoft-Web/HOL-EntityFrameworkScaffoldingAndMigrations).

<a id="Objectives"></a>
### <a name="objectives"></a>Цели

В этом Практическое лабораторное занятие, вы узнаете, как:

- Используйте формирование шаблонов ASP.NET для операций CRUD в контроллерах.
- Измените модель базы данных, с помощью миграций Entity Framework.

<a id="Prerequisites"></a>

<a id="Prerequisites"></a>
### <a name="prerequisites"></a>Предварительные требования

Необходимо иметь следующие элементы во укомплектовать данную лабораторию:

- [Microsoft Visual Studio Express 2012 для Web](https://www.microsoft.com/visualstudio/eng/products/visual-studio-express-for-web) или руководителя (чтение [приложение А](#AppendixA) инструкции по его установке).

<a id="Setup"></a>

<a id="Setup"></a>
### <a name="setup"></a>Установка

**Установка фрагменты кода**

Для удобства большая часть кода, который вы планируете управлять вдоль этой лаборатории доступен как фрагменты кода Visual Studio. Чтобы установить фрагменты кода запуска **.\Source\Setup\CodeSnippets.vsi** файла.

Если вы не знакомы с фрагментов кода Visual Studio и хотите узнать о способах их использования, можно ссылаться в приложение из этого документа &quot; [с помощью фрагментов кода в приложении б](#AppendixB)&quot;.

* * *

<a id="Exercises"></a>

<a id="Exercises"></a>
## <a name="exercises"></a>Упражнения

Следующее упражнение составляющие этот практический семинар:

1. [С помощью формирования шаблонов ASP.NET MVC 4 с помощью миграций Entity Framework](#Exercise1)

> [!NOTE]
> В этом упражнении сопровождается **окончания** папку, содержащую полученное решение, должен быть получен после завершения упражнения. Если вам нужна дополнительная помощь, работа это упражнение, можно использовать это решение по.


Предполагаемое время для выполнения этого лабораторного: **30 минут**

<a id="Exercise1"></a>

<a id="Exercise_1_Using_ASPNET_MVC_4_Scaffolding_with_Entity_Framework_Migrations"></a>
### <a name="exercise-1-using-aspnet-mvc-4-scaffolding-with-entity-framework-migrations"></a>Упражнение 1: С помощью формирования шаблонов ASP.NET MVC 4 с помощью миграций Entity Framework

Формирование шаблонов ASP.NET MVC предоставляет быстрый способ для создания операции CRUD в стандартную процедуру, создание необходимую логику, которая позволяет приложению взаимодействовать с уровня базы данных.

В этом упражнении вы узнаете, как использовать формирование шаблонов ASP.NET MVC 4 с кодом, сначала создайте CRUD-методы. Затем вы узнаете, как обновить модель применение изменений в базе данных с помощью миграций Entity Framework.

<a id="Ex1Task1"></a>

<a id="Task_1-_Creating_a_new_ASPNET_MVC_4_project_using_Scaffolding"></a>
#### <a name="task-1--creating-a-new-aspnet-mvc-4-project-using-scaffolding"></a>Проект 1 — Создание нового ASP.NET MVC 4 задачи с помощью формирования шаблонов

1. Если это еще не открыто, запустите **Visual Studio 2012**.
2. Выберите **файл | Новый проект**. В New Project диалоговое окно, в разделе **Visual C# | Web** выберите **веб-приложение ASP.NET MVC 4**. Назовите проект для **MVC4andEFMigrations** и задайте расположение **Source\Ex1 UsingMVC4ScaffoldingEFMigrations** папку данную лабораторию. Задайте **имя решения** для **начать** , причем **создать каталог для решения** проверяется. Нажмите кнопку **ОК**.

    ![Диалоговое окно "новый проект ASP.NET MVC 4"](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image1.png "диалоговое окно \"новый проект ASP.NET MVC 4\"")

    *Диалоговое окно "новый проект ASP.NET MVC 4"*
3. В **создания проекта ASP.NET MVC 4** диалогового окна выберите **веб-приложение** шаблона и убедитесь, что **Razor** является выбранным **обработчик представлений**. Нажмите кнопку **ОК**, чтобы создать проект.

    ![Новый Интернет-приложения ASP.NET MVC 4](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image2.png "новый Интернет-приложения ASP.NET MVC 4")

    *Новый Интернет-приложения ASP.NET MVC 4*
4. В обозревателе решений щелкните правой кнопкой мыши **моделей** и выберите **Add | Класс** для создания простой класс человека (POCO). Назовите его **Person** и нажмите кнопку **ОК**.
5. Откройте класс Person и вставьте следующие свойства.

    (Code Snippet - *ASP.NET MVC 4 и миграции Entity Framework — свойства сервера Ex1 Person*)

    [!code-csharp[Main](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/samples/sample1.cs)]
6. Нажмите кнопку **сборки | Создание решения** для сохранения изменений и сборки проекта.

    ![Сборка приложения](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image3.png "сборка приложения")

    *Сборка приложения*
7. В обозревателе решений щелкните правой кнопкой мыши папку controllers и выберите **Add | Контроллер**.
8. Назовите контроллер *PersonController* и завершите **параметры формирования шаблонов** со следующими значениями.

   1. В **шаблона** стрелку раскрывающегося списка выберите **контроллер MVC с действиями чтения и записи и представлениями, использующий Entity Framework** параметр.
   2. В **класс модели** стрелку раскрывающегося списка выберите **Person** класса.
   3. В **класс контекста данных** выберите  **&lt;новый контекст данных... &gt;**. Выберите любое имя и нажмите кнопку **ОК**.
   4. В **представления** выберите в раскрывающемся списке, убедитесь, что **Razor** выбран.

      ![Добавление контроллера Person с помощью формирования шаблонов](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image4.png "Добавление контроллера Person с помощью формирования шаблонов")

      *Добавление контроллера Person с помощью формирования шаблонов*
9. Нажмите кнопку **добавить** для создания нового контроллера для пользователя с помощью формирования шаблонов. Теперь вы создали действий контроллера, а также представления.

    ![После создания с помощью формирования шаблонов контроллера Person](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image5.png "после создания с помощью формирования шаблонов контроллера Person")

    *После создания с помощью формирования шаблонов контроллера Person*
10. Откройте **PersonController** класса. Обратите внимание на то, что все CRUD-методы действия были созданы автоматически.

   ![Внутри контроллера Person](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image6.png "контроллера внутри Person")

   *Внутри контроллера Person*

<a id="Ex1Task2"></a>

<a id="Task_2-_Running_the_application"></a>
#### <a name="task-2--running-the-application"></a>Задача 2 выполняющиеся приложения

На этом этапе база данных не еще создается. В этой задаче будет запустить приложение в первый раз и проверки операций CRUD. Базы данных будет создаваться в режиме реального времени с помощью Code First.

1. Нажмите клавишу **F5** для запуска приложения.
2. В браузере, добавьте **/Person** на URL-адрес, чтобы открыть страницу «Person».

    ![Первый запуск приложения](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image7.png "первый запуск приложения")

    *Приложение: сначала запустите*
3. Вы человек страниц и проверки операций CRUD.

    1. Нажмите кнопку **Create New** для добавления нового пользователя. Введите имя и фамилию и нажмите кнопку **создать**.

        ![Добавление нового лица](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image8.png "Добавление нового пользователя")

        *Добавление нового пользователя*
    2. В списке человека можно удалить, изменить или добавить элементы.

        ![список людей](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image9.png "список людей")

        *Список людей*
    3. Нажмите кнопку **сведения** чтобы открыть сведения человека.

        ![Сведения о его](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image10.png "сведения человека")

        *Сведения о человека*
4. Закройте браузер и вернуться в Visual Studio. Обратите внимание на то, что вы создали всю CRUD для сущности person в приложении - из модели к представлениям - без необходимости писать ни одной строки кода!

<a id="Ex1Task3"></a>

<a id="Task_3-_Updating_the_database_using_Entity_Framework_Migrations"></a>
#### <a name="task-3--updating-the-database-using-entity-framework-migrations"></a>Задача 3-обновление базы данных, с помощью миграций Entity Framework

В этой задаче будет обновить базу данных с помощью миграций Entity Framework. Вы поймете, насколько это просто для изменения модели и отражение изменений в базах данных с помощью функции миграции Entity Framework.

1. Откройте консоль диспетчера пакетов. Выберите **инструменты | Диспетчер пакетов библиотеки | Консоль диспетчера пакетов**.
2. В консоли диспетчера пакетов введите следующую команду:

    PMC

    [!code-powershell[Main](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/samples/sample2.ps1)]

    ![Включение Migrations](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image11.png "включение миграции")

    *Включение миграции*

    Команда включения миграции создает **миграций** папку, которая содержит скрипт для инициализации базы данных.

    ![Папку migrations](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image12.png "папку Migrations")

    *Папку migrations*
3. Откройте **Configuration.cs** файл в папку Migrations. Найдите конструктор класса и измените **AutomaticMigrationsEnabled** значение *true*.

    [!code-csharp[Main](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/samples/sample3.cs)]
4. Откройте класс Person и добавьте атрибут для отчества человека. С помощью этого нового атрибута вы изменяете модель.

    [!code-csharp[Main](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/samples/sample4.cs)]
5. Выберите **сборки | Создание решения** меню для построения приложения.

    ![Сборка приложения](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image13.png "сборка приложения")

    *Сборка приложения*
6. В консоли диспетчера пакетов введите следующую команду:

    PMC

    [!code-powershell[Main](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/samples/sample5.ps1)]

    Эта команда выполнит поиск изменений объектов данных, и затем добавьте необходимые команды соответствующим образом изменить базу данных.

    ![Добавление отчество](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image14.png "Добавление отчество")

    *Добавление отчество*
7. (Необязательно) Можно выполнить следующую команду, чтобы создать сценарий SQL разностное обновление. Это позволит вам обновить базу данных вручную (в этом случае он не требуется), или применить изменения в других базах данных:

    PMC

    [!code-powershell[Main](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/samples/sample6.ps1)]

    ![Формирование скрипта SQL](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image15.png "формирование скрипта SQL")

    *Формирование скрипта SQL*

    ![Обновление скрипта SQL](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image16.png "обновления скрипта SQL")

    *Обновление скрипта SQL*
8. В консоли диспетчера пакетов введите следующую команду, чтобы обновить базу данных:

    PMC

    [!code-powershell[Main](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/samples/sample7.ps1)]

    ![Обновление базы данных](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image17.png "обновление базы данных")

    *Обновление базы данных*

    При этом будет добавлено **MiddleName** столбца в **людей** сопоставляемой используется текущее определение таблицы **Person** класса.
9. После обновления базы данных, щелкните правой кнопкой мыши папку контроллера и выберите **Add | Контроллер** Добавление пользователя контроллера снова (полная с теми же значениями). Будут обновлены существующие методы и представления, добавления нового атрибута.

    ![Добавление, обновление контроллера](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image18.png "Добавление обновление контроллера")

    *Обновление контроллера*
10. Нажмите кнопку **Добавить**. Выберите значения **перезаписать PersonController.cs** и **перезаписи связанного представления** и нажмите кнопку **ОК**.

   ![Добавление перезаписать контроллера](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image19.png)

   *Обновление контроллера*

<a id="Ex1Task4"></a>

<a id="Task4-_Running_the_application"></a>
#### <a name="task4--running-the-application"></a>Task4 — выполнение приложения

1. Нажмите клавишу **F5** для запуска приложения.
2. Откройте **/Person**. Обратите внимание на то, что данные была сохранена хотя отчество столбец был добавлен.

    ![Отчество добавлены](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image20.png "отчество добавлены")

    *Отчество добавлены*
3. Если щелкнуть **изменить**, можно будет добавить второе имя текущего пользователя.

    ![Отчество edition](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image21.png "edition отчество")

* * *

<a id="Summary"></a>

<a id="Summary"></a>
## <a name="summary"></a>Сводка

В этой практической работу вы узнали, как простые шаги для создания операции CRUD с формирование шаблонов ASP.NET MVC 4 с помощью любой класс модели. Затем вы узнали, как для выполнения сквозного обновления в приложении - из базы данных к представлениям - с помощью миграций Entity Framework.

<a id="AppendixA"></a>

<a id="Appendix_A_Installing_Visual_Studio_Express_2012_for_Web"></a>
## <a name="appendix-a-installing-visual-studio-express-2012-for-web"></a>Приложение а. Установка Visual Studio Express 2012 для Web

Вы можете установить **Microsoft Visual Studio Express 2012 для Web** или другой &quot;Express&quot; версию с помощью **[Microsoft Web Platform Installer](https://www.microsoft.com/web/downloads/platform.aspx)**. Приведенные ниже инструкции описывают действия, необходимые для установки *Visual studio Express 2012 для Web* с помощью *Microsoft Web Platform Installer*.

1. Перейдите к [ [ https://go.microsoft.com/? linkid = 9810169](https://go.microsoft.com/?linkid=9810169)](https://go.microsoft.com/?linkid=9810169). Кроме того, если вы уже установили установщика веб-платформы, можно открыть его и выполните поиск продукта &quot; <em>Visual Studio Express 2012 для Web с пакетом Windows Azure SDK</em>&quot;.
2. Щелкните **установить сейчас**. Если у вас нет **установщика веб-платформы** вы будете перенаправлены к сначала загрузить и установить его.
3. Один раз **установщика веб-платформы** открыт, нажмите кнопку **установить** для запуска программы установки.

    ![Установка Visual Studio Express](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image22.png "установка Visual Studio Express")

    *Установка Visual Studio Express*
4. Прочтите лицензии и условия все продукты и нажмите кнопку **я принимаю** для продолжения.

    ![Принятие условий лицензии](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image23.png)

    *Принятие условий лицензии*
5. Подождите, пока не завершится процесс загрузки и установки.

    ![Ход установки](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image24.png)

    *Ход выполнения установки*
6. После завершения установки нажмите кнопку **Готово**.

    ![Установка завершена](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image25.png)

    *Установка завершена*
7. Нажмите кнопку **выхода** закрыть установщик веб-платформы.
8. Чтобы открыть Visual Studio Express для Web, перейдите к **запустить** экрана и Займитесь написанием &quot; **VS Express**&quot;, нажмите кнопку на **VS Express для Web** Плитка.

    ![VS Express для Web плитки](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image26.png)

    *VS Express для Web плитки*

<a id="AppendixB"></a>

<a id="Appendix_B_Using_Code_Snippets"></a>
## <a name="appendix-b-using-code-snippets"></a>Приложение б. Использование фрагментов кода

С помощью фрагментов кода у вас есть весь код, который требуется в вашем распоряжении. Лаборатории документ поможет определить точно при их использовании, как показано на рисунке ниже.

![Фрагменты кода Visual Studio, чтобы вставить код в проект](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image27.png "фрагменты кода с помощью Visual Studio, чтобы вставить код в проект")

*Фрагменты кода Visual Studio, чтобы вставить код в проект*

***Чтобы добавить фрагмент кода, с помощью клавиатуры (только C#)***

1. Поместите курсор в место вставки кода.
2. Начните вводить имя фрагмента (без пробелов и дефисов).
3. Посмотрите, как IntelliSense отображает, совпадающие с именами фрагменты.
4. Выберите правильный фрагмент (или продолжите ввод, пока не будет выделен весь фрагмент имени).
5. Нажмите клавишу Tab дважды, чтобы вставить фрагмент в положении курсора.

![Начните вводить имя фрагмента](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image28.png "начните вводить имя фрагмента")

*Начните вводить имя фрагмента*

![Нажмите клавишу Tab, чтобы выделить фрагмент в выделенный](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image29.png "нажмите клавишу Tab, чтобы выделить выделенный фрагмент")

*Нажмите клавишу Tab, чтобы выделить выделенный фрагмент*

![Снова нажмите клавишу Tab и фрагмент будет расширяться](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image30.png "еще раз нажмите клавишу Tab и фрагмент будет расширяться")

*Снова нажмите клавишу Tab и фрагмент будет расширяться*

***Чтобы добавить фрагмент кода, с помощью мыши (C#, Visual Basic и XML)*** 1. Щелкните правой кнопкой мыши место для вставки фрагмента кода.

1. Выберите **вставить фрагмент** следуют **Мои фрагменты кода**.
2. Выберите соответствующий фрагмент из списка, щелкнув его.

![Щелкните правой кнопкой мыши, где необходимо вставить фрагмент кода и выберите Вставить фрагмент](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image31.png "щелкните правой кнопкой мыши, где необходимо вставить фрагмент кода и выберите Вставить фрагмент")

*Щелкните правой кнопкой мыши место для вставки фрагмента кода и выберите Вставить фрагмент*

![Выберите соответствующий фрагмент из списка, щелкнув по ней](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image32.png "выбрать соответствующий фрагмент из списка, щелкнув по ней")

*Выберите соответствующий фрагмент из списка, щелкнув по ней*
