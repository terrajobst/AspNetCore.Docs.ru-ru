---
title: "Замена `<machineKey>` в ASP.NET"
author: rick-anderson
description: "Замена `<machineKey>` в ASP.NET"
manager: wpickett
ms.author: riande
ms.date: 10/14/2016
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: security/data-protection/compatibility/replacing-machinekey
ms.openlocfilehash: 865c949a526e07d41ac4627fdf0411d5422adc16
ms.sourcegitcommit: a510f38930abc84c4b302029d019a34dfe76823b
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/30/2018
---
# <a name="replacing-machinekey-in-aspnet"></a><span data-ttu-id="ea005-103">Замена `<machineKey>` в ASP.NET</span><span class="sxs-lookup"><span data-stu-id="ea005-103">Replacing `<machineKey>` in ASP.NET</span></span>

<a name="compatibility-replacing-machinekey"></a>

<span data-ttu-id="ea005-104">Реализация `<machineKey>` элемент в ASP.NET [заменяемая](https://blogs.msdn.microsoft.com/webdev/2012/10/23/cryptographic-improvements-in-asp-net-4-5-pt-2/).</span><span class="sxs-lookup"><span data-stu-id="ea005-104">The implementation of the `<machineKey>` element in ASP.NET [is replaceable](https://blogs.msdn.microsoft.com/webdev/2012/10/23/cryptographic-improvements-in-asp-net-4-5-pt-2/).</span></span> <span data-ttu-id="ea005-105">Это позволяет большинство вызовы подпрограммы шифрования ASP.NET для направляться через замены механизм защиты данных, включая новую систему защиты данных.</span><span class="sxs-lookup"><span data-stu-id="ea005-105">This allows most calls to ASP.NET cryptographic routines to be routed through a replacement data protection mechanism, including the new data protection system.</span></span>

## <a name="package-installation"></a><span data-ttu-id="ea005-106">Установка пакета</span><span class="sxs-lookup"><span data-stu-id="ea005-106">Package installation</span></span>

> [!NOTE]
> <span data-ttu-id="ea005-107">Новая система защиты данных можно только в том случае, установленных в существующее приложение ASP.NET, предназначенных для .NET 4.5.1 или более поздней версии.</span><span class="sxs-lookup"><span data-stu-id="ea005-107">The new data protection system can only be installed into an existing ASP.NET application targeting .NET 4.5.1 or higher.</span></span> <span data-ttu-id="ea005-108">Установка будет ошибкой, если приложение предназначено для использования .NET 4.5 или ниже.</span><span class="sxs-lookup"><span data-stu-id="ea005-108">Installation will fail if the application targets .NET 4.5 or lower.</span></span>

<span data-ttu-id="ea005-109">Для установки новой системы защиты данных в существующий проект ASP.NET 4.5.1+, установите пакет Microsoft.AspNetCore.DataProtection.SystemWeb.</span><span class="sxs-lookup"><span data-stu-id="ea005-109">To install the new data protection system into an existing ASP.NET 4.5.1+ project, install the package Microsoft.AspNetCore.DataProtection.SystemWeb.</span></span> <span data-ttu-id="ea005-110">Это будет создавать системы защиты данных с помощью [конфигурация по умолчанию](xref:security/data-protection/configuration/default-settings) параметры.</span><span class="sxs-lookup"><span data-stu-id="ea005-110">This will instantiate the data protection system using the [default configuration](xref:security/data-protection/configuration/default-settings) settings.</span></span>

<span data-ttu-id="ea005-111">При установке пакета, он вставляется строка в *Web.config* , сообщает ASP.NET, чтобы использовать его для [наиболее криптографических операций](https://blogs.msdn.microsoft.com/webdev/2012/10/23/cryptographic-improvements-in-asp-net-4-5-pt-2/), включая проверку подлинности форм, состояние представления и вызовы MachineKey.Protect.</span><span class="sxs-lookup"><span data-stu-id="ea005-111">When you install the package, it inserts a line into *Web.config* that tells ASP.NET to use it for [most cryptographic operations](https://blogs.msdn.microsoft.com/webdev/2012/10/23/cryptographic-improvements-in-asp-net-4-5-pt-2/), including forms authentication, view state, and calls to MachineKey.Protect.</span></span> <span data-ttu-id="ea005-112">Строки, которая будет вставлена выглядит следующим образом.</span><span class="sxs-lookup"><span data-stu-id="ea005-112">The line that's inserted reads as follows.</span></span>

```xml
<machineKey compatibilityMode="Framework45" dataProtectorType="..." />
```

>[!TIP]
> <span data-ttu-id="ea005-113">Можно определить, активен ли новая система защиты данных путем проверки поля, как `__VIEWSTATE`, которого должна начинаться с «CfDJ8», как показано в приведенном ниже примере.</span><span class="sxs-lookup"><span data-stu-id="ea005-113">You can tell if the new data protection system is active by inspecting fields like `__VIEWSTATE`, which should begin with "CfDJ8" as in the example below.</span></span> <span data-ttu-id="ea005-114">«CfDJ8» является представлением base64 заголовка magic «09 F0 C9 F0», которое определяет полезные данные, защищенные системой защиты данных.</span><span class="sxs-lookup"><span data-stu-id="ea005-114">"CfDJ8" is the base64 representation of the magic "09 F0 C9 F0" header that identifies a payload protected by the data protection system.</span></span>

```html
<input type="hidden" name="__VIEWSTATE" id="__VIEWSTATE" value="CfDJ8AWPr2EQPTBGs3L2GCZOpk..." />
```

## <a name="package-configuration"></a><span data-ttu-id="ea005-115">Настройка пакета</span><span class="sxs-lookup"><span data-stu-id="ea005-115">Package configuration</span></span>

<span data-ttu-id="ea005-116">Система защиты данных создается с нуля установки конфигурации по умолчанию.</span><span class="sxs-lookup"><span data-stu-id="ea005-116">The data protection system is instantiated with a default zero-setup configuration.</span></span> <span data-ttu-id="ea005-117">Тем не менее поскольку по умолчанию ключи сохраняются в локальной файловой системе, это не подходит для приложений, которые развертываются в ферме.</span><span class="sxs-lookup"><span data-stu-id="ea005-117">However, since by default keys are persisted to the local file system, this won't work for applications which are deployed in a farm.</span></span> <span data-ttu-id="ea005-118">Чтобы устранить эту проблему, можно предоставить конфигурации, создавая тип какие подклассов DataProtectionStartup и переопределяет метод его ConfigureServices.</span><span class="sxs-lookup"><span data-stu-id="ea005-118">To resolve this, you can provide configuration by creating a type which subclasses DataProtectionStartup and overrides its ConfigureServices method.</span></span>

<span data-ttu-id="ea005-119">Ниже приведен пример типа запуска защиты пользовательских данных, который настроен как где сохраняются ключи и как они выполняется в зашифрованном виде.</span><span class="sxs-lookup"><span data-stu-id="ea005-119">Below is an example of a custom data protection startup type which configured both where keys are persisted and how they're encrypted at rest.</span></span> <span data-ttu-id="ea005-120">Оно также переопределяет политику по умолчанию для изоляции приложений, предоставив собственное имя приложения.</span><span class="sxs-lookup"><span data-stu-id="ea005-120">It also overrides the default app isolation policy by providing its own application name.</span></span>

```csharp
using System;
using System.IO;
using Microsoft.AspNetCore.DataProtection;
using Microsoft.AspNetCore.DataProtection.SystemWeb;
using Microsoft.Extensions.DependencyInjection;

namespace DataProtectionDemo
{
    public class MyDataProtectionStartup : DataProtectionStartup
    {
        public override void ConfigureServices(IServiceCollection services)
        {
            services.AddDataProtection()
                .SetApplicationName("my-app")
                .PersistKeysToFileSystem(new DirectoryInfo(@"\\server\share\myapp-keys\"))
                .ProtectKeysWithCertificate("thumbprint");
        }
    }
}
```

>[!TIP]
> <span data-ttu-id="ea005-121">Можно также использовать `<machineKey applicationName="my-app" ... />` вместо явного вызова SetApplicationName.</span><span class="sxs-lookup"><span data-stu-id="ea005-121">You can also use `<machineKey applicationName="my-app" ... />` in place of an explicit call to SetApplicationName.</span></span> <span data-ttu-id="ea005-122">Это удобный механизм, чтобы избежать перезагрузки разработчикам создавать DataProtectionStartup производным типом, если все, что они хотели бы настроить задание имени приложения.</span><span class="sxs-lookup"><span data-stu-id="ea005-122">This is a convenience mechanism to avoid forcing the developer to create a DataProtectionStartup-derived type if all they wanted to configure was setting the application name.</span></span>

<span data-ttu-id="ea005-123">Для включения этой пользовательской конфигурации, вернитесь в файл Web.config и найдите `<appSettings>` элемента, установите пакет добавлен в файл конфигурации.</span><span class="sxs-lookup"><span data-stu-id="ea005-123">To enable this custom configuration, go back to Web.config and look for the `<appSettings>` element that the package install added to the config file.</span></span> <span data-ttu-id="ea005-124">Оно будет иметь вид следующую разметку:</span><span class="sxs-lookup"><span data-stu-id="ea005-124">It will look like the following markup:</span></span>

```xml
<appSettings>
  <!--
  If you want to customize the behavior of the ASP.NET Core Data Protection stack, set the
  "aspnet:dataProtectionStartupType" switch below to be the fully-qualified name of a
  type which subclasses Microsoft.AspNetCore.DataProtection.SystemWeb.DataProtectionStartup.
  -->
  <add key="aspnet:dataProtectionStartupType" value="" />
</appSettings>
```

<span data-ttu-id="ea005-125">Заполните пустое значение с именем сборки DataProtectionStartup производный тип, который вы только что создали.</span><span class="sxs-lookup"><span data-stu-id="ea005-125">Fill in the blank value with the assembly-qualified name of the DataProtectionStartup-derived type you just created.</span></span> <span data-ttu-id="ea005-126">Если имя приложения, DataProtectionDemo должен выглядеть ниже.</span><span class="sxs-lookup"><span data-stu-id="ea005-126">If the name of the application is DataProtectionDemo, this would look like the below.</span></span>

```xml
<add key="aspnet:dataProtectionStartupType"
     value="DataProtectionDemo.MyDataProtectionStartup, DataProtectionDemo" />
```

<span data-ttu-id="ea005-127">Система защиты данных только что настроенная готов для использования в приложении.</span><span class="sxs-lookup"><span data-stu-id="ea005-127">The newly-configured data protection system is now ready for use inside the application.</span></span>
