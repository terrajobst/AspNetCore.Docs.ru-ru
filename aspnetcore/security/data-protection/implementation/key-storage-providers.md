---
title: Поставщики хранилища ключей в ASP.NET Core
author: rick-anderson
description: Дополнительные сведения о поставщиках хранилища ключей в ASP.NET Core и как настроить расположения хранилища ключей.
ms.author: riande
ms.date: 12/05/2019
uid: security/data-protection/implementation/key-storage-providers
ms.openlocfilehash: 219ebc471de32d15e4a43c938eef156c52e5f11e
ms.sourcegitcommit: 85564ee396c74c7651ac47dd45082f3f1803f7a2
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 02/12/2020
ms.locfileid: "77172581"
---
# <a name="key-storage-providers-in-aspnet-core"></a>Поставщики хранилища ключей в ASP.NET Core

Система защиты данных [по умолчанию использует механизм обнаружения,](xref:security/data-protection/configuration/default-settings) чтобы определить, где должны храниться криптографические ключи. Разработчик может переопределить механизм обнаружения по умолчанию и вручную указать расположение.

> [!WARNING]
> При указании опции явные постоянство ключа, система защиты данных отменяет регистрацию ключа шифрования по умолчанию механизм rest, поэтому ключи не шифруются при хранении. Рекомендуется дополнительно [указать явный механизм шифрования ключей](xref:security/data-protection/implementation/key-encryption-at-rest) для развертывания в рабочей среде.

## <a name="file-system"></a>Файловая система

