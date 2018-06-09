---
uid: identity/overview/extensibility/change-primary-key-for-users-in-aspnet-identity
title: Изменение первичного ключа для пользователей в ASP.NET Identity | Документы Microsoft
author: tfitzmac
description: В Visual Studio 2013 веб-приложения по умолчанию использует строковое значение для ключа для учетных записей пользователей. ASP.NET Identity позволяет изменить тип...
ms.author: aspnetcontent
manager: wpickett
ms.date: 09/30/2014
ms.topic: article
ms.assetid: 44925849-5762-4504-a8cd-8f0cd06f6dc3
ms.technology: ''
ms.prod: .net-framework
msc.legacyurl: /identity/overview/extensibility/change-primary-key-for-users-in-aspnet-identity
msc.type: authoredcontent
ms.openlocfilehash: 79812efb4de2461fad3765d6005bbd20393e62b2
ms.sourcegitcommit: 6784510cfb589308c3875ccb5113eb31031766b4
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 06/08/2018
ms.locfileid: "26498233"
---
<a name="change-primary-key-for-users-in-aspnet-identity"></a><span data-ttu-id="7bb0b-104">Изменение первичного ключа для пользователей в ASP.NET Identity</span><span class="sxs-lookup"><span data-stu-id="7bb0b-104">Change Primary Key for Users in ASP.NET Identity</span></span>
====================
<span data-ttu-id="7bb0b-105">по [Tom FitzMacken](https://github.com/tfitzmac)</span><span class="sxs-lookup"><span data-stu-id="7bb0b-105">by [Tom FitzMacken](https://github.com/tfitzmac)</span></span>

> <span data-ttu-id="7bb0b-106">В Visual Studio 2013 веб-приложения по умолчанию использует строковое значение для ключа для учетных записей пользователей.</span><span class="sxs-lookup"><span data-stu-id="7bb0b-106">In Visual Studio 2013, the default web application uses a string value for the key for user accounts.</span></span> <span data-ttu-id="7bb0b-107">ASP.NET Identity позволяет изменить тип ключа для соответствия требованиям данных.</span><span class="sxs-lookup"><span data-stu-id="7bb0b-107">ASP.NET Identity enables you to change the type of the key to meet your data requirements.</span></span> <span data-ttu-id="7bb0b-108">Например можно изменить тип ключа из строки в целое число.</span><span class="sxs-lookup"><span data-stu-id="7bb0b-108">For example, you can change the type of the key from a string to an integer.</span></span>
> 
> <span data-ttu-id="7bb0b-109">В этом разделе показано, как со веб-приложения по умолчанию и изменить на целочисленный ключ учетной записи пользователя.</span><span class="sxs-lookup"><span data-stu-id="7bb0b-109">This topic shows how to start with the default web application and change the user account key to an integer.</span></span> <span data-ttu-id="7bb0b-110">Те же изменения можно использовать для реализации любого типа ключа в своем проекте.</span><span class="sxs-lookup"><span data-stu-id="7bb0b-110">You can use the same modifications to implement any type of key in your project.</span></span> <span data-ttu-id="7bb0b-111">Показано, как внести эти изменения в веб-приложения по умолчанию, но можно применить аналогичные изменения настраиваемого приложения.</span><span class="sxs-lookup"><span data-stu-id="7bb0b-111">It shows how to make these changes in the default web application, but you could apply similar modifications to a customized application.</span></span> <span data-ttu-id="7bb0b-112">Он показывает изменения, необходимые при работе с MVC или веб-форм.</span><span class="sxs-lookup"><span data-stu-id="7bb0b-112">It shows the changes needed when working with MVC or Web Forms.</span></span>
> 
> ## <a name="software-versions-used-in-the-tutorial"></a><span data-ttu-id="7bb0b-113">Версии программного обеспечения, используемая в этом учебнике</span><span class="sxs-lookup"><span data-stu-id="7bb0b-113">Software versions used in the tutorial</span></span>
> 
> 
> - <span data-ttu-id="7bb0b-114">Visual Studio 2013 с обновлением 2 (или более поздней версии)</span><span class="sxs-lookup"><span data-stu-id="7bb0b-114">Visual Studio 2013 with Update 2 (or later)</span></span>
> - <span data-ttu-id="7bb0b-115">ASP.NET Identity 2.1 или более поздней версии</span><span class="sxs-lookup"><span data-stu-id="7bb0b-115">ASP.NET Identity 2.1 or later</span></span>


<span data-ttu-id="7bb0b-116">Для выполнения шагов в этом учебнике, необходимо иметь Visual Studio 2013 с обновлением 2 (или более поздней версии) и веб-приложения, созданные на основе шаблона веб-приложения ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="7bb0b-116">To perform the steps in this tutorial, you must have Visual Studio 2013 Update 2 (or later), and a web application created from the ASP.NET Web Application template.</span></span> <span data-ttu-id="7bb0b-117">Шаблон изменения в обновлении 3.</span><span class="sxs-lookup"><span data-stu-id="7bb0b-117">The template changed in Update 3.</span></span> <span data-ttu-id="7bb0b-118">В этом разделе показано, как изменить шаблон в обновлении 2 и обновлением 3.</span><span class="sxs-lookup"><span data-stu-id="7bb0b-118">This topic shows how to change the template in Update 2 and Update 3.</span></span>

<span data-ttu-id="7bb0b-119">В этом разделе содержатся следующие подразделы.</span><span class="sxs-lookup"><span data-stu-id="7bb0b-119">This topic contains the following sections:</span></span>

- [<span data-ttu-id="7bb0b-120">Измените тип ключа в пользовательском классе удостоверений</span><span class="sxs-lookup"><span data-stu-id="7bb0b-120">Change the type of the key in the Identity user class</span></span>](#userclass)
- [<span data-ttu-id="7bb0b-121">Добавить пользовательские классы удостоверений, использующие тип ключа</span><span class="sxs-lookup"><span data-stu-id="7bb0b-121">Add customized Identity classes that use the key type</span></span>](#customclass)
- [<span data-ttu-id="7bb0b-122">Изменение контекста класса и диспетчера пользователей для использования данного типа ключа</span><span class="sxs-lookup"><span data-stu-id="7bb0b-122">Change the context class and user manager to use the key type</span></span>](#context)
- [<span data-ttu-id="7bb0b-123">Изменить конфигурацию при запуске для использования данного типа ключа</span><span class="sxs-lookup"><span data-stu-id="7bb0b-123">Change start-up configuration to use the key type</span></span>](#startup)
- [<span data-ttu-id="7bb0b-124">Измените AccountController для передачи ключа типа MVC с обновлением 2</span><span class="sxs-lookup"><span data-stu-id="7bb0b-124">For MVC with Update 2, change the AccountController to pass the key type</span></span>](#mvcupdate2)
- [<span data-ttu-id="7bb0b-125">Для MVC с обновлением 3 измените AccountController и ManageController для передачи ключа типа</span><span class="sxs-lookup"><span data-stu-id="7bb0b-125">For MVC with Update 3, change the AccountController and ManageController to pass the key type</span></span>](#mvcupdate3)
- [<span data-ttu-id="7bb0b-126">Для веб-формы с обновлением 2 измените учетную запись страницы для передачи ключа типа</span><span class="sxs-lookup"><span data-stu-id="7bb0b-126">For Web Forms with Update 2, change Account pages to pass the key type</span></span>](#webformsupdate2)
- [<span data-ttu-id="7bb0b-127">Для веб-формы с обновлением 3 измените учетную запись страницы для передачи ключа типа</span><span class="sxs-lookup"><span data-stu-id="7bb0b-127">For Web Forms with Update 3, change Account pages to pass the key type</span></span>](#webformsupdate3)
- [<span data-ttu-id="7bb0b-128">Запуск приложения</span><span class="sxs-lookup"><span data-stu-id="7bb0b-128">Run application</span></span>](#run)
- [<span data-ttu-id="7bb0b-129">Другие ресурсы</span><span class="sxs-lookup"><span data-stu-id="7bb0b-129">Other resources</span></span>](#other)

<a id="userclass"></a>
## <a name="change-the-type-of-the-key-in-the-identity-user-class"></a><span data-ttu-id="7bb0b-130">Измените тип ключа в пользовательском классе удостоверений</span><span class="sxs-lookup"><span data-stu-id="7bb0b-130">Change the type of the key in the Identity user class</span></span>

<span data-ttu-id="7bb0b-131">В проекте, созданные на основе шаблона веб-приложения ASP.NET укажите, что класс ApplicationUser использует целое число для ключа для учетных записей пользователей.</span><span class="sxs-lookup"><span data-stu-id="7bb0b-131">In your project created from the ASP.NET Web Application template, specify that the ApplicationUser class uses an integer for the key for user accounts.</span></span> <span data-ttu-id="7bb0b-132">В IdentityModels.cs, измените класс ApplicationUser наследование от IdentityUser, который имеет тип **int** универсального параметра TKey.</span><span class="sxs-lookup"><span data-stu-id="7bb0b-132">In IdentityModels.cs, change the ApplicationUser class to inherit from IdentityUser that has a type of **int** for the TKey generic parameter.</span></span> <span data-ttu-id="7bb0b-133">Можно также передать имена трех настраиваемого класса, который еще не реализован.</span><span class="sxs-lookup"><span data-stu-id="7bb0b-133">You also pass the names of three customized class which you have not implemented yet.</span></span>

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample1.cs?highlight=1-2)]

<span data-ttu-id="7bb0b-134">Тип ключа был изменен, но по умолчанию остальной части приложения по-прежнему принимает ключ является строка.</span><span class="sxs-lookup"><span data-stu-id="7bb0b-134">You have changed the type of the key, but, by default, the rest of the application still assumes the key is a string.</span></span> <span data-ttu-id="7bb0b-135">Необходимо явно указать тип ключа в программный код, предполагающий строка.</span><span class="sxs-lookup"><span data-stu-id="7bb0b-135">You must explicitly indicate the type of the key in code that assumes a string.</span></span>

<span data-ttu-id="7bb0b-136">В **ApplicationUser** класса, измените **GenerateUserIdentityAsync** метод, чтобы включить int, как показано в приведенном ниже выделенного кода.</span><span class="sxs-lookup"><span data-stu-id="7bb0b-136">In the **ApplicationUser** class, change the **GenerateUserIdentityAsync** method to include int, as shown in the highlighted code below.</span></span> <span data-ttu-id="7bb0b-137">Это изменение не требуется для проектов веб-форм с помощью шаблона обновления 3.</span><span class="sxs-lookup"><span data-stu-id="7bb0b-137">This change is not necessary for Web Forms projects with the Update 3 template.</span></span>

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample2.cs?highlight=2)]

<a id="customclass"></a>
## <a name="add-customized-identity-classes-that-use-the-key-type"></a><span data-ttu-id="7bb0b-138">Добавить пользовательские классы удостоверений, использующие тип ключа</span><span class="sxs-lookup"><span data-stu-id="7bb0b-138">Add customized Identity classes that use the key type</span></span>

<span data-ttu-id="7bb0b-139">И другие классы удостоверений, например IdentityUserRole, IdentityUserClaim, IdentityUserLogin, IdentityRole, UserStore, RoleStore, по-прежнему настраиваются для использования ключа строки.</span><span class="sxs-lookup"><span data-stu-id="7bb0b-139">The other Identity classes, such as IdentityUserRole, IdentityUserClaim, IdentityUserLogin, IdentityRole, UserStore, RoleStore, are still set up to use a string key.</span></span> <span data-ttu-id="7bb0b-140">Создание новых версий этих классов, укажите целочисленное значение для ключа.</span><span class="sxs-lookup"><span data-stu-id="7bb0b-140">Create new versions of these classes that specify an integer for the key.</span></span> <span data-ttu-id="7bb0b-141">Необходимо предоставить объема кода реализации в этих классах, в основном так же установке int как ключ.</span><span class="sxs-lookup"><span data-stu-id="7bb0b-141">You do not need to provide much implementation code in these classes, you are primarily just setting int as the key.</span></span>

<span data-ttu-id="7bb0b-142">Добавьте следующие классы в файл IdentityModels.cs.</span><span class="sxs-lookup"><span data-stu-id="7bb0b-142">Add the following classes to your IdentityModels.cs file.</span></span>

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample3.cs)]

<a id="context"></a>
## <a name="change-the-context-class-and-user-manager-to-use-the-key-type"></a><span data-ttu-id="7bb0b-143">Изменение контекста класса и диспетчера пользователей для использования данного типа ключа</span><span class="sxs-lookup"><span data-stu-id="7bb0b-143">Change the context class and user manager to use the key type</span></span>

<span data-ttu-id="7bb0b-144">В IdentityModels.cs, измените определение **ApplicationDbContext** класс, чтобы использовать новый настроить классы и **int** для ключа, как показано в выделенный код.</span><span class="sxs-lookup"><span data-stu-id="7bb0b-144">In IdentityModels.cs, change the definition of the **ApplicationDbContext** class to use your new customized classes and an **int** for the key, as shown in the highlighted code.</span></span>

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample4.cs?highlight=1-2)]

<span data-ttu-id="7bb0b-145">Параметр ThrowIfV1Schema больше не является допустимым в конструкторе.</span><span class="sxs-lookup"><span data-stu-id="7bb0b-145">The ThrowIfV1Schema parameter is no longer valid in the constructor.</span></span> <span data-ttu-id="7bb0b-146">Измените конструктор, чтобы он не передает значение ThrowIfV1Schema.</span><span class="sxs-lookup"><span data-stu-id="7bb0b-146">Change the constructor so it does not pass a ThrowIfV1Schema value.</span></span>

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample5.cs)]

<span data-ttu-id="7bb0b-147">Откройте IdentityConfig.cs и измените **ApplicationUserManger** класс для использования нового пользователя хранилища класса для сохранения данных и **int** для ключа.</span><span class="sxs-lookup"><span data-stu-id="7bb0b-147">Open IdentityConfig.cs, and change the **ApplicationUserManger** class to use your new user store class for persisting data and an **int** for the key.</span></span>

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample6.cs?highlight=1,3,12,14,32,37,48)]

