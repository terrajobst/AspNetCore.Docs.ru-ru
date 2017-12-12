---
uid: web-forms/overview/older-versions-getting-started/deployment-to-a-hosting-provider/deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12
title: "Развертывание веб-приложения ASP.NET с SQL Server Compact с помощью Visual Studio или Visual Web Developer: развертывание в рабочей среде - 7 12 | Документы Microsoft"
author: tdykstra
description: "Ряд учебниках показано развертывание ASP.NET (публикации) проекта веб-приложения, который содержит базу данных SQL Server Compact с помощью Visual Stu..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 11/17/2011
ms.topic: article
ms.assetid: b83ab819-2b05-4776-b7b4-79ef78d457a5
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/older-versions-getting-started/deployment-to-a-hosting-provider/deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12
msc.type: authoredcontent
ms.openlocfilehash: ad44968975b7929f5b0f70334deabc7238797402
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 11/10/2017
---
<a name="deploying-an-aspnet-web-application-with-sql-server-compact-using-visual-studio-or-visual-web-developer-deploying-to-the-production-environment---7-of-12"></a>Развертывание веб-приложения ASP.NET с SQL Server Compact с помощью Visual Studio или Visual Web Developer: развертывание в рабочей среде - 7 12
====================
По [Tom Dykstra](https://github.com/tdykstra)

[Загрузите начальный проект](http://code.msdn.microsoft.com/Deploying-an-ASPNET-Web-4e31366b)

> Ряд учебниках показано развертывание ASP.NET (публикации) проекта веб-приложения, который содержит базу данных SQL Server Compact с помощью Visual Studio 2012 RC или Visual Studio Express 2012 RC для Web. Также можно использовать Visual Studio 2010, при установке обновления публикации Web. Введение ряда см. в разделе [в первом учебнике ряда](deployment-to-a-hosting-provider-introduction-1-of-12.md).
> 
> Учебник, в котором показаны возможности развертывания, появившиеся после выпуска версии-КАНДИДАТА Visual Studio 2012, показано, как развернуть выпусках SQL Server, отличных от SQL Server Compact и показано, как для развертывания на веб-приложениях службы приложений Azure, в разделе [развертывания веб-приложения ASP.NET с помощью Visual Studio](../../deployment/visual-studio-web-deployment/introduction.md).


## <a name="overview"></a>Обзор

В этом учебнике, настройте учетную запись с поставщиком услуг размещения и развертывание ASP.NET компонентов публикации веб-приложения в рабочей среде с помощью Visual Studio одним щелчком.

Напоминание: Если вы получаете сообщение об ошибке или что-то не работает, как пройти учебник, не забудьте проверить [страницу устранения неполадок](deployment-to-a-hosting-provider-creating-and-installing-deployment-packages-12-of-12.md).

## <a name="selecting-a-hosting-provider"></a>При выборе поставщика услуг размещения

Для этого учебника ряда и приложения Contoso университета необходимо поставщика, который поддерживает ASP.NET 4 и веб-развертывания. Был выбран определенных услуг размещения, таким образом, чтобы учебники удалось иллюстрируют процесс завершения развертывания на работающий веб-сайт. Каждого поставщика услуг размещения предоставляет различные функции и возможности развертывания на их серверах меняется немного. Тем не менее процесс, описанный в этом учебнике типично для всего процесса. Поставщик услуг размещения, используемый в этом учебнике Cytanium.com, является одним из нескольких доступных и использовать его в этом учебнике не является одобрением или рекомендацией.

Когда вы готовы к выберите поставщик услуг размещения, вы можете сравнить возможности и цен в [коллекции поставщиков](https://www.microsoft.com/web/hosting) на веб-сайте Microsoft.com.

## <a name="creating-an-account"></a>Создание учетной записи

Создайте учетную запись на сайте выбранного поставщика. Если поддержка для всей базы данных SQL Server добавляется дополнительный, необходимо выбрать его в этом учебнике, но они потребуются для [миграция на SQL Server](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12.md) учебника далее в этой серии.

Эти руководства не нужно зарегистрировать новое имя домена. Можно проверить для проверки успешного развертывания с помощью временного URL-адрес, назначенные сайту поставщиком.

После создания учетной записи появляется обычно приветственное сообщение электронной почты, который содержит все сведения, необходимые для развертывания и управления веб-узла. Сведения, ваш поставщик услуг размещения отправит вам будет аналогичен показанной здесь. Cytanium приветственное сообщение электронной почты, отправляемое нового владельца учетной записи содержит следующие сведения:

- URL-адрес сайта панели управления поставщика, где можно управлять параметрами для веб-узла. Идентификатор и пароль, которые указывались включаются в этой части приветственное сообщение электронной почты для справки. (Оба были изменены значение Демонстрация данной иллюстрации.)

    [![Welcome_Email_Control_Panel_URL](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image2.png)](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image1.png)
- Версии платформы .NET Framework по умолчанию и сведения о том, как изменить его. Многие размещения сайтов по умолчанию 2.0, который работает с приложениями ASP.NET, предназначенные для .NET Framework 2.0, 3.0 или 3.5. Однако университета Contoso является приложении .NET Framework 4, поэтому необходимо изменить этот параметр. (Для приложения ASP.NET 4.5 будет использоваться параметр .NET 4.0).

    [![Welcome_Email_Framework_Version](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image4.png)](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image3.png)
- Временный URL-адрес, можно использовать для доступа к веб-сайте. При создании этой учетной записи «contosouniversity.com» введено как имя существующего домена. Таким образом является временным URL-адресом `http://contosouniversity.com.vserver01.cytanium.com`.

    [![Welcome_Email_Temporary_URL](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image6.png)](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image5.png)
- Сведения о настройке базы данных и строки подключения, которые необходимы для доступа к ним.

    [![Welcome_Email_Database_Info](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image8.png)](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image7.png)
- Сведения о средствах и параметров для развертывания веб-узла. (Адреса электронной почты от Cytanium также говорится о WebMatrix, который здесь опущен.)

    [![Welcome_Email_Deploy_info](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image10.png)](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image9.png)

## <a name="setting-the-net-framework-version"></a>Задание версии платформы .NET Framework

Приветственное сообщение электронной почты Cytanium ссылка на инструкции о том, как изменить требуемую версию платформы .NET Framework. Эти инструкции объяснить, что это можно сделать с помощью панели управления Cytanium. Другие поставщики у сайтов панели управления, которые выглядят по-разному, или они могут дать вам сделать это по-разному.

Перейдите на URL-адрес панели управления. После входа с использованием имени пользователя и пароля, появится на панели управления.

[![Cytanium_Control_Panel](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image12.png)](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image11.png)

В **размещение пробелы** , наведите указатель мыши на значок Web и выберите **веб-сайтов** в меню.

[![Cytanium_Control_Panel_selecting_Web_Sites](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image14.png)](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image13.png)

В **веб-сайтов** щелкните **contosouniversity.com** (имя узла, который использовался при создании учетной записи).

[![Cytanium_Control_Panel_selecting_contosouniversity](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image16.png)](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image15.png)

В **свойства веб-сайта** выберите **расширения** вкладки.

[![Cytanium_Control_Panel_Extensions_tab](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image18.png)](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image17.png)

Изменить ASP.NET из **2.0 интегрированного конвейера** для **4.0 (интегрированного конвейера)**, а затем нажмите кнопку **обновление**.

## <a name="publishing-to-the-hosting-provider"></a>Публикация с поставщиком услуг размещения

Приветственное сообщение электронной почты от поставщика услуг размещения включает в себя все параметры, необходимые для публикации проекта и ввести их вручную в профиль публикации. Однако более простого и меньше вероятность ошибок метод будет использовать для настройки развертывания поставщику: загрузится *.publishsettings* файл и импортировать их в профиль публикации.

В браузере, откройте панель управления Cytanium и выберите **Web** , а затем выберите **веб-сайтов.**

![Панель управления, выбрав веб-сайтов](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image19.png)

Выберите **contosouniversity.com** веб-сайта.

![При выборе contosouniversity.com панели управления](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image20.png)

Выберите **веб-публикаций** вкладки.

![Вкладка панели веб-публикации для элемента управления](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image21.png)

Создайте учетные данные, используемые для веб-публикации, указав имя пользователя и пароль. Можно ввести те же учетные данные, используемые для входа на панели управления. Нажмите кнопку **включить**.

![Панель управления создать учетные данные для публикации](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image22.png)

Нажмите кнопку **загрузить профиль публикации веб-узла**.

![Элемент управления панели загрузить профиль публикации](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image23.png)

Когда вам будет предложено открыть или сохранить файл, сохраните его.

![Сохраните файл профиля публикации](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image24.png)

В **обозревателе решений** в Visual Studio, щелкните правой кнопкой мыши проект ContosoUniversity и выберите **публикации**. **Веб-публикация** откроется диалоговое окно на **предварительного просмотра** вкладку с **теста** выбрать, так как это последний профиль, вы использовали профиля.

Выберите **профиль** и нажмите кнопку **импорта**.

![Web мастер импорта кнопка публикации](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image25.png)

В **Импорт параметров публикации** выберите *.publishsettings* файл, который был загружен и нажмите кнопку **откройте**. В мастере открывается вкладка «соединение» с все поля заполнены.

![Публикация веб-мастер подключения tab](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image26.png)

Файл PUBLISHSETTINGS помещение плановой постоянных URL-адрес для сайта в поле URL-адрес назначения, но если этот домен еще не приобрела еще, замените значение временного URL-адрес. Например, URL-адрес —  *[http://contosouniversity.com.vserver01.cytanium.com](http://contosouniversity.com.vserver01.cytanium.com).* Единственной целью этого поля является укажите точный URL-адрес, браузер откроет автоматически после успешного после развертывания. Если оставить его пустым, единственным последствием будет, что браузер не будет запускаться автоматически после развертывания.

Нажмите кнопку **проверить подключение** Чтобы проверить правильность параметров и могут подключиться к серверу. Как показано выше, зеленая галочка проверяет, что соединение установлено успешно.

При нажатии кнопки "Проверить подключение", может появиться **ошибка сертификата** диалоговое окно. В противном случае убедитесь, что имя сервера указано как ожидалось. Если это так, выберите **сохранить сертификат для будущих сеансов Visual Studio** и нажмите кнопку **Accept**. (Эта ошибка означает, что избежать затрат на приобретение SSL-сертификат для URL-адрес, который выполняется развертывание выбрала поставщика услуг размещения. При желании установить безопасное подключение с использованием действительного сертификата, обратитесь к поставщику услуг размещения.)

![Ошибка сертификата](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image27.png)

Нажмите кнопку **Далее**.

В **баз данных** раздел **параметры** вкладке, ввести тот же профиль публикации значения, введенные для теста. Строки подключения, необходимо будет находиться в раскрывающихся списках.

- В поле Строка соединения для **SchoolContext,** выберите`Data Source=|DataDirectory|School-Prod.sdf`
- В разделе **SchoolContext**выберите **применить Code First Migrations**.
- В поле Строка соединения для **DefaultConnection**выберите`Data Source=|DataDirectory|aspnet-Prod.sdf`
- В разделе **DefaultConnection**, оставьте **обновление базы данных** очищен.

![Вкладка параметров мастера Web публикация](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image28.png)

Нажмите кнопку **Далее**.

В **предварительного просмотра** щелкните **начать просмотр** для просмотра списка файлов, которые будут скопированы. Появиться тот же список, который ранее был виден при развертывании в IIS на локальном компьютере.

Прежде чем публиковать, измените имя профиля, чтобы файл преобразования Web.Production.config будут применены. Выберите **профиль** и нажмите кнопку **Управление профилями**.

![Управление профилями веб мастера публикации](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image29.png)

В **редактировать профили публикации Web** диалогового окна выберите профиль производства, нажмите кнопку **Переименовать**и измените имя профиля в рабочей среде. Нажмите кнопку **закрыть**.

![Изменение профилей публикации веб-диалоговое окно](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image30.png)

Нажмите кнопку **Опубликовать**.

При публикации приложения с поставщиком услуг размещения. Результат показывает в **вывода** окна.

![Окно вывода после развертывания](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image31.png)

Автоматически откроется браузер на URL-адрес, введенный в **URL-адрес назначения** поле на **подключения** вкладке **веб-публикация** мастера. Вы увидите домашнюю страницу же, как при запуске сайта в Visual Studio, за исключением того, теперь отсутствует «(тест)» или индикатор среды «(Dev)» в строке заголовка. Это означает, что индикатор среды *Web.config* преобразования работал правильно.

> [!NOTE]
> Если «(Test)» по-прежнему отображается в заголовке, удалить *obj* папку из проекта ContosoUniversity и повторного развертывания. В предварительных версиях программного обеспечения ранее примененные преобразования файла (Web.Test.config) могут применяться снова несмотря на то, что вы используете профиль рабочей.


[![Home_page_Prod](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image33.png)](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image32.png)

Перед запуском страницу, которая вызывает доступ к базе данных, убедитесь, что Elmah будут входить все ошибки, возникающие в.

## <a name="setting-folder-permissions-for-elmah"></a>Установка разрешений для папки для Elmah

Как вы помните из этой серии работу с предыдущим учебником, убедитесь, что приложение имеет разрешения на запись для папки в приложении, где Elmah хранит файлы журнала ошибок. При развертывании в IIS локально на компьютере, следует установить эти разрешения вручную. В этом разделе вы увидите, как задать разрешения на Cytanium. (Некоторые поставщики услуг размещения могут не позволяют сделать это, предлагают одну или несколько предопределенных папок с разрешениями на запись. В этом случае будет необходимо изменить приложение для использования указанных папок.)

На панели управления Cytanium можно задать разрешения для папки. Перейдите к элементу управления панели URL-адрес и выберите **диспетчера файлов**.

[![Cytanium_Control_Panel_with_File_Manager_selected](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image35.png)](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image34.png)

В **диспетчера файлов** выберите **contosouniversity.com** и затем **wwwrooot** получить корневую папку приложения. Щелкните значок с изображением замка **Elmah**.

[![Cytanium_Control_Panel_File_Manager_at_root_folder](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image37.png)](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image36.png)

В **файл**/**разрешения для папок** выберите **чтения** и **записи** флажки  **contosouniversity.com** и нажмите кнопку **Настройка разрешений**.

[![Cytanium_Control_Panel_File_Folder_Permissions_Elmah](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image39.png)](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image38.png)

Убедитесь, что Elmah имеет доступ на запись к *Elmah* папку, что вызывает ошибку и отображения Elmah отчет об ошибке. Недопустимый URL-адрес, например запроса *Studentsxxx.aspx*. Как и прежде, можно увидеть *GenericErrorPage.aspx* страницы. Нажмите кнопку **Выход** связь, а затем запустите *Elmah.axd*. Вы получаете **входа в** страницы во-первых, проверяющее, что *Web.config* преобразования успешно добавлен Elmah авторизации. После входа появится отчет, отображающий только причиной ошибки.

[![ELMAH.axd_Prod](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image41.png)](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image40.png)

## <a name="testing-in-the-production-environment"></a>Тестирование в рабочей среде

Запустите **учащихся** страницы. Приложение пытается получить доступ к базе данных School в первый раз, активизирующий Code First Migrations для создания базы данных. При отображении страницы после задержки, через несколько секунд, она показывает, что нет учащихся.

[![Students_page_Prod](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image43.png)](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image42.png)

Запустите **инструкторов** страницу, чтобы убедиться, что начальное значение данные успешно вставлены инструктора данных в базе данных.

[![Instructors_page_Prod](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image45.png)](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image44.png)

Как и в тестовой среде, требуется проверить, что обновления базы данных работают в рабочей среде, но обычно не требуется вводить данные тестирования в рабочей базы данных. В этом учебнике будет использоваться тот же метод, как и в тесте. Но в реальном приложении может потребоваться найти метод, который проверяет этой базы данных обновления выполнены успешно без внесения данных теста в производственной базы данных. В некоторых приложениях может быть целесообразным добавить команду, а затем удалите его.

Добавить студент и просмотрите данные, введенные в **учащихся** страницу, чтобы убедиться, что можно обновлять данные в базе данных.

[![Add_Students_page_Prod](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image47.png)](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image46.png)

[![Students_page_with_new_student_Prod](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image49.png)](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image48.png)

Проверка правильности работы правила авторизации, выбрав **обновление кредиты** из **курсы** меню. **Входа в** -страница. Введите учетные данные учетной записи администратора, нажмите кнопку **входа в**и **обновление кредиты** -страница.

[![Log_In_page_Prod](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image51.png)](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image50.png)

Если вход выполнен успешно, **обновление кредиты** -страница. Это означает, что база данных членства ASP.NET (с учетной записью администратора единого) был успешно развернут.

[![Update_Credits_page_Prod](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image53.png)](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image52.png)

Теперь успешно развернут и тестирование веб-узла и становится доступным публично через Интернет.

## <a name="creating-a-more-reliable-test-environment"></a>Создание более надежного тестовой среды

Как описано в статье [развертывания в среде тестирования](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12.md) учебник, наиболее надежный тестовой среде будет вторую учетную запись у поставщика услуг размещения, как рабочей учетной записи. Это было бы дороже, чем при использовании локальный сервер IIS в тестовой среде, так как имеется второй размещения учетную запись. Но если он не позволяет производства узла ошибок или сбоев, можно решить, стоит затраты.

Основная часть процесса создания и развертывания в тестовой учетной записи похожа на то, что вы уже сделали для развертывания в рабочей среде:

- Создание *Web.config* файл преобразования.
- Создайте учетную запись у поставщика услуг размещения.
- Создать новый профиль публикации и развернуть в тестовой учетной записи.

### <a name="preventing-public-access-to-the-test-site"></a>Предотвращение общего доступа на сайте тестирования

Особенно важно для тестовой учетной записи — что оно станет доступно в Интернете, но вы не хотите пользоваться его. Для сохранения конфиденциальности сайта можно использовать один или несколько из следующих методов:

- Обратитесь к поставщику услуг размещения, чтобы задать правила брандмауэра, разрешающие доступ для тестирования сайта только с IP-адресов, используемых для тестирования.
- Маскировка URL-адрес, чтобы это не похоже на общедоступном сайте URL-адрес.
- Используйте *robots.txt* файл, чтобы убедиться, что поисковые системы не будет обходить ссылок сайта и отчет теста к нему в результатах поиска.

Первый из этих методов — очевидно, что наиболее безопасный, но процедура, характерное для каждого поставщика услуг размещения и не будет рассматриваться в этом учебнике. Если упорядочить с помощью поставщика услуг размещения, чтобы разрешить только IP-адрес для перехода к URL-адрес учетной записи теста, теоретически не нужно беспокоиться об обходе его поиск в поисковых системах. Но даже в этом случае развертывание *robots.txt* файл имеет смысл в резервную копию в случае, если правила брандмауэра, когда-либо случайно отключен.

*Robots.txt* файл находится в папке проекта и должно содержать следующий текст:

[!code-console[Main](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/samples/sample1.cmd)]

`User-agent` Укажет поисковые системы, в файле правила применяются все search engine поисковые (роботов) и `Disallow` строка указывает, следует для обхода нет страниц на сайте.

Возможно, требуется поисковые системы для создания каталога на рабочем сайте, поэтому необходимо исключить этот файл из рабочего развертывания. Чтобы сделать это, в разделе **можно ли исключить определенные файлы или папки из развертывания?** в [вопросы и ответы ASP.NET Web приложения проекта развертывания](https://msdn.microsoft.com/en-us/library/ee942158.aspx#can_i_exclude_specific_files_or_folders_from_deployment). Убедитесь, что указан исключения, только для профиля публикации рабочей среды.

Создание учетной записи второго размещения — подход к работе с тестовую среду, которая не является обязательным, но может иметь смысл дополнительную нагрузку. В этих учебниках вы перейдете для использования IIS в качестве тестовой среды.

В следующем уроке будет обновить код приложения и развернуть изменения для тестовой и рабочей сред.

>[!div class="step-by-step"]
[Назад](deployment-to-a-hosting-provider-setting-folder-permissions-6-of-12.md)
[Вперед](deployment-to-a-hosting-provider-deploying-a-code-only-update-8-of-12.md)
