---
uid: whitepapers/request-validation
title: Проверка - атак скрипт запросов | Документы Microsoft
author: rick-anderson
description: Этот документ описывает функцию проверки запроса ASP.NET, где, по умолчанию, приложение будет запрещено обработки без кодировки HTML-содержимого отправка...
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/10/2010
ms.topic: article
ms.assetid: fa429113-5f8f-4ef4-97c5-5c04900a19fa
ms.technology: ''
ms.prod: .net-framework
msc.legacyurl: /whitepapers/request-validation
msc.type: content
ms.openlocfilehash: 0b24fe2193d2c7a858667505bad9ed0b1d70a328
ms.sourcegitcommit: a510f38930abc84c4b302029d019a34dfe76823b
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/30/2018
ms.locfileid: "28883559"
---
<a name="request-validation---preventing-script-attacks"></a><span data-ttu-id="f0221-103">Проверка - атак скрипт запросов</span><span class="sxs-lookup"><span data-stu-id="f0221-103">Request Validation - Preventing Script Attacks</span></span>
====================
> <span data-ttu-id="f0221-104">Этот документ описывает функцию проверки запроса ASP.NET, где, по умолчанию приложение запрещена без кодировки HTML-содержимого на сервер обработки.</span><span class="sxs-lookup"><span data-stu-id="f0221-104">This paper describes the request validation feature of ASP.NET where, by default, the application is prevented from processing unencoded HTML content submitted to the server.</span></span> <span data-ttu-id="f0221-105">Если приложение разработано для безопасно обработать данные HTML можно отключить эту функцию проверки запроса.</span><span class="sxs-lookup"><span data-stu-id="f0221-105">This request validation feature can be disabled when the application has been designed to safely process HTML data.</span></span>
> 
> <span data-ttu-id="f0221-106">Применяется к ASP.NET версии 1.1 и ASP.NET 2.0.</span><span class="sxs-lookup"><span data-stu-id="f0221-106">Applies to ASP.NET 1.1 and ASP.NET 2.0.</span></span>


<span data-ttu-id="f0221-107">Проверку запросов, функция ASP.NET начиная с версии 1.1, не позволяет серверу принимать содержимого содержащего без кодировки HTML.</span><span class="sxs-lookup"><span data-stu-id="f0221-107">Request validation, a feature of ASP.NET since version 1.1, prevents the server from accepting content containing un-encoded HTML.</span></span> <span data-ttu-id="f0221-108">Эта функция предназначена для предотвращения некоторых атак путем внедрения скриптов, при котором код сценария клиента или HTML могут быть непреднамеренно отправлены на сервер, сохранения и затем представлен другим пользователям.</span><span class="sxs-lookup"><span data-stu-id="f0221-108">This feature is designed to help prevent some script-injection attacks whereby client script code or HTML can be unknowingly submitted to a server, stored, and then presented to other users.</span></span> <span data-ttu-id="f0221-109">По-прежнему настоятельно рекомендуется проверять все входные данные и HTML-кодирование его при необходимости.</span><span class="sxs-lookup"><span data-stu-id="f0221-109">We still strongly recommend that you validate all input data and HTML encode it when appropriate.</span></span>

<span data-ttu-id="f0221-110">Например при создании веб-страницы, запрашивающий адрес электронной почты пользователя, а затем магазинов, адрес электронной почты в базе данных.</span><span class="sxs-lookup"><span data-stu-id="f0221-110">For example, you create a Web page that requests a user's email address and then stores that email address in a database.</span></span> <span data-ttu-id="f0221-111">Если пользователь вводит &lt;СЦЕНАРИЙ&gt;предупреждение («hello из скрипта»)&lt;/SCRIPT&gt; вместо допустимый адрес электронной почты, при выводе данных, этот сценарий может выполняться Если содержимое не был закодирован правильно.</span><span class="sxs-lookup"><span data-stu-id="f0221-111">If the user enters &lt;SCRIPT&gt;alert("hello from script")&lt;/SCRIPT&gt; instead of a valid email address, when that data is presented, this script can be executed if the content was not properly encoded.</span></span> <span data-ttu-id="f0221-112">Функция проверки запросов ASP.NET предотвращает указанные последствия.</span><span class="sxs-lookup"><span data-stu-id="f0221-112">The request validation feature of ASP.NET prevents this from happening.</span></span>

## <a name="why-this-feature-is-useful"></a><span data-ttu-id="f0221-113">Почему эта возможность полезна</span><span class="sxs-lookup"><span data-stu-id="f0221-113">Why this feature is useful</span></span>

<span data-ttu-id="f0221-114">Большое количество сайтов не известно, что они открыты для атак путем внедрения кода простого скрипта.</span><span class="sxs-lookup"><span data-stu-id="f0221-114">Many sites are not aware that they are open to simple script injection attacks.</span></span> <span data-ttu-id="f0221-115">Ли такие атаки предназначен искажать сайта путем отображения HTML или потенциально выполняться клиентского скрипта перенаправлять пользователя на узел злоумышленника, атаки путем внедрения скриптов, проблема, веб-разработчикам приходится сталкиваться с.</span><span class="sxs-lookup"><span data-stu-id="f0221-115">Whether the purpose of these attacks is to deface the site by displaying HTML, or to potentially execute client script to redirect the user to a hacker's site, script injection attacks are a problem that Web developers must contend with.</span></span>

