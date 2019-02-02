---
uid: mvc/overview/getting-started/database-first-development/enhancing-data-validation
title: Учебник. Улучшения проверки данных для EF Database First с помощью приложения ASP.NET MVC
description: Это руководство посвящено добавления заметок к данным в модель данных, чтобы указать требования к проверке и формат отображения.
author: Rick-Anderson
ms.author: riande
ms.date: 01/28/2019
ms.topic: tutorial
ms.assetid: 0ed5e67a-34c0-4b57-84a6-802b0fb3cd00
msc.legacyurl: /mvc/overview/getting-started/database-first-development/enhancing-data-validation
msc.type: authoredcontent
ms.openlocfilehash: 897cd7c6a40445e2a4abede50d81e101372d3233
ms.sourcegitcommit: ed76cc752966c604a795fbc56d5a71d16ded0b58
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 02/02/2019
ms.locfileid: "55667626"
---
# <a name="tutorial-enhance-data-validation-for-ef-database-first-with-aspnet-mvc-app"></a><span data-ttu-id="35baa-103">Учебник. Улучшения проверки данных для EF Database First с помощью приложения ASP.NET MVC</span><span class="sxs-lookup"><span data-stu-id="35baa-103">Tutorial: Enhance data validation for EF Database First with ASP.NET MVC app</span></span>

<span data-ttu-id="35baa-104">С помощью MVC, Entity Framework и формирование шаблонов ASP.NET, можно создать веб-приложение, которое предоставляет интерфейс для существующей базы данных.</span><span class="sxs-lookup"><span data-stu-id="35baa-104">Using MVC, Entity Framework, and ASP.NET Scaffolding, you can create a web application that provides an interface to an existing database.</span></span> <span data-ttu-id="35baa-105">В этой серии руководств показано, как автоматически создавать код, позволяющий пользователям для отображения, изменения и создавать и удалять данные, находящиеся в таблице базы данных.</span><span class="sxs-lookup"><span data-stu-id="35baa-105">This tutorial series shows you how to automatically generate code that enables users to display, edit, create, and delete data that resides in a database table.</span></span> <span data-ttu-id="35baa-106">Созданный код соответствует столбцам в таблице базы данных.</span><span class="sxs-lookup"><span data-stu-id="35baa-106">The generated code corresponds to the columns in the database table.</span></span>

<span data-ttu-id="35baa-107">Это руководство посвящено добавления заметок к данным в модель данных, чтобы указать требования к проверке и формат отображения.</span><span class="sxs-lookup"><span data-stu-id="35baa-107">This tutorial focuses on adding data annotations to the data model to specify validation requirements and display formatting.</span></span> <span data-ttu-id="35baa-108">Она была улучшена зависимости на отзывы от пользователей в разделе "Примечания".</span><span class="sxs-lookup"><span data-stu-id="35baa-108">It was improved based on feedback from users in the comments section.</span></span>

<span data-ttu-id="35baa-109">В этом учебнике рассмотрены следующие задачи.</span><span class="sxs-lookup"><span data-stu-id="35baa-109">In this tutorial, you:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="35baa-110">Добавление заметок к данным</span><span class="sxs-lookup"><span data-stu-id="35baa-110">Add data annotations</span></span>
> * <span data-ttu-id="35baa-111">Добавить классы метаданных</span><span class="sxs-lookup"><span data-stu-id="35baa-111">Add metadata classes</span></span>

## <a name="prerequisites"></a><span data-ttu-id="35baa-112">Предварительные требования</span><span class="sxs-lookup"><span data-stu-id="35baa-112">Prerequisites</span></span>

* [<span data-ttu-id="35baa-113">Настройка представления</span><span class="sxs-lookup"><span data-stu-id="35baa-113">Customize a view</span></span>](customizing-a-view.md)

## <a name="add-data-annotations"></a><span data-ttu-id="35baa-114">Добавление заметок к данным</span><span class="sxs-lookup"><span data-stu-id="35baa-114">Add data annotations</span></span>

