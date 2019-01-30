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
ms.openlocfilehash: 85299d70c6cba52c1d40a42edfd429c96318134a
ms.sourcegitcommit: c47d7c131eebbcd8811e31edda210d64cf4b9d6b
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/30/2019
ms.locfileid: "55236488"
---
# <a name="tutorial-enhance-data-validation-for-ef-database-first-with-aspnet-mvc-app"></a><span data-ttu-id="723f9-103">Учебник. Улучшения проверки данных для EF Database First с помощью приложения ASP.NET MVC</span><span class="sxs-lookup"><span data-stu-id="723f9-103">Tutorial: Enhance data validation for EF Database First with ASP.NET MVC app</span></span>

<span data-ttu-id="723f9-104">С помощью MVC, Entity Framework и формирование шаблонов ASP.NET, можно создать веб-приложение, которое предоставляет интерфейс для существующей базы данных.</span><span class="sxs-lookup"><span data-stu-id="723f9-104">Using MVC, Entity Framework, and ASP.NET Scaffolding, you can create a web application that provides an interface to an existing database.</span></span> <span data-ttu-id="723f9-105">В этой серии руководств показано, как автоматически создавать код, позволяющий пользователям для отображения, изменения и создавать и удалять данные, находящиеся в таблице базы данных.</span><span class="sxs-lookup"><span data-stu-id="723f9-105">This tutorial series shows you how to automatically generate code that enables users to display, edit, create, and delete data that resides in a database table.</span></span> <span data-ttu-id="723f9-106">Созданный код соответствует столбцам в таблице базы данных.</span><span class="sxs-lookup"><span data-stu-id="723f9-106">The generated code corresponds to the columns in the database table.</span></span>

<span data-ttu-id="723f9-107">Это руководство посвящено добавления заметок к данным в модель данных, чтобы указать требования к проверке и формат отображения.</span><span class="sxs-lookup"><span data-stu-id="723f9-107">This tutorial focuses on adding data annotations to the data model to specify validation requirements and display formatting.</span></span> <span data-ttu-id="723f9-108">Она была улучшена зависимости на отзывы от пользователей в разделе "Примечания".</span><span class="sxs-lookup"><span data-stu-id="723f9-108">It was improved based on feedback from users in the comments section.</span></span>

<span data-ttu-id="723f9-109">В этом учебнике рассмотрены следующие задачи.</span><span class="sxs-lookup"><span data-stu-id="723f9-109">In this tutorial, you:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="723f9-110">Добавление заметок к данным</span><span class="sxs-lookup"><span data-stu-id="723f9-110">Add data annotations</span></span>
> * <span data-ttu-id="723f9-111">Добавить классы метаданных</span><span class="sxs-lookup"><span data-stu-id="723f9-111">Add metadata classes</span></span>

## <a name="prerequisites"></a><span data-ttu-id="723f9-112">Предварительные требования</span><span class="sxs-lookup"><span data-stu-id="723f9-112">Prerequisites</span></span>

* [<span data-ttu-id="723f9-113">Настройка представления</span><span class="sxs-lookup"><span data-stu-id="723f9-113">Customize a view</span></span>](customizing-a-view.md)

## <a name="add-data-annotations"></a><span data-ttu-id="723f9-114">Добавление заметок к данным</span><span class="sxs-lookup"><span data-stu-id="723f9-114">Add data annotations</span></span>

<span data-ttu-id="723f9-115">Как видно в предыдущей статье, некоторые правила проверки данных автоматически применяются к вводимых пользователем данных.</span><span class="sxs-lookup"><span data-stu-id="723f9-115">As you saw in an earlier topic, some data validation rules are automatically applied to the user input.</span></span> <span data-ttu-id="723f9-116">Например число может предоставить только для свойства корпоративного уровня.</span><span class="sxs-lookup"><span data-stu-id="723f9-116">For example, you can only provide a number for the Grade property.</span></span> <span data-ttu-id="723f9-117">Чтобы указать дополнительные правила проверки данных, можно добавить заметки к данным в классе модели.</span><span class="sxs-lookup"><span data-stu-id="723f9-117">To specify more data validation rules, you can add data annotations to your model class.</span></span> <span data-ttu-id="723f9-118">Эти заметки применяются в рамках всего веб-приложения для указанного свойства.</span><span class="sxs-lookup"><span data-stu-id="723f9-118">These annotations are applied throughout your web application for the specified property.</span></span> <span data-ttu-id="723f9-119">Можно также применить атрибуты форматирования, которые изменяют способ отображения свойств; например изменив значение, используемое для подписи.</span><span class="sxs-lookup"><span data-stu-id="723f9-119">You can also apply formatting attributes that change how the properties are displayed; such as, changing the value used for text labels.</span></span>