<span data-ttu-id="7bb0b-148">В шаблоне с обновлением 3 необходимо изменить класс ApplicationSignInManager.</span><span class="sxs-lookup"><span data-stu-id="7bb0b-148">In the Update 3 template, you must change the ApplicationSignInManager class.</span></span>

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample7.cs?highlight=1)]

<a id="startup"></a>
## <a name="change-start-up-configuration-to-use-the-key-type"></a><span data-ttu-id="7bb0b-149">Изменить конфигурацию при запуске для использования данного типа ключа</span><span class="sxs-lookup"><span data-stu-id="7bb0b-149">Change start-up configuration to use the key type</span></span>

<span data-ttu-id="7bb0b-150">В Startup.Auth.cs замените код OnValidateIdentity, как показано ниже.</span><span class="sxs-lookup"><span data-stu-id="7bb0b-150">In Startup.Auth.cs, replace the OnValidateIdentity code, as highlighted below.</span></span> <span data-ttu-id="7bb0b-151">Обратите внимание, что определение getUserIdCallback анализирует строковое значение в целое число.</span><span class="sxs-lookup"><span data-stu-id="7bb0b-151">Notice that the getUserIdCallback definition, parses the string value into an integer.</span></span>

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample8.cs?highlight=7-12)]

<span data-ttu-id="7bb0b-152">Если проект не распознает Универсальная реализация **GetUserId** метод, может потребоваться обновление до версии 2.1 пакета ASP.NET Identity NuGet</span><span class="sxs-lookup"><span data-stu-id="7bb0b-152">If your project does not recognize the generic implementation of the **GetUserId** method, you may need to update the ASP.NET Identity NuGet package to version 2.1</span></span>

