---
uid: identity/overview/extensibility/change-primary-key-for-users-in-aspnet-identity
title: Изменение первичного ключа для пользователей в ASP.NET Identity | Документация Майкрософт
author: tfitzmac
description: В Visual Studio 2013 веб-приложения по умолчанию использует строковое значение для ключа для учетных записей пользователей. ASP.NET Identity позволяет изменить тип...
ms.author: aspnetcontent
manager: wpickett
ms.date: 09/30/2014
ms.topic: article
ms.assetid: 44925849-5762-4504-a8cd-8f0cd06f6dc3
ms.technology: ''
msc.legacyurl: /identity/overview/extensibility/change-primary-key-for-users-in-aspnet-identity
msc.type: authoredcontent
ms.openlocfilehash: 20e6b86f50a6ea62f188ae592e0b302c7ef77177
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 07/03/2018
ms.locfileid: "37373332"
---
<a name="change-primary-key-for-users-in-aspnet-identity"></a><span data-ttu-id="6fbe1-104">Изменение первичного ключа для пользователей в ASP.NET Identity</span><span class="sxs-lookup"><span data-stu-id="6fbe1-104">Change Primary Key for Users in ASP.NET Identity</span></span>
====================
<span data-ttu-id="6fbe1-105">по [Tom FitzMacken](https://github.com/tfitzmac)</span><span class="sxs-lookup"><span data-stu-id="6fbe1-105">by [Tom FitzMacken](https://github.com/tfitzmac)</span></span>

> <span data-ttu-id="6fbe1-106">В Visual Studio 2013 веб-приложения по умолчанию использует строковое значение для ключа для учетных записей пользователей.</span><span class="sxs-lookup"><span data-stu-id="6fbe1-106">In Visual Studio 2013, the default web application uses a string value for the key for user accounts.</span></span> <span data-ttu-id="6fbe1-107">ASP.NET Identity позволяет изменить тип ключа в соответствии с требованиями данных.</span><span class="sxs-lookup"><span data-stu-id="6fbe1-107">ASP.NET Identity enables you to change the type of the key to meet your data requirements.</span></span> <span data-ttu-id="6fbe1-108">Например можно изменить тип ключа из строки в целое число.</span><span class="sxs-lookup"><span data-stu-id="6fbe1-108">For example, you can change the type of the key from a string to an integer.</span></span>
> 
> <span data-ttu-id="6fbe1-109">В этом разделе показано, как начать с веб-приложения по умолчанию и измените ключ учетной записи пользователя в целое число.</span><span class="sxs-lookup"><span data-stu-id="6fbe1-109">This topic shows how to start with the default web application and change the user account key to an integer.</span></span> <span data-ttu-id="6fbe1-110">Можно использовать такие же изменения для реализации ключу любого типа в проекте.</span><span class="sxs-lookup"><span data-stu-id="6fbe1-110">You can use the same modifications to implement any type of key in your project.</span></span> <span data-ttu-id="6fbe1-111">Показано, как внести эти изменения в веб-приложения по умолчанию, но вы можете применить аналогичные изменения специальных приложений.</span><span class="sxs-lookup"><span data-stu-id="6fbe1-111">It shows how to make these changes in the default web application, but you could apply similar modifications to a customized application.</span></span> <span data-ttu-id="6fbe1-112">Он показывает изменения, необходимые при работе с веб-форм или MVC.</span><span class="sxs-lookup"><span data-stu-id="6fbe1-112">It shows the changes needed when working with MVC or Web Forms.</span></span>
> 
> ## <a name="software-versions-used-in-the-tutorial"></a><span data-ttu-id="6fbe1-113">Версии программного обеспечения, используемые в этом руководстве</span><span class="sxs-lookup"><span data-stu-id="6fbe1-113">Software versions used in the tutorial</span></span>
> 
> 
> - <span data-ttu-id="6fbe1-114">Visual Studio 2013 с обновлением 2 (или более поздней версии)</span><span class="sxs-lookup"><span data-stu-id="6fbe1-114">Visual Studio 2013 with Update 2 (or later)</span></span>
> - <span data-ttu-id="6fbe1-115">ASP.NET Identity 2.1 или более поздней версии</span><span class="sxs-lookup"><span data-stu-id="6fbe1-115">ASP.NET Identity 2.1 or later</span></span>


<span data-ttu-id="6fbe1-116">Для выполнения шагов в этом руководстве, необходимо иметь Visual Studio 2013 с обновлением 2 (или более поздней версии) и веб-приложения, созданные на основе шаблона веб-приложения ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="6fbe1-116">To perform the steps in this tutorial, you must have Visual Studio 2013 Update 2 (or later), and a web application created from the ASP.NET Web Application template.</span></span> <span data-ttu-id="6fbe1-117">Шаблон, изменения в обновлении 3.</span><span class="sxs-lookup"><span data-stu-id="6fbe1-117">The template changed in Update 3.</span></span> <span data-ttu-id="6fbe1-118">В этом разделе показано, как изменить шаблон с обновлением 2 и с обновлением 3.</span><span class="sxs-lookup"><span data-stu-id="6fbe1-118">This topic shows how to change the template in Update 2 and Update 3.</span></span>

<span data-ttu-id="6fbe1-119">В этом разделе содержатся следующие подразделы.</span><span class="sxs-lookup"><span data-stu-id="6fbe1-119">This topic contains the following sections:</span></span>

- [<span data-ttu-id="6fbe1-120">Измените тип ключа в класс пользовательского удостоверения</span><span class="sxs-lookup"><span data-stu-id="6fbe1-120">Change the type of the key in the Identity user class</span></span>](#userclass)
- [<span data-ttu-id="6fbe1-121">Добавьте настраиваемые классы удостоверений, использующие тип ключа</span><span class="sxs-lookup"><span data-stu-id="6fbe1-121">Add customized Identity classes that use the key type</span></span>](#customclass)
- [<span data-ttu-id="6fbe1-122">Изменение контекста класса и диспетчера пользователей для использования данного типа ключа</span><span class="sxs-lookup"><span data-stu-id="6fbe1-122">Change the context class and user manager to use the key type</span></span>](#context)
- [<span data-ttu-id="6fbe1-123">Изменение конфигурации запуска для использования данного типа ключа</span><span class="sxs-lookup"><span data-stu-id="6fbe1-123">Change start-up configuration to use the key type</span></span>](#startup)
- [<span data-ttu-id="6fbe1-124">Для MVC с обновлением 2 измените AccountController передаваемый тип ключа</span><span class="sxs-lookup"><span data-stu-id="6fbe1-124">For MVC with Update 2, change the AccountController to pass the key type</span></span>](#mvcupdate2)
- [<span data-ttu-id="6fbe1-125">Для MVC с обновлением 3 измените AccountController и ManageController для передачи типа ключа</span><span class="sxs-lookup"><span data-stu-id="6fbe1-125">For MVC with Update 3, change the AccountController and ManageController to pass the key type</span></span>](#mvcupdate3)
- [<span data-ttu-id="6fbe1-126">Для веб-форм с обновлением 2 измените учетную запись страницы для передачи типа ключа</span><span class="sxs-lookup"><span data-stu-id="6fbe1-126">For Web Forms with Update 2, change Account pages to pass the key type</span></span>](#webformsupdate2)
- [<span data-ttu-id="6fbe1-127">Для веб-форм с обновлением 3 измените учетную запись страницы для передачи типа ключа</span><span class="sxs-lookup"><span data-stu-id="6fbe1-127">For Web Forms with Update 3, change Account pages to pass the key type</span></span>](#webformsupdate3)
- [<span data-ttu-id="6fbe1-128">Запуск приложения</span><span class="sxs-lookup"><span data-stu-id="6fbe1-128">Run application</span></span>](#run)
- [<span data-ttu-id="6fbe1-129">Другие ресурсы</span><span class="sxs-lookup"><span data-stu-id="6fbe1-129">Other resources</span></span>](#other)

<a id="userclass"></a>
## <a name="change-the-type-of-the-key-in-the-identity-user-class"></a><span data-ttu-id="6fbe1-130">Измените тип ключа в класс пользовательского удостоверения</span><span class="sxs-lookup"><span data-stu-id="6fbe1-130">Change the type of the key in the Identity user class</span></span>

<span data-ttu-id="6fbe1-131">В проекте, созданные на основе шаблона веб-приложение ASP.NET укажите, что класс ApplicationUser использует целое число для ключа для учетных записей пользователей.</span><span class="sxs-lookup"><span data-stu-id="6fbe1-131">In your project created from the ASP.NET Web Application template, specify that the ApplicationUser class uses an integer for the key for user accounts.</span></span> <span data-ttu-id="6fbe1-132">В IdentityModels.cs, изменить наследование из IdentityUser с типом класса ApplicationUser **int** TKey универсального параметра.</span><span class="sxs-lookup"><span data-stu-id="6fbe1-132">In IdentityModels.cs, change the ApplicationUser class to inherit from IdentityUser that has a type of **int** for the TKey generic parameter.</span></span> <span data-ttu-id="6fbe1-133">Можно также передать имена трех настраиваемый класс, который еще не реализован.</span><span class="sxs-lookup"><span data-stu-id="6fbe1-133">You also pass the names of three customized class which you have not implemented yet.</span></span>

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample1.cs?highlight=1-2)]

<span data-ttu-id="6fbe1-134">Тип ключа был изменен, но, по умолчанию остальной части приложения предполагает, что ключ является строкой.</span><span class="sxs-lookup"><span data-stu-id="6fbe1-134">You have changed the type of the key, but, by default, the rest of the application still assumes the key is a string.</span></span> <span data-ttu-id="6fbe1-135">Необходимо явно указать тип ключа в программный код, предполагающий строка.</span><span class="sxs-lookup"><span data-stu-id="6fbe1-135">You must explicitly indicate the type of the key in code that assumes a string.</span></span>

<span data-ttu-id="6fbe1-136">В **ApplicationUser** измените **GenerateUserIdentityAsync** метод для включения целое число, как показано в выделенный ниже код.</span><span class="sxs-lookup"><span data-stu-id="6fbe1-136">In the **ApplicationUser** class, change the **GenerateUserIdentityAsync** method to include int, as shown in the highlighted code below.</span></span> <span data-ttu-id="6fbe1-137">Это изменение не требуется для проектов веб-форм с помощью шаблона с обновлением 3.</span><span class="sxs-lookup"><span data-stu-id="6fbe1-137">This change is not necessary for Web Forms projects with the Update 3 template.</span></span>

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample2.cs?highlight=2)]

<a id="customclass"></a>
## <a name="add-customized-identity-classes-that-use-the-key-type"></a><span data-ttu-id="6fbe1-138">Добавьте настраиваемые классы удостоверений, использующие тип ключа</span><span class="sxs-lookup"><span data-stu-id="6fbe1-138">Add customized Identity classes that use the key type</span></span>

<span data-ttu-id="6fbe1-139">Другие классы удостоверений, например IdentityUserRole, IdentityUserClaim, IdentityUserLogin, IdentityRole, UserStore, RoleStore, по-прежнему настраиваются для использования ключа строки.</span><span class="sxs-lookup"><span data-stu-id="6fbe1-139">The other Identity classes, such as IdentityUserRole, IdentityUserClaim, IdentityUserLogin, IdentityRole, UserStore, RoleStore, are still set up to use a string key.</span></span> <span data-ttu-id="6fbe1-140">Создание новых версий этих классов, указать целое число для ключа.</span><span class="sxs-lookup"><span data-stu-id="6fbe1-140">Create new versions of these classes that specify an integer for the key.</span></span> <span data-ttu-id="6fbe1-141">Необходимо предоставить много кода реализации в этих классах, главным образом только при установке int как ключ.</span><span class="sxs-lookup"><span data-stu-id="6fbe1-141">You do not need to provide much implementation code in these classes, you are primarily just setting int as the key.</span></span>

<span data-ttu-id="6fbe1-142">Добавьте следующие классы в файле IdentityModels.cs.</span><span class="sxs-lookup"><span data-stu-id="6fbe1-142">Add the following classes to your IdentityModels.cs file.</span></span>

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample3.cs)]

<a id="context"></a>
## <a name="change-the-context-class-and-user-manager-to-use-the-key-type"></a><span data-ttu-id="6fbe1-143">Изменение контекста класса и диспетчера пользователей для использования данного типа ключа</span><span class="sxs-lookup"><span data-stu-id="6fbe1-143">Change the context class and user manager to use the key type</span></span>

<span data-ttu-id="6fbe1-144">В IdentityModels.cs, измените определение **ApplicationDbContext** класс для использования нового настроить классы и **int** для ключа, как показано в выделенном коде.</span><span class="sxs-lookup"><span data-stu-id="6fbe1-144">In IdentityModels.cs, change the definition of the **ApplicationDbContext** class to use your new customized classes and an **int** for the key, as shown in the highlighted code.</span></span>

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample4.cs?highlight=1-2)]

<span data-ttu-id="6fbe1-145">Данный параметр ThrowIfV1Schema больше не является допустимым в конструкторе.</span><span class="sxs-lookup"><span data-stu-id="6fbe1-145">The ThrowIfV1Schema parameter is no longer valid in the constructor.</span></span> <span data-ttu-id="6fbe1-146">Измените конструктор, поэтому он не передает значение ThrowIfV1Schema.</span><span class="sxs-lookup"><span data-stu-id="6fbe1-146">Change the constructor so it does not pass a ThrowIfV1Schema value.</span></span>

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample5.cs)]

<span data-ttu-id="6fbe1-147">Откройте IdentityConfig.cs и измените **ApplicationUserManger** класс для использования нового пользователя хранилища класс для сохранения данных и **int** для ключа.</span><span class="sxs-lookup"><span data-stu-id="6fbe1-147">Open IdentityConfig.cs, and change the **ApplicationUserManger** class to use your new user store class for persisting data and an **int** for the key.</span></span>

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample6.cs?highlight=1,3,12,14,32,37,48)]

