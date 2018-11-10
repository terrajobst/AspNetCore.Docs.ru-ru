---
title: Добавление модели в приложение Razor Pages в ASP.NET Core с помощью Visual Studio Code
author: rick-anderson
description: Узнайте, как добавить модель в приложение Razor Pages в ASP.NET Core с помощью Visual Studio Code.
monikerRange: '>= aspnetcore-2.0'
ms.author: riande
ms.date: 08/27/2017
uid: tutorials/razor-pages-vsc/model
ms.openlocfilehash: c4aef369bb3965b70d1b461cf63e6f5a26a00628
ms.sourcegitcommit: c43a6f1fe72d7c2db4b5815fd532f2b45d964e07
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/30/2018
ms.locfileid: "50244727"
---
# <a name="add-a-model-to-an-aspnet-core-razor-pages-app-with-visual-studio-code"></a>Добавление модели в приложение Razor Pages в ASP.NET Core с помощью Visual Studio Code

[!INCLUDE [model1](../../includes/RP/model1.md)]

## <a name="add-a-data-model"></a>Добавление модели данных

* Добавьте папку с именем *Models*.
* Добавьте класс в папку *Models* с именем *Movie.cs*.
* Добавьте в файл *Models/Movie.cs* следующий код:

[!INCLUDE [model 2](../../includes/RP/model2.md)]

[!INCLUDE [model 2a](../../includes/RP/model2a.md)]

### <a name="entity-framework-core-nuget-package-for-sqlite"></a>Пакет Entity Framework Core NuGet для SQLite

В командной строке выполните следующую команду .NET Core:

```console
dotnet add package Microsoft.EntityFrameworkCore.SQLite
```

<a name="reg"></a>

### <a name="register-the-database-context"></a>Регистрация контекста базы данных

Зарегистрируйте контекст базы данных в контейнере [внедрения зависимостей](xref:fundamentals/dependency-injection) в файле *Startup.cs*.

[!code-csharp[](../../tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie/Startup.cs?name=snippet_ConfigureServices2&highlight=10-11)]

Добавьте следующие инструкции `using` в начало файла *Startup.cs*.

```csharp
using RazorPagesMovie.Models;
using Microsoft.EntityFrameworkCore;
```

Выполните сборку проекта, чтобы убедиться в отсутствии ошибок.

[!INCLUDE [model 3](../../includes/RP/model3.md)]

<a name="scaffold"></a>

### <a name="scaffold-the-movie-model"></a>Модель создания фильма

* Откройте окно командной строки в папке проекта (папке, где находятся файлы *Program.cs*, *Startup.cs* и *.csproj* файлов).
* **Для Windows**: выполните следующую команду.

  ```console
  dotnet aspnet-codegenerator razorpage -m Movie -dc MovieContext -udl -outDir Pages\Movies --referenceScriptLibraries
  ```

* **Для MacOS и Linux**: выполните следующую команду.

  ```console
  dotnet aspnet-codegenerator razorpage -m Movie -dc MovieContext -udl -outDir Pages/Movies --referenceScriptLibraries
  ```

[!INCLUDE [model 4](../../includes/RP/model4.md)]

В следующем учебнике рассматриваются файлы, созданные с помощью формирования шаблонов.

> [!div class="step-by-step"]
> [Предыдущая статья — "Начало работы"](xref:tutorials/razor-pages-vsc/razor-pages-start)
> [Следующая статья — "Сформированные страницы Razor Pages"](xref:tutorials/razor-pages-vsc/page)
