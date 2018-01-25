---
uid: aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/single-sign-on
title: "Единый вход (Создание реальных облачных приложений в Azure) | Документы Microsoft"
author: MikeWasson
description: "Построение реального мира облачными приложениями с помощью Azure электронная книга основан на разработанный Скотт Гатри презентации. Объясняет, 13 шаблоны и рекомендации, которые он может..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/12/2014
ms.topic: article
ms.assetid: 7d82d5e9-0619-4f22-9e03-32a6d52940a5
ms.technology: 
ms.prod: .net-framework
msc.legacyurl: /aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/single-sign-on
msc.type: authoredcontent
ms.openlocfilehash: b3640c94a8ae9ede330c0fe6a392acb5843cb65c
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/24/2018
---
<a name="single-sign-on-building-real-world-cloud-apps-with-azure"></a><span data-ttu-id="8c72a-104">Единый вход (Создание реальных облачных приложений в Azure)</span><span class="sxs-lookup"><span data-stu-id="8c72a-104">Single Sign-On (Building Real-World Cloud Apps with Azure)</span></span>
====================
<span data-ttu-id="8c72a-105">по [Mike Wasson](https://github.com/MikeWasson), [Рик Андерсон](https://github.com/Rick-Anderson), [Tom Dykstra](https://github.com/tdykstra)</span><span class="sxs-lookup"><span data-stu-id="8c72a-105">by [Mike Wasson](https://github.com/MikeWasson), [Rick Anderson](https://github.com/Rick-Anderson), [Tom Dykstra](https://github.com/tdykstra)</span></span>

<span data-ttu-id="8c72a-106">[Загрузка решение проект](http://code.msdn.microsoft.com/Fix-It-app-for-Building-cdd80df4) или [Загрузите электронную](http://blogs.msdn.com/b/microsoft_press/archive/2014/07/23/free-ebook-building-cloud-apps-with-microsoft-azure.aspx)</span><span class="sxs-lookup"><span data-stu-id="8c72a-106">[Download Fix It Project](http://code.msdn.microsoft.com/Fix-It-app-for-Building-cdd80df4) or [Download E-book](http://blogs.msdn.com/b/microsoft_press/archive/2014/07/23/free-ebook-building-cloud-apps-with-microsoft-azure.aspx)</span></span>

> <span data-ttu-id="8c72a-107">**Построение реального мира облачными приложениями с помощью Azure** электронная книга основана на представления, разработанный Скотт Гатри.</span><span class="sxs-lookup"><span data-stu-id="8c72a-107">The **Building Real World Cloud Apps with Azure** e-book is based on a presentation developed by Scott Guthrie.</span></span> <span data-ttu-id="8c72a-108">Объясняются 13 шаблоны и рекомендации, которые помогут вам быть успешной разработки веб-приложений для облака.</span><span class="sxs-lookup"><span data-stu-id="8c72a-108">It explains 13 patterns and practices that can help you be successful developing web apps for the cloud.</span></span> <span data-ttu-id="8c72a-109">Сведения об электронных книг см. в разделе [первой главы](introduction.md).</span><span class="sxs-lookup"><span data-stu-id="8c72a-109">For information about the e-book, see [the first chapter](introduction.md).</span></span>


<span data-ttu-id="8c72a-110">Существует много проблем безопасности, нужно принять во внимание при разработке приложения облака, но для этой серии мы сосредоточимся на несколько: единый вход.</span><span class="sxs-lookup"><span data-stu-id="8c72a-110">There are many security issues to think about when you're developing a cloud app, but for this series we'll focus on just one: single sign-on.</span></span> <span data-ttu-id="8c72a-111">Люди вопрос часто уточните это: «основном создающему приложений для сотрудников компании; как разместить эти приложения в облаке и разрешить им использовать ту же модель безопасности, Мои сотрудники знать и использовать в локальной среде, когда они выполняются приложения, по-прежнему размещены за брандмауэром?»</span><span class="sxs-lookup"><span data-stu-id="8c72a-111">A question people often ask is this: "I'm primarily building apps for the employees of my company; how do I host these apps in the cloud and still enable them to use the same security model that my employees know and use in the on-premises environment when they're running apps that are hosted inside the firewall?"</span></span> <span data-ttu-id="8c72a-112">Одним из способов, мы можем добавить этот сценарий называется Azure Active Directory (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="8c72a-112">One of the ways we enable this scenario is called Azure Active Directory (Azure AD).</span></span> <span data-ttu-id="8c72a-113">Azure AD позволяет выполнять корпоративные бизнес приложения (LOB) доступны через Интернет, и позволяет внести эти приложения также бизнес-партнерами.</span><span class="sxs-lookup"><span data-stu-id="8c72a-113">Azure AD enables you to make enterprise line-of-business (LOB) apps available over the Internet, and it enables you to make these apps available to business partners as well.</span></span>

## <a name="introduction-to-azure-ad"></a><span data-ttu-id="8c72a-114">Введение в Azure AD</span><span class="sxs-lookup"><span data-stu-id="8c72a-114">Introduction to Azure AD</span></span>

<span data-ttu-id="8c72a-115">[Azure AD](https://docs.microsoft.com/azure/active-directory/) предоставляет [Active Directory](https://msdn.microsoft.com/library/windows/desktop/aa746492.aspx) в облаке.</span><span class="sxs-lookup"><span data-stu-id="8c72a-115">[Azure AD](https://docs.microsoft.com/azure/active-directory/) provides [Active Directory](https://msdn.microsoft.com/library/windows/desktop/aa746492.aspx) in the cloud.</span></span> <span data-ttu-id="8c72a-116">Ниже приведены основные функции:</span><span class="sxs-lookup"><span data-stu-id="8c72a-116">Key features include the following:</span></span>

- <span data-ttu-id="8c72a-117">Она интегрируется с локальной Active Directory.</span><span class="sxs-lookup"><span data-stu-id="8c72a-117">It integrates with on-premises Active Directory.</span></span>
- <span data-ttu-id="8c72a-118">Она позволяет единого входа вместе со своими приложениями.</span><span class="sxs-lookup"><span data-stu-id="8c72a-118">It enables single sign-on with your apps.</span></span>
- <span data-ttu-id="8c72a-119">Он поддерживает открытых стандартах, таких как [SAML](http://en.wikipedia.org/wiki/SAML_2.0), [WS-Fed](http://en.wikipedia.org/wiki/WS-Federation), и [OAuth 2.0](http://oauth.net/2/).</span><span class="sxs-lookup"><span data-stu-id="8c72a-119">It supports open standards such as [SAML](http://en.wikipedia.org/wiki/SAML_2.0), [WS-Fed](http://en.wikipedia.org/wiki/WS-Federation), and [OAuth 2.0](http://oauth.net/2/).</span></span>
- <span data-ttu-id="8c72a-120">Он поддерживает Enterprise [Graph REST API](https://msdn.microsoft.com/library/hh974476.aspx).</span><span class="sxs-lookup"><span data-stu-id="8c72a-120">It supports Enterprise [Graph REST API](https://msdn.microsoft.com/library/hh974476.aspx).</span></span>

<span data-ttu-id="8c72a-121">Предположим, что у вас есть в локальную среду Windows Server Active Directory, который позволяет сотрудники могут входить в приложениях интрасети:</span><span class="sxs-lookup"><span data-stu-id="8c72a-121">Suppose you have an on-premises Windows Server Active Directory environment that you use to enable employees to sign on to Intranet apps:</span></span>

![](single-sign-on/_static/image1.png)

<span data-ttu-id="8c72a-122">Azure AD позволяет выполнять является создайте каталог в облаке.</span><span class="sxs-lookup"><span data-stu-id="8c72a-122">What Azure AD enables you to do is create a directory in the cloud.</span></span> <span data-ttu-id="8c72a-123">Это бесплатная функция и легко настроить.</span><span class="sxs-lookup"><span data-stu-id="8c72a-123">It's a free feature and easy to set up.</span></span>

<span data-ttu-id="8c72a-124">Может быть полностью независимым от вашей локальной Active Directory; можно поместить все, кто в его и проверку подлинности их в Интернет-приложений.</span><span class="sxs-lookup"><span data-stu-id="8c72a-124">It can be entirely independent from your on-premises Active Directory; you can put anyone you want in it and authenticate them in Internet apps.</span></span>

![Windows Azure Active Directory](single-sign-on/_static/image2.png)

<span data-ttu-id="8c72a-126">Или его можно интегрировать с локальным AD.</span><span class="sxs-lookup"><span data-stu-id="8c72a-126">Or you can integrate it with your on-premises AD.</span></span>

![AD и WAAD интеграции](single-sign-on/_static/image3.png)

<span data-ttu-id="8c72a-128">Теперь всех сотрудников, которые могут проходить проверку подлинности в локальной среде могут также проходить проверку подлинности в Интернете — без необходимости открывать брандмауэр или развертывание всех новых серверах в центре обработки данных.</span><span class="sxs-lookup"><span data-stu-id="8c72a-128">Now all the employees who can authenticate on-premises can also authenticate over the Internet – without you having to open up a firewall or deploy any new servers in your data center.</span></span> <span data-ttu-id="8c72a-129">Можно продолжать пользоваться преимуществами все существующей среды Active Directory, известно, в настоящее время использует для предоставления единого входа к внутренним приложениям возможность.</span><span class="sxs-lookup"><span data-stu-id="8c72a-129">You can continue to leverage all the existing Active Directory environment that you know and use today to give your internal apps single-sign on capability.</span></span>

<span data-ttu-id="8c72a-130">После внесения этого подключения между AD и Azure AD, можно также включить веб-приложений и мобильных устройств для проверки подлинности ваших сотрудников в облаке и приложения сторонних производителей, таких как Office 365, SalesForce.com или Google apps для приема можно включить в учетные данные сотрудников.</span><span class="sxs-lookup"><span data-stu-id="8c72a-130">Once you've made this connection between AD and Azure AD, you can also enable your web apps and your mobile devices to authenticate your employees in the cloud, and you can enable third-party apps, such as Office 365, SalesForce.com, or Google apps, to accept your employees' credentials.</span></span> <span data-ttu-id="8c72a-131">Если вы используете Office 365, уже настроек с помощью Azure AD, так как Office 365 использует Azure AD для проверки подлинности и авторизации.</span><span class="sxs-lookup"><span data-stu-id="8c72a-131">If you're using Office 365, you're already set up with Azure AD because Office 365 uses Azure AD for authentication and authorization.</span></span>

![сторонние приложения](single-sign-on/_static/image4.png)

<span data-ttu-id="8c72a-133">Преимущество этого подхода, каждый раз организации добавляет или удаляет пользователя, или пользователь изменяет пароль, использовать такой же процесс, используемых в настоящее время в локальной среде.</span><span class="sxs-lookup"><span data-stu-id="8c72a-133">The beauty of this approach is that any time your organization adds or deletes a user, or a user changes a password, you use the same process that you use today in your on-premises environment.</span></span> <span data-ttu-id="8c72a-134">Все из локальных AD изменения автоматически распространяются на облачной среде.</span><span class="sxs-lookup"><span data-stu-id="8c72a-134">All of your on-premises AD changes are automatically propagated to the cloud environment.</span></span>

<span data-ttu-id="8c72a-135">Если вашей компании с помощью или перемещения в Office 365, хорошо то, что у вас будет создан автоматически, так как Office 365 использует Azure AD для проверки подлинности Azure AD.</span><span class="sxs-lookup"><span data-stu-id="8c72a-135">If your company is using or moving to Office 365, the good news is that you'll have Azure AD set up automatically because Office 365 uses Azure AD for authentication.</span></span> <span data-ttu-id="8c72a-136">Поэтому можно легко использовать в собственных приложениях же проверки подлинности, который использует Office 365.</span><span class="sxs-lookup"><span data-stu-id="8c72a-136">So you can easily use in your own apps the same authentication that Office 365 uses.</span></span>

## <a name="set-up-an-azure-ad-tenant"></a><span data-ttu-id="8c72a-137">Настройка клиента Azure AD</span><span class="sxs-lookup"><span data-stu-id="8c72a-137">Set up an Azure AD tenant</span></span>

<span data-ttu-id="8c72a-138">каталог Azure AD в Azure AD, называется [клиента](https://technet.microsoft.com/library/jj573650.aspx), и настройка клиента выполняется очень просто.</span><span class="sxs-lookup"><span data-stu-id="8c72a-138">an Azure AD directory is called an Azure AD [tenant](https://technet.microsoft.com/library/jj573650.aspx), and setting up a tenant is pretty easy.</span></span> <span data-ttu-id="8c72a-139">Мы покажем, как это можно сделать на портале управления Azure для демонстрации основных принципов, но само собой подобно другим функциям портала можно также выполняется с помощью скрипта или API-Интерфейс управления.</span><span class="sxs-lookup"><span data-stu-id="8c72a-139">We'll show you how it's done in the Azure Management Portal in order to illustrate the concepts, but of course like the other portal functions you can also do it by using a script or management API.</span></span>

<span data-ttu-id="8c72a-140">На портале управления щелкните вкладку Active Directory.</span><span class="sxs-lookup"><span data-stu-id="8c72a-140">In the management portal click the Active Directory tab.</span></span>

![WAAD портала](single-sign-on/_static/image5.png)

<span data-ttu-id="8c72a-142">Имеется один клиент Azure AD для учетной записи Azure автоматически, и можно щелкнуть **добавить** , расположенную в нижней части страницы, чтобы создать дополнительные каталоги.</span><span class="sxs-lookup"><span data-stu-id="8c72a-142">You automatically have one Azure AD tenant for your Azure account, and you can click the **Add** button at the bottom of the page to create additional directories.</span></span> <span data-ttu-id="8c72a-143">Может потребоваться одно для тестовой среды и одна для производства, например.</span><span class="sxs-lookup"><span data-stu-id="8c72a-143">You might want one for a test environment and one for production, for example.</span></span> <span data-ttu-id="8c72a-144">Тщательно продумайте имя нового каталога.</span><span class="sxs-lookup"><span data-stu-id="8c72a-144">Think carefully about what you name a new directory.</span></span> <span data-ttu-id="8c72a-145">Если вы используете имя каталога и затем использовать свое имя еще раз для одного из пользователей, которые могут показаться сложными.</span><span class="sxs-lookup"><span data-stu-id="8c72a-145">If you use your name for the directory and then you use your name again for one of the users, that can be confusing.</span></span>

![Добавить каталог](single-sign-on/_static/image6.png)

<span data-ttu-id="8c72a-147">Портал обеспечивает полную поддержку создание, удаление и управление пользователями в данной среде.</span><span class="sxs-lookup"><span data-stu-id="8c72a-147">The portal has full support for creating, deleting, and managing users within this environment.</span></span> <span data-ttu-id="8c72a-148">Например, чтобы добавить пользователя перейдите на **пользователей** и нажмите кнопку **добавить пользователя** кнопки.</span><span class="sxs-lookup"><span data-stu-id="8c72a-148">For example, to add a user go to the **Users** tab and click the **Add User** button.</span></span>

![Добавление кнопки пользователя](single-sign-on/_static/image7.png)

![Добавить пользовательское диалоговое окно](single-sign-on/_static/image8.png)

<span data-ttu-id="8c72a-151">Можно создать нового пользователя, который существует только в этом каталоге или учетной записью Майкрософт может зарегистрировать в качестве пользователя в этом каталоге или регистра или пользователя из другого каталога Azure AD как пользователь в этом каталоге.</span><span class="sxs-lookup"><span data-stu-id="8c72a-151">You can create a new user who exists only in this directory, or you can register a Microsoft Account as a user in this directory, or register or a user from another Azure AD directory as a user in this directory.</span></span> <span data-ttu-id="8c72a-152">(В реальном каталоге домена по умолчанию будет ContosoTest.onmicrosoft.com. Можно также использовать домен по собственному выбору, например contoso.com.)</span><span class="sxs-lookup"><span data-stu-id="8c72a-152">(In a real directory, the default domain would be ContosoTest.onmicrosoft.com. You can also use a domain of your own choosing, like contoso.com.)</span></span>

![Пользовательские типы](single-sign-on/_static/image9.png)

![Добавить пользовательское диалоговое окно](single-sign-on/_static/image10.png)

<span data-ttu-id="8c72a-155">Можно назначить роли пользователя.</span><span class="sxs-lookup"><span data-stu-id="8c72a-155">You can assign the user to a role.</span></span>

![Профиль пользователя](single-sign-on/_static/image11.png)

<span data-ttu-id="8c72a-157">И учетная запись создается временный пароль.</span><span class="sxs-lookup"><span data-stu-id="8c72a-157">And the account is created with a temporary password.</span></span>

![Временный пароль](single-sign-on/_static/image12.png)

<span data-ttu-id="8c72a-159">Пользователи, создании таким образом сразу же можно войти на веб-приложения с помощью этого облачного каталога.</span><span class="sxs-lookup"><span data-stu-id="8c72a-159">The users you create this way can immediately log in to your web apps using this cloud directory.</span></span>

<span data-ttu-id="8c72a-160">Отлично подходит для корпоративного единого входа, однако является **Интеграция каталогов** вкладки:</span><span class="sxs-lookup"><span data-stu-id="8c72a-160">What's great for enterprise single sign-on, though, is the **Directory Integration** tab:</span></span>

![Вкладка интеграции каталогов](single-sign-on/_static/image13.png)

<span data-ttu-id="8c72a-162">Если включена Интеграция каталогов и [Загрузите средство](https://social.technet.microsoft.com/wiki/contents/articles/19098.howto-install-the-windows-azure-active-directory-sync-tool-now-with-pictures.aspx), можно синхронизировать этот каталог облака с существующей локальной среде Active Directory, вы уже используете внутри организации.</span><span class="sxs-lookup"><span data-stu-id="8c72a-162">If you enable directory integration, and [download a tool](https://social.technet.microsoft.com/wiki/contents/articles/19098.howto-install-the-windows-azure-active-directory-sync-tool-now-with-pictures.aspx), you can sync this cloud directory with your existing on-premises Active Directory that you're already using inside your organization.</span></span> <span data-ttu-id="8c72a-163">Затем все пользователи, которые хранятся в каталоге будут отображаться в этом каталоге облака.</span><span class="sxs-lookup"><span data-stu-id="8c72a-163">Then all of the users stored in your directory will show up in this cloud directory.</span></span> <span data-ttu-id="8c72a-164">Облачных приложений теперь могут проходить проверку подлинности всех сотрудников компании с помощью существующих учетных данных Active Directory.</span><span class="sxs-lookup"><span data-stu-id="8c72a-164">Your cloud apps can now authenticate all of your employees using their existing Active Directory credentials.</span></span> <span data-ttu-id="8c72a-165">И все это бесплатно — средство синхронизации и Azure AD.</span><span class="sxs-lookup"><span data-stu-id="8c72a-165">And all this is free – both the sync tool and Azure AD itself.</span></span>

<span data-ttu-id="8c72a-166">Средство — мастер, который прост в использовании, как видно из этих снимках экрана.</span><span class="sxs-lookup"><span data-stu-id="8c72a-166">The tool is a wizard that is easy to use, as you can see from these screen shots.</span></span> <span data-ttu-id="8c72a-167">Они не полные инструкции лишь пример, показывающий, вы в основном процесс.</span><span class="sxs-lookup"><span data-stu-id="8c72a-167">These are not complete instructions, just an example showing you the basic process.</span></span> <span data-ttu-id="8c72a-168">Более подробные сведения инструкциями-в-it, см. по ссылкам в [ресурсов](#resources) в конце раздела.</span><span class="sxs-lookup"><span data-stu-id="8c72a-168">For more detailed how-to-do-it information, see the links in the [Resources](#resources) section at the end of the chapter.</span></span>

![Мастер настройки средства синхронизации WAAD](single-sign-on/_static/image14.png)

<span data-ttu-id="8c72a-170">Нажмите кнопку **Далее**, а затем введите учетные данные Azure Active Directory.</span><span class="sxs-lookup"><span data-stu-id="8c72a-170">Click **Next**, and then enter your Azure Active Directory credentials.</span></span>

![Мастер настройки средства синхронизации WAAD](single-sign-on/_static/image15.png)

<span data-ttu-id="8c72a-172">Нажмите кнопку **Далее**, а затем введите в локальной AD учетные данные.</span><span class="sxs-lookup"><span data-stu-id="8c72a-172">Click **Next**, and then enter your on-premises AD credentials.</span></span>

![Мастер настройки средства синхронизации WAAD](single-sign-on/_static/image16.png)

<span data-ttu-id="8c72a-174">Нажмите кнопку **Далее**и укажите, следует ли хранить хэш паролей AD в облаке.</span><span class="sxs-lookup"><span data-stu-id="8c72a-174">Click **Next**, and then indicate if you want to store a hash of your AD passwords in the cloud.</span></span>

![Мастер настройки средства синхронизации WAAD](single-sign-on/_static/image17.png)

<span data-ttu-id="8c72a-176">Хэш пароля, которые могут храниться в облаке имеют одностороннее хэширование; Фактические пароли никогда не хранятся в Azure AD.</span><span class="sxs-lookup"><span data-stu-id="8c72a-176">The password hash that you can store in the cloud is a one-way hash; actual passwords are never stored in Azure AD.</span></span> <span data-ttu-id="8c72a-177">Если принято решение не хранение хэш в облаке, вам придется использовать [служб федерации Active Directory](https://technet.microsoft.com/library/hh831502.aspx) (ADFS).</span><span class="sxs-lookup"><span data-stu-id="8c72a-177">If you decide against storing hashes in the cloud, you'll have to use [Active Directory Federation Services](https://technet.microsoft.com/library/hh831502.aspx) (ADFS).</span></span> <span data-ttu-id="8c72a-178">Существуют также [другие факторы, которые необходимо учитывать при выборе ли использование служб федерации Active Directory](https://technet.microsoft.com/library/jj573653.aspx).</span><span class="sxs-lookup"><span data-stu-id="8c72a-178">There are also [other factors to consider when choosing whether or not to use ADFS](https://technet.microsoft.com/library/jj573653.aspx).</span></span> <span data-ttu-id="8c72a-179">ADFS требуется несколько дополнительных действий по настройке.</span><span class="sxs-lookup"><span data-stu-id="8c72a-179">The ADFS option requires a few additional configuration steps.</span></span>

<span data-ttu-id="8c72a-180">Если вы решили хранить хэш-коды в облаке, завершения и запускает средство синхронизации каталогов, при нажатии кнопки **Далее**.</span><span class="sxs-lookup"><span data-stu-id="8c72a-180">If you choose to store hashes in the cloud, you're done, and the tool starts synchronizing directories when you click **Next**.</span></span>

![Мастер настройки средства синхронизации WAAD](single-sign-on/_static/image18.png)

<span data-ttu-id="8c72a-182">И в течение нескольких минут завершения.</span><span class="sxs-lookup"><span data-stu-id="8c72a-182">And in a few minutes you're done.</span></span>

![Мастер настройки средства синхронизации WAAD](single-sign-on/_static/image19.png)

<span data-ttu-id="8c72a-184">Необходимо выполнять на одном контроллере домена в организации, в Windows 2003 или более поздней версии.</span><span class="sxs-lookup"><span data-stu-id="8c72a-184">You only have to run this on one domain controller in the organization, on Windows 2003 or higher.</span></span> <span data-ttu-id="8c72a-185">И перезагрузки.</span><span class="sxs-lookup"><span data-stu-id="8c72a-185">And no need to reboot.</span></span> <span data-ttu-id="8c72a-186">Когда закончите, все пользователи находятся в облаке и можно сделать единого входа из любой веб- или мобильного приложения, с помощью SAML, OAuth или WS-Fed.</span><span class="sxs-lookup"><span data-stu-id="8c72a-186">When you're done, all of your users are in the cloud and you can do single sign-on from any web or mobile application, using SAML, OAuth, or WS-Fed.</span></span>

<span data-ttu-id="8c72a-187">Иногда мы получить ответы о том, как безопасный это — корпорация Майкрософт использует для своих собственных конфиденциальных деловых данных?</span><span class="sxs-lookup"><span data-stu-id="8c72a-187">Sometimes we get asked about how secure this is – does Microsoft use it for their own sensitive business data?</span></span> <span data-ttu-id="8c72a-188">И Да, мы делаем.</span><span class="sxs-lookup"><span data-stu-id="8c72a-188">And the answer is yes we do.</span></span> <span data-ttu-id="8c72a-189">Например, при переходе на внутренний узел Microsoft SharePoint на [https://microsoft.sharepoint.com/](https://microsoft.sharepoint.com/), получить предложено выполнить вход.</span><span class="sxs-lookup"><span data-stu-id="8c72a-189">For example, if you go to the internal Microsoft SharePoint site at [https://microsoft.sharepoint.com/](https://microsoft.sharepoint.com/), you get prompted to log in.</span></span>

![Вход в Office 365](single-sign-on/_static/image20.png)

<span data-ttu-id="8c72a-191">Microsoft включения служб федерации Active Directory, поэтому при вводе идентификатора Microsoft, происходит перенаправление на страницу входа служб федерации Active Directory.</span><span class="sxs-lookup"><span data-stu-id="8c72a-191">Microsoft has enabled ADFS, so when you enter a Microsoft ID, you get redirected to an ADFS log-in page.</span></span>

![Вход служб федерации Active Directory](single-sign-on/_static/image21.png)

<span data-ttu-id="8c72a-193">И после ввода учетных данных, хранящихся в учетной записи внутренней Microsoft AD имеется доступ к этому приложению внутренней.</span><span class="sxs-lookup"><span data-stu-id="8c72a-193">And once you enter credentials stored in an internal Microsoft AD account, you have access to this internal application.</span></span>

![Сайт MS SharePoint](single-sign-on/_static/image22.png)

<span data-ttu-id="8c72a-195">Мы используем сервер входа в AD, главным образом, так как мы уже служб федерации Active Directory, прежде чем Azure AD стали доступны, но процесс входа в проходит через каталог Azure AD в облаке.</span><span class="sxs-lookup"><span data-stu-id="8c72a-195">We're using an AD sign-in server mainly because we already had ADFS set up before Azure AD became available, but the log-in process is going through an Azure AD directory in the cloud.</span></span> <span data-ttu-id="8c72a-196">Мы поместить наши важные документы, системы управления версиями, файлы управления производительностью, отчеты о продажах и многое другое, в облаке и защитите их с помощью этого точно того же решения.</span><span class="sxs-lookup"><span data-stu-id="8c72a-196">We put our important documents, source control, performance management files, sales reports, and more, in the cloud and are using this exact same solution to secure them.</span></span>

## <a name="create-an-aspnet-app-that-uses-azure-ad-for-single-sign-on"></a><span data-ttu-id="8c72a-197">Создать приложение ASP.NET, которое использует Azure AD для единого входа</span><span class="sxs-lookup"><span data-stu-id="8c72a-197">Create an ASP.NET app that uses Azure AD for single sign-on</span></span>

<span data-ttu-id="8c72a-198">Visual Studio значительно упрощает создание приложения, использующего Azure AD для единого входа, как видно из несколько снимков экрана.</span><span class="sxs-lookup"><span data-stu-id="8c72a-198">Visual Studio makes it really easy to create an app that uses Azure AD for single sign-on, as you can see from a few screen shots.</span></span>

<span data-ttu-id="8c72a-199">При создании нового приложения ASP.NET MVC или веб-формы, метод проверки подлинности по умолчанию — ASP.NET Identity.</span><span class="sxs-lookup"><span data-stu-id="8c72a-199">When you create a new ASP.NET application, either MVC or Web Forms, the default authentication method is ASP.NET Identity.</span></span> <span data-ttu-id="8c72a-200">Чтобы изменить его в Azure AD, щелкните **изменение проверки подлинности** кнопки.</span><span class="sxs-lookup"><span data-stu-id="8c72a-200">To change that to Azure AD, you click a **Change Authentication** button.</span></span>

![Изменить проверку подлинности](single-sign-on/_static/image23.png)

<span data-ttu-id="8c72a-202">Выберите учетные записи организации, введите имя домена и выберите единого входа.</span><span class="sxs-lookup"><span data-stu-id="8c72a-202">Select Organizational Accounts, enter your domain name, and then select Single Sign On.</span></span>

![Настроить диалоговое окно проверки подлинности](single-sign-on/_static/image24.png)

<span data-ttu-id="8c72a-204">Можно также приложение чтение или чтение и запись разрешения для каталога данных.</span><span class="sxs-lookup"><span data-stu-id="8c72a-204">You can also give the app read or read/write permission for directory data.</span></span> <span data-ttu-id="8c72a-205">Если сделать это, можно использовать [Azure Graph REST API](https://msdn.microsoft.com/library/windowsazure/hh974476.aspx) для поиска пользователей номер телефона, узнайте ли они в офисе, когда они последнего вошел в систему и т. д.</span><span class="sxs-lookup"><span data-stu-id="8c72a-205">If you do that, it can use the [Azure Graph REST API](https://msdn.microsoft.com/library/windowsazure/hh974476.aspx) to look up users' phone number, find out if they're in the office, when they last logged on, etc.</span></span>

<span data-ttu-id="8c72a-206">Это все, что нужно сделать — Visual Studio запрашивает учетные данные администратора для вашего клиента Azure AD, и затем настраивает проект и клиент Azure AD для нового приложения.</span><span class="sxs-lookup"><span data-stu-id="8c72a-206">That's all you have to do - Visual Studio asks for the credentials for an administrator for your Azure AD tenant, and then it configures both your project and your Azure AD tenant for the new application.</span></span>

<span data-ttu-id="8c72a-207">При запуске проекта, вы увидите страницу входа, и можно выполнить вход с учетными данными пользователя вашего каталога Azure AD.</span><span class="sxs-lookup"><span data-stu-id="8c72a-207">When you run the project, you'll see a sign-in page, and you can sign in with credentials of a user in your Azure AD directory.</span></span>

![Вход учетной записи организации.](single-sign-on/_static/image25.png)

![Вход](single-sign-on/_static/image26.png)

<span data-ttu-id="8c72a-210">При развертывании приложения в Azure, необходимо сделать всего выберите **включить проверку подлинности по организации** флажок и снова Visual Studio обеспечивает все конфигурации для вас.</span><span class="sxs-lookup"><span data-stu-id="8c72a-210">When you deploy the app to Azure, all you have to do is select an **Enable Organizational Authentication** check box, and once again Visual Studio takes care of all the configuration for you.</span></span>

![Веб-публикация](single-sign-on/_static/image27.png)

<span data-ttu-id="8c72a-212">Эти снимки экрана берутся из полный Пошаговое руководство, в котором показано, как создать приложение, использующее проверку подлинности Azure AD: [разработка приложений ASP.NET с помощью Azure Active Directory](../../../../identity/overview/getting-started/developing-aspnet-apps-with-windows-azure-active-directory.md).</span><span class="sxs-lookup"><span data-stu-id="8c72a-212">These screen shots come from a complete step-by-step tutorial that shows how to build an app that uses Azure AD authentication: [Developing ASP.NET Apps with Azure Active Directory](../../../../identity/overview/getting-started/developing-aspnet-apps-with-windows-azure-active-directory.md).</span></span>

## <a name="summary"></a><span data-ttu-id="8c72a-213">Сводка</span><span class="sxs-lookup"><span data-stu-id="8c72a-213">Summary</span></span>

<span data-ttu-id="8c72a-214">В этой главе показано, что Azure Active Directory, Visual Studio и ASP.NET, позволяют легко настроить единый вход в веб-приложений для пользователей вашей организации.</span><span class="sxs-lookup"><span data-stu-id="8c72a-214">In this chapter you saw that Azure Active Directory, Visual Studio, and ASP.NET, make it easy to set up single sign-on in Internet applications for your organization's users.</span></span> <span data-ttu-id="8c72a-215">Пользователи могут войти в Интернет-приложениях, использующих те же учетные данные, которые они используют для входа с помощью Active Directory во внутренней сети.</span><span class="sxs-lookup"><span data-stu-id="8c72a-215">Your users can sign on in Internet apps using the same credentials they use to sign on using Active Directory in your internal network.</span></span>

<span data-ttu-id="8c72a-216">[Следующей главе](data-storage-options.md) просматривает параметры хранилища данных, доступных для облачного приложения.</span><span class="sxs-lookup"><span data-stu-id="8c72a-216">The [next chapter](data-storage-options.md) looks at the data storage options available for a cloud app.</span></span>

<a id="resources"></a>
## <a name="resources"></a><span data-ttu-id="8c72a-217">Ресурсы</span><span class="sxs-lookup"><span data-stu-id="8c72a-217">Resources</span></span>

<span data-ttu-id="8c72a-218">Дополнительные сведения см. в следующих ресурсах:</span><span class="sxs-lookup"><span data-stu-id="8c72a-218">For more information, see the following resources:</span></span>

- <span data-ttu-id="8c72a-219">[Документация по Azure Active Directory](https://docs.microsoft.com/azure/active-directory/).</span><span class="sxs-lookup"><span data-stu-id="8c72a-219">[Azure Active Directory Documentation](https://docs.microsoft.com/azure/active-directory/).</span></span> <span data-ttu-id="8c72a-220">Страница портала Azure AD документацию на сайте windowsazure.com.</span><span class="sxs-lookup"><span data-stu-id="8c72a-220">Portal page for Azure AD documentation on the windowsazure.com site.</span></span> <span data-ttu-id="8c72a-221">Пошаговые руководства, см. **разработка** раздела.</span><span class="sxs-lookup"><span data-stu-id="8c72a-221">For step by step tutorials, see the **Develop** section.</span></span>
- <span data-ttu-id="8c72a-222">[Azure Multi-factor Authentication](https://docs.microsoft.com/azure/multi-factor-authentication/).</span><span class="sxs-lookup"><span data-stu-id="8c72a-222">[Azure Multi-Factor Authentication](https://docs.microsoft.com/azure/multi-factor-authentication/).</span></span> <span data-ttu-id="8c72a-223">Страница портала для получения сведений о многофакторной проверки подлинности в Azure.</span><span class="sxs-lookup"><span data-stu-id="8c72a-223">Portal page for documentation about multi-factor authentication in Azure.</span></span>
- <span data-ttu-id="8c72a-224">[Параметры проверки подлинности учетной записи организации](../../../../visual-studio/overview/2013/creating-web-projects-in-visual-studio.md#orgauthoptions).</span><span class="sxs-lookup"><span data-stu-id="8c72a-224">[Organizational account authentication options](../../../../visual-studio/overview/2013/creating-web-projects-in-visual-studio.md#orgauthoptions).</span></span> <span data-ttu-id="8c72a-225">Объяснение параметров проверки подлинности Azure AD в диалоговом окне нового проекта Visual Studio 2013.</span><span class="sxs-lookup"><span data-stu-id="8c72a-225">Explanation of the Azure AD authentication options in the Visual Studio 2013 new-project dialog.</span></span>
- <span data-ttu-id="8c72a-226">[Рекомендации — федеративные удостоверения шаблон](https://msdn.microsoft.com/library/dn589790.aspx).</span><span class="sxs-lookup"><span data-stu-id="8c72a-226">[Microsoft Patterns and Practices - Federated Identity Pattern](https://msdn.microsoft.com/library/dn589790.aspx).</span></span>
- <span data-ttu-id="8c72a-227">[Практическое руководство: Установка средства синхронизации Azure Active Directory](https://social.technet.microsoft.com/wiki/contents/articles/19098.howto-install-the-windows-azure-active-directory-sync-tool-now-with-pictures.aspx).</span><span class="sxs-lookup"><span data-stu-id="8c72a-227">[HowTo: Install the Azure Active Directory Sync Tool](https://social.technet.microsoft.com/wiki/contents/articles/19098.howto-install-the-windows-azure-active-directory-sync-tool-now-with-pictures.aspx).</span></span>
- <span data-ttu-id="8c72a-228">[Службы федерации Active Directory 2.0 Карта содержимого](https://social.technet.microsoft.com/wiki/contents/articles/2735.ad-fs-2-0-content-map.aspx).</span><span class="sxs-lookup"><span data-stu-id="8c72a-228">[Active Directory Federation Services 2.0 Content Map](https://social.technet.microsoft.com/wiki/contents/articles/2735.ad-fs-2-0-content-map.aspx).</span></span> <span data-ttu-id="8c72a-229">Ссылки на документацию о ADFS 2.0.</span><span class="sxs-lookup"><span data-stu-id="8c72a-229">Links to documentation about ADFS 2.0.</span></span>
- <span data-ttu-id="8c72a-230">[Авторизация на основе ролей и на основе ACL в приложении Windows Azure AD](https://code.msdn.microsoft.com/Role-Based-and-ACL-Based-86ad71a1).</span><span class="sxs-lookup"><span data-stu-id="8c72a-230">[Role-Based and ACL-Based Authorization in a Windows Azure AD Application](https://code.msdn.microsoft.com/Role-Based-and-ACL-Based-86ad71a1).</span></span> <span data-ttu-id="8c72a-231">Образец приложения.</span><span class="sxs-lookup"><span data-stu-id="8c72a-231">Sample application.</span></span>
- <span data-ttu-id="8c72a-232">[Блог Azure AD Graph API](https://blogs.msdn.com/b/aadgraphteam/).</span><span class="sxs-lookup"><span data-stu-id="8c72a-232">[Azure Active Directory Graph API blog](https://blogs.msdn.com/b/aadgraphteam/).</span></span>
- <span data-ttu-id="8c72a-233">[Управление доступом в BYOD и Интеграция каталогов в гибридной инфраструктуре идентификации](https://channel9.msdn.com/Events/TechEd/NorthAmerica/2014/PCIT-B213#fbid=).</span><span class="sxs-lookup"><span data-stu-id="8c72a-233">[Access Control in BYOD and Directory Integration in a Hybrid Identity Infrastructure](https://channel9.msdn.com/Events/TechEd/NorthAmerica/2014/PCIT-B213#fbid=).</span></span> <span data-ttu-id="8c72a-234">Технический Ed 2014 сеанс видео по Bagdasaryan Гаяне.</span><span class="sxs-lookup"><span data-stu-id="8c72a-234">Tech Ed 2014 session video by Gayana Bagdasaryan.</span></span>

>[!div class="step-by-step"]
<span data-ttu-id="8c72a-235">[Назад](web-development-best-practices.md)
[Вперед](data-storage-options.md)</span><span class="sxs-lookup"><span data-stu-id="8c72a-235">[Previous](web-development-best-practices.md)
[Next](data-storage-options.md)</span></span>