<span data-ttu-id="6fbe1-148">В шаблоне с обновлением 3 необходимо изменить класс ApplicationSignInManager.</span><span class="sxs-lookup"><span data-stu-id="6fbe1-148">In the Update 3 template, you must change the ApplicationSignInManager class.</span></span>

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample7.cs?highlight=1)]

<a id="startup"></a>
## <a name="change-start-up-configuration-to-use-the-key-type"></a><span data-ttu-id="6fbe1-149">Изменение конфигурации запуска для использования данного типа ключа</span><span class="sxs-lookup"><span data-stu-id="6fbe1-149">Change start-up configuration to use the key type</span></span>

<span data-ttu-id="6fbe1-150">В Startup.Auth.cs замените код OnValidateIdentity, как показано ниже.</span><span class="sxs-lookup"><span data-stu-id="6fbe1-150">In Startup.Auth.cs, replace the OnValidateIdentity code, as highlighted below.</span></span> <span data-ttu-id="6fbe1-151">Обратите внимание на то, что определение getUserIdCallback, анализирует строковое значение в целое число.</span><span class="sxs-lookup"><span data-stu-id="6fbe1-151">Notice that the getUserIdCallback definition, parses the string value into an integer.</span></span>

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample8.cs?highlight=7-12)]

<span data-ttu-id="6fbe1-152">Если проект не распознает универсальную реализацию **GetUserId** метод, может потребоваться обновить пакет NuGet удостоверения ASP.NET до версии 2.1</span><span class="sxs-lookup"><span data-stu-id="6fbe1-152">If your project does not recognize the generic implementation of the **GetUserId** method, you may need to update the ASP.NET Identity NuGet package to version 2.1</span></span>

