---
uid: web-pages/overview/testing-and-debugging/aspnet-web-pages-razor-troubleshooting-guide
title: Веб-страницы ASP.NET руководство по устранению неполадок (Razor) | Документы Microsoft
author: tfitzmac
description: В этой статье описываются проблемы, которые могут возникнуть при работе с веб-страниц ASP.NET (Razor) и некоторые рекомендуемые решения. Страница ASP.NET Web версии программного обеспечения...
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/10/2014
ms.topic: article
ms.assetid: 2a2c1833-0bfe-4e2e-9cc0-341b52c7b121
ms.technology: dotnet-webpages
ms.prod: .net-framework
msc.legacyurl: /web-pages/overview/testing-and-debugging/aspnet-web-pages-razor-troubleshooting-guide
msc.type: authoredcontent
ms.openlocfilehash: ec51169ccea0016712de3fdb28a16a174150a8bd
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/06/2018
ms.locfileid: "30898517"
---
<a name="aspnet-web-pages-razor-troubleshooting-guide"></a><span data-ttu-id="d2bdf-104">Веб-страницы ASP.NET руководство по устранению неполадок (Razor)</span><span class="sxs-lookup"><span data-stu-id="d2bdf-104">ASP.NET Web Pages (Razor) Troubleshooting Guide</span></span>
====================
<span data-ttu-id="d2bdf-105">по [Tom FitzMacken](https://github.com/tfitzmac)</span><span class="sxs-lookup"><span data-stu-id="d2bdf-105">by [Tom FitzMacken](https://github.com/tfitzmac)</span></span>

> <span data-ttu-id="d2bdf-106">В этой статье описываются проблемы, которые могут возникнуть при работе с веб-страниц ASP.NET (Razor) и некоторые рекомендуемые решения.</span><span class="sxs-lookup"><span data-stu-id="d2bdf-106">This article describes issues that you might have when working with ASP.NET Web Pages (Razor) and some suggested solutions.</span></span>
> 
> ## <a name="software-versions"></a><span data-ttu-id="d2bdf-107">Версии программного обеспечения</span><span class="sxs-lookup"><span data-stu-id="d2bdf-107">Software versions</span></span>
> 
> 
> - <span data-ttu-id="d2bdf-108">Веб-страниц ASP.NET (Razor) 3</span><span class="sxs-lookup"><span data-stu-id="d2bdf-108">ASP.NET Web Pages (Razor) 3</span></span>
>   
> 
> <span data-ttu-id="d2bdf-109">Этот учебник также работает с 2 веб-страниц ASP.NET и веб-страниц ASP.NET 1.0.</span><span class="sxs-lookup"><span data-stu-id="d2bdf-109">This tutorial also works with ASP.NET Web Pages 2 and ASP.NET Web Pages 1.0.</span></span>


<span data-ttu-id="d2bdf-110">В этом разделе содержатся следующие подразделы.</span><span class="sxs-lookup"><span data-stu-id="d2bdf-110">This topic contains the following sections:</span></span>

- [<span data-ttu-id="d2bdf-111">Проблемы, связанные с управлением страниц</span><span class="sxs-lookup"><span data-stu-id="d2bdf-111">Issues with Running Pages</span></span>](#Issues_Running_.cshtml_Pages)
- [<span data-ttu-id="d2bdf-112">Проблемы, связанные с кодом Razor</span><span class="sxs-lookup"><span data-stu-id="d2bdf-112">Issues with Razor Code</span></span>](#IssuesWithRazorCode)
- [<span data-ttu-id="d2bdf-113">Проблемы безопасности и членство</span><span class="sxs-lookup"><span data-stu-id="d2bdf-113">Issues with Security and Membership</span></span>](#membership)
- [<span data-ttu-id="d2bdf-114">Проблемы с отправкой электронной почты</span><span class="sxs-lookup"><span data-stu-id="d2bdf-114">Issues with Sending Email</span></span>](#email)
- [<span data-ttu-id="d2bdf-115">Дополнительные ресурсы</span><span class="sxs-lookup"><span data-stu-id="d2bdf-115">Additional Resources</span></span>](#AdditionalResources)

<span data-ttu-id="d2bdf-116">Общие вопросы в разделе [веб-страниц ASP.NET (Razor) часто задаваемые вопросы о](https://go.microsoft.com/fwlink/?LinkId=253000).</span><span class="sxs-lookup"><span data-stu-id="d2bdf-116">For general questions, see [ASP.NET Web Pages (Razor) FAQ](https://go.microsoft.com/fwlink/?LinkId=253000).</span></span>

<a id="Issues_Running_.cshtml_Pages"></a>
## <a name="issues-with-running-pages"></a><span data-ttu-id="d2bdf-117">Проблемы, связанные с управлением страниц</span><span class="sxs-lookup"><span data-stu-id="d2bdf-117">Issues with Running Pages</span></span>

<span data-ttu-id="d2bdf-118">Можно предотвратить различных проблем *.cshtml* и *.vbhtml* страницы из выполняется надлежащим образом.</span><span class="sxs-lookup"><span data-stu-id="d2bdf-118">A variety of issues can prevent *.cshtml* and *.vbhtml* pages from running properly.</span></span> <span data-ttu-id="d2bdf-119">В этом разделе перечислены распространенные сообщения об ошибках и вероятные причины.</span><span class="sxs-lookup"><span data-stu-id="d2bdf-119">This section lists common error messages and likely causes.</span></span>

### <a name="http-error-403---forbidden-access-is-denied"></a><span data-ttu-id="d2bdf-120">Ошибка HTTP 403 - Запрещено: Доступ запрещен</span><span class="sxs-lookup"><span data-stu-id="d2bdf-120">HTTP Error 403 - Forbidden: Access is denied</span></span>

<span data-ttu-id="d2bdf-121">*У вас разрешения на просмотр этого каталога или страницы, используя предоставленные учетные данные.*</span><span class="sxs-lookup"><span data-stu-id="d2bdf-121">*You do not have permission to view this directory or page using the credentials that you supplied.*</span></span>

<span data-ttu-id="d2bdf-122">Эта ошибка может возникать, если сервер не использует правильную версию платформы .NET Framework.</span><span class="sxs-lookup"><span data-stu-id="d2bdf-122">This error can occur if the server is not running the correct version of the .NET Framework.</span></span> <span data-ttu-id="d2bdf-123">Убедитесь, что компьютер, на котором выполняется сервер (локально или удаленно) имеет по крайней мере .NET Framework 4.</span><span class="sxs-lookup"><span data-stu-id="d2bdf-123">Make sure that the computer that's running the server (locally or remotely) has at least the .NET Framework 4 installed.</span></span> <span data-ttu-id="d2bdf-124">Убедитесь, что само приложение настроен для запуска нужную версию.</span><span class="sxs-lookup"><span data-stu-id="d2bdf-124">Also make sure that the application itself is configured to run the right version.</span></span>

<span data-ttu-id="d2bdf-125">При возникновении этой проблемы локально при работе в WebMatrix, выберите **сайта** рабочей области и в дереве нажмите кнопку **параметры**.</span><span class="sxs-lookup"><span data-stu-id="d2bdf-125">If you see this problem locally while working in WebMatrix, click the **Site** workspace, and then in the treeview click **Settings**.</span></span> <span data-ttu-id="d2bdf-126">В **выберите версии .NET Framework** выберите **.NET 4 (интегрированная)**.</span><span class="sxs-lookup"><span data-stu-id="d2bdf-126">In the **Select .NET Framework Version** list, select **.NET 4 (Integrated)**.</span></span> <span data-ttu-id="d2bdf-127">Если эта версия уже установлено, попробуйте запустить WebMatrix от имени администратора.</span><span class="sxs-lookup"><span data-stu-id="d2bdf-127">If this version is already set, try running WebMatrix as an administrator.</span></span>

<span data-ttu-id="d2bdf-128">Убедитесь в том, что корневой веб-сайт имеет хотя бы один *.cshtml* файл в нем.</span><span class="sxs-lookup"><span data-stu-id="d2bdf-128">Make sure that the root of your website has at least one *.cshtml* file in it.</span></span>

<span data-ttu-id="d2bdf-129">Если вы видите эту ошибку при веб-сервера на удаленном сервере, обратитесь к администратору сервера.</span><span class="sxs-lookup"><span data-stu-id="d2bdf-129">If you see this error when the web server is on a remote server, contact the server administrator.</span></span> <span data-ttu-id="d2bdf-130">Убедитесь, что сервер имеет .NET Framework 4 или более поздней версии.</span><span class="sxs-lookup"><span data-stu-id="d2bdf-130">Make sure that the server has the .NET Framework 4 or later installed.</span></span> <span data-ttu-id="d2bdf-131">Убедитесь, что приложение запущено в пуле приложений, настроенный для использования этой версии платформы.NET Framework.</span><span class="sxs-lookup"><span data-stu-id="d2bdf-131">Also make sure that the application is running in an application pool that's configured to use that version of the.NET Framework.</span></span>

<span data-ttu-id="d2bdf-132">Если у вас есть контроль над сервером, убедитесь, что она запущена правильная версия платформы .NET Framework.</span><span class="sxs-lookup"><span data-stu-id="d2bdf-132">If you have control over the server, make sure it's running the correct version of the .NET Framework.</span></span> <span data-ttu-id="d2bdf-133">Можно также попытаться восстановить установку, запустив `aspnet_regiis -iru` команды.</span><span class="sxs-lookup"><span data-stu-id="d2bdf-133">You might also try repairing the installation by running the `aspnet_regiis -iru` command.</span></span> <span data-ttu-id="d2bdf-134">(Например, если IIS устанавливается после установки .NET Framework, службы IIS не настраивается правильно для запуска страницы ASP.NET.) Дополнительные сведения см. в разделе [средство регистрации IIS ASP.NET (Aspnet\_regiis.exe)](https://msdn.microsoft.com/library/k6h9cz8h(v=vs.100).aspx).</span><span class="sxs-lookup"><span data-stu-id="d2bdf-134">(For example, if you install IIS after you install the .NET Framework, IIS will not be correctly configured to run ASP.NET pages.) For more information, see [ASP.NET IIS Registration Tool (Aspnet\_regiis.exe)](https://msdn.microsoft.com/library/k6h9cz8h(v=vs.100).aspx).</span></span>

### <a name="http-error-40314---forbidden"></a><span data-ttu-id="d2bdf-135">Ошибка HTTP 403.14 - запрещено</span><span class="sxs-lookup"><span data-stu-id="d2bdf-135">HTTP Error 403.14 - Forbidden</span></span>

<span data-ttu-id="d2bdf-136">*Веб-сервер настраивается, чтобы не формировать списка содержимого этого каталога.*</span><span class="sxs-lookup"><span data-stu-id="d2bdf-136">*The Web server is configured to not list the contents of this directory.*</span></span>

<span data-ttu-id="d2bdf-137">Эта ошибка может возникать, если запросить ресурс, который защищен (как *Web.config* файла), или в папке, которая защищена (как *приложения\_данные* или *приложения\_Код*).</span><span class="sxs-lookup"><span data-stu-id="d2bdf-137">This error can occur if you request a resource that's protected (like the *Web.config* file) or that's in a folder that's protected (like *App\_Data* or *App\_Code*).</span></span>

### <a name="http-error-40417---not-found"></a><span data-ttu-id="d2bdf-138">Ошибка HTTP 404.17 - не найдено</span><span class="sxs-lookup"><span data-stu-id="d2bdf-138">HTTP Error 404.17 - Not Found</span></span>

<span data-ttu-id="d2bdf-139">*Содержимое запроса является сценарием и не обрабатывается обработчиком статических файлов.*</span><span class="sxs-lookup"><span data-stu-id="d2bdf-139">*The requested content appears to be script and will not be served by the static file handler.*</span></span>

<span data-ttu-id="d2bdf-140">Эта ошибка может возникать, если сервер правильно настроен для использования платформы .NET Framework 4 или более поздней версии и, следовательно, не распознает код в `@{ }` блоков.</span><span class="sxs-lookup"><span data-stu-id="d2bdf-140">This error can occur if the server is not configured correctly to use the .NET Framework 4 or later, and therefore does not recognize the code in `@{ }` blocks.</span></span> <span data-ttu-id="d2bdf-141">См. в описании ранее *ошибки HTTP 403 - Запрещено: доступ запрещен*.</span><span class="sxs-lookup"><span data-stu-id="d2bdf-141">See the description earlier for *HTTP Error 403 - Forbidden: Access is denied*.</span></span>

### <a name="http-error-4047---not-found"></a><span data-ttu-id="d2bdf-142">Ошибка HTTP 404.7 - не найдено</span><span class="sxs-lookup"><span data-stu-id="d2bdf-142">HTTP Error 404.7 - Not Found</span></span>

<span data-ttu-id="d2bdf-143">*Модуль фильтрации запросов настроен для блокировки расширения файла*</span><span class="sxs-lookup"><span data-stu-id="d2bdf-143">*The request filtering module is configured to deny the file extension*</span></span>

<span data-ttu-id="d2bdf-144">Эта ошибка может возникать, если *.cshtml* или *.vbhtml* расширения явно заблокированы на сервере.</span><span class="sxs-lookup"><span data-stu-id="d2bdf-144">This error can occur if *.cshtml* or *.vbhtml* extensions have been explicitly blocked on the server.</span></span> <span data-ttu-id="d2bdf-145">Признаком этой проблемы является этой работы URL-адреса, когда они не включают расширение, но URL-адреса, которые включают *.cshtml* или *.vbhtml* не работают.</span><span class="sxs-lookup"><span data-stu-id="d2bdf-145">A symptom of this problem is that URLs work when they do not include the extension, but URLs that include *.cshtml* or *.vbhtml* do not work.</span></span> <span data-ttu-id="d2bdf-146">Возможным решением является для повторного включения расширения на сайте *Web.config* файла.</span><span class="sxs-lookup"><span data-stu-id="d2bdf-146">A possible solution is to re-enable the extensions in the site's *Web.config* file.</span></span> <span data-ttu-id="d2bdf-147">В следующем примере показано, как включить *.cshtml* расширения.</span><span class="sxs-lookup"><span data-stu-id="d2bdf-147">The following example shows how to enable the *.cshtml* extension.</span></span>

[!code-xml[Main](aspnet-web-pages-razor-troubleshooting-guide/samples/sample1.xml?highlight=5-6)]

### <a name="http-error-4048---not-found"></a><span data-ttu-id="d2bdf-148">Ошибка HTTP 404.8 - не найдено</span><span class="sxs-lookup"><span data-stu-id="d2bdf-148">HTTP Error 404.8 - Not Found</span></span>

<span data-ttu-id="d2bdf-149">*Модуль фильтрации запросов настроен для блокировки пути в URL-адреса, содержащего раздел hiddenSegment.*</span><span class="sxs-lookup"><span data-stu-id="d2bdf-149">*The request filtering module is configured to deny a path in the URL that contains a hiddenSegment section.*</span></span>

<span data-ttu-id="d2bdf-150">Эта ошибка может возникать, если запросить ресурс, который защищен (как *Web.config* файла), или в папке, которая защищена (как *приложения\_данные* или *приложения\_Код*).</span><span class="sxs-lookup"><span data-stu-id="d2bdf-150">This error can occur if you request a resource that's protected (like the *Web.config* file) or that's in a folder that's protected (like *App\_Data* or *App\_Code*).</span></span>

### <a name="this-type-of-page-is-not-served-server-error-in--application"></a><span data-ttu-id="d2bdf-151">Этот тип страницы не обслуживается (ошибка сервера в приложении '/')</span><span class="sxs-lookup"><span data-stu-id="d2bdf-151">This type of page is not served (Server Error in '/' Application)</span></span>

<span data-ttu-id="d2bdf-152">См. в описании ранее 404.17 ошибки HTTP.</span><span class="sxs-lookup"><span data-stu-id="d2bdf-152">See the description earlier for HTTP Error 404.17.</span></span>

<a id="IssuesWithRazorCode"></a>
## <a name="issues-with-razor-code"></a><span data-ttu-id="d2bdf-153">Проблемы, связанные с кодом Razor</span><span class="sxs-lookup"><span data-stu-id="d2bdf-153">Issues with Razor code</span></span>

### <a name="the-name-class-does-not-exist-in-the-current-context"></a><span data-ttu-id="d2bdf-154">Имя "*класс*" не существует в текущем контексте</span><span class="sxs-lookup"><span data-stu-id="d2bdf-154">The name '*class*' does not exist in the current context</span></span>

<span data-ttu-id="d2bdf-155">Часто причина, появится эта ошибка является `class` ссылки вспомогательного класса, но его не установлен.</span><span class="sxs-lookup"><span data-stu-id="d2bdf-155">Often, a reason you see this error is that `class` references a helper, but the helper is not installed.</span></span> <span data-ttu-id="d2bdf-156">Например при попытке использовать вспомогательный класс, но еще не установили пакет из NuGet, вы увидите эту ошибку.</span><span class="sxs-lookup"><span data-stu-id="d2bdf-156">For example, if you try to use a helper, but if you haven't installed the package from NuGet, you'll see this error.</span></span> <span data-ttu-id="d2bdf-157">Коллекция в WebMatrix служит для поиска и установки вспомогательным методом.</span><span class="sxs-lookup"><span data-stu-id="d2bdf-157">Use the Gallery in WebMatrix to find and install the helper.</span></span>

<span data-ttu-id="d2bdf-158">Если вспомогательное приложение устанавливается, но по-прежнему не распознается страницей его, попробуйте добавить добавить `using` инструкции в код.</span><span class="sxs-lookup"><span data-stu-id="d2bdf-158">If the helper is installed, but the page still doesn't recognize it, try adding add a `using` statement to the code.</span></span> <span data-ttu-id="d2bdf-159">В `using` оператор, справочник по языку пространство имен, которое включает модуль поддержки.</span><span class="sxs-lookup"><span data-stu-id="d2bdf-159">In the `using` statement, reference the namespace that includes the helper.</span></span> <span data-ttu-id="d2bdf-160">Например, базовые вспомогательные средства, которые находятся в пакете вспомогательных методов ASP.NET Web находятся в `System.Web.Helpers` пространства имен.</span><span class="sxs-lookup"><span data-stu-id="d2bdf-160">For example, the basic helpers that are in the ASP.NET Web Helpers package are in the `System.Web.Helpers` namespace.</span></span> <span data-ttu-id="d2bdf-161">В верхней части страницы, в которой вы хотите использовать его добавьте следующую строку:</span><span class="sxs-lookup"><span data-stu-id="d2bdf-161">At the top of the page where you want to use the helper, add this line:</span></span>

`@using Microsoft.Web.Helpers;`

<a id="membership"></a>
## <a name="issues-with-security-and-membership"></a><span data-ttu-id="d2bdf-162">Проблемы безопасности и членство</span><span class="sxs-lookup"><span data-stu-id="d2bdf-162">Issues with Security and Membership</span></span>

<span data-ttu-id="d2bdf-163">Если вы используете систему встроенной безопасности (членства) ASP.NET Web Pages (Razor), могут возникнуть следующие проблемы.</span><span class="sxs-lookup"><span data-stu-id="d2bdf-163">If you are using the built-in security (membership) system in ASP.NET Web Pages (Razor), you might encounter the following issues.</span></span>

### <a name="to-call-this-method-the-membershipprovider-property-must-be-an-instance-of-extendedmembershipprovider"></a><span data-ttu-id="d2bdf-164">Чтобы вызвать этот метод, свойство «Membership.Provider» должно быть экземпляр «ExtendedMembershipProvider»</span><span class="sxs-lookup"><span data-stu-id="d2bdf-164">To call this method, the "Membership.Provider" property must be an instance of "ExtendedMembershipProvider"</span></span>

<span data-ttu-id="d2bdf-165">Эта ошибка может указывать, что не `AspNetSqlMembershipProvider` класс настроен.</span><span class="sxs-lookup"><span data-stu-id="d2bdf-165">This error can indicate that no `AspNetSqlMembershipProvider` class is configured.</span></span> <span data-ttu-id="d2bdf-166">(Симптом — что сайт работает локально, но выдает ошибки при публикации на сервере поставщика услуг размещения). Один для исправления этой проблемы является явным образом включить простое членство, добавив следующий код к сайту *Web.config* файла:</span><span class="sxs-lookup"><span data-stu-id="d2bdf-166">(A symptom is that the site works fine locally but throws this error when you publish it to a hosting provider's server.) One fix for this problem is to explicitly enable simple membership by adding the following to the site's *Web.config* file:</span></span>

[!code-xml[Main](aspnet-web-pages-razor-troubleshooting-guide/samples/sample2.xml?highlight=6)]

<a id="email"></a>
## <a name="issues-with-sending-email"></a><span data-ttu-id="d2bdf-167">Проблемы с отправкой электронной почты</span><span class="sxs-lookup"><span data-stu-id="d2bdf-167">Issues with Sending Email</span></span>

<span data-ttu-id="d2bdf-168">Проблемы с отправкой электронной почты может оказаться сложной задачей для отладки.</span><span class="sxs-lookup"><span data-stu-id="d2bdf-168">Problems with sending email can be challenging to debug.</span></span> <span data-ttu-id="d2bdf-169">Первоначальную проблему можно, не удается подключиться к серверу SMTP.</span><span class="sxs-lookup"><span data-stu-id="d2bdf-169">An initial problem can be that you can't connect to the SMTP server.</span></span> <span data-ttu-id="d2bdf-170">Если соединение установлено успешно, ASP.NET отправляет сообщение на SMTP-сервер.</span><span class="sxs-lookup"><span data-stu-id="d2bdf-170">If the connection is successful, ASP.NET hands the message off to the SMTP server.</span></span> <span data-ttu-id="d2bdf-171">Тем не менее могут возникнуть проблемы с само сообщение, препятствующую его отправки SMTP-сервера.</span><span class="sxs-lookup"><span data-stu-id="d2bdf-171">However, there can be problems with the message itself that prevents the SMTP server from sending it.</span></span>

<span data-ttu-id="d2bdf-172">Если приложение не отправить по электронной почте, выполните следующее:</span><span class="sxs-lookup"><span data-stu-id="d2bdf-172">If your application does not successfully send email, try the following:</span></span>

- <span data-ttu-id="d2bdf-173">Имя SMTP-сервера часто является примерно `smtp.provider.com` или `smtp.provider.net`.</span><span class="sxs-lookup"><span data-stu-id="d2bdf-173">The SMTP server name is often something like `smtp.provider.com` or `smtp.provider.net`.</span></span> <span data-ttu-id="d2bdf-174">Однако, если публикации сайта поставщика услуг размещения, имя SMTP-сервера на этом этапе может быть `localhost`.</span><span class="sxs-lookup"><span data-stu-id="d2bdf-174">However, if you publish your site to a hosting provider, the SMTP server name at that point might be `localhost`.</span></span> <span data-ttu-id="d2bdf-175">Это происходит потому, что вы опубликовали и сайт работает на сервере поставщика, SMTP-сервер может быть локальным с точки зрения приложения.</span><span class="sxs-lookup"><span data-stu-id="d2bdf-175">This situation occurs because after you've published and your site is running on the provider's server, the SMTP server might be local from the perspective of your application.</span></span> <span data-ttu-id="d2bdf-176">Это изменение в имена серверов могут означать, что нужно изменить имя SMTP-сервера в рамках процесса публикации.</span><span class="sxs-lookup"><span data-stu-id="d2bdf-176">This change in server names might mean that you have to change the SMTP server name as part of your publishing process.</span></span>
- <span data-ttu-id="d2bdf-177">Номер порта обычно — 25.</span><span class="sxs-lookup"><span data-stu-id="d2bdf-177">The port number is usually 25.</span></span> <span data-ttu-id="d2bdf-178">Тем не менее, некоторые поставщики требуют использовать порт 587 или некоторых других портов.</span><span class="sxs-lookup"><span data-stu-id="d2bdf-178">However, some providers require you to use port 587 or some other port.</span></span> <span data-ttu-id="d2bdf-179">Узнайте у владельца SMTP-сервера, номер порта ожидают, что позволяет использовать.</span><span class="sxs-lookup"><span data-stu-id="d2bdf-179">Check with the owner of the SMTP server what port number they expect you to use.</span></span>
- <span data-ttu-id="d2bdf-180">Убедитесь, что используются правильные учетные данные.</span><span class="sxs-lookup"><span data-stu-id="d2bdf-180">Make sure that you use the right credentials.</span></span> <span data-ttu-id="d2bdf-181">При публикации веб-узла поставщика услуг размещения, используйте учетные данные, которые поставщик указал специально предназначены для электронной почты.</span><span class="sxs-lookup"><span data-stu-id="d2bdf-181">If you've published your site to a hosting provider, use the credentials that the provider has specifically indicated are for email.</span></span> <span data-ttu-id="d2bdf-182">Эти учетные данные могут отличаться от учетных данных, которые можно использовать для публикации.</span><span class="sxs-lookup"><span data-stu-id="d2bdf-182">These credentials might be different from the credentials you use to publish.</span></span>
- <span data-ttu-id="d2bdf-183">Иногда учетные данные не требуются вообще.</span><span class="sxs-lookup"><span data-stu-id="d2bdf-183">Sometimes you don't need credentials at all.</span></span> <span data-ttu-id="d2bdf-184">Если вы отправляете сообщение электронной почты с помощью ваш поставщик услуг Интернета, вашего поставщика услуг электронной почты может быть уже известен учетные данные.</span><span class="sxs-lookup"><span data-stu-id="d2bdf-184">If you're sending email by using your personal ISP, your email provider might already know your credentials.</span></span> <span data-ttu-id="d2bdf-185">После публикации, может потребоваться использовать другие учетные данные, чем при проверке на локальном компьютере.</span><span class="sxs-lookup"><span data-stu-id="d2bdf-185">After you publish, you might need to use different credentials than when you test on your local computer.</span></span>
- <span data-ttu-id="d2bdf-186">Если вашего поставщика услуг электронной почты использует шифрование, `WebMail.EnableSsl` для `true`.</span><span class="sxs-lookup"><span data-stu-id="d2bdf-186">If your email provider uses encryption, set `WebMail.EnableSsl` to `true`.</span></span>

<span data-ttu-id="d2bdf-187">Если возникает ошибка при отправке сообщения электронной почты, может появиться Стандартная ASP.NET сообщение об ошибке, который выглядит следующим образом:</span><span class="sxs-lookup"><span data-stu-id="d2bdf-187">If there is an error sending email, you might see a standard ASP.NET error message, which looks like this:</span></span>

![Сообщение об ошибке ASP.NET при наличии проблем с адресом электронной почты](aspnet-web-pages-razor-troubleshooting-guide/_static/image1.png)

<span data-ttu-id="d2bdf-189">Можно также отлаживать неполадок при отправке электронной почты с помощью `try-catch` блока, как показано в следующем примере.</span><span class="sxs-lookup"><span data-stu-id="d2bdf-189">You can also debug problems with sending email by using a `try-catch` block, as in the following example.</span></span> <span data-ttu-id="d2bdf-190">При использовании `try-catch` блок ASP.NET не отображает сообщения об ошибках standard.</span><span class="sxs-lookup"><span data-stu-id="d2bdf-190">When you use a `try-catch` block, ASP.NET does not display its standard error messages.</span></span> <span data-ttu-id="d2bdf-191">Вместо этого можно записать ошибку в `catch` части блока.</span><span class="sxs-lookup"><span data-stu-id="d2bdf-191">Instead, you can capture the error in the `catch` portion of the block.</span></span>

[!code-cshtml[Main](aspnet-web-pages-razor-troubleshooting-guide/samples/sample3.cshtml)]

<span data-ttu-id="d2bdf-192">Подставьте нужные значения для `your-SMTP-server-name`, и т. д.</span><span class="sxs-lookup"><span data-stu-id="d2bdf-192">Substitute the appropriate values for `your-SMTP-server-name`, and so on.</span></span> <span data-ttu-id="d2bdf-193">Ниже приведены некоторые сообщения об ошибках, которые можно увидеть таким образом:</span><span class="sxs-lookup"><span data-stu-id="d2bdf-193">Some of the error messages you might see this way include the following:</span></span>

- <span data-ttu-id="d2bdf-194">*Сбой при отправке электронной почты.*</span><span class="sxs-lookup"><span data-stu-id="d2bdf-194">*Failure sending mail.*</span></span>

    <span data-ttu-id="d2bdf-195">- или -</span><span class="sxs-lookup"><span data-stu-id="d2bdf-195">-or-</span></span>

    <span data-ttu-id="d2bdf-196">*Попытка подключения не удалась, поскольку подключившейся стороне не ответил должным образом после определенного времени или сбой, так как подключенного узла подключения, не удалось получить отклик*</span><span class="sxs-lookup"><span data-stu-id="d2bdf-196">*A connection attempt failed because the connected party did not properly respond after a period of time, or established connection failed because connected host has failed to respond*</span></span>

    <span data-ttu-id="d2bdf-197">Эта ошибка обычно означает, что приложение не удалось подключиться к серверу SMTP.</span><span class="sxs-lookup"><span data-stu-id="d2bdf-197">This error usually means that the application could not connect to the SMTP server.</span></span> <span data-ttu-id="d2bdf-198">Проверьте имя сервера и номер порта.</span><span class="sxs-lookup"><span data-stu-id="d2bdf-198">Check the server name and port number.</span></span>
- <span data-ttu-id="d2bdf-199"><em>Почтовый ящик недоступен. Получен ответ сервера: 5.1.0 &lt; someuser@invaliddomain &gt; отправителя отклонен: недопустимый отправитель домена</em></span><span class="sxs-lookup"><span data-stu-id="d2bdf-199"><em>Mailbox unavailable. The server response was: 5.1.0 &lt;someuser@invaliddomain&gt; sender rejected : invalid sender domain</em></span></span>

    <span data-ttu-id="d2bdf-200">Это сообщение может указывать, что `From` адрес указано неправильно или отсутствует.</span><span class="sxs-lookup"><span data-stu-id="d2bdf-200">This message can indicate that the `From` address is not correct or is missing.</span></span>
- <span data-ttu-id="d2bdf-201">*Указанная строка не форму, необходимую для адреса электронной почты.*</span><span class="sxs-lookup"><span data-stu-id="d2bdf-201">*The specified string is not in the form required for an email address.*</span></span>

    <span data-ttu-id="d2bdf-202">Эта ошибка может означать, что значение `To` или `From` свойства не распознаются как адреса электронной почты.</span><span class="sxs-lookup"><span data-stu-id="d2bdf-202">This error might indicate that the value of the `To` or `From` properties are not recognized as email addresses.</span></span> <span data-ttu-id="d2bdf-203">(ASP.NET не может проверить, адрес электронной почты допустим, только что он, в правильном формате, например *name@domain.com*.)</span><span class="sxs-lookup"><span data-stu-id="d2bdf-203">(ASP.NET cannot check that the email address is valid, only that it's in the correct format, like *name@domain.com*.)</span></span>

> [!NOTE]
> <span data-ttu-id="d2bdf-204">Удалите разметку, которая выводит сообщение об ошибке (`@errorMessage`) перед публикацией страницы действующем сайте.</span><span class="sxs-lookup"><span data-stu-id="d2bdf-204">Remove the markup that displays the error (`@errorMessage`) before you publish the page to a live site.</span></span> <span data-ttu-id="d2bdf-205">Не рекомендуется разрешать пользователям просматривать сообщения об ошибках, получаемых с сервера.</span><span class="sxs-lookup"><span data-stu-id="d2bdf-205">It's not a good idea to let users see error messages that you get from a server.</span></span>


<a id="AdditionalResources"></a>
## <a name="additional-resources"></a><span data-ttu-id="d2bdf-206">Дополнительные ресурсы</span><span class="sxs-lookup"><span data-stu-id="d2bdf-206">Additional Resources</span></span>

[<span data-ttu-id="d2bdf-207">Вопросы и ответы по веб-страницам ASP.NET (Razor)</span><span class="sxs-lookup"><span data-stu-id="d2bdf-207">ASP.NET Web Pages (Razor) FAQ</span></span>](https://go.microsoft.com/fwlink/?LinkId=253000)

<span data-ttu-id="d2bdf-208">[WebMatrix и ASP.NET Web Pages](https://forums.asp.net/1224.aspx/1?WebMatrix) форума на веб-сайта ASP.NET</span><span class="sxs-lookup"><span data-stu-id="d2bdf-208">[WebMatrix and ASP.NET Web Pages](https://forums.asp.net/1224.aspx/1?WebMatrix) forum on the ASP.NET website</span></span>