<span data-ttu-id="f0221-116">Атаки путем внедрения скриптов действительно угрожают всех веб-разработчиков, используется ли в них ASP.NET, ASP или другие технологии веб-разработки.</span><span class="sxs-lookup"><span data-stu-id="f0221-116">Script injection attacks are a concern of all web developers, whether they are using ASP.NET, ASP, or other web development technologies.</span></span>

<span data-ttu-id="f0221-117">Средство проверки запросов ASP.NET заранее предотвращает такие атаки, не позволяя без кодировки HTML-содержимого, обработанные сервером, если разработчик решает разрешить этого содержимого.</span><span class="sxs-lookup"><span data-stu-id="f0221-117">The ASP.NET request validation feature proactively prevents these attacks by not allowing unencoded HTML content to be processed by the server unless the developer decides to allow that content.</span></span>

## <a name="what-to-expect-error-page"></a><span data-ttu-id="f0221-118">Что произойдет: страница ошибки</span><span class="sxs-lookup"><span data-stu-id="f0221-118">What to expect: Error Page</span></span>

<span data-ttu-id="f0221-119">На приведенном ниже снимке экрана показан пример кода ASP.NET:</span><span class="sxs-lookup"><span data-stu-id="f0221-119">The screen shot below shows some sample ASP.NET code:</span></span>

![](request-validation/_static/image1.png)

<span data-ttu-id="f0221-120">Результаты этого кода выполняется простая страница, которая позволяет вводить текст в текстовом поле, нажмите кнопку и отображения текста в элементе управления label.</span><span class="sxs-lookup"><span data-stu-id="f0221-120">Running this code results in a simple page that allows you to enter some text in the textbox, click the button, and display the text in the label control:</span></span>

![](request-validation/_static/image2.png)

<span data-ttu-id="f0221-121">Однако были JavaScript, таких как `<script>alert("hello!")</script>` ввод и отправке получаются исключение:</span><span class="sxs-lookup"><span data-stu-id="f0221-121">However, were JavaScript, such as `<script>alert("hello!")</script>` to be entered and submitted we would get an exception:</span></span>

![](request-validation/_static/image3.png)

<span data-ttu-id="f0221-122">Сообщение об ошибке, указывающее, что «потенциально опасные Request.Form обнаружено значение» и предоставляет дополнительные сведения в описании относительно именно том, что произошло и как изменить поведение.</span><span class="sxs-lookup"><span data-stu-id="f0221-122">The error message states that a 'potentially dangerous Request.Form value was detected' and provides more details in the description as to exactly what occurred and how to change the behavior.</span></span> <span data-ttu-id="f0221-123">Пример:</span><span class="sxs-lookup"><span data-stu-id="f0221-123">For example:</span></span>

<span data-ttu-id="f0221-124">Проверка запросов обнаружила потенциально опасное входное значение клиента и обработка запроса прервана.</span><span class="sxs-lookup"><span data-stu-id="f0221-124">Request validation has detected a potentially dangerous client input value, and processing of the request has been aborted.</span></span> <span data-ttu-id="f0221-125">Это значение может указывать на попытку нарушения безопасности приложения, такие как атака межсайтовых скриптов.</span><span class="sxs-lookup"><span data-stu-id="f0221-125">This value may indicate an attempt to compromise the security of your application, such as a cross-site scripting attack.</span></span> <span data-ttu-id="f0221-126">Проверка запроса можно отключить, установив `validateRequest=false` в директиве страницы или в разделе конфигурации.</span><span class="sxs-lookup"><span data-stu-id="f0221-126">You can disable request validation by setting `validateRequest=false` in the Page directive or in the configuration section.</span></span> <span data-ttu-id="f0221-127">Что приложении явную проверку всех входных данных в этом случае настоятельно рекомендуется.</span><span class="sxs-lookup"><span data-stu-id="f0221-127">However, it is strongly recommended that your application explicitly check all inputs in this case.</span></span>

## <a name="disabling-request-validation-on-a-page"></a><span data-ttu-id="f0221-128">Отключение проверки запроса на странице</span><span class="sxs-lookup"><span data-stu-id="f0221-128">Disabling request validation on a page</span></span>

<span data-ttu-id="f0221-129">Чтобы отключить проверку запросов на странице, необходимо задать `validateRequest` атрибута директивы страницы для `false`:</span><span class="sxs-lookup"><span data-stu-id="f0221-129">To disable request validation on a page you must set the `validateRequest` attribute of the Page directive to `false`:</span></span>

[!code-aspx[Main](request-validation/samples/sample1.aspx)]

> [!CAUTION]
> <span data-ttu-id="f0221-130">При отключении проверки запроса содержимого можно передать на страницы; Это ответственность разработчика, убедитесь, что содержимое правильно кодируется или обработки.</span><span class="sxs-lookup"><span data-stu-id="f0221-130">When request validation is disabled, content can be submitted to a page; it is the responsibility of the page developer to ensure that content is properly encoded or processed.</span></span>

