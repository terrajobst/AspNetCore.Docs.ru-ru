---
title: "Поставщики хранилища ключей"
author: rick-anderson
description: "Поставщики хранилища ключей"
keywords: Encryption,ASP.NET Core
ms.author: riande
manager: wpickett
ms.date: 01/14/2017
ms.topic: article
ms.assetid: 423e0a79-2f34-44c4-aaf3-146a53c39251
ms.technology: aspnet
ms.prod: asp.net-core
uid: security/data-protection/implementation/key-storage-providers
ms.openlocfilehash: 952fc99d2486b8e78d97356c333257e581a79b42
ms.sourcegitcommit: 8f4d4fad1ca27adf9e396f5c205c9875a3963664
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/13/2017
---
# <a name="key-storage-providers"></a><span data-ttu-id="0a122-104">Поставщики хранилища ключей</span><span class="sxs-lookup"><span data-stu-id="0a122-104">Key storage providers</span></span>

<a name="data-protection-implementation-key-storage-providers"></a>

<span data-ttu-id="0a122-105">По умолчанию система защиты данных [использует эвристику](../configuration/default-settings.md#data-protection-default-settings) для определения, где должны сохраняться материалом ключа шифрования.</span><span class="sxs-lookup"><span data-stu-id="0a122-105">By default the data protection system [employs a heuristic](../configuration/default-settings.md#data-protection-default-settings) to determine where cryptographic key material should be persisted.</span></span> <span data-ttu-id="0a122-106">Разработчик может переопределить эвристика и вручную указать расположение.</span><span class="sxs-lookup"><span data-stu-id="0a122-106">The developer can override the heuristic and manually specify the location.</span></span>

> [!NOTE]
> <span data-ttu-id="0a122-107">При указании расположения явную сохраняемость ключа система защиты данных будет отменить регистрацию шифрования ключа по умолчанию в механизм rest, который предоставляется эвристика, поэтому ключи больше не шифруются при хранении.</span><span class="sxs-lookup"><span data-stu-id="0a122-107">If you specify an explicit key persistence location, the data protection system will deregister the default key encryption at rest mechanism that the heuristic provided, so keys will no longer be encrypted at rest.</span></span> <span data-ttu-id="0a122-108">Рекомендуется, кроме того [укажите механизм явного ключа шифрования](key-encryption-at-rest.md#data-protection-implementation-key-encryption-at-rest-providers) для производственных приложений.</span><span class="sxs-lookup"><span data-stu-id="0a122-108">It is recommended that you additionally [specify an explicit key encryption mechanism](key-encryption-at-rest.md#data-protection-implementation-key-encryption-at-rest-providers) for production applications.</span></span>

<span data-ttu-id="0a122-109">Система защиты данных поставляется с нескольких поставщиков хранилища ключей в поле.</span><span class="sxs-lookup"><span data-stu-id="0a122-109">The data protection system ships with several in-box key storage providers.</span></span>

## <a name="file-system"></a><span data-ttu-id="0a122-110">Файловая система</span><span class="sxs-lookup"><span data-stu-id="0a122-110">File system</span></span>

<span data-ttu-id="0a122-111">Мы полагаем, во многих приложениях будет использовать файл на основе системы ключа репозитория.</span><span class="sxs-lookup"><span data-stu-id="0a122-111">We anticipate that many apps will use a file system-based key repository.</span></span> <span data-ttu-id="0a122-112">Чтобы настроить ее, вызовите [PersistKeysToFileSystem](https://github.com/aspnet/DataProtection/blob/rel/1.1.0/src/Microsoft.AspNetCore.DataProtection/DataProtectionBuilderExtensions.cs) подпрограммы конфигурации, как показано ниже.</span><span class="sxs-lookup"><span data-stu-id="0a122-112">To configure this, call the [PersistKeysToFileSystem](https://github.com/aspnet/DataProtection/blob/rel/1.1.0/src/Microsoft.AspNetCore.DataProtection/DataProtectionBuilderExtensions.cs) configuration routine as shown below.</span></span> <span data-ttu-id="0a122-113">Укажите `DirectoryInfo` команды в репозиторий, где должно храниться ключей.</span><span class="sxs-lookup"><span data-stu-id="0a122-113">Provide a `DirectoryInfo` pointing to the repository where keys should be stored.</span></span>

```csharp
sc.AddDataProtection()
       // persist keys to a specific directory
       .PersistKeysToFileSystem(new DirectoryInfo(@"c:\temp-keys\"));
   ```

<span data-ttu-id="0a122-114">`DirectoryInfo` Может указывать на каталог на локальном компьютере или она может указывать на папку на общем сетевом ресурсе.</span><span class="sxs-lookup"><span data-stu-id="0a122-114">The `DirectoryInfo` can point to a directory on the local machine, or it can point to a folder on a network share.</span></span> <span data-ttu-id="0a122-115">Если каталогу на локальном компьютере (и рекомендуется только приложения на локальном компьютере необходимо использовать этот репозиторий), рассмотрите возможность использования [Windows DPAPI](key-encryption-at-rest.md#data-protection-implementation-key-encryption-at-rest) для шифрования ключей при хранении.</span><span class="sxs-lookup"><span data-stu-id="0a122-115">If pointing to a directory on the local machine (and the scenario is that only applications on the local machine will need to use this repository), consider using [Windows DPAPI](key-encryption-at-rest.md#data-protection-implementation-key-encryption-at-rest) to encrypt the keys at rest.</span></span> <span data-ttu-id="0a122-116">В противном случае рассмотрите возможность использования [сертификат X.509](key-encryption-at-rest.md#data-protection-implementation-key-encryption-at-rest) для шифрования ключей при хранении.</span><span class="sxs-lookup"><span data-stu-id="0a122-116">Otherwise consider using an [X.509 certificate](key-encryption-at-rest.md#data-protection-implementation-key-encryption-at-rest) to encrypt keys at rest.</span></span>

## <a name="azure-and-redis"></a><span data-ttu-id="0a122-117">Azure и Redis</span><span class="sxs-lookup"><span data-stu-id="0a122-117">Azure and Redis</span></span>

<span data-ttu-id="0a122-118">`Microsoft.AspNetCore.DataProtection.AzureStorage` И `Microsoft.AspNetCore.DataProtection.Redis` пакетов разрешено хранение ключей защиты данных в хранилище Azure или кэш Redis.</span><span class="sxs-lookup"><span data-stu-id="0a122-118">The `Microsoft.AspNetCore.DataProtection.AzureStorage` and `Microsoft.AspNetCore.DataProtection.Redis` packages allow storing your data protection keys in Azure Storage or a Redis cache.</span></span> <span data-ttu-id="0a122-119">Ключи могут совместно использоваться в нескольких экземплярах веб-приложения.</span><span class="sxs-lookup"><span data-stu-id="0a122-119">Keys can be shared across several instances of a web app.</span></span> <span data-ttu-id="0a122-120">Приложения ASP.NET Core могут совместно использовать файлы cookie проверки подлинности или защиту CSRF по нескольким серверам.</span><span class="sxs-lookup"><span data-stu-id="0a122-120">Your ASP.NET Core app can share authentication cookies or CSRF protection across multiple servers.</span></span> <span data-ttu-id="0a122-121">Чтобы настроить в Azure, вызовите один из [PersistKeysToAzureBlobStorage](https://github.com/aspnet/DataProtection/blob/rel/1.1.0/src/Microsoft.AspNetCore.DataProtection.AzureStorage/AzureDataProtectionBuilderExtensions.cs) перегрузки метода, как показано ниже.</span><span class="sxs-lookup"><span data-stu-id="0a122-121">To configure on Azure, call one of the [PersistKeysToAzureBlobStorage](https://github.com/aspnet/DataProtection/blob/rel/1.1.0/src/Microsoft.AspNetCore.DataProtection.AzureStorage/AzureDataProtectionBuilderExtensions.cs) overloads as shown below.</span></span>

```
public void ConfigureServices(IServiceCollection services)
{
    services.AddDataProtection()
        .PersistKeysToAzureBlobStorage(new Uri("<blob URI including SAS token>"));

    services.AddMvc();
}
```

<span data-ttu-id="0a122-122">См. также [Azure тестовый код](https://github.com/aspnet/DataProtection/blob/rel/1.1.0/samples/AzureBlob/Program.cs).</span><span class="sxs-lookup"><span data-stu-id="0a122-122">See also the [Azure test code](https://github.com/aspnet/DataProtection/blob/rel/1.1.0/samples/AzureBlob/Program.cs).</span></span>

<span data-ttu-id="0a122-123">Чтобы настроить Redis, вызовите один из [PersistKeysToRedis](https://github.com/aspnet/DataProtection/blob/rel/1.1.0/src/Microsoft.AspNetCore.DataProtection.Redis/RedisDataProtectionBuilderExtensions.cs) перегрузки метода, как показано ниже.</span><span class="sxs-lookup"><span data-stu-id="0a122-123">To configure on Redis, call one of the [PersistKeysToRedis](https://github.com/aspnet/DataProtection/blob/rel/1.1.0/src/Microsoft.AspNetCore.DataProtection.Redis/RedisDataProtectionBuilderExtensions.cs) overloads as shown below.</span></span>

```
public void ConfigureServices(IServiceCollection services)
{
    // Connect to Redis database.
    var redis = ConnectionMultiplexer.Connect("<URI>");
    services.AddDataProtection()
        .PersistKeysToRedis(redis, "DataProtection-Keys");

    services.AddMvc();
}
```

<span data-ttu-id="0a122-124">Более подробную информацию см. в следующих разделах:</span><span class="sxs-lookup"><span data-stu-id="0a122-124">See the following for more information:</span></span>

- [<span data-ttu-id="0a122-125">StackExchange.Redis ConnectionMultiplexer</span><span class="sxs-lookup"><span data-stu-id="0a122-125">StackExchange.Redis ConnectionMultiplexer</span></span>](https://github.com/StackExchange/StackExchange.Redis/blob/master/docs/Basics.md)
- [<span data-ttu-id="0a122-126">Кэш Redis для Azure</span><span class="sxs-lookup"><span data-stu-id="0a122-126">Azure Redis Cache</span></span>](https://docs.microsoft.com/azure/redis-cache/cache-dotnet-how-to-use-azure-redis-cache#connect-to-the-cache)
- <span data-ttu-id="0a122-127">[Тестовый код redis](https://github.com/aspnet/DataProtection/blob/rel/1.1.0/samples/Redis/Program.cs).</span><span class="sxs-lookup"><span data-stu-id="0a122-127">[Redis test code](https://github.com/aspnet/DataProtection/blob/rel/1.1.0/samples/Redis/Program.cs).</span></span>

## <a name="registry"></a><span data-ttu-id="0a122-128">Реестр</span><span class="sxs-lookup"><span data-stu-id="0a122-128">Registry</span></span>

<span data-ttu-id="0a122-129">Иногда приложение не может иметь доступ на запись в файловой системе.</span><span class="sxs-lookup"><span data-stu-id="0a122-129">Sometimes the app might not have write access to the file system.</span></span> <span data-ttu-id="0a122-130">Рассмотрим сценарий, где приложение выполняется виртуальной учетной записью службы (например, удостоверение пула приложений w3wp.exe).</span><span class="sxs-lookup"><span data-stu-id="0a122-130">Consider a scenario where an app is running as a virtual service account (such as w3wp.exe's app pool identity).</span></span> <span data-ttu-id="0a122-131">В таких случаях администратор может подготовить раздел реестра, соответствующий ACLed для учетной записи удостоверения службы.</span><span class="sxs-lookup"><span data-stu-id="0a122-131">In these cases, the administrator may have provisioned a registry key that is appropriate ACLed for the service account identity.</span></span> <span data-ttu-id="0a122-132">Вызовите [PersistKeysToRegistry](https://github.com/aspnet/DataProtection/blob/rel/1.1.0/src/Microsoft.AspNetCore.DataProtection/DataProtectionBuilderExtensions.cs) подпрограммы конфигурации, как показано ниже.</span><span class="sxs-lookup"><span data-stu-id="0a122-132">Call the [PersistKeysToRegistry](https://github.com/aspnet/DataProtection/blob/rel/1.1.0/src/Microsoft.AspNetCore.DataProtection/DataProtectionBuilderExtensions.cs) configuration routine as shown below.</span></span> <span data-ttu-id="0a122-133">Укажите `RegistryKey` указывает место хранения криптографических ключей и значений.</span><span class="sxs-lookup"><span data-stu-id="0a122-133">Provide a `RegistryKey` pointing to the location where cryptographic keys/values should be stored.</span></span>

```csharp
   sc.AddDataProtection()
       // persist keys to a specific location in the system registry
       .PersistKeysToRegistry(Registry.CurrentUser.OpenSubKey(@"SOFTWARE\Sample\keys"));
   ```

<span data-ttu-id="0a122-134">При использовании системного реестра как механизм сохраняемости, рассмотрите возможность использования [Windows DPAPI](key-encryption-at-rest.md#data-protection-implementation-key-encryption-at-rest) для шифрования ключей при хранении.</span><span class="sxs-lookup"><span data-stu-id="0a122-134">If you use the system registry as a persistence mechanism, consider using [Windows DPAPI](key-encryption-at-rest.md#data-protection-implementation-key-encryption-at-rest) to encrypt the keys at rest.</span></span>

## <a name="custom-key-repository"></a><span data-ttu-id="0a122-135">Пользовательские хранилища ключей</span><span class="sxs-lookup"><span data-stu-id="0a122-135">Custom key repository</span></span>

<span data-ttu-id="0a122-136">Если встроенные механизмы не подходят, разработчик может указать собственный механизм сохраняемости ключа, предоставляя пользовательский `IXmlRepository`.</span><span class="sxs-lookup"><span data-stu-id="0a122-136">If the in-box mechanisms are not appropriate, the developer can specify their own key persistence mechanism by providing a custom `IXmlRepository`.</span></span>
