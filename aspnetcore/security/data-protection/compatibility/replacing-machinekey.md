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
# <a name="replace-the-aspnet-machinekey-in-aspnet-core"></a>Замените machineKey ASP.NET в ASP.NET Core

<a name="compatibility-replacing-machinekey"></a>

Реализация `<machineKey>` элемент в ASP.NET [можно заменить](https://blogs.msdn.microsoft.com/webdev/2012/10/23/cryptographic-improvements-in-asp-net-4-5-pt-2/). Благодаря этому большинство вызовы процедуры шифрования ASP.NET, чтобы проходить через замены механизм защиты данных, включая новую систему защиты данных.

## <a name="package-installation"></a>Установка пакета

> [!NOTE]
> Новая система защиты данных можно только в том случае, установленных в существующее приложение ASP.NET, предназначенных для .NET 4.5.1 или более поздней версии. Установки будут завершаться ошибкой, если приложение предназначено для .NET 4.5 или уменьшить.

Чтобы установить новую систему защиты данных в существующий проект ASP.NET 4.5.1+, установите пакет Microsoft.AspNetCore.DataProtection.SystemWeb. Это будет создавать системы защиты данных с помощью [конфигурация по умолчанию](xref:security/data-protection/configuration/default-settings) параметры.

При установке пакета, он вставляет строку в *Web.config* , сообщающий ASP.NET, чтобы использовать его для [наиболее криптографических операций](https://blogs.msdn.microsoft.com/webdev/2012/10/23/cryptographic-improvements-in-asp-net-4-5-pt-2/), включая проверку подлинности форм, состояние представления и вызовы MachineKey.Protect. Строку, которая вставляется выглядит следующим образом.

```xml
<machineKey compatibilityMode="Framework45" dataProtectorType="..." />
```

>[!TIP]
> Чтобы узнать, активен ли новую систему защиты данных, проверяя полей, таких как `__VIEWSTATE`, которой должно начинаться с «CfDJ8», как показано в приведенном ниже примере. «CfDJ8» — это представление в кодировке base64 заголовка, magic «09 F0 C9 F0», который определяет полезные данные, защищенные системой защиты данных.

```html
<input type="hidden" name="__VIEWSTATE" id="__VIEWSTATE" value="CfDJ8AWPr2EQPTBGs3L2GCZOpk...">
```

## <a name="package-configuration"></a>Настройка пакета

Система защиты данных создается с помощью конфигурации по умолчанию ноль setup. Тем не менее поскольку по умолчанию ключи сохраняются в локальной файловой системе, это не будет работать для приложений, которые будут развернуты в ферме. Чтобы устранить эту проблему, можно предоставить конфигурации путем создания типа, который подклассы DataProtectionStartup и переопределяет его метода ConfigureServices.

Ниже приведен пример типа запуска защиты пользовательских данных, который настроили где сохраняются ключи и как они выполняется шифрование неактивных данных. Оно также переопределяет политику изоляции приложений по умолчанию, указав собственное имя приложения.

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
> Можно также использовать `<machineKey applicationName="my-app" ... />` вместо явного вызова SetApplicationName. Это удобный механизм во избежание заставляя разработчика создавать DataProtectionStartup производным типом, если все, что они хотели бы настроить задание имени приложения.

Чтобы включить этот настраиваемой конфигурации, вернитесь в файл Web.config и найдите `<appSettings>` добавлен указанный элемент, установите пакет в файл конфигурации. Он имеет вид следующую разметку:

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

Укажите пустое значение с именем с указанием сборки DataProtectionStartup производный тип, который вы только что создали. Если имя приложения — DataProtectionDemo, это выглядит следующим образом ниже.

```xml
<add key="aspnet:dataProtectionStartupType"
     value="DataProtectionDemo.MyDataProtectionStartup, DataProtectionDemo" />
```

Система защиты данных, только что настроенная теперь готов для использования внутри приложения.
