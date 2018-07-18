---
title: Работа с распределенным кэшем в ASP.NET Core
author: ardalis
description: Узнайте, как использовать ASP.NET Core распределенного кэширования для повышения производительности приложения и масштабируемости, особенно в среде фермы облаком и сервером.
ms.author: riande
ms.custom: mvc
ms.date: 02/14/2017
uid: performance/caching/distributed
ms.openlocfilehash: 861664fcad576c11abe052837b72367eb2b9479a
ms.sourcegitcommit: 3ca527f27c88cfc9d04688db5499e372fbc2c775
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 07/17/2018
ms.locfileid: "39095685"
---
# <a name="work-with-a-distributed-cache-in-aspnet-core"></a>Работа с распределенным кэшем в ASP.NET Core

Автор: [Стив Смит](https://ardalis.com/) (Steve Smith)

Распределенные кэши могут улучшить производительность и масштабируемость приложений ASP.NET Core, особенно в том случае, если размещенные в облаке или ферме серверов.

[Просмотреть или скачать образец кода](https://github.com/aspnet/Docs/tree/master/aspnetcore/performance/caching/distributed/sample) ([как скачивать](xref:tutorials/index#how-to-download-a-sample))

## <a name="what-is-a-distributed-cache"></a>Что такое распределенный кэш

Распределенный кэш является общим для нескольких серверов приложений (см. [основы кэша](memory.md#caching-basics)).  Сведения в кэше не хранятся в памяти отдельных веб-серверов и кэшированных данных доступен для всех серверов приложений. Это обеспечивает ряд преимуществ:

1. Кэшированные данные согласованны на всех веб-серверах. Пользователи получают одинаковые результаты вне зависимости от того, какой веб-сервер обрабатывает их запрос.

2. Кэшированные данные сохраняются при перезагрузке и развертывании веб-сервера. Веб-серверы можно удалять или добавлять без воздействия на кэш.

3. Исходное хранилище данных получает меньшее число запросов (по сравнению с несколькими кэшами в памяти или отсутствием кэша вообще).

> [!NOTE]
> Если используется распределенный кэш SQL Server, некоторые из этих преимуществ верны только в том случае, если для кеша используется отдельный экземпляр базы данных, отличный от того, который используется для исходных данных приложения.

Как и любой кэш, распределенный кэш может значительно повысить быстродействие приложения, поскольку обычно данные из кэша можно извлечь намного быстрее, чем из реляционной базы данных (или веб-службы).

Конфигурация кэша зависит от конкретной реализации. В этой статье описывается, как настроить распределенные кэши для Redis и SQL Server. Независимо от того, какая реализация выбрана, приложение взаимодействует с кэшем через общий интерфейс `IDistributedCache`.

## <a name="the-idistributedcache-interface"></a>Интерфейс IDistributedCache

Интерфейс `IDistributedCache` включает синхронные и асинхронные методы. Он позволяет добавлять, получать и удалять элементы из реализации распределенного кэша. Интерфейс `IDistributedCache` содержит следующие методы:

**Get, GetAsync**

Принимает строковый ключ и извлекает кэшированный элемент в виде `byte[]`, если он найден в кэше.

**Set, SetAsync**

Добавляет элемент (как `byte[]`) в кэш с использованием строкового ключа.

**Refresh, RefreshAsync**

Обновляет элемент в кеше на основе его ключа, сбросив время ожидания его переменного срока действия (если есть).

**Remove, RemoveAsync**

Удаляет запись в кэше по его ключу.

Для использования интерфейса `IDistributedCache`:

   1. Добавьте необходимые пакеты NuGet в файл проекта.

   2. Настройте конкретную реализацию `IDistributedCache` в методе`ConfigureServices` класса `Startup` и добавьте ее в контейнер.

   3. Из классов [ромежуточного уровня](xref:fundamentals/middleware/index) или MVC-контроллера приложения запросите экземпляр `IDistributedCache` в конструкторе.  Экземпляр будет предоставлен через [внедрение зависимостей](../../fundamentals/dependency-injection.md) (DI).

> [!NOTE]
> Время жизни экземпляров `IDistributedCache` (по крайней мере, для встроенных реализаций) не обязательно должно быть ограничено одним объектом или блоком. Вы можете создавать экземпляр где угодно, где это необходимо (вместо использования [внедрения зависимостей](../../fundamentals/dependency-injection.md)), но это может сделать ваш код более сложным для тестирования и нарушает [принцип явных зависимостей](http://deviq.com/explicit-dependencies-principle/).

В следующем примере показано, как использовать экземпляр `IDistributedCache` в простом компоненте промежуточного уровня:

[!code-csharp[](distributed/sample/src/DistCacheSample/StartTimeHeader.cs)]

В приведенном выше коде кэшированное значение считывается, но никогда не записывается. В этом примере значение устанавливается только при запуске сервера и не изменяется. В сценарии с множеством серверов самый последний запущенный сервер перезапишет все предыдущие значения, установленные другими серверами. Методы `Get` и `Set` используют тип `byte[]`. Поэтому строковое значение необходимо преобразовывать с помощью `Encoding.UTF8.GetString` (для `Get`) и `Encoding.UTF8.GetBytes` (для `Set`)

В следующем примере кода из *Startup.cs* показывается, как задать значение:

[!code-csharp[](distributed/sample/src/DistCacheSample/Startup.cs?name=snippet1)]

> [!NOTE]
> Поскольку `IDistributedCache` настраивается в методе`ConfigureServices`, он доступен для метода `Configure` в качестве параметра. Добавление его в качестве параметра позволит получить настроенный экземпляр через DI.

## <a name="using-a-redis-distributed-cache"></a>Использование распределенного кеша Redis

[Redis](https://redis.io/) -это хранилище данных в памяти с открытым исходным кодом, которое часто используется в качестве распределенного кэша. Вы можете использовать его локально, и вы можете настроить [кэш Azure Redis](https://azure.microsoft.com/services/cache/) для приложений ASP.NET Core, размещенных в Azure.  Приложение ASP.NET Core настраивает реализации кэша с помощью `RedisDistributedCache` экземпляра.

Настройте реализацию Redis в `ConfigureServices` и получите доступ к нему в коде приложения, запросив экземпляр `IDistributedCache` (см. код выше).

В примере кода реализация `RedisCache` используется в том случае, если сервер настроен для среды `Staging`. Таким образом, метод `ConfigureStagingServices` настраивает `RedisCache`:

[!code-csharp[](distributed/sample/src/DistCacheSample/Startup.cs?name=snippet2)]

> [!NOTE]
> Чтобы установить Redis на локальном компьютере, необходимо установить пакет chocolatey [https://chocolatey.org/packages/redis-64/](https://chocolatey.org/packages/redis-64/) и запустить `redis-server` из командной строки.

## <a name="using-a-sql-server-distributed-cache"></a>Использование распределенного кеша SQL Server

Реализация SqlServerCache позволяет распределенному кэшу использовать базу данных SQL Server в качестве хранилища. Чтобы создать таблицу SQL Server, вы можете использовать инструмент sql-cache, который создает таблицу с указанным именем и схемой.

::: moniker range="< aspnetcore-2.1"

Добавить `SqlConfig.Tools` для `<ItemGroup>` элемент файла проекта и выполните `dotnet restore`.

```xml
<ItemGroup>
  <DotNetCliToolReference Include="Microsoft.Extensions.Caching.SqlConfig.Tools" 
                          Version="2.0.2" />
</ItemGroup>
```

::: moniker-end

Проверьте SqlConfig.Tools, выполнив следующую команду:

```console
dotnet sql-cache create --help
```

SqlConfig.Tools отображает использование, параметры и справку по команде.

Создайте таблицу в SQL Server, выполнив команду `sql-cache create`:

```console
dotnet sql-cache create "Data Source=(localdb)\v11.0;Initial Catalog=DistCache;Integrated Security=True;" dbo TestCache
info: Microsoft.Extensions.Caching.SqlConfig.Tools.Program[0]
Table and index were created successfully.
```

Созданная таблица имеет следующую схему:

![Таблицы кэша SqlServer](distributed/_static/SqlServerCacheTable.png)

Как и все реализации кэша, ваше приложение должно получать и задавать значения кэша, используя экземпляр `IDistributedCache`, а не `SqlServerCache`. Этот пример реализует `SqlServerCache` в рабочей среде (с настройкой в `ConfigureProductionServices`).

[!code-csharp[](distributed/sample/src/DistCacheSample/Startup.cs?name=snippet3)]

> [!NOTE]
> `ConnectionString` (и, при необходимости, `SchemaName` и `TableName`) обычно должны храниться вне системы управления версиями (например, UserSecrets), так как они могут содержать учетные данные.

## <a name="recommendations"></a>Рекомендации

Когда вы решаете, какая реализация `IDistributedCache` подходит для вашего приложения, выбирайте между Redis и SQL Server на основе вашей существующей инфраструктуры и среды, ваших требований к производительности и опыта вашей команды. Если вашей команде комфортней работать с Redis, это отличный выбор. Если ваша команда предпочитает SQL Server, вы можете быть уверены и в этой реализации. Обратите внимание, что традиционные решения кэширования хранят данные в памяти, что позволяет быстро их извлекать. Храните часто используемые данные в кэше и все данные — в постоянном хранилище, например SQL Server или Azure Storage.  Redis Cache — это решение кэширования, которое обеспечивает более высокую пропускную способность и низкую задержку

## <a name="additional-resources"></a>Дополнительные ресурсы

* [Кэш в Azure redis](https://azure.microsoft.com/documentation/services/redis-cache/)
* [База данных SQL в Azure](https://azure.microsoft.com/documentation/services/sql-database/)
* <xref:performance/caching/memory>
* <xref:fundamentals/primitives/change-tokens>
* <xref:performance/caching/response>
* <xref:performance/caching/middleware>
* <xref:mvc/views/tag-helpers/builtin-th/cache-tag-helper>
* <xref:mvc/views/tag-helpers/builtin-th/distributed-cache-tag-helper>
* <xref:host-and-deploy/web-farm>
