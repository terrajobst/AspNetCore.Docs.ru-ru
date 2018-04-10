---
uid: mvc/overview/older-versions/creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript
title: Создание MVC 3 Application with Razor and Unobtrusive JavaScript | Документы Microsoft
author: microsoft
description: Список пользователей примера веб-приложения показано, как простой является создание приложения ASP.NET MVC 3 с помощью представлений Razor. В образце приложения s...
ms.author: aspnetcontent
manager: wpickett
ms.date: 11/01/2010
ms.topic: article
ms.assetid: 658b149b-d770-46bf-8b4b-4e47cca242f3
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions/creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript
msc.type: authoredcontent
ms.openlocfilehash: 9b273f6827cad2078b581d6da7b127198dfddaa5
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/10/2018
---
<a name="creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript"></a><span data-ttu-id="8ccf8-104">Создание MVC 3 Application with Razor and Unobtrusive JavaScript</span><span class="sxs-lookup"><span data-stu-id="8ccf8-104">Creating a MVC 3 Application with Razor and Unobtrusive JavaScript</span></span>
====================
<span data-ttu-id="8ccf8-105">по [Microsoft](https://github.com/microsoft)</span><span class="sxs-lookup"><span data-stu-id="8ccf8-105">by [Microsoft](https://github.com/microsoft)</span></span>

> <span data-ttu-id="8ccf8-106">Список пользователей примера веб-приложения показано, как простой является создание приложения ASP.NET MVC 3 с помощью представлений Razor.</span><span class="sxs-lookup"><span data-stu-id="8ccf8-106">The User List sample web application demonstrates how simple it is to create ASP.NET MVC 3 applications using the Razor view engine.</span></span> <span data-ttu-id="8ccf8-107">Образец приложения показано, как использовать новый обработчик представлений Razor с ASP.NET MVC версии 3 и Visual Studio 2010 для создания вымышленной список пользователей веб-сайта, включающий функции, такие как создание, отображение, редактирование и удаление пользователей.</span><span class="sxs-lookup"><span data-stu-id="8ccf8-107">The sample application shows how to use the new Razor view engine with ASP.NET MVC version 3 and Visual Studio 2010 to create a fictional User List website that includes functionality such as creating, displaying, editing, and deleting users.</span></span>
> 
> <span data-ttu-id="8ccf8-108">Этот учебник описывает шаги, которые были выполнены для создания примера приложения ASP.NET MVC 3 списка пользователей.</span><span class="sxs-lookup"><span data-stu-id="8ccf8-108">This tutorial describes the steps that were taken in order to build the User List sample ASP.NET MVC 3 application.</span></span> <span data-ttu-id="8ccf8-109">Проект Visual Studio с исходным кодом C# и VB доступна по следующему адресу: [загрузить](https://code.msdn.microsoft.com/aspnetmvcsamples/Release/ProjectReleases.aspx?ReleaseId=5114).</span><span class="sxs-lookup"><span data-stu-id="8ccf8-109">A Visual Studio project with C# and VB source code is available to accompany this topic: [Download](https://code.msdn.microsoft.com/aspnetmvcsamples/Release/ProjectReleases.aspx?ReleaseId=5114).</span></span> <span data-ttu-id="8ccf8-110">Если у вас есть вопросы о этого учебника, опубликуйте их [форум MVC](https://forums.asp.net/1146.aspx).</span><span class="sxs-lookup"><span data-stu-id="8ccf8-110">If you have questions about this tutorial, please post them to the [MVC forum](https://forums.asp.net/1146.aspx).</span></span>


## <a name="overview"></a><span data-ttu-id="8ccf8-111">Обзор</span><span class="sxs-lookup"><span data-stu-id="8ccf8-111">Overview</span></span>

<span data-ttu-id="8ccf8-112">Приложение, которое вы будете построение имеет простой пользовательский список веб-сайта.</span><span class="sxs-lookup"><span data-stu-id="8ccf8-112">The application you'll be building is a simple user list website.</span></span> <span data-ttu-id="8ccf8-113">Пользователи могут вводить, просматривать и обновлять сведения о пользователе.</span><span class="sxs-lookup"><span data-stu-id="8ccf8-113">Users can enter, view, and update user information.</span></span>

![Пример узла](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/_static/image1.png)

<span data-ttu-id="8ccf8-115">Завершенный проект VB и C# можно загрузить [здесь](https://code.msdn.microsoft.com/Creating-a-MVC-3-28883c0f).</span><span class="sxs-lookup"><span data-stu-id="8ccf8-115">You can download the VB and C# completed project [here](https://code.msdn.microsoft.com/Creating-a-MVC-3-28883c0f).</span></span>

## <a name="creating-the-web-application"></a><span data-ttu-id="8ccf8-116">Создание веб-приложения</span><span class="sxs-lookup"><span data-stu-id="8ccf8-116">Creating the Web Application</span></span>

<span data-ttu-id="8ccf8-117">Чтобы запустить учебник, откройте Visual Studio 2010 и создайте новый проект с помощью *веб-приложение ASP.NET MVC 3* шаблона.</span><span class="sxs-lookup"><span data-stu-id="8ccf8-117">To start the tutorial, open Visual Studio 2010 and create a new project using the *ASP.NET MVC 3 Web Application* template.</span></span> <span data-ttu-id="8ccf8-118">Назовите приложение &quot;Mvc3Razor&quot;.</span><span class="sxs-lookup"><span data-stu-id="8ccf8-118">Name the application &quot;Mvc3Razor&quot;.</span></span>

<span data-ttu-id="8ccf8-119">[![Новый проект MVC 3](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/_static/image3.png)](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/_static/image2.png)</span><span class="sxs-lookup"><span data-stu-id="8ccf8-119">[![New MVC 3 project](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/_static/image3.png)](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/_static/image2.png)</span></span>

<span data-ttu-id="8ccf8-120">В **нового проекта ASP.NET MVC 3** диалогового окна выберите **веб-приложение**выберите представлений Razor и нажмите кнопку **ОК**.</span><span class="sxs-lookup"><span data-stu-id="8ccf8-120">In the **New ASP.NET MVC 3 Project** dialog, select **Internet Application**, select the Razor view engine, and then click **OK**.</span></span>

![Диалоговое окно нового проекта ASP.NET MVC 3](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/_static/image4.png)

<span data-ttu-id="8ccf8-122">В этом учебнике будет не использоваться поставщик членства ASP.NET, можно удалить все файлы, связанные с входа и членства.</span><span class="sxs-lookup"><span data-stu-id="8ccf8-122">In this tutorial you will not be using the ASP.NET membership provider, so you can delete all the files associated with logon and membership.</span></span> <span data-ttu-id="8ccf8-123">В **обозревателе решений**, удалите следующие файлы и каталоги:</span><span class="sxs-lookup"><span data-stu-id="8ccf8-123">In **Solution Explorer**, remove the following files and directories:</span></span>

- <span data-ttu-id="8ccf8-124">*Controllers\AccountController*</span><span class="sxs-lookup"><span data-stu-id="8ccf8-124">*Controllers\AccountController*</span></span>
- <span data-ttu-id="8ccf8-125">*Models\AccountModels*</span><span class="sxs-lookup"><span data-stu-id="8ccf8-125">*Models\AccountModels*</span></span>
- <span data-ttu-id="8ccf8-126">*Views\Shared\\_LogOnPartial*</span><span class="sxs-lookup"><span data-stu-id="8ccf8-126">*Views\Shared\\_LogOnPartial*</span></span>
- <span data-ttu-id="8ccf8-127">*Views\Account* (и все файлы в этом каталоге)</span><span class="sxs-lookup"><span data-stu-id="8ccf8-127">*Views\Account* (and all the files in this directory)</span></span>

![Soln Exp](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/_static/image5.png)

<span data-ttu-id="8ccf8-129">Изменить  <em>\_Layout.cshtml</em> и замените разметку в `<div>` элемента с именем `logindisplay` с сообщением <em>&quot;</em>входа отключено&quot;.</span><span class="sxs-lookup"><span data-stu-id="8ccf8-129">Edit the <em>\_Layout.cshtml</em> file and replace the markup inside the `<div>` element named `logindisplay` with the message <em>&quot;</em>Login Disabled&quot;.</span></span> <span data-ttu-id="8ccf8-130">В следующем примере показано новый разметку:</span><span class="sxs-lookup"><span data-stu-id="8ccf8-130">The following example shows the new markup:</span></span>

[!code-cshtml[Main](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/samples/sample1.cshtml)]

## <a name="adding-the-model"></a><span data-ttu-id="8ccf8-131">Добавление модели</span><span class="sxs-lookup"><span data-stu-id="8ccf8-131">Adding the Model</span></span>

<span data-ttu-id="8ccf8-132">В **обозревателе решений**, щелкните правой кнопкой мыши *моделей* выберите **добавить**и нажмите кнопку **класса**.</span><span class="sxs-lookup"><span data-stu-id="8ccf8-132">In **Solution Explorer**, right-click the *Models* folder, select **Add**, and then click **Class**.</span></span>

![Новый класс Mdl пользователя](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/_static/image6.png)

<span data-ttu-id="8ccf8-134">Присвойте классу имя `UserModel`.</span><span class="sxs-lookup"><span data-stu-id="8ccf8-134">Name the class `UserModel`.</span></span> <span data-ttu-id="8ccf8-135">Замените содержимое *UserModel* файла следующим кодом:</span><span class="sxs-lookup"><span data-stu-id="8ccf8-135">Replace the contents of the *UserModel* file with the following code:</span></span>

[!code-csharp[Main](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/samples/sample2.cs)]

<span data-ttu-id="8ccf8-136">`UserModel` Класс представляет пользователей.</span><span class="sxs-lookup"><span data-stu-id="8ccf8-136">The `UserModel` class represents users.</span></span> <span data-ttu-id="8ccf8-137">Каждый член класса помечается с помощью [необходимые](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.requiredattribute.aspx) атрибута из [DataAnnotations](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.aspx) пространства имен.</span><span class="sxs-lookup"><span data-stu-id="8ccf8-137">Each member of the class is annotated with the [Required](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.requiredattribute.aspx) attribute from the [DataAnnotations](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.aspx) namespace.</span></span> <span data-ttu-id="8ccf8-138">Атрибуты в [DataAnnotations](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.aspx) пространства имен обеспечивают проверку автоматического клиента и стороне сервера для веб-приложений.</span><span class="sxs-lookup"><span data-stu-id="8ccf8-138">The attributes in the [DataAnnotations](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.aspx) namespace provide automatic client- and server-side validation for web applications.</span></span>

<span data-ttu-id="8ccf8-139">Откройте `HomeController` и добавьте `using` директив, так что можно получить доступ к `UserModel` и `Users` классы:</span><span class="sxs-lookup"><span data-stu-id="8ccf8-139">Open the `HomeController` class and add a `using` directive so that you can access the `UserModel` and `Users` classes:</span></span>

[!code-csharp[Main](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/samples/sample3.cs)]

<span data-ttu-id="8ccf8-140">Сразу после `HomeController` объявление, добавьте следующий комментарий и ссылку на `Users` класса:</span><span class="sxs-lookup"><span data-stu-id="8ccf8-140">Just after the `HomeController` declaration, add the following comment and the reference to a `Users` class:</span></span>

[!code-csharp[Main](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/samples/sample4.cs)]

<span data-ttu-id="8ccf8-141">`Users` Класса является хранилищем данных упрощенной и в памяти, который будет использоваться в этом учебнике.</span><span class="sxs-lookup"><span data-stu-id="8ccf8-141">The `Users` class is a simplified, in-memory data store that you'll use in this tutorial.</span></span> <span data-ttu-id="8ccf8-142">В реальном приложении используется базы данных для хранения информации о пользователях.</span><span class="sxs-lookup"><span data-stu-id="8ccf8-142">In a real application you would use a database to store user information.</span></span> <span data-ttu-id="8ccf8-143">Первые несколько строк `HomeController` показаны в следующем примере:</span><span class="sxs-lookup"><span data-stu-id="8ccf8-143">The first few lines of the `HomeController` file are shown in the following example:</span></span>

[!code-csharp[Main](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/samples/sample5.cs)]

<span data-ttu-id="8ccf8-144">Постройте приложение, чтобы модель пользователя будут доступны в мастере формирования шаблонов в следующем шаге.</span><span class="sxs-lookup"><span data-stu-id="8ccf8-144">Build the application so that the user model will be available to the scaffolding wizard in the next step.</span></span>

## <a name="creating-the-default-view"></a><span data-ttu-id="8ccf8-145">Создание представления по умолчанию</span><span class="sxs-lookup"><span data-stu-id="8ccf8-145">Creating the Default View</span></span>

<span data-ttu-id="8ccf8-146">Следующим шагом является добавление метода действия и представления для отображения пользователей.</span><span class="sxs-lookup"><span data-stu-id="8ccf8-146">The next step is to add an action method and view to display the users.</span></span>

<span data-ttu-id="8ccf8-147">Удалите существующую *Views\Home\Index* файла.</span><span class="sxs-lookup"><span data-stu-id="8ccf8-147">Delete the existing *Views\Home\Index* file.</span></span> <span data-ttu-id="8ccf8-148">Будет создана новая *индекс* файл для отображения пользователей.</span><span class="sxs-lookup"><span data-stu-id="8ccf8-148">You will create a new *Index* file to display the users.</span></span>

<span data-ttu-id="8ccf8-149">В `HomeController` класса, замените содержимое `Index` метод следующим кодом:</span><span class="sxs-lookup"><span data-stu-id="8ccf8-149">In the `HomeController` class, replace the contents of the `Index` method with the following code:</span></span>

[!code-csharp[Main](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/samples/sample6.cs)]

<span data-ttu-id="8ccf8-150">Щелкните правой кнопкой мыши внутри `Index` метода и нажмите кнопку **добавить представление**.</span><span class="sxs-lookup"><span data-stu-id="8ccf8-150">Right-click inside the `Index` method and then click **Add View**.</span></span>

![Добавить представление](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/_static/image7.png)

<span data-ttu-id="8ccf8-152">Выберите **создать строго типизированное представление** параметр.</span><span class="sxs-lookup"><span data-stu-id="8ccf8-152">Select the **Create a strongly-typed view** option.</span></span> <span data-ttu-id="8ccf8-153">Для **просматривать данные класс**выберите **Mvc3Razor.Models.UserModel**.</span><span class="sxs-lookup"><span data-stu-id="8ccf8-153">For **View data class**, select **Mvc3Razor.Models.UserModel**.</span></span> <span data-ttu-id="8ccf8-154">(Если вы не видите **Mvc3Razor.Models.UserModel** в **просматривать данные класс** поле, необходимое для создания проекта.) Убедитесь, что обработчик представлений присвоено **Razor**.</span><span class="sxs-lookup"><span data-stu-id="8ccf8-154">(If you don't see **Mvc3Razor.Models.UserModel** in the **View data class** box, you need to build the project.) Make sure that the view engine is set to **Razor**.</span></span> <span data-ttu-id="8ccf8-155">Задать **просматривать содержимое** для **списка** и нажмите кнопку **добавить**.</span><span class="sxs-lookup"><span data-stu-id="8ccf8-155">Set **View content** to **List** and then click **Add**.</span></span>

![Добавить представление индекса](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/_static/image8.png)

<span data-ttu-id="8ccf8-157">Новое представление автоматически scaffolds данные пользователя, передаваемое `Index` представления.</span><span class="sxs-lookup"><span data-stu-id="8ccf8-157">The new view automatically scaffolds the user data that's passed to the `Index` view.</span></span> <span data-ttu-id="8ccf8-158">Изучите вновь созданным *Views\Home\Index* файла.</span><span class="sxs-lookup"><span data-stu-id="8ccf8-158">Examine the newly generated *Views\Home\Index* file.</span></span> <span data-ttu-id="8ccf8-159">**Создать новый**, **изменить**, **сведения**, и **удалить** связи не работают, но функционирует остальной части страницы.</span><span class="sxs-lookup"><span data-stu-id="8ccf8-159">The **Create New**, **Edit**, **Details**, and **Delete** links don't work, but the rest of the page is functional.</span></span> <span data-ttu-id="8ccf8-160">Запустите страницу.</span><span class="sxs-lookup"><span data-stu-id="8ccf8-160">Run the page.</span></span> <span data-ttu-id="8ccf8-161">Появится список пользователей.</span><span class="sxs-lookup"><span data-stu-id="8ccf8-161">You see a list of users.</span></span>

![Страница индекса](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/_static/image9.png)

<span data-ttu-id="8ccf8-163">Откройте *Index.cshtml* и замените `ActionLink` разметку для **изменить**, **сведения**, и **удалить** с помощью следующего кода :</span><span class="sxs-lookup"><span data-stu-id="8ccf8-163">Open the *Index.cshtml* file and replace the `ActionLink` markup for **Edit**, **Details**, and **Delete** with the following code:</span></span>

[!code-cshtml[Main](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/samples/sample7.cshtml)]

<span data-ttu-id="8ccf8-164">Имя пользователя используется как идентификатор для найдете выбранную запись в **изменить**, **сведения**, и **удалить** ссылки.</span><span class="sxs-lookup"><span data-stu-id="8ccf8-164">The user name is used as the ID to find the selected record in the **Edit**, **Details**, and **Delete** links.</span></span>

## <a name="creating-the-details-view"></a><span data-ttu-id="8ccf8-165">Создание представления сведений</span><span class="sxs-lookup"><span data-stu-id="8ccf8-165">Creating the Details View</span></span>

<span data-ttu-id="8ccf8-166">Следующим шагом является добавление `Details` метода действия и представления для отображения сведений о пользователе.</span><span class="sxs-lookup"><span data-stu-id="8ccf8-166">The next step is to add a `Details` action method and view in order to display user details.</span></span>

![Подробные сведения](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/_static/image10.png)

<span data-ttu-id="8ccf8-168">Добавьте следующие `Details` метод в контроллер home:</span><span class="sxs-lookup"><span data-stu-id="8ccf8-168">Add the following `Details` method to the home controller:</span></span>

[!code-csharp[Main](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/samples/sample8.cs)]

<span data-ttu-id="8ccf8-169">Щелкните правой кнопкой мыши внутри `Details` метода, а затем выберите <strong>добавить представление</strong>.</span><span class="sxs-lookup"><span data-stu-id="8ccf8-169">Right-click inside the `Details` method and then select <strong>Add View</strong>.</span></span> <span data-ttu-id="8ccf8-170">Убедитесь, что <strong>просматривать данные класс</strong> поле содержит <strong>Mvc3Razor.Models.UserModel</strong><em>.</em></span><span class="sxs-lookup"><span data-stu-id="8ccf8-170">Verify that the <strong>View data class</strong> box contains <strong>Mvc3Razor.Models.UserModel</strong><em>.</em></span></span> <span data-ttu-id="8ccf8-171">Задать <strong>просматривать содержимое</strong> для <strong>сведения</strong> и нажмите кнопку <strong>добавить</strong>.</span><span class="sxs-lookup"><span data-stu-id="8ccf8-171">Set <strong>View content</strong> to <strong>Details</strong> and then click <strong>Add</strong>.</span></span>

![Добавление представления сведений](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/_static/image11.png)

<span data-ttu-id="8ccf8-173">Запустите приложение и выберите ссылку сведения.</span><span class="sxs-lookup"><span data-stu-id="8ccf8-173">Run the application and select a details link.</span></span> <span data-ttu-id="8ccf8-174">Автоматическое формирование шаблонов показана каждого свойства в модели.</span><span class="sxs-lookup"><span data-stu-id="8ccf8-174">The automatic scaffolding shows each property in the model.</span></span>

![Подробные сведения](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/_static/image12.png)

## <a name="creating-the-edit-view"></a><span data-ttu-id="8ccf8-176">Создание представления изменения</span><span class="sxs-lookup"><span data-stu-id="8ccf8-176">Creating the Edit View</span></span>

<span data-ttu-id="8ccf8-177">Добавьте следующие `Edit` метод в контроллер home.</span><span class="sxs-lookup"><span data-stu-id="8ccf8-177">Add the following `Edit` method to the home controller.</span></span>

[!code-csharp[Main](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/samples/sample9.cs)]

<span data-ttu-id="8ccf8-178">Добавление представления, как в предыдущих шагах, но задать **просматривать содержимое** для **изменить**.</span><span class="sxs-lookup"><span data-stu-id="8ccf8-178">Add a view as in the previous steps, but set **View content** to **Edit**.</span></span>

![Добавление представления изменения](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/_static/image13.png)

<span data-ttu-id="8ccf8-180">Запустите приложение и измените имя и фамилия одного из пользователей.</span><span class="sxs-lookup"><span data-stu-id="8ccf8-180">Run the application and edit the first and last name of one of the users.</span></span> <span data-ttu-id="8ccf8-181">Если вы нарушены `DataAnnotation` ограничения, которые были применены к `UserModel` класса, при отправке формы, вы увидите ошибки проверки, созданные с помощью серверного кода.</span><span class="sxs-lookup"><span data-stu-id="8ccf8-181">If you violate any `DataAnnotation` constraints that have been applied to the `UserModel` class, when you submit the form, you will see validation errors that are produced by server code.</span></span> <span data-ttu-id="8ccf8-182">Например, если изменить имя &quot;Анна&quot; для &quot;A&quot;, при отправке формы, в форме отображается следующая ошибка:</span><span class="sxs-lookup"><span data-stu-id="8ccf8-182">For example, if you change the first name &quot;Ann&quot; to &quot;A&quot;, when you submit the form, the following error is displayed on the form:</span></span>

`The field First Name must be a string with a minimum length of 3 and a maximum length of 8.`

<span data-ttu-id="8ccf8-183">В этом учебнике обработке имя пользователя в качестве первичного ключа.</span><span class="sxs-lookup"><span data-stu-id="8ccf8-183">In this tutorial, you're treating the user name as the primary key.</span></span> <span data-ttu-id="8ccf8-184">Таким образом невозможно изменить свойство имени пользователя.</span><span class="sxs-lookup"><span data-stu-id="8ccf8-184">Therefore, the user name property cannot be changed.</span></span> <span data-ttu-id="8ccf8-185">В *Edit.cshtml* файл сразу после `Html.BeginForm` инструкции, задайте имя пользователя в качестве скрытого поля.</span><span class="sxs-lookup"><span data-stu-id="8ccf8-185">In the *Edit.cshtml* file, just after the `Html.BeginForm` statement, set the user name to be a hidden field.</span></span> <span data-ttu-id="8ccf8-186">Это приводит к свойства должны быть переданы в модели.</span><span class="sxs-lookup"><span data-stu-id="8ccf8-186">This causes the property to be passed in the model.</span></span> <span data-ttu-id="8ccf8-187">В следующем фрагменте кода показано размещение `Hidden` инструкции:</span><span class="sxs-lookup"><span data-stu-id="8ccf8-187">The following code fragment shows the placement of the `Hidden` statement:</span></span>

[!code-cshtml[Main](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/samples/sample10.cshtml)]

<span data-ttu-id="8ccf8-188">Замените `TextBoxFor` и `ValidationMessageFor` разметки для имени пользователя с `DisplayFor` вызова.</span><span class="sxs-lookup"><span data-stu-id="8ccf8-188">Replace the `TextBoxFor` and `ValidationMessageFor` markup for the user name with a `DisplayFor` call.</span></span> <span data-ttu-id="8ccf8-189">`DisplayFor` Метод свойство отображается как элемент только для чтения.</span><span class="sxs-lookup"><span data-stu-id="8ccf8-189">The `DisplayFor` method displays the property as a read-only element.</span></span> <span data-ttu-id="8ccf8-190">В следующем примере полная разметка.</span><span class="sxs-lookup"><span data-stu-id="8ccf8-190">The following example shows the completed markup.</span></span> <span data-ttu-id="8ccf8-191">Исходный `TextBoxFor` и `ValidationMessageFor` вызовы закомментированы с символы начала комментария и конец комментария Razor (`@* *@`)</span><span class="sxs-lookup"><span data-stu-id="8ccf8-191">The original `TextBoxFor` and `ValidationMessageFor` calls are commented out with the Razor begin-comment and end-comment characters (`@* *@`)</span></span>

[!code-cshtml[Main](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/samples/sample11.cshtml)]

## <a name="enabling-client-side-validation"></a><span data-ttu-id="8ccf8-192">Включение проверки на стороне клиента</span><span class="sxs-lookup"><span data-stu-id="8ccf8-192">Enabling Client-Side Validation</span></span>

<span data-ttu-id="8ccf8-193">Чтобы включить проверку на стороне клиента в ASP.NET MVC 3, необходимо задать два флага и необходимо включить три файла JavaScript.</span><span class="sxs-lookup"><span data-stu-id="8ccf8-193">To enable client-side validation in ASP.NET MVC 3, you must set two flags and you must include three JavaScript files.</span></span>

<span data-ttu-id="8ccf8-194">Откройте приложение *Web.config* файла.</span><span class="sxs-lookup"><span data-stu-id="8ccf8-194">Open the application's *Web.config* file.</span></span> <span data-ttu-id="8ccf8-195">Проверьте `that ClientValidationEnabled` и `UnobtrusiveJavaScriptEnabled` присвоено значение true, если в параметрах приложения.</span><span class="sxs-lookup"><span data-stu-id="8ccf8-195">Verify `that ClientValidationEnabled` and `UnobtrusiveJavaScriptEnabled` are set to true in the application settings.</span></span> <span data-ttu-id="8ccf8-196">В следующем фрагменте из корня *Web.config* файле отображаются правильные параметры:</span><span class="sxs-lookup"><span data-stu-id="8ccf8-196">The following fragment from the root *Web.config* file shows the correct settings:</span></span>

[!code-xml[Main](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/samples/sample12.xml)]

<span data-ttu-id="8ccf8-197">Параметр `UnobtrusiveJavaScriptEnabled` значение true включает ненавязчивого Ajax и ненавязчивой клиентской проверки.</span><span class="sxs-lookup"><span data-stu-id="8ccf8-197">Setting `UnobtrusiveJavaScriptEnabled` to true enables unobtrusive Ajax and unobtrusive client validation.</span></span> <span data-ttu-id="8ccf8-198">При использовании ненавязчивой проверки правил проверки, преобразуются в атрибуты HTML5.</span><span class="sxs-lookup"><span data-stu-id="8ccf8-198">When you use unobtrusive validation, the validation rules are turned into HTML5 attributes.</span></span> <span data-ttu-id="8ccf8-199">Имена атрибутов HTML5 могут включать только строчные буквы, цифры и дефисы.</span><span class="sxs-lookup"><span data-stu-id="8ccf8-199">HTML5 attribute names can consist of only lowercase letters, numbers, and dashes.</span></span>

<span data-ttu-id="8ccf8-200">Параметр `ClientValidationEnabled` на true включает клиентскую проверку.</span><span class="sxs-lookup"><span data-stu-id="8ccf8-200">Setting `ClientValidationEnabled` to true enables client-side validation.</span></span> <span data-ttu-id="8ccf8-201">Можно установить эти ключи в приложение *Web.config* файл включена клиентская проверка и ненавязчивый JavaScript для всего приложения.</span><span class="sxs-lookup"><span data-stu-id="8ccf8-201">By setting these keys in the application *Web.config* file, you enable client validation and unobtrusive JavaScript for the entire application.</span></span> <span data-ttu-id="8ccf8-202">Кроме того, можно включить или отключить эти параметры в отдельных представлений или методы контроллера, используя следующий код:</span><span class="sxs-lookup"><span data-stu-id="8ccf8-202">You can also enable or disable these settings in individual views or in controller methods using the following code:</span></span>

[!code-csharp[Main](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/samples/sample13.cs)]

<span data-ttu-id="8ccf8-203">Необходимо также включить несколько файлов JavaScript в режиме отображения.</span><span class="sxs-lookup"><span data-stu-id="8ccf8-203">You also need to include several JavaScript files in the rendered view.</span></span> <span data-ttu-id="8ccf8-204">Является простой способ включения JavaScript во всех представлениях, чтобы добавить их в *представления\общие\\_Layout.cshtml* файла.</span><span class="sxs-lookup"><span data-stu-id="8ccf8-204">An easy way to include the JavaScript in all views is to add them to the *Views\Shared\\_Layout.cshtml* file.</span></span> <span data-ttu-id="8ccf8-205">Замените `<head>` элемент  *\_Layout.cshtml* файла следующим кодом:</span><span class="sxs-lookup"><span data-stu-id="8ccf8-205">Replace the `<head>` element of the *\_Layout.cshtml* file with the following code:</span></span>

[!code-cshtml[Main](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/samples/sample14.cshtml)]

<span data-ttu-id="8ccf8-206">Первые два скриптов jQuery размещаются с Microsoft Ajax доставки содержимого сети (CDN).</span><span class="sxs-lookup"><span data-stu-id="8ccf8-206">The first two jQuery scripts are hosted by the Microsoft Ajax Content Delivery Network (CDN).</span></span> <span data-ttu-id="8ccf8-207">Используя преимущества сети Microsoft Ajax CDN, может значительно повысить первого нажатия производительность приложений.</span><span class="sxs-lookup"><span data-stu-id="8ccf8-207">By taking advantage of the Microsoft Ajax CDN, you can significantly improve the first-hit performance of your applications.</span></span>

<span data-ttu-id="8ccf8-208">Запустите приложение и щелкните ссылку Изменить.</span><span class="sxs-lookup"><span data-stu-id="8ccf8-208">Run the application and click an edit link.</span></span> <span data-ttu-id="8ccf8-209">Просмотрите исходный код страницы в браузере.</span><span class="sxs-lookup"><span data-stu-id="8ccf8-209">View the page's source in the browser.</span></span> <span data-ttu-id="8ccf8-210">Исходный браузера содержит множество атрибутов формы `data-val` (для проверки данных).</span><span class="sxs-lookup"><span data-stu-id="8ccf8-210">The browser source shows many attributes of the form `data-val` (for data validation).</span></span> <span data-ttu-id="8ccf8-211">Если включена клиентская проверка и ненавязчивый JavaScript, содержат поля ввода с помощью правила клиентской проверки `data-val="true"` атрибут для запуска ненавязчивой клиентской проверки.</span><span class="sxs-lookup"><span data-stu-id="8ccf8-211">When client validation and unobtrusive JavaScript is enabled, input fields with a client-validation rule contain the `data-val="true"` attribute to trigger unobtrusive client validation.</span></span> <span data-ttu-id="8ccf8-212">Например `City` декорированных поля в модели [необходимые](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.requiredattribute.aspx) атрибут, который приводит к HTML-код, показанный в следующем примере:</span><span class="sxs-lookup"><span data-stu-id="8ccf8-212">For example, the `City` field in the model was decorated with the [Required](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.requiredattribute.aspx) attribute, which results in the HTML shown in the following example:</span></span>

[!code-cshtml[Main](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/samples/sample15.cshtml)]

<span data-ttu-id="8ccf8-213">Для каждого правила клиентской проверки добавить атрибут, который имеет форму `data-val-rulename="message"`.</span><span class="sxs-lookup"><span data-stu-id="8ccf8-213">For each client-validation rule, an attribute is added that has the form `data-val-rulename="message"`.</span></span> <span data-ttu-id="8ccf8-214">С помощью `City` поле пример, приведенный выше, правило требуется проверка клиента создает `data-val-required` атрибут и сообщение &quot;поле «Город» является обязательным&quot;.</span><span class="sxs-lookup"><span data-stu-id="8ccf8-214">Using the `City` field example shown earlier, the required client-validation rule generates the `data-val-required` attribute and the message &quot;The City field is required&quot;.</span></span> <span data-ttu-id="8ccf8-215">Запустите приложение, изменить один из пользователей, а затем снимите `City` поля.</span><span class="sxs-lookup"><span data-stu-id="8ccf8-215">Run the application, edit one of the users, and clear the `City` field.</span></span> <span data-ttu-id="8ccf8-216">При нажатии клавиши tab поле, появится сообщение об ошибке проверки на стороне клиента.</span><span class="sxs-lookup"><span data-stu-id="8ccf8-216">When you tab out of the field, you see a client-side validation error message.</span></span>

![Требуется Город](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/_static/image14.png)

<span data-ttu-id="8ccf8-218">Аналогичным образом, для каждого параметра в правила клиентской проверки, добавляется атрибут, имеет форму `data-val-rulename-paramname=paramvalue`.</span><span class="sxs-lookup"><span data-stu-id="8ccf8-218">Similarly, for each parameter in the client-validation rule, an attribute is added that has the form `data-val-rulename-paramname=paramvalue`.</span></span> <span data-ttu-id="8ccf8-219">Например `FirstName` свойство помечается с помощью [StringLength](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.stringlengthattribute.aspx) атрибут и указывает длину не менее 3 и максимальной длиной 8.</span><span class="sxs-lookup"><span data-stu-id="8ccf8-219">For example, the `FirstName` property is annotated with the [StringLength](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.stringlengthattribute.aspx) attribute and specifies a minimum length of 3 and a maximum length of 8.</span></span> <span data-ttu-id="8ccf8-220">Правила проверки данных с именем `length` имеет имя параметра `max` и значение параметра 8.</span><span class="sxs-lookup"><span data-stu-id="8ccf8-220">The data validation rule named `length` has the parameter name `max` and the parameter value 8.</span></span> <span data-ttu-id="8ccf8-221">В следующем примере показаны HTML, который создается для `FirstName` поля при изменении одного из пользователей:</span><span class="sxs-lookup"><span data-stu-id="8ccf8-221">The following shows the HTML that is generated for the `FirstName` field when you edit one of the users:</span></span>

[!code-cshtml[Main](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/samples/sample16.cshtml)]

<span data-ttu-id="8ccf8-222">Дополнительные сведения о ненавязчивой клиентской проверки, см. в записи [Ненавязчивая клиентская проверка в ASP.NET MVC 3](http://bradwilson.typepad.com/blog/2010/10/mvc3-unobtrusive-validation.html) в блоге Брэда Уилсона.</span><span class="sxs-lookup"><span data-stu-id="8ccf8-222">For more information about unobtrusive client validation, see the entry [Unobtrusive Client Validation in ASP.NET MVC 3](http://bradwilson.typepad.com/blog/2010/10/mvc3-unobtrusive-validation.html) in Brad Wilson's blog.</span></span>

> [!NOTE]
> <span data-ttu-id="8ccf8-223">В ASP.NET MVC 3 бета-версии иногда необходимо для отправки формы, чтобы запустить проверку на стороне клиента.</span><span class="sxs-lookup"><span data-stu-id="8ccf8-223">In ASP.NET MVC 3 Beta, you sometimes need to submit the form in order to start client-side validation.</span></span> <span data-ttu-id="8ccf8-224">Это может быть изменено в окончательной версии.</span><span class="sxs-lookup"><span data-stu-id="8ccf8-224">This might be changed for the final release.</span></span>


## <a name="creating-the-create-view"></a><span data-ttu-id="8ccf8-225">Создание представления создания</span><span class="sxs-lookup"><span data-stu-id="8ccf8-225">Creating the Create View</span></span>

<span data-ttu-id="8ccf8-226">Следующим шагом является добавление `Create` метода действия и представление, чтобы пользователь мог создать нового пользователя.</span><span class="sxs-lookup"><span data-stu-id="8ccf8-226">The next step is to add a `Create` action method and view in order to enable the user to create a new user.</span></span> <span data-ttu-id="8ccf8-227">Добавьте следующие `Create` метод в контроллер home:</span><span class="sxs-lookup"><span data-stu-id="8ccf8-227">Add the following `Create` method to the home controller:</span></span>

[!code-csharp[Main](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/samples/sample17.cs)]

<span data-ttu-id="8ccf8-228">Добавление представления, как в предыдущих шагах, но задать **просматривать содержимое** для **создать**.</span><span class="sxs-lookup"><span data-stu-id="8ccf8-228">Add a view as in the previous steps, but set **View content** to **Create**.</span></span>

![Создать просмотр](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/_static/image15.png)

<span data-ttu-id="8ccf8-230">Запустите приложение, выберите **создать** связать, и добавить нового пользователя.</span><span class="sxs-lookup"><span data-stu-id="8ccf8-230">Run the application, select the **Create** link, and add a new user.</span></span> <span data-ttu-id="8ccf8-231">`Create` Метод автоматически использует преимущества проверки на стороне клиента и на стороне сервера.</span><span class="sxs-lookup"><span data-stu-id="8ccf8-231">The `Create` method automatically takes advantage of client-side and server-side validation.</span></span> <span data-ttu-id="8ccf8-232">Введите имя пользователя, которое содержит пробел, таких как &quot;Бен X&quot;.</span><span class="sxs-lookup"><span data-stu-id="8ccf8-232">Try to enter a user name that contains white space, such as &quot;Ben X&quot;.</span></span> <span data-ttu-id="8ccf8-233">При нажатии клавиши tab поля имени пользователя, ошибки проверки на стороне клиента (`White space is not allowed`) отображается.</span><span class="sxs-lookup"><span data-stu-id="8ccf8-233">When you tab out of the user name field, a client-side validation error (`White space is not allowed`) is displayed.</span></span>

## <a name="add-the-delete-method"></a><span data-ttu-id="8ccf8-234">Добавьте метод Delete</span><span class="sxs-lookup"><span data-stu-id="8ccf8-234">Add the Delete method</span></span>

<span data-ttu-id="8ccf8-235">Для прохождения этого учебника, добавьте следующий `Delete` метод в контроллер home:</span><span class="sxs-lookup"><span data-stu-id="8ccf8-235">To complete the tutorial, add the following `Delete` method to the home controller:</span></span>

[!code-csharp[Main](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/samples/sample18.cs)]

<span data-ttu-id="8ccf8-236">Добавить `Delete` представление, как показано выше, установка **просматривать содержимое** для **удалить**.</span><span class="sxs-lookup"><span data-stu-id="8ccf8-236">Add a `Delete` view as in the previous steps, setting **View content** to **Delete**.</span></span>

![Удаление представления](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/_static/image16.png)

<span data-ttu-id="8ccf8-238">Теперь у вас есть простой, но полнофункциональные приложения ASP.NET MVC 3 с проверкой.</span><span class="sxs-lookup"><span data-stu-id="8ccf8-238">You now have a simple but fully functional ASP.NET MVC 3 application with validation.</span></span>