<span data-ttu-id="35baa-115">Как видно в предыдущей статье, некоторые правила проверки данных автоматически применяются к вводимых пользователем данных.</span><span class="sxs-lookup"><span data-stu-id="35baa-115">As you saw in an earlier topic, some data validation rules are automatically applied to the user input.</span></span> <span data-ttu-id="35baa-116">Например число может предоставить только для свойства корпоративного уровня.</span><span class="sxs-lookup"><span data-stu-id="35baa-116">For example, you can only provide a number for the Grade property.</span></span> <span data-ttu-id="35baa-117">Чтобы указать дополнительные правила проверки данных, можно добавить заметки к данным в классе модели.</span><span class="sxs-lookup"><span data-stu-id="35baa-117">To specify more data validation rules, you can add data annotations to your model class.</span></span> <span data-ttu-id="35baa-118">Эти заметки применяются в рамках всего веб-приложения для указанного свойства.</span><span class="sxs-lookup"><span data-stu-id="35baa-118">These annotations are applied throughout your web application for the specified property.</span></span> <span data-ttu-id="35baa-119">Можно также применить атрибуты форматирования, которые изменяют способ отображения свойств; например изменив значение, используемое для подписи.</span><span class="sxs-lookup"><span data-stu-id="35baa-119">You can also apply formatting attributes that change how the properties are displayed; such as, changing the value used for text labels.</span></span>

<span data-ttu-id="35baa-120">В этом руководстве вы добавите заметок к данным для ограничения длины значения, заданные для свойств FirstName, MiddleName и LastName.</span><span class="sxs-lookup"><span data-stu-id="35baa-120">In this tutorial, you will add data annotations to restrict the length of the values provided for the FirstName, LastName, and MiddleName properties.</span></span> <span data-ttu-id="35baa-121">В базе данных эти значения являются более 50 символов; Тем не менее в веб-приложении этого предельного количества символов в настоящее время не применяется.</span><span class="sxs-lookup"><span data-stu-id="35baa-121">In the database, these values are limited to 50 characters; however, in your web application that character limit is currently not enforced.</span></span> <span data-ttu-id="35baa-122">Если пользователь предоставляет более 50 символов для одного из этих значений, странице произойдет сбой при попытке сохранить значение в базе данных.</span><span class="sxs-lookup"><span data-stu-id="35baa-122">If a user provides more than 50 characters for one of those values, the page will crash when attempting to save the value to the database.</span></span> <span data-ttu-id="35baa-123">Также вы ограничите корпоративного уровня для значения в диапазоне от 0 до 4.</span><span class="sxs-lookup"><span data-stu-id="35baa-123">You will also restrict Grade to values between 0 and 4.</span></span>

<span data-ttu-id="35baa-124">Выберите **моделей** > **ContosoModel.edmx** > **ContosoModel.tt** и откройте *Student.cs* файла.</span><span class="sxs-lookup"><span data-stu-id="35baa-124">Select **Models** > **ContosoModel.edmx** > **ContosoModel.tt** and open the *Student.cs* file.</span></span> <span data-ttu-id="35baa-125">Добавьте выделенный ниже код к классу.</span><span class="sxs-lookup"><span data-stu-id="35baa-125">Add the following highlighted code to the class.</span></span>

[!code-csharp[Main](enhancing-data-validation/samples/sample1.cs?highlight=5,15,17,20)]

<span data-ttu-id="35baa-126">Откройте *Enrollment.cs* и добавьте следующий выделенный код.</span><span class="sxs-lookup"><span data-stu-id="35baa-126">Open *Enrollment.cs* and add the following highlighted code.</span></span>

[!code-csharp[Main](enhancing-data-validation/samples/sample2.cs?highlight=5,10)]

<span data-ttu-id="35baa-127">Постройте решение.</span><span class="sxs-lookup"><span data-stu-id="35baa-127">Build the solution.</span></span>

<span data-ttu-id="35baa-128">Нажмите кнопку **список учащихся** и выберите **изменить**.</span><span class="sxs-lookup"><span data-stu-id="35baa-128">Click **List of students** and select **Edit**.</span></span> <span data-ttu-id="35baa-129">Если попытаться ввести более 50 символов, отображается сообщение об ошибке.</span><span class="sxs-lookup"><span data-stu-id="35baa-129">If you attempt to enter more than 50 characters, an error message is displayed.</span></span>

