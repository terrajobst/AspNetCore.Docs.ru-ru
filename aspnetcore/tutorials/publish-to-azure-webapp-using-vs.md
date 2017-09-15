---
title: "Публикация приложения ASP.NET Core в Azure с помощью Visual Studio"
author: rick-anderson
description: 
keywords: ASP.NET Core
ms.author: riande
manager: wpickett
ms.date: 10/14/2016
ms.topic: get-started-article
ms.assetid: 78571e4a-a143-452d-9cf2-0860f85972e6
ms.technology: aspnet
ms.prod: asp.net-core
uid: tutorials/publish-to-azure-webapp-using-vs
ms.openlocfilehash: c4df5f4551760fbc4ed4b11362d249b24f186e00
ms.sourcegitcommit: 8f5277871eff86134ebf68d3737196cfd4a62c2c
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 08/13/2017
---
# <a name="publish-an-aspnet-core-web-app-to-azure-app-service-using-visual-studio"></a><span data-ttu-id="66b4d-103">Публикация веб-приложения ASP.NET Core в службе приложений Azure с помощью Visual Studio</span><span class="sxs-lookup"><span data-stu-id="66b4d-103">Publish an ASP.NET Core web app to Azure App Service using Visual Studio</span></span>

<span data-ttu-id="66b4d-104">Авторы: [Рик Андерсон (Rick Anderson)](https://twitter.com/RickAndMSFT) и [Сезар Блум Сильвейра (Cesar Blum Silveira)](https://github.com/cesarbs)</span><span class="sxs-lookup"><span data-stu-id="66b4d-104">By [Rick Anderson](https://twitter.com/RickAndMSFT) and [Cesar Blum Silveira](https://github.com/cesarbs)</span></span>

## <a name="set-up-the-development-environment"></a><span data-ttu-id="66b4d-105">Настройка среды разработки</span><span class="sxs-lookup"><span data-stu-id="66b4d-105">Set up the development environment</span></span>

* <span data-ttu-id="66b4d-106">Установите последнюю версию [пакета Azure SDK для Visual Studio](https://www.visualstudio.com/features/azure-tools-vs).</span><span class="sxs-lookup"><span data-stu-id="66b4d-106">Install the latest [Azure SDK for Visual Studio](https://www.visualstudio.com/features/azure-tools-vs).</span></span> <span data-ttu-id="66b4d-107">При установке пакета SDK будет установлена Visual Studio, если это еще не сделано.</span><span class="sxs-lookup"><span data-stu-id="66b4d-107">The SDK installs Visual Studio if you don't already have it.</span></span>

> [!NOTE]
> <span data-ttu-id="66b4d-108">Если на компьютере немного зависимостей, установка пакета SDK может занять более 30 минут.</span><span class="sxs-lookup"><span data-stu-id="66b4d-108">The SDK installation can take more than 30 minutes if your machine doesn't have many of the dependencies.</span></span>

* <span data-ttu-id="66b4d-109">Установите [.NET Core и инструменты Visual Studio](http://go.microsoft.com/fwlink/?LinkID=798306).</span><span class="sxs-lookup"><span data-stu-id="66b4d-109">Install [.NET Core + Visual Studio tooling](http://go.microsoft.com/fwlink/?LinkID=798306)</span></span>

* <span data-ttu-id="66b4d-110">Проверьте свою [учетную запись Azure](https://portal.azure.com/).</span><span class="sxs-lookup"><span data-stu-id="66b4d-110">Verify your [Azure account](https://portal.azure.com/).</span></span> <span data-ttu-id="66b4d-111">Вы можете [открыть бесплатную учетную запись Azure](https://azure.microsoft.com/pricing/free-trial/) или [активировать преимущества для подписчиков Visual Studio](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/).</span><span class="sxs-lookup"><span data-stu-id="66b4d-111">You can [open a free Azure account](https://azure.microsoft.com/pricing/free-trial/) or [Activate Visual Studio subscriber benefits](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/).</span></span>

## <a name="create-a-web-app"></a><span data-ttu-id="66b4d-112">Создание веб-приложения</span><span class="sxs-lookup"><span data-stu-id="66b4d-112">Create a web app</span></span>

<span data-ttu-id="66b4d-113">На начальной странице Visual Studio выберите **Создать проект...** </span><span class="sxs-lookup"><span data-stu-id="66b4d-113">In the Visual Studio Start Page, tap **New Project...**.</span></span>

![Начальная страница](publish-to-azure-webapp-using-vs/_static/new_project.png)

<span data-ttu-id="66b4d-115">Кроме того, для создания проекта можно использовать меню.</span><span class="sxs-lookup"><span data-stu-id="66b4d-115">Alternatively, you can use the menus to create a new project.</span></span> <span data-ttu-id="66b4d-116">Выберите **Файл > Создать > Проект...**</span><span class="sxs-lookup"><span data-stu-id="66b4d-116">Tap **File > New > Project...**.</span></span>

![меню "Файл"](publish-to-azure-webapp-using-vs/_static/alt_new_project.png)

<span data-ttu-id="66b4d-118">В диалоговом окне **Создание проекта** выполните следующие действия.</span><span class="sxs-lookup"><span data-stu-id="66b4d-118">Complete the **New Project** dialog:</span></span>

* <span data-ttu-id="66b4d-119">В левой области выберите **Веб**.</span><span class="sxs-lookup"><span data-stu-id="66b4d-119">In the left pane, tap **Web**</span></span>

* <span data-ttu-id="66b4d-120">В центральной области выберите **Веб-приложение ASP.NET Core (.NET Core)**.</span><span class="sxs-lookup"><span data-stu-id="66b4d-120">In the center pane, tap **ASP.NET Core Web Application (.NET Core)**</span></span>

* <span data-ttu-id="66b4d-121">Нажмите кнопку **ОК**.</span><span class="sxs-lookup"><span data-stu-id="66b4d-121">Tap **OK**</span></span>

![Диалоговое окно создания нового проекта](publish-to-azure-webapp-using-vs/_static/new_prj.png)

<span data-ttu-id="66b4d-123">В диалоговом окне **Создание веб-приложения ASP.NET Core (.NET Core)** выполните следующие действия.</span><span class="sxs-lookup"><span data-stu-id="66b4d-123">In the **New ASP.NET Core Web Application (.NET Core)** dialog:</span></span>

* <span data-ttu-id="66b4d-124">Выберите **Веб-приложение**.</span><span class="sxs-lookup"><span data-stu-id="66b4d-124">Tap **Web Application**</span></span>

* <span data-ttu-id="66b4d-125">Задайте для параметра **Проверка подлинности** значение **Учетные записи отдельных пользователей**</span><span class="sxs-lookup"><span data-stu-id="66b4d-125">Verify **Authentication** is set to **Individual User Accounts**</span></span>

* <span data-ttu-id="66b4d-126">Проверьте, что флажок **Размещение в облаке** **не** установлен.</span><span class="sxs-lookup"><span data-stu-id="66b4d-126">Verify **Host in the cloud** is **not** checked</span></span>

* <span data-ttu-id="66b4d-127">Нажмите кнопку **ОК**.</span><span class="sxs-lookup"><span data-stu-id="66b4d-127">Tap **OK**</span></span>

![Диалоговое окно "Создание веб-приложения ASP.NET Core (.NET Core)"](publish-to-azure-webapp-using-vs/_static/noath.png)

## <a name="test-the-app-locally"></a><span data-ttu-id="66b4d-129">Локальное тестирование приложения</span><span class="sxs-lookup"><span data-stu-id="66b4d-129">Test the app locally</span></span>

* <span data-ttu-id="66b4d-130">Нажмите сочетание клавиш **CTRL+F5** для локального запуска приложения.</span><span class="sxs-lookup"><span data-stu-id="66b4d-130">Press **Ctrl-F5** to run the app locally</span></span>

* <span data-ttu-id="66b4d-131">Щелкните ссылки **О программе** и **Контакт**.</span><span class="sxs-lookup"><span data-stu-id="66b4d-131">Tap the **About** and **Contact** links.</span></span> <span data-ttu-id="66b4d-132">В зависимости от размера устройства может потребоваться щелкнуть значок навигации, чтобы отобразить ссылки.</span><span class="sxs-lookup"><span data-stu-id="66b4d-132">Depending on the size of your device, you might need to tap the navigation icon to show the links</span></span>

![Веб-приложение откроется в localhost.](publish-to-azure-webapp-using-vs/_static/show.png)

* <span data-ttu-id="66b4d-134">Щелкните **Зарегистрировать** и зарегистрируйте нового пользователя.</span><span class="sxs-lookup"><span data-stu-id="66b4d-134">Tap **Register** and register a new user.</span></span> <span data-ttu-id="66b4d-135">Можно использовать вымышленный адрес электронной почты.</span><span class="sxs-lookup"><span data-stu-id="66b4d-135">You can use a fictitious email address.</span></span> <span data-ttu-id="66b4d-136">При отправке появится следующее сообщение об ошибке:</span><span class="sxs-lookup"><span data-stu-id="66b4d-136">When you submit, you'll get the following error:</span></span>

![Внутренняя ошибка сервера: сбой операции из базы данных при обработке запроса.](publish-to-azure-webapp-using-vs/_static/mig.png)

<span data-ttu-id="66b4d-140">Устранить проблему можно двумя разными способами:</span><span class="sxs-lookup"><span data-stu-id="66b4d-140">You can fix the problem in two different ways:</span></span>

* <span data-ttu-id="66b4d-141">Нажмите **Применить миграции** и после обновления страницы перезагрузите ее.</span><span class="sxs-lookup"><span data-stu-id="66b4d-141">Tap **Apply Migrations** and, once the page updates, refresh the page; or</span></span>

* <span data-ttu-id="66b4d-142">Или в командной строке в каталоге проекта выполните следующее:</span><span class="sxs-lookup"><span data-stu-id="66b4d-142">Run the following from a command prompt in the project's directory:</span></span>

  <!-- literal_block {"ids": [], "xml:space": "preserve"} -->

  ```
  dotnet ef database update
     ```

<span data-ttu-id="66b4d-143">Приложение отобразит адрес сообщение электронной почты, которое используется для регистрации нового пользователя, и ссылку **Выход**.</span><span class="sxs-lookup"><span data-stu-id="66b4d-143">The app displays the email used to register the new user and a **Log off** link.</span></span>

![Веб-приложение откроется в Microsoft Edge.](publish-to-azure-webapp-using-vs/_static/hello.png)

## <a name="deploy-the-app-to-azure"></a><span data-ttu-id="66b4d-146">Развертывание приложения в Azure</span><span class="sxs-lookup"><span data-stu-id="66b4d-146">Deploy the app to Azure</span></span>

<span data-ttu-id="66b4d-147">Убедитесь в том, что приложение, публикуемое для развертывания, не запущено.</span><span class="sxs-lookup"><span data-stu-id="66b4d-147">Confirm that the published app for deployment isn't running.</span></span> <span data-ttu-id="66b4d-148">Во время выполнения приложения файлы в папке *publish* блокируются.</span><span class="sxs-lookup"><span data-stu-id="66b4d-148">Files in the *publish* folder are locked when the app is running.</span></span> <span data-ttu-id="66b4d-149">Заблокированные файлы нельзя скопировать, так что развертывание в этом случае не произойдет.</span><span class="sxs-lookup"><span data-stu-id="66b4d-149">Deployment can't occur because locked files can't be copied.</span></span>

<span data-ttu-id="66b4d-150">В обозревателе решений щелкните правой кнопкой мыши проект и выберите команду **Опубликовать…**</span><span class="sxs-lookup"><span data-stu-id="66b4d-150">Right-click on the project in Solution Explorer and select **Publish...**.</span></span>

![Открытое контекстное меню с выделенной ссылкой "Опубликовать"](publish-to-azure-webapp-using-vs/_static/pub.png)

<span data-ttu-id="66b4d-152">В диалоговом окне **Публикация** выберите **Служба приложений Microsoft Azure**.</span><span class="sxs-lookup"><span data-stu-id="66b4d-152">In the **Publish** dialog, tap **Microsoft Azure App Service**.</span></span>

![Диалоговое окно публикации](publish-to-azure-webapp-using-vs/_static/maas1.png)

<span data-ttu-id="66b4d-154">Щелкните **Создать...**, чтобы создать группу ресурсов.</span><span class="sxs-lookup"><span data-stu-id="66b4d-154">Tap **New...** to create a new resource group.</span></span> <span data-ttu-id="66b4d-155">Создание группы ресурсов упростит удаление всех ресурсов Azure, создаваемых в этом учебнике.</span><span class="sxs-lookup"><span data-stu-id="66b4d-155">Creating a new resource group will make it easier to delete all the Azure resources you create in this tutorial.</span></span>

![Диалоговое окно "Служба приложений"](publish-to-azure-webapp-using-vs/_static/newrg1.png)

<span data-ttu-id="66b4d-157">Создайте группу ресурсов и план службы приложений:</span><span class="sxs-lookup"><span data-stu-id="66b4d-157">Create a new resource group and app service plan:</span></span>

* <span data-ttu-id="66b4d-158">Щелкните **Создать...**  рядом с группой ресурсов и введите имя для новой группы ресурсов.</span><span class="sxs-lookup"><span data-stu-id="66b4d-158">Tap **New...** for the resource group and enter a name for the new resource group</span></span>

* <span data-ttu-id="66b4d-159">Щелкните **Создать...**  рядом с планом службы приложений и выберите ближайшее расположение.</span><span class="sxs-lookup"><span data-stu-id="66b4d-159">Tap **New...** for the  app service plan and select a location near you.</span></span> <span data-ttu-id="66b4d-160">Можно сохранить созданное по умолчанию имя.</span><span class="sxs-lookup"><span data-stu-id="66b4d-160">You can keep the default generated name</span></span>

* <span data-ttu-id="66b4d-161">Нажмите **Обзор дополнительных служб Azure**, чтобы создать базу данных.</span><span class="sxs-lookup"><span data-stu-id="66b4d-161">Tap **Explore additional Azure services** to create a new database</span></span>

![Диалоговое окно новой группы ресурсов: панель размещения](publish-to-azure-webapp-using-vs/_static/cas.png)

* <span data-ttu-id="66b4d-163">Щелкните зеленый значок **+**, чтобы создать базу данных SQL.</span><span class="sxs-lookup"><span data-stu-id="66b4d-163">Tap the green **+** icon to create a new SQL Database</span></span>

![Диалоговое окно новой группы ресурсов: панель служб](publish-to-azure-webapp-using-vs/_static/sql.png)

* <span data-ttu-id="66b4d-165">Щелкните **Создать...**  в диалоговом окне **Настройка базы данных SQL**, чтобы создать сервер базы данных.</span><span class="sxs-lookup"><span data-stu-id="66b4d-165">Tap **New...** on the **Configure SQL Database** dialog to create a new database server.</span></span>

![Диалоговое окно "Настройка базы данных SQL"](publish-to-azure-webapp-using-vs/_static/conf.png)

* <span data-ttu-id="66b4d-167">Введите имя пользователя администратора и пароль, а затем нажмите кнопку **ОК**.</span><span class="sxs-lookup"><span data-stu-id="66b4d-167">Enter an administrator user name and password, and then tap **OK**.</span></span> <span data-ttu-id="66b4d-168">Не забудьте имя пользователя и пароль, созданные на этом шаге.</span><span class="sxs-lookup"><span data-stu-id="66b4d-168">Don't forget the user name and password you create in this step.</span></span> <span data-ttu-id="66b4d-169">Можно сохранить созданное по умолчанию значение в поле **Имя сервера**.</span><span class="sxs-lookup"><span data-stu-id="66b4d-169">You can keep the default **Server Name**</span></span>

![Диалоговое окно "Настройка SQL Server"](publish-to-azure-webapp-using-vs/_static/conf_servername.png)

> [!NOTE]
> <span data-ttu-id="66b4d-171">"admin" не может использоваться в качестве имени пользователя администратора.</span><span class="sxs-lookup"><span data-stu-id="66b4d-171">"admin" is not allowed as the administrator user name.</span></span>

* <span data-ttu-id="66b4d-172">В диалоговом окне **Настройка базы данных SQL** нажмите кнопку **ОК**.</span><span class="sxs-lookup"><span data-stu-id="66b4d-172">Tap **OK** on the  **Configure SQL Database** dialog</span></span>

![Диалоговое окно "Настройка базы данных SQL"](publish-to-azure-webapp-using-vs/_static/conf_final.png)

* <span data-ttu-id="66b4d-174">Нажмите кнопку **Создать** в диалоговом окне **Создание службы приложений**.</span><span class="sxs-lookup"><span data-stu-id="66b4d-174">Tap **Create** on the **Create App Service** dialog</span></span>

![Диалоговое окно "Создание службы приложений"](publish-to-azure-webapp-using-vs/_static/create_as.png)

* <span data-ttu-id="66b4d-176">Нажмите кнопку **Далее** в диалоговом окне **Публикация**.</span><span class="sxs-lookup"><span data-stu-id="66b4d-176">Tap **Next** in the **Publish** dialog</span></span>

![Диалоговое окно "Публикация": панель "Подключение"](publish-to-azure-webapp-using-vs/_static/pubc.png)

* <span data-ttu-id="66b4d-178">В разделе **Параметры** диалогового окна **Публикация**:</span><span class="sxs-lookup"><span data-stu-id="66b4d-178">On the **Settings** stage of the **Publish** dialog:</span></span>

  * <span data-ttu-id="66b4d-179">Разверните узел **Базы данных** и установите флажок **Использовать эту строку подключения во время выполнения**.</span><span class="sxs-lookup"><span data-stu-id="66b4d-179">Expand **Databases** and check **Use this connection string at runtime**</span></span>

  * <span data-ttu-id="66b4d-180">Разверните узел **Миграции Entity Framework** и установите флажок **Использовать эту миграцию при публикации**.</span><span class="sxs-lookup"><span data-stu-id="66b4d-180">Expand **Entity Framework Migrations** and check **Apply this migration on publish**</span></span>

* <span data-ttu-id="66b4d-181">Нажмите кнопку **Опубликовать** и подождите, пока Visual Studio завершит публикацию приложения.</span><span class="sxs-lookup"><span data-stu-id="66b4d-181">Tap **Publish** and wait until Visual Studio finishes publishing your app</span></span>

![Диалоговое окно "Публикация": панель "Параметры"](publish-to-azure-webapp-using-vs/_static/pubs.png)

<span data-ttu-id="66b4d-183">Visual Studio опубликует приложение в Azure и запустит облачное приложение в браузере.</span><span class="sxs-lookup"><span data-stu-id="66b4d-183">Visual Studio will publish your app to Azure and launch the cloud app in your browser.</span></span>

### <a name="test-your-app-in-azure"></a><span data-ttu-id="66b4d-184">Тестирование приложения в Azure</span><span class="sxs-lookup"><span data-stu-id="66b4d-184">Test your app in Azure</span></span>

* <span data-ttu-id="66b4d-185">Протестируйте ссылки **О программе** и **Контакт**.</span><span class="sxs-lookup"><span data-stu-id="66b4d-185">Test the **About** and **Contact** links</span></span>

* <span data-ttu-id="66b4d-186">Регистрация нового пользователя</span><span class="sxs-lookup"><span data-stu-id="66b4d-186">Register a new user</span></span>

![Веб-приложение, открытое в Microsoft Edge в службе приложений Azure](publish-to-azure-webapp-using-vs/_static/final.png)

### <a name="update-the-app"></a><span data-ttu-id="66b4d-188">Обновление приложения</span><span class="sxs-lookup"><span data-stu-id="66b4d-188">Update the app</span></span>

* <span data-ttu-id="66b4d-189">Отредактируйте файл представления Razor `Views/Home/About.cshtml` и измените его содержимое.</span><span class="sxs-lookup"><span data-stu-id="66b4d-189">Edit the `Views/Home/About.cshtml` Razor view file and change its contents.</span></span> <span data-ttu-id="66b4d-190">Пример:</span><span class="sxs-lookup"><span data-stu-id="66b4d-190">For example:</span></span>

<!-- literal_block {"ids": [], "linenos": false, "xml:space": "preserve", "language": "html", "highlight_args": {"hl_lines": [7]}} -->

```html
@{
       ViewData["Title"] = "About";
   }
   <h2>@ViewData["Title"].</h2>
   <h3>@ViewData["Message"]</h3>

   <p>My updated about page.</p>
   ```

* <span data-ttu-id="66b4d-191">Щелкните проект правой кнопкой мыши и снова выберите пункт **Опубликовать...**</span><span class="sxs-lookup"><span data-stu-id="66b4d-191">Right-click on the project and tap **Publish...** again</span></span>

![Открытое контекстное меню с выделенной ссылкой "Опубликовать"](publish-to-azure-webapp-using-vs/_static/pub.png)

* <span data-ttu-id="66b4d-193">После публикации приложения проверьте, что внесенные изменения доступны в Azure.</span><span class="sxs-lookup"><span data-stu-id="66b4d-193">After the app is published, verify the changes you made are available on Azure</span></span>

### <a name="clean-up"></a><span data-ttu-id="66b4d-194">Очистка</span><span class="sxs-lookup"><span data-stu-id="66b4d-194">Clean up</span></span>

<span data-ttu-id="66b4d-195">Завершив тестирование приложения, перейдите на [портал Azure](https://portal.azure.com/) и удалите приложение.</span><span class="sxs-lookup"><span data-stu-id="66b4d-195">When you have finished testing the app, go to the [Azure portal](https://portal.azure.com/) and delete the app.</span></span>

* <span data-ttu-id="66b4d-196">Выберите пункт **Группы ресурсов**, затем созданную группу ресурсов.</span><span class="sxs-lookup"><span data-stu-id="66b4d-196">Select **Resource groups**, then tap the resource group you created</span></span>

![Портал Azure: пункт "Группы ресурсов" в меню боковой панели](publish-to-azure-webapp-using-vs/_static/portalrg.png)

* <span data-ttu-id="66b4d-198">В колонке **Группы ресурсов** нажмите кнопку **Удалить**.</span><span class="sxs-lookup"><span data-stu-id="66b4d-198">In the **Resource group** blade, tap **Delete**</span></span>

![Портал Azure: колонка "Группы ресурсов"](publish-to-azure-webapp-using-vs/_static/rgd.png)

* <span data-ttu-id="66b4d-200">Измените имя группы ресурсов, а затем нажмите кнопку **Удалить**.</span><span class="sxs-lookup"><span data-stu-id="66b4d-200">Enter the name of the resource group and tap **Delete**.</span></span> <span data-ttu-id="66b4d-201">Ваше приложение и все ресурсы, созданные в ходе изучения этого учебника, удалены из Azure.</span><span class="sxs-lookup"><span data-stu-id="66b4d-201">Your app and all other resources created in this tutorial are now deleted from Azure</span></span>

### <a name="next-steps"></a><span data-ttu-id="66b4d-202">Следующие шаги</span><span class="sxs-lookup"><span data-stu-id="66b4d-202">Next steps</span></span>

* [<span data-ttu-id="66b4d-203">Начало работы с ASP.NET Core MVC и Visual Studio</span><span class="sxs-lookup"><span data-stu-id="66b4d-203">Getting started with ASP.NET Core MVC and Visual Studio</span></span>](first-mvc-app/start-mvc.md)

* [<span data-ttu-id="66b4d-204">Введение в ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="66b4d-204">Introduction to ASP.NET Core</span></span>](../index.md)

* [<span data-ttu-id="66b4d-205">Основы</span><span class="sxs-lookup"><span data-stu-id="66b4d-205">Fundamentals</span></span>](../fundamentals/index.md)
