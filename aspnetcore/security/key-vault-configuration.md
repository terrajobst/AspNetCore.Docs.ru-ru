---
title: Поставщик конфигурации Azure Key Vault в ASP.NET Core
author: guardrex
description: Узнайте, как использовать поставщик конфигурации Azure Key Vault для настройки приложения с помощью пары "имя значение", загружен во время выполнения.
monikerRange: '>= aspnetcore-1.1'
ms.author: riande
ms.custom: mvc
ms.date: 10/24/2018
uid: security/key-vault-configuration
ms.openlocfilehash: 06445eb2ecec4cf101b23a4bfe131b2c56a18f62
ms.sourcegitcommit: 4d74644f11e0dac52b4510048490ae731c691496
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/25/2018
ms.locfileid: "50090310"
---
# <a name="azure-key-vault-configuration-provider-in-aspnet-core"></a>Поставщик конфигурации Azure Key Vault в ASP.NET Core

По [Люк Лэтем](https://github.com/guardrex) и [Andrew Stanton медсестра](https://github.com/anurse)

В этом документе объясняется, как использовать [Microsoft Azure Key Vault](https://azure.microsoft.com/services/key-vault/) поставщик конфигурации для загрузки значений конфигурации из секреты Azure Key Vault. Azure Key Vault — это облачная служба, которая помогает защитить криптографические ключи и секреты, используемые приложениями и службами. Обычные сценарии включают управление доступом к конфиденциальные данные конфигурации и соответствующие потребностям под стандарт FIPS 140-2 уровня 2 проверяются аппаратных модулей безопасности (HSM), при сохранении данных конфигурации. Эта функция доступна для приложений, предназначенных для ASP.NET Core 1.1 или более поздней версии.

[Просмотреть или скачать образец кода](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/key-vault-configuration/samples) ([как скачивать](xref:tutorials/index#how-to-download-a-sample))

## <a name="package"></a>Пакет

Чтобы воспользоваться поставщиком, добавьте ссылку на [Microsoft.Extensions.Configuration.AzureKeyVault](https://www.nuget.org/packages/Microsoft.Extensions.Configuration.AzureKeyVault/) пакета.

## <a name="app-configuration"></a>Конфигурация приложения

Вы можете изучить поставщика с [примеры приложений](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/key-vault-configuration/samples). После установления хранилища ключей и создайте секреты в хранилище, примеры приложений безопасно загрузить значения секретов в их конфигурации и отобразить их на веб-страницах.

Поставщик добавляется в конфигурацию приложения с `AddAzureKeyVault` расширения. В образце приложения, расширение использует три значения конфигурации, загруженного из *appsettings.json* файла.

| Параметр приложения    | Описание                    | Пример                                      |
| -------------- | ------------------------------ | -------------------------------------------- |
| `Vault`        | Имя хранилища ключей Azure           | contosovault                                 |
| `ClientId`     | Идентификатор приложения Azure Active Directory  | 627e911e-43cc-61d4-992e-12db9c81b413         |
| `ClientSecret` | Ключ приложения Azure Active Directory | g58K3dtg59o1Pa + e59v2Tx829w6VxTB2yv9sv/101di = |

[!code-csharp[Program](key-vault-configuration/samples/basic-sample/2.x/Program.cs?name=snippet1)]

## <a name="create-key-vault-secrets-and-load-configuration-values-basic-sample"></a>Создать секретных кодов хранилища ключей и загрузить значения конфигурации (basic образец)

1. Создание хранилища ключей и настроить Azure Active Directory (Azure AD) для приложении, следуя указаниям в [приступить к работе с хранилищем ключей Azure](https://azure.microsoft.com/documentation/articles/key-vault-get-started/).
   * Добавьте секреты в хранилище ключей с помощью [модуль PowerShell хранилища ключей AzureRM](/powershell/module/azurerm.keyvault) корпорации [коллекции PowerShell](https://www.powershellgallery.com/packages/AzureRM.KeyVault), [API REST хранилища ключей Azure](/rest/api/keyvault/), или [Портала azure](https://portal.azure.com/). Секреты создаются как *вручную* или *сертификат* секреты. *Сертификат* секретов, сертификатов для использования с приложениями и службами, но не поддерживаются поставщиком конфигурации. Следует использовать *вручную* параметр, чтобы создать пару "имя значение" секреты для использования при помощи поставщика конфигурации.
     * Простой секретные данные создаются в виде пар "имя значение". Azure имена секретов Key Vault ограничены буквы, цифры и дефисы.
     * Использование иерархических значений (разделы конфигурации) `--` (двух дефисов) как разделитель в образце. Имена, которые обычно используются для разделения разделе из подраздела в [конфигурации ASP.NET Core](xref:fundamentals/configuration/index), не разрешены в именах секрета. Таким образом двух дефисов используются и поменять местами двоеточие, при загрузке секреты в конфигурацию приложения.
     * Создайте две *вручную* секретные сведения с помощью следующие пары "имя значение". Первой secret — это простое имя и значение, и второй секретный код создает секретное значение с раздел и раздел имя секрета:
       * `SecretName`: `secret_value_1`
       * `Section--SecretName`: `secret_value_2`
   * Регистрация примера приложения с Azure Active Directory.
   * Авторизуйте приложение для доступа к хранилищу ключей. При использовании `Set-AzureRmKeyVaultAccessPolicy` командлет PowerShell, чтобы авторизовать приложение для доступа к хранилищу ключей предоставляют `List` и `Get` доступ к секретам с `-PermissionsToSecrets list,get`.

2. Обновление приложения *appsettings.json* файла со значениями `Vault`, `ClientId`, и `ClientSecret`.
3. Запуск примера приложения, который получает свои значения конфигурации из `IConfigurationRoot` с тем же именем, что имя секрета.
   * Non иерархических значения: значение `SecretName` получается с помощью `config["SecretName"]`.
   * Иерархические значения (разделов): использование `:` нотации (двоеточие) или `GetSection` метода расширения. Чтобы получить значение конфигурации, следует используйте один из этих подходов:
     * `config["Section:SecretName"]`
     * `config.GetSection("Section")["SecretName"]`

При запуске приложения, веб-страницы показывает загруженные значения секрета:

![Окно браузера с значения секретов, загруженном с помощью конфигурации Azure Key Vault Provider](key-vault-configuration/_static/sample1.png)

## <a name="bind-an-array-to-a-class"></a>Привязка массива к классу

Поставщик способный считывать значения конфигурации в массив для привязки к массиву POCO.

При чтении из источника конфигурации, который позволяет ключи содержат двоеточия (`:`) разделители, числового ключа сегмента используется для различения ключи, составляющие массив (`:0:`, `:1:`,... `:{n}:`). Дополнительные сведения см. в разделе [конфигурации: привязка массива к классу](xref:fundamentals/configuration/index#bind-an-array-to-a-class).

Azure Key Vault ключи нельзя использовать двоеточие в качестве разделителя. Подход, описанный в этом разделе используются двойные тире (`--`) как разделитель для значений иерархической (разделов). Массив ключи хранятся в хранилище ключей Azure с double штрихов и числовых ключей сегментов (`--0--`, `--1--`,... `--{n}--`).

Рассмотрим следующую [Serilog](https://serilog.net/) конфигурация поставщика, предоставляемые JSON-файл журнала. Существуют два объектных литералов, определенные в `WriteTo` массива, который отражает две Serilog *приемников*, описывающих назначения для выходных данных ведения журнала:

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

Конфигурации, представленной выше JSON-файл хранится в хранилище ключей Azure с помощью двойной штрих (`--`) нотации и числовые сегменты:

| Ключ | Значение |
| --- | ----- |
| `Serilog--WriteTo--0--Name` | `AzureTableStorage` |
| `Serilog--WriteTo--0--Args--storageTableName` | `logs` |
| `Serilog--WriteTo--0--Args--connectionString` | `DefaultEnd...ountKey=Eby8...GMGw==` |
| `Serilog--WriteTo--1--Name` | `AzureDocumentDB` |
| `Serilog--WriteTo--1--Args--endpointUrl` | `https://contoso.documents.azure.com:443` |
| `Serilog--WriteTo--1--Args--authorizationKey` | `Eby8...GMGw==` |

## <a name="create-prefixed-key-vault-secrets-and-load-configuration-values-key-name-prefix-sample"></a>Создание секретов с префиксом хранилища ключей и загрузить значения конфигурации (ключ имя префикс sample)

`AddAzureKeyVault` также содержит перегрузку, которая принимает реализацию `IKeyVaultSecretManager`, который позволяет управлять ключевых секреты хранилища преобразуются в ключи конфигурации. Например можно реализовать интерфейс для загрузки значения секретов, исходя из значения префикса, указанные вами при запуске приложения. Это позволяет, например, загружать секреты, в зависимости от версии приложения.

> [!WARNING]
> Для размещения секреты для нескольких приложений в том же хранилище ключей или для размещения среды секреты не используйте префиксы секретных кодов хранилища ключей (например, *разработки* и *рабочей* секретов) в одну хранилище. Мы рекомендуем различных приложений и сред разработки, эксплуатации использовать отдельные хранилища ключей для изоляции среды приложений для обеспечения максимальной безопасности.

С помощью второго примера приложения создайте секрет в хранилище ключей для `5000-AppSecret` (периоды не допускаются в именах секрета хранилища ключей) представляющий секрет приложения для версии 5.0.0.0 вашего приложения. Для другой версии, 5.1.0.0, создать секрет для `5100-AppSecret`. Каждая версия приложения загружает собственную секретное значение в его конфигурацию в качестве `AppSecret`, удаление версии при загрузке секрет. Ниже приведен пример реализации:

[!code-csharp[Configuration builder](key-vault-configuration/samples/key-name-prefix-sample/2.x/Program.cs?name=snippet1&highlight=18)]

[!code-csharp[PrefixKeyVaultSecretManager](key-vault-configuration/samples/key-name-prefix-sample/2.x/Startup.cs?name=snippet1)]

`Load` Метод вызывается с помощью алгоритма поставщик, выполняющий итерацию секреты хранилища, чтобы найти те, которые имеют префикс версии. Найденный префикс версии с `Load`, алгоритм использует `GetKey` метод, чтобы вернуть имя конфигурации имя секрета. Она отсекает версии префикс из имени секрета и возвращает остальные имя секрета для загрузки в конфигурацию приложения пары "имя значение".

При реализации этого подхода:

1. Загружаются секретных кодов хранилища ключей.
2. Секрет строка `5000-AppSecret` совпадении.
3. Версии, `5000` (с dash), удаляются из имени ключа оставляя `AppSecret` загружать данные с помощью секретное значение в конфигурацию приложения.

> [!NOTE]
> Можно также предоставить собственную `KeyVaultClient` реализацию `AddAzureKeyVault`. Предоставление пользовательского клиента позволяет совместно использовать один экземпляр клиента между поставщик конфигурации и другие части приложения.

1. Создание хранилища ключей и настроить Azure Active Directory (Azure AD) для приложении, следуя указаниям в [приступить к работе с хранилищем ключей Azure](https://azure.microsoft.com/documentation/articles/key-vault-get-started/).
   * Добавьте секреты в хранилище ключей с помощью [модуль PowerShell хранилища ключей AzureRM](/powershell/module/azurerm.keyvault) корпорации [коллекции PowerShell](https://www.powershellgallery.com/packages/AzureRM.KeyVault), [API REST хранилища ключей Azure](/rest/api/keyvault/), или [Портала azure](https://portal.azure.com/). Секреты создаются как *вручную* или *сертификат* секреты. *Сертификат* секретов, сертификатов для использования с приложениями и службами, но не поддерживаются поставщиком конфигурации. Следует использовать *вручную* параметр, чтобы создать пару "имя значение" секреты для использования при помощи поставщика конфигурации.
     * Использование иерархических значений (разделы конфигурации) `--` (двух дефисов) в качестве разделителя.
     * Создайте две *вручную* секретные сведения с помощью следующие пары "имя значение":
       * `5000-AppSecret`: `5.0.0.0_secret_value`
       * `5100-AppSecret`: `5.1.0.0_secret_value`
   * Регистрация примера приложения с Azure Active Directory.
   * Авторизуйте приложение для доступа к хранилищу ключей. При использовании `Set-AzureRmKeyVaultAccessPolicy` командлет PowerShell, чтобы авторизовать приложение для доступа к хранилищу ключей предоставляют `List` и `Get` доступ к секретам с `-PermissionsToSecrets list,get`.

2. Обновление приложения *appsettings.json* файла со значениями `Vault`, `ClientId`, и `ClientSecret`.
3. Запуск примера приложения, который получает свои значения конфигурации из `IConfigurationRoot` с тем же именем, что имя с префиксом секрета. В этом примере используется версия приложения, который вы указали для префикс `PrefixKeyVaultSecretManager` при добавлении поставщика конфигурации Azure Key Vault. Значение для `AppSecret` получается с помощью `config["AppSecret"]`. Веб-страницы, созданное приложением отображается значение загрузки:

   ![Окно браузера, показывающее значение секрета, загруженном с помощью конфигурации Azure Key Vault Provider при 5.0.0.0 версия приложения](key-vault-configuration/_static/sample2-1.png)

4. Изменить версию сборки приложения в файле проекта из `5.0.0.0` для `5.1.0.0` и снова запустите приложение. На этот раз секретный возвращаемое значение `5.1.0.0_secret_value`. Веб-страницы, созданное приложением отображается значение загрузки:

   ![Окно браузера, показывающее значение секрета, загруженном с помощью конфигурации Azure Key Vault Provider при 5.1.0.0 версия приложения](key-vault-configuration/_static/sample2-2.png)

## <a name="control-access-to-the-clientsecret"></a>Управление доступом к ClientSecret

Используйте [средство Secret Manager](xref:security/app-secrets) для поддержания `ClientSecret` за пределами дерево исходного кода проекта. В менеджере секретов связывается с определенным проектом секретов приложения и использовать их для нескольких проектов.

Разработка приложения .NET Framework в среде, которая поддерживает сертификаты, можно выполнить в хранилище ключей Azure с использованием сертификата X.509. Закрытый ключ сертификата X.509 находится под управлением операционной системы. Дополнительные сведения см. в разделе [проверка подлинности с помощью сертификата вместо секрета клиента](/azure/key-vault/key-vault-use-from-web-application#authenticate-with-a-certificate-instead-of-a-client-secret). Используйте `AddAzureKeyVault` перегрузку, которая принимает `X509Certificate2` (`_env` в следующем примере:

```csharp
var builtConfig = config.Build();

var store = new X509Store(StoreLocation.CurrentUser);
store.Open(OpenFlags.ReadOnly);
var cert = store.Certificates
    .Find(X509FindType.FindByThumbprint, 
        config["CertificateThumbprint"], false);

config.AddAzureKeyVault(
    builtConfig["Vault"],
    builtConfig["ClientId"],
    cert.OfType<X509Certificate2>().Single(),
    new EnvironmentSecretManager(context.HostingEnvironment.ApplicationName));

store.Close();
```

## <a name="reload-secrets"></a>Перезагрузить секретов

Секреты, кэшируются до `IConfigurationRoot.Reload()` вызывается. Истек, отключена, и обновленные секреты в хранилище ключей не учитываются в приложении до `Reload` выполняется.

```csharp
Configuration.Reload();
```

## <a name="disabled-and-expired-secrets"></a>Отключенные и истекшим сроком действия секретов

Отключенные и просроченные секреты throw `KeyVaultClientException`. Чтобы избежать возникновения в приложении, замените приложение или изменить секрет просроченного отключено.

## <a name="troubleshoot"></a>Устранение неполадок

Если приложение не удается загрузить конфигурацию с помощью поставщика, сообщение об ошибке записывается [инфраструктуры ведения журнала ASP.NET Core](xref:fundamentals/logging/index). Следующие условия не позволит конфигурации загрузки:

* Приложение не настроено правильно, в Azure Active Directory.
* Хранилище ключей не существует в хранилище ключей Azure.
* Приложение не имеет разрешения на доступ к хранилищу ключей.
* Политика доступа не включает `Get` и `List` разрешения.
* В хранилище ключей данные конфигурации (пара «имя значение») ошибочно названы, отсутствуют, отключена или истек срок действия.
* Приложение имеет имя неправильный хранилища ключей (`Vault`), идентификатор приложения Azure AD (`ClientId`), или ключ Azure AD (`ClientSecret`).
* Ключ Azure AD (`ClientSecret`) срок действия истек.
* В приложении для значения, которое вы пытаетесь загрузить неправильный ключ конфигурации (имя).

## <a name="additional-resources"></a>Дополнительные ресурсы

* <xref:fundamentals/configuration/index>
* [Microsoft Azure: Хранилище ключей](https://azure.microsoft.com/services/key-vault/)
* [Microsoft Azure: Документация по хранилищу ключей](/azure/key-vault/)
* [Использование защищенных аппаратным модулем безопасности и их передача ключей для хранилища ключей Azure](/azure/key-vault/key-vault-hsm-protected-keys)
* [Класс KeyVaultClient](/dotnet/api/microsoft.azure.keyvault.keyvaultclient)