<span data-ttu-id="7bb0b-153">Много изменений, внесенные в классы инфраструктуры, используемые в ASP.NET Identity.</span><span class="sxs-lookup"><span data-stu-id="7bb0b-153">You have made a lot of changes to the infrastructure classes used by ASP.NET Identity.</span></span> <span data-ttu-id="7bb0b-154">Если компиляция проекта, вы заметите, что большое число ошибок.</span><span class="sxs-lookup"><span data-stu-id="7bb0b-154">If you try compiling the project, you will notice a lot of errors.</span></span> <span data-ttu-id="7bb0b-155">К счастью оставшихся ошибок всех похожи.</span><span class="sxs-lookup"><span data-stu-id="7bb0b-155">Fortunately, the remaining errors are all similar.</span></span> <span data-ttu-id="7bb0b-156">Класс идентификатора ожидает целое число для ключа, но контроллера (или веб-формы) передается строковое значение.</span><span class="sxs-lookup"><span data-stu-id="7bb0b-156">The Identity class expects an integer for the key, but the controller (or Web Form) is passing a string value.</span></span> <span data-ttu-id="7bb0b-157">В каждом случае необходимо преобразовать в строку и целое число путем вызова **GetUserId&lt;int&gt;**.</span><span class="sxs-lookup"><span data-stu-id="7bb0b-157">In each case, you need to convert from a string to and integer by calling **GetUserId&lt;int&gt;**.</span></span> <span data-ttu-id="7bb0b-158">Можно работать через список ошибок из компиляции или выполните ниже изменения.</span><span class="sxs-lookup"><span data-stu-id="7bb0b-158">You can either work through the error list from compilation or follow the changes below.</span></span>

