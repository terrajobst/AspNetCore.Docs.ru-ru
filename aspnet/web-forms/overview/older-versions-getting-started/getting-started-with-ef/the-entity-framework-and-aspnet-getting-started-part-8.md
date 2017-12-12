---
uid: web-forms/overview/older-versions-getting-started/getting-started-with-ef/the-entity-framework-and-aspnet-getting-started-part-8
title: "Приступая к работе с базой данных Entity Framework 4.0 сначала и ASP.NET 4, веб-формы - часть 8 | Документы Microsoft"
author: tdykstra
description: "Contoso университета примера веб-приложения показано, как для создания приложений веб-форм ASP.NET, использующий Entity Framework. Это образец приложения..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 12/03/2010
ms.topic: article
ms.assetid: aaadd9bb-5508-448c-ad57-5497dff90e13
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/older-versions-getting-started/getting-started-with-ef/the-entity-framework-and-aspnet-getting-started-part-8
msc.type: authoredcontent
ms.openlocfilehash: 78a54fb1b0e4c0ebd5445cca175103471925a065
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 11/10/2017
---
<a name="getting-started-with-entity-framework-40-database-first-and-aspnet-4-web-forms---part-8"></a>Приступая к работе с базой данных Entity Framework 4.0 сначала и форм ASP.NET 4 веб - часть 8
====================
По [Tom Dykstra](https://github.com/tdykstra)

> Contoso университета примера веб-приложения показано, как для создания приложений веб-форм ASP.NET, с помощью Entity Framework 4.0 и Visual Studio 2010. Сведения о учебника серии см [в первом учебнике ряда](the-entity-framework-and-aspnet-getting-started-part-1.md)


## <a name="using-dynamic-data-functionality-to-format-and-validate-data"></a>С помощью функций платформы динамических данных для форматирования и проверки данных

В предыдущем учебнике реализации хранимых процедур. Этот учебник будет показано, как функциональные возможности платформы динамических данных может предоставить следующие преимущества:

- Поля форматируются автоматически для отображения на основе их типа данных.
- Поля автоматически проверяются на основе их типа данных.
- Метаданные можно добавить в модель данных для настройки поведения форматирования и проверки. Если это сделать, можно добавить правила форматирования и проверки в одном месте, и они автоматически выполняется применяться везде доступ к поля, с помощью элементов управления платформы динамических данных.

Чтобы увидеть, как это работает, изменим элементов управления, использовать для отображения и редактирования поля в существующем *Students.aspx* страницы и вы добавите форматирования и проверки метаданных поля имя и дата `Student` типа сущности.

[!["Image01"](the-entity-framework-and-aspnet-getting-started-part-8/_static/image2.png)](the-entity-framework-and-aspnet-getting-started-part-8/_static/image1.png)

## <a name="using-dynamicfield-and-dynamiccontrol-controls"></a>С помощью DynamicField и элементы управления DynamicControl

Откройте *Students.aspx* страницы и в `StudentsGridView` управления замены **имя** и **Дата регистрации** `TemplateField` элементы следующей разметкой:

[!code-aspx[Main](the-entity-framework-and-aspnet-getting-started-part-8/samples/sample1.aspx)]

Эта разметка использует `DynamicControl` управляет вместо `TextBox` и `Label` элементов управления в ученик имя шаблона поля, а также `DynamicField` управления для даты регистрации. Указаны никакие строки формата.

Добавить `ValidationSummary` элемента управления после `StudentsGridView` элемента управления.

[!code-aspx[Main](the-entity-framework-and-aspnet-getting-started-part-8/samples/sample2.aspx)]

В `SearchGridView` управления замените существующую разметку для **имя** и **Дата регистрации** столбцы, что вы делали на `StudentsGridView` управления, за исключением опустить `EditItemTemplate` элемента. `Columns` Элемент `SearchGridView` управления теперь содержит следующую разметку:

[!code-aspx[Main](the-entity-framework-and-aspnet-getting-started-part-8/samples/sample3.aspx)]

Откройте *Students.aspx.cs* и добавьте следующее `using` инструкции:

[!code-csharp[Main](the-entity-framework-and-aspnet-getting-started-part-8/samples/sample4.cs)]

Добавление обработчика для этой страницы `Init` событий:

[!code-csharp[Main](the-entity-framework-and-aspnet-getting-started-part-8/samples/sample5.cs)]

Этот код указывает, что обеспечит динамических данных, форматирования и проверки в этих элементах управления с привязкой к данным для полей `Student` сущности. Если вы получаете сообщение об ошибке, как в следующем примере при выполнении страницы, обычно это значит, вы забыли вызвать `EnableDynamicData` метод в `Page_Init`:

`Could not determine a MetaTable. A MetaTable could not be determined for the data source 'StudentsEntityDataSource' and one could not be inferred from the request URL.`

Запустите страницу.

[![Image03](the-entity-framework-and-aspnet-getting-started-part-8/_static/image4.png)](the-entity-framework-and-aspnet-getting-started-part-8/_static/image3.png)

В **Дата регистрации** столбец отображения времени и даты, так как тип свойства `DateTime`. Это будет исправлено позже.

Теперь обратите внимание, что динамических данных обеспечивает автоматическую проверку данных. Например, щелкните **изменить**очистить поля «Дата», нажмите кнопку **обновление**, и вы увидите, что динамических данных автоматически делает это обязательное поле потому, что значение не допускает значения NULL в модели данных. На странице отображается звездочка после поля и сообщение об ошибке в `ValidationSummary` управления:

[![Image05](the-entity-framework-and-aspnet-getting-started-part-8/_static/image6.png)](the-entity-framework-and-aspnet-getting-started-part-8/_static/image5.png)

Можно опустить `ValidationSummary` поскольку элемент управления, также можно навести указатель мыши на звездочку, чтобы просмотреть сообщение об ошибке:

[![Image06](the-entity-framework-and-aspnet-getting-started-part-8/_static/image8.png)](the-entity-framework-and-aspnet-getting-started-part-8/_static/image7.png)

Платформа динамических данных также будет проверять данные, введенные в **Дата регистрации** поле представляет собой допустимую дату:

[![Image04](the-entity-framework-and-aspnet-getting-started-part-8/_static/image10.png)](the-entity-framework-and-aspnet-getting-started-part-8/_static/image9.png)

Как видите, это универсальное сообщение об ошибке. В следующем разделе вы увидите, как настроить сообщения, а также проверки и правила форматирования.

## <a name="adding-metadata-to-the-data-model"></a>Добавление метаданных в модель данных

Как правило требуется настроить функциональные возможности, предоставляемые платформой динамических данных. Например можно изменить способ отображения данных и содержимое сообщения об ошибках. Обычно приходится также настраивать правила проверки данных, чтобы предоставить больше возможностей, чем предоставляет платформа динамических данных автоматически на основе типов данных. Чтобы сделать это, создайте разделяемых классов, которые соответствуют типам сущностей.

В **обозревателе решений**, щелкните правой кнопкой мыши **ContosoUniversity** проекта, выберите **добавить ссылку**и добавьте ссылку на `System.ComponentModel.DataAnnotations`.

[![Image11](the-entity-framework-and-aspnet-getting-started-part-8/_static/image12.png)](the-entity-framework-and-aspnet-getting-started-part-8/_static/image11.png)

В *DAL* папки, создайте новый файл класса, назовите его *Student.cs*и замените код шаблона в него следующий код.

[!code-csharp[Main](the-entity-framework-and-aspnet-getting-started-part-8/samples/sample6.cs)]

Этот код создает разделяемый класс для `Student` сущности. `MetadataType` Атрибут, примененный к этот разделяемый класс определяет класс, который используется для задания метаданных. Класс метаданных может иметь любое имя, но с помощью имя сущности, а также «Метаданные» — это распространенная практика.

Атрибуты, применяемые к свойствам в классе метаданных указать форматирование, проверки, правила и сообщения об ошибках. Атрибуты, приведенные здесь имеет следующие результаты:

- `EnrollmentDate`отобразится дата (без времени).
- Оба имени поля должны быть 25 символов или меньше и сообщения о пользовательской ошибке предоставляется.
- Оба имени поля являются обязательными, и предоставляется пользовательское сообщение об ошибке.

Запустите *Students.aspx* страницу еще раз и разделе, теперь отображаются даты без времени:

[![Image08](the-entity-framework-and-aspnet-getting-started-part-8/_static/image14.png)](the-entity-framework-and-aspnet-getting-started-part-8/_static/image13.png)

Изменить строку и попытайтесь удалить значения в полях имен. Звездочки, указывающие на ошибки поля отображаются по мере оставить поле, прежде чем нажимать кнопку **обновление**. При нажатии кнопки **обновление**, на странице отображается указанное сообщение об ошибке.

[![Image10](the-entity-framework-and-aspnet-getting-started-part-8/_static/image16.png)](the-entity-framework-and-aspnet-getting-started-part-8/_static/image15.png)

Попробуйте ввести имена, которые не более 25 символов, нажмите кнопку **обновление**, и на странице отображается указанное сообщение об ошибке.

[![Image09](the-entity-framework-and-aspnet-getting-started-part-8/_static/image18.png)](the-entity-framework-and-aspnet-getting-started-part-8/_static/image17.png)

Теперь, когда вы настроили эти правила форматирования и проверки в метаданных модели данных, правила будут автоматически применяться на каждой странице, который будет отображать или изменения этих полей позволяет при условии, что вы используете `DynamicControl` или `DynamicField` элементов управления. Это уменьшает объем избыточный код, для записи, благодаря чему программирования и тестирования, и гарантирует, что данные форматирования и проверки согласованы во всем приложении.

## <a name="more-information"></a>Дополнительные сведения

Это заключительный шаг ряд учебники по началу работы с платформой Entity Framework. Дополнительные ресурсы помогут вам освоить использование Entity Framework продолжить работу с [в первом учебнике следующей последовательности учебника Entity Framework](../continuing-with-ef/using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started.md) или посетите следующие сайты:

- [Entity Framework вопросы и ответы](http://www.ef-faq.org/introduction.html)
- [Блог команды Entity Framework](https://blogs.msdn.com/b/adonet/)
- [Entity Framework в библиотеке MSDN](https://msdn.microsoft.com/en-us/library/bb399572.aspx)
- [Entity Framework в центре разработчиков MSDN данных](https://msdn.microsoft.com/en-us/data/ef.aspx)
- [Обзор элемента управления EntityDataSource веб-сервера в библиотеке MSDN](https://msdn.microsoft.com/en-us/library/cc488502.aspx)
- [Элемент управления EntityDataSource Справочник по API в библиотеке MSDN](https://msdn.microsoft.com/en-us/library/system.web.ui.webcontrols.entitydatasource.aspx)
- [Форумы Entity Framework в MSDN](https://social.msdn.microsoft.com/forums/en-US/adodotnetentityframework/)
- [Блог Джули Лерман](http://thedatafarm.com/blog/)

>[!div class="step-by-step"]
[Назад](the-entity-framework-and-aspnet-getting-started-part-7.md)
