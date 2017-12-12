---
title: "Заменив < machineKey > в ASP.NET"
author: rick-anderson
description: "Заменив < machineKey > в ASP.NET"
keywords: "ASP.NET Core, безопасности, < machineKey > machineKey"
ms.author: riande
manager: wpickett
ms.date: 10/14/2016
ms.topic: article
ms.assetid: 5ac13589-3837-4b4d-8abe-81f843942120
ms.technology: aspnet
ms.prod: asp.net-core
uid: security/data-protection/compatibility/replacing-machinekey
ms.openlocfilehash: b5a1be5fee7489f266e8a676956f68b499c6f14f
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 11/10/2017
---
# <a name="replacing-machinekey-in-aspnet"></a>Замена `<machineKey>` в ASP.NET

<a name="compatibility-replacing-machinekey"></a>

Реализация `<machineKey>` элемент в ASP.NET [заменяемая](https://blogs.msdn.microsoft.com/webdev/2012/10/23/cryptographic-improvements-in-asp-net-4-5-pt-2/). Это позволяет большинство вызовы подпрограммы шифрования ASP.NET для направляться через замены механизм защиты данных, включая новую систему защиты данных.

## <a name="package-installation"></a>Установка пакета

> [!NOTE]
> Новая система защиты данных можно только в том случае, установленных в существующее приложение ASP.NET, предназначенных для .NET 4.5.1 или более поздней версии. Установка будет ошибкой, если приложение предназначено для использования .NET 4.5 или ниже.

Для установки новой системы защиты данных в существующий проект ASP.NET 4.5.1+, установите пакет Microsoft.AspNetCore.DataProtection.SystemWeb. Это будет создавать системы защиты данных с помощью [конфигурация по умолчанию](xref:security/data-protection/configuration/default-settings) параметры.

При установке пакета, он вставляется строка в *Web.config* , сообщает ASP.NET, чтобы использовать его для [наиболее криптографических операций](https://blogs.msdn.microsoft.com/webdev/2012/10/23/cryptographic-improvements-in-asp-net-4-5-pt-2/), включая проверку подлинности форм, состояние представления и вызовы MachineKey.Protect. Строки, которая будет вставлена выглядит следующим образом.

```xml
<machineKey compatibilityMode="Framework45" dataProtectorType="..." />
```

>[!TIP]
> Можно определить, активен ли новая система защиты данных путем проверки поля, как `__VIEWSTATE`, которого должна начинаться с «CfDJ8», как показано в приведенном ниже примере. «CfDJ8» является представлением base64 заголовка magic «09 F0 C9 F0», которое определяет полезные данные, защищенные системой защиты данных.

```html
<input type="hidden" name="__VIEWSTATE" id="__VIEWSTATE" value="CfDJ8AWPr2EQPTBGs3L2GCZOpk..." />
```

## <a name="package-configuration"></a>Настройка пакета

Система защиты данных создается с нуля установки конфигурации по умолчанию. Тем не менее поскольку по умолчанию ключи сохраняются в локальной файловой системе, это не подходит для приложений, которые развертываются в ферме. Чтобы устранить эту проблему, можно предоставить конфигурации, создавая тип какие подклассов DataProtectionStartup и переопределяет метод его ConfigureServices.

Ниже приведен пример типа запуска защиты пользовательских данных, который настроен как где сохраняются ключи и как они выполняется в зашифрованном виде. Оно также переопределяет политику по умолчанию для изоляции приложений, предоставив собственное имя приложения.

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
> Можно также использовать `<machineKey applicationName="my-app" ... />` вместо явного вызова SetApplicationName. Это удобный механизм, чтобы избежать перезагрузки разработчикам создавать DataProtectionStartup производным типом, если все, что они хотели бы настроить задание имени приложения.

Для включения этой пользовательской конфигурации, вернитесь в файл Web.config и найдите `<appSettings>` элемента, установите пакет добавлен в файл конфигурации. Оно будет иметь вид следующую разметку:

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

Заполните пустое значение с именем сборки DataProtectionStartup производный тип, который вы только что создали. Если имя приложения, DataProtectionDemo должен выглядеть ниже.

```xml
<add key="aspnet:dataProtectionStartupType"
     value="DataProtectionDemo.MyDataProtectionStartup, DataProtectionDemo" />
```

Система защиты данных только что настроенная готов для использования в приложении.
