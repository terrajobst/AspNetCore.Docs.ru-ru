---
title: Проверка подлинности в облаке с помощью Azure Active Directory B2C в ASP.NET Core
author: camsoper
description: Узнайте, как настроить проверку подлинности Azure Active Directory B2C с помощью ASP.NET Core.
ms.author: casoper
ms.custom: mvc
ms.date: 01/21/2019
uid: security/authentication/azure-ad-b2c
ms.openlocfilehash: 136fa47788456492a9a7fe6d9d9e5996c13e8c20
ms.sourcegitcommit: 9a129f5f3e31cc449742b164d5004894bfca90aa
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/06/2020
ms.locfileid: "78653620"
---
# <a name="cloud-authentication-with-azure-active-directory-b2c-in-aspnet-core"></a>Проверка подлинности в облаке с помощью Azure Active Directory B2C в ASP.NET Core

Автор [Кэм Сопер (Cam Soper)](https://twitter.com/camsoper)

[Azure Active Directory B2C](/azure/active-directory-b2c/active-directory-b2c-overview) (Azure AD B2C) — это решение для управления удостоверениями в облаке для веб-и мобильных приложений. Эта служба предоставляет проверку подлинности для приложений, размещенных в облаке и локальной. Типы проверки подлинности включают отдельные учетные записи, учетные записи социальных сетей и федеративные пользователи корпоративных учетных записей. Кроме того, Azure AD B2C может предоставлять многофакторную проверку подлинности с минимальной конфигурацией.

> [!TIP]
> Azure Active Directory (Azure AD) и Azure AD B2C являются отдельными предложениями продуктов. Клиент Azure AD представляет организацию, хотя клиент Azure AD B2C представляет коллекцию удостоверений для использования с приложениями проверяющей стороны. Дополнительные сведения см. в разделе [Azure AD B2C: часто](/azure/active-directory-b2c/active-directory-b2c-faqs)задаваемые вопросы.

Из этого руководства вы узнаете, как выполнять следующие задачи:

> [!div class="checklist"]
> * Создание клиента Azure Active Directory B2C
> * Регистрация приложения в Azure AD B2C
> * Создание ASP.NET Core веб-приложения, настроенного для использования клиента Azure AD B2C для проверки подлинности, с помощью Visual Studio
> * Настройка политик, управляющих поведением клиента Azure AD B2C

## <a name="prerequisites"></a>предварительные требования

Ниже приведены необходимые для этого пошагового руководства.

* [Подписка Microsoft Azure](https://azure.microsoft.com/free/?ref=microsoft.com&utm_source=microsoft.com&utm_medium=docs&utm_campaign=visualstudio)
* [Visual Studio 2019](https://visualstudio.microsoft.com/downloads/?utm_medium=microsoft&utm_source=docs.microsoft.com&utm_campaign=inline+link&utm_content=download+vs2019)

## <a name="create-the-azure-active-directory-b2c-tenant"></a>Создание клиента Azure Active Directory B2C

Создайте клиент Azure Active Directory B2C [, как описано в документации](/azure/active-directory-b2c/active-directory-b2c-get-started). При появлении сопоставления клиента с подпиской Azure является необязательным в этом руководстве.

## <a name="register-the-app-in-azure-ad-b2c"></a>Регистрация приложения в Azure AD B2C

В созданном клиенте Azure AD B2C Зарегистрируйте приложение, выполнив [действия, описанные в документации](/azure/active-directory-b2c/tutorial-register-applications#register-a-web-application) в разделе **Регистрация веб-приложения** . Прерывать в разделе **Создание секрета клиента веб-приложения** . Секрет клиента не требуется в этом руководстве. 

Используйте следующие значения:

| Параметр                       | Значение                     | Примечания                                                                                                                                                                                              |
|-------------------------------|---------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| **Название**                      | *&lt;имя приложения&gt;*        | Введите **имя** приложения, которое описывает ваше приложение для потребителей.                                                                                                                                 |
| **Включить веб-приложение или веб-интерфейс API** | Да                       |                                                                                                                                                                                                    |
| **Разрешить неявный поток**       | Да                       |                                                                                                                                                                                                    |
| **URL-адрес ответа**                 | `https://localhost:44300/signin-oidc` | URL-адреса ответа — это конечные точки, куда Azure AD B2C возвращает все токены, запрашиваемые вашим приложением. Visual Studio предоставляет URL-адрес ответа для использования. Пока введите `https://localhost:44300/signin-oidc`, чтобы завершить форму. |
| **URI кода приложения**                | Не указывайте               | Не требуется в этом руководстве.                                                                                                                                                                    |
| **Включить собственный клиент**     | нет                        |                                                                                                                                                                                                    |

> [!WARNING]
> При настройке URL-адреса ответа, отличного от localhost, следует учитывать [ограничения, которые разрешены в списке URL-адресов ответа](/azure/active-directory-b2c/tutorial-register-applications#register-a-web-application). 

После регистрации приложения отобразится список приложений в клиенте. Выберите приложение, которое было только что зарегистрировано. Щелкните значок **копирования** справа от поля **идентификатор приложения** , чтобы скопировать его в буфер обмена.

В настоящее время в клиенте Azure AD B2C больше не может быть настроено ничего, но оставьте окно браузера открытым. После создания ASP.NET Core приложения возникает дополнительная настройка.

## <a name="create-an-aspnet-core-app-in-visual-studio"></a>Создание приложения ASP.NET Core в Visual Studio

Шаблон веб-приложения Visual Studio можно настроить для использования клиента Azure AD B2C для проверки подлинности.

В Visual Studio:

1. Создайте новое веб-приложение ASP.NET Core. 
2. Выберите **веб-приложение** из списка шаблонов.
3. Нажмите кнопку **изменить проверку подлинности** .
    
    ![Кнопка "Изменить проверку подлинности"](./azure-ad-b2c/_static/changeauth.png)

4. В диалоговом окне **Изменение проверки подлинности** выберите **отдельные учетные записи пользователей**, а затем в раскрывающемся списке выберите **подключиться к существующему хранилищу пользователя в облаке** . 
    
    ![Диалоговое окно Изменение проверки подлинности](./azure-ad-b2c/_static/changeauthdialog.png)

5. Заполните форму следующими значениями:
    
    | Параметр                       | Значение                                                 |
    |-------------------------------|-------------------------------------------------------|
    | **Доменное имя**               | *&lt;доменное имя клиента B2C&gt;*          |
    | **идентификатор приложения**.            | *&lt;вставить идентификатор приложения из буфера обмена&gt;* |
    | **Путь обратного вызова**             | *&lt;использовать значение по умолчанию&gt;*                       |
    | **Политика регистрации или входа** | `B2C_1_SiUpIn`                                        |
    | **Сброс политики паролей**     | `B2C_1_SSPR`                                          |
    | **Изменение политики профиля**       | *&lt;оставьте пустым&gt;*                                 |
    
    Выберите ссылку **Копировать** рядом с **URI ответа** , чтобы скопировать универсальный код ресурса (URI) ответа в буфер обмена. Нажмите кнопку **ОК** , чтобы закрыть диалоговое окно **Изменение проверки подлинности** . Нажмите кнопку **ОК** , чтобы создать веб-приложение.

## <a name="finish-the-b2c-app-registration"></a>Завершение регистрации приложения B2C

Вернитесь в окно браузера, в котором все еще открыты свойства приложения B2C. Измените указанный ранее **URL-адрес временного ответа** на значение, скопированное из Visual Studio. Нажмите кнопку **сохранить** в верхней части окна.

> [!TIP]
> Если вы не скопировали URL-адрес ответа, используйте адрес HTTPS на вкладке "Отладка" в свойствах веб-проекта и добавьте значение **каллбаккпас** из *appSettings. JSON*.

## <a name="configure-policies"></a>Настройка политик

Выполните действия, описанные в Azure AD B2C документации, чтобы [создать политику регистрации или входа](/azure/active-directory-b2c/active-directory-b2c-reference-policies#user-flow-versions), а затем [Создайте политику сброса паролей](/azure/active-directory-b2c/active-directory-b2c-reference-policies#user-flow-versions). Используйте примеры значений, приведенные в документации по **поставщикам удостоверений**, **атрибутам регистрации**и **утверждениям приложений**. Использование кнопки **выполнить сейчас** для проверки политик, как описано в документации, является необязательным.

> [!WARNING]
> Убедитесь, что имена политик в точности соответствуют описанию в документации, так как эти политики использовались в диалоговом окне **Изменение проверки подлинности** в Visual Studio. Имена политик можно проверить в *appSettings. JSON*.

## <a name="configure-the-underlying-openidconnectoptionsjwtbearercookie-options"></a>Настройка базовых параметров Опенидконнектоптионс/JwtBearer/cookie

Чтобы настроить базовые параметры напрямую, используйте соответствующую константу схемы в `Startup.ConfigureServices`:

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

## <a name="run-the-app"></a>Запустите приложение

В Visual Studio нажмите клавишу **F5** , чтобы создать и запустить приложение. После запуска веб-приложения выберите **принять** , чтобы принять использование файлов cookie (при появлении запроса), а затем выберите **Вход**.

![Вход в приложение](./azure-ad-b2c/_static/signin.png)

Браузер перенаправляется на клиент Azure AD B2C. Войдите с помощью существующей учетной записи (если она была создана при тестировании политик) или выберите **зарегистрироваться сейчас** , чтобы создать новую учетную запись. **Забыт пароль?** ссылка используется для сброса забытого пароля.

![Вход в Azure Active Directory B2C](./azure-ad-b2c/_static/b2csts.png)

После успешного входа в систему браузер перенаправляется в веб-приложение.

![Успешно](./azure-ad-b2c/_static/success.png)

## <a name="next-steps"></a>Дальнейшие действия

В этом руководстве вы узнали, как выполнять следующие задачи:

> [!div class="checklist"]
> * Создание клиента Azure Active Directory B2C
> * Регистрация приложения в Azure AD B2C
> * Создание ASP.NET Core веб-приложения, настроенного для использования клиента Azure AD B2C для проверки подлинности, с помощью Visual Studio
> * Настройка политик, управляющих поведением клиента Azure AD B2C

Теперь, когда ASP.NET Core приложение настроено для использования Azure AD B2C для проверки подлинности, [атрибут авторизации](xref:security/authorization/simple) можно использовать для защиты приложения. Продолжайте разработку приложения, изучая следующие действия:

* [Настройка пользовательского интерфейса Azure AD B2C](/azure/active-directory-b2c/active-directory-b2c-reference-ui-customization).
* [Настройте требования к сложности пароля](/azure/active-directory-b2c/active-directory-b2c-reference-password-complexity).
* [Включите многофакторную проверку подлинности](/azure/active-directory-b2c/active-directory-b2c-reference-mfa).
* Настройте дополнительные поставщики удостоверений, такие как [Microsoft](/azure/active-directory-b2c/active-directory-b2c-setup-msa-app), [Facebook](/azure/active-directory-b2c/active-directory-b2c-setup-fb-app), [Google](/azure/active-directory-b2c/active-directory-b2c-setup-goog-app), [Amazon](/azure/active-directory-b2c/active-directory-b2c-setup-amzn-app), [Twitter](/azure/active-directory-b2c/active-directory-b2c-setup-twitter-app)и др.
* [Используйте API Graph Azure AD](/azure/active-directory-b2c/active-directory-b2c-devquickstarts-graph-dotnet) для получения дополнительных сведений о пользователе, таких как членство в группе, из клиента Azure AD B2C.
* [Защита ASP.NET Core веб-API с помощью Azure AD B2C](https://azure.microsoft.com/resources/samples/active-directory-b2c-dotnetcore-webapi/).
* [Вызов веб-API .NET из веб-приложения .NET с помощью Azure AD B2C](/azure/active-directory-b2c/active-directory-b2c-devquickstarts-web-api-dotnet).