---
title: Настройка внешнего входа в Twitter с помощью ASP.NET Core
author: rick-anderson
description: В этом руководстве демонстрируется интеграция аутентификации пользователя учетной записи Twitter с существующим ASP.NET Core приложением.
ms.author: riande
ms.custom: mvc
ms.date: 05/11/2019
uid: security/authentication/twitter-logins
ms.openlocfilehash: 5182f1647acb664bf35f086fcddbe909559a62f7
ms.sourcegitcommit: 215954a638d24124f791024c66fd4fb9109fd380
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 09/18/2019
ms.locfileid: "71082303"
---
# <a name="twitter-external-sign-in-setup-with-aspnet-core"></a>Настройка внешнего входа в Twitter с помощью ASP.NET Core

Авторы: [Валерий Новицкий](https://github.com/01binary) (Valeriy Novytskyy) и [Рик Андерсон](https://twitter.com/RickAndMSFT) (Rick Anderson)

В этом примере показано, как разрешить пользователям [входить с помощью учетной записи Twitter](https://dev.twitter.com/web/sign-in/desktop-browser) , используя пример проекта ASP.NET Core 2,2, созданный на [предыдущей странице](xref:security/authentication/social/index).

## <a name="create-the-app-in-twitter"></a>Создание приложения в Twitter

* Перейдите к [ https://apps.twitter.com/ ](https://apps.twitter.com/) и войдите в систему. Если у вас еще нет учетной записи Twitter, используйте ссылку **[зарегистрироваться сейчас](https://twitter.com/signup)** , чтобы создать ее.

* Коснитесь элемента **создать новое приложение** и введите URI **имени**приложения, **описания** и общедоступного **веб-сайта** (это может быть временным, пока не будет зарегистрировано доменное имя):

* Введите URI разработки с `/signin-twitter` добавлением в **допустимое поле URI перенаправления OAuth** (например: `https://webapp128.azurewebsites.net/signin-twitter`). Схема проверки подлинности Twitter, настроенная далее в этом примере, `/signin-twitter` автоматически обрабатывает запросы по маршруту для реализации потока OAuth.

  > [!NOTE]
  > Сегмент `/signin-twitter` URI задается в качестве обратного вызова по умолчанию для поставщика проверки подлинности Twitter. URI обратного вызова по умолчанию можно изменить при настройке по промежуточного слоя проверки подлинности Twitter с помощью унаследованного свойства [ремотеаусентикатионоптионс. каллбаккпас](/dotnet/api/microsoft.aspnetcore.authentication.remoteauthenticationoptions.callbackpath) класса [твиттероптионс](/dotnet/api/microsoft.aspnetcore.authentication.twitter.twitteroptions) .

* Заполните оставшуюся часть формы и коснитесь пункта **создать приложение Twitter**. Отобразятся сведения о новом приложении:

## <a name="storing-twitter-consumer-api-key-and-secret"></a>Хранение ключа и секрета API потребителя Twitter

Выполните следующие команды для безопасного хранения `ClientId` и `ClientSecret` использования [диспетчера секретов](xref:security/app-secrets):

```dotnetcli
dotnet user-secrets set Authentication:Twitter:ConsumerAPIKey <Key>
dotnet user-secrets set Authentication:Twitter:ConsumerSecret <Secret>
```

Свяжите конфиденциальные параметры, `Consumer Key` такие `Consumer Secret` как Twitter, и конфигурацию приложения с помощью [диспетчера секретов](xref:security/app-secrets). Для целей этого примера назовите маркеры `Authentication:Twitter:ConsumerKey` и. `Authentication:Twitter:ConsumerSecret`

Эти маркеры можно найти на вкладке **ключи и маркеры доступа** после создания нового приложения Twitter.

## <a name="configure-twitter-authentication"></a>Настройка проверки подлинности Twitter

Добавьте службу Twitter в `ConfigureServices` метод в файле *Startup.CS* :

[!code-csharp[](~/security/authentication/social/social-code/StartupTwitter.cs?name=snippet&highlight=10-14)]

[!INCLUDE [default settings configuration](includes/default-settings.md)]

[!INCLUDE[](includes/chain-auth-providers.md)]

Дополнительные сведения о параметрах конфигурации, поддерживаемых проверкой подлинности в Twitter, см. в справочнике по API [твиттероптионс](/dotnet/api/microsoft.aspnetcore.builder.twitteroptions) . Это может использоваться для запроса различные сведения о пользователе.

## <a name="sign-in-with-twitter"></a>Вход с помощью Twitter

Запустите приложение и выберите **Вход**. Появится возможность входа с помощью Twitter:

При щелчке **Twitter** в Twitter перенаправления для проверки подлинности:

После ввода учетных данных Twitter вы будете перенаправлены на веб-сайт, на котором можно задать свой адрес электронной почты.

Теперь вы выполнили вход с использованием учетных данных Twitter:

[!INCLUDE[Forward request information when behind a proxy or load balancer section](includes/forwarded-headers-middleware.md)]

## <a name="troubleshooting"></a>Устранение неполадок

* **Только ASP.NET Core 2. x:** Если удостоверение не настроено `services.AddIdentity` с `ConfigureServices`помощью вызова в, попытка проверки подлинности приведет *к появлению исключения ArgumentException: Необходимо указать*параметр "сигнинсчеме". Шаблон проекта, используемый в этом образце, гарантирует, что это будет сделано.
* Если база данных сайта не был создан путем применения первоначальной миграции, вы получите *сбой операции из базы данных при обработке запроса* ошибки. Коснитесь **применить миграции** для создания базы данных и обновить, чтобы продолжить выполнение после ошибки.

## <a name="next-steps"></a>Следующие шаги

* В этой статье показано, как можно пройти проверку подлинности в Twitter. Можно выполнить аналогичный подход для проверки подлинности с помощью других поставщиков, перечисленных на [предыдущую страницу](xref:security/authentication/social/index).

* После публикации веб-сайта в веб-приложение Azure необходимо сбросить настройки `ConsumerSecret` на портале разработчика Twitter.

* Задайте `Authentication:Twitter:ConsumerKey` и `Authentication:Twitter:ConsumerSecret` как параметры приложения на портале Azure. Система конфигурации предназначена для чтения разделов из переменных среды.
