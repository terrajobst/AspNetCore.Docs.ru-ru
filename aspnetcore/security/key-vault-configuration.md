---
title: "Поставщик конфигурации Azure хранилища ключей"
author: guardrex
description: "Узнайте, как использовать поставщик конфигурации хранилища ключей Azure для настройки приложения с помощью пары имя значение во время выполнения."
keywords: "ASP.NET Core, конфигурации, хранилище ключей Azure"
ms.author: riande
manager: wpickett
ms.date: 08/09/2017
ms.topic: article
ms.assetid: 0292bdae-b3ed-4637-bd67-19b9bb8b65cb
ms.prod: asp.net-core
uid: security/key-vault-configuration
ms.openlocfilehash: c5d8506c1bc8e6364d01596a0c82e1da41eea4ca
ms.sourcegitcommit: 732cd2684246e49e796836596643a8d37e20c46d
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/01/2017
---
# <a name="azure-key-vault-configuration-provider"></a>Поставщик конфигурации Azure хранилища ключей

По [Latham Люк](https://github.com/guardrex) и [Эндрю Stanton сиделка](https://github.com/anurse)

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

Просмотреть или загрузить образец кода для 2.x:

* [Пример базового](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/key-vault-configuration/samples/basic-sample/2.x) ([загрузке](xref:tutorials/index#how-to-download-a-sample))-считывает значения секрета в приложение.
* [Образец префикс имени ключа](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/key-vault-configuration/samples/key-name-prefix-sample/2.x) ([загрузке](xref:tutorials/index#how-to-download-a-sample)) — значения секрета операций чтения с помощью префикса имя ключа, представляющую версию приложения, который позволяет загрузить другой набор секретного значения для каждой версии приложения.

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

Просмотреть или загрузить образец кода для 1.x:

* [Пример базового](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/key-vault-configuration/samples/basic-sample/1.x) ([загрузке](xref:tutorials/index#how-to-download-a-sample))-считывает значения секрета в приложение.
* [Образец префикс имени ключа](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/key-vault-configuration/samples/key-name-prefix-sample/1.x) ([загрузке](xref:tutorials/index#how-to-download-a-sample)) — значения секрета операций чтения с помощью префикса имя ключа, представляющую версию приложения, который позволяет загрузить другой набор секретного значения для каждой версии приложения. 

---

В этом документе описывается использование [Microsoft Azure Key Vault](https://azure.microsoft.com/services/key-vault/) поставщик конфигурации для загрузки значения конфигурации из хранилища ключей Azure секретные данные. Хранилище ключей Azure является облачная служба, которая помогает защитить криптографических ключей и используемые приложения и службы. Обычные сценарии включают управление доступом к конфиденциальные данные конфигурации и удовлетворения требования для FIPS 140-2 уровня 2 проверяется аппаратные модули безопасности (HSM) во время хранения данных конфигурации. Эта функция предназначена для приложений, предназначенных для ASP.NET Core 1.1 или более поздней версии.

## <a name="package"></a>Пакет
Чтобы использовать поставщик, добавьте ссылку на [Microsoft.Extensions.Configuration.AzureKeyVault](https://www.nuget.org/packages/Microsoft.Extensions.Configuration.AzureKeyVault/) пакета.

## <a name="application-configuration"></a>Настройка приложения
Вы можете просматривать поставщика с [образец приложения](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/key-vault-configuration/samples). После установления хранилища ключей и создать секреты в хранилище, в примерах приложений безопасно загрузить значения секрета в своей конфигурации и отобразить их на веб-страницах.

Поставщик добавляется `ConfigurationBuilder` с `AddAzureKeyVault` расширения. В примерах приложений, расширение использует три значения конфигурации, загруженного из *appsettings.json* файла.

| Параметры приложения    | Описание                    | Пример                                      |
| -------------- | ------------------------------ | -------------------------------------------- |
| `Vault`        | Имя хранилища ключей Azure           | contosovault                                 |
| `ClientId`     | Идентификатор приложения Azure Active Directory  | 627e911e-43CC-61d4-992e-12db9c81b413         |
| `ClientSecret` | Ключ приложения Azure Active Directory | g58K3dtg59o1Pa + e59v2Tx829w6VxTB2yv9sv/101di = |

[!code-csharp[Program](key-vault-configuration/samples/basic-sample/2.x/Program.cs?name=snippet1&highlight=2,7-10)]

## <a name="creating-key-vault-secrets-and-loading-configuration-values-basic-sample"></a>Создание хранилища ключей секреты и загрузки значений конфигурации (basic образец)
1. Создание хранилища ключей и настроить Azure Active Directory (Azure AD) для приложения согласно инструкциям в [приступить к работе с хранилищем ключей Azure](https://azure.microsoft.com/documentation/articles/key-vault-get-started/).
  * Добавьте секреты в хранилище ключей с использованием [модуль AzureRM ключ хранилища PowerShell](/powershell/module/azurerm.keyvault) доступны из [коллекции PowerShell](https://www.powershellgallery.com/packages/AzureRM.KeyVault), [API REST хранилища ключей Azure](/rest/api/keyvault/), или [Портал azure](https://portal.azure.com/). Секреты создаются как *вручную* или *сертификат* секретные данные. *Сертификат* секретные данные, сертификаты для использования приложений и служб, но не поддерживаются поставщиком конфигурации. Следует использовать *вручную* параметр, чтобы создать секреты пары "имя значение" для использования с поставщиком конфигурации.
    * Простой секреты, создаются в виде пар "имя значение". Azure имена секрета хранилища ключей, ограничены буквы, цифры и дефисы.
    * Используйте иерархическими значениями (разделы конфигурации) `--` (двух дефисов) в качестве разделителя в образце. Имена, которые обычно используются для разделения раздел из подраздела в [конфигурации ASP.NET Core](xref:fundamentals/configuration), не разрешены в именах секрета. Таким образом используются и при загрузке секреты в конфигурации приложения для двоеточие местами двух дефисов.
    * Создайте два *вручную* секреты с помощью следующих пар имя значение. Первый секретный код является простым именем и значением и второй секретный код создает секретное значение с раздел и раздел имя секрета:
      * `SecretName`: `secret_value_1`
      * `Section--SecretName`: `secret_value_2`
  * Регистрация примера приложения в Azure Active Directory.
  * Разрешить приложению доступ к хранилища ключей. При использовании `Set-AzureRmKeyVaultAccessPolicy` командлет PowerShell, чтобы разрешить приложению доступ к хранилища ключей, обеспечивают `List` и `Get` доступ к секретной информации с `-PermissionsToKeys list,get`.
2. Обновление приложения *appsettings.json* файл со значениями `Vault`, `ClientId`, и `ClientSecret`.
3. Запустите пример приложения, который получает свои значения конфигурации `IConfigurationRoot` с тем же именем, как имя секрета.
  * Не иерархическими значениями: значение для `SecretName` получен с `config["SecretName"]`.
  * Иерархическими значениями (разделов): используйте `:` нотации (двоеточие) или `GetSection` метода расширения. Выполните один из этих подходов, чтобы получить значение конфигурации.
    * `config["Section:SecretName"]`
    * `config.GetSection("Section")["SecretName"]`

При запуске приложения, загруженного секретного значения показаны веб-страницы:

![Окно обозревателя, показывающее значения секрета, загруженного с помощью поставщика конфигурации хранилища ключей Azure](key-vault-configuration/_static/sample1.png)

## <a name="creating-prefixed-key-vault-secrets-and-loading-configuration-values-key-name-prefix-sample"></a>Создание секреты с префиксом хранилища ключей и загрузки значений конфигурации (ключ имя префикс образец)
`AddAzureKeyVault`также предоставляет перегрузку, которая принимает реализацию `IKeyVaultSecretManager`, позволяет управлять как ключевые хранилище секретов, преобразуются в конфигурации ключи. Например можно реализовать интерфейс для загрузки значения секрета, на основе значения префикс вами при запуске приложения. Это позволит, например, загрузить секреты, в зависимости от версии приложения.

> [!WARNING]
> Поместите секретных сведений для нескольких приложений в одном хранилище ключа или размещение среды секретов не используйте префиксы секреты в хранилище ключей (например, *разработки* verus *рабочей* секреты) в один хранилище. Мы рекомендуем различных приложений и разработки и рабочих средах использовать отдельный ключа хранилища для изоляции приложения среды для самого высокого уровня безопасности.

С помощью второй пример приложения создайте секрета в хранилище ключей для `5000-AppSecret` (периоды не разрешены в именах секрета хранилища ключей) представляющий секрет приложения для версии 5.0.0.0 приложения. Для другой версии, 5.1.0.0, создать секрет для `5100-AppSecret`. Каждая версия приложения загружает собственные секретное значение в его конфигурации как `AppSecret`, чередует off версии при загрузке секрета. Ниже приводится пример реализации.

[!code-csharp[Configuration builder](key-vault-configuration/samples/key-name-prefix-sample/2.x/Program.cs?name=snippet1&highlight=12)]

[!code-csharp[PrefixKeyVaultSecretManager](key-vault-configuration/samples/key-name-prefix-sample/2.x/Startup.cs?name=snippet1)]

`Load` Метод вызывается с помощью алгоритма поставщик, выполняющий перебор элементов хранилище секретов, чтобы найти те, которые имеют префикс версии. Если найдена версия префикс с `Load`, алгоритм использует `GetKey` метод для возврата имени конфигурации имя секрета. Он отсекает версии префикс из имени секрета и возвращает остальные имя секрета для загрузки в конфигурации приложения пары "имя значение".

При реализации этого подхода:

1. Загружаются секреты хранилища ключей.
2. Строка секрет для `5000-AppSecret` совпадении.
3. Версия `5000` (на тире), исключается из оставить имя ключа `AppSecret` для загрузки с секретное значение в конфигурации приложения.

> [!NOTE]
> Можно также предоставить собственную `KeyVaultClient` реализации `AddAzureKeyVault`. Предоставление пользовательских клиентских позволяет совместно использовать один экземпляр клиента между поставщика конфигурации и другие части приложения.

1. Создание хранилища ключей и настроить Azure Active Directory (Azure AD) для приложения согласно инструкциям в [приступить к работе с хранилищем ключей Azure](https://azure.microsoft.com/documentation/articles/key-vault-get-started/).
  * Добавьте секреты в хранилище ключей с использованием [модуль AzureRM ключ хранилища PowerShell](/powershell/module/azurerm.keyvault) доступны из [коллекции PowerShell](https://www.powershellgallery.com/packages/AzureRM.KeyVault), [API REST хранилища ключей Azure](/rest/api/keyvault/), или [Портал azure](https://portal.azure.com/). Секреты создаются как *вручную* или *сертификат* секретные данные. *Сертификат* секретные данные, сертификаты для использования приложений и служб, но не поддерживаются поставщиком конфигурации. Следует использовать *вручную* параметр, чтобы создать секреты пары "имя значение" для использования с поставщиком конфигурации.
    * Используйте иерархическими значениями (разделы конфигурации) `--` (двух дефисов) в качестве разделителя.
    * Создайте два *вручную* секреты с помощью следующих пар имя значение:
      * `5000-AppSecret`: `5.0.0.0_secret_value`
      * `5100-AppSecret`: `5.1.0.0_secret_value`
  * Регистрация примера приложения в Azure Active Directory.
  * Разрешить приложению доступ к хранилища ключей. При использовании `Set-AzureRmKeyVaultAccessPolicy` командлет PowerShell, чтобы разрешить приложению доступ к хранилища ключей, обеспечивают `List` и `Get` доступ к секретной информации с `-PermissionsToKeys list,get`.
2. Обновление приложения *appsettings.json* файл со значениями `Vault`, `ClientId`, и `ClientSecret`.
3. Запустите пример приложения, который получает свои значения конфигурации `IConfigurationRoot` с тем же именем, как имя с префиксом секрета. В этом образце используется префикс версию приложения, что вы указали `PrefixKeyVaultSecretManager` при добавлении поставщика хранилища ключей Azure конфигурации. Значение для `AppSecret` получен с `config["AppSecret"]`. Веб-страницы, созданный приложением показано загруженное значение.

   ![Окно обозревателя, показывающее секретное значение, загруженного с помощью поставщика конфигурации хранилища ключей Azure при 5.0.0.0 версии приложения](key-vault-configuration/_static/sample2-1.png)

4. Изменить версию сборки приложения в файле проекта из `5.0.0.0` для `5.1.0.0` и снова запустите приложение. На этот раз секретный возвращаемое значение `5.1.0.0_secret_value`. Веб-страницы, созданный приложением показано загруженное значение.

   ![Окно обозревателя, показывающее секретное значение, загруженного с помощью поставщика конфигурации хранилища ключей Azure при 5.1.0.0 версии приложения](key-vault-configuration/_static/sample2-2.png)

## <a name="controlling-access-to-the-clientsecret"></a>Управление доступом к ClientSecret
Используйте [секрет диспетчера](xref:security/app-secrets) для поддержания `ClientSecret` вне дерева исходного проекта. С помощью диспетчера секрет связывается с конкретным проектом секрета приложения и использовать их совместно в нескольких проектах.

При разработке приложения .NET Framework в среде, поддерживающей сертификаты, можно проверить подлинность в хранилище ключей Azure с помощью сертификата X.509. Операционная система управляет закрытый ключ сертификата X.509. Дополнительные сведения см. в разделе [проверка подлинности с помощью сертификата, вместо секрет клиента](https://docs.microsoft.com/azure/key-vault/key-vault-use-from-web-application#authenticate-with-a-certificate-instead-of-a-client-secret). Используйте `AddAzureKeyVault` перегрузку, которая принимает `X509Certificate2`.

```csharp
var store = new X509Store(StoreLocation.CurrentUser);
store.Open(OpenFlags.ReadOnly);
var cert = store.Certificates.Find(X509FindType.FindByThumbprint, config["CertificateThumbprint"], false);

builder.AddAzureKeyVault(
    config["Vault"],
    config["ClientId"],
    cert.OfType<X509Certificate2>().Single(),
    new EnvironmentSecretManager(env.ApplicationName));
store.Close();

Configuration = builder.Build();
```

## <a name="reloading-secrets"></a>Повторная загрузка секретов
Секретные данные кэшируются до `IConfigurationRoot.Reload()` вызывается. Истек, отключена, и приложению, пока не учитываются обновленные секреты в хранилище ключей `Reload` выполняется.

```csharp
Configuration.Reload();
```

## <a name="disabled-and-expired-secrets"></a>Секреты отключено и устаревших записей
Секреты отключено и устаревших throw `KeyVaultClientException`. Чтобы избежать возникновения приложения, замените приложение или обновление секрета отключена или истек срок действия.

## <a name="troubleshooting"></a>Устранение неполадок
Если приложению не удается загрузить конфигурацию с помощью поставщика, сообщение об ошибке записывается [инфраструктуры ASP.NET ведения журнала](xref:fundamentals/logging). Следующие условия не позволит конфигурации загрузки:
* Приложения не настроен правильно в Azure Active Directory.
* Хранилище ключей не существует в хранилище ключей Azure.
* Приложение не имеет разрешения доступа к хранилищу ключей.
* Политика доступа не включает `Get` и `List` разрешения.
* В хранилище ключей данных конфигурации (пара «имя значение») неправильно называется, отсутствует, отключена или истек срок действия.
* Приложение имеет имя неверного ключа хранилища (`Vault`), идентификатор приложения Azure AD (`ClientId`), или ключ Azure AD (`ClientSecret`).
* Ключ Azure AD (`ClientSecret`) истек срок действия.
* В приложении для значение, которое вы пытаетесь загрузить неверный ключ конфигурации (имя).

## <a name="additional-resources"></a>Дополнительные ресурсы
* <xref:fundamentals/configuration>
* [Microsoft Azure: Хранилище ключей](https://azure.microsoft.com/services/key-vault/)
* [Microsoft Azure: Документации хранилища ключей](https://docs.microsoft.com/azure/key-vault/)
* [Описание способов создания и передачи, защищенных HSM ключей для хранилища ключей Azure](https://docs.microsoft.com/azure/key-vault/key-vault-hsm-protected-keys)
* [Класс KeyVaultClient](https://docs.microsoft.com/dotnet/api/microsoft.azure.keyvault.keyvaultclient)