<span data-ttu-id="6fbe1-153">Классы инфраструктуры, используемые для идентификации ASP.NET внесено много изменений.</span><span class="sxs-lookup"><span data-stu-id="6fbe1-153">You have made a lot of changes to the infrastructure classes used by ASP.NET Identity.</span></span> <span data-ttu-id="6fbe1-154">При попытке компиляции проекта, можно заметить большое число ошибок.</span><span class="sxs-lookup"><span data-stu-id="6fbe1-154">If you try compiling the project, you will notice a lot of errors.</span></span> <span data-ttu-id="6fbe1-155">К счастью оставшихся ошибок все похожи.</span><span class="sxs-lookup"><span data-stu-id="6fbe1-155">Fortunately, the remaining errors are all similar.</span></span> <span data-ttu-id="6fbe1-156">Класс идентификации ожидает, что целое число для ключа, но контроллера (или веб-формы) передает значение строки.</span><span class="sxs-lookup"><span data-stu-id="6fbe1-156">The Identity class expects an integer for the key, but the controller (or Web Form) is passing a string value.</span></span> <span data-ttu-id="6fbe1-157">В каждом случае необходимо преобразовать в строку и целое число путем вызова **GetUserId&lt;int&gt;**.</span><span class="sxs-lookup"><span data-stu-id="6fbe1-157">In each case, you need to convert from a string to and integer by calling **GetUserId&lt;int&gt;**.</span></span> <span data-ttu-id="6fbe1-158">Можно работать через список ошибок из компиляции или ниже изменения.</span><span class="sxs-lookup"><span data-stu-id="6fbe1-158">You can either work through the error list from compilation or follow the changes below.</span></span>

