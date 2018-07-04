---
uid: mvc/overview/getting-started/database-first-development/enhancing-data-validation
title: 'EF Database First с ASP.NET MVC: улучшение проверки данных | Документация Майкрософт'
author: tfitzmac
description: С помощью MVC, Entity Framework и формирование шаблонов ASP.NET, можно создать веб-приложение, которое предоставляет интерфейс для существующей базы данных. Этот учебник seri...
ms.author: aspnetcontent
manager: wpickett
ms.date: 12/29/2014
ms.topic: article
ms.assetid: 0ed5e67a-34c0-4b57-84a6-802b0fb3cd00
ms.technology: dotnet-mvc
msc.legacyurl: /mvc/overview/getting-started/database-first-development/enhancing-data-validation
msc.type: authoredcontent
ms.openlocfilehash: a328aa8aec2c512d77ddabec31b3785b8742e018
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 07/03/2018
ms.locfileid: "37376718"
---
<a name="ef-database-first-with-aspnet-mvc-enhancing-data-validation"></a><span data-ttu-id="86d04-104">EF Database First с ASP.NET MVC: улучшение проверки данных</span><span class="sxs-lookup"><span data-stu-id="86d04-104">EF Database First with ASP.NET MVC: Enhancing Data Validation</span></span>
====================
<span data-ttu-id="86d04-105">по [Tom FitzMacken](https://github.com/tfitzmac)</span><span class="sxs-lookup"><span data-stu-id="86d04-105">by [Tom FitzMacken](https://github.com/tfitzmac)</span></span>

> <span data-ttu-id="86d04-106">С помощью MVC, Entity Framework и формирование шаблонов ASP.NET, можно создать веб-приложение, которое предоставляет интерфейс для существующей базы данных.</span><span class="sxs-lookup"><span data-stu-id="86d04-106">Using MVC, Entity Framework, and ASP.NET Scaffolding, you can create a web application that provides an interface to an existing database.</span></span> <span data-ttu-id="86d04-107">В этой серии руководств показано, как автоматически создавать код, позволяющий пользователям для отображения, изменения и создавать и удалять данные, находящиеся в таблице базы данных.</span><span class="sxs-lookup"><span data-stu-id="86d04-107">This tutorial series shows you how to automatically generate code that enables users to display, edit, create, and delete data that resides in a database table.</span></span> <span data-ttu-id="86d04-108">Созданный код соответствует столбцам в таблице базы данных.</span><span class="sxs-lookup"><span data-stu-id="86d04-108">The generated code corresponds to the columns in the database table.</span></span>
> 
> <span data-ttu-id="86d04-109">Части этой серии рассматривается добавление заметок к данным в модель данных, чтобы указать требования к проверке и формат отображения.</span><span class="sxs-lookup"><span data-stu-id="86d04-109">This part of the series focuses on adding data annotations to the data model to specify validation requirements and display formatting.</span></span> <span data-ttu-id="86d04-110">Она была улучшена зависимости на отзывы от пользователей в разделе "Примечания".</span><span class="sxs-lookup"><span data-stu-id="86d04-110">It was improved based on feedback from users in the comments section.</span></span>


## <a name="add-data-annotations"></a><span data-ttu-id="86d04-111">Добавление заметок к данным</span><span class="sxs-lookup"><span data-stu-id="86d04-111">Add data annotations</span></span>

<span data-ttu-id="86d04-112">Как видно в предыдущей статье, некоторые правила проверки данных автоматически применяются к вводимых пользователем данных.</span><span class="sxs-lookup"><span data-stu-id="86d04-112">As you saw in an earlier topic, some data validation rules are automatically applied to the user input.</span></span> <span data-ttu-id="86d04-113">Например число может предоставить только для свойства корпоративного уровня.</span><span class="sxs-lookup"><span data-stu-id="86d04-113">For example, you can only provide a number for the Grade property.</span></span> <span data-ttu-id="86d04-114">Чтобы указать дополнительные правила проверки данных, можно добавить заметки к данным в классе модели.</span><span class="sxs-lookup"><span data-stu-id="86d04-114">To specify more data validation rules, you can add data annotations to your model class.</span></span> <span data-ttu-id="86d04-115">Эти заметки применяются в рамках всего веб-приложения для указанного свойства.</span><span class="sxs-lookup"><span data-stu-id="86d04-115">These annotations are applied throughout your web application for the specified property.</span></span> <span data-ttu-id="86d04-116">Можно также применить атрибуты форматирования, которые изменяют способ отображения свойств; например изменив значение, используемое для подписи.</span><span class="sxs-lookup"><span data-stu-id="86d04-116">You can also apply formatting attributes that change how the properties are displayed; such as, changing the value used for text labels.</span></span>

<span data-ttu-id="86d04-117">В этом руководстве вы добавите заметок к данным для ограничения длины значения, заданные для свойств FirstName, MiddleName и LastName.</span><span class="sxs-lookup"><span data-stu-id="86d04-117">In this tutorial, you will add data annotations to restrict the length of the values provided for the FirstName, LastName, and MiddleName properties.</span></span> <span data-ttu-id="86d04-118">В базе данных эти значения являются более 50 символов; Тем не менее в веб-приложении этого предельного количества символов в настоящее время не применяется.</span><span class="sxs-lookup"><span data-stu-id="86d04-118">In the database, these values are limited to 50 characters; however, in your web application that character limit is currently not enforced.</span></span> <span data-ttu-id="86d04-119">Если пользователь предоставляет более 50 символов для одного из этих значений, странице произойдет сбой при попытке сохранить значение в базе данных.</span><span class="sxs-lookup"><span data-stu-id="86d04-119">If a user provides more than 50 characters for one of those values, the page will crash when attempting to save the value to the database.</span></span> <span data-ttu-id="86d04-120">Также вы ограничите корпоративного уровня для значения в диапазоне от 0 до 4.</span><span class="sxs-lookup"><span data-stu-id="86d04-120">You will also restrict Grade to values between 0 and 4.</span></span>

<span data-ttu-id="86d04-121">Откройте **Student.cs** файл **моделей** папки.</span><span class="sxs-lookup"><span data-stu-id="86d04-121">Open the **Student.cs** file in the **Models** folder.</span></span> <span data-ttu-id="86d04-122">Добавьте выделенный ниже код к классу.</span><span class="sxs-lookup"><span data-stu-id="86d04-122">Add the following highlighted code to the class.</span></span>

[!code-csharp[Main](enhancing-data-validation/samples/sample1.cs?highlight=5,15,17,20)]

<span data-ttu-id="86d04-123">В Enrollment.cs добавьте следующий выделенный код.</span><span class="sxs-lookup"><span data-stu-id="86d04-123">In Enrollment.cs, add the following highlighted code.</span></span>

[!code-csharp[Main](enhancing-data-validation/samples/sample2.cs?highlight=5,10)]

<span data-ttu-id="86d04-124">Постройте решение.</span><span class="sxs-lookup"><span data-stu-id="86d04-124">Build the solution.</span></span>

<span data-ttu-id="86d04-125">Перейдите на страницу редактирования или создания учащегося.</span><span class="sxs-lookup"><span data-stu-id="86d04-125">Browse to a page for editing or creating a student.</span></span> <span data-ttu-id="86d04-126">Если попытаться ввести более 50 символов, отображается сообщение об ошибке.</span><span class="sxs-lookup"><span data-stu-id="86d04-126">If you attempt to enter more than 50 characters, an error message is displayed.</span></span>

![Отображение сообщения об ошибке](enhancing-data-validation/_static/image1.png)

<span data-ttu-id="86d04-128">Перейдите на страницу для редактирования регистраций и пытается предоставить оценку выше 4.</span><span class="sxs-lookup"><span data-stu-id="86d04-128">Browse to the page for editing enrollments, and attempt to provide a grade above 4.</span></span>

![Ошибка диапазона корпоративного уровня](enhancing-data-validation/_static/image2.png)

<span data-ttu-id="86d04-130">Полный список проверки заметок к данным можно применить к свойства и классы, см. в разделе [System.ComponentModel.DataAnnotations](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.aspx).</span><span class="sxs-lookup"><span data-stu-id="86d04-130">For a full list of data validation annotations you can apply to properties and classes, see [System.ComponentModel.DataAnnotations](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.aspx).</span></span>

## <a name="add-metadata-classes"></a><span data-ttu-id="86d04-131">Добавить классы метаданных</span><span class="sxs-lookup"><span data-stu-id="86d04-131">Add metadata classes</span></span>

<span data-ttu-id="86d04-132">Добавление атрибутов проверки непосредственно к классу модели работает, если вы не ожидаете, что базы данных для изменения; Тем не менее если изменения базы данных и вам нужно повторно создать класс модели, вы потеряете все атрибуты, которые были применены к классу модели.</span><span class="sxs-lookup"><span data-stu-id="86d04-132">Adding the validation attributes directly to the model class works when you do not expect the database to change; however, if your database changes and you need to regenerate the model class, you will lose all of the attributes you had applied to the model class.</span></span> <span data-ttu-id="86d04-133">Этот подход может оказаться очень неэффективным и подвержен потери важных условий.</span><span class="sxs-lookup"><span data-stu-id="86d04-133">This approach can be very inefficient and prone to losing important validation rules.</span></span>

<span data-ttu-id="86d04-134">Чтобы избежать этой проблемы, можно добавить класс метаданных, содержащий атрибуты.</span><span class="sxs-lookup"><span data-stu-id="86d04-134">To avoid this problem, you can add a metadata class that contains the attributes.</span></span> <span data-ttu-id="86d04-135">При связывании класса модели к классу метаданных, эти атрибуты применяются к модели.</span><span class="sxs-lookup"><span data-stu-id="86d04-135">When you associate the model class to the metadata class, those attributes are applied to the model.</span></span> <span data-ttu-id="86d04-136">В этом случае можно повторно создать класс модели без потери всех атрибутов, примененных к классу метаданных.</span><span class="sxs-lookup"><span data-stu-id="86d04-136">In this approach, the model class can be regenerated without losing all of the attributes that have been applied to the metadata class.</span></span>

<span data-ttu-id="86d04-137">В **моделей** папки, добавьте класс с именем **Metadata.cs**.</span><span class="sxs-lookup"><span data-stu-id="86d04-137">In the **Models** folder, add a class named **Metadata.cs**.</span></span>

![Добавление класса метаданных](enhancing-data-validation/_static/image3.png)

<span data-ttu-id="86d04-139">Замените код в Metadata.cs следующим кодом.</span><span class="sxs-lookup"><span data-stu-id="86d04-139">Replace the code in Metadata.cs with the following code.</span></span>

[!code-csharp[Main](enhancing-data-validation/samples/sample3.cs)]

<span data-ttu-id="86d04-140">Эти классы метаданных содержат все атрибуты проверки, которые ранее были применены к классам модели.</span><span class="sxs-lookup"><span data-stu-id="86d04-140">These metadata classes contain all of the validation attributes that you had previously applied to the model classes.</span></span> <span data-ttu-id="86d04-141">**Отображения** атрибута можно изменить значение, используемое для подписи.</span><span class="sxs-lookup"><span data-stu-id="86d04-141">The **Display** attribute is used to change the value used for text labels.</span></span>

<span data-ttu-id="86d04-142">Теперь классы моделей необходимо связать с классов метаданных.</span><span class="sxs-lookup"><span data-stu-id="86d04-142">Now, you must associate the model classes with the metadata classes.</span></span>

<span data-ttu-id="86d04-143">В **моделей** папки, добавьте класс с именем **PartialClasses.cs**.</span><span class="sxs-lookup"><span data-stu-id="86d04-143">In the **Models** folder, add a class named **PartialClasses.cs**.</span></span>

<span data-ttu-id="86d04-144">Замените содержимое файла следующим кодом.</span><span class="sxs-lookup"><span data-stu-id="86d04-144">Replace the contents of the file with the following code.</span></span>

[!code-csharp[Main](enhancing-data-validation/samples/sample4.cs)]

<span data-ttu-id="86d04-145">Обратите внимание, что каждый класс обозначен как `partial` класс и все совпадает с именем и пространством имен как класс, который создается автоматически.</span><span class="sxs-lookup"><span data-stu-id="86d04-145">Notice that each class is marked as a `partial` class, and each matches the name and namespace as the class that is automatically generated.</span></span> <span data-ttu-id="86d04-146">Применение атрибута метаданных в разделяемый класс, убедитесь, что атрибуты проверки данных будет применяться к классу автоматически создается.</span><span class="sxs-lookup"><span data-stu-id="86d04-146">By applying the metadata attribute to the partial class, you ensure that the data validation attributes will be applied to the automatically-generated class.</span></span> <span data-ttu-id="86d04-147">Эти атрибуты не будут потеряны при повторном создании классов модели, так как атрибут метаданных применяется в разделяемых классах, которые не восстанавливаются.</span><span class="sxs-lookup"><span data-stu-id="86d04-147">These attributes will not be lost when you regenerate the model classes because the metadata attribute is applied in partial classes that are not regenerated.</span></span>

<span data-ttu-id="86d04-148">Для повторного создания автоматически генерируемых классов, откройте файл ContosoModel.edmx.</span><span class="sxs-lookup"><span data-stu-id="86d04-148">To regenerate the automatically-generated classes, open the ContosoModel.edmx file.</span></span> <span data-ttu-id="86d04-149">Еще раз щелкните правой кнопкой мыши на область конструктора и выберите **обновить модель из базы данных**.</span><span class="sxs-lookup"><span data-stu-id="86d04-149">Once again, right-click on the design surface and select **Update Model from Database**.</span></span> <span data-ttu-id="86d04-150">Несмотря на то, что вы не изменили базы данных, этот процесс приведет к повторному созданию классов.</span><span class="sxs-lookup"><span data-stu-id="86d04-150">Even though you have not changed the database, this process will regenerate the classes.</span></span> <span data-ttu-id="86d04-151">В **обновить** выберите **таблиц** и **Готово**.</span><span class="sxs-lookup"><span data-stu-id="86d04-151">In the **Refresh** tab, select **Tables** and **Finish**.</span></span>

![обновить таблицы](enhancing-data-validation/_static/image4.png)

<span data-ttu-id="86d04-153">Сохраните файл ContosoModel.edmx, чтобы применить изменения.</span><span class="sxs-lookup"><span data-stu-id="86d04-153">Save the ContosoModel.edmx file to apply the changes.</span></span>

<span data-ttu-id="86d04-154">Откройте файл Student.cs или Enrollment.cs и обратите внимание на то, что атрибуты проверки данных, которые были применены ранее, больше не находятся в файле.</span><span class="sxs-lookup"><span data-stu-id="86d04-154">Open the Student.cs file or the Enrollment.cs file, and notice that the data validation attributes you applied earlier are no longer in the file.</span></span> <span data-ttu-id="86d04-155">Тем не менее запустите приложение и обратите внимание на то, что по-прежнему применяются правила проверки при вводе данных.</span><span class="sxs-lookup"><span data-stu-id="86d04-155">However, run the application, and notice that the validation rules are still applied when you enter data.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="86d04-156">[Назад](customizing-a-view.md)
> [Вперед](publish-to-azure.md)</span><span class="sxs-lookup"><span data-stu-id="86d04-156">[Previous](customizing-a-view.md)
[Next](publish-to-azure.md)</span></span>
