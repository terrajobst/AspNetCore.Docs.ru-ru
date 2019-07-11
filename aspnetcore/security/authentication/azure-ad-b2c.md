---
title: Проверка подлинности Cloud с Azure Active Directory B2C в ASP.NET Core
author: camsoper
description: Узнайте, как настроить проверку подлинности Azure Active Directory B2C с помощью ASP.NET Core.
ms.author: casoper
ms.custom: mvc
ms.date: 02/27/2019
uid: security/authentication/azure-ad-b2c
ms.openlocfilehash: 54117bf0dd45305d060eef5fecfb98ed45f8ecdb
ms.sourcegitcommit: 8516b586541e6ba402e57228e356639b85dfb2b9
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 07/11/2019
ms.locfileid: "67815301"
---
# <a name="cloud-authentication-with-azure-active-directory-b2c-in-aspnet-core"></a>Проверка подлинности Cloud с Azure Active Directory B2C в ASP.NET Core

Автор [Кэм Сопер (Cam Soper)](https://twitter.com/camsoper)

[Azure Active Directory B2C](/azure/active-directory-b2c/active-directory-b2c-overview) (Azure AD B2C) является это облаке решение управления удостоверениями для Интернета и мобильных приложений. Эта служба предоставляет проверку подлинности для приложений, размещенных в облаке и локальной. Типы проверки подлинности включают отдельные учетные записи, учетные записи социальных сетей и федеративные пользователи корпоративных учетных записей. Кроме того Azure AD B2C обеспечивает многофакторную проверку подлинности с минимальной конфигурацией.

> [!TIP]
> Azure Active Directory (Azure AD) и Azure AD B2C являются отдельными предложениями продуктов. Клиент Azure AD представляет организацию, хотя клиент Azure AD B2C представляет коллекцию удостоверений для использования с приложениями проверяющей стороны. Дополнительные сведения см. в разделе [Azure AD B2C: Часто задаваемые вопросы (FAQ)](/azure/active-directory-b2c/active-directory-b2c-faqs).

В этом руководстве описано, как:

> [!div class="checklist"]
> * Создание клиента Azure Active Directory B2C
> * Регистрация приложения в Azure AD B2C
> * Используйте Visual Studio для создания веб-приложение ASP.NET Core, настроены на использование клиентом Azure B2C для проверки подлинности
> * Настройка политик, управляющих поведением клиента Azure AD B2C

## <a name="prerequisites"></a>Предварительные требования

Ниже приведены необходимые для этого пошагового руководства.

* [Подписки Microsoft Azure](https://azure.microsoft.com/free/?ref=microsoft.com&utm_source=microsoft.com&utm_medium=docs&utm_campaign=visualstudio)
* [Visual Studio 2019](https://visualstudio.microsoft.com/downloads/?utm_medium=microsoft&utm_source=docs.microsoft.com&utm_campaign=inline+link&utm_content=download+vs2019)

## <a name="create-the-azure-active-directory-b2c-tenant"></a>Создание клиента Azure Active Directory B2C

Создание клиента Azure Active Directory B2C [как описано в документации по](/azure/active-directory-b2c/active-directory-b2c-get-started). При появлении сопоставления клиента с подпиской Azure является необязательным в этом руководстве.

## <a name="register-the-app-in-azure-ad-b2c"></a>Регистрация приложения в Azure AD B2C

В только что созданный клиент Azure AD B2C, зарегистрируйте приложение, нажав [действия, описанные в документации по](/azure/active-directory-b2c/tutorial-register-applications#register-a-web-application) под **зарегистрировать веб-приложение** раздел. Остановиться на **создать секрет клиента приложения web** раздел. Секрет клиента не требуется в этом руководстве. 

Используйте следующие значения:

| Параметр                       | Значение                     | Примечания                                                                                                                                                                                              |
|-------------------------------|---------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| **Name**                      | *&lt;Имя приложения&gt;*        | Введите **имя** для приложения, которые описывают приложение для потребителей.                                                                                                                                 |
| **Включить веб-приложение или веб-API** | Да                       |                                                                                                                                                                                                    |
| **Разрешить неявный поток**       | Да                       |                                                                                                                                                                                                    |
| **URL-адрес ответа**                 | `https://localhost:44300/signin-oidc` | URL-адреса ответа — это конечные точки, куда Azure AD B2C возвращает все токены, запрашиваемые вашим приложением. Visual Studio предоставляет URL-адрес ответа для использования. Теперь введите `https://localhost:44300/signin-oidc` нужно заполнить форму. |
| **URI идентификатора приложения**                | Оставьте поле пустым               | Не требуется в этом руководстве.                                                                                                                                                                    |
| **Включить собственный клиент**     | Нет                        |                                                                                                                                                                                                    |

> [!WARNING]
> Если настройка URL-адреса ответа не относящиеся к localhost, необходимо учитывать [ограничения на том, что разрешено в списке URL-адрес ответа](/azure/active-directory-b2c/tutorial-register-applications#register-a-web-application). 

После регистрации приложения, отображается список приложений в клиенте. Выберите приложение, которое только что был зарегистрирован. Выберите **копирования** значок справа от **идентификатор приложения** поле, чтобы скопировать его в буфер обмена.

Ничего больше можно настроить в клиенте Azure AD B2C в настоящее время, но не закрывайте окно браузера. После создания приложения ASP.NET Core есть дополнительные конфигурации.

## <a name="create-an-aspnet-core-app-in-visual-studio"></a>Создание приложения ASP.NET Core в Visual Studio

Шаблон веб-приложения Visual Studio можно настроить для использования клиента Azure AD B2C для проверки подлинности.

В Visual Studio сделайте следующее:

1. Создайте новое веб-приложение ASP.NET Core. 
2. Выберите **веб-приложение** из списка шаблонов.
3. Выберите **изменить способ проверки подлинности** кнопки.
    
    ![Кнопка "Изменить проверку подлинности"](./azure-ad-b2c/_static/changeauth.png)

4. В **изменить способ проверки подлинности** диалоговом окне выберите **учетные записи отдельных пользователей**, а затем выберите **подключение к существующему хранилищу пользователей в облаке** в раскрывающемся списке. 
    
    ![Диалоговое окно Изменение проверки подлинности](./azure-ad-b2c/_static/changeauthdialog.png)

5. Заполните форму следующими значениями:
    
    | Параметр                       | Значение                                                 |
    |-------------------------------|-------------------------------------------------------|
    | **Имя домена**               | *&lt;имя домена клиента B2C&gt;*          |
    | **Идентификатор приложения**            | *&lt;Вставьте идентификатор приложения из буфера обмена&gt;* |
    | **Путь обратной связи**             | *&lt;Используйте значение по умолчанию&gt;*                       |
    | **Политики регистрации или входа в систему** | `B2C_1_SiUpIn`                                        |
    | **Политику сброса паролей**     | `B2C_1_SSPR`                                          |
    | **Изменение профиля политики**       | *&lt;Оставьте поле пустым&gt;*                                 |
    
    Выберите **копирования** ссылку рядом с полем **URI ответа** скопировать URI ответа в буфер обмена. Выберите **ОК** закрыть **изменить способ проверки подлинности** диалоговое окно. Выберите **ОК** для создания веб-приложения.

## <a name="finish-the-b2c-app-registration"></a>Завершите регистрацию приложения B2C

Вернитесь в окно браузера со свойствами приложения B2C еще открыт. Измените временную **URL-адрес ответа** указано ранее, чтобы значение копируется из Visual Studio. Выберите **Сохранить** в верхней части окна.

> [!TIP]
> Если вы не скопировали URL-адрес ответа, используйте HTTPS-адрес на вкладке "Отладка" в свойствах Интернета проекта и добавление **CallbackPath** значение из *appsettings.json*.

## <a name="configure-policies"></a>Настройка политик

Следуйте инструкциям в документации по Azure AD B2C для [создать политику регистрации или входа в систему](/azure/active-directory-b2c/active-directory-b2c-reference-policies#user-flow-versions), а затем [создать политику сброса паролей](/azure/active-directory-b2c/active-directory-b2c-reference-policies#user-flow-versions). Используйте примеры значений, приведенных в документации для **поставщиков удостоверений**, **атрибуты регистрации**, и **утверждения приложения**. С помощью **запустить сейчас** кнопку для проверки политики, как описано в документации является необязательным.

> [!WARNING]
> Убедитесь, имена политик именно так, как описано в документации, как эти политики были использованы в **изменить способ проверки подлинности** диалоговое окно в Visual Studio. Имена политик можно проверить в *appsettings.json*.

## <a name="configure-the-underlying-openidconnectoptionsjwtbearercookie-options"></a>Настройка базовых параметров OpenIdConnectOptions/JwtBearer/файл Cookie

Чтобы напрямую настроить базовые параметры, используйте соответствующую схему константы в `Startup.ConfigureServices`:

```csharp
services.Configure<OpenIdConnectOptions>(
    AzureAD[B2C]Defaults.OpenIdScheme, options => 
    {
        // Omitted for brevity
    });

services.Configure<CookieAuthenticationOptions>(
    AzureAD[B2C]Defaults.CookieScheme, options => 
    {
        // Omitted for brevity
    });

services.Configure<JwtBearerOptions>(
    AzureAD[B2C]Defaults.JwtBearerAuthenticationScheme, options => 
    {
        // Omitted for brevity
    });
```

## <a name="run-the-app"></a>Запуск приложения

В Visual Studio нажмите клавишу **F5** для сборки и запуска приложения. После запуска веб-приложения выберите **Accept** для принятия на использование файлов cookie (при появлении соответствующего запроса), а затем выберите **вход**.

![Вход в приложение](./azure-ad-b2c/_static/signin.png)

Браузер перенаправляет клиента Azure AD B2C. Войдите с помощью существующей учетной записи (если один была создана тестирования политики) или выберите **Зарегистрируйтесь сейчас** для создания новой учетной записи. **Забыли пароль?** связи используются для сбрасывать забытый пароль.

![Вход в Azure AD B2C](./azure-ad-b2c/_static/b2csts.png)

После успешного входа, браузер перенаправляет веб-приложения.

![Success](./azure-ad-b2c/_static/success.png)

## <a name="next-steps"></a>Следующие шаги

В этом руководстве вы узнали, как:

> [!div class="checklist"]
> * Создание клиента Azure Active Directory B2C
> * Регистрация приложения в Azure AD B2C
> * Используйте Visual Studio для создания веб-приложение ASP.NET Core настроен для использования клиента Azure AD B2C для проверки подлинности
> * Настройка политик, управляющих поведением клиента Azure AD B2C

Теперь, когда приложение ASP.NET Core настроен для использования Azure AD B2C для проверки подлинности, [атрибут Authorize](xref:security/authorization/simple) может использоваться для защиты приложения. Продолжите разработку приложения, Научившись:

* [Настройка пользовательского интерфейса Azure AD B2C](/azure/active-directory-b2c/active-directory-b2c-reference-ui-customization).
* [Настройка требований к сложности пароля](/azure/active-directory-b2c/active-directory-b2c-reference-password-complexity).
* [Включить многофакторную проверку подлинности](/azure/active-directory-b2c/active-directory-b2c-reference-mfa).
* Настройка дополнительных поставщиков удостоверений, таких как [Microsoft](/azure/active-directory-b2c/active-directory-b2c-setup-msa-app), [Facebook](/azure/active-directory-b2c/active-directory-b2c-setup-fb-app), [Google](/azure/active-directory-b2c/active-directory-b2c-setup-goog-app), [Amazon](/azure/active-directory-b2c/active-directory-b2c-setup-amzn-app), [Twitter ](/azure/active-directory-b2c/active-directory-b2c-setup-twitter-app)и другие.
* [С помощью API Azure AD Graph](/azure/active-directory-b2c/active-directory-b2c-devquickstarts-graph-dotnet) для получения дополнительных сведений о пользователе, например членство в группе, из клиента Azure AD B2C.
* [Защита с помощью Azure AD B2C веб-API ASP.NET Core](xref:security/authentication/azure-ad-b2c-webapi).
* [Вызов веб-API .NET из веб-приложения .NET с помощью Azure AD B2C](/azure/active-directory-b2c/active-directory-b2c-devquickstarts-web-api-dotnet).