## <a name="disabling-request-validation-for-your-application"></a><span data-ttu-id="f0221-131">Отключение проверки запроса для приложения</span><span class="sxs-lookup"><span data-stu-id="f0221-131">Disabling request validation for your application</span></span>

<span data-ttu-id="f0221-132">Чтобы отключить проверку запросов для приложения, необходимо изменить или создать файл Web.config для приложения и установите для атрибута validateRequest `<pages />` раздел `false`:</span><span class="sxs-lookup"><span data-stu-id="f0221-132">To disable request validation for your application, you must modify or create a Web.config file for your application and set the validateRequest attribute of the `<pages />` section to `false`:</span></span>

[!code-xml[Main](request-validation/samples/sample2.xml)]

<span data-ttu-id="f0221-133">Если вы хотите отключить проверку запросов для всех приложений на сервере, можно сделать этого изменения в файл Machine.config.</span><span class="sxs-lookup"><span data-stu-id="f0221-133">If you wish to disable request validation for all applications on your server, you can make this modification to your Machine.config file.</span></span>

> [!CAUTION]
> <span data-ttu-id="f0221-134">При отключении проверки запроса содержимого можно передать в приложение; Это ответственность разработчика приложения, чтобы убедиться, что содержимое правильно кодируется или обработки.</span><span class="sxs-lookup"><span data-stu-id="f0221-134">When request validation is disabled, content can be submitted to your application; it is the responsibility of the application developer to ensure that content is properly encoded or processed.</span></span>

<span data-ttu-id="f0221-135">В следующем примере кода изменяется, чтобы отключить проверку запросов:</span><span class="sxs-lookup"><span data-stu-id="f0221-135">The code below is modified to turn off request validation:</span></span>

![](request-validation/_static/image4.png)

<span data-ttu-id="f0221-136">Теперь, если введено следующего кода JavaScript в текстовое поле `<script>alert("hello!")</script>` получим:</span><span class="sxs-lookup"><span data-stu-id="f0221-136">Now if the following JavaScript was entered into the textbox `<script>alert("hello!")</script>` the result would be:</span></span>

![](request-validation/_static/image5.png)

<span data-ttu-id="f0221-137">Чтобы предотвратить возникновение этой ситуации, при проверке запросов отключен, мы должны необходима кодировка HTML содержимое.</span><span class="sxs-lookup"><span data-stu-id="f0221-137">To prevent this from happening, with request validation turned off, we need to HTML encode the content.</span></span>

## <a name="how-to-html-encode-content"></a><span data-ttu-id="f0221-138">Каким образом в формате HTML кодирование содержимого</span><span class="sxs-lookup"><span data-stu-id="f0221-138">How to HTML encode content</span></span>

<span data-ttu-id="f0221-139">При отключении проверки запроса рекомендуется для HTML-кодирование содержимого, которое будет храниться для использования в будущем.</span><span class="sxs-lookup"><span data-stu-id="f0221-139">If you have disabled request validation, it is good practice to HTML-encode content that will be stored for future use.</span></span> <span data-ttu-id="f0221-140">Кодировка HTML автоматически заменяет любой "&lt;«или»&gt;" (а также несколько других символов) с кодом HTML, их соответствующие кодировке представление.</span><span class="sxs-lookup"><span data-stu-id="f0221-140">HTML encoding will automatically replace any ‘&lt;' or ‘&gt;' (together with several other symbols) with their corresponding HTML encoded representation.</span></span> <span data-ttu-id="f0221-141">Например "&lt;«заменяется на»&amp;lt;" и "&gt;«заменяется на»&amp;gt;".</span><span class="sxs-lookup"><span data-stu-id="f0221-141">For example, ‘&lt;' is replaced by ‘&amp;lt;' and ‘&gt;' is replaced by ‘&amp;gt;'.</span></span> <span data-ttu-id="f0221-142">Браузеры используют эти специальные коды для отображения "&lt;«или»&gt;" в браузере.</span><span class="sxs-lookup"><span data-stu-id="f0221-142">Browsers use these special codes to display the ‘&lt;' or ‘&gt;' in the browser.</span></span>

<span data-ttu-id="f0221-143">Содержимое может быть легко HTML-кодирование на сервере с помощью `Server.HtmlEncode(string)` API.</span><span class="sxs-lookup"><span data-stu-id="f0221-143">Content can be easily HTML-encoded on the server using the `Server.HtmlEncode(string)` API.</span></span> <span data-ttu-id="f0221-144">Содержимое также можно легко HTML-закодированном формате, то есть, возвращенными в стандартный HTML с помощью `Server.HtmlDecode(string)` метод.</span><span class="sxs-lookup"><span data-stu-id="f0221-144">Content can also be easily HTML-decoded, that is, reverted back to standard HTML using the `Server.HtmlDecode(string)` method.</span></span>

![](request-validation/_static/image6.png)

<span data-ttu-id="f0221-145">В результате.</span><span class="sxs-lookup"><span data-stu-id="f0221-145">Resulting in:</span></span>

![](request-validation/_static/image7.png)
