---
uid: web-api/overview/data/using-web-api-with-entity-framework/part-3
title: "Использовать для инициализации базы данных Code First Migrations | Документы Microsoft"
author: MikeWasson
description: 
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/16/2014
ms.topic: article
ms.assetid: 76e2013a-65b7-488c-834d-9448ecea378e
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/data/using-web-api-with-entity-framework/part-3
msc.type: authoredcontent
ms.openlocfilehash: df75a69644033cc76fee86b5a9692ab65beb4d01
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 11/10/2017
---
<a name="use-code-first-migrations-to-seed-the-database"></a>Использовать Code First Migrations для инициализации базы данных
====================
по [Mike Wasson](https://github.com/MikeWasson)

[Загрузка завершенного проекта](https://github.com/MikeWasson/BookService)

В этом разделе используется [Code First Migrations](https://msdn.microsoft.com/en-us/data/jj591621) в EF в качестве начального базы данных с тестовыми данными.

Из **средства** последовательно выберите пункты **диспетчер пакетов библиотеки**, а затем выберите **консоль диспетчера пакетов**. В окне консоли диспетчера пакетов введите следующую команду:

[!code-console[Main](part-3/samples/sample1.cmd)]

Эта команда добавляет папку с именем миграции в проект, а также файл кода с именем Configuration.cs в папке миграции.

![](part-3/_static/image1.png)

Откройте файл Configuration.cs. Добавьте следующие **с помощью** инструкции.

[!code-csharp[Main](part-3/samples/sample2.cs)]

Затем добавьте следующий код в **Configuration.Seed** метод:

[!code-csharp[Main](part-3/samples/sample3.cs)]

В окне консоли диспетчера пакетов введите следующие команды:

[!code-console[Main](part-3/samples/sample4.cmd)]

Первая команда создает код, который создает базу данных, а вторая команда выполняет этот код. База данных создается локально, с помощью [LocalDB](https://msdn.microsoft.com/en-us/library/hh510202.aspx).

![](part-3/_static/image2.png)

## <a name="explore-the-api-optional"></a>Просмотр API (необязательно)

Нажмите клавишу F5 для запуска приложения в режиме отладки. Visual Studio запускает IIS Express и веб-приложения. Затем Visual Studio запускает браузер и открывает домашнюю страницу приложения.

При запуске веб-проекта Visual Studio назначается номер порта. На рисунке ниже номер порта — 50524. При запуске приложения вы увидите другой номер порта.

![](part-3/_static/image3.png)

На домашней странице реализуется с помощью ASP.NET MVC. В верхней части страницы есть ссылка с надписью «API». Эта ссылка позволяет на страницу справки, автоматически созданный для веб-API. (Чтобы узнать, как создается Эта страница справки и сведения о добавлении вашей собственной документации на страницу, в разделе [Создание страницах справки для веб-API ASP.NET](../../getting-started-with-aspnet-web-api/creating-api-help-pages.md).) Справка щелкайте ссылки на страницы для просмотра сведений об API-Интерфейсе, включая формат запроса и ответа.

![](part-3/_static/image4.png)

API-Интерфейс позволяет операций CRUD в базе данных. В следующей таблице показаны API.

| Authors |  |
| --- | -- |
| Получение api и авторов | Получение всех авторов. |
| Api GET/авторы / {id} | Получить автора по идентификатору. |
| POST/api/авторов | Создание нового автора. |
| ПОМЕСТИТЕ /api/авторы / {id} | Обновите существующие автора. |
| УДАЛИТЬ /api/авторы / {id} | Удалите автора. |

| Books |  |
| --- | -- |
| ПОЛУЧИТЬ /api/books | Получение всех книг. |
| ПОЛУЧИТЬ /api/books / {id} | Получение книги по идентификатору. |
| ОТПРАВЛЯТЬ/api/книги | Создайте новую книгу. |
| ПОМЕСТИТЕ /api/books / {id} | Обновите существующую книгу. |
| УДАЛИТЬ /api/books / {id} | Удалите книгу. |

## <a name="view-the-database-optional"></a>Просмотр базы данных (необязательно)

При выполнении команды Update-Database, EF создана база данных и вызывается `Seed` метод. При локальном запуске приложения использует EF [LocalDB](https://blogs.msdn.com/b/sqlexpress/archive/2011/07/12/introducing-localdb-a-better-sql-express.aspx). В Visual Studio можно просматривать базы данных. Из **представление** последовательно выберите пункты **обозреватель объектов SQL Server**.

![](part-3/_static/image5.png)

В **соединение с сервером** диалоговое окно, в **имя сервера** "Правка", введите «(localdb) \v11.0». Оставить **проверки подлинности** параметра «Проверка подлинности Windows». Нажмите кнопку **Подключиться**.

![](part-3/_static/image6.png)

Visual Studio подключится к LocalDB и показывает существующих баз данных в окне обозревателя объектов SQL Server. Можно развернуть узлы для просмотра таблицы, созданные EF.

![](part-3/_static/image7.png)

Чтобы просмотреть данные, щелкните правой кнопкой мыши таблицу и выберите **данные представления**.

![](part-3/_static/image8.png)

На следующем рисунке показан результаты для электронной таблицы. Обратите внимание, что EF наполнения базы данных с данными начального значения и таблица содержит внешний ключ к таблице Authors.

![](part-3/_static/image9.png)

>[!div class="step-by-step"]
[Назад](part-2.md)
[Вперед](part-4.md)
