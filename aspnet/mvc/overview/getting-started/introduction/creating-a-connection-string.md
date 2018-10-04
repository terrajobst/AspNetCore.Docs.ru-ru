---
uid: mvc/overview/getting-started/introduction/creating-a-connection-string
title: Создание строки подключения и работа с SQL Server LocalDB | Документация Майкрософт
author: Rick-Anderson
description: ''
ms.author: riande
ms.date: 10/17/2013
ms.assetid: 6127804d-c1a9-414d-8429-7f3dd0f56e97
msc.legacyurl: /mvc/overview/getting-started/introduction/creating-a-connection-string
msc.type: authoredcontent
ms.openlocfilehash: 746101344832793b199d2b3f3dfcfcd4e3b9a8da
ms.sourcegitcommit: 7b4e3936feacb1a8fcea7802aab3e2ea9c8af5b4
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/04/2018
ms.locfileid: "48578527"
---
<a name="creating-a-connection-string-and-working-with-sql-server-localdb"></a>Создание строки подключения и работа с SQL Server LocalDB
====================
по [Рик Андерсон]((https://twitter.com/RickAndMSFT))

[!INCLUDE [Tutorial Note](sample/code-location.md)]

## <a name="creating-a-connection-string-and-working-with-sql-server-localdb"></a>Создание строки подключения и работа с SQL Server LocalDB

`MovieDBContext` Созданный класс обрабатывает задачи подключения к базе данных и сопоставления `Movie` объектов для записи базы данных. Один вопрос, на который вы можете спросить, однако, как указать базу данных, которая подключается к. Вы фактически нет необходимости указывать базу данных использовать, Entity Framework по умолчанию будет использовать [LocalDB](https://docs.microsoft.com/sql/database-engine/configure-windows/sql-server-2016-express-localdb). В этом разделе мы будем явно добавить строку подключения в *Web.config* файл приложения.

## <a name="sql-server-express-localdb"></a>SQL Server Express LocalDB

[LocalDB](https://docs.microsoft.com/sql/database-engine/configure-windows/sql-server-2016-express-localdb) — это облегченная версия SQL Server Express Database Engine, запускаемая по запросу и работает в пользовательском режиме. LocalDB выполняется в специального режима выполнения SQL Server Express, которая позволяет работать с базами данных как *.mdf* файлов. Как правило, хранятся файлы базы данных LocalDB в *приложения\_данных* папку веб-проекта.

SQL Server Express не рекомендуется для использования в рабочей среде веб-приложений. LocalDB в частности не следует для рабочей среды веб-приложению, так как он не предназначен для работы со службами IIS. Тем не менее базу данных LocalDB можно легко перенести в SQL Server или SQL Azure.

В Visual Studio 2017 LocalDB устанавливается по умолчанию с помощью Visual Studio.

По умолчанию, Entity Framework ищет строку подключения с именем, так же, как класс контекста объекта (`MovieDBContext` для этого проекта). Дополнительные сведения см. в разделе [строки подключения SQL Server для веб-приложений ASP.NET](https://msdn.microsoft.com/library/jj653752.aspx).

Откройте корневой каталог приложения *Web.config* файл, показанный ниже. (Не *Web.config* файл *представления* папки.)

![](creating-a-connection-string/_static/image1.png)

Найти `<connectionStrings>` элемент:

![](creating-a-connection-string/_static/image2.png)

Добавьте следующую строку подключения для `<connectionStrings>` элемент в *Web.config* файл.

[!code-xml[Main](creating-a-connection-string/samples/sample1.xml)]

В следующем примере показано часть *Web.config* файл с добавить новую строку подключения:

[!code-xml[Main](creating-a-connection-string/samples/sample2.xml)]

Две строки подключения, очень похожи. Первая строка подключения имеет имя `DefaultConnection` и используется для базы данных членства для контроля пользователей, которые могут работать с приложением. В строке подключения, вы добавили указан с именем базы данных LocalDB *Movie.mdf* в *приложения\_данных* папки. Мы не использовать базы данных членства в этом руководстве, Дополнительные сведения о членстве, проверка подлинности и безопасности, см. в разделе my руководства [Создание приложения ASP.NET MVC с проверкой подлинности и база данных SQL и развертывание в службе приложений Azure](https://docs.microsoft.com/aspnet/core/security/authorization/secure-data).

Имя строки подключения должно соответствовать имя [DbContext](https://msdn.microsoft.com/library/system.data.entity.dbcontext(v=vs.103).aspx) класса.

[!code-csharp[Main](creating-a-connection-string/samples/sample3.cs?highlight=15)]

Вам не нужен добавить `MovieDBContext` строку подключения. Если не указать строку подключения, Entity Framework создаст базу данных LocalDB в каталоге пользователи с полным именем из [DbContext](https://msdn.microsoft.com/library/system.data.entity.dbcontext(v=vs.103).aspx) класса (в данном случае `MvcMovie.Models.MovieDBContext`). Вы можно назвать базы данных вам нравится, до тех пор, пока он имеет *. MDF* суффикс. Например, можно назвать базе *MyFilms.mdf*.

Далее предстоит создать новый `MoviesController` класс, который можно использовать для отображения данных фильма и разрешить пользователям создавать новые вхождения фильма.

> [!div class="step-by-step"]
> [Назад](adding-a-model.md)
> [Вперед](accessing-your-models-data-from-a-controller.md)
