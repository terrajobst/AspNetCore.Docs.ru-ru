---
title: "Добавление модели в приложение Razor Pages с помощью Visual Studio для Mac"
author: rick-anderson
description: "Добавление модели в приложение Razor Pages в ASP.NET Core с помощью Visual Studio для Mac"
keywords: "ASP.NET Core,Razor Pages,Razor,MVC,модель"
ms.author: riande
manager: wpickett
ms.date: 08/27/2017
ms.topic: get-started-article
ms.technology: aspnet
ms.prod: aspnet-core
uid: tutorials/razor-pages-vsc/model
ms.openlocfilehash: f2f09909f4c307ce3e1a0c8571b82a709e1f88f9
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 11/10/2017
---
# <a name="adding-a-model-to-a-razor-pages-app-in-aspnet-core-with-visual-studio-for-mac"></a>Добавление модели в приложение Razor Pages в ASP.NET Core с помощью Visual Studio для Mac

[!INCLUDE[model1](../../includes/RP/model1.md)]

## <a name="add-a-data-model"></a>Добавление модели данных

* Добавьте папку с именем *Models*.
* Добавьте класс в папку *Models* с именем *Movie.cs*.
* Добавьте в файл *ToUpperPlugin.cs* следующий код:

[!INCLUDE[model 2](../../includes/RP/model2.md)]
[!INCLUDE[model 2a](../../includes/RP/model2a.md)]

[!code-csharp[Main](../../tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie/Startup.cs?name=snippet_ConfigureServices2&highlight=3-6)]

Выполните сборку проекта, чтобы убедиться в отсутствии ошибок.

### <a name="entity-framework-core-nuget-packages-for-migrations"></a>Пакеты Entity Framework Core NuGet для миграций

Средства EF для интерфейса командной строки (CLI) доступны в [Microsoft.EntityFrameworkCore.Tools.DotNet](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.Tools.DotNet). Чтобы установить этот пакет, добавьте его в коллекцию `DotNetCliToolReference` в файле *.csproj*. **Примечание**. Необходимо установить этот пакет, отредактировав файл *.csproj*; использовать команду `install-package` или графический пользовательский интерфейс диспетчера пакетов нельзя.

Измените файл *RazorPagesMovie.csproj*.

* Выберите **Файл** > **Открыть файл**, а затем выберите файл *RazorPagesMovie.csproj*.
* Добавьте ссылку на инструмент `Microsoft.EntityFrameworkCore.Tools.DotNet` во вторую группу **\<ItemGroup>**:

[!code-xml[Main](../../tutorials/razor-pages/razor-pages-start/snapshot_cli_sample/RazorPagesMovie/RazorPagesMovie.cli.csproj?range=12-16&highlight=4)]

[!INCLUDE[model 3](../../includes/RP/model3.md)]

<a name="scaffold"></a>
### <a name="scaffold-the-movie-model"></a>Модель создания фильма

* Откройте окно командной строки в папке проекта (папке, где находятся файлы *Program.cs*, *Startup.cs* и *.csproj* файлов).
* Выполните следующую команду:

**Примечание. Выполните в Windows следующую команду. Команда для MacOS и Linux указана ниже**.

  ```console
  dotnet aspnet-codegenerator razorpage -m Movie -dc MovieContext -udl -outDir Pages\Movies --referenceScriptLibraries
  ```

* На MacOS и Linux выполните следующую команду.

  ```console
  dotnet aspnet-codegenerator razorpage -m Movie -dc MovieContext -udl -outDir Pages/Movies --referenceScriptLibraries
  ```

Если возникает ошибка.
  ```
  The process cannot access the file 
 'RazorPagesMovie/bin/Debug/netcoreapp2.0/RazorPagesMovie.dll' 
  because it is being used by another process.
  ```

Выйдите из Visual Studio и выполните команду еще раз.

[!INCLUDE[model 4](../../includes/RP/model4.md)] В следующем учебнике рассматриваются файлы, созданные с помощью формирования шаблонов.

>[!div class="step-by-step"]
[Назад: Начало работы](xref:tutorials/razor-pages-vsc/razor-pages-start)
[Далее: Сформированные страницы Razor Pages](xref:tutorials/razor-pages/page)
