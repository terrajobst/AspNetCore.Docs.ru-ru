---
title: Настройка внешней учетной записи учетной записи Майкрософт с помощью ASP.NET Core
author: rick-anderson
description: В этом примере демонстрируется интеграция проверки подлинности пользователя учетной записи Майкрософт в существующее приложение ASP.NET Core.
ms.author: riande
ms.custom: mvc
ms.date: 5/11/2019
uid: security/authentication/microsoft-logins
ms.openlocfilehash: 1c78cc957b6ff77c91c8ca4aef59a1cacd85a8ca
ms.sourcegitcommit: 3376f224b47a89acf329b2d2f9260046a372f924
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 05/10/2019
ms.locfileid: "65517092"
---
# <a name="microsoft-account-external-login-setup-with-aspnet-core"></a>Настройка внешней учетной записи учетной записи Майкрософт с помощью ASP.NET Core

Авторы: [Валерий Новицкий](https://github.com/01binary) (Valeriy Novytskyy) и [Рик Андерсон](https://twitter.com/RickAndMSFT) (Rick Anderson)

В этом примере показано, как разрешить пользователям выполнить вход с использованием своей учетной записью Майкрософт, с помощью ASP.NET Core 2.2 проект, созданный на [предыдущую страницу](xref:security/authentication/social/index).

## <a name="create-the-app-in-microsoft-developer-portal"></a>Создание приложения на портале разработчика Майкрософт

* Перейдите к [Azure портал регистрации приложений](https://go.microsoft.com/fwlink/?linkid=2083908) странице и создавать или входить в учетную запись Майкрософт:

Если у вас нет учетной записи Майкрософт, выберите **создать**. После входа вы будете перенаправлены на **регистрация приложений** страницы:

* Выберите **Новая регистрация**
* Введите **имя**.
* Выберите параметр для **поддерживаемые типы учетных записей**.  <!-- Accounts for any org work with MS domain accounts. Most folks probably want the last option, personal MS accounts -->
* В разделе **URI перенаправления**, введите URL-адрес разработки с `/signin-microsoft` добавляется. Например, `https://localhost:44389/signin-microsoft`. Схема проверки подлинности Майкрософт, Настройка описывается далее в этом образце автоматически будет обрабатывать запросы на `/signin-microsoft` маршрута, чтобы реализовать поток OAuth.
* Выберите **регистрации**

### <a name="create-client-secret"></a>Создайте секрет клиента

* В левой области выберите **сертификаты и секреты**.
* В разделе **секреты клиента**выберите **новый секрет клиента**

  * Добавьте описание секрет клиента.
  * Выберите **добавить** кнопки.

* В разделе **секреты клиента**, скопируйте значение секрета клиента.

> [!NOTE]
> Сегмент URI `/signin-microsoft` задан в качестве обратного вызова по умолчанию поставщика проверки подлинности Майкрософт. URI обратного вызова по умолчанию можно изменить при настройке по промежуточного слоя проверки подлинности Майкрософт с помощью наследуемого [RemoteAuthenticationOptions.CallbackPath](/dotnet/api/microsoft.aspnetcore.authentication.remoteauthenticationoptions.callbackpath) свойство [MicrosoftAccountOptions](/dotnet/api/microsoft.aspnetcore.authentication.microsoftaccount.microsoftaccountoptions) класса.

## <a name="store-the-microsoft-client-id-and-client-secret"></a>Store Microsoft клиента идентификатор и секрет клиента

Выполните следующие команды, чтобы обеспечить безопасное хранение `ClientId` и `ClientSecret` с помощью [Secret Manager](xref:security/app-secrets):

```console
dotnet user-secrets set Authentication:Microsoft:ClientId <Client-Id>
dotnet user-secrets set Authentication:Microsoft:ClientSecret <Client-Secret>
```

Связать конфиденциальные параметры, такие как Microsoft `ClientId` и `ClientSecret` для конфигурации приложения с помощью [Secret Manager](xref:security/app-secrets). В целях этого примера назовите токены `Authentication:Microsoft:ClientId` и `Authentication:Microsoft:ClientSecret`.

[!INCLUDE[](~/includes/environmentVarableColon.md)]

## <a name="configure-microsoft-account-authentication"></a>Настройка проверки подлинности учетной записи Майкрософт

Добавление службы учетной записи Майкрософт в `ConfigureServices` метод в *Startup.cs* файла:

[!code-csharp[](~/security/authentication/social/social-code/StartupMS.cs?name=snippet&highlight=10-14)]

[!INCLUDE [default settings configuration](includes/default-settings.md)]

[!INCLUDE[](includes/chain-auth-providers.md)]

См. в разделе [MicrosoftAccountOptions](/dotnet/api/microsoft.aspnetcore.builder.microsoftaccountoptions) Справочник по API, Дополнительные сведения о параметрах конфигурации, поддерживается проверка подлинности учетной записи Майкрософт. Это может использоваться для запроса различные сведения о пользователе.

## <a name="sign-in-with-microsoft-account"></a>Войдите с помощью учетной записи Майкрософт

Запустите и нажмите кнопку **вход**. Появится возможность входа с учетной записью Майкрософт. Если щелкнуть on Microsoft, вы будете перенаправлены в корпорацию Майкрософт для проверки подлинности. После входа под учетной записью Майкрософт (если это не сделано) вам будет предложено разрешить приложению доступ к вашим сведениям:

Коснитесь **Да** и вы будете перенаправлены обратно на веб-узел, где вы можете задать свой адрес электронной почты.

Теперь вы вошли с использованием учетных данных Майкрософт:

[!INCLUDE[Forward request information when behind a proxy or load balancer section](includes/forwarded-headers-middleware.md)]

## <a name="troubleshooting"></a>Устранение неполадок

* Если поставщик учетной записи Майкрософт вы будете перенаправлены к странице ошибки входа, обратите внимание, ошибка заголовок и описание параметров строки запроса сразу после `#` (хэштег) в Uri.

  Несмотря на то, что сообщение об ошибке кажется указывают на проблему с использованием проверки подлинности, наиболее распространенной причиной является Uri, не соответствующих ни одному из приложения **URI перенаправления** для **Web** платформы .
* Если удостоверение не настроена, вызвав `services.AddIdentity` в `ConfigureServices`, пытающиеся выполнить проверку подлинности приведет к *ArgumentException: Необходимо указать параметр «SignInScheme»*. Шаблон проекта, используемый в этом примере гарантирует, что это будет сделано.
* Если база данных сайта не был создан путем применения первоначальной миграции, вы получите *сбой операции из базы данных при обработке запроса* ошибки. Коснитесь **применить миграции** для создания базы данных и обновить, чтобы продолжить выполнение после ошибки.

## <a name="next-steps"></a>Следующие шаги

* В этой статье объясняется, как можно выполнить проверку подлинности с корпорацией Майкрософт. Можно выполнить аналогичный подход для проверки подлинности с помощью других поставщиков, перечисленных на [предыдущую страницу](xref:security/authentication/social/index).

* После публикации веб-сайт веб-приложение Azure, создание нового клиента секретов на портале разработчика Microsoft.

* Задайте `Authentication:Microsoft:ClientId` и `Authentication:Microsoft:ClientSecret` как параметры приложения на портале Azure. Система конфигурации предназначена для чтения разделов из переменных среды.