---
uid: whitepapers/request-validation
title: Проверка - предотвращение атак сценариев запросов | Документация Майкрософт
author: rick-anderson
description: В этом документе описывается функция проверки запросов ASP.NET, где, по умолчанию приложение не обработки без кодировки HTML-содержимого отправка...
ms.author: aspnetcontent
ms.date: 02/10/2010
ms.assetid: fa429113-5f8f-4ef4-97c5-5c04900a19fa
msc.legacyurl: /whitepapers/request-validation
msc.type: content
ms.openlocfilehash: 0dfbfcae70792c57d530fc5e6fb73f8f96ec6e02
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 07/05/2018
ms.locfileid: "37809749"
---
<a name="request-validation---preventing-script-attacks"></a><span data-ttu-id="9f454-103">Проверка - предотвращение атак сценариев запросов</span><span class="sxs-lookup"><span data-stu-id="9f454-103">Request Validation - Preventing Script Attacks</span></span>
====================
> <span data-ttu-id="9f454-104">В этом документе описывается функция проверки запросов ASP.NET, где, по умолчанию приложение запрещено обработки без кодировки HTML-содержимое, отправки на сервер.</span><span class="sxs-lookup"><span data-stu-id="9f454-104">This paper describes the request validation feature of ASP.NET where, by default, the application is prevented from processing unencoded HTML content submitted to the server.</span></span> <span data-ttu-id="9f454-105">Эту функцию проверки запроса можно отключить, когда приложение разработано для безопасно обрабатывать данные HTML.</span><span class="sxs-lookup"><span data-stu-id="9f454-105">This request validation feature can be disabled when the application has been designed to safely process HTML data.</span></span>
> 
> <span data-ttu-id="9f454-106">Применяется к ASP.NET 1.1 и ASP.NET 2.0.</span><span class="sxs-lookup"><span data-stu-id="9f454-106">Applies to ASP.NET 1.1 and ASP.NET 2.0.</span></span>


<span data-ttu-id="9f454-107">Проверка запросов — это функция ASP.NET, начиная с версии 1.1, в результате сервер перестает принимать содержимого содержащего незашифрованным HTML-кодом.</span><span class="sxs-lookup"><span data-stu-id="9f454-107">Request validation, a feature of ASP.NET since version 1.1, prevents the server from accepting content containing un-encoded HTML.</span></span> <span data-ttu-id="9f454-108">Эта функция предназначена для предотвращения некоторые атаки путем внедрения скрипта, при котором код скрипта клиента или HTML могут оправлять к серверу, хранить и затем представлен другим пользователям.</span><span class="sxs-lookup"><span data-stu-id="9f454-108">This feature is designed to help prevent some script-injection attacks whereby client script code or HTML can be unknowingly submitted to a server, stored, and then presented to other users.</span></span> <span data-ttu-id="9f454-109">По-прежнему настоятельно рекомендуется проверять все входные данные и кодировать в HTML ее при необходимости.</span><span class="sxs-lookup"><span data-stu-id="9f454-109">We still strongly recommend that you validate all input data and HTML encode it when appropriate.</span></span>

<span data-ttu-id="9f454-110">Например можно создать веб-страницы, который запрашивает адрес электронной почты пользователя, а затем хранилищ, адрес электронной почты в базе данных.</span><span class="sxs-lookup"><span data-stu-id="9f454-110">For example, you create a Web page that requests a user's email address and then stores that email address in a database.</span></span> <span data-ttu-id="9f454-111">Если пользователь введет &lt;СКРИПТ&gt;предупреждение ("hello из скрипта")&lt;/SCRIPT&gt; вместо допустимый адрес электронной почты, при появлении этих данных, этот сценарий может выполняться Если содержимое не зашифровано должным образом.</span><span class="sxs-lookup"><span data-stu-id="9f454-111">If the user enters &lt;SCRIPT&gt;alert("hello from script")&lt;/SCRIPT&gt; instead of a valid email address, when that data is presented, this script can be executed if the content was not properly encoded.</span></span> <span data-ttu-id="9f454-112">Функция проверки запросов ASP.NET предотвращает указанные последствия.</span><span class="sxs-lookup"><span data-stu-id="9f454-112">The request validation feature of ASP.NET prevents this from happening.</span></span>

## <a name="why-this-feature-is-useful"></a><span data-ttu-id="9f454-113">Почему эта возможность оказывается полезной</span><span class="sxs-lookup"><span data-stu-id="9f454-113">Why this feature is useful</span></span>

<span data-ttu-id="9f454-114">Множество узлов, не знают, что они открыты для атаки путем внедрения кода простого сценария.</span><span class="sxs-lookup"><span data-stu-id="9f454-114">Many sites are not aware that they are open to simple script injection attacks.</span></span> <span data-ttu-id="9f454-115">Атаки путем внедрения скриптов, проблема, веб-разработчикам приходится сталкиваться с, ли искажать сайта путем отображения HTML или потенциально выполнить скрипт клиента для перенаправления пользователя на узел злоумышленника — цель таких атак.</span><span class="sxs-lookup"><span data-stu-id="9f454-115">Whether the purpose of these attacks is to deface the site by displaying HTML, or to potentially execute client script to redirect the user to a hacker's site, script injection attacks are a problem that Web developers must contend with.</span></span>