<span data-ttu-id="723f9-120">В этом руководстве вы добавите заметок к данным для ограничения длины значения, заданные для свойств FirstName, MiddleName и LastName.</span><span class="sxs-lookup"><span data-stu-id="723f9-120">In this tutorial, you will add data annotations to restrict the length of the values provided for the FirstName, LastName, and MiddleName properties.</span></span> <span data-ttu-id="723f9-121">В базе данных эти значения являются более 50 символов; Тем не менее в веб-приложении этого предельного количества символов в настоящее время не применяется.</span><span class="sxs-lookup"><span data-stu-id="723f9-121">In the database, these values are limited to 50 characters; however, in your web application that character limit is currently not enforced.</span></span> <span data-ttu-id="723f9-122">Если пользователь предоставляет более 50 символов для одного из этих значений, странице произойдет сбой при попытке сохранить значение в базе данных.</span><span class="sxs-lookup"><span data-stu-id="723f9-122">If a user provides more than 50 characters for one of those values, the page will crash when attempting to save the value to the database.</span></span> <span data-ttu-id="723f9-123">Также вы ограничите корпоративного уровня для значения в диапазоне от 0 до 4.</span><span class="sxs-lookup"><span data-stu-id="723f9-123">You will also restrict Grade to values between 0 and 4.</span></span>

<span data-ttu-id="723f9-124">Выберите **моделей** > **ContosoModel.edmx** > **ContosoModel.tt** и откройте *Student.cs* файла.</span><span class="sxs-lookup"><span data-stu-id="723f9-124">Select **Models** > **ContosoModel.edmx** > **ContosoModel.tt** and open the *Student.cs* file.</span></span> <span data-ttu-id="723f9-125">Добавьте выделенный ниже код к классу.</span><span class="sxs-lookup"><span data-stu-id="723f9-125">Add the following highlighted code to the class.</span></span>

[!code-csharp[Main](enhancing-data-validation/samples/sample1.cs?highlight=5,15,17,20)]

<span data-ttu-id="723f9-126">Откройте *Enrollment.cs* и добавьте следующий выделенный код.</span><span class="sxs-lookup"><span data-stu-id="723f9-126">Open *Enrollment.cs* and add the following highlighted code.</span></span>

[!code-csharp[Main](enhancing-data-validation/samples/sample2.cs?highlight=5,10)]

<span data-ttu-id="723f9-127">Постройте решение.</span><span class="sxs-lookup"><span data-stu-id="723f9-127">Build the solution.</span></span>

<span data-ttu-id="723f9-128">Нажмите кнопку **список учащихся** и выберите **изменить**.</span><span class="sxs-lookup"><span data-stu-id="723f9-128">Click **List of students** and select **Edit**.</span></span> <span data-ttu-id="723f9-129">Если попытаться ввести более 50 символов, отображается сообщение об ошибке.</span><span class="sxs-lookup"><span data-stu-id="723f9-129">If you attempt to enter more than 50 characters, an error message is displayed.</span></span>

![Отображение сообщения об ошибке](enhancing-data-validation/_static/image1.png)

<span data-ttu-id="723f9-131">Вернитесь на домашнюю страницу.</span><span class="sxs-lookup"><span data-stu-id="723f9-131">Go back to the home page.</span></span> <span data-ttu-id="723f9-132">Нажмите кнопку **список регистраций** и выберите **изменить**.</span><span class="sxs-lookup"><span data-stu-id="723f9-132">Click **List of enrollments** and select **Edit**.</span></span> <span data-ttu-id="723f9-133">Пытается предоставить оценку выше 4.</span><span class="sxs-lookup"><span data-stu-id="723f9-133">Attempt to provide a grade above 4.</span></span> <span data-ttu-id="723f9-134">Вы получите эту ошибку: *Поле корпоративного класса должно быть от 0 до 4.*</span><span class="sxs-lookup"><span data-stu-id="723f9-134">You will receive this error: *The field Grade must be between 0 and 4.*</span></span>