![Отображение сообщения об ошибке](enhancing-data-validation/_static/image1.png)

<span data-ttu-id="35baa-131">Вернитесь на домашнюю страницу.</span><span class="sxs-lookup"><span data-stu-id="35baa-131">Go back to the home page.</span></span> <span data-ttu-id="35baa-132">Нажмите кнопку **список регистраций** и выберите **изменить**.</span><span class="sxs-lookup"><span data-stu-id="35baa-132">Click **List of enrollments** and select **Edit**.</span></span> <span data-ttu-id="35baa-133">Пытается предоставить оценку выше 4.</span><span class="sxs-lookup"><span data-stu-id="35baa-133">Attempt to provide a grade above 4.</span></span> <span data-ttu-id="35baa-134">Вы получите эту ошибку: *Поле корпоративного класса должно быть от 0 до 4.*</span><span class="sxs-lookup"><span data-stu-id="35baa-134">You will receive this error: *The field Grade must be between 0 and 4.*</span></span>

## <a name="add-metadata-classes"></a><span data-ttu-id="35baa-135">Добавить классы метаданных</span><span class="sxs-lookup"><span data-stu-id="35baa-135">Add metadata classes</span></span>

<span data-ttu-id="35baa-136">Добавление атрибутов проверки непосредственно к классу модели работает, если вы не ожидаете, что базы данных для изменения; Тем не менее если изменения базы данных и вам нужно повторно создать класс модели, вы потеряете все атрибуты, которые были применены к классу модели.</span><span class="sxs-lookup"><span data-stu-id="35baa-136">Adding the validation attributes directly to the model class works when you do not expect the database to change; however, if your database changes and you need to regenerate the model class, you will lose all of the attributes you had applied to the model class.</span></span> <span data-ttu-id="35baa-137">Этот подход может оказаться очень неэффективным и подвержен потери важных условий.</span><span class="sxs-lookup"><span data-stu-id="35baa-137">This approach can be very inefficient and prone to losing important validation rules.</span></span>

<span data-ttu-id="35baa-138">Чтобы избежать этой проблемы, можно добавить класс метаданных, содержащий атрибуты.</span><span class="sxs-lookup"><span data-stu-id="35baa-138">To avoid this problem, you can add a metadata class that contains the attributes.</span></span> <span data-ttu-id="35baa-139">При связывании класса модели к классу метаданных, эти атрибуты применяются к модели.</span><span class="sxs-lookup"><span data-stu-id="35baa-139">When you associate the model class to the metadata class, those attributes are applied to the model.</span></span> <span data-ttu-id="35baa-140">В этом случае можно повторно создать класс модели без потери всех атрибутов, примененных к классу метаданных.</span><span class="sxs-lookup"><span data-stu-id="35baa-140">In this approach, the model class can be regenerated without losing all of the attributes that have been applied to the metadata class.</span></span>

<span data-ttu-id="35baa-141">В **моделей** папки, добавьте класс с именем *Metadata.cs*.</span><span class="sxs-lookup"><span data-stu-id="35baa-141">In the **Models** folder, add a class named *Metadata.cs*.</span></span>

<span data-ttu-id="35baa-142">Замените код в *Metadata.cs* следующим кодом.</span><span class="sxs-lookup"><span data-stu-id="35baa-142">Replace the code in *Metadata.cs* with the following code.</span></span>

[!code-csharp[Main](enhancing-data-validation/samples/sample3.cs)]

<span data-ttu-id="35baa-143">Эти классы метаданных содержат все атрибуты проверки, которые ранее были применены к классам модели.</span><span class="sxs-lookup"><span data-stu-id="35baa-143">These metadata classes contain all of the validation attributes that you had previously applied to the model classes.</span></span> <span data-ttu-id="35baa-144">**Отображения** атрибута можно изменить значение, используемое для подписи.</span><span class="sxs-lookup"><span data-stu-id="35baa-144">The **Display** attribute is used to change the value used for text labels.</span></span>

<span data-ttu-id="35baa-145">Теперь классы моделей необходимо связать с классов метаданных.</span><span class="sxs-lookup"><span data-stu-id="35baa-145">Now, you must associate the model classes with the metadata classes.</span></span>