<span data-ttu-id="6fbe1-159">Оставшиеся изменения зависят от типа проекта, вы создаете и какие обновления установлены в Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="6fbe1-159">The remaining changes depend on the type of project you are creating and which update you have installed in Visual Studio.</span></span> <span data-ttu-id="6fbe1-160">Вы можете перейти непосредственно на соответствующий раздел по ссылке</span><span class="sxs-lookup"><span data-stu-id="6fbe1-160">You can go directly to the relevant section through the following links</span></span>

- [<span data-ttu-id="6fbe1-161">Для MVC с обновлением 2 измените AccountController передаваемый тип ключа</span><span class="sxs-lookup"><span data-stu-id="6fbe1-161">For MVC with Update 2, change the AccountController to pass the key type</span></span>](#mvcupdate2)
- [<span data-ttu-id="6fbe1-162">Для MVC с обновлением 3 измените AccountController и ManageController для передачи типа ключа</span><span class="sxs-lookup"><span data-stu-id="6fbe1-162">For MVC with Update 3, change the AccountController and ManageController to pass the key type</span></span>](#mvcupdate3)
- [<span data-ttu-id="6fbe1-163">Для веб-форм с обновлением 2 измените учетную запись страницы для передачи типа ключа</span><span class="sxs-lookup"><span data-stu-id="6fbe1-163">For Web Forms with Update 2, change Account pages to pass the key type</span></span>](#webformsupdate2)
- [<span data-ttu-id="6fbe1-164">Для веб-форм с обновлением 3 измените учетную запись страницы для передачи типа ключа</span><span class="sxs-lookup"><span data-stu-id="6fbe1-164">For Web Forms with Update 3, change Account pages to pass the key type</span></span>](#webformsupdate3)

<a id="mvcupdate2"></a>
## <a name="for-mvc-with-update-2-change-the-accountcontroller-to-pass-the-key-type"></a><span data-ttu-id="6fbe1-165">Для MVC с обновлением 2 измените AccountController передаваемый тип ключа</span><span class="sxs-lookup"><span data-stu-id="6fbe1-165">For MVC with Update 2, change the AccountController to pass the key type</span></span>

<span data-ttu-id="6fbe1-166">Откройте файл AccountController.cs.</span><span class="sxs-lookup"><span data-stu-id="6fbe1-166">Open the AccountController.cs file.</span></span> <span data-ttu-id="6fbe1-167">Вам нужно изменить следующие методы.</span><span class="sxs-lookup"><span data-stu-id="6fbe1-167">You need to change the following methods.</span></span>

<span data-ttu-id="6fbe1-168">**ConfirmEmail** метод</span><span class="sxs-lookup"><span data-stu-id="6fbe1-168">**ConfirmEmail** method</span></span>

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample9.cs?highlight=1,3)]

<span data-ttu-id="6fbe1-169">**Отмените связь** метод</span><span class="sxs-lookup"><span data-stu-id="6fbe1-169">**Disassociate** method</span></span>

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample10.cs?highlight=5,9)]