## <a name="add-metadata-classes"></a><span data-ttu-id="723f9-135">Добавить классы метаданных</span><span class="sxs-lookup"><span data-stu-id="723f9-135">Add metadata classes</span></span>

<span data-ttu-id="723f9-136">Добавление атрибутов проверки непосредственно к классу модели работает, если вы не ожидаете, что базы данных для изменения; Тем не менее если изменения базы данных и вам нужно повторно создать класс модели, вы потеряете все атрибуты, которые были применены к классу модели.</span><span class="sxs-lookup"><span data-stu-id="723f9-136">Adding the validation attributes directly to the model class works when you do not expect the database to change; however, if your database changes and you need to regenerate the model class, you will lose all of the attributes you had applied to the model class.</span></span> <span data-ttu-id="723f9-137">Этот подход может оказаться очень неэффективным и подвержен потери важных условий.</span><span class="sxs-lookup"><span data-stu-id="723f9-137">This approach can be very inefficient and prone to losing important validation rules.</span></span>

<span data-ttu-id="723f9-138">Чтобы избежать этой проблемы, можно добавить класс метаданных, содержащий атрибуты.</span><span class="sxs-lookup"><span data-stu-id="723f9-138">To avoid this problem, you can add a metadata class that contains the attributes.</span></span> <span data-ttu-id="723f9-139">При связывании класса модели к классу метаданных, эти атрибуты применяются к модели.</span><span class="sxs-lookup"><span data-stu-id="723f9-139">When you associate the model class to the metadata class, those attributes are applied to the model.</span></span> <span data-ttu-id="723f9-140">В этом случае можно повторно создать класс модели без потери всех атрибутов, примененных к классу метаданных.</span><span class="sxs-lookup"><span data-stu-id="723f9-140">In this approach, the model class can be regenerated without losing all of the attributes that have been applied to the metadata class.</span></span>

<span data-ttu-id="723f9-141">В **моделей** папки, добавьте класс с именем *Metadata.cs*.</span><span class="sxs-lookup"><span data-stu-id="723f9-141">In the **Models** folder, add a class named *Metadata.cs*.</span></span>

<span data-ttu-id="723f9-142">Замените код в *Metadata.cs* следующим кодом.</span><span class="sxs-lookup"><span data-stu-id="723f9-142">Replace the code in *Metadata.cs* with the following code.</span></span>

[!code-csharp[Main](enhancing-data-validation/samples/sample3.cs)]

<span data-ttu-id="723f9-143">Эти классы метаданных содержат все атрибуты проверки, которые ранее были применены к классам модели.</span><span class="sxs-lookup"><span data-stu-id="723f9-143">These metadata classes contain all of the validation attributes that you had previously applied to the model classes.</span></span> <span data-ttu-id="723f9-144">**Отображения** атрибута можно изменить значение, используемое для подписи.</span><span class="sxs-lookup"><span data-stu-id="723f9-144">The **Display** attribute is used to change the value used for text labels.</span></span>

<span data-ttu-id="723f9-145">Теперь классы моделей необходимо связать с классов метаданных.</span><span class="sxs-lookup"><span data-stu-id="723f9-145">Now, you must associate the model classes with the metadata classes.</span></span>

<span data-ttu-id="723f9-146">В **моделей** папки, добавьте класс с именем *PartialClasses.cs*.</span><span class="sxs-lookup"><span data-stu-id="723f9-146">In the **Models** folder, add a class named *PartialClasses.cs*.</span></span>

<span data-ttu-id="723f9-147">Замените содержимое файла следующим кодом.</span><span class="sxs-lookup"><span data-stu-id="723f9-147">Replace the contents of the file with the following code.</span></span>

[!code-csharp[Main](enhancing-data-validation/samples/sample4.cs)]

<span data-ttu-id="723f9-148">Обратите внимание, что каждый класс обозначен как `partial` класс и все совпадает с именем и пространством имен как класс, который создается автоматически.</span><span class="sxs-lookup"><span data-stu-id="723f9-148">Notice that each class is marked as a `partial` class, and each matches the name and namespace as the class that is automatically generated.</span></span> <span data-ttu-id="723f9-149">Применение атрибута метаданных в разделяемый класс, убедитесь, что атрибуты проверки данных будет применяться к классу автоматически создается.</span><span class="sxs-lookup"><span data-stu-id="723f9-149">By applying the metadata attribute to the partial class, you ensure that the data validation attributes will be applied to the automatically-generated class.</span></span> <span data-ttu-id="723f9-150">Эти атрибуты не будут потеряны при повторном создании классов модели, так как атрибут метаданных применяется в разделяемых классах, которые не восстанавливаются.</span><span class="sxs-lookup"><span data-stu-id="723f9-150">These attributes will not be lost when you regenerate the model classes because the metadata attribute is applied in partial classes that are not regenerated.</span></span>