<span data-ttu-id="7bb0b-159">Оставшиеся изменения зависят от типа проекта, вы создаете и какие обновления установки в Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="7bb0b-159">The remaining changes depend on the type of project you are creating and which update you have installed in Visual Studio.</span></span> <span data-ttu-id="7bb0b-160">Можно перейти непосредственно к соответствующий раздел по ссылке</span><span class="sxs-lookup"><span data-stu-id="7bb0b-160">You can go directly to the relevant section through the following links</span></span>

- [<span data-ttu-id="7bb0b-161">Измените AccountController для передачи ключа типа MVC с обновлением 2</span><span class="sxs-lookup"><span data-stu-id="7bb0b-161">For MVC with Update 2, change the AccountController to pass the key type</span></span>](#mvcupdate2)
- [<span data-ttu-id="7bb0b-162">Для MVC с обновлением 3 измените AccountController и ManageController для передачи ключа типа</span><span class="sxs-lookup"><span data-stu-id="7bb0b-162">For MVC with Update 3, change the AccountController and ManageController to pass the key type</span></span>](#mvcupdate3)
- [<span data-ttu-id="7bb0b-163">Для веб-формы с обновлением 2 измените учетную запись страницы для передачи ключа типа</span><span class="sxs-lookup"><span data-stu-id="7bb0b-163">For Web Forms with Update 2, change Account pages to pass the key type</span></span>](#webformsupdate2)
- [<span data-ttu-id="7bb0b-164">Для веб-формы с обновлением 3 измените учетную запись страницы для передачи ключа типа</span><span class="sxs-lookup"><span data-stu-id="7bb0b-164">For Web Forms with Update 3, change Account pages to pass the key type</span></span>](#webformsupdate3)

<a id="mvcupdate2"></a>
## <a name="for-mvc-with-update-2-change-the-accountcontroller-to-pass-the-key-type"></a><span data-ttu-id="7bb0b-165">Измените AccountController для передачи ключа типа MVC с обновлением 2</span><span class="sxs-lookup"><span data-stu-id="7bb0b-165">For MVC with Update 2, change the AccountController to pass the key type</span></span>

<span data-ttu-id="7bb0b-166">Откройте файл AccountController.cs.</span><span class="sxs-lookup"><span data-stu-id="7bb0b-166">Open the AccountController.cs file.</span></span> <span data-ttu-id="7bb0b-167">Необходимо изменить следующие методы.</span><span class="sxs-lookup"><span data-stu-id="7bb0b-167">You need to change the following methods.</span></span>

<span data-ttu-id="7bb0b-168">**ConfirmEmail** метод</span><span class="sxs-lookup"><span data-stu-id="7bb0b-168">**ConfirmEmail** method</span></span>

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample9.cs?highlight=1,3)]

<span data-ttu-id="7bb0b-169">**Отмените регистрацию** метод</span><span class="sxs-lookup"><span data-stu-id="7bb0b-169">**Disassociate** method</span></span>

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample10.cs?highlight=5,9)]