<span data-ttu-id="6fbe1-170">**Manage(ManageUserViewModel)** метод</span><span class="sxs-lookup"><span data-stu-id="6fbe1-170">**Manage(ManageUserViewModel)** method</span></span>

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample11.cs?highlight=11,17,41)]

<span data-ttu-id="6fbe1-171">**LinkLoginCallback** метод</span><span class="sxs-lookup"><span data-stu-id="6fbe1-171">**LinkLoginCallback** method</span></span>

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample12.cs?highlight=10)]

<span data-ttu-id="6fbe1-172">**RemoveAccountList** метод</span><span class="sxs-lookup"><span data-stu-id="6fbe1-172">**RemoveAccountList** method</span></span>

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample13.cs?highlight=3)]

<span data-ttu-id="6fbe1-173">**HasPassword** метод</span><span class="sxs-lookup"><span data-stu-id="6fbe1-173">**HasPassword** method</span></span>

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample14.cs?highlight=3)]

<span data-ttu-id="6fbe1-174">Теперь вы можете [запустите приложение](#run) и зарегистрируйте нового пользователя.</span><span class="sxs-lookup"><span data-stu-id="6fbe1-174">You can now [run the application](#run) and register a new user.</span></span>

<a id="mvcupdate3"></a>
## <a name="for-mvc-with-update-3-change-the-accountcontroller-and-managecontroller-to-pass-the-key-type"></a><span data-ttu-id="6fbe1-175">Для MVC с обновлением 3 измените AccountController и ManageController для передачи типа ключа</span><span class="sxs-lookup"><span data-stu-id="6fbe1-175">For MVC with Update 3, change the AccountController and ManageController to pass the key type</span></span>

<span data-ttu-id="6fbe1-176">Откройте файл AccountController.cs.</span><span class="sxs-lookup"><span data-stu-id="6fbe1-176">Open the AccountController.cs file.</span></span> <span data-ttu-id="6fbe1-177">Вам нужно изменить следующий метод.</span><span class="sxs-lookup"><span data-stu-id="6fbe1-177">You need to change the following method.</span></span>

<span data-ttu-id="6fbe1-178">**ConfirmEmail** метод</span><span class="sxs-lookup"><span data-stu-id="6fbe1-178">**ConfirmEmail** method</span></span>

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample15.cs?highlight=1,3)]