<span data-ttu-id="35baa-146">В **моделей** папки, добавьте класс с именем *PartialClasses.cs*.</span><span class="sxs-lookup"><span data-stu-id="35baa-146">In the **Models** folder, add a class named *PartialClasses.cs*.</span></span>

<span data-ttu-id="35baa-147">Замените содержимое файла следующим кодом.</span><span class="sxs-lookup"><span data-stu-id="35baa-147">Replace the contents of the file with the following code.</span></span>

[!code-csharp[Main](enhancing-data-validation/samples/sample4.cs)]

<span data-ttu-id="35baa-148">Обратите внимание, что каждый класс обозначен как `partial` класс и все совпадает с именем и пространством имен как класс, который создается автоматически.</span><span class="sxs-lookup"><span data-stu-id="35baa-148">Notice that each class is marked as a `partial` class, and each matches the name and namespace as the class that is automatically generated.</span></span> <span data-ttu-id="35baa-149">Применение атрибута метаданных в разделяемый класс, убедитесь, что атрибуты проверки данных будет применяться к классу автоматически создается.</span><span class="sxs-lookup"><span data-stu-id="35baa-149">By applying the metadata attribute to the partial class, you ensure that the data validation attributes will be applied to the automatically-generated class.</span></span> <span data-ttu-id="35baa-150">Эти атрибуты не будут потеряны при повторном создании классов модели, так как атрибут метаданных применяется в разделяемых классах, которые не восстанавливаются.</span><span class="sxs-lookup"><span data-stu-id="35baa-150">These attributes will not be lost when you regenerate the model classes because the metadata attribute is applied in partial classes that are not regenerated.</span></span>

<span data-ttu-id="35baa-151">Для повторного создания автоматически генерируемых классов, откройте *ContosoModel.edmx* файла.</span><span class="sxs-lookup"><span data-stu-id="35baa-151">To regenerate the automatically-generated classes, open the *ContosoModel.edmx* file.</span></span> <span data-ttu-id="35baa-152">Еще раз щелкните правой кнопкой мыши на область конструктора и выберите **обновить модель из базы данных**.</span><span class="sxs-lookup"><span data-stu-id="35baa-152">Once again, right-click on the design surface and select **Update Model from Database**.</span></span> <span data-ttu-id="35baa-153">Несмотря на то, что вы не изменили базы данных, этот процесс приведет к повторному созданию классов.</span><span class="sxs-lookup"><span data-stu-id="35baa-153">Even though you have not changed the database, this process will regenerate the classes.</span></span> <span data-ttu-id="35baa-154">В **обновить** выберите **таблиц** и **Готово**.</span><span class="sxs-lookup"><span data-stu-id="35baa-154">In the **Refresh** tab, select **Tables** and **Finish**.</span></span>

<span data-ttu-id="35baa-155">Сохранить *ContosoModel.edmx* файл, чтобы применить изменения.</span><span class="sxs-lookup"><span data-stu-id="35baa-155">Save the *ContosoModel.edmx* file to apply the changes.</span></span>

<span data-ttu-id="35baa-156">Откройте *Student.cs* файл или *Enrollment.cs* файл и обратите внимание, что ранее примененные атрибуты проверки данных больше не находятся в файле.</span><span class="sxs-lookup"><span data-stu-id="35baa-156">Open the *Student.cs* file or the *Enrollment.cs* file, and notice that the data validation attributes you applied earlier are no longer in the file.</span></span> <span data-ttu-id="35baa-157">Тем не менее запустите приложение и обратите внимание на то, что по-прежнему применяются правила проверки при вводе данных.</span><span class="sxs-lookup"><span data-stu-id="35baa-157">However, run the application, and notice that the validation rules are still applied when you enter data.</span></span>

## <a name="conclusion"></a><span data-ttu-id="35baa-158">Заключение</span><span class="sxs-lookup"><span data-stu-id="35baa-158">Conclusion</span></span>

