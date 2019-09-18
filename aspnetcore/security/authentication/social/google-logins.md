---
title: Настройка внешней учетной записи Google в ASP.NET Core
author: rick-anderson
description: В этом учебнике показано интеграцию Google учетной записи пользователя и проверки подлинности в существующее приложение ASP.NET Core.
ms.author: riande
ms.custom: mvc, seodec18
ms.date: 06/19/2019
uid: security/authentication/google-logins
ms.openlocfilehash: e12d831d2e0a5c9acae5ea41fb4187ad4ca6b0ea
ms.sourcegitcommit: 215954a638d24124f791024c66fd4fb9109fd380
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 09/18/2019
ms.locfileid: "71082484"
---
# <a name="google-external-login-setup-in-aspnet-core"></a>Настройка внешней учетной записи Google в ASP.NET Core

Авторы: [Валерий Новицкий](https://github.com/01binary) (Valeriy Novytskyy) и [Рик Андерсон](https://twitter.com/RickAndMSFT) (Rick Anderson)

[Устаревшие API-интерфейсы Google + были закрыты до 7 марта 2019 г](https://developers.google.com/+/api-shutdown). Вход в Google + и разработчики должны перейти на новую систему входа Google. Пакеты ASP.NET Core 2,1 и 2,2 для проверки подлинности Google обновлены в соответствии с изменениями. Дополнительные сведения и временные меры для ASP.NET Core см. в [этой статье о проблемах GitHub](https://github.com/aspnet/AspNetCore/issues/6486). Этот учебник был обновлен с учетом нового процесса установки.

В этом руководстве показано, как разрешить пользователям входить в систему с помощью учетной записи Google, используя проект ASP.NET Core 2,2, созданный на [предыдущей странице](xref:security/authentication/social/index).

## <a name="create-a-google-api-console-project-and-client-id"></a>Создание проекта консоли Google API и идентификатора клиента

* Перейдите к разделу [Интеграция Google-входа в веб-приложение](https://developers.google.com/identity/sign-in/web/devconsole-project) и выберите **Настройка проекта**.
* В диалоговом окне **Настройка клиента OAuth** выберите **веб-сервер**.
* В текстовом поле " **зарегистрированные URI перенаправления** " задайте URI перенаправления. Например: `https://localhost:5001/signin-google`
* Сохраните **идентификатор клиента** и **секрет клиента**.
* При развертывании сайта зарегистрируйте новый общедоступный URL-адрес из **консоли Google**.

## <a name="store-google-clientid-and-clientsecret"></a>Store Google ClientID и ClientSecret

Храните конфиденциальные параметры, такие как `Client ID` Google `Client Secret` и [Диспетчер секретов](xref:security/app-secrets). В рамках этого руководства назовите маркеры `Authentication:Google:ClientId` и: `Authentication:Google:ClientSecret`

```dotnetcli
dotnet user-secrets set "Authentication:Google:ClientId" "X.apps.googleusercontent.com"
dotnet user-secrets set "Authentication:Google:ClientSecret" "<client secret>"
```

[!INCLUDE[](~/includes/environmentVarableColon.md)]

Вы можете управлять учетными данными и использованием API в [консоли API](https://console.developers.google.com/apis/dashboard).

## <a name="configure-google-authentication"></a>Настройка проверки подлинности Google

Добавьте службу Google в `Startup.ConfigureServices`:

[!code-csharp[](~/security/authentication/social/social-code/StartupGoogle.cs?name=snippet_ConfigureServices&highlight=10-18)]

[!INCLUDE [default settings configuration](includes/default-settings2-2.md)]

## <a name="sign-in-with-google"></a>Войдите с помощью Google

* Запустите приложение и нажмите кнопку **войти**. Появится возможность войти в систему с помощью Google.
* Нажмите кнопку **Google** , которая перенаправляет на Google для проверки подлинности.
* После ввода учетных данных Google вы будете перенаправлены обратно на веб-сайт.

[!INCLUDE[Forward request information when behind a proxy or load balancer section](includes/forwarded-headers-middleware.md)]

[!INCLUDE[](includes/chain-auth-providers.md)]

Дополнительные сведения <xref:Microsoft.AspNetCore.Authentication.Google.GoogleOptions> о параметрах конфигурации, поддерживаемых проверкой подлинности Google, см. в справочнике по API. Это может использоваться для запроса различные сведения о пользователе.

## <a name="change-the-default-callback-uri"></a>Изменение URI обратного вызова по умолчанию

Сегмент URI `/signin-google` задан в качестве обратного вызова по умолчанию поставщик проверки подлинности Google. URI обратного вызова по умолчанию можно изменить при настройке по промежуточного слоя проверки подлинности Google с помощью наследуемого [RemoteAuthenticationOptions.CallbackPath](/dotnet/api/microsoft.aspnetcore.authentication.remoteauthenticationoptions.callbackpath) свойство [GoogleOptions](/dotnet/api/microsoft.aspnetcore.authentication.google.googleoptions) класса.

## <a name="troubleshooting"></a>Устранение неполадок

* Если вход не работает и вы не получаете никаких ошибок, переключитесь в режим разработки, чтобы упростить отладку.
* Если удостоверение не настроено `services.AddIdentity` с `ConfigureServices`помощью вызова в, попытка проверить *подлинность результатов в ArgumentException: Необходимо указать*параметр "сигнинсчеме". Шаблон проекта, используемый в этом руководстве гарантирует, что это будет сделано.
* Если база данных сайта не был создан путем применения первоначальной миграции, вы получаете *сбой операции из базы данных при обработке запроса* ошибки. Выберите **Применить миграции** , чтобы создать базу данных, и обновите страницу, чтобы продолжить работу после ошибки.

## <a name="next-steps"></a>Следующие шаги

* В этой статье объясняется, как можно выполнить проверку подлинности с помощью Google. Можно выполнить аналогичный подход для проверки подлинности с помощью других поставщиков, перечисленных на [предыдущую страницу](xref:security/authentication/social/index).
* После публикации приложения в Azure сбросьте его `ClientSecret` в консоли Google API.
* Задайте `Authentication:Google:ClientId` и `Authentication:Google:ClientSecret` как параметры приложения на портале Azure. Система конфигурации предназначена для чтения разделов из переменных среды.
