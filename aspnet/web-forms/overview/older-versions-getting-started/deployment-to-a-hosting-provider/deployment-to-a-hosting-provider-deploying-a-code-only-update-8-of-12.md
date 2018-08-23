---
uid: web-forms/overview/older-versions-getting-started/deployment-to-a-hosting-provider/deployment-to-a-hosting-provider-deploying-a-code-only-update-8-of-12
title: 'Развертывание веб-приложения ASP.NET с SQL Server Compact с помощью Visual Studio или Visual Web Developer: развертывание обновления Code-Only - 8 из 12 | Документация Майкрософт'
author: tdykstra
description: Этой серии руководств показано, как развернуть (публикации) ASP.NET проекта веб-приложения, который содержит базу данных SQL Server Compact с помощью Visual Stu...
ms.author: riande
ms.date: 11/17/2011
ms.assetid: ddf6252f-9413-4c0c-a360-2cef8d231717
msc.legacyurl: /web-forms/overview/older-versions-getting-started/deployment-to-a-hosting-provider/deployment-to-a-hosting-provider-deploying-a-code-only-update-8-of-12
msc.type: authoredcontent
ms.openlocfilehash: cc994f989734153592ce78af5f4be57953b24458
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 08/16/2018
ms.locfileid: "41836716"
---
<a name="deploying-an-aspnet-web-application-with-sql-server-compact-using-visual-studio-or-visual-web-developer-deploying-a-code-only-update---8-of-12"></a>Развертывание веб-приложения ASP.NET с SQL Server Compact с помощью Visual Studio или Visual Web Developer: развертывание обновления Code-Only - 8 из 12
====================
по [том Дайкстра](https://github.com/tdykstra)

[Загрузите начальный проект](http://code.msdn.microsoft.com/Deploying-an-ASPNET-Web-4e31366b)

> Этой серии руководств показано, как развернуть (публикации) ASP.NET проекта веб-приложения, который содержит базу данных SQL Server Compact с помощью Visual Studio 2012 RC или Visual Studio Express 2012 RC для Web. Также можно использовать Visual Studio 2010, если установить обновление веб-публикации. Введение в серии, см. в разделе [в первом учебнике серии](deployment-to-a-hosting-provider-introduction-1-of-12.md).
> 
> Учебник, в котором показаны компоненты развертывания, появившиеся после выпуска версии-КАНДИДАТА Visual Studio 2012, содержит сведения о развертывании выпусков SQL Server, отличных от SQL Server Compact и содержит сведения о развертывании веб-приложения службы приложений Azure, см. в разделе [веб-развертывание ASP.NET с помощью Visual Studio](../../deployment/visual-studio-web-deployment/introduction.md).


## <a name="overview"></a>Обзор

После первоначального развертывания продолжает работу обслуживание и разработка веб-сайт, и вскоре требуется развернуть обновления. В этом руководстве описываются процесс развертывания обновления в код приложения. Это обновление не влечет за собой изменение базы данных; Вы увидите, чем отличается развертывание изменения базы данных в следующем учебном курсе.

Напоминание: Если что-то не работает, как работать с учебником, вы получаете сообщение об ошибке, не забудьте установить флажок [страница устранения неполадок](deployment-to-a-hosting-provider-creating-and-installing-deployment-packages-12-of-12.md).

## <a name="making-a-code-change"></a>Изменение кода

В качестве простого примера обновления для приложения, вы добавите **преподавателей** странице Список курсов, проводимых выбранным преподавателем.

При запуске **преподавателей** страницы, можно заметить, что существуют **выберите** ссылки в сетке, но они не было присвоено имя, отличное от марки серый цвет фона Включение строк.

[![Instructors_page](deployment-to-a-hosting-provider-deploying-a-code-only-update-8-of-12/_static/image2.png)](deployment-to-a-hosting-provider-deploying-a-code-only-update-8-of-12/_static/image1.png)

Теперь добавьте код, который выполняется при запуске **выберите** ссылку нажатии и отображает список курсов, проводимых выбранным преподавателем.

В *Instructors.aspx*, добавьте следующую разметку сразу после **ErrorMessageLabel** `Label` управления:

[!code-aspx[Main](deployment-to-a-hosting-provider-deploying-a-code-only-update-8-of-12/samples/sample1.aspx)]

Откройте страницу и выберите преподавателя. Появится список курсов, проводимых этого преподавателя.

[![Instructors_page_with_courses](deployment-to-a-hosting-provider-deploying-a-code-only-update-8-of-12/_static/image4.png)](deployment-to-a-hosting-provider-deploying-a-code-only-update-8-of-12/_static/image3.png)

## <a name="deploying-the-code-update-to-the-test-environment"></a>Развертывание обновления кода в тестовую среду

Развертывание в тестовую среду — просто выполняется одним щелчком повторной публикации. Чтобы сделать этот процесс быстрее, можно использовать **веб-публикация одним щелкните** панели инструментов.

В **представление** меню, выберите **панелей инструментов** , а затем выберите **веб-публикация одним щелкните**.

![Selecting_One_Click_Publish_toolbar](deployment-to-a-hosting-provider-deploying-a-code-only-update-8-of-12/_static/image5.png)

В **обозревателе решений**, выберите проект ContosoUniversity.

**веб-публикация одним щелкните** панели инструментов выберите **теста** профиль публикации, а затем нажмите кнопку **веб-публикации** (значок со стрелками влево и вправо).

![Web_One_Click_Publish_toolbar](deployment-to-a-hosting-provider-deploying-a-code-only-update-8-of-12/_static/image6.png)

Visual Studio развертывает обновленное приложение и в браузере автоматически откроется на домашнюю страницу. Откройте страницу Instructors и выберите преподавателя, чтобы проверить, что обновление успешно развернут.

[![Instructors_page_with_courses_Test](deployment-to-a-hosting-provider-deploying-a-code-only-update-8-of-12/_static/image8.png)](deployment-to-a-hosting-provider-deploying-a-code-only-update-8-of-12/_static/image7.png)

Обычным также выполнить тестирование регрессии (то есть проверка остальной части сайта, чтобы убедиться в том, что новое изменение не нарушили существующие функциональные возможности). Но в этом руководстве будет пропустить этот шаг и продолжить развертывание обновления в рабочей среде.

## <a name="preventing-redeployment-of-the-initial-database-state-to-production"></a>Предотвращение повторного развертывания исходного состояния базы данных в рабочей среде

В реальном приложении пользователи взаимодействуют с рабочий сайт после первоначального развертывания и баз данных заполняются реальные данные. Таким образом вы не хотите повторно развернуть базы данных членства в его начальное состояние, в которой будет очищать все реальные данные. Так как базы данных SQL Server Compact — это файлы в *приложения\_данных* папку, необходимо предотвратить это, изменив параметры развертывания, так что файлы в *приложения\_данных* папки не развернут.

Откройте **свойства проекта** окно для ContosoUniversity проект и выберите **пакета и публикация веб-** вкладки. Убедитесь, что **конфигурации** раскрывающийся список содержит либо **Активная (Release)** или **выпуска** , щелкните **исключить файлы из приложения\_Папка данных**.

![Exclude_files_from_the_App_Data_folder](deployment-to-a-hosting-provider-deploying-a-code-only-update-8-of-12/_static/image9.png)

В случае, если вы решили развернуть отладочную сборку в будущем, рекомендуется сделать для отладочной конфигурации сборки: изменить **конфигурации** для **Отладка** , а затем выберите **исключить файлы из приложения\_папка данных**.

Сохраните и закройте **пакета и публикация веб-** вкладки.

> [!NOTE] 
> 
> [!IMPORTANT]
> Убедитесь, что у вас нет **удалять дополнительные файлы в месте назначения** выбран в профилях публикации. Если этот флажок установлен, процесс развертывания будут удалены базы данных, у вас есть в приложении\_данных в развернутого веб-сайта и он приведет к удалению приложения\_самой папке данных.


## <a name="preventing-user-access-to-the-production-site-during-update"></a>Предотвращение доступа пользователя к рабочему сайту во время обновления

Изменение, которое вы развертываете теперь является простое изменение на одной странице. Но иногда развертывание более масштабным изменениям, и в этом случае сайт может вести себя некорректно, если пользователь запрашивает страницу до завершения развертывания. Чтобы избежать этого, можно использовать *приложения\_offline.htm* файла. Если поместить файл с именем *приложения\_offline.htm* в корневой папке приложения, службы IIS автоматически отображает этот файл, а не посредством запуска приложения. Поэтому для предотвращения доступа во время развертывания, находиться *приложения\_offline.htm* в корневой папке, запустите процесс развертывания, а затем удалите *приложения\_offline.htm*.

В **обозревателе решений**, щелкните правой кнопкой мыши решение (не проект) и выберите **новую папку решения**.

![Creating_a_solution_folder](deployment-to-a-hosting-provider-deploying-a-code-only-update-8-of-12/_static/image10.png)

Назовите папку *SolutionFiles*.

В новой папке создайте HTML-страницу с именем *приложения\_offline.htm*. Замените имеющееся содержимое следующей разметкой:

[!code-html[Main](deployment-to-a-hosting-provider-deploying-a-code-only-update-8-of-12/samples/sample2.html)]

Вы можете скопировать *приложения\_offline.htm* файл на сайт с помощью FTP-подключение или **диспетчер файлов** в панели управления поставщика услуг размещения. В этом учебнике будет использоваться **диспетчер файлов**.

Откройте панель управления и выберите **диспетчер файлов** как это делалось [развертывание в рабочей среде](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12.md) руководства. Выберите **contosouniversity.com** и затем **wwwroot** получить корневую папку приложения, а затем нажмите кнопку **отправить**.

[![Upload_button_in_File_Manager](deployment-to-a-hosting-provider-deploying-a-code-only-update-8-of-12/_static/image12.png)](deployment-to-a-hosting-provider-deploying-a-code-only-update-8-of-12/_static/image11.png)

В **передать файл** выберите *приложения\_offline.htm* файла и нажмите кнопку **отправить**.

[![Upload_dialog_box_in_File_Manager](deployment-to-a-hosting-provider-deploying-a-code-only-update-8-of-12/_static/image14.png)](deployment-to-a-hosting-provider-deploying-a-code-only-update-8-of-12/_static/image13.png)

Перейдите к URL-адрес веб сайта. Вы увидите, что *приложения\_offline.htm* страницы будет отображаться вместо домашней страницы.

[![App_offline.htm_page_in_production](deployment-to-a-hosting-provider-deploying-a-code-only-update-8-of-12/_static/image16.png)](deployment-to-a-hosting-provider-deploying-a-code-only-update-8-of-12/_static/image15.png)

Теперь вы готовы к развертыванию в производственной среде.

## <a name="deploying-the-code-update-to-the-production-environment"></a>Развертывание обновления кода в рабочую среду

В **веб-публикация одним щелкните** панели инструментов выберите **рабочей** профиль публикации, а затем нажмите кнопку **веб-публикации**.

Visual Studio развернет обновленное приложение и откроет в браузере домашнюю страницу. *Приложения\_offline.htm* отобразится файл. Перед тестированием для проверки успешного развертывания, необходимо удалить *приложения\_offline.htm* файла.

Вернитесь к **диспетчер файлов** приложения панели управления. Выберите **contosouniversity.com** и **wwwroot**выберите **приложения\_offline.htm**, а затем нажмите кнопку **удалить**.

[![Deleting_app_offline.htm](deployment-to-a-hosting-provider-deploying-a-code-only-update-8-of-12/_static/image18.png)](deployment-to-a-hosting-provider-deploying-a-code-only-update-8-of-12/_static/image17.png)

В браузере откройте страницу Instructors в общедоступный сайт и выберите преподавателя, чтобы проверить, что обновление успешно развернут.

[![Instructors_page_with_courses_Prod](deployment-to-a-hosting-provider-deploying-a-code-only-update-8-of-12/_static/image20.png)](deployment-to-a-hosting-provider-deploying-a-code-only-update-8-of-12/_static/image19.png)

Теперь вы развернули обновления приложения, не включающее изменение базы данных. Следующее руководство показано, как развернуть изменения базы данных.

> [!div class="step-by-step"]
> [Назад](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12.md)
> [Вперед](deployment-to-a-hosting-provider-deploying-a-database-update-9-of-12.md)