<span data-ttu-id="6fbe1-179">**SendCode** метод</span><span class="sxs-lookup"><span data-stu-id="6fbe1-179">**SendCode** method</span></span>

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample16.cs?highlight=4)]

<span data-ttu-id="6fbe1-180">Откройте файл ManageController.cs.</span><span class="sxs-lookup"><span data-stu-id="6fbe1-180">Open the ManageController.cs file.</span></span> <span data-ttu-id="6fbe1-181">Вам нужно изменить следующие методы.</span><span class="sxs-lookup"><span data-stu-id="6fbe1-181">You need to change the following methods.</span></span>

<span data-ttu-id="6fbe1-182">**Индекс** метод</span><span class="sxs-lookup"><span data-stu-id="6fbe1-182">**Index** method</span></span>

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample17.cs?highlight=15-17)]

<span data-ttu-id="6fbe1-183">**RemoveLogin** методы</span><span class="sxs-lookup"><span data-stu-id="6fbe1-183">**RemoveLogin** methods</span></span>

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample18.cs?highlight=3,13,17)]

<span data-ttu-id="6fbe1-184">**AddPhoneNumber** метод</span><span class="sxs-lookup"><span data-stu-id="6fbe1-184">**AddPhoneNumber** method</span></span>

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample19.cs?highlight=9)]

<span data-ttu-id="6fbe1-185">**EnableTwoFactorAuthentication** метод</span><span class="sxs-lookup"><span data-stu-id="6fbe1-185">**EnableTwoFactorAuthentication** method</span></span>

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample20.cs?highlight=3-4)]

<span data-ttu-id="6fbe1-186">**DisableTwoFactorAuthentication** метод</span><span class="sxs-lookup"><span data-stu-id="6fbe1-186">**DisableTwoFactorAuthentication** method</span></span>

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample21.cs?highlight=3-4)]

<span data-ttu-id="6fbe1-187">**VerifyPhoneNumber** методы</span><span class="sxs-lookup"><span data-stu-id="6fbe1-187">**VerifyPhoneNumber** methods</span></span>

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample22.cs?highlight=4,18,21)]

<span data-ttu-id="6fbe1-188">**RemovePhoneNumber** метод</span><span class="sxs-lookup"><span data-stu-id="6fbe1-188">**RemovePhoneNumber** method</span></span>

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample23.cs?highlight=3,8)]

<span data-ttu-id="6fbe1-189">**ChangePassword** метод</span><span class="sxs-lookup"><span data-stu-id="6fbe1-189">**ChangePassword** method</span></span>

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample24.cs?highlight=10,13)]

<span data-ttu-id="6fbe1-190">**SetPassword** метод</span><span class="sxs-lookup"><span data-stu-id="6fbe1-190">**SetPassword** method</span></span>

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample25.cs?highlight=5,8)]

<span data-ttu-id="6fbe1-191">**ManageLogins** метод</span><span class="sxs-lookup"><span data-stu-id="6fbe1-191">**ManageLogins** method</span></span>

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample26.cs?highlight=7,12)]

<span data-ttu-id="6fbe1-192">**LinkLoginCallback** метод</span><span class="sxs-lookup"><span data-stu-id="6fbe1-192">**LinkLoginCallback** method</span></span>

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample27.cs?highlight=8)]

<span data-ttu-id="6fbe1-193">**HasPassword** метод</span><span class="sxs-lookup"><span data-stu-id="6fbe1-193">**HasPassword** method</span></span>

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample28.cs?highlight=3)]

<span data-ttu-id="6fbe1-194">**HasPhoneNumber** метод</span><span class="sxs-lookup"><span data-stu-id="6fbe1-194">**HasPhoneNumber** method</span></span>

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample29.cs?highlight=3)]

