---
title: Twitter внешнего входа в программу установки с помощью ASP.NET Core
author: rick-anderson
description: В этом учебнике показано интеграции проверки подлинности пользователя учетной записи Twitter в существующее приложение ASP.NET Core.
ms.author: riande
ms.custom: mvc
ms.date: 05/11/2019
uid: security/authentication/twitter-logins
ms.openlocfilehash: d816ed27898639b0af6896a51ac035d5526c5d29
ms.sourcegitcommit: 8516b586541e6ba402e57228e356639b85dfb2b9
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 07/11/2019
ms.locfileid: "67814075"
---
# <a name="twitter-external-sign-in-setup-with-aspnet-core"></a>Twitter внешнего входа в программу установки с помощью ASP.NET Core

Авторы: [Валерий Новицкий](https://github.com/01binary) (Valeriy Novytskyy) и [Рик Андерсон](https://twitter.com/RickAndMSFT) (Rick Anderson)

В этом примере показано, как разрешить пользователям проходить [войдите с помощью своей учетной записью Twitter](https://dev.twitter.com/web/sign-in/desktop-browser) используя образец проекта ASP.NET Core 2.2 создан на [предыдущую страницу](xref:security/authentication/social/index).

## <a name="create-the-app-in-twitter"></a>Создайте приложение в Twitter

* Перейдите к [ https://apps.twitter.com/ ](https://apps.twitter.com/) и войдите в систему. Если у вас нет учетной записи Twitter, используйте **[Зарегистрируйтесь сейчас](https://twitter.com/signup)** ссылку, чтобы создать его.

* Коснитесь **создать новое приложение** и заполните приложения **имя**, **описание** и общедоступных **веб-сайт** URI (это могут быть временными пока не будут Зарегистрируйте доменное имя):

* Введите URI разработки с `/signin-twitter` добавляется в **допустимый URI перенаправления OAuth** поле (например: `https://webapp128.azurewebsites.net/signin-twitter`). Схема проверки подлинности Twitter, Настройка описывается далее в этом образце автоматически будет обрабатывать запросы на `/signin-twitter` маршрута, чтобы реализовать поток OAuth.

  > [!NOTE]
  > Сегмент URI `/signin-twitter` задан в качестве обратного вызова по умолчанию поставщика проверки подлинности Twitter. URI обратного вызова по умолчанию можно изменить при настройке по промежуточного слоя проверки подлинности Twitter с помощью наследуемого [RemoteAuthenticationOptions.CallbackPath](/dotnet/api/microsoft.aspnetcore.authentication.remoteauthenticationoptions.callbackpath) свойство [TwitterOptions](/dotnet/api/microsoft.aspnetcore.authentication.twitter.twitteroptions) класс.

* Заполните оставшиеся поля формы и коснитесь **создать приложение Twitter**. Отображаются сведения о новом приложения:

## <a name="storing-twitter-consumer-api-key-and-secret"></a>Хранение потребителей API Twitter ключа и секрета

Выполните следующие команды, чтобы обеспечить безопасное хранение `ClientId` и `ClientSecret` с помощью [Secret Manager](xref:security/app-secrets):

```console
dotnet user-secrets set Authentication:Twitter:ConsumerAPIKey <Key>
dotnet user-secrets set Authentication:Twitter:ConsumerAPISecret <Secret>
```

Связать конфиденциальные параметры, такие как Twitter `Consumer Key` и `Consumer Secret` для конфигурации приложения с помощью [Secret Manager](xref:security/app-secrets). В целях этого примера назовите токены `Authentication:Twitter:ConsumerKey` и `Authentication:Twitter:ConsumerSecret`.

Эти маркеры можно найти на **ключи и токены доступа** вкладку после создания нового приложения Twitter:

## <a name="configure-twitter-authentication"></a>Настройка проверки подлинности Twitter

Добавление службы Twitter в `ConfigureServices` метод в *Startup.cs* файла:

[!code-csharp[](~/security/authentication/social/social-code/StartupTwitter.cs?name=snippet&highlight=10-14)]

[!INCLUDE [default settings configuration](includes/default-settings.md)]

[!INCLUDE[](includes/chain-auth-providers.md)]

См. в разделе [TwitterOptions](/dotnet/api/microsoft.aspnetcore.builder.twitteroptions) Справочник по API, Дополнительные сведения о параметрах конфигурации, поддерживаемых проверкой подлинности Twitter. Это может использоваться для запроса различные сведения о пользователе.

## <a name="sign-in-with-twitter"></a>Войдите с помощью Twitter

Запустите приложение и выберите **вход**. Появится возможность войти с помощью Twitter:

Щелкнув **Twitter** перенаправляет Twitter для проверки подлинности:

После ввода учетных данных Twitter, вы будете перенаправлены обратно на веб-узел, где вы можете задать свой адрес электронной почты.

Теперь вы вошли с использованием учетных данных Twitter:

[!INCLUDE[Forward request information when behind a proxy or load balancer section](includes/forwarded-headers-middleware.md)]

## <a name="troubleshooting"></a>Устранение неполадок

* **ASP.NET Core 2.x только:** Если удостоверение не настроена, вызвав `services.AddIdentity` в `ConfigureServices`, пытающиеся выполнить проверку подлинности приведет к *ArgumentException: Необходимо указать параметр «SignInScheme»* . Шаблон проекта, используемый в этом примере гарантирует, что это будет сделано.
* Если база данных сайта не был создан путем применения первоначальной миграции, вы получите *сбой операции из базы данных при обработке запроса* ошибки. Коснитесь **применить миграции** для создания базы данных и обновить, чтобы продолжить выполнение после ошибки.

## <a name="next-steps"></a>Следующие шаги

* В этой статье объясняется, как можно выполнить проверку подлинности с помощью Twitter. Можно выполнить аналогичный подход для проверки подлинности с помощью других поставщиков, перечисленных на [предыдущую страницу](xref:security/authentication/social/index).

* После публикации веб-сайт веб-приложение Azure, необходимо сбросить `ConsumerSecret` на портале разработчика Twitter.

* Задайте `Authentication:Twitter:ConsumerKey` и `Authentication:Twitter:ConsumerSecret` как параметры приложения на портале Azure. Система конфигурации предназначена для чтения разделов из переменных среды.