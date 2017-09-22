---
title: "Основные ASP.NET MVC с основными EF - Migrations - 4 из 10"
author: tdykstra
description: "В этом учебнике сначала с помощью EF Core миграции для управления изменений модели данных в приложении ASP.NET Core MVC."
keywords: "ASP.NET Core,Entity Framework Core,миграция"
ms.author: tdykstra
manager: wpickett
ms.date: 03/15/2017
ms.topic: get-started-article
ms.assetid: 81f6c9c2-a819-4f3a-97a4-4b0503b56c26
ms.technology: aspnet
ms.prod: asp.net-core
uid: data/ef-mvc/migrations
ms.openlocfilehash: 638bef0cda14f53a326c66c6a5da3f3c1bb762c6
ms.sourcegitcommit: 78d28178345a0eea91556e4cd1adad98b1446db8
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 09/22/2017
---
# <a name="migrations---ef-core-with-aspnet-core-mvc-tutorial-4-of-10"></a>Миграция - Core EF учебнику ASP.NET Core MVC (4 из 10)

По [Tom Dykstra](https://github.com/tdykstra) и [Рик Андерсон](https://twitter.com/RickAndMSFT)

Contoso университета примера веб-приложения показано, как создавать веб-приложения ASP.NET MVC ядра с помощью основного Entity Framework и Visual Studio. Сведения о учебника серии см [в первом учебнике ряда](intro.md).

В этом учебнике сначала с помощью EF Core миграции для управления изменения в модели данных. На следующих занятиях рассматривается вы добавите дополнительные миграции при изменении модели данных.

## <a name="introduction-to-migrations"></a>Общие сведения о миграции

При разработке нового приложения модель данных меняется часто и каждый раз изменения модели, она возвращает синхронизации с базой данных. Эти учебники запущенную настройка платформы Entity Framework для создания базы данных, если он не существует. Затем при каждом изменении модели данных — добавить, удалить, или классы сущностей или изменить класс DbContext — можно удалить базу данных и EF создает новый, который соответствует модели и заполняет его с тестовыми данными.

Синхронизация базы данных с моделью данных, этот метод работает хорошо, пока развертывание приложения в рабочей среде. Когда приложение выполняется в производственной среде, он обычно хранит данные, которые вы хотите сохранить, и вы не хотите потерять все, что каждый раз вносятся изменения, такие как добавление нового столбца. Средство миграции Core EF решает эту проблему, позволяя EF обновить схему базы данных вместо создания новой базы данных.

## <a name="entity-framework-core-nuget-packages-for-migrations"></a>Пакеты Entity Framework Core NuGet для миграций

Для работы с миграции, можно использовать **консоль диспетчера пакетов** (PMC) или интерфейса командной строки (CLI).  Эти учебники демонстрируют использование команды CLI. Сведения о PMC находится в [конце этого учебника](#pmc).

Средства EF для интерфейса командной строки (CLI) доступны в [Microsoft.EntityFrameworkCore.Tools.DotNet](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.Tools.DotNet). Чтобы установить этот пакет, добавьте его в `DotNetCliToolReference` коллекции в *.csproj* файла, как показано. **Примечание**. Необходимо установить этот пакет, отредактировав файл *.csproj*; использовать команду `install-package` или графический пользовательский интерфейс диспетчера пакетов нельзя. Можно изменить *.csproj* файла, щелкнув правой кнопкой мыши имя проекта в **обозревателе решений** и выбрав **изменить ContosoUniversity.csproj**.

[!code-xml[](intro/samples/cu/ContosoUniversity.csproj?range=12-15&highlight=2)]
  
(Номера версий, в этом примере были действительны, во время написания учебника.) 

## <a name="change-the-connection-string"></a>Измените строку подключения

В *appsettings.json* файл, измените имя базы данных в строке соединения на ContosoUniversity2 или другое имя, вы не использовали на компьютере, который вы используете.

[!code-json[Main](intro/samples/cu/appsettings2.json?range=1-4)]

Это изменение настраивает проект, чтобы первый миграции создает новую базу данных. Это не является необходимым для начала работы с миграции, но вы увидите Далее почему он имеет смысл.

> [!NOTE]
> В качестве альтернативы для изменения имени базы данных можно удалить базы данных. Используйте **обозреватель объектов SQL Server** (SSOX) или `database drop` команду CLI:
> ```console
> dotnet ef database drop
> ```
> Следующий раздел объясняет, как для выполнения команд CLI.

## <a name="create-an-initial-migration"></a>Создание первоначальной миграции

Сохраните изменения и выполните построение проекта. Затем откройте окно командной строки и перейдите в папку проекта. Вот быстрый способ сделать это:

* В **обозревателе решений**, щелкните правой кнопкой мыши проект и выберите **открыть в проводнике** в контекстном меню.

  ![Открыть в проводнике пункта меню](migrations/_static/open-in-file-explorer.png)

* В адресной строке введите «cmd» и нажмите клавишу ВВОД.

  ![Откройте командное окно](migrations/_static/open-command-window.png)

Введите в командном окне следующую команду:

```console
dotnet ef migrations add InitialCreate
```

Вывод в командной строке следующим образом:

```console
info: Microsoft.AspNetCore.DataProtection.KeyManagement.XmlKeyManager[0]
      User profile is available. Using 'C:\Users\username\AppData\Local\ASP.NET\DataProtection-Keys' as key repository and Windows DPAPI to encrypt keys at rest.
info: Microsoft.EntityFrameworkCore.Infrastructure[100403]
      Entity Framework Core 2.0.0-rtm-26452 initialized 'SchoolContext' using provider 'Microsoft.EntityFrameworkCore.SqlServer' with options: None
Done. To undo this action, use 'ef migrations remove'
```

> [!NOTE]
> Если вы видите сообщение об ошибке *не исполняемый объект найден соответствующий команда «dotnet-ef»*, в разделе [этой записи блога](http://thedatafarm.com/data-access/no-executable-found-matching-command-dotnet-ef/) справку по устранению неполадок.

Если вы видите сообщение об ошибке «*не может получить доступ к файла... ContosoUniversity.dll, так как он используется другим процессом. *» значок IIS Express поиска на панели задач Windows, щелкните его правой кнопкой мыши, затем щелкните **ContosoUniversity > остановка узла**.

## <a name="examine-the-up-and-down-methods"></a>Изучите вверх и вниз методы

При выполнении `migrations add` команды EF созданный код, который создаст базу данных с нуля. Этот код находится в *миграций* папки в файл с именем * \<timestamp > _InitialCreate.cs*. `Up` Метод `InitialCreate` класс создает таблицы базы данных, которые соответствуют наборов сущностей модели данных, и `Down` метод удаляет их, как показано в следующем примере.

[!code-csharp[Main](intro/samples/cu/Migrations/20170215220724_InitialCreate.cs?range=92-118)]

Миграция вызовы `Up` метод для реализации изменений модели данных для миграции. При вводе команды для отката обновления, вызывает миграции `Down` метод.

Данный пример кода является для первоначальной миграции, который был создан при вводе `migrations add InitialCreate` команды. Параметр name миграции («InitialCreate» в примере) используется для имени файла, а также можно указать любое. Рекомендуется выбрать слово или фразу, в которой говорится, что выполняется в процессе переноса. Например может имя последующей миграции «AddDepartmentTable».

При создании первоначальной миграции базы данных уже существует, создается код создания базы данных, но он не должен выполняться, так как база данных, уже соответствует модели данных. При развертывании приложения в другую среду, где базы данных не существует, однако, этот код будет выполняться Создание базы данных, поэтому рекомендуется сначала протестировать. Вот почему было изменено имя базы данных в более ранней версии — строку подключения, чтобы миграции можно создать новую с нуля.

## <a name="examine-the-data-model-snapshot"></a>Изучите снимок модели данных

Миграция также создает *моментального снимка* текущей схемы базы данных в *Migrations/SchoolContextModelSnapshot.cs*. Вот, как выглядит этот код:

[!code-csharp[Main](intro/samples/cu/Migrations/SchoolContextModelSnapshot1.cs?name=snippet_Truncate)]

Поскольку текущей схемы базы данных представлен в коде, EF Core не должен взаимодействовать с базой данных для создания миграции. При добавлении миграции EF определяет, что изменилось, сравнивая модели данных в файле моментального снимка. EF взаимодействует с базой данных, только когда требуется обновить базу данных. 

Должен обеспечивать синхронизацию с миграций, которые ее создать, поэтому невозможно удалить миграции, просто удалив файл с именем файла моментального снимка * \<timestamp > _\<migrationname > .cs*. Если вы удалите этот файл, оставшиеся операции миграции будут синхронизированы с файлом моментального снимка базы данных. Чтобы удалить добавленный последней миграции, используйте [dotnet ef миграции удалить](https://docs.microsoft.com/ef/core/miscellaneous/cli/dotnet#dotnet-ef-migrations-remove) команды.

## <a name="apply-the-migration-to-the-database"></a>Применить к базе данных миграции

В окне командной строки введите следующую команду, чтобы создать базу данных и таблиц в ней.

```console
dotnet ef database update
```

Выходные данные команды аналогичен `migrations add` команды, за исключением того, что вы видите журналы для команд, Настройка базы данных SQL. Большинство журналов опущены в следующий результат. Если вы предпочитаете не см. такой уровень детализации сообщений журнала, можно изменить уровни журнала в *appsettings. Development.JSON* файл. Дополнительные сведения см. в разделе [введение к ведению журнала](xref:fundamentals/logging).

```text
info: Microsoft.AspNetCore.DataProtection.KeyManagement.XmlKeyManager[0]
      User profile is available. Using 'C:\Users\username\AppData\Local\ASP.NET\DataProtection-Keys' as key repository and Windows DPAPI to encrypt keys at rest.
info: Microsoft.EntityFrameworkCore.Infrastructure[100403]
      Entity Framework Core 2.0.0-rtm-26452 initialized 'SchoolContext' using provider 'Microsoft.EntityFrameworkCore.SqlServer' with options: None
info: Microsoft.EntityFrameworkCore.Database.Command[200101]
      Executed DbCommand (467ms) [Parameters=[], CommandType='Text', CommandTimeout='60']
      CREATE DATABASE [ContosoUniversity2];
info: Microsoft.EntityFrameworkCore.Database.Command[200101]
      Executed DbCommand (20ms) [Parameters=[], CommandType='Text', CommandTimeout='30']
      CREATE TABLE [__EFMigrationsHistory] (
          [MigrationId] nvarchar(150) NOT NULL,
          [ProductVersion] nvarchar(32) NOT NULL,
          CONSTRAINT [PK___EFMigrationsHistory] PRIMARY KEY ([MigrationId])
      );

<logs omitted for brevity>

info: Microsoft.EntityFrameworkCore.Database.Command[200101]
      Executed DbCommand (3ms) [Parameters=[], CommandType='Text', CommandTimeout='30']
      INSERT INTO [__EFMigrationsHistory] ([MigrationId], [ProductVersion])
      VALUES (N'20170816151242_InitialCreate', N'2.0.0-rtm-26452');
Done.
```

Используйте **обозреватель объектов SQL Server** проверить базу данных, как описано в первом руководстве.  Вы заметите Добавление таблицы __EFMigrationsHistory, отслеживающей миграций, которые были применены к базе данных. Просмотр данных в этой таблице, и вы увидите одну строку для первой миграции. (Последний журнал в предыдущем примере вывода CLI показывает инструкции INSERT, которая создает эту строку).

Запустите приложение, чтобы убедиться, что все работает по-прежнему прежней.

![Страница индекса студентов](migrations/_static/students-index.png)

<a id="pmc"></a>
## <a name="command-line-interface-cli-vs-package-manager-console-pmc"></a>Интерфейс командной строки (CLI) vs. Консоль диспетчера пакетов (PMC)

EF средства управления миграции доступен из команды .NET Core CLI или командлеты PowerShell в Visual Studio **консоль диспетчера пакетов** (PMC) окна. Этот учебник показывает, как использовать CLI, но при желании можно указать PMC.

EF команды для команд PMC, в [Microsoft.EntityFrameworkCore.Tools](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.Tools) пакета. Этот пакет уже включены в [Microsoft.AspNetCore.All](xref:fundamentals/metapackage) metapackage, поэтому устанавливать его не требуется.

**Важно:** это не тот же пакет, как установить для CLI, изменив *.csproj* файла. Имя данного объекта заканчивается на `Tools`, в отличие от имени пакета CLI, который заканчивается на `Tools.DotNet`.

Дополнительные сведения о командах CLI см. в разделе [.NET Core CLI](https://docs.microsoft.com/ef/core/miscellaneous/cli/dotnet). 

Дополнительные сведения о командах PMC см. в разделе [Package Manager Console (Visual Studio)](https://docs.microsoft.com/ef/core/miscellaneous/cli/powershell).

## <a name="summary"></a>Сводка

В этом учебнике вы знаете, как создать и применить переноса первого. В следующем уроке вы начнете, просмотрев более сложные вопросы, развернув модели данных. Попутно вы создадите и применения дополнительных миграции.

>[!div class="step-by-step"]
[Назад](sort-filter-page.md)
[Вперед](complex-data-model.md)  
