---
title: Поставщики хранилища ключей в ASP.NET Core
author: rick-anderson
description: Дополнительные сведения о поставщиках хранилища ключей в ASP.NET Core и как настроить расположения хранилища ключей.
ms.author: riande
ms.date: 07/16/2018
uid: security/data-protection/implementation/key-storage-providers
ms.openlocfilehash: 0e64a65ab1d65efa9f2e4d36a23663b607f206d7
ms.sourcegitcommit: 9bdba90b2c97a4016188434657194b2d7027d6e3
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 09/27/2018
ms.locfileid: "47402072"
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

## <a name="azure-and-redis"></a>Azure и Redis

[Microsoft.AspNetCore.DataProtection.AzureStorage](https://www.nuget.org/packages/Microsoft.AspNetCore.DataProtection.AzureStorage/) и [Microsoft.AspNetCore.DataProtection.Redis](https://www.nuget.org/packages/Microsoft.AspNetCore.DataProtection.Redis/) пакетов разрешено хранить ключи защиты данных в хранилище Azure или кэша Redis. Ключи могут совместно использоваться в нескольких экземплярах веб-приложения. Приложения могут совместно использовать файлы cookie проверки подлинности или CSRF защиты на нескольких серверах. Чтобы настроить поставщик хранилища больших двоичных объектов Azure, вызовите один из [PersistKeysToAzureBlobStorage](/dotnet/api/microsoft.aspnetcore.dataprotection.azuredataprotectionbuilderextensions.persistkeystoazureblobstorage) перегрузки:

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddDataProtection()
        .PersistKeysToAzureBlobStorage(new Uri("<blob URI including SAS token>"));
}
```

Чтобы настроить на Redis, вызовите один из [PersistKeysToRedis](/dotnet/api/microsoft.aspnetcore.dataprotection.redisdataprotectionbuilderextensions.persistkeystoredis) перегрузки:

```csharp
public void ConfigureServices(IServiceCollection services)
{
    var redis = ConnectionMultiplexer.Connect("<URI>");
    services.AddDataProtection()
        .PersistKeysToRedis(redis, "DataProtection-Keys");
}
```

Дополнительные сведения см. в следующих разделах:

* [StackExchange.Redis ConnectionMultiplexer](https://github.com/StackExchange/StackExchange.Redis/blob/master/docs/Basics.md)
* [Кэш Redis для Azure](/azure/redis-cache/cache-dotnet-how-to-use-azure-redis-cache#connect-to-the-cache)
* [Примеры ASPNET/DataProtection](https://github.com/aspnet/DataProtection/tree/master/samples)

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

Чтобы настроить поставщика EF Core, вызовите [ `PersistKeysToDbContext<TContext>` ](/dotnet/api/microsoft.aspnetcore.dataprotection.entityframeworkcoredataprotectionextensions.persistkeystodbcontext) метод:

[!code-csharp[Main](key-storage-providers/sample/Startup.cs?name=snippet&highlight=13-15)]

Универсальный параметр `TContext`, должен наследовать от [DbContext](/dotnet/api/microsoft.entityframeworkcore.dbcontext) и [IDataProtectionKeyContext](/dotnet/api/microsoft.aspnetcore.dataprotection.entityframeworkcore.idataprotectionkeycontext):

[!code-csharp[Main](key-storage-providers/sample/MyKeysContext.cs)]

::: moniker-end

## <a name="custom-key-repository"></a>Пользовательский репозиторий ключа

Если механизмы в поле не подходит, разработчик может указать собственный механизм сохраняемости ключа, предоставляя пользовательский [IXmlRepository](/dotnet/api/microsoft.aspnetcore.dataprotection.repositories.ixmlrepository).
