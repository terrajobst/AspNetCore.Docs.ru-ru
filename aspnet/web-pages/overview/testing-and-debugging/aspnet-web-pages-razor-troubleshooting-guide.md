---
uid: web-pages/overview/testing-and-debugging/aspnet-web-pages-razor-troubleshooting-guide
title: Веб-страницы ASP.NET (Razor) устранение неполадок | Документация Майкрософт
author: tfitzmac
description: В этой статье описываются проблемы, вы можете столкнуться при работе с веб-страниц ASP.NET (Razor) и некоторые предлагаемые решения. Версии программного обеспечения — страница ASP.NET Web...
ms.author: riande
ms.date: 02/10/2014
ms.assetid: 2a2c1833-0bfe-4e2e-9cc0-341b52c7b121
msc.legacyurl: /web-pages/overview/testing-and-debugging/aspnet-web-pages-razor-troubleshooting-guide
msc.type: authoredcontent
ms.openlocfilehash: c27139a720decd34a4ab89e6f93e71c97d123b45
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 08/16/2018
ms.locfileid: "41836557"
---
<a name="aspnet-web-pages-razor-troubleshooting-guide"></a><span data-ttu-id="0b1d6-104">Веб-страницы ASP.NET (Razor) устранение неполадок</span><span class="sxs-lookup"><span data-stu-id="0b1d6-104">ASP.NET Web Pages (Razor) Troubleshooting Guide</span></span>
====================
<span data-ttu-id="0b1d6-105">по [Tom FitzMacken](https://github.com/tfitzmac)</span><span class="sxs-lookup"><span data-stu-id="0b1d6-105">by [Tom FitzMacken](https://github.com/tfitzmac)</span></span>

> <span data-ttu-id="0b1d6-106">В этой статье описываются проблемы, вы можете столкнуться при работе с веб-страниц ASP.NET (Razor) и некоторые предлагаемые решения.</span><span class="sxs-lookup"><span data-stu-id="0b1d6-106">This article describes issues that you might have when working with ASP.NET Web Pages (Razor) and some suggested solutions.</span></span>
> 
> ## <a name="software-versions"></a><span data-ttu-id="0b1d6-107">Версии программного обеспечения</span><span class="sxs-lookup"><span data-stu-id="0b1d6-107">Software versions</span></span>
> 
> 
> - <span data-ttu-id="0b1d6-108">Веб-страниц ASP.NET (Razor) 3</span><span class="sxs-lookup"><span data-stu-id="0b1d6-108">ASP.NET Web Pages (Razor) 3</span></span>
>   
> 
> <span data-ttu-id="0b1d6-109">Этот учебник также работает с 2 веб-страниц ASP.NET и веб-страниц ASP.NET 1.0.</span><span class="sxs-lookup"><span data-stu-id="0b1d6-109">This tutorial also works with ASP.NET Web Pages 2 and ASP.NET Web Pages 1.0.</span></span>


<span data-ttu-id="0b1d6-110">В этом разделе содержатся следующие подразделы.</span><span class="sxs-lookup"><span data-stu-id="0b1d6-110">This topic contains the following sections:</span></span>

- [<span data-ttu-id="0b1d6-111">Проблемы с запуском страниц</span><span class="sxs-lookup"><span data-stu-id="0b1d6-111">Issues with Running Pages</span></span>](#Issues_Running_.cshtml_Pages)
- [<span data-ttu-id="0b1d6-112">Проблемы с кодом Razor</span><span class="sxs-lookup"><span data-stu-id="0b1d6-112">Issues with Razor Code</span></span>](#IssuesWithRazorCode)
- [<span data-ttu-id="0b1d6-113">Проблемы безопасности и членство</span><span class="sxs-lookup"><span data-stu-id="0b1d6-113">Issues with Security and Membership</span></span>](#membership)
- [<span data-ttu-id="0b1d6-114">Проблемы с отправкой электронной почты</span><span class="sxs-lookup"><span data-stu-id="0b1d6-114">Issues with Sending Email</span></span>](#email)
- [<span data-ttu-id="0b1d6-115">Дополнительные ресурсы</span><span class="sxs-lookup"><span data-stu-id="0b1d6-115">Additional Resources</span></span>](#AdditionalResources)

<span data-ttu-id="0b1d6-116">Общие вопросы см. в разделе [веб-страниц ASP.NET (Razor) часто задаваемые вопросы о](https://go.microsoft.com/fwlink/?LinkId=253000).</span><span class="sxs-lookup"><span data-stu-id="0b1d6-116">For general questions, see [ASP.NET Web Pages (Razor) FAQ](https://go.microsoft.com/fwlink/?LinkId=253000).</span></span>

<a id="Issues_Running_.cshtml_Pages"></a>
## <a name="issues-with-running-pages"></a><span data-ttu-id="0b1d6-117">Проблемы с запуском страниц</span><span class="sxs-lookup"><span data-stu-id="0b1d6-117">Issues with Running Pages</span></span>

<span data-ttu-id="0b1d6-118">Разнообразные проблемы можно избежать *.cshtml* и *.vbhtml* страницы из выполняется надлежащим образом.</span><span class="sxs-lookup"><span data-stu-id="0b1d6-118">A variety of issues can prevent *.cshtml* and *.vbhtml* pages from running properly.</span></span> <span data-ttu-id="0b1d6-119">В этом разделе перечислены распространенные сообщения об ошибках и возможные причины.</span><span class="sxs-lookup"><span data-stu-id="0b1d6-119">This section lists common error messages and likely causes.</span></span>

### <a name="http-error-403---forbidden-access-is-denied"></a><span data-ttu-id="0b1d6-120">Ошибка HTTP 403 — Запрещено: Доступ запрещен</span><span class="sxs-lookup"><span data-stu-id="0b1d6-120">HTTP Error 403 - Forbidden: Access is denied</span></span>

<span data-ttu-id="0b1d6-121">*У вас нет разрешения на просмотр этого каталога или страницы, используя учетные данные, которые были указаны.*</span><span class="sxs-lookup"><span data-stu-id="0b1d6-121">*You do not have permission to view this directory or page using the credentials that you supplied.*</span></span>

<span data-ttu-id="0b1d6-122">Эта ошибка может возникать, если сервер не работает под управлением правильную версию платформы .NET Framework.</span><span class="sxs-lookup"><span data-stu-id="0b1d6-122">This error can occur if the server is not running the correct version of the .NET Framework.</span></span> <span data-ttu-id="0b1d6-123">Убедитесь, что компьютер, на котором выполняется сервер (локально или удаленно) имеет по крайней мере .NET Framework 4 установлена.</span><span class="sxs-lookup"><span data-stu-id="0b1d6-123">Make sure that the computer that's running the server (locally or remotely) has at least the .NET Framework 4 installed.</span></span> <span data-ttu-id="0b1d6-124">Кроме того, убедитесь, что само приложение настраивается для запуска правильную версию.</span><span class="sxs-lookup"><span data-stu-id="0b1d6-124">Also make sure that the application itself is configured to run the right version.</span></span>

<span data-ttu-id="0b1d6-125">Если вы видите эту проблему локально при работе в WebMatrix, щелкните **сайта** рабочей области, а затем в представлении в виде дерева щелкните **параметры**.</span><span class="sxs-lookup"><span data-stu-id="0b1d6-125">If you see this problem locally while working in WebMatrix, click the **Site** workspace, and then in the treeview click **Settings**.</span></span> <span data-ttu-id="0b1d6-126">В **выберите версию .NET Framework** выберите **.NET 4 (интегрированная)**.</span><span class="sxs-lookup"><span data-stu-id="0b1d6-126">In the **Select .NET Framework Version** list, select **.NET 4 (Integrated)**.</span></span> <span data-ttu-id="0b1d6-127">Если эта версия уже установлено, попробуйте запустить WebMatrix с правами администратора.</span><span class="sxs-lookup"><span data-stu-id="0b1d6-127">If this version is already set, try running WebMatrix as an administrator.</span></span>

<span data-ttu-id="0b1d6-128">Убедитесь, что корневой веб-сайт имеет по крайней мере *.cshtml* файл в ней.</span><span class="sxs-lookup"><span data-stu-id="0b1d6-128">Make sure that the root of your website has at least one *.cshtml* file in it.</span></span>

<span data-ttu-id="0b1d6-129">Если вы видите эту ошибку при веб-сервера на удаленном сервере, обратитесь к администратору сервера.</span><span class="sxs-lookup"><span data-stu-id="0b1d6-129">If you see this error when the web server is on a remote server, contact the server administrator.</span></span> <span data-ttu-id="0b1d6-130">Убедитесь, что на сервере установлены платформы .NET Framework 4 или более поздней версии.</span><span class="sxs-lookup"><span data-stu-id="0b1d6-130">Make sure that the server has the .NET Framework 4 or later installed.</span></span> <span data-ttu-id="0b1d6-131">Кроме того, убедитесь, что приложение выполняется в пуле приложений, который настроен для использования этой версии платформы.NET Framework.</span><span class="sxs-lookup"><span data-stu-id="0b1d6-131">Also make sure that the application is running in an application pool that's configured to use that version of the.NET Framework.</span></span>

<span data-ttu-id="0b1d6-132">Если у вас есть контроль над сервером, убедитесь, что он работает правильную версию платформы .NET Framework.</span><span class="sxs-lookup"><span data-stu-id="0b1d6-132">If you have control over the server, make sure it's running the correct version of the .NET Framework.</span></span> <span data-ttu-id="0b1d6-133">Можно также попытаться исправить установку, запустив `aspnet_regiis -iru` команды.</span><span class="sxs-lookup"><span data-stu-id="0b1d6-133">You might also try repairing the installation by running the `aspnet_regiis -iru` command.</span></span> <span data-ttu-id="0b1d6-134">(Например, если устанавливаются службы IIS после установки .NET Framework, IIS будет не настроен правильно для запуска страницы ASP.NET.) Дополнительные сведения см. в разделе [средство регистрации IIS ASP.NET (Aspnet\_regiis.exe)](https://msdn.microsoft.com/library/k6h9cz8h(v=vs.100).aspx).</span><span class="sxs-lookup"><span data-stu-id="0b1d6-134">(For example, if you install IIS after you install the .NET Framework, IIS will not be correctly configured to run ASP.NET pages.) For more information, see [ASP.NET IIS Registration Tool (Aspnet\_regiis.exe)](https://msdn.microsoft.com/library/k6h9cz8h(v=vs.100).aspx).</span></span>

### <a name="http-error-40314---forbidden"></a><span data-ttu-id="0b1d6-135">Ошибка HTTP 403.14 - запрещено</span><span class="sxs-lookup"><span data-stu-id="0b1d6-135">HTTP Error 403.14 - Forbidden</span></span>

<span data-ttu-id="0b1d6-136">*Веб-сервер настроен не список содержимого каталога.*</span><span class="sxs-lookup"><span data-stu-id="0b1d6-136">*The Web server is configured to not list the contents of this directory.*</span></span>

<span data-ttu-id="0b1d6-137">Эта ошибка может возникать, если вы запрашиваете ресурс, который защищен (как *Web.config* файл) или расположенного в папке, которая защищена (как *приложения\_данных* или *приложения\_Код*).</span><span class="sxs-lookup"><span data-stu-id="0b1d6-137">This error can occur if you request a resource that's protected (like the *Web.config* file) or that's in a folder that's protected (like *App\_Data* or *App\_Code*).</span></span>

### <a name="http-error-40417---not-found"></a><span data-ttu-id="0b1d6-138">Ошибка HTTP 404.17 файл - не найдено</span><span class="sxs-lookup"><span data-stu-id="0b1d6-138">HTTP Error 404.17 - Not Found</span></span>

<span data-ttu-id="0b1d6-139">*Запрошенное содержимое отображается скрипт и не будет обслуживаться обработчик статических файлов.*</span><span class="sxs-lookup"><span data-stu-id="0b1d6-139">*The requested content appears to be script and will not be served by the static file handler.*</span></span>

<span data-ttu-id="0b1d6-140">Эта ошибка может возникать, если сервер не настроен для использования в .NET Framework 4 или более поздней версии и поэтому не распознает код в `@{ }` блоков.</span><span class="sxs-lookup"><span data-stu-id="0b1d6-140">This error can occur if the server is not configured correctly to use the .NET Framework 4 or later, and therefore does not recognize the code in `@{ }` blocks.</span></span> <span data-ttu-id="0b1d6-141">См. в описании ранее для *Ошибка HTTP 403 — Запрещено: Отказано в доступе*.</span><span class="sxs-lookup"><span data-stu-id="0b1d6-141">See the description earlier for *HTTP Error 403 - Forbidden: Access is denied*.</span></span>

### <a name="http-error-4047---not-found"></a><span data-ttu-id="0b1d6-142">Ошибка HTTP 404.7 - не найдено</span><span class="sxs-lookup"><span data-stu-id="0b1d6-142">HTTP Error 404.7 - Not Found</span></span>

<span data-ttu-id="0b1d6-143">*Модуль фильтрации запросов настроены на отклонение расширение файла*</span><span class="sxs-lookup"><span data-stu-id="0b1d6-143">*The request filtering module is configured to deny the file extension*</span></span>

<span data-ttu-id="0b1d6-144">Эта ошибка может возникать, если *.cshtml* или *.vbhtml* расширения заблокированы явным образом на сервере.</span><span class="sxs-lookup"><span data-stu-id="0b1d6-144">This error can occur if *.cshtml* or *.vbhtml* extensions have been explicitly blocked on the server.</span></span> <span data-ttu-id="0b1d6-145">Признаком этой проблемы является этой работы URL-адреса, когда они не включают расширение, но URL-адреса, которые включают *.cshtml* или *.vbhtml* не работают.</span><span class="sxs-lookup"><span data-stu-id="0b1d6-145">A symptom of this problem is that URLs work when they do not include the extension, but URLs that include *.cshtml* or *.vbhtml* do not work.</span></span> <span data-ttu-id="0b1d6-146">Возможное решение заключается в повторном активировании расширения на сайте *Web.config* файл.</span><span class="sxs-lookup"><span data-stu-id="0b1d6-146">A possible solution is to re-enable the extensions in the site's *Web.config* file.</span></span> <span data-ttu-id="0b1d6-147">В следующем примере показано, как включить *.cshtml* расширения.</span><span class="sxs-lookup"><span data-stu-id="0b1d6-147">The following example shows how to enable the *.cshtml* extension.</span></span>

[!code-xml[Main](aspnet-web-pages-razor-troubleshooting-guide/samples/sample1.xml?highlight=5-6)]

### <a name="http-error-4048---not-found"></a><span data-ttu-id="0b1d6-148">Ошибка HTTP 404.8 - не найдено</span><span class="sxs-lookup"><span data-stu-id="0b1d6-148">HTTP Error 404.8 - Not Found</span></span>

<span data-ttu-id="0b1d6-149">*Модуль фильтрации запросов настроены на отклонение пути в URL-адрес, содержащий раздел hiddenSegment.*</span><span class="sxs-lookup"><span data-stu-id="0b1d6-149">*The request filtering module is configured to deny a path in the URL that contains a hiddenSegment section.*</span></span>

<span data-ttu-id="0b1d6-150">Эта ошибка может возникать, если вы запрашиваете ресурс, который защищен (как *Web.config* файл) или расположенного в папке, которая защищена (как *приложения\_данных* или *приложения\_Код*).</span><span class="sxs-lookup"><span data-stu-id="0b1d6-150">This error can occur if you request a resource that's protected (like the *Web.config* file) or that's in a folder that's protected (like *App\_Data* or *App\_Code*).</span></span>

### <a name="this-type-of-page-is-not-served-server-error-in--application"></a><span data-ttu-id="0b1d6-151">Этот тип страницы не обслуживается (ошибка сервера в приложении «/»)</span><span class="sxs-lookup"><span data-stu-id="0b1d6-151">This type of page is not served (Server Error in '/' Application)</span></span>

<span data-ttu-id="0b1d6-152">См. в описании ранее для 404.17 файл ошибки HTTP.</span><span class="sxs-lookup"><span data-stu-id="0b1d6-152">See the description earlier for HTTP Error 404.17.</span></span>

<a id="IssuesWithRazorCode"></a>
## <a name="issues-with-razor-code"></a><span data-ttu-id="0b1d6-153">Проблемы с кодом Razor</span><span class="sxs-lookup"><span data-stu-id="0b1d6-153">Issues with Razor code</span></span>

### <a name="the-name-class-does-not-exist-in-the-current-context"></a><span data-ttu-id="0b1d6-154">Имя "*класс*" не существует в текущем контексте</span><span class="sxs-lookup"><span data-stu-id="0b1d6-154">The name '*class*' does not exist in the current context</span></span>

<span data-ttu-id="0b1d6-155">Часто причина, при появлении этой ошибки является `class` ссылки, вспомогательный объект, но вспомогательное приложение не устанавливается.</span><span class="sxs-lookup"><span data-stu-id="0b1d6-155">Often, a reason you see this error is that `class` references a helper, but the helper is not installed.</span></span> <span data-ttu-id="0b1d6-156">Например если вы попытаетесь использовать вспомогательный объект, но если вы еще не установили пакет из NuGet, вы увидите эту ошибку.</span><span class="sxs-lookup"><span data-stu-id="0b1d6-156">For example, if you try to use a helper, but if you haven't installed the package from NuGet, you'll see this error.</span></span> <span data-ttu-id="0b1d6-157">Используйте коллекцию в WebMatrix, чтобы найти и установить вспомогательное приложение.</span><span class="sxs-lookup"><span data-stu-id="0b1d6-157">Use the Gallery in WebMatrix to find and install the helper.</span></span>

<span data-ttu-id="0b1d6-158">Если вспомогательное приложение устанавливается, но страницы по-прежнему не распознает его, попробуйте добавить добавить `using` инструкции к коду.</span><span class="sxs-lookup"><span data-stu-id="0b1d6-158">If the helper is installed, but the page still doesn't recognize it, try adding add a `using` statement to the code.</span></span> <span data-ttu-id="0b1d6-159">В `using` оператор, справочник по языку пространство имен, которое включает в себя вспомогательное приложение.</span><span class="sxs-lookup"><span data-stu-id="0b1d6-159">In the `using` statement, reference the namespace that includes the helper.</span></span> <span data-ttu-id="0b1d6-160">Например, базовые вспомогательные средства, которые находятся в пакете вспомогательные веб-модули ASP.NET находятся в `System.Web.Helpers` пространства имен.</span><span class="sxs-lookup"><span data-stu-id="0b1d6-160">For example, the basic helpers that are in the ASP.NET Web Helpers package are in the `System.Web.Helpers` namespace.</span></span> <span data-ttu-id="0b1d6-161">В верхней части страницы, где вы хотите использовать вспомогательный метод добавьте следующую строку:</span><span class="sxs-lookup"><span data-stu-id="0b1d6-161">At the top of the page where you want to use the helper, add this line:</span></span>

`@using Microsoft.Web.Helpers;`

<a id="membership"></a>
## <a name="issues-with-security-and-membership"></a><span data-ttu-id="0b1d6-162">Проблемы безопасности и членство</span><span class="sxs-lookup"><span data-stu-id="0b1d6-162">Issues with Security and Membership</span></span>

<span data-ttu-id="0b1d6-163">При использовании системы встроенных средств безопасности (членства) в веб-страниц ASP.NET (Razor), могут возникнуть следующие проблемы.</span><span class="sxs-lookup"><span data-stu-id="0b1d6-163">If you are using the built-in security (membership) system in ASP.NET Web Pages (Razor), you might encounter the following issues.</span></span>

### <a name="to-call-this-method-the-membershipprovider-property-must-be-an-instance-of-extendedmembershipprovider"></a><span data-ttu-id="0b1d6-164">Чтобы вызвать этот метод, свойство «Membership.Provider» должен быть экземпляр «ExtendedMembershipProvider»</span><span class="sxs-lookup"><span data-stu-id="0b1d6-164">To call this method, the "Membership.Provider" property must be an instance of "ExtendedMembershipProvider"</span></span>

<span data-ttu-id="0b1d6-165">Эта ошибка может указывать, что не `AspNetSqlMembershipProvider` настроен класс.</span><span class="sxs-lookup"><span data-stu-id="0b1d6-165">This error can indicate that no `AspNetSqlMembershipProvider` class is configured.</span></span> <span data-ttu-id="0b1d6-166">(Признаком является, что сайт работает локально, но вызывает эту ошибку, при его публикации на сервере поставщика услуг размещения). Одно исправление этой проблемы является явным образом включить простое членство, добавив следующий узел *Web.config* файла:</span><span class="sxs-lookup"><span data-stu-id="0b1d6-166">(A symptom is that the site works fine locally but throws this error when you publish it to a hosting provider's server.) One fix for this problem is to explicitly enable simple membership by adding the following to the site's *Web.config* file:</span></span>

[!code-xml[Main](aspnet-web-pages-razor-troubleshooting-guide/samples/sample2.xml?highlight=6)]

<a id="email"></a>
## <a name="issues-with-sending-email"></a><span data-ttu-id="0b1d6-167">Проблемы с отправкой электронной почты</span><span class="sxs-lookup"><span data-stu-id="0b1d6-167">Issues with Sending Email</span></span>

<span data-ttu-id="0b1d6-168">Проблемы с отправкой электронной почты может оказаться сложной задачей для отладки.</span><span class="sxs-lookup"><span data-stu-id="0b1d6-168">Problems with sending email can be challenging to debug.</span></span> <span data-ttu-id="0b1d6-169">Первоначальная проблема может быть, что не удается подключиться к SMTP-сервера.</span><span class="sxs-lookup"><span data-stu-id="0b1d6-169">An initial problem can be that you can't connect to the SMTP server.</span></span> <span data-ttu-id="0b1d6-170">Если подключение установлено успешно, ASP.NET передает сообщение на SMTP-сервер.</span><span class="sxs-lookup"><span data-stu-id="0b1d6-170">If the connection is successful, ASP.NET hands the message off to the SMTP server.</span></span> <span data-ttu-id="0b1d6-171">Тем не менее могут возникнуть проблемы с само сообщение, которое препятствует отправкой SMTP-сервера.</span><span class="sxs-lookup"><span data-stu-id="0b1d6-171">However, there can be problems with the message itself that prevents the SMTP server from sending it.</span></span>

<span data-ttu-id="0b1d6-172">Если приложение не отправлять по электронной почте, сделайте следующее:</span><span class="sxs-lookup"><span data-stu-id="0b1d6-172">If your application does not successfully send email, try the following:</span></span>

- <span data-ttu-id="0b1d6-173">Имя SMTP-сервера часто выглядит примерно так `smtp.provider.com` или `smtp.provider.net`.</span><span class="sxs-lookup"><span data-stu-id="0b1d6-173">The SMTP server name is often something like `smtp.provider.com` or `smtp.provider.net`.</span></span> <span data-ttu-id="0b1d6-174">Однако если вы публикуете веб-узла поставщика услуг размещения, имя SMTP-сервера на этом этапе может быть `localhost`.</span><span class="sxs-lookup"><span data-stu-id="0b1d6-174">However, if you publish your site to a hosting provider, the SMTP server name at that point might be `localhost`.</span></span> <span data-ttu-id="0b1d6-175">Такая ситуация возникает, так как после того как вы опубликовали и ваш сайт работает на сервере поставщика, SMTP-сервер может быть локальным с точки зрения приложения.</span><span class="sxs-lookup"><span data-stu-id="0b1d6-175">This situation occurs because after you've published and your site is running on the provider's server, the SMTP server might be local from the perspective of your application.</span></span> <span data-ttu-id="0b1d6-176">Это изменение в имена серверов может означать, что необходимо изменить имя SMTP-сервера как часть процесс публикации.</span><span class="sxs-lookup"><span data-stu-id="0b1d6-176">This change in server names might mean that you have to change the SMTP server name as part of your publishing process.</span></span>
- <span data-ttu-id="0b1d6-177">Номер порта, обычно — 25.</span><span class="sxs-lookup"><span data-stu-id="0b1d6-177">The port number is usually 25.</span></span> <span data-ttu-id="0b1d6-178">Тем не менее некоторые поставщики требуют использовать порт 587 или некоторых других портов.</span><span class="sxs-lookup"><span data-stu-id="0b1d6-178">However, some providers require you to use port 587 or some other port.</span></span> <span data-ttu-id="0b1d6-179">Узнайте у владельца SMTP-сервера, номер порта они ожидают, что позволяет использовать.</span><span class="sxs-lookup"><span data-stu-id="0b1d6-179">Check with the owner of the SMTP server what port number they expect you to use.</span></span>
- <span data-ttu-id="0b1d6-180">Убедитесь, что используется правильные учетные данные.</span><span class="sxs-lookup"><span data-stu-id="0b1d6-180">Make sure that you use the right credentials.</span></span> <span data-ttu-id="0b1d6-181">Если вы опубликовали свой сайт у поставщика услуг размещения, используйте учетные данные, которые поставщик указал специально предназначены для электронной почты.</span><span class="sxs-lookup"><span data-stu-id="0b1d6-181">If you've published your site to a hosting provider, use the credentials that the provider has specifically indicated are for email.</span></span> <span data-ttu-id="0b1d6-182">Эти учетные данные, может отличаться от учетных данных, используемых для публикации.</span><span class="sxs-lookup"><span data-stu-id="0b1d6-182">These credentials might be different from the credentials you use to publish.</span></span>
- <span data-ttu-id="0b1d6-183">Иногда вообще не требуются учетные данные.</span><span class="sxs-lookup"><span data-stu-id="0b1d6-183">Sometimes you don't need credentials at all.</span></span> <span data-ttu-id="0b1d6-184">При отправке электронной почты с помощью ваш поставщик услуг Интернета, поставщика электронной почты могут уже знать свои учетные данные.</span><span class="sxs-lookup"><span data-stu-id="0b1d6-184">If you're sending email by using your personal ISP, your email provider might already know your credentials.</span></span> <span data-ttu-id="0b1d6-185">После публикации, может потребоваться использовать другие учетные данные, чем при тестировании на локальном компьютере.</span><span class="sxs-lookup"><span data-stu-id="0b1d6-185">After you publish, you might need to use different credentials than when you test on your local computer.</span></span>
- <span data-ttu-id="0b1d6-186">Если поставщика электронной почты использует шифрование, `WebMail.EnableSsl` для `true`.</span><span class="sxs-lookup"><span data-stu-id="0b1d6-186">If your email provider uses encryption, set `WebMail.EnableSsl` to `true`.</span></span>

<span data-ttu-id="0b1d6-187">Если ошибка при отправке электронной почты, может появиться стандартное сообщение Ошибка ASP.NET, который выглядит следующим образом:</span><span class="sxs-lookup"><span data-stu-id="0b1d6-187">If there is an error sending email, you might see a standard ASP.NET error message, which looks like this:</span></span>

![ASP.NET сообщение об ошибке при возникновении проблемы с электронной почты](aspnet-web-pages-razor-troubleshooting-guide/_static/image1.png)

<span data-ttu-id="0b1d6-189">Также можно отлаживать проблемах отправки электронной почты с помощью `try-catch` блока, как показано в следующем примере.</span><span class="sxs-lookup"><span data-stu-id="0b1d6-189">You can also debug problems with sending email by using a `try-catch` block, as in the following example.</span></span> <span data-ttu-id="0b1d6-190">При использовании `try-catch` блока, ASP.NET не отображает его стандартные сообщения об ошибках.</span><span class="sxs-lookup"><span data-stu-id="0b1d6-190">When you use a `try-catch` block, ASP.NET does not display its standard error messages.</span></span> <span data-ttu-id="0b1d6-191">Вместо этого можно записать ошибку в `catch` части блока.</span><span class="sxs-lookup"><span data-stu-id="0b1d6-191">Instead, you can capture the error in the `catch` portion of the block.</span></span>

[!code-cshtml[Main](aspnet-web-pages-razor-troubleshooting-guide/samples/sample3.cshtml)]

<span data-ttu-id="0b1d6-192">Подставьте соответствующие значения для `your-SMTP-server-name`, и т. д.</span><span class="sxs-lookup"><span data-stu-id="0b1d6-192">Substitute the appropriate values for `your-SMTP-server-name`, and so on.</span></span> <span data-ttu-id="0b1d6-193">Ниже приведены некоторые сообщения об ошибках, которые можно увидеть таким образом:</span><span class="sxs-lookup"><span data-stu-id="0b1d6-193">Some of the error messages you might see this way include the following:</span></span>

- <span data-ttu-id="0b1d6-194">*Сбой при отправке почты.*</span><span class="sxs-lookup"><span data-stu-id="0b1d6-194">*Failure sending mail.*</span></span>

    <span data-ttu-id="0b1d6-195">- или -</span><span class="sxs-lookup"><span data-stu-id="0b1d6-195">-or-</span></span>

    <span data-ttu-id="0b1d6-196">*Попытка подключения не удалось, так как подключенная сторона не ответила правильно после определенного периода времени, или установленное соединение, так как подключенный узел не отвечает*</span><span class="sxs-lookup"><span data-stu-id="0b1d6-196">*A connection attempt failed because the connected party did not properly respond after a period of time, or established connection failed because connected host has failed to respond*</span></span>

    <span data-ttu-id="0b1d6-197">Эта ошибка обычно означает, что приложение не удалось подключиться к SMTP-сервера.</span><span class="sxs-lookup"><span data-stu-id="0b1d6-197">This error usually means that the application could not connect to the SMTP server.</span></span> <span data-ttu-id="0b1d6-198">Проверьте имя сервера и номер порта.</span><span class="sxs-lookup"><span data-stu-id="0b1d6-198">Check the server name and port number.</span></span>
- <span data-ttu-id="0b1d6-199"><em>Почтовый ящик недоступен. Получен ответ сервера: 5.1.0 &lt; someuser@invaliddomain &gt; отправителя отклонен: недопустимый отправитель домена</em></span><span class="sxs-lookup"><span data-stu-id="0b1d6-199"><em>Mailbox unavailable. The server response was: 5.1.0 &lt;someuser@invaliddomain&gt; sender rejected : invalid sender domain</em></span></span>

    <span data-ttu-id="0b1d6-200">Это сообщение может указывать, что `From` адрес указано неправильно или отсутствует.</span><span class="sxs-lookup"><span data-stu-id="0b1d6-200">This message can indicate that the `From` address is not correct or is missing.</span></span>
- <span data-ttu-id="0b1d6-201">*Указанная строка не в форму, необходимую для адреса электронной почты.*</span><span class="sxs-lookup"><span data-stu-id="0b1d6-201">*The specified string is not in the form required for an email address.*</span></span>

    <span data-ttu-id="0b1d6-202">Эта ошибка может означать, что значение `To` или `From` свойства не распознаются как адреса электронной почты.</span><span class="sxs-lookup"><span data-stu-id="0b1d6-202">This error might indicate that the value of the `To` or `From` properties are not recognized as email addresses.</span></span> <span data-ttu-id="0b1d6-203">(ASP.NET не может проверить, что адрес электронной почты допустим, только что он, в правильном формате, например *name@domain.com*.)</span><span class="sxs-lookup"><span data-stu-id="0b1d6-203">(ASP.NET cannot check that the email address is valid, only that it's in the correct format, like *name@domain.com*.)</span></span>

> [!NOTE]
> <span data-ttu-id="0b1d6-204">Удалите разметку, с ошибкой (`@errorMessage`) перед публикацией страницы на действующем сайте.</span><span class="sxs-lookup"><span data-stu-id="0b1d6-204">Remove the markup that displays the error (`@errorMessage`) before you publish the page to a live site.</span></span> <span data-ttu-id="0b1d6-205">Не рекомендуется разрешать пользователям просматривать сообщения об ошибках, получаемых с сервера.</span><span class="sxs-lookup"><span data-stu-id="0b1d6-205">It's not a good idea to let users see error messages that you get from a server.</span></span>


<a id="AdditionalResources"></a>
## <a name="additional-resources"></a><span data-ttu-id="0b1d6-206">Дополнительные ресурсы</span><span class="sxs-lookup"><span data-stu-id="0b1d6-206">Additional Resources</span></span>

[<span data-ttu-id="0b1d6-207">Вопросы и ответы по веб-страницам ASP.NET (Razor)</span><span class="sxs-lookup"><span data-stu-id="0b1d6-207">ASP.NET Web Pages (Razor) FAQ</span></span>](https://go.microsoft.com/fwlink/?LinkId=253000)

<span data-ttu-id="0b1d6-208">[WebMatrix и ASP.NET Web Pages](https://forums.asp.net/1224.aspx/1?WebMatrix) форум на веб-сайта ASP.NET</span><span class="sxs-lookup"><span data-stu-id="0b1d6-208">[WebMatrix and ASP.NET Web Pages](https://forums.asp.net/1224.aspx/1?WebMatrix) forum on the ASP.NET website</span></span>
