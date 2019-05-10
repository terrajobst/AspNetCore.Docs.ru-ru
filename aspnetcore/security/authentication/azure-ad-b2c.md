---
title: Проверка подлинности Cloud с Azure Active Directory B2C в ASP.NET Core
author: camsoper
description: Узнайте, как настроить проверку подлинности Azure Active Directory B2C с помощью ASP.NET Core.
ms.date: 02/27/2019
ms.custom: mvc
uid: security/authentication/azure-ad-b2c
ms.openlocfilehash: 86be999e02cfe34193bd594dcf89e8872590cca5
ms.sourcegitcommit: 5b0eca8c21550f95de3bb21096bd4fd4d9098026
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/27/2019
ms.locfileid: "64898021"
---
# <a name="cloud-authentication-with-azure-active-directory-b2c-in-aspnet-core"></a><span data-ttu-id="379ae-103">Проверка подлинности Cloud с Azure Active Directory B2C в ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="379ae-103">Cloud authentication with Azure Active Directory B2C in ASP.NET Core</span></span>

<span data-ttu-id="379ae-104">Автор [Кэм Сопер (Cam Soper)](https://twitter.com/camsoper)</span><span class="sxs-lookup"><span data-stu-id="379ae-104">By [Cam Soper](https://twitter.com/camsoper)</span></span>

<span data-ttu-id="379ae-105">[Azure Active Directory B2C](/azure/active-directory-b2c/active-directory-b2c-overview) (Azure AD B2C) является это облаке решение управления удостоверениями для Интернета и мобильных приложений.</span><span class="sxs-lookup"><span data-stu-id="379ae-105">[Azure Active Directory B2C](/azure/active-directory-b2c/active-directory-b2c-overview) (Azure AD B2C) is a cloud identity management solution for web and mobile apps.</span></span> <span data-ttu-id="379ae-106">Эта служба предоставляет проверку подлинности для приложений, размещенных в облаке и локальной.</span><span class="sxs-lookup"><span data-stu-id="379ae-106">The service provides authentication for apps hosted in the cloud and on-premises.</span></span> <span data-ttu-id="379ae-107">Типы проверки подлинности включают отдельные учетные записи, учетные записи социальных сетей и федеративные пользователи корпоративных учетных записей.</span><span class="sxs-lookup"><span data-stu-id="379ae-107">Authentication types include individual accounts, social network accounts, and federated enterprise accounts.</span></span> <span data-ttu-id="379ae-108">Кроме того Azure AD B2C обеспечивает многофакторную проверку подлинности с минимальной конфигурацией.</span><span class="sxs-lookup"><span data-stu-id="379ae-108">Additionally, Azure AD B2C can provide multi-factor authentication with minimal configuration.</span></span>

> [!TIP]
> <span data-ttu-id="379ae-109">Azure Active Directory (Azure AD) и Azure AD B2C являются отдельными предложениями продуктов.</span><span class="sxs-lookup"><span data-stu-id="379ae-109">Azure Active Directory (Azure AD) and Azure AD B2C are separate product offerings.</span></span> <span data-ttu-id="379ae-110">Клиент Azure AD представляет организацию, хотя клиент Azure AD B2C представляет коллекцию удостоверений для использования с приложениями проверяющей стороны.</span><span class="sxs-lookup"><span data-stu-id="379ae-110">An Azure AD tenant represents an organization, while an Azure AD B2C tenant represents a collection of identities to be used with relying party applications.</span></span> <span data-ttu-id="379ae-111">Дополнительные сведения см. в разделе [Azure AD B2C: Часто задаваемые вопросы (FAQ)](/azure/active-directory-b2c/active-directory-b2c-faqs).</span><span class="sxs-lookup"><span data-stu-id="379ae-111">To learn more, see [Azure AD B2C: Frequently asked questions (FAQ)](/azure/active-directory-b2c/active-directory-b2c-faqs).</span></span>

<span data-ttu-id="379ae-112">В этом руководстве описано, как:</span><span class="sxs-lookup"><span data-stu-id="379ae-112">In this tutorial, learn how to:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="379ae-113">Создание клиента Azure Active Directory B2C</span><span class="sxs-lookup"><span data-stu-id="379ae-113">Create an Azure Active Directory B2C tenant</span></span>
> * <span data-ttu-id="379ae-114">Регистрация приложения в Azure AD B2C</span><span class="sxs-lookup"><span data-stu-id="379ae-114">Register an app in Azure AD B2C</span></span>
> * <span data-ttu-id="379ae-115">Используйте Visual Studio для создания веб-приложение ASP.NET Core, настроены на использование клиентом Azure B2C для проверки подлинности</span><span class="sxs-lookup"><span data-stu-id="379ae-115">Use Visual Studio to create an ASP.NET Core web app configured to use the Azure AD B2C tenant for authentication</span></span>
> * <span data-ttu-id="379ae-116">Настройка политик, управляющих поведением клиента Azure AD B2C</span><span class="sxs-lookup"><span data-stu-id="379ae-116">Configure policies controlling the behavior of the Azure AD B2C tenant</span></span>

## <a name="prerequisites"></a><span data-ttu-id="379ae-117">Предварительные требования</span><span class="sxs-lookup"><span data-stu-id="379ae-117">Prerequisites</span></span>

<span data-ttu-id="379ae-118">Ниже приведены необходимые для этого пошагового руководства.</span><span class="sxs-lookup"><span data-stu-id="379ae-118">The following are required for this walkthrough:</span></span>

* [<span data-ttu-id="379ae-119">Подписки Microsoft Azure</span><span class="sxs-lookup"><span data-stu-id="379ae-119">Microsoft Azure subscription</span></span>](https://azure.microsoft.com/free/?ref=microsoft.com&utm_source=microsoft.com&utm_medium=docs&utm_campaign=visualstudio)
* <span data-ttu-id="379ae-120">[Visual Studio 2017](https://aka.ms/vsdownload?utm_source=mscom&utm_campaign=msdocs) (любой выпуск)</span><span class="sxs-lookup"><span data-stu-id="379ae-120">[Visual Studio 2017](https://aka.ms/vsdownload?utm_source=mscom&utm_campaign=msdocs) (any edition)</span></span>

## <a name="create-the-azure-active-directory-b2c-tenant"></a><span data-ttu-id="379ae-121">Создание клиента Azure Active Directory B2C</span><span class="sxs-lookup"><span data-stu-id="379ae-121">Create the Azure Active Directory B2C tenant</span></span>

<span data-ttu-id="379ae-122">Создание клиента Azure Active Directory B2C [как описано в документации по](/azure/active-directory-b2c/active-directory-b2c-get-started).</span><span class="sxs-lookup"><span data-stu-id="379ae-122">Create an Azure Active Directory B2C tenant [as described in the documentation](/azure/active-directory-b2c/active-directory-b2c-get-started).</span></span> <span data-ttu-id="379ae-123">При появлении сопоставления клиента с подпиской Azure является необязательным в этом руководстве.</span><span class="sxs-lookup"><span data-stu-id="379ae-123">When prompted, associating the tenant with an Azure subscription is optional for this tutorial.</span></span>

## <a name="register-the-app-in-azure-ad-b2c"></a><span data-ttu-id="379ae-124">Регистрация приложения в Azure AD B2C</span><span class="sxs-lookup"><span data-stu-id="379ae-124">Register the app in Azure AD B2C</span></span>

<span data-ttu-id="379ae-125">В только что созданный клиент Azure AD B2C, зарегистрируйте приложение, нажав [действия, описанные в документации по](/azure/active-directory-b2c/active-directory-b2c-app-registration#register-a-web-app) под **зарегистрировать веб-приложение** раздел.</span><span class="sxs-lookup"><span data-stu-id="379ae-125">In the newly created Azure AD B2C tenant, register your app using [the steps in the documentation](/azure/active-directory-b2c/active-directory-b2c-app-registration#register-a-web-app) under the **Register a web app** section.</span></span> <span data-ttu-id="379ae-126">Остановиться на **создать секрет клиента приложения web** раздел.</span><span class="sxs-lookup"><span data-stu-id="379ae-126">Stop at the **Create a web app client secret** section.</span></span> <span data-ttu-id="379ae-127">Секрет клиента не требуется в этом руководстве.</span><span class="sxs-lookup"><span data-stu-id="379ae-127">A client secret isn't required for this tutorial.</span></span> 

<span data-ttu-id="379ae-128">Используйте следующие значения:</span><span class="sxs-lookup"><span data-stu-id="379ae-128">Use the following values:</span></span>

| <span data-ttu-id="379ae-129">Параметр</span><span class="sxs-lookup"><span data-stu-id="379ae-129">Setting</span></span>                       | <span data-ttu-id="379ae-130">Значение</span><span class="sxs-lookup"><span data-stu-id="379ae-130">Value</span></span>                     | <span data-ttu-id="379ae-131">Примечания</span><span class="sxs-lookup"><span data-stu-id="379ae-131">Notes</span></span>                                                                                                                                                                                              |
|-------------------------------|---------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| <span data-ttu-id="379ae-132">**Name**</span><span class="sxs-lookup"><span data-stu-id="379ae-132">**Name**</span></span>                      | <span data-ttu-id="379ae-133">*&lt;Имя приложения&gt;*</span><span class="sxs-lookup"><span data-stu-id="379ae-133">*&lt;app name&gt;*</span></span>        | <span data-ttu-id="379ae-134">Введите **имя** для приложения, которые описывают приложение для потребителей.</span><span class="sxs-lookup"><span data-stu-id="379ae-134">Enter a **Name** for the app that describes your app to consumers.</span></span>                                                                                                                                 |
| <span data-ttu-id="379ae-135">**Включить веб-приложение или веб-API**</span><span class="sxs-lookup"><span data-stu-id="379ae-135">**Include web app / web API**</span></span> | <span data-ttu-id="379ae-136">Да</span><span class="sxs-lookup"><span data-stu-id="379ae-136">Yes</span></span>                       |                                                                                                                                                                                                    |
| <span data-ttu-id="379ae-137">**Разрешить неявный поток**</span><span class="sxs-lookup"><span data-stu-id="379ae-137">**Allow implicit flow**</span></span>       | <span data-ttu-id="379ae-138">Да</span><span class="sxs-lookup"><span data-stu-id="379ae-138">Yes</span></span>                       |                                                                                                                                                                                                    |
| <span data-ttu-id="379ae-139">**URL-адрес ответа**</span><span class="sxs-lookup"><span data-stu-id="379ae-139">**Reply URL**</span></span>                 | `https://localhost:44300/signin-oidc` | <span data-ttu-id="379ae-140">URL-адреса ответа — это конечные точки, куда Azure AD B2C возвращает все токены, запрашиваемые вашим приложением.</span><span class="sxs-lookup"><span data-stu-id="379ae-140">Reply URLs are endpoints where Azure AD B2C returns any tokens that your app requests.</span></span> <span data-ttu-id="379ae-141">Visual Studio предоставляет URL-адрес ответа для использования.</span><span class="sxs-lookup"><span data-stu-id="379ae-141">Visual Studio provides the Reply URL to use.</span></span> <span data-ttu-id="379ae-142">Теперь введите `https://localhost:44300/signin-oidc` нужно заполнить форму.</span><span class="sxs-lookup"><span data-stu-id="379ae-142">For now, enter `https://localhost:44300/signin-oidc` to complete the form.</span></span> |
| <span data-ttu-id="379ae-143">**URI идентификатора приложения**</span><span class="sxs-lookup"><span data-stu-id="379ae-143">**App ID URI**</span></span>                | <span data-ttu-id="379ae-144">Оставьте поле пустым</span><span class="sxs-lookup"><span data-stu-id="379ae-144">Leave blank</span></span>               | <span data-ttu-id="379ae-145">Не требуется в этом руководстве.</span><span class="sxs-lookup"><span data-stu-id="379ae-145">Not required for this tutorial.</span></span>                                                                                                                                                                    |
| <span data-ttu-id="379ae-146">**Включить собственный клиент**</span><span class="sxs-lookup"><span data-stu-id="379ae-146">**Include native client**</span></span>     | <span data-ttu-id="379ae-147">Нет</span><span class="sxs-lookup"><span data-stu-id="379ae-147">No</span></span>                        |                                                                                                                                                                                                    |

> [!WARNING]
> <span data-ttu-id="379ae-148">Если настройка URL-адреса ответа не относящиеся к localhost, необходимо учитывать [ограничения на том, что разрешено в списке URL-адрес ответа](/azure/active-directory-b2c/active-directory-b2c-app-registration#choosing-a-web-app-or-api-reply-url).</span><span class="sxs-lookup"><span data-stu-id="379ae-148">If setting up a non-localhost Reply URL, be aware of the [constraints on what is allowed in the Reply URL list](/azure/active-directory-b2c/active-directory-b2c-app-registration#choosing-a-web-app-or-api-reply-url).</span></span> 

<span data-ttu-id="379ae-149">После регистрации приложения, отображается список приложений в клиенте.</span><span class="sxs-lookup"><span data-stu-id="379ae-149">After the app is registered, the list of apps in the tenant is displayed.</span></span> <span data-ttu-id="379ae-150">Выберите приложение, которое только что был зарегистрирован.</span><span class="sxs-lookup"><span data-stu-id="379ae-150">Select the app that was just registered.</span></span> <span data-ttu-id="379ae-151">Выберите **копирования** значок справа от **идентификатор приложения** поле, чтобы скопировать его в буфер обмена.</span><span class="sxs-lookup"><span data-stu-id="379ae-151">Select the **Copy** icon to the right of the **Application ID** field to copy it to the clipboard.</span></span>

<span data-ttu-id="379ae-152">Ничего больше можно настроить в клиенте Azure AD B2C в настоящее время, но не закрывайте окно браузера.</span><span class="sxs-lookup"><span data-stu-id="379ae-152">Nothing more can be configured in the Azure AD B2C tenant at this time, but leave the browser window open.</span></span> <span data-ttu-id="379ae-153">После создания приложения ASP.NET Core есть дополнительные конфигурации.</span><span class="sxs-lookup"><span data-stu-id="379ae-153">There is more configuration after the ASP.NET Core app is created.</span></span>

## <a name="create-an-aspnet-core-app-in-visual-studio-2017"></a><span data-ttu-id="379ae-154">Создание приложения ASP.NET Core в Visual Studio 2017</span><span class="sxs-lookup"><span data-stu-id="379ae-154">Create an ASP.NET Core app in Visual Studio 2017</span></span>

<span data-ttu-id="379ae-155">Шаблон веб-приложения Visual Studio можно настроить для использования клиента Azure AD B2C для проверки подлинности.</span><span class="sxs-lookup"><span data-stu-id="379ae-155">The Visual Studio Web Application template can be configured to use the Azure AD B2C tenant for authentication.</span></span>

<span data-ttu-id="379ae-156">В Visual Studio сделайте следующее:</span><span class="sxs-lookup"><span data-stu-id="379ae-156">In Visual Studio:</span></span>

1. <span data-ttu-id="379ae-157">Создайте новое веб-приложение ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="379ae-157">Create a new ASP.NET Core Web Application.</span></span> 
2. <span data-ttu-id="379ae-158">Выберите **веб-приложение** из списка шаблонов.</span><span class="sxs-lookup"><span data-stu-id="379ae-158">Select **Web Application** from the list of templates.</span></span>
3. <span data-ttu-id="379ae-159">Выберите **изменить способ проверки подлинности** кнопки.</span><span class="sxs-lookup"><span data-stu-id="379ae-159">Select the **Change Authentication** button.</span></span>
    
    ![Кнопка "Изменить проверку подлинности"](./azure-ad-b2c/_static/changeauth.png)

4. <span data-ttu-id="379ae-161">В **изменить способ проверки подлинности** диалоговом окне выберите **учетные записи отдельных пользователей**, а затем выберите **подключение к существующему хранилищу пользователей в облаке** в раскрывающемся списке.</span><span class="sxs-lookup"><span data-stu-id="379ae-161">In the **Change Authentication** dialog, select **Individual User Accounts**, and then select **Connect to an existing user store in the cloud** in the dropdown.</span></span> 
    
    ![Диалоговое окно Изменение проверки подлинности](./azure-ad-b2c/_static/changeauthdialog.png)

5. <span data-ttu-id="379ae-163">Заполните форму следующими значениями:</span><span class="sxs-lookup"><span data-stu-id="379ae-163">Complete the form with the following values:</span></span>
    
    | <span data-ttu-id="379ae-164">Параметр</span><span class="sxs-lookup"><span data-stu-id="379ae-164">Setting</span></span>                       | <span data-ttu-id="379ae-165">Значение</span><span class="sxs-lookup"><span data-stu-id="379ae-165">Value</span></span>                                                 |
    |-------------------------------|-------------------------------------------------------|
    | <span data-ttu-id="379ae-166">**Имя домена**</span><span class="sxs-lookup"><span data-stu-id="379ae-166">**Domain Name**</span></span>               | <span data-ttu-id="379ae-167">*&lt;имя домена клиента B2C&gt;*</span><span class="sxs-lookup"><span data-stu-id="379ae-167">*&lt;the domain name of your B2C tenant&gt;*</span></span>          |
    | <span data-ttu-id="379ae-168">**Идентификатор приложения**</span><span class="sxs-lookup"><span data-stu-id="379ae-168">**Application ID**</span></span>            | <span data-ttu-id="379ae-169">*&lt;Вставьте идентификатор приложения из буфера обмена&gt;*</span><span class="sxs-lookup"><span data-stu-id="379ae-169">*&lt;paste the Application ID from the clipboard&gt;*</span></span> |
    | <span data-ttu-id="379ae-170">**Путь обратной связи**</span><span class="sxs-lookup"><span data-stu-id="379ae-170">**Callback Path**</span></span>             | <span data-ttu-id="379ae-171">*&lt;Используйте значение по умолчанию&gt;*</span><span class="sxs-lookup"><span data-stu-id="379ae-171">*&lt;use the default value&gt;*</span></span>                       |
    | <span data-ttu-id="379ae-172">**Политики регистрации или входа в систему**</span><span class="sxs-lookup"><span data-stu-id="379ae-172">**Sign-up or sign-in policy**</span></span> | `B2C_1_SiUpIn`                                        |
    | <span data-ttu-id="379ae-173">**Политику сброса паролей**</span><span class="sxs-lookup"><span data-stu-id="379ae-173">**Reset password policy**</span></span>     | `B2C_1_SSPR`                                          |
    | <span data-ttu-id="379ae-174">**Изменение профиля политики**</span><span class="sxs-lookup"><span data-stu-id="379ae-174">**Edit profile policy**</span></span>       | <span data-ttu-id="379ae-175">*&lt;Оставьте поле пустым&gt;*</span><span class="sxs-lookup"><span data-stu-id="379ae-175">*&lt;leave blank&gt;*</span></span>                                 |
    
    <span data-ttu-id="379ae-176">Выберите **копирования** ссылку рядом с полем **URI ответа** скопировать URI ответа в буфер обмена.</span><span class="sxs-lookup"><span data-stu-id="379ae-176">Select the **Copy** link next to **Reply URI** to copy the Reply URI to the clipboard.</span></span> <span data-ttu-id="379ae-177">Выберите **ОК** закрыть **изменить способ проверки подлинности** диалоговое окно.</span><span class="sxs-lookup"><span data-stu-id="379ae-177">Select **OK** to close the **Change Authentication** dialog.</span></span> <span data-ttu-id="379ae-178">Выберите **ОК** для создания веб-приложения.</span><span class="sxs-lookup"><span data-stu-id="379ae-178">Select **OK** to create the web app.</span></span>

## <a name="finish-the-b2c-app-registration"></a><span data-ttu-id="379ae-179">Завершите регистрацию приложения B2C</span><span class="sxs-lookup"><span data-stu-id="379ae-179">Finish the B2C app registration</span></span>

<span data-ttu-id="379ae-180">Вернитесь в окно браузера со свойствами приложения B2C еще открыт.</span><span class="sxs-lookup"><span data-stu-id="379ae-180">Return to the browser window with the B2C app properties still open.</span></span> <span data-ttu-id="379ae-181">Измените временную **URL-адрес ответа** указано ранее, чтобы значение копируется из Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="379ae-181">Change the temporary **Reply URL** specified earlier to the value copied from Visual Studio.</span></span> <span data-ttu-id="379ae-182">Выберите **Сохранить** в верхней части окна.</span><span class="sxs-lookup"><span data-stu-id="379ae-182">Select **Save** at the top of the window.</span></span>

> [!TIP]
> <span data-ttu-id="379ae-183">Если вы не скопировали URL-адрес ответа, используйте HTTPS-адрес на вкладке "Отладка" в свойствах Интернета проекта и добавление **CallbackPath** значение из *appsettings.json*.</span><span class="sxs-lookup"><span data-stu-id="379ae-183">If you didn't copy the Reply URL, use the HTTPS address from the Debug tab in the web project properties, and append the **CallbackPath** value from *appsettings.json*.</span></span>

## <a name="configure-policies"></a><span data-ttu-id="379ae-184">Настройка политик</span><span class="sxs-lookup"><span data-stu-id="379ae-184">Configure policies</span></span>

<span data-ttu-id="379ae-185">Следуйте инструкциям в документации по Azure AD B2C для [создать политику регистрации или входа в систему](/azure/active-directory-b2c/active-directory-b2c-reference-policies#create-a-sign-up-or-sign-in-policy), а затем [создать политику сброса паролей](/azure/active-directory-b2c/active-directory-b2c-reference-policies#create-a-password-reset-policy).</span><span class="sxs-lookup"><span data-stu-id="379ae-185">Use the steps in the Azure AD B2C documentation to [create a sign-up or sign-in policy](/azure/active-directory-b2c/active-directory-b2c-reference-policies#create-a-sign-up-or-sign-in-policy), and then [create a password reset policy](/azure/active-directory-b2c/active-directory-b2c-reference-policies#create-a-password-reset-policy).</span></span> <span data-ttu-id="379ae-186">Используйте примеры значений, приведенных в документации для **поставщиков удостоверений**, **атрибуты регистрации**, и **утверждения приложения**.</span><span class="sxs-lookup"><span data-stu-id="379ae-186">Use the example values provided in the documentation for **Identity providers**, **Sign-up attributes**, and **Application claims**.</span></span> <span data-ttu-id="379ae-187">С помощью **запустить сейчас** кнопку для проверки политики, как описано в документации является необязательным.</span><span class="sxs-lookup"><span data-stu-id="379ae-187">Using the **Run now** button to test the policies as described in the documentation is optional.</span></span>

> [!WARNING]
> <span data-ttu-id="379ae-188">Убедитесь, имена политик именно так, как описано в документации, как эти политики были использованы в **изменить способ проверки подлинности** диалоговое окно в Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="379ae-188">Ensure the policy names are exactly as described in the documentation, as those policies were used in the **Change Authentication** dialog in Visual Studio.</span></span> <span data-ttu-id="379ae-189">Имена политик можно проверить в *appsettings.json*.</span><span class="sxs-lookup"><span data-stu-id="379ae-189">The policy names can be verified in *appsettings.json*.</span></span>

## <a name="configure-the-underlying-openidconnectoptionsjwtbearercookie-options"></a><span data-ttu-id="379ae-190">Настройка базовых параметров OpenIdConnectOptions/JwtBearer/файл Cookie</span><span class="sxs-lookup"><span data-stu-id="379ae-190">Configure the underlying OpenIdConnectOptions/JwtBearer/Cookie options</span></span>

<span data-ttu-id="379ae-191">Чтобы напрямую настроить базовые параметры, используйте соответствующую схему константы в `Startup.ConfigureServices`:</span><span class="sxs-lookup"><span data-stu-id="379ae-191">To configure the underlying options directly, use the appropriate scheme constant in `Startup.ConfigureServices`:</span></span>

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

## <a name="run-the-app"></a><span data-ttu-id="379ae-192">Запуск приложения</span><span class="sxs-lookup"><span data-stu-id="379ae-192">Run the app</span></span>

<span data-ttu-id="379ae-193">В Visual Studio нажмите клавишу **F5** для сборки и запуска приложения.</span><span class="sxs-lookup"><span data-stu-id="379ae-193">In Visual Studio, press **F5** to build and run the app.</span></span> <span data-ttu-id="379ae-194">После запуска веб-приложения выберите **Accept** для принятия на использование файлов cookie (при появлении соответствующего запроса), а затем выберите **вход**.</span><span class="sxs-lookup"><span data-stu-id="379ae-194">After the web app launches, select **Accept** to accept the use of cookies (if prompted), and then select **Sign in**.</span></span>

![Вход в приложение](./azure-ad-b2c/_static/signin.png)

<span data-ttu-id="379ae-196">Браузер перенаправляет клиента Azure AD B2C.</span><span class="sxs-lookup"><span data-stu-id="379ae-196">The browser redirects to the Azure AD B2C tenant.</span></span> <span data-ttu-id="379ae-197">Войдите с помощью существующей учетной записи (если один была создана тестирования политики) или выберите **Зарегистрируйтесь сейчас** для создания новой учетной записи.</span><span class="sxs-lookup"><span data-stu-id="379ae-197">Sign in with an existing account (if one was created testing the policies) or select **Sign up now** to create a new account.</span></span> <span data-ttu-id="379ae-198">**Забыли пароль?** связи используются для сбрасывать забытый пароль.</span><span class="sxs-lookup"><span data-stu-id="379ae-198">The **Forgot your password?** link is used to reset a forgotten password.</span></span>

![Вход в Azure AD B2C](./azure-ad-b2c/_static/b2csts.png)

<span data-ttu-id="379ae-200">После успешного входа, браузер перенаправляет веб-приложения.</span><span class="sxs-lookup"><span data-stu-id="379ae-200">After successfully signing in, the browser redirects to the web app.</span></span>

![Success](./azure-ad-b2c/_static/success.png)

## <a name="next-steps"></a><span data-ttu-id="379ae-202">Следующие шаги</span><span class="sxs-lookup"><span data-stu-id="379ae-202">Next steps</span></span>

<span data-ttu-id="379ae-203">В этом руководстве вы узнали, как:</span><span class="sxs-lookup"><span data-stu-id="379ae-203">In this tutorial, you learned how to:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="379ae-204">Создание клиента Azure Active Directory B2C</span><span class="sxs-lookup"><span data-stu-id="379ae-204">Create an Azure Active Directory B2C tenant</span></span>
> * <span data-ttu-id="379ae-205">Регистрация приложения в Azure AD B2C</span><span class="sxs-lookup"><span data-stu-id="379ae-205">Register an app in Azure AD B2C</span></span>
> * <span data-ttu-id="379ae-206">Используйте Visual Studio для создания веб-приложение ASP.NET Core настроен для использования клиента Azure AD B2C для проверки подлинности</span><span class="sxs-lookup"><span data-stu-id="379ae-206">Use Visual Studio to create an ASP.NET Core Web Application configured to use the Azure AD B2C tenant for authentication</span></span>
> * <span data-ttu-id="379ae-207">Настройка политик, управляющих поведением клиента Azure AD B2C</span><span class="sxs-lookup"><span data-stu-id="379ae-207">Configure policies controlling the behavior of the Azure AD B2C tenant</span></span>

<span data-ttu-id="379ae-208">Теперь, когда приложение ASP.NET Core настроен для использования Azure AD B2C для проверки подлинности, [атрибут Authorize](xref:security/authorization/simple) может использоваться для защиты приложения.</span><span class="sxs-lookup"><span data-stu-id="379ae-208">Now that the ASP.NET Core app is configured to use Azure AD B2C for authentication, the [Authorize attribute](xref:security/authorization/simple) can be used to secure your app.</span></span> <span data-ttu-id="379ae-209">Продолжите разработку приложения, Научившись:</span><span class="sxs-lookup"><span data-stu-id="379ae-209">Continue developing your app by learning to:</span></span>

* <span data-ttu-id="379ae-210">[Настройка пользовательского интерфейса Azure AD B2C](/azure/active-directory-b2c/active-directory-b2c-reference-ui-customization).</span><span class="sxs-lookup"><span data-stu-id="379ae-210">[Customize the Azure AD B2C user interface](/azure/active-directory-b2c/active-directory-b2c-reference-ui-customization).</span></span>
* <span data-ttu-id="379ae-211">[Настройка требований к сложности пароля](/azure/active-directory-b2c/active-directory-b2c-reference-password-complexity).</span><span class="sxs-lookup"><span data-stu-id="379ae-211">[Configure password complexity requirements](/azure/active-directory-b2c/active-directory-b2c-reference-password-complexity).</span></span>
* <span data-ttu-id="379ae-212">[Включить многофакторную проверку подлинности](/azure/active-directory-b2c/active-directory-b2c-reference-mfa).</span><span class="sxs-lookup"><span data-stu-id="379ae-212">[Enable multi-factor authentication](/azure/active-directory-b2c/active-directory-b2c-reference-mfa).</span></span>
* <span data-ttu-id="379ae-213">Настройка дополнительных поставщиков удостоверений, таких как [Microsoft](/azure/active-directory-b2c/active-directory-b2c-setup-msa-app), [Facebook](/azure/active-directory-b2c/active-directory-b2c-setup-fb-app), [Google](/azure/active-directory-b2c/active-directory-b2c-setup-goog-app), [Amazon](/azure/active-directory-b2c/active-directory-b2c-setup-amzn-app), [Twitter ](/azure/active-directory-b2c/active-directory-b2c-setup-twitter-app)и другие.</span><span class="sxs-lookup"><span data-stu-id="379ae-213">Configure additional identity providers, such as [Microsoft](/azure/active-directory-b2c/active-directory-b2c-setup-msa-app), [Facebook](/azure/active-directory-b2c/active-directory-b2c-setup-fb-app), [Google](/azure/active-directory-b2c/active-directory-b2c-setup-goog-app), [Amazon](/azure/active-directory-b2c/active-directory-b2c-setup-amzn-app), [Twitter](/azure/active-directory-b2c/active-directory-b2c-setup-twitter-app), and others.</span></span>
* <span data-ttu-id="379ae-214">[С помощью API Azure AD Graph](/azure/active-directory-b2c/active-directory-b2c-devquickstarts-graph-dotnet) для получения дополнительных сведений о пользователе, например членство в группе, из клиента Azure AD B2C.</span><span class="sxs-lookup"><span data-stu-id="379ae-214">[Use the Azure AD Graph API](/azure/active-directory-b2c/active-directory-b2c-devquickstarts-graph-dotnet) to retrieve additional user information, such as group membership, from the Azure AD B2C tenant.</span></span>
* <span data-ttu-id="379ae-215">[Защита с помощью Azure AD B2C веб-API ASP.NET Core](xref:security/authentication/azure-ad-b2c-webapi).</span><span class="sxs-lookup"><span data-stu-id="379ae-215">[Secure an ASP.NET Core web API using Azure AD B2C](xref:security/authentication/azure-ad-b2c-webapi).</span></span>
* <span data-ttu-id="379ae-216">[Вызов веб-API .NET из веб-приложения .NET с помощью Azure AD B2C](/azure/active-directory-b2c/active-directory-b2c-devquickstarts-web-api-dotnet).</span><span class="sxs-lookup"><span data-stu-id="379ae-216">[Call a .NET web API from a .NET web app using Azure AD B2C](/azure/active-directory-b2c/active-directory-b2c-devquickstarts-web-api-dotnet).</span></span>
