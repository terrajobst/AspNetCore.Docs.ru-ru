---
uid: web-api/overview/security/working-with-ssl-in-web-api
title: Работа с SSL в веб-API | Документы Microsoft
author: MikeWasson
description: Показано, как использовать протокол SSL с веб-API ASP.NET, в том числе с использованием SSL-сертификатов клиента.
ms.author: aspnetcontent
manager: wpickett
ms.date: 12/12/2012
ms.topic: article
ms.assetid: 97f6164f-59cf-45c0-b820-e4aa29b45396
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/security/working-with-ssl-in-web-api
msc.type: authoredcontent
ms.openlocfilehash: 127b336cb628e55bd59481ecb1c4df83960dc25b
ms.sourcegitcommit: 6784510cfb589308c3875ccb5113eb31031766b4
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 06/08/2018
ms.locfileid: "28036168"
---
<a name="working-with-ssl-in-web-api"></a><span data-ttu-id="a9806-103">Работа с SSL в веб-API</span><span class="sxs-lookup"><span data-stu-id="a9806-103">Working with SSL in Web API</span></span>
====================
<span data-ttu-id="a9806-104">по [Mike Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="a9806-104">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

<span data-ttu-id="a9806-105">Некоторые распространенные схемы проверки подлинности не являются безопасными через простой HTTP.</span><span class="sxs-lookup"><span data-stu-id="a9806-105">Several common authentication schemes are not secure over plain HTTP.</span></span> <span data-ttu-id="a9806-106">В частности Обычная проверка подлинности и проверки подлинности форм отправлять незашифрованные учетные данные.</span><span class="sxs-lookup"><span data-stu-id="a9806-106">In particular, Basic authentication and forms authentication send unencrypted credentials.</span></span> <span data-ttu-id="a9806-107">Для обеспечения безопасности, эти схемы проверки подлинности *должен* использовать протокол SSL.</span><span class="sxs-lookup"><span data-stu-id="a9806-107">To be secure, these authentication schemes *must* use SSL.</span></span> <span data-ttu-id="a9806-108">Кроме того SSL-сертификатов клиента может использоваться для проверки подлинности клиентов.</span><span class="sxs-lookup"><span data-stu-id="a9806-108">In addition, SSL client certificates can be used to authenticate clients.</span></span>

## <a name="enabling-ssl-on-the-server"></a><span data-ttu-id="a9806-109">При включении SSL на сервере</span><span class="sxs-lookup"><span data-stu-id="a9806-109">Enabling SSL on the Server</span></span>

<span data-ttu-id="a9806-110">Настройка SSL в IIS 7 или более поздней версии:</span><span class="sxs-lookup"><span data-stu-id="a9806-110">To set up SSL in IIS 7 or later:</span></span>

- <span data-ttu-id="a9806-111">Создать или получить сертификат.</span><span class="sxs-lookup"><span data-stu-id="a9806-111">Create or get a certificate.</span></span> <span data-ttu-id="a9806-112">Для тестирования, можно создать самозаверяющий сертификат.</span><span class="sxs-lookup"><span data-stu-id="a9806-112">For testing, you can create a self-signed certificate.</span></span>
- <span data-ttu-id="a9806-113">Добавьте HTTPS-привязки.</span><span class="sxs-lookup"><span data-stu-id="a9806-113">Add an HTTPS binding.</span></span>

<span data-ttu-id="a9806-114">Дополнительные сведения см. в разделе [как выполнить настройку SSL в службах IIS 7](https://www.iis.net/learn/manage/configuring-security/how-to-set-up-ssl-on-iis).</span><span class="sxs-lookup"><span data-stu-id="a9806-114">For details, see [How to Set Up SSL on IIS 7](https://www.iis.net/learn/manage/configuring-security/how-to-set-up-ssl-on-iis).</span></span>

<span data-ttu-id="a9806-115">Для локального тестирования можно включить SSL в IIS Express из Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="a9806-115">For local testing, you can enable SSL in IIS Express from Visual Studio.</span></span> <span data-ttu-id="a9806-116">В окне свойств задайте **протокол SSL включен** для **True**.</span><span class="sxs-lookup"><span data-stu-id="a9806-116">In the Properties window, set **SSL Enabled** to **True**.</span></span> <span data-ttu-id="a9806-117">Обратите внимание на значение **URL-адрес SSL**; использовать этот URL-адрес для тестирования подключения по протоколу HTTPS.</span><span class="sxs-lookup"><span data-stu-id="a9806-117">Note the value of **SSL URL**; use this URL for testing HTTPS connections.</span></span>

![](working-with-ssl-in-web-api/_static/image1.png)

### <a name="enforcing-ssl-in-a-web-api-controller"></a><span data-ttu-id="a9806-118">Применение протокола SSL в контроллер Web API</span><span class="sxs-lookup"><span data-stu-id="a9806-118">Enforcing SSL in a Web API Controller</span></span>

<span data-ttu-id="a9806-119">Если у вас есть привязку HTTP и HTTPS, клиенты могут использовать HTTP для доступа к сайту.</span><span class="sxs-lookup"><span data-stu-id="a9806-119">If you have both an HTTPS and an HTTP binding, clients can still use HTTP to access the site.</span></span> <span data-ttu-id="a9806-120">Можно разрешить некоторые ресурсы с использованием HTTP, а для других ресурсов требуется SSL.</span><span class="sxs-lookup"><span data-stu-id="a9806-120">You might allow some resources to be available through HTTP, while other resources require SSL.</span></span> <span data-ttu-id="a9806-121">В этом случае используйте фильтр действий обязательного использования протокола SSL к защищенным ресурсам.</span><span class="sxs-lookup"><span data-stu-id="a9806-121">In that case, use an action filter to require SSL for the protected resources.</span></span> <span data-ttu-id="a9806-122">В следующем коде показано фильтр проверки подлинности веб-API, который проверяет наличие SSL:</span><span class="sxs-lookup"><span data-stu-id="a9806-122">The following code shows a Web API authentication filter that checks for SSL:</span></span>

[!code-csharp[Main](working-with-ssl-in-web-api/samples/sample1.cs)]

<span data-ttu-id="a9806-123">Добавьте этот фильтр действия веб-API, которые требуется SSL:</span><span class="sxs-lookup"><span data-stu-id="a9806-123">Add this filter to any Web API actions that require SSL:</span></span>

[!code-csharp[Main](working-with-ssl-in-web-api/samples/sample2.cs)]

## <a name="ssl-client-certificates"></a><span data-ttu-id="a9806-124">SSL-сертификатов клиента</span><span class="sxs-lookup"><span data-stu-id="a9806-124">SSL Client Certificates</span></span>

<span data-ttu-id="a9806-125">SSL обеспечивает проверку подлинности с помощью сертификатов инфраструктуры открытого ключа.</span><span class="sxs-lookup"><span data-stu-id="a9806-125">SSL provides authentication by using Public Key Infrastructure certificates.</span></span> <span data-ttu-id="a9806-126">Сервер должен предоставить сертификат, который проверяет подлинность сервера на клиент.</span><span class="sxs-lookup"><span data-stu-id="a9806-126">The server must provide a certificate that authenticates the server to the client.</span></span> <span data-ttu-id="a9806-127">Менее часто клиент предоставлял сертификат на сервере, но это один из вариантов проверки подлинности клиентов.</span><span class="sxs-lookup"><span data-stu-id="a9806-127">It is less common for the client to provide a certificate to the server, but this is one option for authenticating clients.</span></span> <span data-ttu-id="a9806-128">Чтобы использовать клиентские сертификаты с протоколом SSL, необходимо способ распространения сертификаты для пользователей.</span><span class="sxs-lookup"><span data-stu-id="a9806-128">To use client certificates with SSL, you need a way to distribute signed certificates to your users.</span></span> <span data-ttu-id="a9806-129">Для многих типов приложений это не будет эффективное взаимодействие с пользователем, но в некоторых средах (например, предприятие) может быть нецелесообразно.</span><span class="sxs-lookup"><span data-stu-id="a9806-129">For many application types, this will not be a good user experience, but in some environments (for example, enterprise) it may be feasible.</span></span>

| <span data-ttu-id="a9806-130">Преимущества</span><span class="sxs-lookup"><span data-stu-id="a9806-130">Advantages</span></span> | <span data-ttu-id="a9806-131">Недостатки</span><span class="sxs-lookup"><span data-stu-id="a9806-131">Disadvantages</span></span> |
| --- | --- |
| <span data-ttu-id="a9806-132">-Учетные данные сертификата надежнее, чем имя пользователя и пароль.</span><span class="sxs-lookup"><span data-stu-id="a9806-132">- Certificate credentials are stronger than username/password.</span></span> <span data-ttu-id="a9806-133">-SSL предоставляет полный безопасный канал с проверкой подлинности, целостность сообщений и шифрования сообщений.</span><span class="sxs-lookup"><span data-stu-id="a9806-133">- SSL provides a complete secure channel, with authentication, message integrity, and message encryption.</span></span> | <span data-ttu-id="a9806-134">-Необходимо получить и управлять сертификатами PKI.</span><span class="sxs-lookup"><span data-stu-id="a9806-134">- You must obtain and manage PKI certificates.</span></span> <span data-ttu-id="a9806-135">-Платформа клиента должна поддерживать SSL-сертификатов клиента.</span><span class="sxs-lookup"><span data-stu-id="a9806-135">- The client platform must support SSL client certificates.</span></span> |

<span data-ttu-id="a9806-136">Чтобы настроить службы IIS, чтобы принимать сертификаты клиентов, откройте диспетчер служб IIS и выполните следующие действия:</span><span class="sxs-lookup"><span data-stu-id="a9806-136">To configure IIS to accept client certificates, open IIS Manager and perform the following steps:</span></span>

1. <span data-ttu-id="a9806-137">Щелкните узел в древовидном представлении.</span><span class="sxs-lookup"><span data-stu-id="a9806-137">Click the site node in the tree view.</span></span>
2. <span data-ttu-id="a9806-138">Дважды щелкните **параметры SSL** функцию в средней области.</span><span class="sxs-lookup"><span data-stu-id="a9806-138">Double-click the **SSL Settings** feature in the middle pane.</span></span>
3. <span data-ttu-id="a9806-139">В разделе **клиентских сертификатов**, выберите один из следующих вариантов:</span><span class="sxs-lookup"><span data-stu-id="a9806-139">Under **Client Certificates**, select one of these options:</span></span> 

    - <span data-ttu-id="a9806-140">**Принять**: IIS будет принять сертификат клиента, но не требуется.</span><span class="sxs-lookup"><span data-stu-id="a9806-140">**Accept**: IIS will accept a certificate from the client, but does not require one.</span></span>
    - <span data-ttu-id="a9806-141">**Требуется**: требуется сертификат клиента.</span><span class="sxs-lookup"><span data-stu-id="a9806-141">**Require**: Require a client certificate.</span></span> <span data-ttu-id="a9806-142">(Чтобы включить этот параметр, необходимо также выбрать «Требовать SSL»)</span><span class="sxs-lookup"><span data-stu-id="a9806-142">(To enable this option, you must also select "Require SSL")</span></span>

<span data-ttu-id="a9806-143">В файле ApplicationHost.config, можно также задать следующие параметры:</span><span class="sxs-lookup"><span data-stu-id="a9806-143">You can also set these options in the ApplicationHost.config file:</span></span>

[!code-xml[Main](working-with-ssl-in-web-api/samples/sample3.xml)]

<span data-ttu-id="a9806-144">**SslNegotiateCert** флаг означает IIS будет принять сертификат клиента, но не требуются (эквивалент параметра «Принять» в диспетчере служб IIS).</span><span class="sxs-lookup"><span data-stu-id="a9806-144">The **SslNegotiateCert** flag means IIS will accept a certificate from the client, but does not require one (equivalent to the "Accept" option in IIS Manager).</span></span> <span data-ttu-id="a9806-145">Чтобы требуется сертификат, установите **для SslRequireCert** флаг.</span><span class="sxs-lookup"><span data-stu-id="a9806-145">To require a certificate, set the **SslRequireCert** flag.</span></span> <span data-ttu-id="a9806-146">Для тестирования, можно также задать эти параметры в IIS Express, в локальном узле applicationhost. Файл конфигурации, расположенный в «Documents\IISExpress\config».</span><span class="sxs-lookup"><span data-stu-id="a9806-146">For testing, you can also set these options in IIS Express, in the local applicationhost.Config file, located in "Documents\IISExpress\config".</span></span>

### <a name="creating-a-client-certificate-for-testing"></a><span data-ttu-id="a9806-147">Создание сертификата клиента для проверки</span><span class="sxs-lookup"><span data-stu-id="a9806-147">Creating a Client Certificate for Testing</span></span>

<span data-ttu-id="a9806-148">В целях тестирования можно использовать [MakeCert.exe](https://msdn.microsoft.com/library/bfsktky3.aspx) Создание сертификата клиента.</span><span class="sxs-lookup"><span data-stu-id="a9806-148">For testing purposes, you can use [MakeCert.exe](https://msdn.microsoft.com/library/bfsktky3.aspx) to create a client certificate.</span></span> <span data-ttu-id="a9806-149">Сначала создайте тестовым корневым центром.</span><span class="sxs-lookup"><span data-stu-id="a9806-149">First, create a test root authority:</span></span>

[!code-console[Main](working-with-ssl-in-web-api/samples/sample4.cmd)]

<span data-ttu-id="a9806-150">Makecert предложит ввести пароль для закрытого ключа.</span><span class="sxs-lookup"><span data-stu-id="a9806-150">Makecert will prompt you to enter a password for the private key.</span></span>

<span data-ttu-id="a9806-151">Добавьте сертификат в тест, который хранения сервера «доверенные корневые центры сертификации», как показано ниже:</span><span class="sxs-lookup"><span data-stu-id="a9806-151">Next, add the certificate to the test server's "Trusted Root Certification Authorities" store, as follows:</span></span>

1. <span data-ttu-id="a9806-152">Откройте консоль MMC.</span><span class="sxs-lookup"><span data-stu-id="a9806-152">Open MMC.</span></span>
2. <span data-ttu-id="a9806-153">В разделе **файл**выберите **добавить или удалить оснастку**.</span><span class="sxs-lookup"><span data-stu-id="a9806-153">Under **File**, select **Add/Remove Snap-In**.</span></span>
3. <span data-ttu-id="a9806-154">Выберите **учетная запись компьютера**.</span><span class="sxs-lookup"><span data-stu-id="a9806-154">Select **Computer Account**.</span></span>
4. <span data-ttu-id="a9806-155">Выберите **локального компьютера** и завершите работу мастера.</span><span class="sxs-lookup"><span data-stu-id="a9806-155">Select **Local computer** and complete the wizard.</span></span>
5. <span data-ttu-id="a9806-156">В области навигации разверните узел «Доверенные корневые центры сертификации».</span><span class="sxs-lookup"><span data-stu-id="a9806-156">Under the navigation pane, expand the "Trusted Root Certification Authorities" node.</span></span>
6. <span data-ttu-id="a9806-157">На **действия** последовательно выберите пункты **все задачи**, а затем нажмите кнопку **импорта** для запуска мастера импорта сертификатов.</span><span class="sxs-lookup"><span data-stu-id="a9806-157">On the **Action** menu, point to **All Tasks**, and then click **Import** to start the Certificate Import Wizard.</span></span>
7. <span data-ttu-id="a9806-158">Перейдите к файлу сертификата TempCA.cer.</span><span class="sxs-lookup"><span data-stu-id="a9806-158">Browse to the certificate file, TempCA.cer.</span></span>
8. <span data-ttu-id="a9806-159">Нажмите кнопку **откройте**, нажмите кнопку **Далее** и завершите работу мастера.</span><span class="sxs-lookup"><span data-stu-id="a9806-159">Click **Open**, then click **Next** and complete the wizard.</span></span> <span data-ttu-id="a9806-160">(Вам будет предложено повторно ввести пароль.)</span><span class="sxs-lookup"><span data-stu-id="a9806-160">(You will be prompted to re-enter the password.)</span></span>

<span data-ttu-id="a9806-161">Теперь создайте сертификат клиента, который подписывается первый сертификат:</span><span class="sxs-lookup"><span data-stu-id="a9806-161">Now create a client certificate that is signed by the first certificate:</span></span>

[!code-console[Main](working-with-ssl-in-web-api/samples/sample5.cmd)]

### <a name="using-client-certificates-in-web-api"></a><span data-ttu-id="a9806-162">Использование сертификатов клиентов в веб-API</span><span class="sxs-lookup"><span data-stu-id="a9806-162">Using Client Certificates in Web API</span></span>

<span data-ttu-id="a9806-163">На стороне сервера можно получить сертификат клиента, вызвав [GetClientCertificate](https://msdn.microsoft.com/library/system.net.http.httprequestmessageextensions.getclientcertificate.aspx) в сообщении запроса.</span><span class="sxs-lookup"><span data-stu-id="a9806-163">On the server side, you can get the client certificate by calling [GetClientCertificate](https://msdn.microsoft.com/library/system.net.http.httprequestmessageextensions.getclientcertificate.aspx) on the request message.</span></span> <span data-ttu-id="a9806-164">Метод возвращает значение null, если нет сертификата клиента.</span><span class="sxs-lookup"><span data-stu-id="a9806-164">The method returns null if there is no client certificate.</span></span> <span data-ttu-id="a9806-165">В противном случае он возвращает **X509Certificate2** экземпляра.</span><span class="sxs-lookup"><span data-stu-id="a9806-165">Otherwise, it returns an **X509Certificate2** instance.</span></span> <span data-ttu-id="a9806-166">Этот объект можно используйте для получения сведений из сертификатов, такие как издатель и субъект.</span><span class="sxs-lookup"><span data-stu-id="a9806-166">Use this object to get information from the certificate, such as the issuer and subject.</span></span> <span data-ttu-id="a9806-167">Затем эти сведения можно использовать для проверки подлинности или авторизации.</span><span class="sxs-lookup"><span data-stu-id="a9806-167">Then you can use this information for authentication and/or authorization.</span></span>

[!code-csharp[Main](working-with-ssl-in-web-api/samples/sample6.cs)]
