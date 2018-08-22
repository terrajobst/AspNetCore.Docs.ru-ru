---
uid: web-api/overview/data/using-web-api-with-entity-framework/part-3
title: Использовать Code First Migrations заполнить базу данных | Документация Майкрософт
author: MikeWasson
description: ''
ms.author: riande
ms.date: 06/16/2014
ms.assetid: 76e2013a-65b7-488c-834d-9448ecea378e
msc.legacyurl: /web-api/overview/data/using-web-api-with-entity-framework/part-3
msc.type: authoredcontent
ms.openlocfilehash: 533d3a6e9a69584fb2523b2c0a604755e372f031
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 08/16/2018
ms.locfileid: "41837656"
---
<a name="use-code-first-migrations-to-seed-the-database"></a>Использовать Code First Migrations заполнить базу данных
====================
по [Майк Уоссон](https://github.com/MikeWasson)

[Скачать завершенный проект](https://github.com/MikeWasson/BookService)

В этом разделе вы воспользуетесь [Code First Migrations](https://msdn.microsoft.com/data/jj591621) в EF для заполнения базы тестовыми данными.

Из **средства** меню, выберите **диспетчер пакетов библиотеки**, а затем выберите **консоль диспетчера пакетов**. В окне консоли диспетчера пакетов введите следующую команду:

[!code-console[Main](part-3/samples/sample1.cmd)]

Эта команда добавляет папку с именем миграции в проект, а также файл кода с именем Configuration.cs в папку Migrations.

![](part-3/_static/image1.png)

Откройте файл Configuration.cs. Добавьте следующий **с помощью** инструкции.

[!code-csharp[Main](part-3/samples/sample2.cs)]

Затем добавьте следующий код, чтобы **Configuration.Seed** метод:

[!code-csharp[Main](part-3/samples/sample3.cs)]

В окне консоли диспетчера пакетов введите следующие команды:

[!code-console[Main](part-3/samples/sample4.cmd)]

Первая команда создает код, который создает базу данных, а вторая команда выполняет этот код. База данных создается локально, с помощью [LocalDB](https://msdn.microsoft.com/library/hh510202.aspx).

![](part-3/_static/image2.png)

## <a name="explore-the-api-optional"></a>Изучить API (необязательно)

Нажмите клавишу F5, чтобы запустить приложение в режиме отладки. Visual Studio запускает IIS Express и запускает веб-приложения. Затем Visual Studio запустит браузер и откроется домашняя страница приложения.

При запуске веб-проекта в Visual Studio, он назначает номер порта. На рисунке ниже номер порта — 50524. При запуске приложения вы увидите другой номер порта.

![](part-3/_static/image3.png)

На домашней странице реализуется с помощью ASP.NET MVC. В верхней части страницы есть ссылка с текстом «API». Эта ссылка позволяет открыть страницу автоматически созданную справку для веб-API. (Чтобы узнать, как создается Эта страница справки, и как можно добавить вашу собственную документацию на страницу, см. в разделе [Создание страницы справки для веб-API ASP.NET](../../getting-started-with-aspnet-web-api/creating-api-help-pages.md).) Можно щелкнуть справки странице приведены ссылки, чтобы просмотреть сведения об API, включая формат запроса и ответа.

![](part-3/_static/image4.png)

API-Интерфейс позволяет операции CRUD в базе данных. В следующей таблице показаны API.

| Authors |  |
| --- | -- |
| Получение api/authors | Получение всех авторов. |
| GET api/authors / {id} | Получить автора по идентификатору. |
| POST/api/authors | Создание нового автора. |
| PUT/API/authors / {id} | Обновите существующие автора. |
| DELETE/API/authors / {id} | Удалите автора. |

| Books |  |
| --- | -- |
| ПОЛУЧИТЬ /api/books | Получите все книги. |
| GET/API/books / {id} | Получите книгу по идентификатору. |
| POST/api/книг | Создайте новую книгу. |
| PUT/API/books / {id} | Обновите существующую книгу. |
| DELETE/API/books / {id} | Удалите книгу. |

## <a name="view-the-database-optional"></a>Представление базы данных (необязательно)

При выполнении команды Update-Database, EF создает базу данных и вызывается `Seed` метод. При локальном запуске приложения EF использует [LocalDB](https://blogs.msdn.com/b/sqlexpress/archive/2011/07/12/introducing-localdb-a-better-sql-express.aspx). Базы данных можно просмотреть в Visual Studio. Из **представление** меню, выберите **обозреватель объектов SQL Server**.

![](part-3/_static/image5.png)

В **соединение с сервером** диалоговое окно, в **имя_сервера** поле ввода, введите «(localdb) \v11.0». Оставьте **проверки подлинности** для параметра «Проверка подлинности Windows». Нажмите кнопку **Подключиться**.

![](part-3/_static/image6.png)

Visual Studio подключается к LocalDB и отображает существующие базы данных в окне обозревателя объектов SQL Server. Можно развернуть узлы для просмотра таблиц, которые созданы EF.

![](part-3/_static/image7.png)

Чтобы просмотреть данные, щелкните правой кнопкой мыши таблицу и выберите **данные представления**.

![](part-3/_static/image8.png)

На следующем рисунке показан результаты в электронной таблице. Обратите внимание на то, что EF заполняет базу данных начального значения, а таблица содержит внешний ключ к таблице Authors.

![](part-3/_static/image9.png)

> [!div class="step-by-step"]
> [Назад](part-2.md)
> [Вперед](part-4.md)
