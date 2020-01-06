---
title: Поставщик конфигурации Azure Key Vault в ASP.NET Core
author: guardrex
description: Узнайте, как использовать поставщик конфигурации Azure Key Vault для настройки приложения с помощью пар "имя-значение", загружаемых во время выполнения.
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.custom: mvc
ms.date: 12/16/2019
uid: security/key-vault-configuration
ms.openlocfilehash: 37ba756cc4170c145d2ab1f9f0a465057cc826c1
ms.sourcegitcommit: 2cb857f0de774df421e35289662ba92cfe56ffd1
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 12/25/2019
ms.locfileid: "75358712"
---
# <a name="azure-key-vault-configuration-provider-in-aspnet-core"></a>Поставщик конфигурации Azure Key Vault в ASP.NET Core

[Люк ЛаСаМ](https://github.com/guardrex) и [Эндрю Стантон-медперсонала](https://github.com/anurse)

В этом документе объясняется, как использовать поставщик конфигурации [Microsoft Azure Key Vault](https://azure.microsoft.com/services/key-vault/) для загрузки значений конфигурации приложения из Azure Key Vault секреты. Azure Key Vault — это облачная служба, которая помогает защитить криптографические ключи и секреты, используемые приложениями и службами. Распространенные сценарии использования Azure Key Vault с приложениями ASP.NET Core:

* Управление доступом к конфиденциальным данным конфигурации.
* Соблюдайте требования для FIPS 140-2 уровня 2 проверенных аппаратных модулей безопасности (HSM) при хранении данных конфигурации.

[Просмотреть или скачать образец кода](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/security/key-vault-configuration/samples) ([как скачивать](xref:index#how-to-download-a-sample))

## <a name="packages"></a>пакеты,

Добавьте ссылку на пакет в пакет [Microsoft. Extensions. Configuration. AzureKeyVault](https://www.nuget.org/packages/Microsoft.Extensions.Configuration.AzureKeyVault/) .

## <a name="sample-app"></a>Пример приложения

Пример приложения выполняется в одном из двух режимов, определенных инструкцией `#define` в верхней части файла *Program.CS* :

* `Certificate` &ndash; демонстрирует использование идентификатора клиента Azure Key Vault и сертификата X. 509 для доступа к секретам, хранящимся в Azure Key Vault. Эту версию примера можно запустить из любого расположения, развернуть в службе приложений Azure или на любом узле, способном обслуживать ASP.NET Coreное приложение.
* `Managed` &ndash; демонстрируется использование [управляемых удостоверений для ресурсов Azure](/azure/active-directory/managed-identities-azure-resources/overview) для проверки подлинности приложения при Azure Key Vault с проверкой подлинности Azure AD без учетных данных, хранящихся в коде или конфигурации приложения. При использовании управляемых удостоверений для проверки подлинности не требуется идентификатор и пароль приложения Azure AD (секрет клиента). Версия `Managed` образца должна быть развернута в Azure. Следуйте указаниям в разделе [Использование управляемых удостоверений для ресурсов Azure](#use-managed-identities-for-azure-resources) .

Дополнительные сведения о настройке примера приложения с помощью директив препроцессора (`#define`) см. в разделе <xref:index#preprocessor-directives-in-sample-code>.

## <a name="secret-storage-in-the-development-environment"></a>Секретное хранилище в среде разработки

Локальное задание секретов с помощью [средства диспетчера секретов](xref:security/app-secrets). Когда пример приложения выполняется на локальном компьютере в среде разработки, секреты загружаются из локального хранилища диспетчера секретов.

Для средства диспетчера секретов требуется свойство `<UserSecretsId>` в файле проекта приложения. Задайте для свойства значение (`{GUID}`) любой уникальный GUID:

```xml
<PropertyGroup>
  <UserSecretsId>{GUID}</UserSecretsId>
</PropertyGroup>
```

Секреты создаются как пары "имя-значение". Иерархические значения (разделы конфигурации) используют `:` (двоеточие) в качестве разделителя в именах [ASP.NET Core ключей конфигурации](xref:fundamentals/configuration/index) .

Диспетчер секретов используется из командной оболочки, открытой в [корне содержимого](xref:fundamentals/index#content-root)проекта, где `{SECRET NAME}` — это имя, а `{SECRET VALUE}` — значение.

```dotnetcli
dotnet user-secrets set "{SECRET NAME}" "{SECRET VALUE}"
```

Выполните следующие команды в командной оболочке из [корневого каталога содержимого](xref:fundamentals/index#content-root) проекта, чтобы задать секреты для примера приложения:

```dotnetcli
dotnet user-secrets set "SecretName" "secret_value_1_dev"
dotnet user-secrets set "Section:SecretName" "secret_value_2_dev"
```

Если эти секреты хранятся в Azure Key Vault в [хранилище секретов в рабочей среде с Azure Key Vaultным](#secret-storage-in-the-production-environment-with-azure-key-vault) разделом, суффикс `_dev` меняется на `_prod`. Суффикс предоставляет визуальную подсказку в выходных данных приложения, указывающую источник значений конфигурации.

## <a name="secret-storage-in-the-production-environment-with-azure-key-vault"></a>Секретное хранилище в рабочей среде с Azure Key Vault

Инструкции, приведенные в [кратком руководстве по установке и извлечению секретов из Azure Key Vault с помощью Azure CLI](/azure/key-vault/quick-create-cli) разделе, приведены здесь для создания Azure Key Vault и хранения секретов, используемых в примере приложения. Дополнительные сведения см. в разделе.

1. Откройте Azure Cloud Shell одним из следующих способов в [портал Azure](https://portal.azure.com/):

   * Нажмите кнопку **Попробовать** в правом верхнем углу блока с кодом. Используйте строку поиска "Azure CLI" в текстовом поле.
   * Откройте Cloud Shell в браузере с помощью кнопки **запустить Cloud Shell** .
   * Нажмите кнопку меню **Cloud Shell** в правом верхнем углу окна портала Azure.

   Дополнительные сведения см. в статьях [интерфейс командной строки Azure (CLI)](/cli/azure/) и [общие сведения о Azure Cloud Shell](/azure/cloud-shell/overview).

1. Если вы еще не прошли проверку подлинности, выполните вход с помощью команды `az login`.

1. Создайте группу ресурсов с помощью следующей команды, где `{RESOURCE GROUP NAME}` — это имя группы ресурсов для новой группы ресурсов, а `{LOCATION}` — регион Azure (центр обработки данных):

   ```azure-cli
   az group create --name "{RESOURCE GROUP NAME}" --location {LOCATION}
   ```

1. Создайте хранилище ключей в группе ресурсов с помощью следующей команды, где `{KEY VAULT NAME}` — это имя нового хранилища ключей, а `{LOCATION}` — регион Azure (центр обработки данных):

   ```azure-cli
   az keyvault create --name {KEY VAULT NAME} --resource-group "{RESOURCE GROUP NAME}" --location {LOCATION}
   ```

1. Создайте секреты в хранилище ключей в виде пар "имя-значение".

   Azure Key Vault имена секретов ограничены буквенно-цифровыми символами и тире. Иерархические значения (разделы конфигурации) используют `--` (два тире) в качестве разделителя. Двоеточия, которые обычно используются для разделения раздела из подраздела в [ASP.NET Core конфигурации](xref:fundamentals/configuration/index), не допускаются в именах секретов хранилища ключей. Поэтому при загрузке секретов в конфигурацию приложения используются два дефиса и они переключаются на двоеточие.

   Следующие секреты предназначены для использования с примером приложения. Значения включают суффикс `_prod`, чтобы отличить их от `_dev`ных суффиксов, загруженных в среде разработки из секретов пользователя. Замените `{KEY VAULT NAME}` именем хранилища ключей, созданным на предыдущем шаге:

   ```azure-cli
   az keyvault secret set --vault-name {KEY VAULT NAME} --name "SecretName" --value "secret_value_1_prod"
   az keyvault secret set --vault-name {KEY VAULT NAME} --name "Section--SecretName" --value "secret_value_2_prod"
   ```

## <a name="use-application-id-and-x509-certificate-for-non-azure-hosted-apps"></a>Использование идентификатора приложения и сертификата X. 509 для приложений, не размещенных в Azure

Настройте Azure AD, Azure Key Vault и приложение для использования идентификатора Azure Active Directory приложения и сертификата X. 509 для проверки подлинности в хранилище ключей, **Если приложение размещено за пределами Azure**. См. дополнительные сведения о [ключах, секретах и сертификатах](/azure/key-vault/about-keys-secrets-and-certificates).

> [!NOTE]
> Хотя использование идентификатора приложения и сертификата X. 509 поддерживается для приложений, размещенных в Azure, мы рекомендуем использовать [управляемые удостоверения для ресурсов Azure](#use-managed-identities-for-azure-resources) при размещении приложения в Azure. Для управляемых удостоверений не требуется хранить сертификат в приложении или в среде разработки.

В примере приложения используется идентификатор приложения и сертификат X. 509, если в инструкции `#define` в верхней части файла *Program.CS* установлено значение `Certificate`.

1. Создайте архивный сертификат PKCS # 12 (*PFX*-файл). Для создания сертификатов можно использовать [MakeCert в Windows](/windows/desktop/seccrypto/makecert) и [OpenSSL](https://www.openssl.org/).
1. Установите сертификат в личное хранилище сертификатов текущего пользователя. Пометка ключа как экспортируемого необязательна. Запишите отпечаток сертификата, который будет использоваться далее в этом процессе.
1. Экспортируйте архивный сертификат PKCS # 12 (*PFX*) как сертификат в формате DER (*CER*).
1. Зарегистрируйте приложение в Azure AD (**Регистрация приложений**).
1. Отправьте сертификат в кодировке DER (*CER*) в Azure AD:
   1. Выберите приложение в Azure AD.
   1. Перейдите к разделу **сертификаты & секреты**.
   1. Выберите **отправить сертификат** , чтобы отправить сертификат, который содержит открытый ключ. Сертификат *. cer*, *. pem*или *. CRT* является приемлемым.
1. Сохраните имя хранилища ключей, идентификатор приложения и отпечаток сертификата в файле *appSettings. JSON* приложения.
1. Перейдите к **разделу хранилища ключей** в портал Azure.
1. Выберите хранилище ключей, созданное в [хранилище секретов в рабочей среде с помощью Azure Key Vault](#secret-storage-in-the-production-environment-with-azure-key-vault) разделе.
1. Выберите **Политики доступа**.
1. Выберите **Добавить политику доступа**.
1. Откройте **разрешения для секрета** и предоставьте приложению разрешения **Get** и **List** .
1. Щелкните **выбрать субъект** и выберите зарегистрированное приложение по имени. Нажмите кнопку **Выбрать**.
1. Нажмите кнопку **ОК**.
1. Нажмите кнопку **Сохранить**.
1. Разверните приложение.

`Certificate` пример приложения получает значения конфигурации из `IConfigurationRoot` с тем же именем, что и имя секрета:

* Неиерархические значения: значение для `SecretName` получено с `config["SecretName"]`.
* Иерархические значения (разделы): используйте нотацию `:` (двоеточие) или метод расширения `GetSection`. Используйте любой из этих подходов для получения значения конфигурации:
  * `config["Section:SecretName"]`
  * `config.GetSection("Section")["SecretName"]`

Сертификат X. 509 управляется ОС. Приложение вызывает <xref:Microsoft.Extensions.Configuration.AzureKeyVaultConfigurationExtensions.AddAzureKeyVault*> со значениями, предоставленными файлом *appSettings. JSON* :

::: moniker range=">= aspnetcore-3.0"

[!code-csharp[](key-vault-configuration/samples/3.x/SampleApp/Program.cs?name=snippet1&highlight=20-23)]

::: moniker-end

::: moniker range="< aspnetcore-3.0"

[!code-csharp[](key-vault-configuration/samples/2.x/SampleApp/Program.cs?name=snippet1&highlight=20-23)]

::: moniker-end

Примеры значений

* Имя хранилища ключей: `contosovault`
* Идентификатор приложения: `627e911e-43cc-61d4-992e-12db9c81b413`
* Отпечаток сертификата: `fe14593dd66b2406c5269d742d04b6e1ab03adb1`

*appsettings.json*:

::: moniker range=">= aspnetcore-3.0"

[!code-json[](key-vault-configuration/samples/3.x/SampleApp/appsettings.json?highlight=10-12)]

::: moniker-end

::: moniker range="< aspnetcore-3.0"

[!code-json[](key-vault-configuration/samples/2.x/SampleApp/appsettings.json?highlight=10-12)]

::: moniker-end

При запуске приложения на веб-странице отображаются загруженные значения секрета. В среде разработки значения секрета загружаются с суффиксом `_dev`. В рабочей среде значения загружаются с суффиксом `_prod`.

## <a name="use-managed-identities-for-azure-resources"></a>Использование управляемых удостоверений для ресурсов Azure

**Приложение, развернутое в Azure** , может использовать преимущества [управляемых удостоверений для ресурсов Azure](/azure/active-directory/managed-identities-azure-resources/overview), что позволяет приложению проходить проверку подлинности в Azure Key Vault с помощью проверки подлинности Azure AD без учетных данных (идентификатор приложения и пароль/секрет клиента), хранимых в приложении.

Пример приложения использует управляемые удостоверения для ресурсов Azure, когда инструкция `#define` в верхней части файла *Program.CS* имеет значение `Managed`.

Введите имя хранилища в файл *appSettings. JSON* приложения. В примере приложения не требуется идентификатор приложения и пароль (секрет клиента), если задана версия `Managed`, поэтому эти записи конфигурации можно игнорировать. Приложение развертывается в Azure, а Azure выполняет проверку подлинности приложения для доступа к Azure Key Vault только с использованием имени хранилища, хранящегося в файле *appSettings. JSON* .

Разверните пример приложения в службе приложений Azure.

Приложение, развернутое в службе приложений Azure, автоматически регистрируется в Azure AD при создании службы. Получите идентификатор объекта из развертывания для использования в следующей команде. Идентификатор объекта отображается в портал Azure на панели **удостоверений** службы приложений.

Используя Azure CLI и идентификатор объекта приложения, укажите приложение с разрешениями `list` и `get` для доступа к хранилищу ключей:

```azure-cli
az keyvault set-policy --name {KEY VAULT NAME} --object-id {OBJECT ID} --secret-permissions get list
```

**Перезапустите приложение** с помощью Azure CLI, PowerShell или портал Azure.

Пример приложения:

* Создает экземпляр класса `AzureServiceTokenProvider` без строки подключения. Если строка подключения не указана, поставщик пытается получить маркер доступа из управляемых удостоверений для ресурсов Azure.
* Новый <xref:Microsoft.Azure.KeyVault.KeyVaultClient> создается с помощью обратного вызова маркера экземпляра `AzureServiceTokenProvider`.
* <xref:Microsoft.Azure.KeyVault.KeyVaultClient> экземпляр используется с реализацией по умолчанию <xref:Microsoft.Extensions.Configuration.AzureKeyVault.IKeyVaultSecretManager>, которая загружает все значения секрета и заменяет двойные тире (`--`) двоеточием (`:`) в именах ключей.

::: moniker range=">= aspnetcore-3.0"

[!code-csharp[](key-vault-configuration/samples/3.x/SampleApp/Program.cs?name=snippet2&highlight=13-21)]

::: moniker-end

::: moniker range="< aspnetcore-3.0"

[!code-csharp[](key-vault-configuration/samples/2.x/SampleApp/Program.cs?name=snippet2&highlight=13-21)]

::: moniker-end

Пример значения имени хранилища ключей: `contosovault`
    
*appsettings.json*:

```json
{
  "KeyVaultName": "Key Vault Name"
}
```

При запуске приложения на веб-странице отображаются загруженные значения секрета. В среде разработки значения секрета имеют суффикс `_dev`, так как они предоставляются секретами пользователя. В рабочей среде значения загружаются с суффиксом `_prod`, так как они предоставляются Azure Key Vault.

При возникновении ошибки `Access denied` убедитесь, что приложение зарегистрировано в Azure AD и предоставил доступ к хранилищу ключей. Убедитесь, что вы перезапустите службу в Azure.

Сведения об использовании поставщика с управляемым удостоверением и конвейером Azure DevOps см. в статье [Создание подключения Azure Resource Manager службы к виртуальной машине с помощью управляемого удостоверения службы](/azure/devops/pipelines/library/connect-to-azure#create-an-azure-resource-manager-service-connection-to-a-vm-with-a-managed-service-identity).

::: moniker range=">= aspnetcore-3.0"

## <a name="configuration-options"></a>Параметры конфигурации

<xref:Microsoft.Extensions.Configuration.AzureKeyVaultConfigurationExtensions.AddAzureKeyVault*> может принимать <xref:Microsoft.Extensions.Configuration.AzureKeyVault.AzureKeyVaultConfigurationOptions>:

```csharp
config.AddAzureKeyVault(
    new AzureKeyVaultConfigurationOptions()
    {
        ...
    });
```

| Идентификаторы         | Описание |
| ---------------- | ----------- |
| `Client`         | <xref:Microsoft.Azure.KeyVault.KeyVaultClient>, используемый для получения значений. |
| `Manager`        | <xref:Microsoft.Extensions.Configuration.AzureKeyVault.IKeyVaultSecretManager> экземпляр, используемый для управления загрузкой секрета. |
| `ReloadInterval` | `Timespan` ожидания между попытками опроса хранилища ключей на предмет изменений. Значение по умолчанию — `null` (конфигурация не перегружается). |
| `Vault`          | URI хранилища ключей. |

::: moniker-end

## <a name="use-a-key-name-prefix"></a>Использование префикса имени ключа

<xref:Microsoft.Extensions.Configuration.AzureKeyVaultConfigurationExtensions.AddAzureKeyVault*> предоставляет перегрузку, которая принимает реализацию <xref:Microsoft.Extensions.Configuration.AzureKeyVault.IKeyVaultSecretManager>, которая позволяет управлять преобразованием секретов ключей хранилища в конфигурационные ключи. Например, можно реализовать интерфейс для загрузки секретных значений на основе значения префикса, которое вы задают при запуске приложения. Это позволяет, например, загружать секреты на основе версии приложения.

> [!WARNING]
> Не используйте префиксы в секретах хранилища ключей, чтобы размещать секреты для нескольких приложений в одном хранилище ключей или для размещения секретов среды (например, для *разработки* и *рабочих* секретов) в одном хранилище. Мы рекомендуем использовать отдельные хранилища ключей для различных приложений и сред разработки и рабочей среды, чтобы изолировать среды приложений для обеспечения наивысшего уровня безопасности.

В следующем примере в хранилище ключей устанавливается секрет (и используется средство диспетчера секретов для среды разработки) для `5000-AppSecret` (точки не допускаются в именах секретов хранилища ключей). Этот секрет представляет секрет приложения для версии 5.0.0.0 приложения. Для другой версии приложения 5.1.0.0 секрет добавляется в хранилище ключей (и с помощью средства диспетчера секретов) для `5100-AppSecret`. Каждая версия приложения загружает секретное значение в конфигурацию в виде `AppSecret`, отключая версию при загрузке секрета.

<xref:Microsoft.Extensions.Configuration.AzureKeyVaultConfigurationExtensions.AddAzureKeyVault*> вызывается с настраиваемым <xref:Microsoft.Extensions.Configuration.AzureKeyVault.IKeyVaultSecretManager>:

[!code-csharp[](key-vault-configuration/samples_snapshot/Program.cs)]

<xref:Microsoft.Extensions.Configuration.AzureKeyVault.IKeyVaultSecretManager>ная реализация реагирует на префиксы версий секретов, чтобы загрузить правильный секрет в конфигурацию:

* `Load` загружает секрет, если его имя начинается с префикса. Другие секреты не загружены.
* `GetKey`.
  * Удаляет префикс из имени секрета.
  * Заменяет два тире в любом имени на `KeyDelimiter`, который является разделителем, используемым в конфигурации (обычно это двоеточие). Azure Key Vault не допускает двоеточия в именах секретов.

[!code-csharp[](key-vault-configuration/samples_snapshot/Startup.cs)]

Метод `Load` вызывается алгоритмом поставщика, который выполняет перебор секретов хранилища для поиска тех, у которых есть префикс версии. При обнаружении префикса версии с `Load`алгоритм использует метод `GetKey`, чтобы получить имя конфигурации для имени секрета. Он отделяет префикс версии от имени секрета и возвращает оставшуюся часть имени секрета для загрузки в пары "имя-значение" конфигурации приложения.

При реализации этого подхода:

1. Версия приложения, указанная в файле проекта приложения. В следующем примере для версии приложения задано значение `5.0.0.0`:

   ```xml
   <PropertyGroup>
     <Version>5.0.0.0</Version>
   </PropertyGroup>
   ```

1. Убедитесь, что в файле проекта приложения имеется `<UserSecretsId>` свойство, где `{GUID}` является идентификатором GUID, предоставляемым пользователем:

   ```xml
   <PropertyGroup>
     <UserSecretsId>{GUID}</UserSecretsId>
   </PropertyGroup>
   ```

   Сохраните следующие секреты локально с помощью [средства диспетчера секретов](xref:security/app-secrets):

   ```dotnetcli
   dotnet user-secrets set "5000-AppSecret" "5.0.0.0_secret_value_dev"
   dotnet user-secrets set "5100-AppSecret" "5.1.0.0_secret_value_dev"
   ```

1. Секреты сохраняются в Azure Key Vault с помощью следующих Azure CLI команд:

   ```azure-cli
   az keyvault secret set --vault-name {KEY VAULT NAME} --name "5000-AppSecret" --value "5.0.0.0_secret_value_prod"
   az keyvault secret set --vault-name {KEY VAULT NAME} --name "5100-AppSecret" --value "5.1.0.0_secret_value_prod"
   ```

1. При запуске приложения загружаются секреты хранилища ключей. Строковый секрет для `5000-AppSecret` соответствует версии приложения, указанной в файле проекта приложения (`5.0.0.0`).

1. Версия `5000` (с тире) удаляется из имени ключа. На протяжении всего приложения чтение конфигурации с ключом `AppSecret` загружает секретное значение.

1. Если версия приложения изменена в файле проекта на `5.1.0.0` и приложение запускается снова, то возвращаемое значение секрета будет `5.1.0.0_secret_value_dev` в среде разработки и `5.1.0.0_secret_value_prod` в производстве.

> [!NOTE]
> Для <xref:Microsoft.Extensions.Configuration.AzureKeyVaultConfigurationExtensions.AddAzureKeyVault*>можно также предоставить собственную реализацию <xref:Microsoft.Azure.KeyVault.KeyVaultClient>. Настраиваемый клиент позволяет совместно использовать один экземпляр клиента в приложении.

## <a name="bind-an-array-to-a-class"></a>Привязка массива к классу

Поставщик может считывать значения конфигурации в массив для привязки к массиву POCO.

При чтении из источника конфигурации, позволяющего ключам содержать разделители с двоеточием (`:`), используется числовой сегмент ключа для различения ключей, составляющих массив (`:0:`, `:1:`,... `:{n}:`). Дополнительные сведения см. в разделе [Configuration: привязка массива к классу](xref:fundamentals/configuration/index#bind-an-array-to-a-class).

Ключи Azure Key Vault не могут использовать двоеточие в качестве разделителя. Описанный в этом разделе подход использует двойные тире (`--`) в качестве разделителя иерархических значений (разделов). Ключи массивов хранятся в Azure Key Vault с двойными тире и числовыми сегментами ключей (`--0--`, `--1--`, &hellip; `--{n}--`).

Изучите следующую конфигурацию поставщика ведения журнала [Serilog](https://serilog.net/) , предоставляемую файлом JSON. В массиве `WriteTo` определены два объектных литерала, отражающих два *приемника*Serilog, которые описывают назначения для вывода журнала:

```json
"Serilog": {
  "WriteTo": [
    {
      "Name": "AzureTableStorage",
      "Args": {
        "storageTableName": "logs",
        "connectionString": "DefaultEnd...ountKey=Eby8...GMGw=="
      }
    },
    {
      "Name": "AzureDocumentDB",
      "Args": {
        "endpointUrl": "https://contoso.documents.azure.com:443",
        "authorizationKey": "Eby8...GMGw=="
      }
    }
  ]
}
```

Конфигурация, показанная в предыдущем JSON-файле, хранится в Azure Key Vault с использованием нотации двойного тире (`--`) и числовых сегментов.

| Key | {2&gt;Value&lt;2} |
| --- | ----- |
| `Serilog--WriteTo--0--Name` | `AzureTableStorage` |
| `Serilog--WriteTo--0--Args--storageTableName` | `logs` |
| `Serilog--WriteTo--0--Args--connectionString` | `DefaultEnd...ountKey=Eby8...GMGw==` |
| `Serilog--WriteTo--1--Name` | `AzureDocumentDB` |
| `Serilog--WriteTo--1--Args--endpointUrl` | `https://contoso.documents.azure.com:443` |
| `Serilog--WriteTo--1--Args--authorizationKey` | `Eby8...GMGw==` |

## <a name="reload-secrets"></a>Перезагрузка секретов

Секреты кэшируются до тех пор, пока не будет вызван `IConfigurationRoot.Reload()`. Срок действия истек, отключен и обновленные секреты в хранилище ключей не учитываются приложением, пока не будет выполнено `Reload`.

```csharp
Configuration.Reload();
```

## <a name="disabled-and-expired-secrets"></a>Отключенные секреты и секреты с истекшим сроком действия

Отключенные и просроченные секреты создают <xref:Microsoft.Azure.KeyVault.Models.KeyVaultErrorException>. Чтобы предотвратить создание приложения, укажите конфигурацию с помощью другого поставщика конфигурации или обновите закрытый или истекший секрет.

## <a name="troubleshoot"></a>Устранение неполадок

Если приложению не удается загрузить конфигурацию с помощью поставщика, в [инфраструктуру ведения журнала ASP.NET Core](xref:fundamentals/logging/index)записывается сообщение об ошибке. При следующих условиях загрузка конфигурации будет запрещена:

* Приложение или сертификат неправильно настроены в Azure Active Directory.
* Хранилище ключей не существует в Azure Key Vault.
* Приложение не имеет прав доступа к хранилищу ключей.
* Политика доступа не включает разрешения `Get` и `List`.
* В хранилище ключей данные конфигурации (пара "имя-значение") неправильно именованы, отсутствуют, отключены или устарели.
* Приложение имеет неправильное имя хранилища ключей (`KeyVaultName`), идентификатор приложения Azure AD (`AzureADApplicationId`) или отпечаток сертификата Azure AD (`AzureADCertThumbprint`).
* Неверный ключ конфигурации (имя) в приложении для загружаемого значения.
* При добавлении политики доступа для приложения в хранилище ключей была создана политика, но кнопка **сохранить** не была выбрана в пользовательском интерфейсе **политик доступа** .

## <a name="additional-resources"></a>Дополнительные ресурсы

* <xref:fundamentals/configuration/index>
* [Microsoft Azure: Key Vault](https://azure.microsoft.com/services/key-vault/)
* [Microsoft Azure: Документация по Key Vault](/azure/key-vault/)
* [Создание и перенос ключей, защищенных АППАРАТным модулем безопасности, для Azure Key Vault](/azure/key-vault/key-vault-hsm-protected-keys)
* [Класс KeyVaultClient](/dotnet/api/microsoft.azure.keyvault.keyvaultclient)
* [Краткое руководство. Установка и получение секрета из Azure Key Vault с помощью веб-приложения .NET](/azure/key-vault/quick-create-net)
* [Руководство. Использование Azure Key Vault с виртуальной машиной Azure Windows в .NET](/azure/key-vault/tutorial-net-windows-virtual-machine)
