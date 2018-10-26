---
uid: config-builder
title: Построители конфигурации для ASP.NET
author: rick-anderson
description: Узнайте, как получить данные конфигурации из источников, отличных от web.config значения из внешних источников.
ms.author: riande
ms.date: 10/29/2018
ms.technology: aspnet
msc.type: content
ms.openlocfilehash: d5a3916c3df9778d14be80342bafbc3456a69a03
ms.sourcegitcommit: f2d14a7518d6ee51aca9333818ac1276e7b5ecef
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/26/2018
ms.locfileid: "50134539"
---
# <a name="configuration-builders-for-aspnet"></a>Построители конфигурации для ASP.NET

По [Molloy Стивен](https://github.com/StephenMolloy) и [Рик Андерсон](https://twitter.com/RickAndMSFT)

Построители конфигурации предоставляют механизм современных и гибкой разработки для приложений ASP.NET получить значения конфигурации из внешних источников.

Построители конфигурации:

* Становятся доступными в .NET Framework 4.7.1 и более поздней версии.
* Предоставляют гибкий механизм для чтения значения конфигурации.
* Устранить некоторые из основных потребностей приложений, когда они перемещаются в контейнер и облачную среду с фокусом ввода.
* Можно использовать для улучшения защиты данных конфигурации путем рисования из источников, которые ранее недоступны (например, Azure Key Vault и переменные среды) в систему конфигурации .NET.

## <a name="keyvalue-configuration-builders"></a>Ключ/значение построителей конфигурации

Распространенный сценарий, который может обрабатываться с помощью построителей конфигурации — предоставить механизм замены базовый ключ значение для разделов конфигурации, которые соответствуют шаблону ключ/значение. .NET Framework концепцию ConfigurationBuilders не ограничивается разделы конфигурации или шаблоны. Тем не менее многие из построителей конфигурации в `Microsoft.Configuration.ConfigurationBuilders` ([github](https://github.com/aspnet/MicrosoftConfigurationBuilders)), [NuGet](https://www.nuget.org/packages?q=Microsoft.Configuration.ConfigurationBuilders) работать внутри шаблона ключ/значение.

## <a name="keyvalue-configuration-builders-settings"></a>Параметры построители конфигурации ключ значение

Следующие параметры применяются к все построители конфигурации ключ/значение в `Microsoft.Configuration.ConfigurationBuilders`.

### <a name="mode"></a>Режим

Построители конфигурации использование внешнего источника данных ключ/значение для заполнения элементов выбранный ключ значение конфигурации системы. В частности `<appSettings/>` и `<connectionStrings/>` разделы имеют специальную обработку из построителей конфигурации. Построители работать в трех режимах:

* `Strict` — Режим по умолчанию. В этом режиме построитель конфигурации работает только с хорошо известные ключ значение ориентированное и разделы конфигурации. `Strict` режим перечисляет каждый ключ раздела. Если найден соответствующий ключ из внешнего источника:

   * Построители конфигурации замените значение в результате раздел конфигурации со значением из внешнего источника.
* `Greedy` — В этом режиме тесно связано с `Strict` режим. Вместо того чтобы ограничения ключей, которые уже существуют в исходной конфигурации:

  * Построители конфигурации добавляет все пары "ключ/значение" из внешнего источника, в результате раздел конфигурации.

* `Expand` -Обрабатывает необработанный код XML, прежде чем оно разбивается на объект раздела конфигурации. Он может рассматриваться как расширение слова токенов в строке. Любую часть необработанные XML-строку, которая соответствует шаблону `${token}` является кандидатом на расширение токена. При обнаружении без соответствующего значения из внешнего источника маркера не изменяется. Построители в этом режиме не ограничиваются `<appSettings/>` и `<connectionStrings/>` разделы.

Следующая разметка из *web.config* позволяет [EnvironmentConfigBuilder](https://www.nuget.org/packages/Microsoft.Configuration.ConfigurationBuilders.Environment/) в `Strict` режиме:

[!code-xml[Main](config-builder/MyConfigBuilders/WebDefault.config?name=snippet)]

В следующем примере кода операций чтения `<appSettings/>` и `<connectionStrings/>` показан в предыдущем *web.config* файла:

[!code-csharp[Main](config-builder/MyConfigBuilders/About.aspx.cs)]

Приведенный выше код будет присвоено значения свойств:

* Значения в *web.config* файла, если ключи не заданы в переменных среды.
* Значения среды переменной, если задать.

Например `ServiceID` будет содержать:

* «Value ServiceID из файла web.config», если переменная среды `ServiceID` не задано.
* Значение `ServiceID` переменной, среды, если задать.

На следующем рисунке показана `<appSettings/>` ключей/значений из предыдущей *web.config* набор файлов в редакторе среды:

![редактор среды](config-builder/static/env.png)

Примечание: Может потребоваться закрыть и перезапустить Visual Studio, чтобы увидеть изменения в переменных среды.

### <a name="prefix-handling"></a>Обработка префикс

Префиксы ключа можно упростить ключи параметров, так как:

* Конфигурации платформы .NET Framework — сложный и вложенных.
* Источники внешних ключей и значений обычно представляют собой базовый и плоский по своей природе. Например переменные среды не являются вложенными.

Использовать любой из следующих методов для вставки оба `<appSettings/>` и `<connectionStrings/>` в конфигурацию через переменные среды:

* С помощью `EnvironmentConfigBuilder` в используемом по умолчанию `Strict` режим и соответствующие имена ключей в файле конфигурации. Предыдущий код и разметку принимает этот подход. При таком подходе вы можете **не** одинаковые имена ключей в обоих `<appSettings/>` и `<connectionStrings/>`.
* Используйте два `EnvironmentConfigBuilder`s в `Greedy` режиме с помощью различных префиксов и `stripPrefix`. В этом случае приложение может считывать `<appSettings/>` и `<connectionStrings/>` без необходимости обновления файла конфигурации. Следующий раздел, [stripPrefix](#stripprefix), показано, как это сделать.
* Используйте два `EnvironmentConfigBuilder`s в `Greedy` режиме с помощью различных префиксов. При таком подходе не может иметь повторяющиеся имена ключей, так как имена ключей должны отличаться по префиксу.  Пример:

[!code-xml[Main](config-builder/MyConfigBuilders/WebPrefix.config?name=snippet&highlight=11-99)]

С помощью предыдущего исправления можно использовать один и тот же источник неструктурированного ключ значение для заполнения конфигурации в двух разных разделах.

На следующем рисунке показана `<appSettings/>` и `<connectionStrings/>` ключей/значений из предыдущей *web.config* набор файлов в редакторе среды:

![редактор среды](config-builder/static/prefix.png)

В следующем примере кода операций чтения `<appSettings/>` и `<connectionStrings/>` ключи/значения, содержащиеся в предыдущем *web.config* файла:

[!code-csharp[Main](config-builder/MyConfigBuilders/Contact.aspx.cs?name=snippet)]

Приведенный выше код будет присвоено значения свойств:

* Значения в *web.config* файла, если ключи не заданы в переменных среды.
* Значения среды переменной, если задать.

Например, с помощью предыдущего *web.config* файл, заданы ключи/значения на предыдущем рисунке редактор среды и предыдущий код, следующие значения:

|  Ключ              | Значение |
| ----------------- | ------------ |
|     AppSetting_ServiceID           | AppSetting_ServiceID из переменных среды|
|    AppSetting_default            | Значение AppSetting_default из env |
|       ConnStr_default         | Val ConnStr_default из env|

### <a name="stripprefix"></a>stripPrefix

`stripPrefix`: логическое значение, по умолчанию равно `false`. 

Предыдущая разметка XML, разделяет параметры приложения из строки подключения, но требует все ключи в *web.config* файл для использования указанного префикса. Например, префикс `AppSetting` должны добавляться `ServiceID` ключ («AppSetting_ServiceID»). С помощью `stripPrefix`, префикс не используется в *web.config* файл. Префикс необходим в источнике построитель конфигурации (например, в среде.) Мы полагаем, что большинство разработчиков будет использовать `stripPrefix`.

Приложения обычно отбросить префикс. Следующие *web.config* отделяет префикс:

[!code-xml[Main](config-builder/MyConfigBuilders/WebPrefixStrip.config?name=snippet&highlight=14,19)]

В предыдущем *web.config* файл, `default` ключа входит в обе `<appSettings/>` и `<connectionStrings/>`.

На следующем рисунке показана `<appSettings/>` и `<connectionStrings/>` ключей/значений из предыдущей *web.config* набор файлов в редакторе среды:

![редактор среды](config-builder/static/prefix.png)

В следующем примере кода операций чтения `<appSettings/>` и `<connectionStrings/>` ключи/значения, содержащиеся в предыдущем *web.config* файла:

[!code-csharp[Main](config-builder/MyConfigBuilders/About2.aspx.cs?name=snippet)]

Приведенный выше код будет присвоено значения свойств:

* Значения в *web.config* файла, если ключи не заданы в переменных среды.
* Значения среды переменной, если задать.

Например, с помощью предыдущего *web.config* файл, заданы ключи/значения на предыдущем рисунке редактор среды и предыдущий код, следующие значения:

|  Ключ              | Значение |
| ----------------- | ------------ |
|     ServiceID           | AppSetting_ServiceID из переменных среды|
|    default            | Значение AppSetting_default из env |
|    default         | Val ConnStr_default из env|

### <a name="tokenpattern"></a>tokenPattern

`tokenPattern`: Строка, значение по умолчанию — `@"\$\{(\w+)\}"`

`Expand` Поведение из построителей осуществляет необработанный код XML для токенов, которые выглядят как `${token}`. Поиск выполняется с помощью регулярного выражения по умолчанию `@"\$\{(\w+)\}"`. Набор символов, соответствующий `\w` является более строгой, чем XML и множества источников конфигурации. Используйте `tokenPattern` при больше символов, чем `@"\$\{(\w+)\}"` требуются имя токена.

`tokenPattern`: Строка:

* Разработчики могут изменить регулярных выражений, который используется для поиска маркера.
* Чтобы убедиться в том, что это регулярное выражение правильного формата, отличные от опасных не проверяются.
* Он должен содержать группы записи. Всего регулярное выражение должно соответствовать весь токен. Первой записи должно быть имя токена, который ищется в файле конфигурации.

## <a name="configuration-builders-in-microsoftconfigurationconfigurationbuilders"></a>Построители конфигурации в Microsoft.Configuration.ConfigurationBuilders

### <a name="environmentconfigbuilder"></a>EnvironmentConfigBuilder

```xml
<add name="Environment"
    [mode|prefix|stripPrefix|tokenPattern] 
    type="Microsoft.Configuration.ConfigurationBuilders.EnvironmentConfigBuilder,
    Microsoft.Configuration.ConfigurationBuilders.Environment" />
```

[EnvironmentConfigBuilder](https://www.nuget.org/packages/Microsoft.Configuration.ConfigurationBuilders.Environment/):

* Это самый простой из построителей конфигурации.
* Считывает значения из среды.
* Не поддерживает любые дополнительные параметры конфигурации.
* `name` Значение атрибута является произвольным.

**Примечание:** в контейнерной среде Windows, переменные, заданные во время выполнения только вводится в среде процесса точки входа. Приложения, выполняются как службы или процесса не EntryPoint не получат эти переменные, если только они внедряются в противном случае через механизм в контейнере. Для [IIS](https://github.com/Microsoft/iis-docker/pull/41)/[ASP.NET](https://github.com/Microsoft/aspnet-docker)-контейнеров, текущая версия на основе [ServiceMonitor.exe](https://github.com/Microsoft/iis-docker/pull/41) обрабатывает это в *DefaultAppPool* только. Другие варианты контейнера на основе Windows может потребоваться разработать собственный механизм внедрения для процессов без точки входа.

### <a name="usersecretsconfigbuilder"></a>UserSecretsConfigBuilder

> [!WARNING]
> Никогда не хранить пароли, строки соединения конфиденциальные или других конфиденциальных данных в исходном коде. Не следует использовать секреты рабочей среды для разработки или тестирования.

```xml
<add name="UserSecrets"
    [mode|prefix|stripPrefix|tokenPattern]
    (userSecretsId="{secret string, typically a GUID}" | userSecretsFile="~\secrets.file")
    [optional="true"]
    type="Microsoft.Configuration.ConfigurationBuilders.UserSecretsConfigBuilder,
    Microsoft.Configuration.ConfigurationBuilders.UserSecrets" />
```

Этот построитель конфигурации предоставляет аналогичную функцию [менеджера секретов ASP.NET Core](/aspnet/core/security/app-secrets).

[UserSecretsConfigBuilder](https://www.nuget.org/packages/Microsoft.Configuration.ConfigurationBuilders.UserSecrets/) можно использовать в проектах .NET Framework, но должен быть указан файл секретов. Кроме того, можно определить `UserSecretsId` свойство в проекте файл и создает файл необработанных секретов в нужное место для чтения. Чтобы сохранить внешние зависимости из проекта, файл с секретом является формате XML. Форматирование XML-кода — это деталь реализации, а формат не следует полагаться на. Если вам нужно совместно использовать *secrets.json* файл с проектами .NET Core, рассмотрите возможность использования [SimpleJsonConfigBuilder](#simplejsonconfig). `SimpleJsonConfigBuilder` Форматирования для .NET Core следует также рассматривать деталь реализации может быть изменена.

Атрибуты конфигурации для `UserSecretsConfigBuilder`:

* `userSecretsId` -Это предпочтительный способ идентификации секретные данные в XML-файл. Он работает аналогично в .NET Core, который использует `UserSecretsId` свойство для хранения этого идентификатора проекта. Строка должна быть уникальной, он не обязательно должен быть GUID. С этим атрибутом `UserSecretsConfigBuilder` искать в хорошо известных локальное расположение (`%APPDATA%\Microsoft\UserSecrets\<UserSecrets Id>\secrets.xml`) для файла секреты, принадлежащего этот идентификатор.
* `userSecretsFile` — Необязательный атрибут, указав файл, содержащий секреты. `~` Символ в начале может использоваться для ссылки на корневой каталог приложения. Либо данный атрибут или `userSecretsId` атрибут является обязательным. Если указаны оба, `userSecretsFile` имеет более высокий приоритет.
* `optional`: логическое значение, значение по умолчанию `true` -исключение, если не удается найти файл секретов. 
* `name` Значение атрибута является произвольным.

Файл секретов имеет следующий формат:

```xml
<?xml version="1.0" encoding="utf-8" ?>
<root>
  <secrets ver="1.0">
    <secret name="secret key name" value="secret value" />
  </secrets>
</root>
```

### <a name="azurekeyvaultconfigbuilder"></a>AzureKeyVaultConfigBuilder

```xml
<add name="AzureKeyVault"
    [mode|prefix|stripPrefix|tokenPattern]
    (vaultName="MyVaultName" |
     uri="https:/MyVaultName.vault.azure.net")
    [connectionString="connection string"]
    [version="secrets version"]
    [preloadSecretNames="true"]
    type="Microsoft.Configuration.ConfigurationBuilders.AzureKeyVaultConfigBuilder,
    Microsoft.Configuration.ConfigurationBuilders.Azure" />
```

[AzureKeyVaultConfigBuilder](https://www.nuget.org/packages/Microsoft.Configuration.ConfigurationBuilders.Azure/) считывает значения, хранящиеся в [Azure Key Vault](/azure/key-vault/key-vault-whatis).

`vaultName` — требуется либо (имя хранилища), либо URI в хранилище. Другие атрибуты элемента управления, о каком хранилище для подключения к, но являются только в том случае, если приложение не выполняется в среде, которая работает с `Microsoft.Azure.Services.AppAuthentication`. Библиотека проверки подлинности служб Azure позволяет автоматически загружать сведения о соединении из среды выполнения, если это возможно. Вы можете переопределить автоматически получить сведения о соединении, предоставляя строку подключения.

* `vaultName` — Обязательный параметр, если `uri` в не предоставляется. Задает имя хранилища в подписке Azure, из которого считывается пары "ключ значение".
* `connectionString` -Строку подключения можно использовать с [AzureServiceTokenProvider](https://docs.microsoft.com/en-us/azure/key-vault/service-to-service-authentication#connection-string-support)
* `uri` — Подключается к другим поставщикам хранилища ключей с указанным `uri` значение. Если не указано, Azure (`vaultName`) является поставщиком хранилища.
* `version` — Для azure Key Vault предоставляет возможность управления версиями для секретов. Если `version` указан, сборщик только извлекает секретные данные, соответствующие этой версии.
* `preloadSecretNames` — По умолчанию, этот построитель querys **все** имен в хранилище ключей разделов, при инициализации. Чтобы предотвратить чтение всех значений ключа, установите значение `false`. Этому параметру `false` считывает секреты одному за раз. Чтение одного объекта одновременно может оказаться полезным, если хранилище позволяет доступа «Get», но не «список» доступ к операциям с секретами. **Примечание:** при использовании `Greedy` режиме `preloadSecretNames` должно быть `true` (по умолчанию).

### <a name="keyperfileconfigbuilder"></a>KeyPerFileConfigBuilder

```xml
<add name="KeyPerFile"
    [mode|prefix|stripPrefix|tokenPattern]
    (directoryPath="PathToSourceDirectory")
    [ignorePrefix="ignore."]
    [keyDelimiter=":"]
    [optional="false"]
    type="Microsoft.Configuration.ConfigurationBuilders.KeyPerFileConfigBuilder,
    Microsoft.Configuration.ConfigurationBuilders.KeyPerFile" />
```

[KeyPerFileConfigBuilder](https://www.nuget.org/packages/Microsoft.Configuration.ConfigurationBuilders.KeyPerFile/) — это построитель базовой конфигурации, который использует файлы каталога в качестве источника значений. Имя файла является ключом, и содержимое значение. Этот построитель конфигурации можно использовать при работе в среде организованным контейнера. Системы, такие как Docker Swarm и Kubernetes предоставляют `secrets` на их контейнеры организованным windows таким образом ключ каждого файла.

Сведения об атрибуте:

* `directoryPath` — обязательный. Указывает путь для поиска значения. Docker для Windows, секреты хранятся в *C:\ProgramData\Docker\secrets* каталог по умолчанию.
* `ignorePrefix` -Исключаются файлы, которые начинаются с этого префикса. По умолчанию «ignore».
* `keyDelimiter` -Значение по умолчанию — `null`. Если указано, построитель конфигурации проходит через несколько уровней в каталоге, создавая имена ключей, используя этот разделитель. Если это значение равно `null`, построитель конфигурации осуществляет только на верхнем уровне каталога.
* `optional` -Значение по умолчанию — `false`. Указывает, должен ли построитель конфигурации вызвать ошибки, если исходный каталог не существует.

### <a name="simplejsonconfigbuilder"></a>SimpleJsonConfigBuilder

> [!WARNING]
> Никогда не хранить пароли, строки соединения конфиденциальные или других конфиденциальных данных в исходном коде. Не следует использовать секреты рабочей среды для разработки или тестирования.

```xml
<add name="SimpleJson"
    [mode|prefix|stripPrefix|tokenPattern]
    jsonFile="~\config.json"
    [optional="true"]
    [jsonMode="(Flat|Sectional)"]
    type="Microsoft.Configuration.ConfigurationBuilders.SimpleJsonConfigBuilder,
    Microsoft.Configuration.ConfigurationBuilders.Json" />
```

Проекты .NET core часто используется JSON-файлов конфигурации. [SimpleJsonConfigBuilder](https://www.nuget.org/packages/Microsoft.Configuration.ConfigurationBuilders.Json/) builder позволяет .NET Core JSON-файлов для использования в .NET Framework. Этот построитель конфигурации — это обеспечивает основные сопоставление из источника неструктурированного ключ/значение в области определенной ключ значение конфигурации .NET Framework. Делает этот конфигурации **не** обеспечения иерархической конфигурации. Резервный файл JSON аналогична словарь, не сложные иерархические объект. Можно использовать многоуровневые иерархические файл. Этот поставщик `flatten`s глубина путем добавления имени свойства на каждом уровне с помощью `:` как разделитель.

Сведения об атрибуте:

* `jsonFile` — обязательный. Задает файл для чтения из JSON. `~` Символ в начале может использоваться для ссылки на корня приложения.
* `optional` -Логическое, значение по умолчанию — `true`. Предотвращает создание исключений, если не удается найти файл JSON.
* `jsonMode` - `[Flat|Sectional]`. Значение по умолчанию — `Flat`. Когда `jsonMode` является `Flat`, JSON-файл является источником одной неструктурированный ключ значение. `EnvironmentConfigBuilder` И `AzureKeyVaultConfigBuilder` также являются источники неструктурированных один ключ значение. Когда `SimpleJsonConfigBuilder` настраивается в `Sectional` режиме:

  * JSON-файл делится только на верхнем уровне несколько словарей.
  * Каждый из словари применяется только в раздел конфигурации, который соответствует имени свойства верхнего уровня, присоединенными к ним. Пример:

```json
    {
        "appSettings" : {
            "setting1" : "value1",
            "setting2" : "value2",
            "complex" : {
                "setting1" : "complex:value1",
                "setting2" : "complex:value2",
            }
        }
    }
```

## <a name="implementing-a-custom-keyvalue-configuration-builder"></a>Реализация построителя конфигурации пользовательский ключ значение

Если построителей конфигурации не соответствуют требованиям, можно создать новый. `KeyValueConfigBuilder` Базовый класс обрабатывает режимы подстановки и большинство проблем префикс. Требуются только проекте реализации:

* Наследовать от базового класса и реализации основных источника пар "ключ значение" через `GetValue` и `GetAllValues`:
* Добавить [Microsoft.Configuration.ConfigurationBuilders.Base](https://www.nuget.org/packages/Microsoft.Configuration.ConfigurationBuilders.Base/) в проект.

[!code-csharp[Main](config-builder/MyConfigBuilders/MyCustomConfigBuilder.cs)]

`KeyValueConfigBuilder` Базовый класс предоставляет большую часть рабочих и согласованное поведение через ключ/значение построителей конфигурации.

## <a name="additional-resources"></a>Дополнительные ресурсы

* [Репозиторий GitHub построители конфигурации](https://github.com/aspnet/MicrosoftConfigurationBuilders)
* [Служба служба проверки подлинности в хранилище ключей Azure с помощью .NET](/azure/key-vault/service-to-service-authentication#connection-string-support)