<span data-ttu-id="7bb0b-170">**Manage(ManageUserViewModel)** метод</span><span class="sxs-lookup"><span data-stu-id="7bb0b-170">**Manage(ManageUserViewModel)** method</span></span>

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample11.cs?highlight=11,17,41)]

<span data-ttu-id="7bb0b-171">**LinkLoginCallback** метод</span><span class="sxs-lookup"><span data-stu-id="7bb0b-171">**LinkLoginCallback** method</span></span>

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample12.cs?highlight=10)]

<span data-ttu-id="7bb0b-172">**RemoveAccountList** метод</span><span class="sxs-lookup"><span data-stu-id="7bb0b-172">**RemoveAccountList** method</span></span>

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample13.cs?highlight=3)]

<span data-ttu-id="7bb0b-173">**HasPassword** метод</span><span class="sxs-lookup"><span data-stu-id="7bb0b-173">**HasPassword** method</span></span>

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample14.cs?highlight=3)]

<span data-ttu-id="7bb0b-174">Теперь вы можете [запустите приложение](#run) и регистрации нового пользователя.</span><span class="sxs-lookup"><span data-stu-id="7bb0b-174">You can now [run the application](#run) and register a new user.</span></span>

<a id="mvcupdate3"></a>
## <a name="for-mvc-with-update-3-change-the-accountcontroller-and-managecontroller-to-pass-the-key-type"></a><span data-ttu-id="7bb0b-175">Для MVC с обновлением 3 измените AccountController и ManageController для передачи ключа типа</span><span class="sxs-lookup"><span data-stu-id="7bb0b-175">For MVC with Update 3, change the AccountController and ManageController to pass the key type</span></span>

<span data-ttu-id="7bb0b-176">Откройте файл AccountController.cs.</span><span class="sxs-lookup"><span data-stu-id="7bb0b-176">Open the AccountController.cs file.</span></span> <span data-ttu-id="7bb0b-177">Необходимо изменить следующий метод.</span><span class="sxs-lookup"><span data-stu-id="7bb0b-177">You need to change the following method.</span></span>

<span data-ttu-id="7bb0b-178">**ConfirmEmail** метод</span><span class="sxs-lookup"><span data-stu-id="7bb0b-178">**ConfirmEmail** method</span></span>

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample15.cs?highlight=1,3)]

