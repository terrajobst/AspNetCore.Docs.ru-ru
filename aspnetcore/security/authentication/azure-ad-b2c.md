---
title: Проверки подлинности в облаке с Azure Active Directory B2C в ASP.NET Core
author: camsoper
description: Узнайте, как настроить проверку подлинности Azure Active Directory B2C с ASP.NET Core.
manager: wpickett
ms.date: 01/25/2018
ms.topic: tutorial
ms.technology: aspnet
ms.prod: asp.net-core
ms.custom: mvc
uid: security/authentication/azure-ad-b2c
ms.openlocfilehash: a7bad452a68cf7fe7aa81645d79a0ee9e7719fe7
ms.sourcegitcommit: 493a215355576cfa481773365de021bcf04bb9c7
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/15/2018
ms.locfileid: "29905079"
---
# <a name="cloud-authentication-with-azure-active-directory-b2c-in-aspnet-core"></a>Проверки подлинности в облаке с Azure Active Directory B2C в ASP.NET Core

Автор [Кэм Сопер (Cam Soper)](https://twitter.com/camsoper)

[Azure Active Directory B2C](/azure/active-directory-b2c/active-directory-b2c-overview) (Azure AD B2C) — это Облачное решение управления удостоверения для веб- и мобильных приложений. Служба обеспечивает проверку подлинности для приложений, размещенных в облаке и в локальной среде. Типы проверки подлинности включают отдельные учетные записи, учетные записи социальных сетей и федеративные учетные записи предприятия. Кроме того Azure AD B2C можно указать многофакторной проверки подлинности с минимальной конфигурацией.

> [!TIP]
> Azure Active Directory (Azure AD) Azure AD B2C являются отдельные продукты. Клиент Azure AD представляет организации, а клиент Azure AD B2C представляет коллекцию удостоверений для использования с приложениями проверяющей стороны. Дополнительные сведения см. в разделе [Azure AD B2C: часто задаваемые вопросы (FAQ)](/azure/active-directory-b2c/active-directory-b2c-faqs).

В этом учебнике показано, как:

> [!div class="checklist"]
> * Создание клиента Azure Active Directory B2C
> * Регистрация приложения в Azure AD B2C
> * Visual Studio можно использовать для создания веб-приложение ASP.NET Core настроен для использования проверки подлинности клиента Azure AD B2C
> * Настройка политик управления поведением клиента Azure AD B2C

## <a name="prerequisites"></a>Предварительные требования

Ниже приведены для данного пошагового руководства требуется:

* [Подписка на Microsoft Azure](https://azure.microsoft.com/free/?ref=microsoft.com&utm_source=microsoft.com&utm_medium=docs&utm_campaign=visualstudio)
* [Visual Studio 2017 г](https://aka.ms/vsdownload?utm_source=mscom&utm_campaign=msdocs) (любой выпуск)

## <a name="create-the-azure-active-directory-b2c-tenant"></a>Создание клиента Azure Active Directory B2C

Создание клиента Azure Active Directory B2C [как описано в документации по](/azure/active-directory-b2c/active-directory-b2c-get-started). При появлении запроса связь клиента с подпиской Azure является необязательным для этого учебника.

## <a name="register-the-app-in-azure-ad-b2c"></a>Регистрация приложения в Azure AD B2C

В только что созданный клиента Azure AD B2C, зарегистрировать приложение с помощью [действия, описанные в документации по](/azure/active-directory-b2c/active-directory-b2c-app-registration#register-a-web-app) под **зарегистрировать веб-приложение** раздела. Остановиться на **создать секрет клиента приложения web** раздела. В этом учебнике секрета клиента не требуется. 

Используйте следующие значения:

| Параметр                       | Значение                     | Примечания                                                                                                                                                                                              |
|-------------------------------|---------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| **Name**                      | *&lt;Имя приложения&gt;*        | Введите **имя** для приложения, описывающих приложение пользователям.                                                                                                                                 |
| **Включить веб-приложения или веб-API** | Да                       |                                                                                                                                                                                                    |
| **Разрешить неявного потока**       | Да                       |                                                                                                                                                                                                    |
| **URL-адрес ответа**                 | `https://localhost:44300` | URL-адреса ответа, конечные точки, где Azure AD B2C возвращает токенами, которые запрашивает приложение. Visual Studio предоставляет URL-адрес ответа для использования. Теперь введите `https://localhost:44300` для заполнения формы. |
| **URI ИД приложения**                | Не указывайте               | Не требуется для этого учебника.                                                                                                                                                                    |
| **Включить собственный клиент**     | Нет                        |                                                                                                                                                                                                    |

> [!WARNING]
> Если настройка URL-адреса ответа не localhost, помните о [ограничения на то, что может быть в списке URL-адрес ответа](/azure/active-directory-b2c/active-directory-b2c-app-registration#choosing-a-web-app-or-api-reply-url). 

После регистрации приложения отображается список управляемых приложений в клиенте. Выберите приложение, которое только что было зарегистрировано. Выберите **копирования** значок справа от **идентификатор приложения** поле, чтобы скопировать его в буфер обмена.

Ничего более можно настроить в клиенте Azure AD B2C в данный момент, но не закрывайте окно браузера. После создания приложения ASP.NET Core нет дополнительные конфигурации.

## <a name="create-an-aspnet-core-app-in-visual-studio-2017"></a>Создание приложения ASP.NET Core в Visual Studio 2017 г.

Шаблон веб-приложения Visual Studio можно настроить для использования проверки подлинности клиента Azure AD B2C.

В Visual Studio:

1. Создайте новое веб-приложение ASP.NET Core. 
2. Выберите **веб-приложение** из списка шаблонов.
3. Выберите **изменить аутентификацию** кнопки.
    
    ![Кнопка изменения проверки подлинности](./azure-ad-b2c/_static/changeauth.png)

4. В **изменить проверку подлинности** диалогового окна выберите **отдельных учетных записей пользователей**, а затем выберите **подключиться к существующему хранилищу пользователей в облаке** в раскрывающемся списке. 
    
    ![Диалоговое окно Изменение проверки подлинности](./azure-ad-b2c/_static/changeauthdialog.png)

5. Заполните форму со следующими значениями:
    
    | Параметр                       | Значение                                                 |
    |-------------------------------|-------------------------------------------------------|
    | **Имя домена**               | *&lt;имя домена клиента B2C&gt;*          |
    | **Идентификатор приложения**            | *&lt;Вставьте идентификатор приложения из буфера обмена&gt;* |
    | **Путь обратного вызова**             | *&lt;использовать значение по умолчанию&gt;*                       |
    | **Политики регистрации или входа в систему** | `B2C_1_SiUpIn`                                        |
    | **Политика сброса пароля**     | `B2C_1_SSPR`                                          |
    | **Изменение политики профиля**       | *&lt;Не указывайте&gt;*                                 |
    
    Выберите **копирования** ссылку рядом с **ответа Reply URI** для копирования в буфер обмена URI ответа. Выберите **ОК** закрыть **изменить аутентификацию** диалогового окна. Выберите **ОК** для создания веб-приложения.

## <a name="finish-the-b2c-app-registration"></a>Регистрация приложения B2C Готово

Вернитесь в окно браузера со свойствами B2C приложение все еще открыт. Измените временную **URL-адрес ответа** указано ранее, чтобы значение копируется из Visual Studio. Выберите **Сохранить** в верхней части окна.

> [!TIP]
> Если не удалось скопировать URL-адрес ответа, используйте адрес SSL на вкладке «Отладка» в свойствах проекта веб и добавить **CallbackPath** значение из *appsettings.json*.

## <a name="configure-policies"></a>Настройка политик

Следуйте инструкциям в документации по Azure AD B2C для [создать политику регистрации или входа в](/azure/active-directory-b2c/active-directory-b2c-reference-policies#create-a-sign-up-or-sign-in-policy), а затем [Создание политики сброса паролей](/azure/active-directory-b2c/active-directory-b2c-reference-policies#create-a-password-reset-policy). Использовать пример значения, предоставленные в документации по **Поставщики удостоверений**, **регистрации атрибуты**, и **утверждений приложения**. С помощью **запустить сейчас** кнопку для тестирования политик, как описано в документации является необязательным.

> [!WARNING]
> Убедитесь, имена политик точно так, как описано в документации, как эти политики были использованы в **изменить аутентификацию** диалоговое окно в Visual Studio. Имена можно проверить в *appsettings.json*.

## <a name="run-the-app"></a>Запуск приложения

В Visual Studio нажмите клавишу **F5** для построения и запуска приложения. После запуска веб-приложения выберите **входа**.

![Вход в приложение](./azure-ad-b2c/_static/signin.png)

Перенаправляет браузер клиента Azure AD B2C. Войдите с помощью существующей учетной записи (если один создан тестирования политики) или выберите **Зарегистрируйтесь сейчас** для создания новой учетной записи. **Забыли пароль?** связь используется для сброса забытого пароля.

![Имя входа Azure AD B2C](./azure-ad-b2c/_static/b2csts.png)

После успешного входа в веб-приложение перенаправляет браузер.

![Success](./azure-ad-b2c/_static/success.png)

## <a name="next-steps"></a>Следующие шаги

В этом руководстве вы узнали, как:

> [!div class="checklist"]
> * Создание клиента Azure Active Directory B2C
> * Регистрация приложения в Azure AD B2C
> * Visual Studio можно использовать для создания веб-приложения ASP.NET Core настроен для использования проверки подлинности клиента Azure AD B2C
> * Настройка политик управления поведением клиента Azure AD B2C

Теперь, когда приложение ASP.NET Core настраивается для использования при проверке подлинности Azure AD B2C [авторизовать атрибут](xref:security/authorization/simple) может использоваться для обеспечения безопасности приложения. Продолжать разработку приложения, обучение, чтобы:

* [Настройка пользовательского интерфейса Azure AD B2C](/azure/active-directory-b2c/active-directory-b2c-reference-ui-customization).
* [Настройте требования к сложности пароля](/azure/active-directory-b2c/active-directory-b2c-reference-password-complexity).
* [Включение многофакторной проверки подлинности](/azure/active-directory-b2c/active-directory-b2c-reference-mfa).
* Настройка дополнительных поставщиков, такие как [Microsoft](/azure/active-directory-b2c/active-directory-b2c-setup-msa-app), [Facebook](/azure/active-directory-b2c/active-directory-b2c-setup-fb-app), [Google](/azure/active-directory-b2c/active-directory-b2c-setup-goog-app), [Amazon](/azure/active-directory-b2c/active-directory-b2c-setup-amzn-app), [Twitter ](/azure/active-directory-b2c/active-directory-b2c-setup-twitter-app)и др.
* [Использовать API Azure AD Graph](/azure/active-directory-b2c/active-directory-b2c-devquickstarts-graph-dotnet) для получения дополнительных сведений, таких как членство в группе из клиента Azure AD B2C.
* [Защита с помощью Azure AD B2C веб-API ASP.NET Core](xref:security/authentication/azure-ad-b2c-webapi).
* [Вызов веб-API .NET из веб-приложения .NET с помощью Azure AD B2C](/azure/active-directory-b2c/active-directory-b2c-devquickstarts-web-api-dotnet).