<span data-ttu-id="9f454-116">Атаки путем внедрения скриптов заботят все веб-разработчиков, ли они с помощью ASP.NET, ASP и других веб-технологий разработки.</span><span class="sxs-lookup"><span data-stu-id="9f454-116">Script injection attacks are a concern of all web developers, whether they are using ASP.NET, ASP, or other web development technologies.</span></span>

<span data-ttu-id="9f454-117">Функция проверки запросов ASP.NET заранее предотвращает такие атаки, не позволяя без кодировки HTML-содержимого, обрабатываются сервером, если разработчик решает разрешить этого содержимого.</span><span class="sxs-lookup"><span data-stu-id="9f454-117">The ASP.NET request validation feature proactively prevents these attacks by not allowing unencoded HTML content to be processed by the server unless the developer decides to allow that content.</span></span>

## <a name="what-to-expect-error-page"></a><span data-ttu-id="9f454-118">Что произойдет: страница ошибки</span><span class="sxs-lookup"><span data-stu-id="9f454-118">What to expect: Error Page</span></span>

<span data-ttu-id="9f454-119">На рисунке ниже показан пример кода ASP.NET:</span><span class="sxs-lookup"><span data-stu-id="9f454-119">The screen shot below shows some sample ASP.NET code:</span></span>

![](request-validation/_static/image1.png)

<span data-ttu-id="9f454-120">Выполнения этого кода приводит простая страница, которая позволяет ввести некоторый текст в текстовом поле, нажмите кнопку и отображения текста в элементе управления label.</span><span class="sxs-lookup"><span data-stu-id="9f454-120">Running this code results in a simple page that allows you to enter some text in the textbox, click the button, and display the text in the label control:</span></span>

![](request-validation/_static/image2.png)

<span data-ttu-id="9f454-121">Тем не менее, были JavaScript, таких как `<script>alert("hello!")</script>` и отправки, то получили бы исключение:</span><span class="sxs-lookup"><span data-stu-id="9f454-121">However, were JavaScript, such as `<script>alert("hello!")</script>` to be entered and submitted we would get an exception:</span></span>

![](request-validation/_static/image3.png)

<span data-ttu-id="9f454-122">Сообщение об ошибке в том, что «потенциально опасных Request.Form обнаружено значение» и Дополнительные сведения в описании для только что произошло и как изменить поведение.</span><span class="sxs-lookup"><span data-stu-id="9f454-122">The error message states that a 'potentially dangerous Request.Form value was detected' and provides more details in the description as to exactly what occurred and how to change the behavior.</span></span> <span data-ttu-id="9f454-123">Пример:</span><span class="sxs-lookup"><span data-stu-id="9f454-123">For example:</span></span>

<span data-ttu-id="9f454-124">Проверка запросов обнаружила потенциально опасных входное значение клиента, и обработка запроса прервана.</span><span class="sxs-lookup"><span data-stu-id="9f454-124">Request validation has detected a potentially dangerous client input value, and processing of the request has been aborted.</span></span> <span data-ttu-id="9f454-125">Это значение может указывать на попытку нарушения безопасности приложения, такие как атаки межузловых сценариев.</span><span class="sxs-lookup"><span data-stu-id="9f454-125">This value may indicate an attempt to compromise the security of your application, such as a cross-site scripting attack.</span></span> <span data-ttu-id="9f454-126">Проверка запросов можно отключить, установив `validateRequest=false` в директиве Page или в разделе конфигурации.</span><span class="sxs-lookup"><span data-stu-id="9f454-126">You can disable request validation by setting `validateRequest=false` in the Page directive or in the configuration section.</span></span> <span data-ttu-id="9f454-127">Что в приложении явную проверку всех входных данных в этом случае настоятельно рекомендуется.</span><span class="sxs-lookup"><span data-stu-id="9f454-127">However, it is strongly recommended that your application explicitly check all inputs in this case.</span></span>

## <a name="disabling-request-validation-on-a-page"></a><span data-ttu-id="9f454-128">Отключение проверки запросов на странице</span><span class="sxs-lookup"><span data-stu-id="9f454-128">Disabling request validation on a page</span></span>

<span data-ttu-id="9f454-129">Чтобы отключить проверку запроса на странице, необходимо задать `validateRequest` атрибутов директивы Page для `false`:</span><span class="sxs-lookup"><span data-stu-id="9f454-129">To disable request validation on a page you must set the `validateRequest` attribute of the Page directive to `false`:</span></span>

[!code-aspx[Main](request-validation/samples/sample1.aspx)]

> [!CAUTION]
> <span data-ttu-id="9f454-130">При отключении проверки запросов содержимого могут отправляться на страницу; Это ответственность разработчика, убедитесь, что содержимое в кодировке или обработанных должным образом.</span><span class="sxs-lookup"><span data-stu-id="9f454-130">When request validation is disabled, content can be submitted to a page; it is the responsibility of the page developer to ensure that content is properly encoded or processed.</span></span>

