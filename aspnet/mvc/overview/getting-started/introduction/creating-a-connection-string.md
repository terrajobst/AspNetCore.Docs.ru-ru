---
uid: mvc/overview/getting-started/introduction/creating-a-connection-string
title: Создание строки подключения и работе с SQL Server LocalDB | Документы Microsoft
author: Rick-Anderson
description: ''
ms.author: aspnetcontent
manager: wpickett
ms.date: 10/17/2013
ms.topic: article
ms.assetid: 6127804d-c1a9-414d-8429-7f3dd0f56e97
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/getting-started/introduction/creating-a-connection-string
msc.type: authoredcontent
ms.openlocfilehash: edbd46ef8a03670f0cb7527142babe9bd5846c7a
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/06/2018
ms.locfileid: "30867922"
---
<a name="creating-a-connection-string-and-working-with-sql-server-localdb"></a>Создание строки подключения и работе с SQL Server LocalDB
====================
по [Рик Андерсон](https://github.com/Rick-Anderson)

[!INCLUDE [Tutorial Note](sample/code-location.md)]

## <a name="creating-a-connection-string-and-working-with-sql-server-localdb"></a>Создание строки подключения и работе с SQL Server LocalDB

`MovieDBContext` Созданный класс обрабатывает задачу подключения к базе данных и сопоставления `Movie` объектов для записи базы данных. Один вопрос, который может возникнуть вопрос, является, как указать базу данных, в которой будут подключаться. Вы фактически нет необходимости указывать базу данных, используемую, Entity Framework по умолчанию будет использовать [LocalDB](https://docs.microsoft.com/sql/database-engine/configure-windows/sql-server-2016-express-localdb). В этом разделе мы будем явным образом добавить строку подключения в *Web.config* файл приложения.

## <a name="sql-server-express-localdb"></a>SQL Server Express LocalDB

[LocalDB](https://docs.microsoft.com/sql/database-engine/configure-windows/sql-server-2016-express-localdb) — это облегченная версия SQL Server Express Database Engine, запускаемая по запросу и запускается в пользовательском режиме. LocalDB выполняется в режиме выполнения специальных SQL Server Express, позволяет работать с базами данных как *.mdf* файлов. Как правило, хранятся файлы базы данных LocalDB в *приложения\_данные* папку веб-проекта.

SQL Server Express не рекомендуется для использования в рабочей среде веб-приложений. LocalDB в частности не предназначены для рабочих веб-приложению, так как он не предназначен для работы со службами IIS. Тем не менее, можно легко перенести базу данных LocalDB в SQL Server или SQL Azure.

В Visual Studio 2017 г. LocalDB устанавливается по умолчанию вместе с Visual Studio.

По умолчанию, Entity Framework ищет строку подключения, называются так же, как класс контекста объекта (`MovieDBContext` для этого проекта). Дополнительные сведения см. [строки подключения SQL Server для веб-приложений ASP.NET](https://msdn.microsoft.com/library/jj653752.aspx).

Откройте корневой каталог приложения *Web.config* файл приведенный ниже. (Не *Web.config* файла в *представления* папки.)

![](creating-a-connection-string/_static/image1.png)

Найти `<connectionStrings>` элемента:

![](creating-a-connection-string/_static/image2.png)

Добавьте следующую строку подключения для `<connectionStrings>` элемент в *Web.config* файла.

[!code-xml[Main](creating-a-connection-string/samples/sample1.xml)]

В следующем примере показано часть *Web.config* файл с добавить новую строку подключения:

[!code-xml[Main](creating-a-connection-string/samples/sample2.xml)]

Строки соединения двух очень похожи. Первая строка подключения с именем `DefaultConnection` и используется для базы данных членства для управления доступом приложения. В строке подключения, вы добавили указан с именем базы данных LocalDB *Movie.mdf* в *приложения\_данные* папки. Мы не будет использовать базу данных членства в этом учебнике, Дополнительные сведения о членстве, проверку подлинности и безопасности см. в разделе Мои учебника [создать приложение ASP.NET MVC с помощью проверки подлинности и баз данных SQL Server и развернуть в службе приложений Azure](https://docs.microsoft.com/aspnet/core/security/authorization/secure-data).

Имя строки подключения должно соответствовать имя [DbContext](https://msdn.microsoft.com/library/system.data.entity.dbcontext(v=vs.103).aspx) класса.

[!code-csharp[Main](creating-a-connection-string/samples/sample3.cs?highlight=15)]

Не требуется фактически добавить `MovieDBContext` строку подключения. Если не указать строку соединения, Entity Framework будет создать базу данных LocalDB в каталоге пользователи с полным именем [DbContext](https://msdn.microsoft.com/library/system.data.entity.dbcontext(v=vs.103).aspx) класса (в данном случае `MvcMovie.Models.MovieDBContext`). Базы данных любое имя вам нравится, при условии, что он имеет *. MDF* суффикс. Например, мы имя базы данных *MyFilms.mdf*.

Далее мы создадим новый `MoviesController` класс, который можно использовать для отображения данных фильма и позволяют пользователям создавать новые вхождения фильма.

> [!div class="step-by-step"]
> [Назад](adding-a-model.md)
> [Вперед](accessing-your-models-data-from-a-controller.md)
