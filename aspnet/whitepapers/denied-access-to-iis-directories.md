---
uid: whitepapers/denied-access-to-iis-directories
title: ASP.NET отказано в доступе к каталогам IIS | Документация Майкрософт
author: rick-anderson
description: В этом документе описывается, что необходимо сделать, если запрос на приложение ASP.NET возвращает ошибку, «запрещен доступ к каталогу имя_каталога. Не удалось s...
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/10/2010
ms.topic: article
ms.assetid: 3cb27b8a-354f-4332-bfe0-232b13bbf8aa
ms.technology: ''
msc.legacyurl: /whitepapers/denied-access-to-iis-directories
msc.type: content
ms.openlocfilehash: 22868738e02472f5f433c729967ac324131a0fdc
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 07/03/2018
ms.locfileid: "37383066"
---
<a name="aspnet-denied-access-to-iis-directories"></a><span data-ttu-id="19e97-104">Отказано в доступе к каталогам IIS ASP.NET</span><span class="sxs-lookup"><span data-stu-id="19e97-104">ASP.NET Denied Access to IIS Directories</span></span>
====================
> <span data-ttu-id="19e97-105">В этом документе описывается, что необходимо сделать, если запрос в приложение ASP.NET возвращает ошибку «отказано в доступе к *имя_каталога* каталога.</span><span class="sxs-lookup"><span data-stu-id="19e97-105">This whitepaper describes what you must do if a request to your ASP.NET application returns the error, "Denied Access to *DirectoryName* directory.</span></span> <span data-ttu-id="19e97-106">Не удалось запустить отслеживание изменений папки.»</span><span class="sxs-lookup"><span data-stu-id="19e97-106">Failed to start monitoring directory changes."</span></span>
> 
> <span data-ttu-id="19e97-107">Применяется к ASP.NET 1.0 и ASP.NET 1.1.</span><span class="sxs-lookup"><span data-stu-id="19e97-107">Applies to ASP.NET 1.0 and ASP.NET 1.1.</span></span>


<span data-ttu-id="19e97-108">RTM ASP.NET версии 1 теперь выполняется с помощью меньше Привилегированная учетная запись windows - зарегистрирован как учетная запись «ASPNET» на локальном компьютере.</span><span class="sxs-lookup"><span data-stu-id="19e97-108">ASP.NET V1 RTM now runs using a less privileged windows account - registered as the "ASPNET" account on a local machine.</span></span>

<span data-ttu-id="19e97-109">На некоторых заблокированы систем Эта учетная запись может не по умолчанию иметь безопасности доступ на чтение каталогов содержимого веб сайта, корневой каталог приложения или корневой каталог веб-сайта.</span><span class="sxs-lookup"><span data-stu-id="19e97-109">On some locked down systems, this account may not by default have read security access to a website's content directories, the application root directory, or the web site root directory.</span></span> <span data-ttu-id="19e97-110">В этом случае вы получите следующую ошибку при запросе страницы из данного веб-приложения:</span><span class="sxs-lookup"><span data-stu-id="19e97-110">In this case you will receive the following error when requesting pages from a given web application:</span></span>

![](denied-access-to-iis-directories/_static/image1.jpg)

<span data-ttu-id="19e97-111">Чтобы устранить эту проблему, необходимо изменить разрешения безопасности на соответствующие каталоги.</span><span class="sxs-lookup"><span data-stu-id="19e97-111">To fix this you will need to change the security permissions on the appropriate directories.</span></span>

<span data-ttu-id="19e97-112">В частности, ASP.NET требует чтение, выполнение и Просмотр списка доступа для учетной записи ASPNET для корневого каталога веб-сайта (например: c:\inetpub\wwwroot или другом каталоге альтернативного сайта, настроенные в службах IIS), каталог содержимого и корневой каталог приложения Чтобы отслеживать изменения в файле конфигурации.</span><span class="sxs-lookup"><span data-stu-id="19e97-112">Specifically, ASP.NET requires read, execute, and list access for the ASPNET account for the web site root (for example: c:\inetpub\wwwroot or any alternative site directory you may have configured in IIS), the content directory and the application root directory in order to monitor for configuration file changes.</span></span> <span data-ttu-id="19e97-113">Путь к папке, связанной с виртуальным каталогом приложения в средстве администрирования IIS (inetmgr) соответствует корню приложения.</span><span class="sxs-lookup"><span data-stu-id="19e97-113">The application root corresponds to the folder path associated with the application virtual directory in the IIS Administration tool (inetmgr).</span></span>

<span data-ttu-id="19e97-114">Например рассмотрим следующую иерархию приложения в папке wwwroot.</span><span class="sxs-lookup"><span data-stu-id="19e97-114">For example, consider the following application hierarchy under the wwwroot folder.</span></span>