## <a name="disabling-request-validation-for-your-application"></a><span data-ttu-id="9f454-131">Отключение проверки запросов для вашего приложения</span><span class="sxs-lookup"><span data-stu-id="9f454-131">Disabling request validation for your application</span></span>

<span data-ttu-id="9f454-132">Чтобы отключить проверку запросов для приложения, необходимо изменить или создать файл Web.config для приложения и установите атрибут validateRequest `<pages />` раздел `false`:</span><span class="sxs-lookup"><span data-stu-id="9f454-132">To disable request validation for your application, you must modify or create a Web.config file for your application and set the validateRequest attribute of the `<pages />` section to `false`:</span></span>

[!code-xml[Main](request-validation/samples/sample2.xml)]

<span data-ttu-id="9f454-133">Если вы хотите отключить проверку запросов для всех приложений на сервере, необходимо сделать это изменение в файл Machine.config.</span><span class="sxs-lookup"><span data-stu-id="9f454-133">If you wish to disable request validation for all applications on your server, you can make this modification to your Machine.config file.</span></span>

> [!CAUTION]
> <span data-ttu-id="9f454-134">При отключении проверки запросов содержимого могут отправляться в приложение; Это ответственность разработчика приложения, убедитесь, что содержимое в кодировке или обработанных должным образом.</span><span class="sxs-lookup"><span data-stu-id="9f454-134">When request validation is disabled, content can be submitted to your application; it is the responsibility of the application developer to ensure that content is properly encoded or processed.</span></span>

<span data-ttu-id="9f454-135">В приведенном ниже коде изменяется, чтобы отключить проверку запроса:</span><span class="sxs-lookup"><span data-stu-id="9f454-135">The code below is modified to turn off request validation:</span></span>

![](request-validation/_static/image4.png)

<span data-ttu-id="9f454-136">Теперь, если следующий код JavaScript, введенный в текстовое поле `<script>alert("hello!")</script>` результат будет иметь:</span><span class="sxs-lookup"><span data-stu-id="9f454-136">Now if the following JavaScript was entered into the textbox `<script>alert("hello!")</script>` the result would be:</span></span>

![](request-validation/_static/image5.png)

<span data-ttu-id="9f454-137">Чтобы предотвратить такую ситуацию, при проверке запросов отключен, мы должны необходима кодировка HTML содержимое.</span><span class="sxs-lookup"><span data-stu-id="9f454-137">To prevent this from happening, with request validation turned off, we need to HTML encode the content.</span></span>

## <a name="how-to-html-encode-content"></a><span data-ttu-id="9f454-138">Как в виде HTML кодирования содержимого</span><span class="sxs-lookup"><span data-stu-id="9f454-138">How to HTML encode content</span></span>

<span data-ttu-id="9f454-139">Если вы отключили проверку запросов, рекомендуется кодировать в HTML содержимое, которое будет храниться для использования в будущем.</span><span class="sxs-lookup"><span data-stu-id="9f454-139">If you have disabled request validation, it is good practice to HTML-encode content that will be stored for future use.</span></span> <span data-ttu-id="9f454-140">HTML-кодирования автоматически заменяют любые "&lt;«или»&gt;" (а также несколько других символов) с их соответствующий HTML-код представление кодировке.</span><span class="sxs-lookup"><span data-stu-id="9f454-140">HTML encoding will automatically replace any ‘&lt;' or ‘&gt;' (together with several other symbols) with their corresponding HTML encoded representation.</span></span> <span data-ttu-id="9f454-141">Например "&lt;«заменяется на»&amp;lt;" и "&gt;«заменяется на»&amp;gt;".</span><span class="sxs-lookup"><span data-stu-id="9f454-141">For example, ‘&lt;' is replaced by ‘&amp;lt;' and ‘&gt;' is replaced by ‘&amp;gt;'.</span></span> <span data-ttu-id="9f454-142">Браузеры используют эти специальные коды для отображения "&lt;«или»&gt;" в браузере.</span><span class="sxs-lookup"><span data-stu-id="9f454-142">Browsers use these special codes to display the ‘&lt;' or ‘&gt;' in the browser.</span></span>

<span data-ttu-id="9f454-143">Содержимое можно без труда HTML-кодирование на сервере, используя `Server.HtmlEncode(string)` API.</span><span class="sxs-lookup"><span data-stu-id="9f454-143">Content can be easily HTML-encoded on the server using the `Server.HtmlEncode(string)` API.</span></span> <span data-ttu-id="9f454-144">Содержимое также может быть легко HTML-декодирования, то есть, возвращенными в стандартный HTML с помощью `Server.HtmlDecode(string)` метод.</span><span class="sxs-lookup"><span data-stu-id="9f454-144">Content can also be easily HTML-decoded, that is, reverted back to standard HTML using the `Server.HtmlDecode(string)` method.</span></span>

![](request-validation/_static/image6.png)

<span data-ttu-id="9f454-145">В результате чего:</span><span class="sxs-lookup"><span data-stu-id="9f454-145">Resulting in:</span></span>

![](request-validation/_static/image7.png)