<span data-ttu-id="7bb0b-179">**SendCode** метод</span><span class="sxs-lookup"><span data-stu-id="7bb0b-179">**SendCode** method</span></span>

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample16.cs?highlight=4)]

<span data-ttu-id="7bb0b-180">Откройте файл ManageController.cs.</span><span class="sxs-lookup"><span data-stu-id="7bb0b-180">Open the ManageController.cs file.</span></span> <span data-ttu-id="7bb0b-181">Необходимо изменить следующие методы.</span><span class="sxs-lookup"><span data-stu-id="7bb0b-181">You need to change the following methods.</span></span>

<span data-ttu-id="7bb0b-182">**Индекс** метод</span><span class="sxs-lookup"><span data-stu-id="7bb0b-182">**Index** method</span></span>

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample17.cs?highlight=15-17)]

<span data-ttu-id="7bb0b-183">**RemoveLogin** методы</span><span class="sxs-lookup"><span data-stu-id="7bb0b-183">**RemoveLogin** methods</span></span>

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample18.cs?highlight=3,13,17)]

<span data-ttu-id="7bb0b-184">**AddPhoneNumber** метод</span><span class="sxs-lookup"><span data-stu-id="7bb0b-184">**AddPhoneNumber** method</span></span>

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample19.cs?highlight=9)]

<span data-ttu-id="7bb0b-185">**EnableTwoFactorAuthentication** метод</span><span class="sxs-lookup"><span data-stu-id="7bb0b-185">**EnableTwoFactorAuthentication** method</span></span>

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample20.cs?highlight=3-4)]

<span data-ttu-id="7bb0b-186">**DisableTwoFactorAuthentication** метод</span><span class="sxs-lookup"><span data-stu-id="7bb0b-186">**DisableTwoFactorAuthentication** method</span></span>

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample21.cs?highlight=3-4)]

<span data-ttu-id="7bb0b-187">**VerifyPhoneNumber** методы</span><span class="sxs-lookup"><span data-stu-id="7bb0b-187">**VerifyPhoneNumber** methods</span></span>

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample22.cs?highlight=4,18,21)]

<span data-ttu-id="7bb0b-188">**RemovePhoneNumber** метод</span><span class="sxs-lookup"><span data-stu-id="7bb0b-188">**RemovePhoneNumber** method</span></span>

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample23.cs?highlight=3,8)]

<span data-ttu-id="7bb0b-189">**ChangePassword** метод</span><span class="sxs-lookup"><span data-stu-id="7bb0b-189">**ChangePassword** method</span></span>

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample24.cs?highlight=10,13)]

<span data-ttu-id="7bb0b-190">**SetPassword** метод</span><span class="sxs-lookup"><span data-stu-id="7bb0b-190">**SetPassword** method</span></span>

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample25.cs?highlight=5,8)]

<span data-ttu-id="7bb0b-191">**ManageLogins** метод</span><span class="sxs-lookup"><span data-stu-id="7bb0b-191">**ManageLogins** method</span></span>

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample26.cs?highlight=7,12)]

<span data-ttu-id="7bb0b-192">**LinkLoginCallback** метод</span><span class="sxs-lookup"><span data-stu-id="7bb0b-192">**LinkLoginCallback** method</span></span>

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample27.cs?highlight=8)]

<span data-ttu-id="7bb0b-193">**HasPassword** метод</span><span class="sxs-lookup"><span data-stu-id="7bb0b-193">**HasPassword** method</span></span>

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample28.cs?highlight=3)]

<span data-ttu-id="7bb0b-194">**HasPhoneNumber** метод</span><span class="sxs-lookup"><span data-stu-id="7bb0b-194">**HasPhoneNumber** method</span></span>

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample29.cs?highlight=3)]