<span data-ttu-id="35baa-159">В этой серии предоставлен простой пример того, как создать код из существующей базы данных, который позволяет пользователям изменять, обновлять, создавать и удалять данные.</span><span class="sxs-lookup"><span data-stu-id="35baa-159">This series provided a simple example of how to generate code from an existing database that enables users to edit, update, create and delete data.</span></span> <span data-ttu-id="35baa-160">Он используется ASP.NET MVC 5, Entity Framework и формирование шаблонов ASP.NET для создания проекта.</span><span class="sxs-lookup"><span data-stu-id="35baa-160">It used ASP.NET MVC 5, Entity Framework and ASP.NET Scaffolding to create the project.</span></span> 

<span data-ttu-id="35baa-161">Вводный пример шаблона разработки Code First, см. в разделе [Приступая к работе с ASP.NET MVC 5](../introduction/getting-started.md).</span><span class="sxs-lookup"><span data-stu-id="35baa-161">For an introductory example of Code First development, see [Getting Started with ASP.NET MVC 5](../introduction/getting-started.md).</span></span> 

<span data-ttu-id="35baa-162">Более сложный пример, см. в разделе [Создание модели данных Entity Framework для приложения ASP.NET MVC 4](../getting-started-with-ef-using-mvc/creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md).</span><span class="sxs-lookup"><span data-stu-id="35baa-162">For a more advanced example, see [Creating an Entity Framework Data Model for an ASP.NET MVC 4 App](../getting-started-with-ef-using-mvc/creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md).</span></span> <span data-ttu-id="35baa-163">Обратите внимание, что API DbContext, который используется для работы с данными в базе данных первого так же, как API, который используется для работы с данными в Code First.</span><span class="sxs-lookup"><span data-stu-id="35baa-163">Note that the DbContext API that you use for working with data in Database First is the same as the API you use for working with data in Code First.</span></span> <span data-ttu-id="35baa-164">Даже если вы планируете использовать Database First, рассказывается, как обрабатывать более сложные сценарии, такие как чтение и обновление связанных данных, обработка конфликтов параллелизма, и так далее из учебника Code First.</span><span class="sxs-lookup"><span data-stu-id="35baa-164">Even if you intend to use Database First, you can learn how to handle more complex scenarios such as reading and updating related data, handling concurrency conflicts, and so forth from a Code First tutorial.</span></span> <span data-ttu-id="35baa-165">Единственное различие — в том, как создать базу данных, класс контекста и классов сущностей.</span><span class="sxs-lookup"><span data-stu-id="35baa-165">The only difference is in how the database, context class, and entity classes are created.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="35baa-166">Дополнительные ресурсы</span><span class="sxs-lookup"><span data-stu-id="35baa-166">Additional resources</span></span>

<span data-ttu-id="35baa-167">Полный список проверки заметок к данным можно применить к свойства и классы, см. в разделе [System.ComponentModel.DataAnnotations](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.aspx).</span><span class="sxs-lookup"><span data-stu-id="35baa-167">For a full list of data validation annotations you can apply to properties and classes, see [System.ComponentModel.DataAnnotations](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.aspx).</span></span>

## <a name="next-steps"></a><span data-ttu-id="35baa-168">Следующие шаги</span><span class="sxs-lookup"><span data-stu-id="35baa-168">Next steps</span></span>

<span data-ttu-id="35baa-169">В этом учебнике рассмотрены следующие задачи.</span><span class="sxs-lookup"><span data-stu-id="35baa-169">In this tutorial, you:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="35baa-170">Заметки в добавленных данных</span><span class="sxs-lookup"><span data-stu-id="35baa-170">Added data annotations</span></span>
> * <span data-ttu-id="35baa-171">Добавляемые метаданные классов</span><span class="sxs-lookup"><span data-stu-id="35baa-171">Added metadata classes</span></span>

<span data-ttu-id="35baa-172">Дополнительные сведения о развертывании веб-приложения и базы данных SQL в службе приложений Azure, см. в разделе этого руководства:</span><span class="sxs-lookup"><span data-stu-id="35baa-172">To learn how to deploy a web app and SQL database to Azure App Service, see this tutorial:</span></span>
> [!div class="nextstepaction"]
> [<span data-ttu-id="35baa-173">Развертывание приложения .NET в службе приложений Azure</span><span class="sxs-lookup"><span data-stu-id="35baa-173">Deploy a .NET app to Azure App Service</span></span>](/azure/app-service/app-service-web-tutorial-dotnet-sqldatabase/)
