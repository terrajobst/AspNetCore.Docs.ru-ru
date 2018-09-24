---
title: ASP.NET Core MVC с EF Core — миграции — 4 из 10
author: rick-anderson
description: В этом руководстве вы начинаете использовать функцию миграций EF Core для управления изменениями модели данных в приложении ASP.NET Core MVC.
ms.author: tdykstra
ms.date: 03/15/2018
uid: data/ef-mvc/migrations
ms.openlocfilehash: 556d7d4ad05679ebfce6c909b29610482bb3f350
ms.sourcegitcommit: b2723654af4969a24545f09ebe32004cb5e84a96
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 09/18/2018
ms.locfileid: "46011473"
---
# <a name="aspnet-core-mvc-with-ef-core---migrations---4-of-10"></a>ASP.NET Core MVC с EF Core — миграции — 4 из 10

[!INCLUDE [RP better than MVC](~/includes/RP-EF/rp-over-mvc-21.md)]

::: moniker range="= aspnetcore-2.0"

Авторы: [Том Дайкстра](https://github.com/tdykstra) (Tom Dykstra) и [Рик Андерсон](https://twitter.com/RickAndMSFT) (Rick Anderson)

На примере учебного веб-приложения "Университет Contoso" демонстрируется процесс создания веб-приложений ASP.NET Core MVC с помощью Entity Framework Core и Visual Studio. Сведения о серии руководств см. в [первом руководстве серии](intro.md).

В этом руководстве вы начинаете использовать функцию миграций EF Core для управления изменениями модели данных. В последующих руководствах вы добавите дополнительные миграции по мере изменения модели данных.

## <a name="introduction-to-migrations"></a>Введение в миграции

При разработке нового приложения ваша модель данных часто меняется, и при каждом таком изменении она нарушает синхронизацию с базой данных. Вы начали работу с этими руководствами, настроив Entity Framework для создания базы данных, если она еще не существует. Затем при каждом изменении модели данных — добавлении, удалении или изменении классов сущностей или изменении класса DbContext — вы можете удалить базу данных, после чего EF создает новую, которая соответствует данной модели, и заполняет ее тестовыми данными.

Этот способ для обеспечения синхронизации базы данных с моделью данных хорошо работает до развертывания приложения в рабочей среде. Когда приложение выполняется в рабочей среде, оно обычно хранит данные, которые вы хотите сохранить, и вам нежелательно терять все при каждом изменении, например при добавлении нового столбца. Функция миграций EF Core решает эту проблему, позволяя EF обновить схему базы данных вместо создания базы данных.

## <a name="entity-framework-core-nuget-packages-for-migrations"></a>Пакеты Entity Framework Core NuGet для миграций

Для работы с миграциями можно использовать **консоль диспетчера пакетов** (PMC) или интерфейс командной строки (CLI).  Эти руководства демонстрируют использование команд интерфейса командной строки. Дополнительные сведения о PMC [приведены в конце этого руководства](#pmc).

Средства EF для интерфейса командной строки (CLI) доступны в [Microsoft.EntityFrameworkCore.Tools.DotNet](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.Tools.DotNet). Чтобы установить этот пакет, добавьте его в коллекцию `DotNetCliToolReference` в файле *CSPROJ*, как показано ниже. **Примечание**. Необходимо установить этот пакет, отредактировав файл *.csproj*; использовать команду `install-package` или графический пользовательский интерфейс диспетчера пакетов нельзя. Вы можете изменить файл *CSPROJ*, щелкнув правой кнопкой мыши имя проекта в **обозревателе решений** и выбрав пункт **Изменить ContosoUniversity.csproj**.

[!code-xml[](intro/samples/cu/ContosoUniversity.csproj?range=12-15&highlight=2)]
  
(На момент написания руководства номера версий в этом примере были актуальными.)

## <a name="change-the-connection-string"></a>Изменение строки подключения

В файле *appsettings.json* измените имя базы данных в строке подключения на ContosoUniversity2 или другое имя, которое вы еще не использовали на компьютере, с которым работаете.

[!code-json[](intro/samples/cu/appsettings2.json?range=1-4)]

Это изменение настраивает проект, чтобы первая миграция создала базу данных. Это необязательно для начала работы с миграциями, но позднее вы поймете, почему так стоит сделать.

> [!NOTE]
> Вместо изменения имени базы данных можно удалить ее. Воспользуйтесь **обозревателем объектов SQL Server** (SSOX) или командой интерфейса командной строки `database drop`:
> ```console
> dotnet ef database drop
> ```
> Следующий раздел описывает выполнение команд интерфейса командной строки.

## <a name="create-an-initial-migration"></a>Создание первоначальной миграции

Сохраните изменения и выполните сборку проекта. После этого откройте командное окно и в папку проекта. Вот как это можно сделать быстро:

* Щелкните правой кнопкой мыши проект в **обозревателе решений** и выберите пункт **Открыть в проводнике**.

  ![Элемент меню "Открыть в проводнике"](migrations/_static/open-in-file-explorer.png)

* Введите "cmd" в адресной строке и нажмите клавишу ВВОД.

  ![Открытие командного окна](migrations/_static/open-command-window.png)

Введите в командном окне следующую команду:

```console
dotnet ef migrations add InitialCreate
```

В командном окне отображаются следующие выходные данные:

```console
info: Microsoft.AspNetCore.DataProtection.KeyManagement.XmlKeyManager[0]
      User profile is available. Using 'C:\Users\username\AppData\Local\ASP.NET\DataProtection-Keys' as key repository and Windows DPAPI to encrypt keys at rest.
info: Microsoft.EntityFrameworkCore.Infrastructure[100403]
      Entity Framework Core 2.0.0-rtm-26452 initialized 'SchoolContext' using provider 'Microsoft.EntityFrameworkCore.SqlServer' with options: None
Done. To undo this action, use 'ef migrations remove'
```

> [!NOTE]
> Если отображается сообщение об ошибке *No executable found matching command "dotnet-ef"* (Не найден исполняемый файл, соответствующий команде "dotnet-ef"), сведения об устранении проблемы см. в [этой записи блога](http://thedatafarm.com/data-access/no-executable-found-matching-command-dotnet-ef/).

Если отображается сообщение об ошибке "*Доступ к файлу... ContosoUniversity.dll невозможен, так как файл используется другим процессом*", найдите значок IIS Express в области уведомлений Windows, щелкните его правой кнопкой мыши, а затем выберите **ContosoUniversity > Остановить сайт**.

## <a name="examine-the-up-and-down-methods"></a>Обзор методов Up и Down

При выполнении команды `migrations add` система EF сформировала код, который создаст базу данных с нуля. Этот код находится в файле *\<метка_времени>_InitialCreate.cs* внутри папки *Migrations*. Метод `Up` класса `InitialCreate` создает таблицы базы данных, которые соответствуют наборам сущностей модели данных, а метод `Down` удаляет их, как показано в следующем примере.

[!code-csharp[](intro/samples/cu/Migrations/20170215220724_InitialCreate.cs?range=92-118)]

Функция миграций вызывает метод `Up`, чтобы реализовать изменения модели данных для миграции. При вводе команды для отката обновления функция миграций вызывает метод `Down`.

Этот пример кода предназначен для первоначальной миграции, созданной при вводе команды `migrations add InitialCreate`. Параметр имени миграции (в примере это "InitialCreate") используется в качестве имени файла и может быть любым. Рекомендуется выбрать слово или фразу, которые кратко описывают назначение миграции. Например, последнюю миграцию можно назвать "AddDepartmentTable".

Если вы создали первоначальную миграцию, когда база данных уже существовала, код для создания базы данных формируется, но выполнять его не требуется, так как база данных уже соответствует модели данных. Однако при развертывании приложения в другой среде, где база данных еще не существует, этот код будет выполняться для создания базы данных, поэтому рекомендуется сначала его протестировать. Вот почему ранее вы изменили имя базы данных в строке подключения — это позволяет функции миграций создать базу данных с нуля.

## <a name="the-data-model-snapshot"></a>Моментальный снимок модели данных

Функция миграций создает *моментальный снимок* текущей схемы базы данных в *Migrations/SchoolContextModelSnapshot.cs*. При добавлении миграции EF определяет, что именно изменилось, сравнивая модель данных с файлом моментального снимка.

При удалении миграции используйте команду [dotnet ef migrations remove](https://docs.microsoft.com/ef/core/miscellaneous/cli/dotnet#dotnet-ef-migrations-remove). `dotnet ef migrations remove` удаляет миграцию и гарантирует корректный сброс моментального снимка.

Дополнительные сведения об использовании файла моментального снимка см. в разделе [Миграции Core EF в средах групп](/ef/core/managing-schemas/migrations/teams).

## <a name="apply-the-migration-to-the-database"></a>Применение миграции к базе данных

Введите следующую команду в командном окне, чтобы создать базу данных и таблицы в ней.

```console
dotnet ef database update
```

Выходные данные команды аналогичны команде `migrations add`, за исключением того, что вы видите журналы для команд SQL, настраивающих базу данных. В приведенном ниже примере выходных данных большинство журналов опущено. Если вам не нужен такой уровень детализации сообщений журнала, можно изменить уровень ведения журнала в файле *appsettings.Development.json*. Дополнительные сведения см. в статье [Введение в ведение журналов](xref:fundamentals/logging/index).

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

Используйте **обозреватель объектов SQL Server** для проверки базы данных, как описано в первом руководстве.  Вы заметите добавление таблицы __EFMigrationsHistory, отслеживающей миграции, которые были применены к базе данных. Просмотрев данные в этой таблице, вы увидите одну строку для первой миграции. (Последний журнал в предыдущем примере выходных данных интерфейса командной строки показывает оператор INSERT, создающий эту строку.)

Запустите приложение, чтобы убедиться, что все работает, как и раньше.

![Страница указателя учащихся](migrations/_static/students-index.png)

<a id="pmc"></a>
## <a name="command-line-interface-cli-vs-package-manager-console-pmc"></a>Сравнение интерфейса командной строки (CLI) и консоли диспетчера пакетов (PMC)

Средства EF для управления миграциями доступны в виде команд интерфейса командной строки .NET Core или командлетов PowerShell в окне **консоли диспетчера пакетов** Visual Studio. Это руководство описывает использование интерфейса командной строки, но вы можете использовать и консоль диспетчера пакетов.

Команды EF для команд консоли диспетчера пакетов находятся в пакете [Microsoft.EntityFrameworkCore.Tools](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.Tools). Этот пакет уже входит в состав метапакета [Microsoft.AspNetCore.All](xref:fundamentals/metapackage), поэтому устанавливать его не требуется.

**Важно!** Это не тот же пакет, который вы устанавливаете для интерфейса командной строки, изменив файл *CSPROJ*. Его имя заканчивается на `Tools`, а имя пакета интерфейса командной строки — на `Tools.DotNet`.

Дополнительные сведения о командах интерфейса командной строки см. в разделе [.NET Core CLI](https://docs.microsoft.com/ef/core/miscellaneous/cli/dotnet).

Дополнительные сведения о командах консоли диспетчера пакетов см. в разделе [Консоль диспетчера пакетов (Visual Studio)](https://docs.microsoft.com/ef/core/miscellaneous/cli/powershell).

## <a name="summary"></a>Сводка

В этом руководстве вы узнали, как создать и применить первую миграцию. В следующем руководстве вы начнете рассматривать более сложные вопросы, развернув модель данных. Попутно вы создадите и примените дополнительные миграции.

::: moniker-end

> [!div class="step-by-step"]
> [Назад](sort-filter-page.md)
> [Вперед](complex-data-model.md)
