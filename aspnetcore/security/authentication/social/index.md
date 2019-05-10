---
title: Проверка подлинности Facebook, Google и внешних поставщиков в ASP.NET Core
author: rick-anderson
description: В этом руководстве демонстрируется построение приложения ASP.NET Core 2.x с использованием OAuth 2.0 с внешними поставщиками проверки подлинности.
ms.author: riande
ms.custom: mvc
ms.date: 4/19/2019
uid: security/authentication/social/index
ms.openlocfilehash: 61482481358256dc9ddd1a0a894541040a8a452f
ms.sourcegitcommit: 5b0eca8c21550f95de3bb21096bd4fd4d9098026
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/27/2019
ms.locfileid: "64882009"
---
# <a name="facebook-google-and-external-provider-authentication-in-aspnet-core"></a>Проверка подлинности Facebook, Google и внешних поставщиков в ASP.NET Core

Авторы: [Валерий Новицкий](https://github.com/01binary) (Valeriy Novytskyy) и [Рик Андерсон](https://twitter.com/RickAndMSFT) (Rick Anderson)

В этом руководстве показано создание приложения ASP.NET Core 2.2, позволяющего пользователям выполнять вход с помощью OAuth 2.0 и учетных данных от внешних поставщиков проверки подлинности.

В следующих разделах рассматриваются такие поставщики, как [Facebook](xref:security/authentication/facebook-logins), [Twitter](xref:security/authentication/twitter-logins), [Google](xref:security/authentication/google-logins), и [Microsoft](xref:security/authentication/microsoft-logins). В сторонних пакетах также доступны другие поставщики, такие как [AspNet.Security.OAuth.Providers](https://github.com/aspnet-contrib/AspNet.Security.OAuth.Providers) и [AspNet.Security.OpenId.Providers](https://github.com/aspnet-contrib/AspNet.Security.OpenId.Providers).

![Значки социальных сетей для Facebook, Twitter, Google+ и Windows](index/_static/social.png)

Позволяет пользователям входить в систему с имеющимися учетными данными:
* Удобно для пользователей.
* Многие задачи, связанные с управлением процессом входа, передаются сторонним производителям. 

Демонстрацию того, как вход с использованием учетных данных социальных сетей помогает повысить трафик и количество конверсий, см. в примерах для [Facebook](https://www.facebook.com/unsupportedbrowser) и [Twitter](https://dev.twitter.com/resources/case-studies).

## <a name="create-a-new-aspnet-core-project"></a>Создание проекта ASP.NET Core

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

* В меню **Файл** Visual Studio откройте меню **Создать** > **Проект**.
* Создайте новое веб-приложение ASP.NET Core.
* Выберите в раскрывающемся списке **ASP.NET Core 2.2**, а затем **Веб-приложение**.
* Выберите **Изменить проверку подлинности** и задайте способ **Учетные записи отдельных пользователей**.

# <a name="visual-studio-codetabvisual-studio-code"></a>[Visual Studio Code.](#tab/visual-studio-code)

* Откройте [Интегрированный терминал](https://code.visualstudio.com/docs/editor/integrated-terminal).

* Смените каталог (`cd`) на папку, в которой будет содержаться проект.

* Выполните следующие команды:

  ```console
  dotnet new webapp -o WebApp1
  code -r WebApp1
  ```

  * Команда `dotnet new` создает новый проект Razor Pages в папке *WebApp1*.
  * Команда `code` открывает папку *WebApp1* в новом экземпляре Visual Studio Code.

  Появится диалоговое окно с предупреждением **В WebApp1 отсутствуют необходимые ресурсы для сборки и отладки. Добавить их?**

* Выберите ответ **Да**.

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[Visual Studio для Mac](#tab/visual-studio-mac)

В терминале выполните следующую команду:

<!-- TODO: update these instruction once mac support 2.2 projects -->

```console
dotnet new webapp -o WebApp1
```

Указанные выше команды используют [интерфейс командной строки .NET Core](/dotnet/core/tools/dotnet) для создания проекта Razor Pages.

## <a name="open-the-project"></a>Открытие проекта

В Visual Studio откройте меню **Файл > Открыть** и выберите файл *WebApp1.csproj*.

<!-- End of VS tabs -->

---

## <a name="apply-migrations"></a>Применение миграции

* Запустите приложение и щелкните ссылку **Регистрация**.
* Введите адрес электронной почты и пароль для новой учетной записи, а затем щелкните **Зарегистрироваться**.
* Следуйте инструкциям по применению миграции.

[!INCLUDE[Forward request information when behind a proxy or load balancer section](includes/forwarded-headers-middleware.md)]

## <a name="use-secretmanager-to-store-tokens-assigned-by-login-providers"></a>Хранение маркеров безопасности, предоставленных поставщиками входа, с помощью SecretManager

Поставщики входа социальных сетей назначают маркеры **идентификатора приложения** и **секрета приложения** в процессе регистрации. Точные имена маркеров зависят от поставщика. Эти маркеры соответствуют учетным данным, которые используются приложением для доступа к API. Маркеры предоставляют "секреты", которые можно подключить к конфигурации приложения с помощью [Secret Manager](xref:security/app-secrets#secret-manager) (Диспетчера секретов). Secret Manager — более безопасная альтернатива хранению маркеров в файле конфигурации, например в *appsettings.json*.

> [!IMPORTANT]
> Secret Manager предназначен только для разработки. Для хранения и защиты секретов Azure в ходе тестирования и непосредственной работы используйте [Поставщик конфигурации Azure Key Vault](xref:security/key-vault-configuration).

В разделе [Безопасное хранение секретов приложений во время разработки в ASP.NET Core](xref:security/app-secrets) описано, как хранить маркеры, назначаемые приведенными ниже поставщиками входа.

## <a name="setup-login-providers-required-by-your-application"></a>Настройка поставщиков входа, используемых приложением

В следующих разделах приводятся инструкции по настройке приложения для работы с соответствующими поставщиками:

* Инструкции для [Facebook](xref:security/authentication/facebook-logins)
* Инструкции для [Twitter](xref:security/authentication/twitter-logins)
* Инструкции для [Google](xref:security/authentication/google-logins)
* Инструкции для [Майкрософт](xref:security/authentication/microsoft-logins)
* Инструкции для [других поставщиков](xref:security/authentication/otherlogins)

[!INCLUDE[](includes/chain-auth-providers.md)]

## <a name="optionally-set-password"></a>Необязательная установка пароля

При регистрации с использованием внешнего поставщика входа у вас нет пароля, зарегистрированного в приложении. Благодаря этому вам не нужно создавать и запоминать пароль для сайта, однако при этом возникает зависимость от внешнего поставщика входа. Если внешний поставщик входа недоступен, вы не сможете войти на веб-сайт.

Чтобы создать пароль и войти с использованием адреса электронной почты, который был настроен при входе с использованием внешнего поставщика, выполните следующие действия:

* Щелкните ссылку **Здравствуйте, &lt;псевдоним электронной почты&gt;** в правом верхнем углу, чтобы перейти к представлению **Управление**.

![Представление управления веб-приложения](index/_static/pass1a.png)

* Выберите **Создать**.

![Настройте страницу пароля](index/_static/pass2a.png)

* Введите допустимый пароль, который будет использоваться для входа с применением этого адреса электронной почты.

## <a name="next-steps"></a>Следующие шаги

* В этой статье описывается внешняя проверка подлинности и приводятся предварительные требования для добавления внешних поставщиков входа в приложение ASP.NET Core.

* Инструкции по настройке учетных данных для входа, используемых вашим приложением, см. на соответствующих страницах поставщиков.

* Вы можете сохранить дополнительные данные о пользователях и их маркерах доступа и обновления. Для получения дополнительной информации см. <xref:security/authentication/social/additional-claims>.
