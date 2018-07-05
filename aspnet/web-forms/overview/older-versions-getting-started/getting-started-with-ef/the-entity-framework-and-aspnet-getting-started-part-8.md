---
uid: web-forms/overview/older-versions-getting-started/getting-started-with-ef/the-entity-framework-and-aspnet-getting-started-part-8
title: Начало работы с Entity Framework 4.0 Database First и ASP.NET 4 Web Forms - часть 8 | Документация Майкрософт
author: tdykstra
description: Пример веб-приложение университета Contoso демонстрирует создание приложений веб-форм ASP.NET, с помощью Entity Framework. Пример приложения является...
ms.author: aspnetcontent
manager: wpickett
ms.date: 12/03/2010
ms.topic: article
ms.assetid: aaadd9bb-5508-448c-ad57-5497dff90e13
ms.technology: dotnet-webforms
msc.legacyurl: /web-forms/overview/older-versions-getting-started/getting-started-with-ef/the-entity-framework-and-aspnet-getting-started-part-8
msc.type: authoredcontent
ms.openlocfilehash: f5ad2c1caf6036a0d8ee2ebbd07de60009090f1b
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 07/03/2018
ms.locfileid: "37374056"
---
<a name="getting-started-with-entity-framework-40-database-first-and-aspnet-4-web-forms---part-8"></a>Начало работы с Entity Framework 4.0 Database First и ASP.NET 4 Web Forms - часть 8
====================
по [том Дайкстра](https://github.com/tdykstra)

> Пример веб-приложение университета Contoso демонстрирует создание приложения веб-форм ASP.NET, используя Entity Framework 4.0 и Visual Studio 2010. Сведения об этой серии руководств см. в разделе [в первом учебнике серии](the-entity-framework-and-aspnet-getting-started-part-1.md)


## <a name="using-dynamic-data-functionality-to-format-and-validate-data"></a>С помощью функции динамических данных для форматирования и проверки данных

В предыдущем учебнике было реализовано хранимых процедур. Этом руководстве показано, как функциональные возможности платформы динамических данных может предоставить следующие преимущества:

- Поля автоматически форматируются для отображения на основе их типа данных.
- Поля автоматически проверяются на основе их типа данных.
- Можно добавить метаданные в модель данных для настройки поведения форматирования и проверки. При этом, вы можете добавить правила форматирования и проверки в одном месте, и они автоматически применяются везде вы получить доступ к полям, с помощью элементов управления платформы динамических данных.

Чтобы увидеть, как это работает, изменим элементов управления, использовать для отображения и изменения полей в существующем *Students.aspx* страницы и вы добавите форматирование и проверка метаданных поля имя и дата `Student` типа сущности.

[!["Image01"](the-entity-framework-and-aspnet-getting-started-part-8/_static/image2.png)](the-entity-framework-and-aspnet-getting-started-part-8/_static/image1.png)

## <a name="using-dynamicfield-and-dynamiccontrol-controls"></a>С помощью DynamicField и элементы управления DynamicControl

Откройте *Students.aspx* страницы и в `StudentsGridView` управления replace **имя** и **Дата регистрации** `TemplateField` элементов при помощи следующую разметку:

[!code-aspx[Main](the-entity-framework-and-aspnet-getting-started-part-8/samples/sample1.aspx)]

Эта разметка использует `DynamicControl` управляет вместо `TextBox` и `Label` поле шаблона имени элементов управления в учащегося, а он использует `DynamicField` управления для даты регистрации. Указаны не строки формата.

Добавить `ValidationSummary` элемента управления после `StudentsGridView` элемента управления.

[!code-aspx[Main](the-entity-framework-and-aspnet-getting-started-part-8/samples/sample2.aspx)]

В `SearchGridView` управления замените существующую разметку для **имя** и **Дата регистрации** столбцы, что вы сделали в `StudentsGridView` управления, за исключением того, опустите `EditItemTemplate` элемент. `Columns` Элемент `SearchGridView` управления теперь содержит следующую разметку:

[!code-aspx[Main](the-entity-framework-and-aspnet-getting-started-part-8/samples/sample3.aspx)]

Откройте *Students.aspx.cs* и добавьте следующие `using` инструкции:

[!code-csharp[Main](the-entity-framework-and-aspnet-getting-started-part-8/samples/sample4.cs)]

Добавить обработчик для страницы `Init` событий:

[!code-csharp[Main](the-entity-framework-and-aspnet-getting-started-part-8/samples/sample5.cs)]

Этот код указывает, что будет предоставлять динамических данных, форматирования и проверки в этих элементах управления с привязкой к данным для полей `Student` сущности. Если вы получаете сообщение об ошибке, как в следующем примере при выполнении страницы, обычно это значит, вы забыли вызвать `EnableDynamicData` метод в `Page_Init`:

`Could not determine a MetaTable. A MetaTable could not be determined for the data source 'StudentsEntityDataSource' and one could not be inferred from the request URL.`

Откройте страницу.

[![Image03](the-entity-framework-and-aspnet-getting-started-part-8/_static/image4.png)](the-entity-framework-and-aspnet-getting-started-part-8/_static/image3.png)

В **Дата регистрации** столбца, а также дата отображается время, поскольку тип свойства `DateTime`. Мы исправим это позже.

Теперь обратите внимание на то, что Dynamic Data автоматически предоставляет основные данные проверки. Например, щелкните **изменить**, очистить поля «Дата», нажмите кнопку **обновления**, и вы увидите, что динамических данных автоматически делает это обязательное поле, так как значение не допускает значения NULL, в модели данных. На странице отображается звездочка после поля и сообщение об ошибке в `ValidationSummary` управления:

[![Image05](the-entity-framework-and-aspnet-getting-started-part-8/_static/image6.png)](the-entity-framework-and-aspnet-getting-started-part-8/_static/image5.png)

Можно опустить `ValidationSummary` управления, так как можно также навести указатель мыши на звездочки, чтобы получить сообщение об ошибке:

[![Image06](the-entity-framework-and-aspnet-getting-started-part-8/_static/image8.png)](the-entity-framework-and-aspnet-getting-started-part-8/_static/image7.png)

Платформа динамических данных также будет проверять данные, введенные в **Дата регистрации** поле является допустимой датой:

[![Image04](the-entity-framework-and-aspnet-getting-started-part-8/_static/image10.png)](the-entity-framework-and-aspnet-getting-started-part-8/_static/image9.png)

Как вы видите, это общее сообщение об ошибке. В следующем разделе вы увидите, как настроить сообщения, а также проверки и правила форматирования.

## <a name="adding-metadata-to-the-data-model"></a>Добавление метаданных в модель данных

Как правило требуется настроить функциональные возможности, предоставляемые платформой динамических данных. Например можно изменить способ отображения данных и содержимое сообщения об ошибках. Обычно приходится также настраивать правила проверки данных, чтобы предоставить больше возможностей, чем Dynamic Data предоставляет автоматически на основе типов данных. Чтобы сделать это, создайте разделяемые классы, соответствующие типы сущностей.

В **обозревателе решений**, щелкните правой кнопкой мыши **ContosoUniversity** проекта, выберите **добавить ссылку**и добавьте ссылку на `System.ComponentModel.DataAnnotations`.

[![Image11](the-entity-framework-and-aspnet-getting-started-part-8/_static/image12.png)](the-entity-framework-and-aspnet-getting-started-part-8/_static/image11.png)

В *DAL* папки, создайте новый файл класса, назовите его *Student.cs*и замените код шаблона в его следующим кодом.

[!code-csharp[Main](the-entity-framework-and-aspnet-getting-started-part-8/samples/sample6.cs)]

Этот код создает разделяемый класс для `Student` сущности. `MetadataType` Атрибут, примененный к этот частичный класс определяет класс, который используется для задания метаданных. Класс метаданных может иметь любое имя, но с использованием имени сущности, а также «Метаданные» — это распространенная практика.

Атрибуты, примененные к свойствам в классе метаданных укажите форматирования, проверки, правила и сообщения об ошибках. Атрибуты, приведенные здесь будет иметь следующие результаты:

- `EnrollmentDate` будет отображаться как дата (без времени).
- Оба имени поля должны быть 25 символов или меньше длины, а также пользовательское сообщение об ошибке предоставляется.
- Оба имени поля являются обязательными, и предоставляется пользовательское сообщение об ошибке.

Запустите *Students.aspx* странице еще раз, и вы увидите, что даты теперь отображаются без времени:

[![Image08](the-entity-framework-and-aspnet-getting-started-part-8/_static/image14.png)](the-entity-framework-and-aspnet-getting-started-part-8/_static/image13.png)

Изменить строку и попытаться очистить значения в полях имен. Звездочки, указывающие на ошибки поля отображаются сразу же оставить поле, прежде чем нажимать кнопку **обновления**. При нажатии кнопки **обновления**, на странице отображается сообщение об ошибке, вы указали.

[![Image10](the-entity-framework-and-aspnet-getting-started-part-8/_static/image16.png)](the-entity-framework-and-aspnet-getting-started-part-8/_static/image15.png)

Попробуйте ввести имена которых не более 25 символов, щелкните **обновления**, и на странице отображается сообщение об ошибке, вы указали.

[![Image09](the-entity-framework-and-aspnet-getting-started-part-8/_static/image18.png)](the-entity-framework-and-aspnet-getting-started-part-8/_static/image17.png)

Теперь, когда вы настроили эти правила форматирования и проверки в метаданных модели данных, правила будут автоматически применяться на каждой странице, отображающая или позволяет вносить изменения в эти поля, до тех пор, пока используется `DynamicControl` или `DynamicField` элементов управления. Это уменьшает объем избыточный код, который необходимо написать, что делает программирование и упростит тестирование, и гарантирует, что данные форматирования и проверки они единообразны во всем приложении.

## <a name="more-information"></a>Дополнительные сведения

Это заключительный шаг этой серии руководств по началу работы с Entity Framework. Дополнительные ресурсы, которые помогут вам понять, как использовать Entity Framework, следует выполнить [в первом учебнике этой серии руководств Далее Entity Framework](../continuing-with-ef/using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started.md) или посетите следующие сайты:

- [Платформа Entity Framework часто задаваемые вопросы](http://www.ef-faq.org/introduction.html)
- [Блог группы разработчиков Entity Framework](https://blogs.msdn.com/b/adonet/)
- [Entity Framework в библиотеке MSDN](https://msdn.microsoft.com/library/bb399572.aspx)
- [Entity Framework в центре данных для разработчиков MSDN](https://msdn.microsoft.com/data/ef.aspx)
- [Обзор элемента управления EntityDataSource веб-сервера в библиотеке MSDN](https://msdn.microsoft.com/library/cc488502.aspx)
- [Элемент управления EntityDataSource Справочник по API в библиотеке MSDN](https://msdn.microsoft.com/library/system.web.ui.webcontrols.entitydatasource.aspx)
- [Entity Framework форумы MSDN, посвященные](https://social.msdn.microsoft.com/forums/adodotnetentityframework/)
- [Джули Лерман блог](http://thedatafarm.com/blog/)

> [!div class="step-by-step"]
> [Назад](the-entity-framework-and-aspnet-getting-started-part-7.md)
