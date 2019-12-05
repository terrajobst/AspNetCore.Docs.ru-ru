---
title: Настройка внешнего входа учетной записи Майкрософт с ASP.NET Core
author: rick-anderson
description: В этом примере демонстрируется интеграция учетная запись Майкрософт проверки подлинности пользователей в существующее приложение ASP.NET Core.
ms.author: riande
ms.custom: mvc
ms.date: 12/4/2019
monikerRange: '>= aspnetcore-3.0'
uid: security/authentication/microsoft-logins
ms.openlocfilehash: ddaae1a25a1dcf167ffae0f24b480e2cde6aca5b
ms.sourcegitcommit: f4cd3828e26e6d549ba8d0c36a17be35ad9e5a51
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 12/04/2019
ms.locfileid: "74825469"
---
# <a name="microsoft-account-external-login-setup-with-aspnet-core"></a>Настройка внешнего входа учетной записи Майкрософт с ASP.NET Core

Авторы: [Валерий Новицкий](https://github.com/01binary) (Valeriy Novytskyy) и [Рик Андерсон](https://twitter.com/RickAndMSFT) (Rick Anderson)

В этом примере показано, как разрешить пользователям входить с помощью учетная запись Майкрософт с помощью проекта ASP.NET Core 3,0, созданного на [предыдущей странице](xref:security/authentication/social/index).

## <a name="create-the-app-in-microsoft-developer-portal"></a>Создание приложения на портале разработчика Майкрософт

* Добавьте в проект пакет NuGet [Microsoft. AspNetCore. Authentication. MicrosoftAccount](https://www.nuget.org/packages/Microsoft.AspNetCore.Authentication.MicrosoftAccount/) .
* Перейдите на страницу [портал Azure-регистрация приложений](https://go.microsoft.com/fwlink/?linkid=2083908) и создайте или войдите в учетная запись Майкрософт:

Если у вас нет учетная запись Майкрософт, выберите **создать**. После входа вы будете перенаправлены на **Регистрация приложений** страницу:

* Выбрать **новую регистрацию**
* Введите **Имя**.
* Выберите вариант для **поддерживаемых типов учетных записей**.  <!-- Accounts for any org work with MS domain accounts. Most folks probably want the last option, personal MS accounts -->
* В разделе **URI перенаправления**введите URL-адрес разработки с добавленным `/signin-microsoft`. Например, `https://localhost:5001/signin-microsoft`. Схема проверки подлинности Майкрософт, настроенная далее в этом примере, автоматически обрабатывает запросы на `/signin-microsoft` маршруте для реализации потока OAuth.
* Выбор **регистра**

### <a name="create-client-secret"></a>Создать секрет клиента

* В левой области выберите **сертификаты & секреты**.
* В разделе **секреты клиента**выберите **новый секрет клиента** .

  * Добавьте описание секрета клиента.
  * Нажмите кнопку **Добавить**.

* В разделе **секреты клиента**скопируйте значение секрета клиента.

> [!NOTE]
> Сегмент URI `/signin-microsoft` задается в качестве обратного вызова по умолчанию для поставщика проверки подлинности Майкрософт. URI обратного вызова по умолчанию можно изменить при настройке по промежуточного слоя проверки подлинности Майкрософт с помощью унаследованного свойства [ремотеаусентикатионоптионс. каллбаккпас](/dotnet/api/microsoft.aspnetcore.authentication.remoteauthenticationoptions.callbackpath) класса [микрософтаккаунтоптионс](/dotnet/api/microsoft.aspnetcore.authentication.microsoftaccount.microsoftaccountoptions) .

## <a name="store-the-microsoft-client-id-and-client-secret"></a>Хранение идентификатора клиента и секрета клиента Майкрософт

Выполните следующие команды для безопасного хранения `ClientId` и `ClientSecret` с помощью [диспетчера секретов](xref:security/app-secrets):

```dotnetcli
dotnet user-secrets set Authentication:Microsoft:ClientId <Client-Id>
dotnet user-secrets set Authentication:Microsoft:ClientSecret <Client-Secret>
```

Свяжите конфиденциальные параметры, такие как Microsoft `ClientId` и `ClientSecret` с конфигурацией приложения с помощью [диспетчера секретов](xref:security/app-secrets). Для целей этого примера назовите маркеры `Authentication:Microsoft:ClientId` и `Authentication:Microsoft:ClientSecret`.

[!INCLUDE[](~/includes/environmentVarableColon.md)]

## <a name="configure-microsoft-account-authentication"></a>Настройка проверки подлинности учетной записи Майкрософт

Добавьте службу учетных записей Майкрософт в `Startup.ConfigureServices`:

[!code-csharp[](~/security/authentication/social/social-code/3.x/StartupMS3x.cs?name=snippet&highlight=10-14)]

[!INCLUDE [default settings configuration](includes/default-settings.md)]

[!INCLUDE[](includes/chain-auth-providers.md)]

Дополнительные сведения о параметрах конфигурации, поддерживаемых проверкой подлинности учетной записи Майкрософт, см. в справочнике по API [микрософтаккаунтоптионс](/dotnet/api/microsoft.aspnetcore.builder.microsoftaccountoptions) . Это может использоваться для запроса различные сведения о пользователе.

## <a name="sign-in-with-microsoft-account"></a>Учетная запись Войдите с помощью учетной записи Майкрософт

Запустите приложение и нажмите кнопку **войти**. Появится возможность войти в систему с помощью Microsoft. Если щелкнуть Майкрософт, вы будете перенаправлены в корпорацию Майкрософт для проверки подлинности. После входа с помощью учетной записи Майкрософт вам будет предложено предоставить приложению доступ к вашим сведениям:

Коснитесь кнопки **Да** , и вы будете перенаправлены на веб-сайт, на котором можно задать свой адрес электронной почты.

Теперь вы выполнили вход с использованием учетных данных Майкрософт:

[!INCLUDE[Forward request information when behind a proxy or load balancer section](includes/forwarded-headers-middleware.md)]

## <a name="troubleshooting"></a>Диагностика

* Если поставщик учетных записей Майкрософт перенаправит вас на страницу ошибки входа, обратите внимание на заголовок ошибки и описание параметры строки запроса непосредственно после `#` (хэш-кода) в универсальном коде ресурса (URI).

  Несмотря на то, что сообщение об ошибке указывает на проблему с проверкой подлинности Майкрософт, наиболее распространенной причиной является URI приложения, не совпадающий ни с одним из **URI перенаправления** , указанных для **веб-** платформы.
* Если удостоверение не настроено путем вызова `services.AddIdentity` в `ConfigureServices`, попытка проверки подлинности приведет к появлению *исключения ArgumentException: необходимо указать параметр "сигнинсчеме"* . Шаблон проекта, используемый в этом образце, гарантирует, что это будет сделано.
* Если база данных сайта не был создан путем применения первоначальной миграции, вы получите *сбой операции из базы данных при обработке запроса* ошибки. Коснитесь **применить миграции** для создания базы данных и обновить, чтобы продолжить выполнение после ошибки.

## <a name="next-steps"></a>Следующие шаги

* В этой статье показано, как можно пройти проверку подлинности в Майкрософт. Можно выполнить аналогичный подход для проверки подлинности с помощью других поставщиков, перечисленных на [предыдущую страницу](xref:security/authentication/social/index).

* После публикации веб-сайта в веб-приложение Azure создайте секретные данные клиента на портале разработчика Майкрософт.

* Задайте `Authentication:Microsoft:ClientId` и `Authentication:Microsoft:ClientSecret` как параметры приложения на портале Azure. Система конфигурации предназначена для чтения разделов из переменных среды.