<span data-ttu-id="7bb0b-195">Теперь вы можете [запустите приложение](#run) и регистрации нового пользователя.</span><span class="sxs-lookup"><span data-stu-id="7bb0b-195">You can now [run the application](#run) and register a new user.</span></span>

<a id="webformsupdate2"></a>
## <a name="for-web-forms-with-update-2-change-account-pages-to-pass-the-key-type"></a><span data-ttu-id="7bb0b-196">Для веб-формы с обновлением 2 измените учетную запись страницы для передачи ключа типа</span><span class="sxs-lookup"><span data-stu-id="7bb0b-196">For Web Forms with Update 2, change Account pages to pass the key type</span></span>

<span data-ttu-id="7bb0b-197">Для веб-формы с обновлением 2 необходимо изменить на следующих страницах.</span><span class="sxs-lookup"><span data-stu-id="7bb0b-197">For Web Forms with Update 2, you need to change the following pages.</span></span>

<span data-ttu-id="7bb0b-198">**Confirm.aspx.CX**</span><span class="sxs-lookup"><span data-stu-id="7bb0b-198">**Confirm.aspx.cx**</span></span>

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample30.cs?highlight=8)]

<span data-ttu-id="7bb0b-199">**RegisterExternalLogin.aspx.cs**</span><span class="sxs-lookup"><span data-stu-id="7bb0b-199">**RegisterExternalLogin.aspx.cs**</span></span>

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample31.cs?highlight=36)]

<span data-ttu-id="7bb0b-200">**Manage.aspx.cs**</span><span class="sxs-lookup"><span data-stu-id="7bb0b-200">**Manage.aspx.cs**</span></span>

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample32.cs?highlight=3,22,47,52,69,85,93,98)]

<span data-ttu-id="7bb0b-201">Теперь вы можете [запустите приложение](#run) и регистрации нового пользователя.</span><span class="sxs-lookup"><span data-stu-id="7bb0b-201">You can now [run the application](#run) and register a new user.</span></span>

<a id="webformsupdate3"></a>
## <a name="for-web-forms-with-update-3-change-account-pages-to-pass-the-key-type"></a><span data-ttu-id="7bb0b-202">Для веб-формы с обновлением 3 измените учетную запись страницы для передачи ключа типа</span><span class="sxs-lookup"><span data-stu-id="7bb0b-202">For Web Forms with Update 3, change Account pages to pass the key type</span></span>

<span data-ttu-id="7bb0b-203">Для веб-формы с обновлением 3 необходимо изменить на следующих страницах.</span><span class="sxs-lookup"><span data-stu-id="7bb0b-203">For Web Forms with Update 3, you need to change the following pages.</span></span>

<span data-ttu-id="7bb0b-204">**Confirm.aspx.CX**</span><span class="sxs-lookup"><span data-stu-id="7bb0b-204">**Confirm.aspx.cx**</span></span>

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample33.cs?highlight=8)]

<span data-ttu-id="7bb0b-205">**RegisterExternalLogin.aspx.cs**</span><span class="sxs-lookup"><span data-stu-id="7bb0b-205">**RegisterExternalLogin.aspx.cs**</span></span>

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample34.cs?highlight=36)]

<span data-ttu-id="7bb0b-206">**Manage.aspx.cs**</span><span class="sxs-lookup"><span data-stu-id="7bb0b-206">**Manage.aspx.cs**</span></span>

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample35.cs?highlight=11,27,32,34,82,87,99,108)]

<span data-ttu-id="7bb0b-207">**VerifyPhoneNumber.aspx.cs**</span><span class="sxs-lookup"><span data-stu-id="7bb0b-207">**VerifyPhoneNumber.aspx.cs**</span></span>

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample36.cs?highlight=8,23,27)]

<span data-ttu-id="7bb0b-208">**AddPhoneNumber.aspx.cs**</span><span class="sxs-lookup"><span data-stu-id="7bb0b-208">**AddPhoneNumber.aspx.cs**</span></span>

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample37.cs?highlight=7)]

<span data-ttu-id="7bb0b-209">**ManagePassword.aspx.cs**</span><span class="sxs-lookup"><span data-stu-id="7bb0b-209">**ManagePassword.aspx.cs**</span></span>

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample38.cs?highlight=11,47,50,68)]

<span data-ttu-id="7bb0b-210">**ManageLogins.aspx.cs**</span><span class="sxs-lookup"><span data-stu-id="7bb0b-210">**ManageLogins.aspx.cs**</span></span>

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample39.cs?highlight=16,23,32,41,45)]

<span data-ttu-id="7bb0b-211">**TwoFactorAuthenticationSignIn.aspx.cs**</span><span class="sxs-lookup"><span data-stu-id="7bb0b-211">**TwoFactorAuthenticationSignIn.aspx.cs**</span></span>

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample40.cs?highlight=14-15,29,53)]

