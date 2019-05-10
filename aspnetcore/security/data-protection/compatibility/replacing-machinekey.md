---
title: Замените machineKey ASP.NET в ASP.NET Core
author: rick-anderson
description: Узнайте, как заменить machineKey в ASP.NET, чтобы разрешить использование новых и более безопасные система защиты данных.
ms.author: riande
ms.date: 04/06/2019
uid: security/data-protection/compatibility/replacing-machinekey
ms.openlocfilehash: 2317cb50cfe63226baf336ebfc5d681d1cebe5c6
ms.sourcegitcommit: 5b0eca8c21550f95de3bb21096bd4fd4d9098026
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/27/2019
ms.locfileid: "64895341"
---
# <a name="replace-the-aspnet-machinekey-in-aspnet-core"></a><span data-ttu-id="eb252-103">Замените machineKey ASP.NET в ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="eb252-103">Replace the ASP.NET machineKey in ASP.NET Core</span></span>

<a name="compatibility-replacing-machinekey"></a>

<span data-ttu-id="eb252-104">Реализация `<machineKey>` элемент в ASP.NET [можно заменить](https://blogs.msdn.microsoft.com/webdev/2012/10/23/cryptographic-improvements-in-asp-net-4-5-pt-2/).</span><span class="sxs-lookup"><span data-stu-id="eb252-104">The implementation of the `<machineKey>` element in ASP.NET [is replaceable](https://blogs.msdn.microsoft.com/webdev/2012/10/23/cryptographic-improvements-in-asp-net-4-5-pt-2/).</span></span> <span data-ttu-id="eb252-105">Благодаря этому большинство вызовы процедуры шифрования ASP.NET, чтобы проходить через замены механизм защиты данных, включая новую систему защиты данных.</span><span class="sxs-lookup"><span data-stu-id="eb252-105">This allows most calls to ASP.NET cryptographic routines to be routed through a replacement data protection mechanism, including the new data protection system.</span></span>

## <a name="package-installation"></a><span data-ttu-id="eb252-106">Установка пакета</span><span class="sxs-lookup"><span data-stu-id="eb252-106">Package installation</span></span>

> [!NOTE]
> <span data-ttu-id="eb252-107">Новая система защиты данных можно только в том случае, установленных в существующее приложение ASP.NET, предназначенных для .NET 4.5.1 или более поздней версии.</span><span class="sxs-lookup"><span data-stu-id="eb252-107">The new data protection system can only be installed into an existing ASP.NET application targeting .NET 4.5.1 or later.</span></span> <span data-ttu-id="eb252-108">Установки будут завершаться ошибкой, если приложение предназначено для .NET 4.5 или уменьшить.</span><span class="sxs-lookup"><span data-stu-id="eb252-108">Installation will fail if the application targets .NET 4.5 or lower.</span></span>

<span data-ttu-id="eb252-109">Чтобы установить новую систему защиты данных в существующий проект ASP.NET 4.5.1+, установите пакет Microsoft.AspNetCore.DataProtection.SystemWeb.</span><span class="sxs-lookup"><span data-stu-id="eb252-109">To install the new data protection system into an existing ASP.NET 4.5.1+ project, install the package Microsoft.AspNetCore.DataProtection.SystemWeb.</span></span> <span data-ttu-id="eb252-110">Это будет создавать системы защиты данных с помощью [конфигурация по умолчанию](xref:security/data-protection/configuration/default-settings) параметры.</span><span class="sxs-lookup"><span data-stu-id="eb252-110">This will instantiate the data protection system using the [default configuration](xref:security/data-protection/configuration/default-settings) settings.</span></span>

<span data-ttu-id="eb252-111">При установке пакета, он вставляет строку в *Web.config* , сообщающий ASP.NET, чтобы использовать его для [наиболее криптографических операций](https://blogs.msdn.microsoft.com/webdev/2012/10/23/cryptographic-improvements-in-asp-net-4-5-pt-2/), включая проверку подлинности форм, состояние представления и вызовы MachineKey.Protect.</span><span class="sxs-lookup"><span data-stu-id="eb252-111">When you install the package, it inserts a line into *Web.config* that tells ASP.NET to use it for [most cryptographic operations](https://blogs.msdn.microsoft.com/webdev/2012/10/23/cryptographic-improvements-in-asp-net-4-5-pt-2/), including forms authentication, view state, and calls to MachineKey.Protect.</span></span> <span data-ttu-id="eb252-112">Строку, которая вставляется выглядит следующим образом.</span><span class="sxs-lookup"><span data-stu-id="eb252-112">The line that's inserted reads as follows.</span></span>

```xml
<machineKey compatibilityMode="Framework45" dataProtectorType="..." />
```

>[!TIP]
> <span data-ttu-id="eb252-113">Чтобы узнать, активен ли новую систему защиты данных, проверяя полей, таких как `__VIEWSTATE`, которой должно начинаться с «CfDJ8», как показано в приведенном ниже примере.</span><span class="sxs-lookup"><span data-stu-id="eb252-113">You can tell if the new data protection system is active by inspecting fields like `__VIEWSTATE`, which should begin with "CfDJ8" as in the example below.</span></span> <span data-ttu-id="eb252-114">«CfDJ8» — это представление в кодировке base64 заголовка, magic «09 F0 C9 F0», который определяет полезные данные, защищенные системой защиты данных.</span><span class="sxs-lookup"><span data-stu-id="eb252-114">"CfDJ8" is the base64 representation of the magic "09 F0 C9 F0" header that identifies a payload protected by the data protection system.</span></span>

```html
<input type="hidden" name="__VIEWSTATE" id="__VIEWSTATE" value="CfDJ8AWPr2EQPTBGs3L2GCZOpk...">
```

## <a name="package-configuration"></a><span data-ttu-id="eb252-115">Настройка пакета</span><span class="sxs-lookup"><span data-stu-id="eb252-115">Package configuration</span></span>

<span data-ttu-id="eb252-116">Система защиты данных создается с помощью конфигурации по умолчанию ноль setup.</span><span class="sxs-lookup"><span data-stu-id="eb252-116">The data protection system is instantiated with a default zero-setup configuration.</span></span> <span data-ttu-id="eb252-117">Тем не менее поскольку по умолчанию ключи сохраняются в локальной файловой системе, это не будет работать для приложений, которые будут развернуты в ферме.</span><span class="sxs-lookup"><span data-stu-id="eb252-117">However, since by default keys are persisted to the local file system, this won't work for applications which are deployed in a farm.</span></span> <span data-ttu-id="eb252-118">Чтобы устранить эту проблему, можно предоставить конфигурации путем создания типа, который подклассы DataProtectionStartup и переопределяет его метода ConfigureServices.</span><span class="sxs-lookup"><span data-stu-id="eb252-118">To resolve this, you can provide configuration by creating a type which subclasses DataProtectionStartup and overrides its ConfigureServices method.</span></span>

<span data-ttu-id="eb252-119">Ниже приведен пример типа запуска защиты пользовательских данных, который настроили где сохраняются ключи и как они выполняется шифрование неактивных данных.</span><span class="sxs-lookup"><span data-stu-id="eb252-119">Below is an example of a custom data protection startup type which configured both where keys are persisted and how they're encrypted at rest.</span></span> <span data-ttu-id="eb252-120">Оно также переопределяет политику изоляции приложений по умолчанию, указав собственное имя приложения.</span><span class="sxs-lookup"><span data-stu-id="eb252-120">It also overrides the default app isolation policy by providing its own application name.</span></span>

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
> <span data-ttu-id="eb252-121">Можно также использовать `<machineKey applicationName="my-app" ... />` вместо явного вызова SetApplicationName.</span><span class="sxs-lookup"><span data-stu-id="eb252-121">You can also use `<machineKey applicationName="my-app" ... />` in place of an explicit call to SetApplicationName.</span></span> <span data-ttu-id="eb252-122">Это удобный механизм во избежание заставляя разработчика создавать DataProtectionStartup производным типом, если все, что они хотели бы настроить задание имени приложения.</span><span class="sxs-lookup"><span data-stu-id="eb252-122">This is a convenience mechanism to avoid forcing the developer to create a DataProtectionStartup-derived type if all they wanted to configure was setting the application name.</span></span>

<span data-ttu-id="eb252-123">Чтобы включить этот настраиваемой конфигурации, вернитесь в файл Web.config и найдите `<appSettings>` добавлен указанный элемент, установите пакет в файл конфигурации.</span><span class="sxs-lookup"><span data-stu-id="eb252-123">To enable this custom configuration, go back to Web.config and look for the `<appSettings>` element that the package install added to the config file.</span></span> <span data-ttu-id="eb252-124">Он имеет вид следующую разметку:</span><span class="sxs-lookup"><span data-stu-id="eb252-124">It will look like the following markup:</span></span>

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

<span data-ttu-id="eb252-125">Укажите пустое значение с именем с указанием сборки DataProtectionStartup производный тип, который вы только что создали.</span><span class="sxs-lookup"><span data-stu-id="eb252-125">Fill in the blank value with the assembly-qualified name of the DataProtectionStartup-derived type you just created.</span></span> <span data-ttu-id="eb252-126">Если имя приложения — DataProtectionDemo, это выглядит следующим образом ниже.</span><span class="sxs-lookup"><span data-stu-id="eb252-126">If the name of the application is DataProtectionDemo, this would look like the below.</span></span>

```xml
<add key="aspnet:dataProtectionStartupType"
     value="DataProtectionDemo.MyDataProtectionStartup, DataProtectionDemo" />
```

<span data-ttu-id="eb252-127">Система защиты данных, только что настроенная теперь готов для использования внутри приложения.</span><span class="sxs-lookup"><span data-stu-id="eb252-127">The newly-configured data protection system is now ready for use inside the application.</span></span>
