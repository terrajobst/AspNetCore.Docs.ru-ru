---
uid: mvc/overview/getting-started/database-first-development/publish-to-azure
title: Опубликовать MVC базы данных первого сайта в Azure | Документы Microsoft
author: tfitzmac
description: С помощью MVC, Entity Framework и формирование шаблонов ASP.NET, можно создать веб-приложения, который предоставляет интерфейс для существующей базы данных. Этот учебник seri...
ms.author: aspnetcontent
manager: wpickett
ms.date: 12/22/2014
ms.topic: article
ms.assetid: 7131f1c1-cef3-4396-ab44-ed4519676546
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/getting-started/database-first-development/publish-to-azure
msc.type: authoredcontent
ms.openlocfilehash: 839bbceba6f0e098303facd40dbb1496bd449ba3
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/06/2018
ms.locfileid: "30869963"
---
<a name="publish-mvc-database-first-site-to-azure"></a><span data-ttu-id="caa85-104">Опубликовать MVC базы данных первого сайта в Azure</span><span class="sxs-lookup"><span data-stu-id="caa85-104">Publish MVC Database First site to Azure</span></span>
====================
<span data-ttu-id="caa85-105">по [Tom FitzMacken](https://github.com/tfitzmac)</span><span class="sxs-lookup"><span data-stu-id="caa85-105">by [Tom FitzMacken](https://github.com/tfitzmac)</span></span>

> <span data-ttu-id="caa85-106">С помощью MVC, Entity Framework и формирование шаблонов ASP.NET, можно создать веб-приложения, который предоставляет интерфейс для существующей базы данных.</span><span class="sxs-lookup"><span data-stu-id="caa85-106">Using MVC, Entity Framework, and ASP.NET Scaffolding, you can create a web application that provides an interface to an existing database.</span></span> <span data-ttu-id="caa85-107">Этот ряд учебника показано, как автоматически создают код, который дает пользователям возможность отображения, изменения, создания и удаления данных, которые хранятся в таблице базы данных.</span><span class="sxs-lookup"><span data-stu-id="caa85-107">This tutorial series shows you how to automatically generate code that enables users to display, edit, create, and delete data that resides in a database table.</span></span> <span data-ttu-id="caa85-108">Созданный код соответствует столбцам в таблице базы данных.</span><span class="sxs-lookup"><span data-stu-id="caa85-108">The generated code corresponds to the columns in the database table.</span></span>
> 
> <span data-ttu-id="caa85-109">Эта часть ряда фокусируется на публикации веб-приложения и базы данных в Azure.</span><span class="sxs-lookup"><span data-stu-id="caa85-109">This part of the series focuses on publishing the web app and database to Azure.</span></span> <span data-ttu-id="caa85-110">Можно прочитать этот раздел, чтобы узнать о публикации веб-приложения и базы данных, но для фактического выполнения шагов, которые должно начинаться в начале этого учебника.</span><span class="sxs-lookup"><span data-stu-id="caa85-110">You can read this topic to learn about publishing a web app and database, but to actually perform the steps you must start at the beginning of the tutorial.</span></span> <span data-ttu-id="caa85-111">В разделе [Приступая к работе](setting-up-database.md).</span><span class="sxs-lookup"><span data-stu-id="caa85-111">See [Getting Started](setting-up-database.md).</span></span>


## <a name="deploy-your-web-app-on-azure"></a><span data-ttu-id="caa85-112">Развертывание веб-приложения в Azure</span><span class="sxs-lookup"><span data-stu-id="caa85-112">Deploy your web app on Azure</span></span>

<span data-ttu-id="caa85-113">Требуется учетная запись Azure для выполнения заданий данного учебника:</span><span class="sxs-lookup"><span data-stu-id="caa85-113">You need an Azure account to complete this tutorial:</span></span>

- <span data-ttu-id="caa85-114">Вы можете [открыть учетную запись Azure бесплатно](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A261C142F) -вы получаете кредиты можно использовать, чтобы испытать оплаты служб Azure и даже после того, они используются до можно хранить учетной записи, и используйте освободить служб Azure.</span><span class="sxs-lookup"><span data-stu-id="caa85-114">You can [open an Azure account for free](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A261C142F) - You get credits you can use to try out paid Azure services, and even after they're used up you can keep the account and use free Azure services.</span></span>
- <span data-ttu-id="caa85-115">Вы можете [активировать преимущества для подписчиков MSDN](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/?WT.mc_id=A261C142F) -подписка Your MSDN дает кредиты каждого месяца, можно использовать для оплаты служб Azure.</span><span class="sxs-lookup"><span data-stu-id="caa85-115">You can [activate MSDN subscriber benefits](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/?WT.mc_id=A261C142F) - Your MSDN subscription gives you credits every month that you can use for paid Azure services.</span></span>

<span data-ttu-id="caa85-116">Чтобы опубликовать веб-приложения, щелкните правой кнопкой мыши проект и выберите **публикации**.</span><span class="sxs-lookup"><span data-stu-id="caa85-116">To publish your web app, right-click the project and select **Publish**.</span></span>

![опубликовать сайт](publish-to-azure/_static/image1.png)

<span data-ttu-id="caa85-118">Выберите веб-сайтов Microsoft Azure.</span><span class="sxs-lookup"><span data-stu-id="caa85-118">Select Microsoft Azure Websites.</span></span>

![Выберите Azure](publish-to-azure/_static/image2.png)

<span data-ttu-id="caa85-120">Если вы не вошли Azure, введите учетные данные учетной записи Azure.</span><span class="sxs-lookup"><span data-stu-id="caa85-120">If you are not signed in to Azure, provide your Azure account credentials.</span></span> <span data-ttu-id="caa85-121">Выберите создать, чтобы создать новое веб-приложение.</span><span class="sxs-lookup"><span data-stu-id="caa85-121">Then, select New to create a new web app.</span></span>

![новый сайт](publish-to-azure/_static/image3.png)

<span data-ttu-id="caa85-123">Создайте уникальное имя для веб-приложения.</span><span class="sxs-lookup"><span data-stu-id="caa85-123">Create a unique name for your web app.</span></span> <span data-ttu-id="caa85-124">Вы будете знать, что имя уникально Если отображается зеленая галочка справа от поля «имя».</span><span class="sxs-lookup"><span data-stu-id="caa85-124">You will know the name is unique if you see a green check mark to the right of the name field.</span></span> <span data-ttu-id="caa85-125">Выберите регион для веб-приложения.</span><span class="sxs-lookup"><span data-stu-id="caa85-125">Select a region for your web app.</span></span> <span data-ttu-id="caa85-126">Выберите **создать новый сервер** для базы данных и укажите имя пользователя и пароль для этого нового сервера базы данных.</span><span class="sxs-lookup"><span data-stu-id="caa85-126">Select **Create new server** for the database, and provide a user name and password for this new database server.</span></span> <span data-ttu-id="caa85-127">По завершении нажмите кнопку **создать**.</span><span class="sxs-lookup"><span data-stu-id="caa85-127">When finished, click **Create**.</span></span>

![Создание сайта](publish-to-azure/_static/image4.png)

<span data-ttu-id="caa85-129">Значения подключения теперь все настроены.</span><span class="sxs-lookup"><span data-stu-id="caa85-129">Your connection values are now all set.</span></span> <span data-ttu-id="caa85-130">Можно оставить их без изменений.</span><span class="sxs-lookup"><span data-stu-id="caa85-130">You can leave these values unchanged.</span></span>

![connection](publish-to-azure/_static/image5.png)

<span data-ttu-id="caa85-132">Нажмите кнопку **Далее**.</span><span class="sxs-lookup"><span data-stu-id="caa85-132">Click **Next**.</span></span>

<span data-ttu-id="caa85-133">Обратите внимание, что два подключений к базе данных указаны для параметров - ApplicationDBContext и ContosoUniversityDataEntities.</span><span class="sxs-lookup"><span data-stu-id="caa85-133">For Settings, notice that two database connections are specified - ApplicationDBContext and ContosoUniversityDataEntities.</span></span> <span data-ttu-id="caa85-134">ApplicationDBContext является соединение таблиц учетной записи пользователя.</span><span class="sxs-lookup"><span data-stu-id="caa85-134">ApplicationDBContext is the connection for user account tables.</span></span> <span data-ttu-id="caa85-135">Эти значения отображаются только строки подключения для базы данных.</span><span class="sxs-lookup"><span data-stu-id="caa85-135">These values only show the connection strings for the databases.</span></span> <span data-ttu-id="caa85-136">Это означает, что эти базы данных будет опубликован, при публикации веб-узла.</span><span class="sxs-lookup"><span data-stu-id="caa85-136">It does not mean that these databases will be published when you publish your site.</span></span> <span data-ttu-id="caa85-137">После завершения публикации веб-приложения вы публикуете проект базы данных.</span><span class="sxs-lookup"><span data-stu-id="caa85-137">You will publish your database project after you have finished publishing the web app.</span></span>

![](publish-to-azure/_static/image6.png)

<span data-ttu-id="caa85-138">Многоточие (...) рядом с подключения к базе данных показаны сведения о строке подключения.</span><span class="sxs-lookup"><span data-stu-id="caa85-138">The ellipsis (...) next to the database connection shows you the details of the connection string.</span></span> <span data-ttu-id="caa85-139">Нажмите кнопку с многоточием рядом с ContosoUniversityDataEntities.</span><span class="sxs-lookup"><span data-stu-id="caa85-139">Click the ellipsis next to ContosoUniversityDataEntities.</span></span>

![](publish-to-azure/_static/image7.png)

<span data-ttu-id="caa85-140">Обратите внимание на имя сервера базы данных и базы данных.</span><span class="sxs-lookup"><span data-stu-id="caa85-140">Note the name of the database server and the database.</span></span> <span data-ttu-id="caa85-141">Имя сервера формируется случайным образом.</span><span class="sxs-lookup"><span data-stu-id="caa85-141">The server name is randomly generated.</span></span> <span data-ttu-id="caa85-142">Имя базы данных — простое имя узла с  **\_db** добавляется.</span><span class="sxs-lookup"><span data-stu-id="caa85-142">The database name is simple the name of your site with **\_db** appended.</span></span> <span data-ttu-id="caa85-143">Понадобится имена обоих позже при публикации базы данных.</span><span class="sxs-lookup"><span data-stu-id="caa85-143">You will need both names later when you publish your database.</span></span>

<span data-ttu-id="caa85-144">Нажмите кнопку **ОК** закрыть окно строку подключения базы данных.</span><span class="sxs-lookup"><span data-stu-id="caa85-144">Click **OK** to close the database connection string window.</span></span>

<span data-ttu-id="caa85-145">В окне «Публикация веб-сайта» выберите **Далее** предварительного просмотра.</span><span class="sxs-lookup"><span data-stu-id="caa85-145">In the Publish Web window, click **Next** to see the preview.</span></span>

![](publish-to-azure/_static/image8.png)

<span data-ttu-id="caa85-146">Можно щелкнуть Запуск предварительного просмотра, чтобы увидеть список файлов для публикации.</span><span class="sxs-lookup"><span data-stu-id="caa85-146">You can click Start Preview to see a list of the files to publish.</span></span> <span data-ttu-id="caa85-147">Поскольку это первый раз, необходимо опубликовать этот сайт, список будет каждого необходимого файла в проекте.</span><span class="sxs-lookup"><span data-stu-id="caa85-147">Since this is the first time you have published this site, the list is every relevant file in the project.</span></span>

<span data-ttu-id="caa85-148">Нажмите кнопку **Опубликовать**.</span><span class="sxs-lookup"><span data-stu-id="caa85-148">Click **Publish**.</span></span>

<span data-ttu-id="caa85-149">Область вывода результат будет отображен в публикации.</span><span class="sxs-lookup"><span data-stu-id="caa85-149">The Output pane will display the result of your publication.</span></span>

![](publish-to-azure/_static/image9.png)

<span data-ttu-id="caa85-150">После публикации этот сайт является immedialely, запущенный в веб-браузере.</span><span class="sxs-lookup"><span data-stu-id="caa85-150">After publication, the site is immedialely launched in a web browser.</span></span> <span data-ttu-id="caa85-151">Веб-узла была развернута и вы можете зарегистрировать нового пользователя на сайте. Однако таблицы в проекте ContosoUniversityData не еще были опубликованы.</span><span class="sxs-lookup"><span data-stu-id="caa85-151">Your site has been deployed and you can register a new user to the site; however, your tables in the ContosoUniversityData project have not yet been published.</span></span> <span data-ttu-id="caa85-152">Если щелкнуть в списке учащихся ссылку вы получите ошибку.</span><span class="sxs-lookup"><span data-stu-id="caa85-152">If you click on the List of students link you will receive an error.</span></span>

## <a name="publish-database-to-sql-azure"></a><span data-ttu-id="caa85-153">Публикация базы данных SQL Azure</span><span class="sxs-lookup"><span data-stu-id="caa85-153">Publish database to SQL Azure</span></span>

<span data-ttu-id="caa85-154">Перед публикацией базы данных, необходимо убедиться, что ваш локальный компьютер может подключиться к серверу базы данных.</span><span class="sxs-lookup"><span data-stu-id="caa85-154">Before publishing your database, you must make sure your local computer can connect to the database server.</span></span> <span data-ttu-id="caa85-155">Ограничивает брандмауэра для сервера базы данных, которой машины могут подключаться к базе данных.</span><span class="sxs-lookup"><span data-stu-id="caa85-155">The firewall for your database server restricts which machines can connect to the database.</span></span> <span data-ttu-id="caa85-156">Необходимо добавить IP-адрес компьютера разрешенных IP-адресов для брандмауэра.</span><span class="sxs-lookup"><span data-stu-id="caa85-156">You need to add the IP address of your computer to the allowed IP addresses for the firewall.</span></span>

<span data-ttu-id="caa85-157">Имя входа для учетной записи Azure на портале Azure.</span><span class="sxs-lookup"><span data-stu-id="caa85-157">Login to your Azure account through the Azure portal.</span></span>

<span data-ttu-id="caa85-158">Выберите новую базу данных и выберите **управление**.</span><span class="sxs-lookup"><span data-stu-id="caa85-158">Select your new database and select **Manage**.</span></span>

![Управление](publish-to-azure/_static/image10.png)

<span data-ttu-id="caa85-160">Необходимо настроить для разрешения подключений с компьютера сервера базы данных.</span><span class="sxs-lookup"><span data-stu-id="caa85-160">You must configure your database server to allow connections from your computer.</span></span> <span data-ttu-id="caa85-161">При выборе управление, вам будет предложено добавить текущий IP-адрес, разрешенными для сервера базы данных.</span><span class="sxs-lookup"><span data-stu-id="caa85-161">When you select Manage, you are asked to add the current IP address as permitted to the database server.</span></span> <span data-ttu-id="caa85-162">Выберите "Да".</span><span class="sxs-lookup"><span data-stu-id="caa85-162">Select Yes.</span></span>

![Добавьте IP-адрес](publish-to-azure/_static/image11.png)

<span data-ttu-id="caa85-164">Имеется возможность того, что IP-адрес, добавленного на предыдущем шаге не только IP-адрес, необходимые для настройки соединений.</span><span class="sxs-lookup"><span data-stu-id="caa85-164">There is a chance that the IP address you added in the previous step is not the only IP address you need to configure for connections.</span></span> <span data-ttu-id="caa85-165">Можно попытаться войти в базу данных на предмет соединений были правильно настроены.</span><span class="sxs-lookup"><span data-stu-id="caa85-165">You can attempt to login to the database to see if the connections have been properly set up.</span></span> <span data-ttu-id="caa85-166">Укажите имя пользователя и пароль, который был создан ранее.</span><span class="sxs-lookup"><span data-stu-id="caa85-166">Provide the user and password you created earlier.</span></span>

![Вход](publish-to-azure/_static/image12.png)

<span data-ttu-id="caa85-168">Если появляется сообщение об ошибке, необходимо добавить другой IP-адрес.</span><span class="sxs-lookup"><span data-stu-id="caa85-168">If you receive an error message, you need to add another IP address.</span></span> <span data-ttu-id="caa85-169">Щелкните сообщение об ошибке для получения дополнительных сведений об ошибке.</span><span class="sxs-lookup"><span data-stu-id="caa85-169">Click the error message to see more details about error.</span></span> <span data-ttu-id="caa85-170">В области сведений вы увидите IP-адрес, который необходимо добавить.</span><span class="sxs-lookup"><span data-stu-id="caa85-170">In the details you will see the IP address that you need to add.</span></span> <span data-ttu-id="caa85-171">Обратите внимание, этот IP-адрес.</span><span class="sxs-lookup"><span data-stu-id="caa85-171">Note this IP address.</span></span>

![не допускается](publish-to-azure/_static/image13.png)

<span data-ttu-id="caa85-173">Закройте это окно входа в систему и вернуться к порталу Azure.</span><span class="sxs-lookup"><span data-stu-id="caa85-173">Close this login window, and return to the Azure portal.</span></span>

<span data-ttu-id="caa85-174">Перейдите к панели мониторинга для базы данных.</span><span class="sxs-lookup"><span data-stu-id="caa85-174">Navigate to the Dashboard for your database.</span></span> <span data-ttu-id="caa85-175">Нажмите кнопку **управление разрешенные IP-адреса**.</span><span class="sxs-lookup"><span data-stu-id="caa85-175">Click **Manage allowed IP addresses**.</span></span>

![Управление IP-адресами](publish-to-azure/_static/image14.png)

<span data-ttu-id="caa85-177">Теперь необходимо добавить IP-адрес из сообщения об ошибке.</span><span class="sxs-lookup"><span data-stu-id="caa85-177">You must now add the IP address from the error message.</span></span> <span data-ttu-id="caa85-178">Изменить диапазон разрешенных IP-адресов, чтобы включить один из сообщения об ошибке, либо добавить этот IP-адрес как отдельный элемент.</span><span class="sxs-lookup"><span data-stu-id="caa85-178">Either change the range of allowed IP addresses to include the one from the error message, or add that IP address as a separate entry.</span></span>

![Добавить новый адрес](publish-to-azure/_static/image15.png)

<span data-ttu-id="caa85-180">Сохраните изменения, разрешенные IP-адреса.</span><span class="sxs-lookup"><span data-stu-id="caa85-180">Save the change to allowed IP addresses.</span></span>

<span data-ttu-id="caa85-181">Щелкните "Управление" и повторите вход в систему базы данных.</span><span class="sxs-lookup"><span data-stu-id="caa85-181">Click Manage, and try logging in again to the database.</span></span> <span data-ttu-id="caa85-182">Может потребоваться подождать несколько минут, прежде чем разрешенных IP-адресов настроены правильно для брандмауэра.</span><span class="sxs-lookup"><span data-stu-id="caa85-182">You may need to wait a few minutes before the allowed IP addresses are correctly configured for the firewall.</span></span> <span data-ttu-id="caa85-183">Если вы можете успешно войти базы данных, завершения установки подключения к базе данных.</span><span class="sxs-lookup"><span data-stu-id="caa85-183">When you can successfully log in the database, you have finished setting up your connection to the database.</span></span>

<span data-ttu-id="caa85-184">Это окно управления можно оставить открытым, так как результат развертывания базы данных будет проверять скоро.</span><span class="sxs-lookup"><span data-stu-id="caa85-184">You can leave this management window open because you will check the result of your database deployment shortly.</span></span>

<span data-ttu-id="caa85-185">Вернитесь в проект базы данных.</span><span class="sxs-lookup"><span data-stu-id="caa85-185">Return to your database project.</span></span> <span data-ttu-id="caa85-186">Щелкните правой кнопкой мыши проект и выберите **публикации**.</span><span class="sxs-lookup"><span data-stu-id="caa85-186">Right-click the project and select **Publish**.</span></span>

![Публикация базы данных](publish-to-azure/_static/image16.png)

<span data-ttu-id="caa85-188">В окне «базы данных публикации» выберите **изменить**.</span><span class="sxs-lookup"><span data-stu-id="caa85-188">In the Publish Database window, select **Edit**.</span></span>

![изменение;](publish-to-azure/_static/image17.png)

<span data-ttu-id="caa85-190">Укажите имя сервера базы данных и учетные данные проверки подлинности для сервера.</span><span class="sxs-lookup"><span data-stu-id="caa85-190">Provide the name of the database server and your authentication credentials for the server.</span></span> <span data-ttu-id="caa85-191">После предоставления учетных данных, выберите базу данных, созданный из списка доступных баз данных.</span><span class="sxs-lookup"><span data-stu-id="caa85-191">After providing the credentials, select the database you created from the list of available databases.</span></span> <span data-ttu-id="caa85-192">По умолчанию Visual Studio задает имя проекта, который не может быть таким же, как базы данных, которую вы создали имя поля базы данных.</span><span class="sxs-lookup"><span data-stu-id="caa85-192">By default, Visual Studio sets the name of the database field to the name of your project which might not be the same as the database you created.</span></span>

![](publish-to-azure/_static/image18.png)

<span data-ttu-id="caa85-193">Нажмите кнопку ОК.</span><span class="sxs-lookup"><span data-stu-id="caa85-193">Click OK.</span></span>

<span data-ttu-id="caa85-194">Возможно, потребуется сохранить профиль, чтобы опубликовать обновления в будущем без необходимости повторного ввода всех сведений о соединении.</span><span class="sxs-lookup"><span data-stu-id="caa85-194">You will probably want to save this profile so you can publish updates in the future without re-entering all of the connection information.</span></span> <span data-ttu-id="caa85-195">Выберите **Создать профиль**.</span><span class="sxs-lookup"><span data-stu-id="caa85-195">Select **Create Profile**.</span></span>

![Сохранение профиля](publish-to-azure/_static/image19.png)

<span data-ttu-id="caa85-197">Она создаст файл с именем проекта **ContosoUniversityData.publish.xml**.</span><span class="sxs-lookup"><span data-stu-id="caa85-197">It will create a file in your project named **ContosoUniversityData.publish.xml**.</span></span> <span data-ttu-id="caa85-198">В следующий раз, необходимо опубликовать базу данных в Azure, просто загрузить этот профиль.</span><span class="sxs-lookup"><span data-stu-id="caa85-198">The next time you want to publish the database to Azure, simply load that profile.</span></span>

<span data-ttu-id="caa85-199">Теперь щелкните **публикации** для создания базы данных в Azure.</span><span class="sxs-lookup"><span data-stu-id="caa85-199">Now, click **Publish** to create the database on Azure.</span></span>

![publish](publish-to-azure/_static/image20.png)

<span data-ttu-id="caa85-201">После выполнения команды на некоторое время публикации результатов отображаются.</span><span class="sxs-lookup"><span data-stu-id="caa85-201">After running for a while, the publishing results are displayed.</span></span>

![results](publish-to-azure/_static/image21.png)

<span data-ttu-id="caa85-203">Теперь можно перейти обратно на портал управления для базы данных.</span><span class="sxs-lookup"><span data-stu-id="caa85-203">Now, you can go back to the management portal for your database.</span></span> <span data-ttu-id="caa85-204">Обновите представление конструктора и обратите внимание, что были развернуты 3 таблицы предварительно заполняется данными.</span><span class="sxs-lookup"><span data-stu-id="caa85-204">Refresh the design view, and notice the 3 tables with pre-filled data have been deployed.</span></span>

![новые таблицы](publish-to-azure/_static/image22.png)

<span data-ttu-id="caa85-206">Теперь все готово для тестирования веб-приложения, которое развертывается в Azure.</span><span class="sxs-lookup"><span data-stu-id="caa85-206">Now you are ready to test the web app that is deployed to Azure.</span></span> <span data-ttu-id="caa85-207">Перейдите к веб-приложения в Azure (например, http://contosositeexample.azurewebsites.net/).</span><span class="sxs-lookup"><span data-stu-id="caa85-207">Navigate to the web app on Azure (such as http://contosositeexample.azurewebsites.net/).</span></span> <span data-ttu-id="caa85-208">Щелкните ссылку для списка учащихся, и вы увидите представление index для учащихся.</span><span class="sxs-lookup"><span data-stu-id="caa85-208">Click the link for List of students and you should see the index view for students.</span></span>

![view](publish-to-azure/_static/image23.png)

<span data-ttu-id="caa85-210">Иногда базы данных и подключение, потребуется немного времени, чтобы правильно настроить.</span><span class="sxs-lookup"><span data-stu-id="caa85-210">Occasionally, the database and connection need a little time to be properly configured.</span></span> <span data-ttu-id="caa85-211">Если появляется ошибка, подождите несколько минут и повторите попытку.</span><span class="sxs-lookup"><span data-stu-id="caa85-211">If you receive an error, wait a few minutes and then try again.</span></span>

## <a name="conclusion"></a><span data-ttu-id="caa85-212">Заключение</span><span class="sxs-lookup"><span data-stu-id="caa85-212">Conclusion</span></span>

<span data-ttu-id="caa85-213">Эта серия предоставлен простой пример того, как для создания кода из существующей базы данных, позволяющий пользователям изменять, обновлять, создания и удаления данных.</span><span class="sxs-lookup"><span data-stu-id="caa85-213">This series provided a simple example of how to generate code from an existing database that enables users to edit, update, create and delete data.</span></span> <span data-ttu-id="caa85-214">Он используется ASP.NET MVC 5, Entity Framework и формирование шаблонов ASP.NET для создания проекта.</span><span class="sxs-lookup"><span data-stu-id="caa85-214">It used ASP.NET MVC 5, Entity Framework and ASP.NET Scaffolding to create the project.</span></span>

<span data-ttu-id="caa85-215">Простейший пример шаблона разработки Code First, в разделе [Приступая к работе с ASP.NET MVC 5](../introduction/getting-started.md).</span><span class="sxs-lookup"><span data-stu-id="caa85-215">For an introductory example of Code First development, see [Getting Started with ASP.NET MVC 5](../introduction/getting-started.md).</span></span>

<span data-ttu-id="caa85-216">Более сложный пример, в разделе [Создание модели данных Entity Framework для приложения ASP.NET MVC 4](../getting-started-with-ef-using-mvc/creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md).</span><span class="sxs-lookup"><span data-stu-id="caa85-216">For a more advanced example, see [Creating an Entity Framework Data Model for an ASP.NET MVC 4 App](../getting-started-with-ef-using-mvc/creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md).</span></span> <span data-ttu-id="caa85-217">Обратите внимание, что DbContext API, который используется для работы с данными в первой базы данных совпадает с API, можно использовать для работы с данными в Code First.</span><span class="sxs-lookup"><span data-stu-id="caa85-217">Note that the DbContext API that you use for working with data in Database First is the same as the API you use for working with data in Code First.</span></span> <span data-ttu-id="caa85-218">Даже если вы планируете использовать первой базы данных, можно узнать, как обрабатывать более сложные сценарии, такие как чтение и обновление связанных данных обработки конфликтов параллелизма, и так далее из учебника Code First.</span><span class="sxs-lookup"><span data-stu-id="caa85-218">Even if you intend to use Database First, you can learn how to handle more complex scenarios such as reading and updating related data, handling concurrency conflicts, and so forth from a Code First tutorial.</span></span> <span data-ttu-id="caa85-219">Единственное отличие заключается в том, как были созданы базы данных, класс контекста и классы сущностей.</span><span class="sxs-lookup"><span data-stu-id="caa85-219">The only difference is in how the database, context class, and entity classes are created.</span></span>

> [!div class="step-by-step"]
> [<span data-ttu-id="caa85-220">Назад</span><span class="sxs-lookup"><span data-stu-id="caa85-220">Previous</span></span>](enhancing-data-validation.md)