<span data-ttu-id="723f9-151">Для повторного создания автоматически генерируемых классов, откройте *ContosoModel.edmx* файла.</span><span class="sxs-lookup"><span data-stu-id="723f9-151">To regenerate the automatically-generated classes, open the *ContosoModel.edmx* file.</span></span> <span data-ttu-id="723f9-152">Еще раз щелкните правой кнопкой мыши на область конструктора и выберите **обновить модель из базы данных**.</span><span class="sxs-lookup"><span data-stu-id="723f9-152">Once again, right-click on the design surface and select **Update Model from Database**.</span></span> <span data-ttu-id="723f9-153">Несмотря на то, что вы не изменили базы данных, этот процесс приведет к повторному созданию классов.</span><span class="sxs-lookup"><span data-stu-id="723f9-153">Even though you have not changed the database, this process will regenerate the classes.</span></span> <span data-ttu-id="723f9-154">В **обновить** выберите **таблиц** и **Готово**.</span><span class="sxs-lookup"><span data-stu-id="723f9-154">In the **Refresh** tab, select **Tables** and **Finish**.</span></span>

<span data-ttu-id="723f9-155">Сохранить *ContosoModel.edmx* файл, чтобы применить изменения.</span><span class="sxs-lookup"><span data-stu-id="723f9-155">Save the *ContosoModel.edmx* file to apply the changes.</span></span>

<span data-ttu-id="723f9-156">Откройте *Student.cs* файл или *Enrollment.cs* файл и обратите внимание, что ранее примененные атрибуты проверки данных больше не находятся в файле.</span><span class="sxs-lookup"><span data-stu-id="723f9-156">Open the *Student.cs* file or the *Enrollment.cs* file, and notice that the data validation attributes you applied earlier are no longer in the file.</span></span> <span data-ttu-id="723f9-157">Тем не менее запустите приложение и обратите внимание на то, что по-прежнему применяются правила проверки при вводе данных.</span><span class="sxs-lookup"><span data-stu-id="723f9-157">However, run the application, and notice that the validation rules are still applied when you enter data.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="723f9-158">Дополнительные ресурсы</span><span class="sxs-lookup"><span data-stu-id="723f9-158">Additional resources</span></span>

<span data-ttu-id="723f9-159">Полный список проверки заметок к данным можно применить к свойства и классы, см. в разделе [System.ComponentModel.DataAnnotations](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.aspx).</span><span class="sxs-lookup"><span data-stu-id="723f9-159">For a full list of data validation annotations you can apply to properties and classes, see [System.ComponentModel.DataAnnotations](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.aspx).</span></span>

## <a name="next-steps"></a><span data-ttu-id="723f9-160">Следующие шаги</span><span class="sxs-lookup"><span data-stu-id="723f9-160">Next steps</span></span>

<span data-ttu-id="723f9-161">В этом учебнике рассмотрены следующие задачи.</span><span class="sxs-lookup"><span data-stu-id="723f9-161">In this tutorial, you:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="723f9-162">Заметки в добавленных данных</span><span class="sxs-lookup"><span data-stu-id="723f9-162">Added data annotations</span></span>
> * <span data-ttu-id="723f9-163">Добавляемые метаданные классов</span><span class="sxs-lookup"><span data-stu-id="723f9-163">Added metadata classes</span></span>

<span data-ttu-id="723f9-164">Перейдите к следующему руководству, чтобы узнать, как опубликовать веб-приложения и базы данных в Azure.</span><span class="sxs-lookup"><span data-stu-id="723f9-164">Advance to the next tutorial to learn how to publish the web app and database to Azure.</span></span>
> [!div class="nextstepaction"]
> [<span data-ttu-id="723f9-165">Публикация в Azure</span><span class="sxs-lookup"><span data-stu-id="723f9-165">Publish to Azure</span></span>](publish-to-azure.md)