---
title: Razor Pages с EF Core в ASP.NET Core — миграции — 4 из 8
author: rick-anderson
description: В этом учебнике вы начинаете использовать функцию миграций EF Core для управления изменениями модели данных в приложении ASP.NET Core MVC.
ms.author: riande
ms.date: 6/31/2017
uid: data/ef-rp/migrations
ms.openlocfilehash: 406b94d74a99dcec3b2ce01a5eb3313c48819148
ms.sourcegitcommit: 3376f224b47a89acf329b2d2f9260046a372f924
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 05/10/2019
ms.locfileid: "65517005"
---
# <a name="razor-pages-with-ef-core-in-aspnet-core---migrations---4-of-8"></a>Razor Pages с EF Core в ASP.NET Core — миграции — 4 из 8

Авторы: [Том Дайкстра](https://github.com/tdykstra) (Tom Dykstra), [Йон П. Смит](https://twitter.com/thereformedprog) (Jon P Smith) и [Рик Андерсон](https://twitter.com/RickAndMSFT) (Rick Anderson)

[!INCLUDE [about the series](~/includes/RP-EF/intro.md)]

В этом учебнике используется функция миграций EF Core для управления изменениями модели данных.

При возникновении проблем, которые вам не удается устранить, скачайте [готовое приложение](
https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/data/ef-rp/intro/samples).

При разработке нового приложения модель данных часто изменяется. При каждом изменении нарушается синхронизация модели с базой данных. Вы начали работу с этим учебником, настроив платформу Entity Framework для создания базы данных, если она еще не существует. Каждый раз при изменении модели данных:

* База данных удаляется.
* Платформа EF создает новую базу данных, соответствующую модели.
* Приложение заполняет базу тестовыми данными.

Этот подход к обеспечению синхронизации базы данных с моделью данных хорошо работает до развертывания приложения в рабочей среде. Приложение, выполняющееся в рабочей среде, обычно содержит данные. Приложение не может запускаться с тестовой базой данных каждый раз при внесении изменений (например, при добавлении столбца). Функция миграций EF Core решает эту проблему, позволяя платформе EF Core обновить схему базы данных вместо создания новой базы.

Вместо удаления и повторного создания базы данных при изменении модели функция миграций обновляет схему, сохраняя существующие данные.

## <a name="drop-the-database"></a>Удаление базы данных

Воспользуйтесь **обозревателем объектов SQL Server** (SSOX) или командой `database drop`:

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

Выполните следующие команды в **консоли диспетчера пакетов** (PMC):

```PMC
Drop-Database
```

Чтобы просмотреть справочную информацию, выполните команду `Get-Help about_EntityFrameworkCore` в PMC.

# <a name="net-core-clitabnetcore-cli"></a>[Интерфейс командной строки .NET Core](#tab/netcore-cli)

Откройте командное окно и перейдите в папку проекта. Папка проекта содержит файл *Startup.cs*.

Введите в командном окне следующее:

 ```console
 dotnet ef database drop
 ```

---

## <a name="create-an-initial-migration-and-update-the-db"></a>Создание первоначальной миграции и обновление базы данных

Выполните построение проекта и создайте первую миграцию.

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

```PMC
Add-Migration InitialCreate
Update-Database
```

# <a name="net-core-clitabnetcore-cli"></a>[Интерфейс командной строки .NET Core](#tab/netcore-cli)

```console
dotnet ef migrations add InitialCreate
dotnet ef database update
```

---

### <a name="examine-the-up-and-down-methods"></a>Обзор методов Up и Down

Команда EF Core `migrations add` создала код для создания базы данных. Код миграции находится в файле *Migrations\<метка_времени>_InitialCreate.cs*. Метод `Up` класса `InitialCreate` создает таблицы базы данных, соответствующие наборам сущностей модели данных. Метод `Down` удаляет их, как показано в следующем примере:

[!code-csharp[](intro/samples/cu21/Migrations/20180626224812_InitialCreate.cs?range=7-24,77-88)]

Функция миграций вызывает метод `Up`, чтобы реализовать изменения модели данных для миграции. При вводе команды для отката обновления функция миграций вызывает метод `Down`.

Приведенный выше код предназначен для первоначальной миграции. Этот код был создан при выполнении команды `migrations add InitialCreate`. Параметр имени миграции (в примере это "InitialCreate") используется в качестве имени файла. В качестве имени миграции можно использовать любое допустимое имя файла. Рекомендуется выбрать слово или фразу, которые кратко описывают назначение миграции. Например, миграция, обеспечивающая добавление таблицы кафедр, может называться "AddDepartmentTable".

Если создается первоначальная миграция и база данных существует:

* Формируется код создания базы данных.
* Выполнять код создания базы данных не нужно, поскольку база уже соответствует модели данных. В случае выполнения код создания базы данных не вносит никаких изменений, поскольку база уже соответствует модели данных.

При развертывании приложения в новой среде этот код необходимо выполнить для создания новой базы данных.

Предыдущая база данных была удалена и не существует, поэтому функция миграций создает новую базу данных.

### <a name="the-data-model-snapshot"></a>Моментальный снимок модели данных

Функция миграций создает *моментальный снимок* текущей схемы базы данных в *Migrations/SchoolContextModelSnapshot.cs*. При добавлении миграции EF определяет, что именно изменилось, сравнивая модель данных с файлом моментального снимка.

Чтобы удалить миграцию, выполните следующую команду:

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

Remove-Migration

# <a name="net-core-clitabnetcore-cli"></a>[Интерфейс командной строки .NET Core](#tab/netcore-cli)

```console
dotnet ef migrations remove
```

Дополнительные сведения см. в разделе [dotnet ef migrations remove](/ef/core/miscellaneous/cli/dotnet#dotnet-ef-migrations-remove).

---

Команда remove migrations удаляет миграцию и гарантирует корректный сброс моментального снимка.

### <a name="remove-ensurecreated-and-test-the-app"></a>Удаление EnsureCreated и тестирование приложения

Для ранней разработки использовалась команда `EnsureCreated`. В этом учебнике используются миграции. `EnsureCreated` имеет следующие ограничения:

* Пропускает миграции и создает базу данных и схему.
* Не создает таблицу миграций.
* *Не может* использоваться с миграциями.
* Разработана для тестирования или быстрого создания прототипов, когда часто требуется удаление и повторное создание базы данных.

Удалите `EnsureCreated`:

```csharp
context.Database.EnsureCreated();
```

Запустите приложение и убедитесь, что база заполняется данными.

### <a name="inspect-the-database"></a>Проверка базы данных

Используйте **обозреватель объектов SQL Server** для проверки базы данных. Обратите внимание на добавление таблицы `__EFMigrationsHistory`. Таблица `__EFMigrationsHistory` используется для отслеживания миграций, которые были применены к базе данных. Просмотрев данные в таблице `__EFMigrationsHistory`, вы увидите одну строку для первой миграции. Последний журнал в предыдущем примере выходных данных интерфейса командной строки показывает инструкцию INSERT, создающую эту строку.

Запустите приложение и убедитесь, что все функции работают.

## <a name="applying-migrations-in-production"></a>Применение миграций в рабочей среде

Для рабочих приложений **не рекомендуется** вызывать [Database.Migrate](/dotnet/api/microsoft.entityframeworkcore.relationaldatabasefacadeextensions.migrate?view=efcore-2.0#Microsoft_EntityFrameworkCore_RelationalDatabaseFacadeExtensions_Migrate_Microsoft_EntityFrameworkCore_Infrastructure_DatabaseFacade_) при запуске приложения. `Migrate` не следует вызывать из приложения в ферме серверов. Например, если приложение было развернуто в облаке с горизонтальным масштабированием (выполняется несколько экземпляров приложения).

Миграция базы данных должна выполняться контролируемым способом в рамках развертывания. Подход к миграции рабочей базы данных включает следующее:

* Использование миграций для создания сценариев SQL и их использования в развертывании.
* Выполнение `dotnet ef database update` из контролируемой среды.

Платформа EF Core использует таблицу `__MigrationsHistory` для определения необходимости выполнить какие-либо миграции. Если база данных находится в актуальном состоянии, миграция не выполняется.

## <a name="troubleshooting"></a>Устранение неполадок

Скачайте [готовое приложение](
https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/data/ef-rp/intro/samples/StageSnapShots/cu-part4-migrations).

Приложение создает следующее исключение:

```text
SqlException: Cannot open database "ContosoUniversity" requested by the login.
The login failed.
Login failed for user 'user name'.
```

Решение: Запуск `dotnet ef database update`

### <a name="additional-resources"></a>Дополнительные ресурсы

* [Версия руководства на YouTube](https://www.youtube.com/watch?v=OWSUuMLKTJo)
* [Интерфейс командной строки .NET Core](/ef/core/miscellaneous/cli/dotnet).
* [Консоль диспетчера пакетов (Visual Studio)](/ef/core/miscellaneous/cli/powershell)



> [!div class="step-by-step"]
> [Назад](xref:data/ef-rp/sort-filter-page)
> [Вперед](xref:data/ef-rp/complex-data-model)
