---
uid: web-forms/overview/older-versions-getting-started/deployment-to-a-hosting-provider/deployment-to-a-hosting-provider-deploying-a-code-only-update-8-of-12
title: 'Развертывание веб-приложения ASP.NET с SQL Server Compact с помощью Visual Studio или Visual Web Developer: развертывание обновления Code-Only - 8, 12 | Документы Microsoft'
author: tdykstra
description: Ряд учебниках показано развертывание ASP.NET (публикации) проекта веб-приложения, который содержит базу данных SQL Server Compact с помощью Visual Stu...
ms.author: aspnetcontent
manager: wpickett
ms.date: 11/17/2011
ms.topic: article
ms.assetid: ddf6252f-9413-4c0c-a360-2cef8d231717
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/older-versions-getting-started/deployment-to-a-hosting-provider/deployment-to-a-hosting-provider-deploying-a-code-only-update-8-of-12
msc.type: authoredcontent
ms.openlocfilehash: 6305a8cb87d9b00b6bb4c4f8fa6114bddc4eb89f
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/06/2018
ms.locfileid: "30886918"
---
<a name="deploying-an-aspnet-web-application-with-sql-server-compact-using-visual-studio-or-visual-web-developer-deploying-a-code-only-update---8-of-12"></a>Развертывание веб-приложения ASP.NET с SQL Server Compact с помощью Visual Studio или Visual Web Developer: развертывание обновления Code-Only - 8, 12
====================
по [Tom Dykstra](https://github.com/tdykstra)

[Загрузите начальный проект](http://code.msdn.microsoft.com/Deploying-an-ASPNET-Web-4e31366b)

> Ряд учебниках показано развертывание ASP.NET (публикации) проекта веб-приложения, который содержит базу данных SQL Server Compact с помощью Visual Studio 2012 RC или Visual Studio Express 2012 RC для Web. Также можно использовать Visual Studio 2010, при установке обновления публикации Web. Введение ряда см. в разделе [в первом учебнике ряда](deployment-to-a-hosting-provider-introduction-1-of-12.md).
> 
> Учебник, в котором показаны возможности развертывания, появившиеся после выпуска версии-КАНДИДАТА Visual Studio 2012, показано, как развернуть выпусках SQL Server, отличных от SQL Server Compact и показано, как для развертывания на веб-приложениях службы приложений Azure, в разделе [развертывания веб-приложения ASP.NET с помощью Visual Studio](../../deployment/visual-studio-web-deployment/introduction.md).


## <a name="overview"></a>Обзор

После первоначального развертывания продолжает работу обслуживание и разработка веб-узла и вскоре требуется развернуть обновления. Этот учебник поможет выполнить процесс развертывания обновления в код приложения. Это обновление включает изменения базы данных; Вы увидите отличия в развертывании изменение базы данных в следующем уроке.

Напоминание: Если вы получаете сообщение об ошибке или что-то не работает, как пройти учебник, не забудьте проверить [страницу устранения неполадок](deployment-to-a-hosting-provider-creating-and-installing-deployment-packages-12-of-12.md).

## <a name="making-a-code-change"></a>Изменения кода

В качестве простого примера обновления в приложение, мы добавим **инструкторов** страница списка выбранных инструктора при изучении курсов.

При запуске **инструкторов** страницы, вы заметите, что существуют **выберите** ссылки в сетке, но они не совершают ничего, кроме убедитесь серый включить фон строки.

[![Instructors_page](deployment-to-a-hosting-provider-deploying-a-code-only-update-8-of-12/_static/image2.png)](deployment-to-a-hosting-provider-deploying-a-code-only-update-8-of-12/_static/image1.png)

Теперь добавьте код, выполняемый при **выберите** ссылка нажатии и отображает список выбранных инструктора при изучении курсов.

В *Instructors.aspx*, добавьте следующую разметку сразу после **ErrorMessageLabel** `Label` управления:

[!code-aspx[Main](deployment-to-a-hosting-provider-deploying-a-code-only-update-8-of-12/samples/sample1.aspx)]

Запустите страницу и выберите инструктор. Можно просмотреть список при изучении этого инструктора курсов.

[![Instructors_page_with_courses](deployment-to-a-hosting-provider-deploying-a-code-only-update-8-of-12/_static/image4.png)](deployment-to-a-hosting-provider-deploying-a-code-only-update-8-of-12/_static/image3.png)

## <a name="deploying-the-code-update-to-the-test-environment"></a>Развертывание обновления кода в тестовой среде

Развертывание в тестовую среду — это простой механизм выполнения одним щелчком повторной публикации. Чтобы сделать этот процесс быстрее, можно использовать **веб-публикация одним щелкните** инструментов.

В **представление** меню, выберите **панели инструментов** , а затем выберите **веб-публикация одним щелкните**.

![Selecting_One_Click_Publish_toolbar](deployment-to-a-hosting-provider-deploying-a-code-only-update-8-of-12/_static/image5.png)

В **обозревателе решений**, выберите проект ContosoUniversity.

**веб-публикация одним щелкните** инструментов, выберите **тест** профиль публикации, а затем нажмите кнопку **веб-публикация** (значок со стрелками влево и вправо).

![Web_One_Click_Publish_toolbar](deployment-to-a-hosting-provider-deploying-a-code-only-update-8-of-12/_static/image6.png)

Visual Studio развертывает обновленное приложение, и автоматически откроется браузер на домашнюю страницу. Запустите страницу инструкторов и выберите инструктор, чтобы убедиться, что обновление было успешно развернуто.

[![Instructors_page_with_courses_Test](deployment-to-a-hosting-provider-deploying-a-code-only-update-8-of-12/_static/image8.png)](deployment-to-a-hosting-provider-deploying-a-code-only-update-8-of-12/_static/image7.png)

Обычным также выполнить тест регрессии (то есть тестирование остальной части сайта, чтобы убедиться в том, что новое изменение не разбить существующие функциональные возможности). Но для этого учебника и вы перейдете пропустить этот шаг для развертывания обновления в рабочей среде.

## <a name="preventing-redeployment-of-the-initial-database-state-to-production"></a>Предотвращение повторного развертывания исходного состояния базы данных в рабочей среде

В реальном приложении пользователи взаимодействуют с производственного сайта после первоначального развертывания и баз данных заполняются на основе оперативных данных. Таким образом вы не хотите повторно развернуть базу данных членства в его начальное состояние, в которой будет очищают все динамические данные. Так как базы данных SQL Server Compact — это файлы в *приложения\_данные* папку, необходимо предотвратить это, изменив параметры развертывания, что файлы в *приложения\_данных* папки не развернуто.

Откройте **свойства проекта** окна ContosoUniversity проекта, а затем выберите **Пакет/Публикация веб-сайта** вкладку. Убедитесь, что **конфигурации** раскрывающийся список содержит либо **Активная (Release)** или **выпуска** выбран, выберите **исключить файлы из приложения\_Папки данных**.

![Exclude_files_from_the_App_Data_folder](deployment-to-a-hosting-provider-deploying-a-code-only-update-8-of-12/_static/image9.png)

В случае, если вы решили развернуть отладочную сборку в будущем, рекомендуется внести такое же изменение для конфигурации построения отладки: изменение **конфигурации** для **отладки** , а затем выберите **исключения файлы из приложения\_папки данных**.

Сохраните и закройте **Пакет/Публикация веб-сайта** вкладки.

> [!NOTE] 
> 
> [!IMPORTANT]
> Убедитесь, что нет **удалить дополнительные файлы в месте назначения** выбранных в профилях публикации. Если этот параметр выбран, процесс развертывания приведет к удалению базы данных, которые имеют в приложении\_данных в развернутой сайта, приложение будет удалено\_саму папку данных.


## <a name="preventing-user-access-to-the-production-site-during-update"></a>Ограничивая доступ пользователей к рабочего сайта во время обновления

Изменение, которое выполняется развертывание теперь является простое изменение на одной странице. Но иногда развертывании больших изменений и в этом случае сайта могут вести себя ухудшается, если пользователь запрашивает страницу до завершения развертывания. Чтобы избежать этого, можно использовать *приложения\_offline.htm* файла. При переводе в файл с именем *приложения\_offline.htm* в корневой папке приложения, службы IIS автоматически отображает этот файл вместо запуска приложения. Поэтому для предотвращения доступа во время развертывания, поместить *приложения\_offline.htm* в корневой папке запуска процесса развертывания, а затем удалите *приложения\_offline.htm*.

В **обозревателе решений**, щелкните правой кнопкой мыши решение (не один из проектов) и выберите **новую папку решения**.

![Creating_a_solution_folder](deployment-to-a-hosting-provider-deploying-a-code-only-update-8-of-12/_static/image10.png)

Назовите папку *SolutionFiles*.

В новую папку необходимо создать HTML-страницу с именем *приложения\_offline.htm*. Замените существующее содержимое следующую разметку:

[!code-html[Main](deployment-to-a-hosting-provider-deploying-a-code-only-update-8-of-12/samples/sample2.html)]

Можно скопировать *приложения\_offline.htm* файл на сайт с помощью FTP-соединение или **диспетчера файлов** служебной программы на панели управления поставщика услуг размещения. В этом учебнике будет использоваться **диспетчера файлов**.

Откройте панель управления и выберите **диспетчера файлов** как в [развертывание в рабочей среде](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12.md) учебника. Выберите **contosouniversity.com** и затем **wwwroot** получить корневую папку приложения, а затем нажмите кнопку **отправить**.

[![Upload_button_in_File_Manager](deployment-to-a-hosting-provider-deploying-a-code-only-update-8-of-12/_static/image12.png)](deployment-to-a-hosting-provider-deploying-a-code-only-update-8-of-12/_static/image11.png)

В **передать файл** выберите *приложения\_offline.htm* файла и нажмите кнопку **отправить**.

[![Upload_dialog_box_in_File_Manager](deployment-to-a-hosting-provider-deploying-a-code-only-update-8-of-12/_static/image14.png)](deployment-to-a-hosting-provider-deploying-a-code-only-update-8-of-12/_static/image13.png)

Перейдите на URL-адрес веб-узла. Вы увидите, что *приложения\_offline.htm* страницы будет отображаться вместо домашней страницы.

[![App_offline.htm_page_in_production](deployment-to-a-hosting-provider-deploying-a-code-only-update-8-of-12/_static/image16.png)](deployment-to-a-hosting-provider-deploying-a-code-only-update-8-of-12/_static/image15.png)

Теперь вы готовы к развертыванию в рабочей среде.

## <a name="deploying-the-code-update-to-the-production-environment"></a>Развертывание обновления кода в рабочей среде

В **веб-публикация одним щелкните** инструментов, выберите **рабочей** профиль публикации, а затем нажмите кнопку **веб-публикация**.

Visual Studio развертывает обновленное приложение и открывает в браузере домашнюю страницу. *Приложения\_offline.htm* отображается файл. Перед началом тестирования для проверки успешного развертывания, необходимо удалить *приложения\_offline.htm* файла.

Вернитесь к **диспетчера файлов** приложение панели управления. Выберите **contosouniversity.com** и **wwwroot**выберите **приложения\_offline.htm**, а затем нажмите кнопку **удалить**.

[![Deleting_app_offline.htm](deployment-to-a-hosting-provider-deploying-a-code-only-update-8-of-12/_static/image18.png)](deployment-to-a-hosting-provider-deploying-a-code-only-update-8-of-12/_static/image17.png)

В браузере откройте страницу инструкторов в общедоступном сайте и выберите инструктор, чтобы убедиться, что обновление было успешно развернуто.

[![Instructors_page_with_courses_Prod](deployment-to-a-hosting-provider-deploying-a-code-only-update-8-of-12/_static/image20.png)](deployment-to-a-hosting-provider-deploying-a-code-only-update-8-of-12/_static/image19.png)

Теперь, когда развертывание обновления приложения, не включающее изменение базы данных. Далее учебнике показано, как развернуть изменение базы данных.

> [!div class="step-by-step"]
> [Назад](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12.md)
> [Вперед](deployment-to-a-hosting-provider-deploying-a-database-update-9-of-12.md)