Чтобы настроить репозиторий ключей на основе файловой системы, вызовите подпрограммы настройки [персисткэйстофилесистем](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionbuilderextensions.persistkeystofilesystem) , как показано ниже. Укажите [DirectoryInfo](/dotnet/api/system.io.directoryinfo) , указывающий на репозиторий, в котором должны храниться ключи:

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddDataProtection()
        .PersistKeysToFileSystem(new DirectoryInfo(@"c:\temp-keys\"));
}
```

`DirectoryInfo` может указывать на каталог на локальном компьютере или на папку в общей сетевой папке. Если указывает на каталог на локальном компьютере (и сценарий заключается в том, что доступ к этому репозиторию требуется получить только для приложений на локальном компьютере), рассмотрите возможность использования [Windows DPAPI](xref:security/data-protection/implementation/key-encryption-at-rest) (в Windows) для шифрования неактивных ключей. В противном случае рассмотрите возможность использования [сертификата X. 509](xref:security/data-protection/implementation/key-encryption-at-rest) для шифрования неактивных ключей.

## <a name="azure-storage"></a>Хранилище Azure

Пакет [Microsoft. AspNetCore. Data Protection. AzureStorage](https://www.nuget.org/packages/Microsoft.AspNetCore.DataProtection.AzureStorage/) позволяет хранить ключи защиты данных в хранилище BLOB-объектов Azure. Ключи могут совместно использоваться в нескольких экземплярах веб-приложения. Приложения могут совместно использовать файлы cookie проверки подлинности или CSRF защиты на нескольких серверах.

Чтобы настроить поставщик хранилища BLOB-объектов Azure, вызовите одну из перегрузок [персисткэйстоазуреблобстораже](/dotnet/api/microsoft.aspnetcore.dataprotection.azuredataprotectionbuilderextensions.persistkeystoazureblobstorage) .

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddDataProtection()
        .PersistKeysToAzureBlobStorage(new Uri("<blob URI including SAS token>"));
}
```

Если веб-приложение работает как служба Azure, токены проверки подлинности можно создать автоматически с помощью [Microsoft. Azure. Services. AppAuthentication](https://www.nuget.org/packages/Microsoft.Azure.Services.AppAuthentication/).

```csharp
var tokenProvider = new AzureServiceTokenProvider();
var token = await tokenProvider.GetAccessTokenAsync("https://storage.azure.com/");
var credentials = new StorageCredentials(new TokenCredential(token));
var storageAccount = new CloudStorageAccount(credentials, "mystorageaccount", "core.windows.net", useHttps: true);
var client = storageAccount.CreateCloudBlobClient();
var container = client.GetContainerReference("my-key-container");

// optional - provision the container automatically
await container.CreateIfNotExistsAsync();

services.AddDataProtection()
    .PersistKeysToAzureBlobStorage(container, "keys.xml");
```

См [. Дополнительные сведения о настройке проверки подлинности между службами.](/azure/key-vault/service-to-service-authentication)

## <a name="redis"></a>Redis

::: moniker range=">= aspnetcore-2.2"

Пакет [Microsoft. AspNetCore. Data Protection. стаккексчанжередис](https://www.nuget.org/packages/Microsoft.AspNetCore.DataProtection.StackExchangeRedis/) позволяет хранить ключи защиты данных в кэше Redis. Ключи могут совместно использоваться в нескольких экземплярах веб-приложения. Приложения могут совместно использовать файлы cookie проверки подлинности или CSRF защиты на нескольких серверах.

::: moniker-end

::: moniker range="< aspnetcore-2.2"

Пакет [Microsoft. AspNetCore. Data Protection. Redis](https://www.nuget.org/packages/Microsoft.AspNetCore.DataProtection.Redis/) позволяет хранить ключи защиты данных в кэше Redis. Ключи могут совместно использоваться в нескольких экземплярах веб-приложения. Приложения могут совместно использовать файлы cookie проверки подлинности или CSRF защиты на нескольких серверах.

::: moniker-end

::: moniker range=">= aspnetcore-2.2"

Чтобы настроить в Redis, вызовите одну из перегрузок [персисткэйстостаккексчанжередис](/dotnet/api/microsoft.aspnetcore.dataprotection.stackexchangeredisdataprotectionbuilderextensions.persistkeystostackexchangeredis) :

```csharp
public void ConfigureServices(IServiceCollection services)
{
    var redis = ConnectionMultiplexer.Connect("<URI>");
    services.AddDataProtection()
        .PersistKeysToStackExchangeRedis(redis, "DataProtection-Keys");
}
```

::: moniker-end

::: moniker range="< aspnetcore-2.2"

Чтобы настроить в Redis, вызовите одну из перегрузок [персисткэйсторедис](/dotnet/api/microsoft.aspnetcore.dataprotection.redisdataprotectionbuilderextensions.persistkeystoredis) :

```csharp
public void ConfigureServices(IServiceCollection services)
{
    var redis = ConnectionMultiplexer.Connect("<URI>");
    services.AddDataProtection()
        .PersistKeysToRedis(redis, "DataProtection-Keys");
}
```

::: moniker-end

Дополнительные сведения см. в следующих разделах:

* [StackExchange. Redis ConnectionMultiplexer](https://github.com/StackExchange/StackExchange.Redis/blob/master/docs/Basics.md)
* [кэш Azure Redis](/azure/redis-cache/cache-dotnet-how-to-use-azure-redis-cache#connect-to-the-cache)
* [ASP.NET Core примеры защиты](https://github.com/dotnet/AspNetCore/tree/2.2.0/src/DataProtection/samples)

## <a name="registry"></a>Реестр

**Применяется только к развертываниям Windows.**

Иногда приложение может не иметь доступ на запись в файловой системе. Рассмотрим ситуацию, когда приложение запускается как учетная запись виртуальной службы (например, удостоверение пула приложений *w3wp. exe*). В этом случае администратор может подготовить раздел реестра, доступную учетную запись службы. Вызовите метод расширения [персисткэйсторегистри](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionbuilderextensions.persistkeystoregistry) , как показано ниже. Укажите [RegistryKey](/dotnet/api/microsoft.aspnetcore.dataprotection.repositories.registryxmlrepository.registrykey) , указывающий на расположение, в котором должны храниться криптографические ключи:

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddDataProtection()
        .PersistKeysToRegistry(Registry.CurrentUser.OpenSubKey(@"SOFTWARE\Sample\keys"));
}
```

> [!IMPORTANT]
> Мы рекомендуем использовать [Windows DPAPI](xref:security/data-protection/implementation/key-encryption-at-rest) для шифрования неактивных ключей.

::: moniker range=">= aspnetcore-2.2"

## <a name="entity-framework-core"></a>Entity Framework Core

Пакет [Microsoft. AspNetCore. Data Protection. EntityFrameworkCore](https://www.nuget.org/packages/Microsoft.AspNetCore.DataProtection.EntityFrameworkCore/) предоставляет механизм для хранения ключей защиты данных в базе данных с помощью Entity Framework Core. `Microsoft.AspNetCore.DataProtection.EntityFrameworkCore` пакет NuGet необходимо добавить в файл проекта, но он не входит в состав [Microsoft. AspNetCore. app метапакет](xref:fundamentals/metapackage-app).

При использовании этого пакета ключи могут совместно использоваться несколькими экземплярами веб-приложения.

Чтобы настроить поставщик EF Core, вызовите метод [персисткэйстодбконтекст\<тконтекст >](/dotnet/api/microsoft.aspnetcore.dataprotection.entityframeworkcoredataprotectionextensions.persistkeystodbcontext) .

[!code-csharp[Main](key-storage-providers/sample/Startup.cs?name=snippet&highlight=13-20)]

Универсальный параметр, `TContext`, должен наследовать от [DbContext](/dotnet/api/microsoft.entityframeworkcore.dbcontext) и реализовывать [идатапротектионкэйконтекст](/dotnet/api/microsoft.aspnetcore.dataprotection.entityframeworkcore.idataprotectionkeycontext):

[!code-csharp[Main](key-storage-providers/sample/MyKeysContext.cs)]

Создайте таблицу `DataProtectionKeys`.

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

Выполните следующие команды в окне **консоли диспетчера пакетов** (PMC):

```powershell
Add-Migration AddDataProtectionKeys -Context MyKeysContext
Update-Database -Context MyKeysContext
```

# <a name="net-core-clitabnetcore-cli"></a>[Интерфейс командной строки .NET Core](#tab/netcore-cli)

Выполните следующие команды в командной оболочке:

```dotnetcli
dotnet ef migrations add AddDataProtectionKeys --context MyKeysContext
dotnet ef database update --context MyKeysContext
```

---

`MyKeysContext` — это `DbContext`, определенные в предыдущем примере кода. Если вы используете `DbContext` с другим именем, замените имя `DbContext` на `MyKeysContext`.

`DataProtectionKeys` класс или сущность принимает структуру, показанную в следующей таблице.

| Свойство/поле | Тип CLR | Тип SQL              |
| -------------- | -------- | --------------------- |
| `Id`           | `int`    | `int`, PK, NOT NULL   |
| `FriendlyName` | `string` | `nvarchar(MAX)`, null |
| `Xml`          | `string` | `nvarchar(MAX)`, null |

::: moniker-end

## <a name="custom-key-repository"></a>Пользовательский репозиторий ключа

Если встроенные механизмы не подходят, разработчик может указать собственный механизм сохранения ключей, предоставив пользовательский [иксмлрепоситори](/dotnet/api/microsoft.aspnetcore.dataprotection.repositories.ixmlrepository).
