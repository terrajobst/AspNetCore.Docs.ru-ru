---
title: Razor Pages с EF Core в ASP.NET Core — миграции — 4 из 8
author: rick-anderson
description: В этом учебнике вы начинаете использовать функцию миграций EF Core для управления изменениями модели данных в приложении ASP.NET Core MVC.
manager: wpickett
ms.author: riande
ms.date: 10/15/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: get-started-article
uid: data/ef-rp/migrations
ms.openlocfilehash: 690beaabeab098cf9b764730b1bf1bd04bf6b003
ms.sourcegitcommit: 5130b3034165f5cf49d829fe7475a84aa33d2693
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 05/03/2018
---
# <a name="razor-pages-with-ef-core-in-aspnet-core---migrations---4-of-8"></a>Razor Pages с EF Core в ASP.NET Core — миграции — 4 из 8

Авторы: [Том Дайкстра](https://github.com/tdykstra) (Tom Dykstra), [Йон П. Смит](https://twitter.com/thereformedprog) (Jon P Smith) и [Рик Андерсон](https://twitter.com/RickAndMSFT) (Rick Anderson)

[!INCLUDE [about the series](../../includes/RP-EF/intro.md)]

В этом учебнике используется функция миграций EF Core для управления изменениями модели данных.

При возникновении проблем, которые вам не удается устранить, скачайте [готовое приложение для этого этапа](
https://github.com/aspnet/Docs/tree/master/aspnetcore/data/ef-rp/intro/samples/StageSnapShots/cu-part4-migrations).

При разработке нового приложения модель данных часто изменяется. При каждом изменении нарушается синхронизация модели с базой данных. Вы начали работу с этим учебником, настроив платформу Entity Framework для создания базы данных, если она еще не существует. Каждый раз при изменении модели данных:

* База данных удаляется.
* Платформа EF создает новую базу данных, соответствующую модели.
* Приложение заполняет базу тестовыми данными.

Этот подход к обеспечению синхронизации базы данных с моделью данных хорошо работает до развертывания приложения в рабочей среде. Приложение, выполняющееся в рабочей среде, обычно содержит данные. Приложение не может запускаться с тестовой базой данных каждый раз при внесении изменений (например, при добавлении столбца). Функция миграций EF Core решает эту проблему, позволяя платформе EF Core обновить схему базы данных вместо создания новой базы.

Вместо удаления и повторного создания базы данных при изменении модели функция миграций обновляет схему, сохраняя существующие данные.

## <a name="entity-framework-core-nuget-packages-for-migrations"></a>Пакеты Entity Framework Core NuGet для миграций

Для работы с миграциями можно использовать **консоль диспетчера пакетов** (PMC) или интерфейс командной строки (CLI). Эти руководства демонстрируют использование команд интерфейса командной строки. Дополнительные сведения о PMC [приведены в конце этого руководства](#pmc).

Средства EF Core для интерфейса командной строки (CLI) доступны в [Microsoft.EntityFrameworkCore.Tools.DotNet](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.Tools.DotNet). Чтобы установить этот пакет, добавьте его в коллекцию `DotNetCliToolReference` в файле *CSPROJ*, как показано ниже. **Примечание.** Для установки этого пакета необходимо отредактировать файл *CSPROJ*. Для установки этого пакета нельзя использовать команду `install-package` или графический пользовательский интерфейс диспетчера пакетов. Измените файл *CSPROJ*, щелкнув правой кнопкой мыши имя проекта в **обозревателе решений** и выбрав пункт **Изменить ContosoUniversity.csproj**.

В следующем примере разметки показан обновленный файл *CSPROJ* с выделенными средствами интерфейса командной строки EF Core:

[!code-xml[](intro/samples/cu/ContosoUniversity.csproj?highlight=12)]
  
На момент написания руководства номера версий в предыдущем примере были актуальными. Используйте версию средств интерфейса командной строки EF Core, которая находится в других пакетах.

## <a name="change-the-connection-string"></a>Изменение строки подключения

В файле *appsettings.json* измените имя базы данных в строке подключения на ContosoUniversity2.

[!code-json[](intro/samples/cu/appsettings2.json?range=1-4)]

Изменение имени базы данных в строке подключения приводит к выполнению первой миграции для создания новой базы данных. Новая база данных создается в связи с тем, что база с указанным именем не существует. Для начала работы с миграциями изменять строку подключения необязательно.

Вместо изменения имени базы данных можно удалить ее. Воспользуйтесь **обозревателем объектов SQL Server** (SSOX) или командой интерфейса командной строки `database drop`:

 ```console
 dotnet ef database drop
 ```

Следующий раздел описывает выполнение команд интерфейса командной строки.

## <a name="create-an-initial-migration"></a>Создание первоначальной миграции

Выполните построение проекта.

Откройте командное окно и перейдите в папку проекта. Папка проекта содержит файл *Startup.cs*.

Введите в командном окне следующее:

```console
dotnet ef migrations add InitialCreate
```

В командном окне отображается информация следующего вида:

```console
info: Microsoft.AspNetCore.DataProtection.KeyManagement.XmlKeyManager[0]
      User profile is available. Using 'C:\Users\username\AppData\Local\ASP.NET\DataProtection-Keys' as key repository and Windows DPAPI to encrypt keys at rest.
info: Microsoft.EntityFrameworkCore.Infrastructure[100403]
      Entity Framework Core 2.0.0-rtm-26452 initialized 'SchoolContext' using provider 'Microsoft.EntityFrameworkCore.SqlServer' with options: None
Done. To undo this action, use 'ef migrations remove'
```

Если миграция завершается сбоем с сообщением "*Нет доступа к файлу... ContosoUniversity.dll, так как он используется другим процессом.*" на экране:

* Остановите IIS Express.

   * Выйдите из среды Visual Studio и перезапустите ее, либо
   * Найдите значок IIS Express в панели задач Windows.
   * Щелкните значок IIS Express правой кнопкой мыши и выберите **ContosoUniversity > Остановить сайт**.

Если отображается сообщение об ошибке "Ошибка сборки", выполните команду еще раз. Если вы получаете такую ошибку, оставьте комментарий в конце этого учебника.

### <a name="examine-the-up-and-down-methods"></a>Обзор методов Up и Down

Команда EF Core `migrations add` создала код для создания базы данных. Код миграции находится в файле *Migrations\<метка_времени>_InitialCreate.cs*. Метод `Up` класса `InitialCreate` создает таблицы базы данных, соответствующие наборам сущностей модели данных. Метод `Down` удаляет их, как показано в следующем примере:

[!code-csharp[](intro/samples/cu/Migrations/20171026010210_InitialCreate.cs?range=8-24,77-)]

Функция миграций вызывает метод `Up`, чтобы реализовать изменения модели данных для миграции. При вводе команды для отката обновления функция миграций вызывает метод `Down`.

Приведенный выше код предназначен для первоначальной миграции. Этот код был создан при выполнении команды `migrations add InitialCreate`. Параметр имени миграции (в примере это "InitialCreate") используется в качестве имени файла. В качестве имени миграции можно использовать любое допустимое имя файла. Рекомендуется выбрать слово или фразу, которые кратко описывают назначение миграции. Например, миграция, обеспечивающая добавление таблицы кафедр, может называться "AddDepartmentTable".

Если создается первоначальная миграция и база данных существует:

* Формируется код создания базы данных.
* Выполнять код создания базы данных не нужно, поскольку база уже соответствует модели данных. В случае выполнения код создания базы данных не вносит никаких изменений, поскольку база уже соответствует модели данных.

При развертывании приложения в новой среде этот код необходимо выполнить для создания новой базы данных.

Ранее была изменена строка подключения для использования нового имени базы данных. Поскольку указанная база данных не существует, миграция создает ее.

### <a name="the-data-model-snapshot"></a>Моментальный снимок модели данных

Функция миграций создает *моментальный снимок* текущей схемы базы данных в *Migrations/SchoolContextModelSnapshot.cs*. При добавлении миграции EF определяет, что именно изменилось, сравнивая модель данных с файлом моментального снимка.

При удалении миграции используйте команду [dotnet ef migrations remove](https://docs.microsoft.com/ef/core/miscellaneous/cli/dotnet#dotnet-ef-migrations-remove). `dotnet ef migrations remove` удаляет миграцию и гарантирует корректный сброс моментального снимка.

Дополнительные сведения об использовании файла моментального снимка см. в разделе [Миграции Core EF в средах групп](/ef/core/managing-schemas/migrations/teams).

## <a name="remove-ensurecreated"></a>Удаление EnsureCreated

Для ранней разработки использовалась команда `EnsureCreated`. В этом учебнике используются миграции. `EnsureCreated` имеет следующие ограничения:

* Пропускает миграции и создает базу данных и схему.
* Не создает таблицу миграций.
* *Не может* использоваться с миграциями.
* Разработана для тестирования или быстрого создания прототипов, когда часто требуется удаление и повторное создание базы данных.

Удалите следующую строку из `DbInitializer`:

```csharp
context.Database.EnsureCreated();
```

## <a name="apply-the-migration-to-the-db-in-development"></a>Применение миграции к базе данных при разработке

Введите следующую команду в командном окне, чтобы создать базу данных и таблицы.

```console
dotnet ef database update
```

Примечание. Если команда `update` возвращает ошибку "Ошибка сборки",

* выполните команду еще раз.
* Если ошибка повторится, выйдите из среды Visual Studio и затем выполните команду `update`.
* Оставьте сообщение внизу страницы.

Выходные данные этой команды аналогичны команде `migrations add`. Для предыдущей команды отображаются журналы команд SQL, которые настроены для базы данных. В приведенном ниже примере выходных данных большинство журналов опущено:

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

Чтобы снизить уровень детализации сообщений журнала, можно изменить уровень ведения журнала в файле *appsettings.Development.json*. Дополнительные сведения см. в статье [Введение в ведение журналов](xref:fundamentals/logging/index).

Используйте **обозреватель объектов SQL Server** для проверки базы данных. Обратите внимание на добавление таблицы `__EFMigrationsHistory`. Таблица `__EFMigrationsHistory` используется для отслеживания миграций, которые были применены к базе данных. Просмотрев данные в таблице `__EFMigrationsHistory`, вы увидите одну строку для первой миграции. Последний журнал в предыдущем примере выходных данных интерфейса командной строки показывает инструкцию INSERT, создающую эту строку.

Запустите приложение и убедитесь, что все функции работают.

## <a name="applying-migrations-in-production"></a>Применение миграций в рабочей среде

Для рабочих приложений **не рекомендуется** вызывать [Database.Migrate](/dotnet/api/microsoft.entityframeworkcore.relationaldatabasefacadeextensions.migrate?view=efcore-2.0#Microsoft_EntityFrameworkCore_RelationalDatabaseFacadeExtensions_Migrate_Microsoft_EntityFrameworkCore_Infrastructure_DatabaseFacade_) при запуске приложения. `Migrate` не следует вызывать из приложения в ферме серверов. Например, если приложение было развернуто в облаке с горизонтальным масштабированием (выполняется несколько экземпляров приложения).

Миграция базы данных должна выполняться контролируемым способом в рамках развертывания. Подход к миграции рабочей базы данных включает следующее:

* Использование миграций для создания сценариев SQL и их использования в развертывании.
* Выполнение `dotnet ef database update` из контролируемой среды.

Платформа EF Core использует таблицу `__MigrationsHistory` для определения необходимости выполнить какие-либо миграции. Если база данных находится в актуальном состоянии, миграция не выполняется.

<a id="pmc"></a>
## <a name="command-line-interface-cli-vs-package-manager-console-pmc"></a>Сравнение интерфейса командной строки (CLI) и консоли диспетчера пакетов (PMC)

Средства EF Core для управления миграциями предоставляются в следующих источниках:

* Команды интерфейса командной строки .NET Core.
* Командлеты PowerShell в окне **консоли диспетчера пакетов** (PMC) Visual Studio.

В этом учебнике демонстрируется использование интерфейса командной строки, однако некоторые разработчики предпочитают использовать консоль диспетчера пакетов.

Команды EF Core для команд консоли диспетчера пакетов находятся в пакете [Microsoft.EntityFrameworkCore.Tools](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.Tools). Этот пакет уже входит в состав метапакета [Microsoft.AspNetCore.All](xref:fundamentals/metapackage), поэтому устанавливать его не требуется.

**Важно!** Это не тот же пакет, который вы устанавливаете для интерфейса командной строки, изменив файл *CSPROJ*. Его имя заканчивается на `Tools`, а имя пакета интерфейса командной строки — на `Tools.DotNet`.

Дополнительные сведения о командах интерфейса командной строки см. в разделе [.NET Core CLI](https://docs.microsoft.com/ef/core/miscellaneous/cli/dotnet).

Дополнительные сведения о командах консоли диспетчера пакетов см. в разделе [Консоль диспетчера пакетов (Visual Studio)](https://docs.microsoft.com/ef/core/miscellaneous/cli/powershell).

## <a name="troubleshooting"></a>Устранение неполадок

Скачайте [готовое приложение для этого этапа](
https://github.com/aspnet/Docs/tree/master/aspnetcore/data/ef-rp/intro/samples/StageSnapShots/cu-part4-migrations).

Приложение создает следующее исключение:

```text
SqlException: Cannot open database "ContosoUniversity" requested by the login.
The login failed.
Login failed for user 'user name'.
```

Решение. Выполните `dotnet ef database update`

Если команда `update` возвращает ошибку "Ошибка сборки",

* выполните команду еще раз.
* Оставьте сообщение внизу страницы.

> [!div class="step-by-step"]
> [Назад](xref:data/ef-rp/sort-filter-page)
> [Вперед](xref:data/ef-rp/complex-data-model)
