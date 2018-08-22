---
uid: web-api/overview/security/working-with-ssl-in-web-api
title: Работа с SSL в веб-API | Документация Майкрософт
author: MikeWasson
description: Показано, как использовать протокол SSL с веб-API ASP.NET, в том числе с использованием SSL-сертификатов клиента.
ms.author: riande
ms.date: 12/12/2012
ms.assetid: 97f6164f-59cf-45c0-b820-e4aa29b45396
msc.legacyurl: /web-api/overview/security/working-with-ssl-in-web-api
msc.type: authoredcontent
ms.openlocfilehash: b11b35f58a1f033423f5e6ea5f5373df0d1fcb5f
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 08/16/2018
ms.locfileid: "41828144"
---
<a name="working-with-ssl-in-web-api"></a><span data-ttu-id="ccf5b-103">Работа с SSL в веб-API</span><span class="sxs-lookup"><span data-stu-id="ccf5b-103">Working with SSL in Web API</span></span>
====================
<span data-ttu-id="ccf5b-104">по [Майк Уоссон](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="ccf5b-104">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

<span data-ttu-id="ccf5b-105">Несколько общих схем проверки подлинности не являются безопасными через простой HTTP.</span><span class="sxs-lookup"><span data-stu-id="ccf5b-105">Several common authentication schemes are not secure over plain HTTP.</span></span> <span data-ttu-id="ccf5b-106">В частности Обычная проверка подлинности и проверки подлинности форм отправляют незашифрованные учетные данные.</span><span class="sxs-lookup"><span data-stu-id="ccf5b-106">In particular, Basic authentication and forms authentication send unencrypted credentials.</span></span> <span data-ttu-id="ccf5b-107">Для обеспечения безопасности, эти схемы проверки подлинности *необходимо* использование протокола SSL.</span><span class="sxs-lookup"><span data-stu-id="ccf5b-107">To be secure, these authentication schemes *must* use SSL.</span></span> <span data-ttu-id="ccf5b-108">Кроме того можно использовать SSL-сертификатов клиента для проверки подлинности клиентов.</span><span class="sxs-lookup"><span data-stu-id="ccf5b-108">In addition, SSL client certificates can be used to authenticate clients.</span></span>

## <a name="enabling-ssl-on-the-server"></a><span data-ttu-id="ccf5b-109">Включение SSL на сервере</span><span class="sxs-lookup"><span data-stu-id="ccf5b-109">Enabling SSL on the Server</span></span>

<span data-ttu-id="ccf5b-110">Настройка SSL в IIS 7 или более поздней версии:</span><span class="sxs-lookup"><span data-stu-id="ccf5b-110">To set up SSL in IIS 7 or later:</span></span>

- <span data-ttu-id="ccf5b-111">Создать или получить сертификат.</span><span class="sxs-lookup"><span data-stu-id="ccf5b-111">Create or get a certificate.</span></span> <span data-ttu-id="ccf5b-112">Для тестирования, можно создать самозаверяющий сертификат.</span><span class="sxs-lookup"><span data-stu-id="ccf5b-112">For testing, you can create a self-signed certificate.</span></span>
- <span data-ttu-id="ccf5b-113">Добавьте привязку HTTPS.</span><span class="sxs-lookup"><span data-stu-id="ccf5b-113">Add an HTTPS binding.</span></span>

<span data-ttu-id="ccf5b-114">Дополнительные сведения см. в разделе [как выполнить настройку SSL в службах IIS 7](https://www.iis.net/learn/manage/configuring-security/how-to-set-up-ssl-on-iis).</span><span class="sxs-lookup"><span data-stu-id="ccf5b-114">For details, see [How to Set Up SSL on IIS 7](https://www.iis.net/learn/manage/configuring-security/how-to-set-up-ssl-on-iis).</span></span>

<span data-ttu-id="ccf5b-115">Для локального тестирования, вы можете включить SSL в IIS Express из Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="ccf5b-115">For local testing, you can enable SSL in IIS Express from Visual Studio.</span></span> <span data-ttu-id="ccf5b-116">В окне «Свойства» задайте **протокол SSL включен** для **True**.</span><span class="sxs-lookup"><span data-stu-id="ccf5b-116">In the Properties window, set **SSL Enabled** to **True**.</span></span> <span data-ttu-id="ccf5b-117">Обратите внимание на значение **URL-адрес SSL**; используйте этот URL-адрес для тестирования подключения по протоколу HTTPS.</span><span class="sxs-lookup"><span data-stu-id="ccf5b-117">Note the value of **SSL URL**; use this URL for testing HTTPS connections.</span></span>

![](working-with-ssl-in-web-api/_static/image1.png)

### <a name="enforcing-ssl-in-a-web-api-controller"></a><span data-ttu-id="ccf5b-118">Принудительное применение протокола SSL в контроллер веб-API</span><span class="sxs-lookup"><span data-stu-id="ccf5b-118">Enforcing SSL in a Web API Controller</span></span>

<span data-ttu-id="ccf5b-119">Если у вас есть привязку HTTP и HTTPS, клиенты могут использовать HTTP для доступа к сайту.</span><span class="sxs-lookup"><span data-stu-id="ccf5b-119">If you have both an HTTPS and an HTTP binding, clients can still use HTTP to access the site.</span></span> <span data-ttu-id="ccf5b-120">Можно предоставить некоторые ресурсы, которые доступны через HTTP, а для других ресурсов требуется SSL.</span><span class="sxs-lookup"><span data-stu-id="ccf5b-120">You might allow some resources to be available through HTTP, while other resources require SSL.</span></span> <span data-ttu-id="ccf5b-121">В этом случае используйте фильтр действий для требования SSL для защищенных ресурсов.</span><span class="sxs-lookup"><span data-stu-id="ccf5b-121">In that case, use an action filter to require SSL for the protected resources.</span></span> <span data-ttu-id="ccf5b-122">В следующем коде показано фильтр проверки подлинности веб-API, которое проверяет наличие SSL:</span><span class="sxs-lookup"><span data-stu-id="ccf5b-122">The following code shows a Web API authentication filter that checks for SSL:</span></span>

[!code-csharp[Main](working-with-ssl-in-web-api/samples/sample1.cs)]

<span data-ttu-id="ccf5b-123">Добавьте этот фильтр ко всем действиям веб-API, для которых требуется SSL:</span><span class="sxs-lookup"><span data-stu-id="ccf5b-123">Add this filter to any Web API actions that require SSL:</span></span>

[!code-csharp[Main](working-with-ssl-in-web-api/samples/sample2.cs)]

## <a name="ssl-client-certificates"></a><span data-ttu-id="ccf5b-124">SSL-сертификатов клиента</span><span class="sxs-lookup"><span data-stu-id="ccf5b-124">SSL Client Certificates</span></span>

<span data-ttu-id="ccf5b-125">SSL обеспечивает проверку подлинности с помощью сертификатов инфраструктуры открытых ключей.</span><span class="sxs-lookup"><span data-stu-id="ccf5b-125">SSL provides authentication by using Public Key Infrastructure certificates.</span></span> <span data-ttu-id="ccf5b-126">Сервер должен предоставить сертификат, который проверяет подлинность сервера на клиент.</span><span class="sxs-lookup"><span data-stu-id="ccf5b-126">The server must provide a certificate that authenticates the server to the client.</span></span> <span data-ttu-id="ccf5b-127">Это менее типичный для клиента, чтобы предоставить сертификат на сервер, но это один из вариантов проверки подлинности клиентов.</span><span class="sxs-lookup"><span data-stu-id="ccf5b-127">It is less common for the client to provide a certificate to the server, but this is one option for authenticating clients.</span></span> <span data-ttu-id="ccf5b-128">Чтобы использовать сертификаты клиента с помощью протокола SSL, необходимо способа распространения сертификаты для пользователей.</span><span class="sxs-lookup"><span data-stu-id="ccf5b-128">To use client certificates with SSL, you need a way to distribute signed certificates to your users.</span></span> <span data-ttu-id="ccf5b-129">Для многих типов приложений это не будет эффективное взаимодействие с пользователем, но в некоторых средах (например, enterprise) может быть нецелесообразно.</span><span class="sxs-lookup"><span data-stu-id="ccf5b-129">For many application types, this will not be a good user experience, but in some environments (for example, enterprise) it may be feasible.</span></span>

| <span data-ttu-id="ccf5b-130">Преимущества</span><span class="sxs-lookup"><span data-stu-id="ccf5b-130">Advantages</span></span> | <span data-ttu-id="ccf5b-131">Недостатки</span><span class="sxs-lookup"><span data-stu-id="ccf5b-131">Disadvantages</span></span> |
| --- | --- |
| <span data-ttu-id="ccf5b-132">— Учетные данные сертификат, надежнее, чем имя пользователя и пароль.</span><span class="sxs-lookup"><span data-stu-id="ccf5b-132">- Certificate credentials are stronger than username/password.</span></span> <span data-ttu-id="ccf5b-133">-SSL предоставляет полный безопасный канал с проверкой подлинности, проверку целостности сообщений и шифрования сообщений.</span><span class="sxs-lookup"><span data-stu-id="ccf5b-133">- SSL provides a complete secure channel, with authentication, message integrity, and message encryption.</span></span> | <span data-ttu-id="ccf5b-134">-Необходимо получить и управление сертификатами PKI.</span><span class="sxs-lookup"><span data-stu-id="ccf5b-134">- You must obtain and manage PKI certificates.</span></span> <span data-ttu-id="ccf5b-135">-Клиентской платформы должен поддерживать SSL-сертификатов клиента.</span><span class="sxs-lookup"><span data-stu-id="ccf5b-135">- The client platform must support SSL client certificates.</span></span> |

<span data-ttu-id="ccf5b-136">Чтобы настроить службы IIS, чтобы принимать сертификаты клиентов, откройте диспетчер IIS и выполните следующие действия:</span><span class="sxs-lookup"><span data-stu-id="ccf5b-136">To configure IIS to accept client certificates, open IIS Manager and perform the following steps:</span></span>

1. <span data-ttu-id="ccf5b-137">Щелкните узел в представлении в виде дерева.</span><span class="sxs-lookup"><span data-stu-id="ccf5b-137">Click the site node in the tree view.</span></span>
2. <span data-ttu-id="ccf5b-138">Дважды щелкните **параметры SSL** функцию в средней области.</span><span class="sxs-lookup"><span data-stu-id="ccf5b-138">Double-click the **SSL Settings** feature in the middle pane.</span></span>
3. <span data-ttu-id="ccf5b-139">В разделе **сертификаты клиента**, выберите один из следующих вариантов:</span><span class="sxs-lookup"><span data-stu-id="ccf5b-139">Under **Client Certificates**, select one of these options:</span></span> 

    - <span data-ttu-id="ccf5b-140">**Принять**: IIS будет принимать сертификат клиента, но не требуются.</span><span class="sxs-lookup"><span data-stu-id="ccf5b-140">**Accept**: IIS will accept a certificate from the client, but does not require one.</span></span>
    - <span data-ttu-id="ccf5b-141">**Требовать**: требуется сертификат клиента.</span><span class="sxs-lookup"><span data-stu-id="ccf5b-141">**Require**: Require a client certificate.</span></span> <span data-ttu-id="ccf5b-142">(Чтобы включить этот параметр, необходимо также выбрать «Требовать SSL»)</span><span class="sxs-lookup"><span data-stu-id="ccf5b-142">(To enable this option, you must also select "Require SSL")</span></span>

<span data-ttu-id="ccf5b-143">Можно также задать эти параметры в файле ApplicationHost.config:</span><span class="sxs-lookup"><span data-stu-id="ccf5b-143">You can also set these options in the ApplicationHost.config file:</span></span>

[!code-xml[Main](working-with-ssl-in-web-api/samples/sample3.xml)]

<span data-ttu-id="ccf5b-144">**SslNegotiateCert** флаг означает, что IIS будет принимать сертификат клиента, но не требуются (эквивалентно значению параметра «Принять» в диспетчере служб IIS).</span><span class="sxs-lookup"><span data-stu-id="ccf5b-144">The **SslNegotiateCert** flag means IIS will accept a certificate from the client, but does not require one (equivalent to the "Accept" option in IIS Manager).</span></span> <span data-ttu-id="ccf5b-145">Чтобы требуется сертификат, установите **SslRequireCert** флаг.</span><span class="sxs-lookup"><span data-stu-id="ccf5b-145">To require a certificate, set the **SslRequireCert** flag.</span></span> <span data-ttu-id="ccf5b-146">Для тестирования, можно также задать эти параметры в IIS Express, в локальный узел applicationhost. Файл конфигурации, расположенный в «Documents\IISExpress\config».</span><span class="sxs-lookup"><span data-stu-id="ccf5b-146">For testing, you can also set these options in IIS Express, in the local applicationhost.Config file, located in "Documents\IISExpress\config".</span></span>

### <a name="creating-a-client-certificate-for-testing"></a><span data-ttu-id="ccf5b-147">Создание сертификата клиента для тестирования</span><span class="sxs-lookup"><span data-stu-id="ccf5b-147">Creating a Client Certificate for Testing</span></span>

<span data-ttu-id="ccf5b-148">В целях тестирования можно использовать [MakeCert.exe](https://msdn.microsoft.com/library/bfsktky3.aspx) для создания сертификата клиента.</span><span class="sxs-lookup"><span data-stu-id="ccf5b-148">For testing purposes, you can use [MakeCert.exe](https://msdn.microsoft.com/library/bfsktky3.aspx) to create a client certificate.</span></span> <span data-ttu-id="ccf5b-149">Во-первых создайте корневого центра тестирования:</span><span class="sxs-lookup"><span data-stu-id="ccf5b-149">First, create a test root authority:</span></span>

[!code-console[Main](working-with-ssl-in-web-api/samples/sample4.cmd)]

<span data-ttu-id="ccf5b-150">Makecert предложит ввести пароль для закрытого ключа.</span><span class="sxs-lookup"><span data-stu-id="ccf5b-150">Makecert will prompt you to enter a password for the private key.</span></span>

<span data-ttu-id="ccf5b-151">Добавьте сертификат в тест хранилища сервера «доверенные корневые центры сертификации», следующим образом:</span><span class="sxs-lookup"><span data-stu-id="ccf5b-151">Next, add the certificate to the test server's "Trusted Root Certification Authorities" store, as follows:</span></span>

1. <span data-ttu-id="ccf5b-152">Откройте консоль MMC.</span><span class="sxs-lookup"><span data-stu-id="ccf5b-152">Open MMC.</span></span>
2. <span data-ttu-id="ccf5b-153">В разделе **файл**выберите **Add/Remove Snap-In**.</span><span class="sxs-lookup"><span data-stu-id="ccf5b-153">Under **File**, select **Add/Remove Snap-In**.</span></span>
3. <span data-ttu-id="ccf5b-154">Выберите **учетная запись компьютера**.</span><span class="sxs-lookup"><span data-stu-id="ccf5b-154">Select **Computer Account**.</span></span>
4. <span data-ttu-id="ccf5b-155">Выберите **локального компьютера** и завершите работу мастера.</span><span class="sxs-lookup"><span data-stu-id="ccf5b-155">Select **Local computer** and complete the wizard.</span></span>
5. <span data-ttu-id="ccf5b-156">В области навигации разверните узел «Доверенные корневые центры сертификации».</span><span class="sxs-lookup"><span data-stu-id="ccf5b-156">Under the navigation pane, expand the "Trusted Root Certification Authorities" node.</span></span>
6. <span data-ttu-id="ccf5b-157">На **действие** последовательно выберите пункты **все задачи**, а затем нажмите кнопку **импорта** для запуска мастера импорта сертификатов.</span><span class="sxs-lookup"><span data-stu-id="ccf5b-157">On the **Action** menu, point to **All Tasks**, and then click **Import** to start the Certificate Import Wizard.</span></span>
7. <span data-ttu-id="ccf5b-158">Перейдите к файлу сертификата, TempCA.cer.</span><span class="sxs-lookup"><span data-stu-id="ccf5b-158">Browse to the certificate file, TempCA.cer.</span></span>
8. <span data-ttu-id="ccf5b-159">Нажмите кнопку **откройте**, затем нажмите кнопку **Далее** и завершите работу мастера.</span><span class="sxs-lookup"><span data-stu-id="ccf5b-159">Click **Open**, then click **Next** and complete the wizard.</span></span> <span data-ttu-id="ccf5b-160">(Вам будет предложено повторно ввести пароль.)</span><span class="sxs-lookup"><span data-stu-id="ccf5b-160">(You will be prompted to re-enter the password.)</span></span>

<span data-ttu-id="ccf5b-161">Теперь создайте сертификат клиента, который подписывается первый сертификат:</span><span class="sxs-lookup"><span data-stu-id="ccf5b-161">Now create a client certificate that is signed by the first certificate:</span></span>

[!code-console[Main](working-with-ssl-in-web-api/samples/sample5.cmd)]

### <a name="using-client-certificates-in-web-api"></a><span data-ttu-id="ccf5b-162">С помощью сертификатов клиента в веб-API</span><span class="sxs-lookup"><span data-stu-id="ccf5b-162">Using Client Certificates in Web API</span></span>

<span data-ttu-id="ccf5b-163">На стороне сервера, можно получить сертификат клиента, вызвав [GetClientCertificate](https://msdn.microsoft.com/library/system.net.http.httprequestmessageextensions.getclientcertificate.aspx) в сообщении запроса.</span><span class="sxs-lookup"><span data-stu-id="ccf5b-163">On the server side, you can get the client certificate by calling [GetClientCertificate](https://msdn.microsoft.com/library/system.net.http.httprequestmessageextensions.getclientcertificate.aspx) on the request message.</span></span> <span data-ttu-id="ccf5b-164">Метод возвращает значение null, если отсутствует сертификат клиента.</span><span class="sxs-lookup"><span data-stu-id="ccf5b-164">The method returns null if there is no client certificate.</span></span> <span data-ttu-id="ccf5b-165">В противном случае возвращается **X509Certificate2** экземпляра.</span><span class="sxs-lookup"><span data-stu-id="ccf5b-165">Otherwise, it returns an **X509Certificate2** instance.</span></span> <span data-ttu-id="ccf5b-166">Этот объект можно используйте для получения сведений из сертификата, например издателя и субъекта.</span><span class="sxs-lookup"><span data-stu-id="ccf5b-166">Use this object to get information from the certificate, such as the issuer and subject.</span></span> <span data-ttu-id="ccf5b-167">Затем эти сведения можно использовать для проверки подлинности и/или авторизации.</span><span class="sxs-lookup"><span data-stu-id="ccf5b-167">Then you can use this information for authentication and/or authorization.</span></span>

[!code-csharp[Main](working-with-ssl-in-web-api/samples/sample6.cs)]
