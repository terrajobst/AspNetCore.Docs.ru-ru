---
uid: mvc/overview/older-versions/getting-started-with-aspnet-mvc4/adding-a-model
title: Добавление модели | Документы Microsoft
author: Rick-Anderson
description: 'Примечание: Обновленную версию этого учебника доступен здесь, использующий ASP.NET MVC 5 и Visual Studio 2013. Это более безопасный, гораздо проще выполните и демонстрационных...'
ms.author: aspnetcontent
manager: wpickett
ms.date: 08/28/2012
ms.topic: article
ms.assetid: 53db72da-e0b9-44d9-b60b-6e6988c00b28
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions/getting-started-with-aspnet-mvc4/adding-a-model
msc.type: authoredcontent
ms.openlocfilehash: 562a06e22aad62b6982aca3316a2dfe18a6eba2e
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/06/2018
---
<a name="adding-a-model"></a>Добавление модели
====================
по [Рик Андерсон](https://github.com/Rick-Anderson)

> > [!NOTE]
> > Доступна обновленная версия этого учебника [здесь](../../getting-started/introduction/getting-started.md) , с использованием ASP.NET MVC 5 и Visual Studio 2013. Он является более безопасны, выполните гораздо проще и показаны дополнительные возможности.


В этом разделе вы добавите некоторые классы для управления фильмов в базе данных. Эти классы будет &quot;модели&quot; частью приложения ASP.NET MVC.

Используемой технологии доступа к данным .NET Framework, известный как [Entity Framework](https://msdn.microsoft.com/library/bb399572(VS.110).aspx) для определения и работы с этими классами модели. Поддерживает платформы Entity Framework (часто обозначается как EF), который называется парадигмы разработки *Code First*. Код сначала позволяет создавать объекты модели путем написания простых классов. (Они также известны как классов POCO из &quot;объектов plain old CLR.&quot;) Затем можно установить базу данных создан автоматически из классов, который позволяет очень простой и быстрой разработки рабочего процесса.

## <a name="adding-model-classes"></a>Добавление классов модели

В **обозревателе решений**, щелкните правой кнопкой мыши *моделей* выберите **добавить**и выберите **класса**.

![](adding-a-model/_static/image1.png)

Введите *класса* имя &quot;фильма&quot;.

Добавьте следующие пять свойства `Movie` класса:

[!code-csharp[Main](adding-a-model/samples/sample1.cs)]

Мы будем использовать `Movie` класс для представления фильмов в базе данных. Каждый экземпляр `Movie` объекта будет соответствовать строки в таблицу базы данных и каждое свойство `Movie` класса сопоставляются со столбцом в таблице.

В том же файле добавьте следующий `MovieDBContext` класса:

[!code-csharp[Main](adding-a-model/samples/sample2.cs)]

`MovieDBContext` Класс представляет контекст базы данных Entity Framework фильм, который обрабатывает выборка, хранения и обновления `Movie` класса экземпляров в базе данных. `MovieDBContext` Является производным от `DbContext` базовый класс, предоставляемый платформой Entity Framework.

Чтобы иметь возможность ссылаться на `DbContext` и `DbSet`, необходимо добавить следующие `using` инструкции в верхней части файла:

[!code-csharp[Main](adding-a-model/samples/sample3.cs)]

Полный *Movie.cs* файла показан ниже. (Несколько с помощью инструкций, которые не требуется были удалены.)

[!code-csharp[Main](adding-a-model/samples/sample4.cs)]

## <a name="creating-a-connection-string-and-working-with-sql-server-localdb"></a>Создание строки подключения и работе с SQL Server LocalDB

`MovieDBContext` Созданный класс обрабатывает задачу подключения к базе данных и сопоставления `Movie` объектов для записи базы данных. Один вопрос, который может возникнуть вопрос, является, как указать базу данных, в которой будут подключаться. Который предстоит выполнить путем добавления сведений о соединении в *Web.config* файл приложения.

Откройте корневой каталог приложения *Web.config* файла. (Не *Web.config* файла в *представления* папки.) Откройте *Web.config* файл выделено красным цветом.

![](adding-a-model/_static/image2.png)

Добавьте следующую строку подключения для `<connectionStrings>` элемент в *Web.config* файла.

[!code-xml[Main](adding-a-model/samples/sample5.xml)]

В следующем примере показано часть *Web.config* файл с добавить новую строку подключения:

[!code-xml[Main](adding-a-model/samples/sample6.xml?highlight=6-9)]

Этот небольшой объем кода и XML предоставляет все необходимое для записи для представления и хранить в базе данных фильма.

Далее мы создадим новый `MoviesController` класс, который можно использовать для отображения данных фильма и позволяют пользователям создавать новые вхождения фильма.

> [!div class="step-by-step"]
> [Назад](adding-a-view.md)
> [Вперед](accessing-your-models-data-from-a-controller.md)