<span data-ttu-id="6fbe1-195">Теперь вы можете [запустите приложение](#run) и зарегистрируйте нового пользователя.</span><span class="sxs-lookup"><span data-stu-id="6fbe1-195">You can now [run the application](#run) and register a new user.</span></span>

<a id="webformsupdate2"></a>
## <a name="for-web-forms-with-update-2-change-account-pages-to-pass-the-key-type"></a><span data-ttu-id="6fbe1-196">Для веб-форм с обновлением 2 измените учетную запись страницы для передачи типа ключа</span><span class="sxs-lookup"><span data-stu-id="6fbe1-196">For Web Forms with Update 2, change Account pages to pass the key type</span></span>

<span data-ttu-id="6fbe1-197">Для веб-форм с обновлением 2 необходимо изменить на следующих страницах.</span><span class="sxs-lookup"><span data-stu-id="6fbe1-197">For Web Forms with Update 2, you need to change the following pages.</span></span>

<span data-ttu-id="6fbe1-198">**Confirm.aspx.CX**</span><span class="sxs-lookup"><span data-stu-id="6fbe1-198">**Confirm.aspx.cx**</span></span>

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample30.cs?highlight=8)]

<span data-ttu-id="6fbe1-199">**RegisterExternalLogin.aspx.cs**</span><span class="sxs-lookup"><span data-stu-id="6fbe1-199">**RegisterExternalLogin.aspx.cs**</span></span>

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample31.cs?highlight=36)]

<span data-ttu-id="6fbe1-200">**Manage.aspx.cs**</span><span class="sxs-lookup"><span data-stu-id="6fbe1-200">**Manage.aspx.cs**</span></span>

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample32.cs?highlight=3,22,47,52,69,85,93,98)]

<span data-ttu-id="6fbe1-201">Теперь вы можете [запустите приложение](#run) и зарегистрируйте нового пользователя.</span><span class="sxs-lookup"><span data-stu-id="6fbe1-201">You can now [run the application](#run) and register a new user.</span></span>

<a id="webformsupdate3"></a>
## <a name="for-web-forms-with-update-3-change-account-pages-to-pass-the-key-type"></a><span data-ttu-id="6fbe1-202">Для веб-форм с обновлением 3 измените учетную запись страницы для передачи типа ключа</span><span class="sxs-lookup"><span data-stu-id="6fbe1-202">For Web Forms with Update 3, change Account pages to pass the key type</span></span>

<span data-ttu-id="6fbe1-203">Для веб-форм с обновлением 3 необходимо изменить на следующих страницах.</span><span class="sxs-lookup"><span data-stu-id="6fbe1-203">For Web Forms with Update 3, you need to change the following pages.</span></span>

<span data-ttu-id="6fbe1-204">**Confirm.aspx.CX**</span><span class="sxs-lookup"><span data-stu-id="6fbe1-204">**Confirm.aspx.cx**</span></span>

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample33.cs?highlight=8)]

<span data-ttu-id="6fbe1-205">**RegisterExternalLogin.aspx.cs**</span><span class="sxs-lookup"><span data-stu-id="6fbe1-205">**RegisterExternalLogin.aspx.cs**</span></span>

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample34.cs?highlight=36)]

<span data-ttu-id="6fbe1-206">**Manage.aspx.cs**</span><span class="sxs-lookup"><span data-stu-id="6fbe1-206">**Manage.aspx.cs**</span></span>

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample35.cs?highlight=11,27,32,34,82,87,99,108)]

<span data-ttu-id="6fbe1-207">**VerifyPhoneNumber.aspx.cs**</span><span class="sxs-lookup"><span data-stu-id="6fbe1-207">**VerifyPhoneNumber.aspx.cs**</span></span>

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample36.cs?highlight=8,23,27)]

<span data-ttu-id="6fbe1-208">**AddPhoneNumber.aspx.cs**</span><span class="sxs-lookup"><span data-stu-id="6fbe1-208">**AddPhoneNumber.aspx.cs**</span></span>

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample37.cs?highlight=7)]

<span data-ttu-id="6fbe1-209">**ManagePassword.aspx.cs**</span><span class="sxs-lookup"><span data-stu-id="6fbe1-209">**ManagePassword.aspx.cs**</span></span>

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample38.cs?highlight=11,47,50,68)]

<span data-ttu-id="6fbe1-210">**ManageLogins.aspx.cs**</span><span class="sxs-lookup"><span data-stu-id="6fbe1-210">**ManageLogins.aspx.cs**</span></span>

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample39.cs?highlight=16,23,32,41,45)]

<span data-ttu-id="6fbe1-211">**TwoFactorAuthenticationSignIn.aspx.cs**</span><span class="sxs-lookup"><span data-stu-id="6fbe1-211">**TwoFactorAuthenticationSignIn.aspx.cs**</span></span>

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample40.cs?highlight=14-15,29,53)]

<a id="run"></a>
## <a name="run-application"></a><span data-ttu-id="6fbe1-212">Запуск приложения</span><span class="sxs-lookup"><span data-stu-id="6fbe1-212">Run application</span></span>

