---
title: Поставщики хранилища ключей в ASP.NET Core
author: rick-anderson
description: Дополнительные сведения о поставщиках хранилища ключей в ASP.NET Core и как настроить расположения хранилища ключей.
ms.author: riande
ms.date: 12/05/2019
uid: security/data-protection/implementation/key-storage-providers
ms.openlocfilehash: c1d2ac1304230af88e63e1aca441f044b32038fd
ms.sourcegitcommit: 7dfe6cc8408ac6a4549c29ca57b0c67ec4baa8de
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/09/2020
ms.locfileid: "75829092"
---
# <a name="key-storage-providers-in-aspnet-core"></a>Поставщики хранилища ключей в ASP.NET Core

Система защиты данных [использует механизм обнаружения по умолчанию](xref:security/data-protection/configuration/default-settings) для определения, где должны сохраняться криптографических ключей. Разработчик может переопределить механизм обнаружения по умолчанию и вручную указать расположение.

> [!WARNING]
> При указании опции явные постоянство ключа, система защиты данных отменяет регистрацию ключа шифрования по умолчанию механизм rest, поэтому ключи не шифруются при хранении. Рекомендуется, вы Кроме того [укажите механизм явного ключа шифрования](xref:security/data-protection/implementation/key-encryption-at-rest) для развертывания в рабочей среде.

## <a name="file-system"></a>Файловая система

Чтобы настроить файл на основе системы ключа репозиторием, вызовите [PersistKeysToFileSystem](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionbuilderextensions.persistkeystofilesystem) конфигурации подпрограммы, как показано ниже. Укажите [DirectoryInfo](/dotnet/api/system.io.directoryinfo) указанием на репозиторий, где должны храниться ключи:

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddDataProtection()
        .PersistKeysToFileSystem(new DirectoryInfo(@"c:\temp-keys\"));
}
```

`DirectoryInfo` Можно указать папку на локальном компьютере, или он может указывать на папку в общей сетевой папке. Если указывает на каталог на локальном компьютере (и в случае, является то, что только приложения на локальном компьютере требуется доступ, чтобы использовать этот репозиторий), рассмотрите возможность использования [Windows DPAPI](xref:security/data-protection/implementation/key-encryption-at-rest) (в Windows) для шифрования ключей при хранении. В противном случае рассмотрите возможность использования [сертификат X.509](xref:security/data-protection/implementation/key-encryption-at-rest) для шифрования ключей при хранении.

## <a name="azure-storage"></a>Служба хранилища Azure

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

Чтобы настроить на Redis, вызовите один из [PersistKeysToStackExchangeRedis](/dotnet/api/microsoft.aspnetcore.dataprotection.stackexchangeredisdataprotectionbuilderextensions.persistkeystostackexchangeredis) перегрузки:

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

Чтобы настроить на Redis, вызовите один из [PersistKeysToRedis](/dotnet/api/microsoft.aspnetcore.dataprotection.redisdataprotectionbuilderextensions.persistkeystoredis) перегрузки:

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

* [StackExchange.Redis ConnectionMultiplexer](https://github.com/StackExchange/StackExchange.Redis/blob/master/docs/Basics.md)
* [Кэш Redis для Azure](/azure/redis-cache/cache-dotnet-how-to-use-azure-redis-cache#connect-to-the-cache)
* [ASP.NET Core примеры защиты](https://github.com/dotnet/AspNetCore/tree/2.2.0/src/DataProtection/samples)

## <a name="registry"></a>Реестр

**Применяется только для развертывания Windows.**

Иногда приложение может не иметь доступ на запись в файловой системе. Рассмотрим ситуацию, где приложение выполняется как учетная запись виртуальной службы (такие как *w3wp.exe*удостоверение пула приложений). В этом случае администратор может подготовить раздел реестра, доступную учетную запись службы. Вызовите [PersistKeysToRegistry](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionbuilderextensions.persistkeystoregistry) метод расширения, как показано ниже. Укажите [RegistryKey](/dotnet/api/microsoft.aspnetcore.dataprotection.repositories.registryxmlrepository.registrykey) указывает на место хранения криптографических ключей:

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddDataProtection()
        .PersistKeysToRegistry(Registry.CurrentUser.OpenSubKey(@"SOFTWARE\Sample\keys"));
}
```

> [!IMPORTANT]
> Мы рекомендуем использовать [Windows DPAPI](xref:security/data-protection/implementation/key-encryption-at-rest) для шифрования ключей при хранении.

::: moniker range=">= aspnetcore-2.2"

## <a name="entity-framework-core"></a>Entity Framework Core

[Microsoft.AspNetCore.DataProtection.EntityFrameworkCore](https://www.nuget.org/packages/Microsoft.AspNetCore.DataProtection.EntityFrameworkCore/) пакет предоставляет механизм для хранения ключей защиты данных в базу данных с помощью Entity Framework Core. `Microsoft.AspNetCore.DataProtection.EntityFrameworkCore` Пакет NuGet должны добавляться в файл проекта не является частью [метапакет Microsoft.AspNetCore.App](xref:fundamentals/metapackage-app).

При использовании этого пакета ключи могут совместно использоваться несколькими экземплярами веб-приложения.

Чтобы настроить поставщик EF Core, вызовите метод [персисткэйстодбконтекст\<тконтекст >](/dotnet/api/microsoft.aspnetcore.dataprotection.entityframeworkcoredataprotectionextensions.persistkeystodbcontext) .

[!code-csharp[Main](key-storage-providers/sample/Startup.cs?name=snippet&highlight=13-20)]

Универсальный параметр, `TContext`, должен наследовать от [DbContext](/dotnet/api/microsoft.entityframeworkcore.dbcontext) и реализовывать [идатапротектионкэйконтекст](/dotnet/api/microsoft.aspnetcore.dataprotection.entityframeworkcore.idataprotectionkeycontext):

[!code-csharp[Main](key-storage-providers/sample/MyKeysContext.cs)]

Создайте таблицу `DataProtectionKeys`.

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

Выполните следующие команды в окне **консоли диспетчера пакетов** (PMC):

```PowerShell
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

Если механизмы в поле не подходит, разработчик может указать собственный механизм сохраняемости ключа, предоставляя пользовательский [IXmlRepository](/dotnet/api/microsoft.aspnetcore.dataprotection.repositories.ixmlrepository).
