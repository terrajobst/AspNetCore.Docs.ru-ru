---
title: "Страниц Razor с основными EF - Migrations - 4, 8."
author: rick-anderson
description: "В этом учебнике сначала с помощью EF Core миграции для управления изменений модели данных в приложении ASP.NET Core MVC."
manager: wpickett
ms.author: riande
ms.date: 10/15/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: get-started-article
uid: data/ef-rp/migrations
ms.openlocfilehash: e89d95702cb94556bc6e5dc73253c51acaa11578
ms.sourcegitcommit: a510f38930abc84c4b302029d019a34dfe76823b
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/30/2018
---
# <a name="migrations---ef-core-with-razor-pages-tutorial-4-of-8"></a>Миграция - Core EF учебнику страниц Razor (4, 8)

По [Tom Dykstra](https://github.com/tdykstra), [Jon P Smith](https://twitter.com/thereformedprog), и [Рик Андерсон](https://twitter.com/RickAndMSFT)

[!INCLUDE[about the series](../../includes/RP-EF/intro.md)]

В этом учебнике функция миграции EF основных компонентов для управления изменений модели данных используется.

Если возникли проблемы, не удается устранить, загрузите [завершенное приложение для данного этапа](
https://github.com/aspnet/Docs/tree/master/aspnetcore/data/ef-rp/intro/samples/StageSnapShots/cu-part4-migrations).

При разработке нового приложения, данные изменений в модели часто. Каждый раз изменения модели, модель получает синхронизации с базой данных. Этот учебник, запущенную настройка платформы Entity Framework для создания базы данных, если он не существует. Каждый раз, изменений в модели данных:

* Базы данных может быть удален.
* EF создает новый, который соответствует модели.
* Приложение заполняет DB тестовыми данными.

Такой подход для синхронизации базы данных с моделью данных работает хорошо, пока развертывание приложения в рабочей среде. Когда приложение выполняется в производственной среде, он обычно хранит данные, которые необходимо сохранять. Приложение не может начинаться с тестирования базы данных при каждом изменении (например, Добавление нового столбца). Средство миграции Core EF решает эту проблему, позволяя EF основных компонентов для обновления схемы базы данных вместо создания новой базы данных.

Вместо удаления и повторного создания базы данных, когда изменений в модели данных, миграция обновляет схему и сохраняет существующие данные.

## <a name="entity-framework-core-nuget-packages-for-migrations"></a>Пакеты Entity Framework Core NuGet для миграций

Для работы с миграций, используйте **консоль диспетчера пакетов** (PMC) или интерфейса командной строки (CLI). Эти учебники демонстрируют использование команды CLI. Сведения о PMC находится в [конце этого учебника](#pmc).

EF основные инструменты для командной строки (CLI), представлено в [Microsoft.EntityFrameworkCore.Tools.DotNet](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.Tools.DotNet). Чтобы установить этот пакет, добавьте его в `DotNetCliToolReference` коллекции в *.csproj* файла, как показано. **Примечание:** этот пакет должен быть установлен путем редактирования *.csproj* файла. `install-package` Команды или диспетчер пакетов графического интерфейса пользователя не может использоваться для установки этого пакета. Изменить *.csproj* файла, щелкнув правой кнопкой мыши имя проекта в **обозревателе решений** и выбрав **изменить ContosoUniversity.csproj**.

В следующем примере показана обновленный *.csproj* файла с помощью средств EF Core CLI выделяются:

[!code-xml[](intro/samples/cu/ContosoUniversity.csproj?highlight=12)]
  
Номера версий в предыдущем примере были действительны, во время написания учебника. Для CLI EF основных средств, имеющихся в другие пакеты используют ту же версию.

## <a name="change-the-connection-string"></a>Измените строку подключения

В *appsettings.json* файл, измените имя базы данных в строке соединения на ContosoUniversity2.

[!code-json[Main](intro/samples/cu/appsettings2.json?range=1-4)]

Изменение имени базы данных в строке подключения приводит к первой миграции для создания новой базы данных. Новые базы данных создается, поскольку его с таким именем не существует. Изменение строки подключения не требуется для начала работы с миграции.

Вместо изменения имени базы данных удаление базы данных. Используйте **обозреватель объектов SQL Server** (SSOX) или `database drop` команду CLI:

 ```console
 dotnet ef database drop
 ```

Следующий раздел объясняет, как для выполнения команд CLI.

## <a name="create-an-initial-migration"></a>Создание первоначальной миграции

Выполните построение проекта.

Откройте окно командной строки и перейдите в папку проекта. Папка проекта содержит *файла Startup.cs* файл.

В командной строке введите следующую команду:

```console
dotnet ef migrations add InitialCreate
```

В командном окне отображается информация, аналогично приведенным ниже:

```console
info: Microsoft.AspNetCore.DataProtection.KeyManagement.XmlKeyManager[0]
      User profile is available. Using 'C:\Users\username\AppData\Local\ASP.NET\DataProtection-Keys' as key repository and Windows DPAPI to encrypt keys at rest.
info: Microsoft.EntityFrameworkCore.Infrastructure[100403]
      Entity Framework Core 2.0.0-rtm-26452 initialized 'SchoolContext' using provider 'Microsoft.EntityFrameworkCore.SqlServer' with options: None
Done. To undo this action, use 'ef migrations remove'
```

Если происходит сбой с сообщением «*не может получить доступ к файла... ContosoUniversity.dll, так как он используется другим процессом.* " отображается:

* Остановить IIS Express.

   * Закройте и перезапустите Visual Studio или
   * Значок IIS Express поиска на панели задач Windows.
   * Щелкните правой кнопкой мыши значок IIS Express, а затем нажмите кнопку **ContosoUniversity > Остановить узел**.

Если сообщение об ошибке ««Сбой построения. отображается, выполните команду еще раз. Если эта ошибка появляется, оставьте комментарий в нижней части этого учебника.

### <a name="examine-the-up-and-down-methods"></a>Изучите вверх и вниз методы

Команда EF Core `migrations add` созданного кода для создания из базы данных. Этот код миграции находится в *миграций\<timestamp > _InitialCreate.cs* файл. `Up` Метод `InitialCreate` класс создает таблицы базы данных, которые соответствуют наборов сущностей модели данных. `Down` Метод удаляет их, как показано в следующем примере:

[!code-csharp[Main](intro/samples/cu/Migrations/20171026010210_InitialCreate.cs?range=8-24,77-)]

Миграция вызовы `Up` метод для реализации изменений модели данных для миграции. При вводе команды для отката обновления, вызывает миграции `Down` метод.

Предыдущий код предназначен для первоначальной миграции. Этот код создан `migrations add InitialCreate` была выполнена команда. Для имени файла используется параметр name миграции («InitialCreate» в примере). Имя миграции, может быть любое допустимое имя файла. Рекомендуется выбрать слово или фразу, в которой говорится, что выполняется в процессе переноса. Например миграции, которая добавлена таблица department может иметь название «AddDepartmentTable».

Если создается первоначальной миграции и выходит из базы данных:

* Создается код создания базы данных.
* Создание кода БД не нужно выполнить, так как базы данных уже соответствует модели данных. При выполнении кода создания DB не вносите никаких изменений, так как базы данных, уже соответствует модели данных.

При развертывании приложения в новую среду создания кода БД необходимо выполнить для создания базы данных.

Ранее строки подключения была изменена для использования нового имени для базы данных. Указанной базы данных не существует, поэтому миграция создает базу данных.

### <a name="examine-the-data-model-snapshot"></a>Изучите снимок модели данных

Миграция создает *моментального снимка* текущей схемы базы данных в *Migrations/SchoolContextModelSnapshot.cs*:

[!code-csharp[Main](intro/samples/cu/Migrations/SchoolContextModelSnapshot1.cs?name=snippet_Truncate)]

Поскольку текущая схема базы данных представлен в коде, EF Core не должны взаимодействовать с базы данных для создания миграции. При добавлении миграции EF Core определяет, что изменилось, сравнивая модели данных в файле моментального снимка. EF Core взаимодействует с базы данных, только когда требуется обновление базы данных.

Файл моментальных снимков должен быть синхронизирован с миграций, которые он создан. Нельзя удалить миграции, удалив файл  *\<timestamp > _\<migrationname > .cs*. Если этот файл удаляется, оставшиеся операции миграции не синхронизированы с файлом моментального снимка базы данных. Чтобы удалить последний переноса добавлен, используйте [dotnet ef миграции удалить](https://docs.microsoft.com/ef/core/miscellaneous/cli/dotnet#dotnet-ef-migrations-remove) команды.

## <a name="remove-ensurecreated"></a>Удалить EnsureCreated

Для раннего разработки `EnsureCreated` была использована. В этом учебнике используется миграции. `EnsureCreated`имеет следующие ограничения:

* Пропускает миграции и создает базы данных и схемы.
* Не приводит к созданию таблицы миграций.
* Можно *не* использоваться с миграции.
* Предназначен для тестирования или быстрого создания прототипов. она где базы данных будет удалена и создана заново часто.

Удалите следующую строку из `DbInitializer`:

```csharp
context.Database.EnsureCreated();
```

## <a name="apply-the-migration-to-the-db-in-development"></a>Применить к базе данных. в разработке миграции

В окне командной строки введите следующую команду, чтобы создать базы данных и таблицы.

```console
dotnet ef database update
```

Примечание: Если `update` команда возвращает ошибку «Ошибка построения.»:

* Выполните команду еще раз.
* Если ошибка повториться, выйдите из Visual Studio, а затем запустите `update` команды.
* Оставьте сообщение в нижней части страницы.

Выходные данные команды аналогичен `migrations add` выходные данные команды. В предыдущей команде отображаются журналы для команды SQL, которые настроены в базе данных. Большинство журналов опущены в следующий результат:

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

Чтобы снизить уровень детализации сообщений журнала, можно изменить уровни журнала в *appsettings. Development.JSON* файл. Дополнительные сведения см. в разделе [введение к ведению журнала](xref:fundamentals/logging/index).

Используйте **обозреватель объектов SQL Server** для проверки базы данных. Обратите внимание на добавленную `__EFMigrationsHistory` таблицы. `__EFMigrationsHistory` Таблица отслеживает отслеживания объекта миграций, которые были применены к базе данных. Просмотр данных в `__EFMigrationsHistory` таблицу, она показана одна строка для первой миграции. Последний журнал в предыдущем примере вывода CLI показывает инструкции INSERT, которая создает эту строку.

Запустите приложение и убедитесь, что все работает.

## <a name="appling-migrations-in-production"></a>Применение миграции в рабочей среде

Мы рекомендуем производственных приложений следует **не** вызвать [Database.Migrate](https://docs.microsoft.com/dotnet/api/microsoft.entityframeworkcore.relationaldatabasefacadeextensions.migrate?view=efcore-2.0#Microsoft_EntityFrameworkCore_RelationalDatabaseFacadeExtensions_Migrate_Microsoft_EntityFrameworkCore_Infrastructure_DatabaseFacade_) при запуске приложения. `Migrate`не следует вызывать из приложения в ферме серверов. Например, если приложение было облака, развернутых с помощью масштабирования (запущено несколько экземпляров приложения).

Миграция базы данных должно выполняться как часть развертывания и контролируемым образом. Следующие подходы к переносу рабочей базы данных.

* Создание сценариев SQL и использование скриптов SQL в среде с помощью миграций.
* Под управлением `dotnet ef database update` из управляемой среды.

EF Core использует `__MigrationsHistory` таблицу для просмотра, если любой миграции необходимо выполнить. Если базы данных находится в актуальном состоянии, перенос не выполняется.

<a id="pmc"></a>
## <a name="command-line-interface-cli-vs-package-manager-console-pmc"></a>Интерфейс командной строки (CLI) vs. Консоль диспетчера пакетов (PMC)

EF основные средства для управления миграции можно найти:

* Команды .NET core CLI.
* Командлеты PowerShell в Visual Studio **консоль диспетчера пакетов** (PMC) окна.

Этот учебник показывает, как использовать CLI, некоторые разработчики предпочитают с помощью PMC.

EF основные команды для PMC находятся в [Microsoft.EntityFrameworkCore.Tools](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.Tools) пакета. Этот пакет включен в [Microsoft.AspNetCore.All](xref:fundamentals/metapackage) metapackage, поэтому устанавливать его не требуется.

**Важно:** это не тот же пакет, как установить для CLI, изменив *.csproj* файла. Имя данного объекта заканчивается на `Tools`, в отличие от имени пакета CLI, который заканчивается на `Tools.DotNet`.

Дополнительные сведения о командах CLI см. в разделе [.NET Core CLI](https://docs.microsoft.com/ef/core/miscellaneous/cli/dotnet).

Дополнительные сведения о командах PMC см. в разделе [Package Manager Console (Visual Studio)](https://docs.microsoft.com/ef/core/miscellaneous/cli/powershell).

## <a name="troubleshooting"></a>Устранение неполадок

Загрузить [завершенное приложение для данного этапа](
https://github.com/aspnet/Docs/tree/master/aspnetcore/data/ef-rp/intro/samples/StageSnapShots/cu-part4-migrations).

Приложение создает следующее исключение:

```text
`SqlException: Cannot open database "ContosoUniversity" requested by the login.
The login failed.
Login failed for user 'user name'.
```

Решение: запустите`dotnet ef database update`

Если `update` команда возвращает ошибку «Ошибка построения.»:

* Выполните команду еще раз.
* Оставьте сообщение в нижней части страницы.

>[!div class="step-by-step"]
[Назад](xref:data/ef-rp/sort-filter-page)
[Вперед](xref:data/ef-rp/complex-data-model)