`C:\inetpub\wwwroot\myapp\default.aspx`

<span data-ttu-id="19e97-115">Например учетной записи ASPNET должен разрешения на чтение, определенный выше для содержимого в myapp и каталог wwwroot.</span><span class="sxs-lookup"><span data-stu-id="19e97-115">For this example, the ASPNET account needs the read permissions defined above for content in both the myapp and the wwwroot directory.</span></span> <span data-ttu-id="19e97-116">Единый унаследованные ACL в корневой папке можно также при необходимости использовать для обоих каталогов если они все вложенные.</span><span class="sxs-lookup"><span data-stu-id="19e97-116">A single inherited ACL on the root folder can also be optionally used for both directories if they're nested.</span></span>

<span data-ttu-id="19e97-117">Чтобы добавить разрешения в каталог, выполните следующие действия.</span><span class="sxs-lookup"><span data-stu-id="19e97-117">To add permissions to a directory, perform the following steps:</span></span>

- <span data-ttu-id="19e97-118">В проводнике Windows перейдите в каталог</span><span class="sxs-lookup"><span data-stu-id="19e97-118">Using the Windows explorer, navigate to the directory</span></span>
- <span data-ttu-id="19e97-119">Щелкните правой кнопкой папку каталога и выберите «Свойства»</span><span class="sxs-lookup"><span data-stu-id="19e97-119">Right click on the directory folder and choose "Properties"</span></span>
- <span data-ttu-id="19e97-120">Перейдите на вкладку «Безопасность» в диалоговом окне Свойства</span><span class="sxs-lookup"><span data-stu-id="19e97-120">Navigate to the "Security" tab on the property dialog</span></span>
- <span data-ttu-id="19e97-121">Нажмите кнопку «Добавить» и введите имя компьютера, за которым следует имя учетной записи ASPNET.</span><span class="sxs-lookup"><span data-stu-id="19e97-121">Click the "Add" button and enter the machine name followed by the ASPNET account name.</span></span> <span data-ttu-id="19e97-122">Например на компьютере с именем «webdev», будет введите webdev\ASPNET и нажмите «ОК».</span><span class="sxs-lookup"><span data-stu-id="19e97-122">For example, on a machine named "webdev", you would enter webdev\ASPNET and hit "OK".</span></span>
- <span data-ttu-id="19e97-123">Убедитесь в наличии учетной записи ASPNET «чтения &amp; Execute», «Список содержимого папки» и «Чтение» флажков.</span><span class="sxs-lookup"><span data-stu-id="19e97-123">Ensure that the ASPNET account has the "Read &amp; Execute", "List Folder Contents", and "Read" checkboxes checked.</span></span>
- <span data-ttu-id="19e97-124">Нажмите кнопку ОК, чтобы закрыть диалоговое окно и сохранить изменения.</span><span class="sxs-lookup"><span data-stu-id="19e97-124">Hit OK to dismiss the dialog and save the changes.</span></span>

![](denied-access-to-iis-directories/_static/image2.jpg)

<span data-ttu-id="19e97-125">При необходимости эти изменения можно автоматизировать с помощью скриптов или средство «cacls.exe», который поставляется с Windows.</span><span class="sxs-lookup"><span data-stu-id="19e97-125">If desired, these changes can be automated using scripts or the "cacls.exe" tool that ships with Windows.</span></span> <span data-ttu-id="19e97-126">Дополнительные сведения об учетной записи ASPNET, см. в разделе [документа часто задаваемые вопросы о](https://go.microsoft.com/fwlink/?LinkId=5828).</span><span class="sxs-lookup"><span data-stu-id="19e97-126">For more information on the ASPNET account, please see the [FAQ document](https://go.microsoft.com/fwlink/?LinkId=5828).</span></span>

<span data-ttu-id="19e97-127">Если данного веб-приложения зависит от того, записывать или изменять разрешения для конкретной папки или файла, это могут быть предоставлены ту же процедуру и проверив флажки «Write» или «Изменить».</span><span class="sxs-lookup"><span data-stu-id="19e97-127">If a given web application relies on having write or modify permissions to a particular folder or file, this can be granted by following the same procedure and checking the "Write" and/or "Modify" checkboxes.</span></span>

<span data-ttu-id="19e97-128">На компьютерах, которые позволяют всем пользователям или пользователям группы доступ на чтение к этим каталогам (что является конфигурацией по умолчанию), будут возникать проблемы отсутствуют, и описанные выше действия не потребуются.</span><span class="sxs-lookup"><span data-stu-id="19e97-128">On machines that allow Everyone or the Users group read access to these directories (which is the default configuration), no issues will be encountered and the above steps will not be required.</span></span>