<span data-ttu-id="6fbe1-213">Вы завершили все изменения, необходимые для шаблона веб-приложения по умолчанию.</span><span class="sxs-lookup"><span data-stu-id="6fbe1-213">You have finished all of the required changes to the default Web Application template.</span></span> <span data-ttu-id="6fbe1-214">Запустите приложение и зарегистрируйте нового пользователя.</span><span class="sxs-lookup"><span data-stu-id="6fbe1-214">Run the application and register a new user.</span></span> <span data-ttu-id="6fbe1-215">После регистрации пользователя можно заметить, что таблица AspNetUsers содержит идентификатор столбца, должно быть целым числом.</span><span class="sxs-lookup"><span data-stu-id="6fbe1-215">After registering the user, you will notice that the AspNetUsers table has an Id column that is an integer.</span></span>

![новый первичный ключ](change-primary-key-for-users-in-aspnet-identity/_static/image1.png)

<span data-ttu-id="6fbe1-217">Если вы ранее создали ASP.NET Identity таблицы с другой первичный ключ, необходимо внести некоторые дополнительные изменения.</span><span class="sxs-lookup"><span data-stu-id="6fbe1-217">If you have previously created the ASP.NET Identity tables with a different primary key, you need to make some additional changes.</span></span> <span data-ttu-id="6fbe1-218">Если это возможно просто удалите существующую базу данных.</span><span class="sxs-lookup"><span data-stu-id="6fbe1-218">If possible, just delete the existing database.</span></span> <span data-ttu-id="6fbe1-219">База данных будет восстановлено с правильный подход, при запуске веб-приложения и добавить нового пользователя.</span><span class="sxs-lookup"><span data-stu-id="6fbe1-219">The database will be re-created with the correct design when you run the web application and add a new user.</span></span> <span data-ttu-id="6fbe1-220">Если удаление не поддерживается, запуск code first migrations для изменения таблиц.</span><span class="sxs-lookup"><span data-stu-id="6fbe1-220">If deletion is not possible, run code first migrations to change the tables.</span></span> <span data-ttu-id="6fbe1-221">Тем не менее на новый первичный ключ целое число не будет быть установлен как свойство ИДЕНТИФИКАТОРОВ SQL в базе данных.</span><span class="sxs-lookup"><span data-stu-id="6fbe1-221">However, the new integer primary key will not be set up as a SQL IDENTITY property in the database.</span></span> <span data-ttu-id="6fbe1-222">Столбец идентификаторов необходимо вручную задать в качестве УДОСТОВЕРЕНИЯ.</span><span class="sxs-lookup"><span data-stu-id="6fbe1-222">You must manually set the Id column as an IDENTITY.</span></span>

<a id="other"></a>
## <a name="other-resources"></a><span data-ttu-id="6fbe1-223">Другие источники</span><span class="sxs-lookup"><span data-stu-id="6fbe1-223">Other resources</span></span>

- [<span data-ttu-id="6fbe1-224">Обзор пользовательских поставщиков хранилищ для ASP.NET Identity</span><span class="sxs-lookup"><span data-stu-id="6fbe1-224">Overview of Custom Storage Providers for ASP.NET Identity</span></span>](overview-of-custom-storage-providers-for-aspnet-identity.md)
- [<span data-ttu-id="6fbe1-225">Миграция существующего веб-сайта из членства SQL в ASP.NET Identity</span><span class="sxs-lookup"><span data-stu-id="6fbe1-225">Migrating an Existing Website from SQL Membership to ASP.NET Identity</span></span>](../migrations/migrating-an-existing-website-from-sql-membership-to-aspnet-identity.md)
- [<span data-ttu-id="6fbe1-226">Перенос данных универсального поставщика членства и профилей пользователей в ASP.NET Identity</span><span class="sxs-lookup"><span data-stu-id="6fbe1-226">Migrating Universal Provider Data for Membership and User Profiles to ASP.NET Identity</span></span>](../migrations/migrating-universal-provider-data-for-membership-and-user-profiles-to-aspnet-identity.md)
- <span data-ttu-id="6fbe1-227">[Пример приложения](https://aspnet.codeplex.com/SourceControl/latest#Samples/Identity/ChangePK/readme.txt) измененные первичный ключ</span><span class="sxs-lookup"><span data-stu-id="6fbe1-227">[Sample application](https://aspnet.codeplex.com/SourceControl/latest#Samples/Identity/ChangePK/readme.txt) with changed primary key</span></span>