<a id="run"></a>
## <a name="run-application"></a><span data-ttu-id="7bb0b-212">Запуск приложения</span><span class="sxs-lookup"><span data-stu-id="7bb0b-212">Run application</span></span>

<span data-ttu-id="7bb0b-213">Вы завершили все необходимые изменения в шаблон веб-приложения по умолчанию.</span><span class="sxs-lookup"><span data-stu-id="7bb0b-213">You have finished all of the required changes to the default Web Application template.</span></span> <span data-ttu-id="7bb0b-214">Запустите приложение и регистрации нового пользователя.</span><span class="sxs-lookup"><span data-stu-id="7bb0b-214">Run the application and register a new user.</span></span> <span data-ttu-id="7bb0b-215">После регистрации пользователя, можно заметить, что таблица AspNetUsers содержит идентификатор столбца, который является целым числом.</span><span class="sxs-lookup"><span data-stu-id="7bb0b-215">After registering the user, you will notice that the AspNetUsers table has an Id column that is an integer.</span></span>

![новый первичный ключ](change-primary-key-for-users-in-aspnet-identity/_static/image1.png)

<span data-ttu-id="7bb0b-217">Если созданный ранее ASP.NET Identity таблицы с другой первичный ключ, необходимо внести некоторые дополнительные изменения.</span><span class="sxs-lookup"><span data-stu-id="7bb0b-217">If you have previously created the ASP.NET Identity tables with a different primary key, you need to make some additional changes.</span></span> <span data-ttu-id="7bb0b-218">Если это возможно удалите существующую базу данных.</span><span class="sxs-lookup"><span data-stu-id="7bb0b-218">If possible, just delete the existing database.</span></span> <span data-ttu-id="7bb0b-219">База данных будет восстановлено с правильный подход, при запуске веб-приложения и добавить нового пользователя.</span><span class="sxs-lookup"><span data-stu-id="7bb0b-219">The database will be re-created with the correct design when you run the web application and add a new user.</span></span> <span data-ttu-id="7bb0b-220">Если удаление не поддерживается, выполнения миграции code first для изменения таблиц.</span><span class="sxs-lookup"><span data-stu-id="7bb0b-220">If deletion is not possible, run code first migrations to change the tables.</span></span> <span data-ttu-id="7bb0b-221">Тем не менее новый первичный ключ целое число будет не установлен как свойство ИДЕНТИФИКАТОРОВ SQL в базе данных.</span><span class="sxs-lookup"><span data-stu-id="7bb0b-221">However, the new integer primary key will not be set up as a SQL IDENTITY property in the database.</span></span> <span data-ttu-id="7bb0b-222">Столбец идентификаторов необходимо вручную задать как УДОСТОВЕРЕНИЕ.</span><span class="sxs-lookup"><span data-stu-id="7bb0b-222">You must manually set the Id column as an IDENTITY.</span></span>

<a id="other"></a>
## <a name="other-resources"></a><span data-ttu-id="7bb0b-223">Другие источники</span><span class="sxs-lookup"><span data-stu-id="7bb0b-223">Other resources</span></span>

- [<span data-ttu-id="7bb0b-224">Обзор пользовательских поставщиков хранилищ для ASP.NET Identity</span><span class="sxs-lookup"><span data-stu-id="7bb0b-224">Overview of Custom Storage Providers for ASP.NET Identity</span></span>](overview-of-custom-storage-providers-for-aspnet-identity.md)
- [<span data-ttu-id="7bb0b-225">Миграция существующего веб-сайта из членства SQL в ASP.NET Identity</span><span class="sxs-lookup"><span data-stu-id="7bb0b-225">Migrating an Existing Website from SQL Membership to ASP.NET Identity</span></span>](../migrations/migrating-an-existing-website-from-sql-membership-to-aspnet-identity.md)
- [<span data-ttu-id="7bb0b-226">Перенос данных универсальной поставщик членства и профили пользователей для ASP.NET Identity</span><span class="sxs-lookup"><span data-stu-id="7bb0b-226">Migrating Universal Provider Data for Membership and User Profiles to ASP.NET Identity</span></span>](../migrations/migrating-universal-provider-data-for-membership-and-user-profiles-to-aspnet-identity.md)
- <span data-ttu-id="7bb0b-227">[Образец приложения](https://aspnet.codeplex.com/SourceControl/latest#Samples/Identity/ChangePK/readme.txt) с измененной первичного ключа</span><span class="sxs-lookup"><span data-stu-id="7bb0b-227">[Sample application](https://aspnet.codeplex.com/SourceControl/latest#Samples/Identity/ChangePK/readme.txt) with changed primary key</span></span>
