---
uid: web-forms/overview/older-versions-getting-started/deployment-to-a-hosting-provider/deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12
title: 'Развертывание веб-приложения ASP.NET с SQL Server Compact с помощью Visual Studio или Visual Web Developer: развертывание в рабочей среде — 7 из 12 | Документация Майкрософт'
author: tdykstra
description: Этой серии руководств показано, как развернуть (публикации) ASP.NET проекта веб-приложения, который содержит базу данных SQL Server Compact с помощью Visual Stu...
ms.author: aspnetcontent
manager: wpickett
ms.date: 11/17/2011
ms.topic: article
ms.assetid: b83ab819-2b05-4776-b7b4-79ef78d457a5
ms.technology: dotnet-webforms
msc.legacyurl: /web-forms/overview/older-versions-getting-started/deployment-to-a-hosting-provider/deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12
msc.type: authoredcontent
ms.openlocfilehash: 26a832fd336f886ba1ddfb1682930afa4df56c58
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 07/03/2018
ms.locfileid: "37383640"
---
<a name="deploying-an-aspnet-web-application-with-sql-server-compact-using-visual-studio-or-visual-web-developer-deploying-to-the-production-environment---7-of-12"></a>Развертывание веб-приложения ASP.NET с SQL Server Compact с помощью Visual Studio или Visual Web Developer: развертывание в рабочей среде — 7 из 12
====================
по [том Дайкстра](https://github.com/tdykstra)

[Загрузите начальный проект](http://code.msdn.microsoft.com/Deploying-an-ASPNET-Web-4e31366b)

> Этой серии руководств показано, как развернуть (публикации) ASP.NET проекта веб-приложения, который содержит базу данных SQL Server Compact с помощью Visual Studio 2012 RC или Visual Studio Express 2012 RC для Web. Также можно использовать Visual Studio 2010, если установить обновление веб-публикации. Введение в серии, см. в разделе [в первом учебнике серии](deployment-to-a-hosting-provider-introduction-1-of-12.md).
> 
> Учебник, в котором показаны компоненты развертывания, появившиеся после выпуска версии-КАНДИДАТА Visual Studio 2012, содержит сведения о развертывании выпусков SQL Server, отличных от SQL Server Compact и содержит сведения о развертывании веб-приложения службы приложений Azure, см. в разделе [веб-развертывание ASP.NET с помощью Visual Studio](../../deployment/visual-studio-web-deployment/introduction.md).


## <a name="overview"></a>Обзор

В этом руководстве вы настроить учетную запись с помощью поставщика услуг размещения и развертывания приложения ASP.NET функция публикации веб-приложения в рабочей среде с помощью Visual Studio одним щелчком.

Напоминание: Если что-то не работает, как работать с учебником, вы получаете сообщение об ошибке, не забудьте установить флажок [страница устранения неполадок](deployment-to-a-hosting-provider-creating-and-installing-deployment-packages-12-of-12.md).

## <a name="selecting-a-hosting-provider"></a>Выбор поставщика услуг размещения

Для приложения университета Contoso и этой серии руководств необходим поставщик, поддерживающий ASP.NET 4 и веб-развертывания. Определенные хостинг-компания был выбран таким образом, чтобы учебники удалось проиллюстрировать полноценной работы развертывания в веб-сайта. Каждого поставщика услуг размещения предоставляет различные функции и возможности развертывания на своих серверах меняется, немного. Тем не менее, как описано в этом учебнике является типичным для всего процесса. Поставщик услуг размещения, используемый в этом учебнике, Cytanium.com, является одним из многих доступных и является одобрением или рекомендация не является его использование в этом руководстве.

Когда будете готовы для выбора собственного поставщика услуг размещения, вы можете сравнить возможности и цены в [коллекции поставщиков](https://www.microsoft.com/web/hosting) на веб-сайте Microsoft.com.

## <a name="creating-an-account"></a>Создание учетной записи

Создайте учетную запись на сайте выбранного поставщика. Если поддержка всей базы данных SQL Server является добавленную дополнительного, необходимо выбрать его в этом руководстве, но они потребуются для [переход на SQL Server](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12.md) далее в этой серии руководств.

Для этих учебников вы нет необходимости регистрировать новое доменное имя. Можно проверить Проверка успешного развертывания с помощью временный URL-адреса, назначенные сайту поставщиком.

После создания учетной записи, обычно получают приветственное сообщение электронной почты, который содержит все сведения, необходимые для развертывания и управления сайтом. Данные, ваш поставщик услуг размещения отправляет вам будет аналогичен приведенных здесь. Cytanium приветственного сообщения, отправляемые новых владельцев учетной записи содержит следующую информацию:

- URL-адрес сайта панель элемента управления поставщика, где вы можете управлять параметрами для вашего сайта. Идентификатор и пароль, указанный включаются в этой части приветственное сообщение электронной почты для справки. (Оба были изменены в значение демонстрации в этом примере.)

    [![Welcome_Email_Control_Panel_URL](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image2.png)](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image1.png)
- По умолчанию для .NET Framework и сведения о том, как изменить его. Многие размещения сайтов по умолчанию 2.0, который работает с приложениями ASP.NET, предназначенных для .NET Framework 2.0, 3.0 или 3.5. Тем не менее университета Contoso является приложением .NET Framework 4, поэтому вам нужно изменить этот параметр. (Для приложения ASP.NET 4.5 можно использовать параметр .NET 4.0.)

    [![Welcome_Email_Framework_Version](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image4.png)](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image3.png)
- Временный URL-адрес, можно использовать для доступа к веб-сайт. При создании этой учетной записи, как его имя было введено «contosouniversity.com». Таким образом временный URL-адрес является `http://contosouniversity.com.vserver01.cytanium.com`.

    [![Welcome_Email_Temporary_URL](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image6.png)](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image5.png)
- Сведения о настройке баз данных и строки подключения, которые необходимы для доступа к ним:

    [![Welcome_Email_Database_Info](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image8.png)](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image7.png)
- Сведения о средствах и параметры для развертывания веб-узла. (Адрес электронной почты из Cytanium также упоминаются WebMatrix, который здесь опущен).

    [![Welcome_Email_Deploy_info](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image10.png)](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image9.png)

## <a name="setting-the-net-framework-version"></a>Установка версии платформы .NET Framework

Cytanium приветственное сообщение электронной почты со ссылкой на инструкции о том, как изменить версию платформы .NET Framework. Эти инструкции объяснить, что это можно сделать через панель управления Cytanium. Другие поставщики имеют сайтов панели управления, которые выглядят иначе, или они могут дать вам сделать это по-разному.

Перейдите на URL-адрес панели управления. После входа в систему с помощью имени пользователя и пароля, см. на панели управления.

[![Cytanium_Control_Panel](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image12.png)](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image11.png)

В **размещение пробелы** , наведите указатель на значок «Web» и кнопку **веб-сайтов** в меню.

[![Cytanium_Control_Panel_selecting_Web_Sites](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image14.png)](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image13.png)

В **веб-сайтов** выберите **contosouniversity.com** (имя узла, который использовался при создании учетной записи).

[![Cytanium_Control_Panel_selecting_contosouniversity](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image16.png)](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image15.png)

В **свойства веб-сайта** выберите **расширения** вкладки.

[![Cytanium_Control_Panel_Extensions_tab](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image18.png)](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image17.png)

Изменить ASP.NET из **2.0 интегрированного конвейера** для **4.0 (интегрированного конвейера)**, а затем нажмите кнопку **обновления**.

## <a name="publishing-to-the-hosting-provider"></a>Публикация с поставщиком услуг размещения

Приветственное сообщение электронной почты от поставщика услуг размещения включает в себя все параметры, которые необходимы для публикации проекта, и ввести их вручную в профиль публикации. Но мы будем использовать более простой и менее подверженных ошибкам метод для настройки развертывания к поставщику: вы скачаете *.publishsettings* файл и импортировать его в профиль публикации.

В браузере перейдите к панели управления Cytanium и выберите **Web** , а затем выберите **веб-сайтов.**

![Панель управления, выбрав веб-сайтов](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image19.png)

Выберите **contosouniversity.com** веб-сайта.

![Панель управления, выбрав contosouniversity.com](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image20.png)

Выберите **веб-публикаций** вкладки.

![Вкладка панели веб-публикаций с элементами управления](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image21.png)

Создайте учетные данные для веб-публикации, указав имя пользователя и пароль. Можно ввести те же учетные данные, используемые для входа на панели управления. Нажмите кнопку **включить**.

![Панель управления создание учетных данных для публикации](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image22.png)

Нажмите кнопку **загрузить профиль публикации веб-узла**.

![Элемент управления панели загрузить профиль публикации](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image23.png)

Когда будет предложено открыть или сохранить файл, сохраните его.

![Сохраните файл профиля публикации](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image24.png)

В **обозревателе решений** в Visual Studio щелкните правой кнопкой мыши проект ContosoUniversity и выберите **публикации**. **Веб-публикации** откроется диалоговое окно на **предварительной версии** вкладку с **теста** выборе, потому что это последний профиль, вы использовали профиля.

Выберите **профиль** вкладке и нажмите кнопку **импорта**.

![Web мастер импорта кнопка "Опубликовать"](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image25.png)

В **Импорт параметров публикации** выберите *.publishsettings* файл, который был загружен и нажмите кнопку **откройте**. В мастере открывается на вкладке «подключение» с все поля заполнены.

![Публикация веб-tab, мастер подключения](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image26.png)

Файл PUBLISHSETTINGS помещает плановой постоянный URL-адрес для сайта в поле URL-адрес назначения, но если вы еще не еще приобрели этот домен, замените значение временный URL-адрес. В этом примере является URL-адрес  *[ http://contosouniversity.com.vserver01.cytanium.com ](http://contosouniversity.com.vserver01.cytanium.com).* Это окно предназначена исключительно для указания какой URL-адрес, браузер откроет автоматически после успешного после развертывания. Если оставить его пустым, единственным последствием будет, что браузер не будет запускаться автоматически после развертывания.

Нажмите кнопку **проверить подключение** для убедитесь, что параметры указаны правильно, и вы можете подключиться к серверу. Как вы уже видели, зеленая галочка проверяет, что подключение выполнено успешно.

Если щелкнуть, чтобы проверить подключение, может появиться **сообщение об ошибке сертификата** диалоговое окно. В противном случае убедитесь, что имя сервера как ожидалось. Если это так, выберите **сохранить этот сертификат для будущих сеансов Visual Studio** и нажмите кнопку **Accept**. (Эта ошибка означает, что избежать затрат на приобретение SSL-сертификат для URL-адрес, который вы развертываете выбрал поставщика услуг размещения. Если вы предпочитаете установить безопасное подключение с использованием действительного сертификата, обратитесь к поставщику услуг размещения.)

![Ошибка сертификата](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image27.png)

Нажмите кнопку **Далее**.

В **баз данных** раздел **параметры** вкладке, ввести тот же профиль публикации значения, введенные для теста. Вы найдете строки подключения, необходимые в раскрывающихся списках.

- В поле Строка подключения для **SchoolContext,** выберите `Data Source=|DataDirectory|School-Prod.sdf`
- В разделе **SchoolContext**выберите **применить Code First Migrations**.
- В поле Строка подключения для **DefaultConnection**выберите `Data Source=|DataDirectory|aspnet-Prod.sdf`
- В разделе **DefaultConnection**, оставьте **обновления базы данных** очищен.

![Публикация веб-tab параметры мастера](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image28.png)

Нажмите кнопку **Далее**.

В **предварительной версии** щелкните **начать просмотр** для просмотра списка файлов, которые будут скопированы. Появится тот же список, который вы уже видели при развертывании в IIS на локальном компьютере.

Перед публикацией, измените имя профиля, таким образом, чтобы файл преобразования Web.Production.config будут применены. Выберите **профиль** вкладку и нажмите кнопку **Управление профилями**.

![Профили управления Web мастера публикации](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image29.png)

В **изменить профили публикации Web** диалогового окна поле, выберите профиль для производства, щелкните **Переименовать**и измените имя профиля в рабочей среде. Нажмите кнопку **закрыть**.

![Изменение профилей публикации веб-диалоговое окно](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image30.png)

Нажмите кнопку **Опубликовать**.

При публикации приложения с поставщиком услуг размещения. Результат показывает, в **вывода** окна.

![Окно вывода после развертывания](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image31.png)

В браузере автоматически откроется URL-адрес, введенный в **URL-адрес назначения** поле **подключения** вкладке **веб-публикации** мастера. Вы увидите ту же страницу домашней как при запуске узла в Visual Studio, за исключением того, теперь отсутствует «(тест)» или индикатор среды «(Dev)» в строке заголовка. Это означает, что индикатор среды *Web.config* преобразования прошла без ошибок.

> [!NOTE]
> Если вы по-прежнему см. в разделе «(Test)» в заголовке, удалите *obj* папку из проекта ContosoUniversity и повторное развертывание. В предварительных версий программного обеспечения ранее примененных преобразования файла (Web.Test.config) могут применяться еще раз несмотря на то, что вы используете профиль производства.


[![Home_page_Prod](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image33.png)](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image32.png)

Перед запуском страницу, которая вызывает доступа к базе данных, убедитесь, что Elmah могли выполнять вход обо всех возникающих ошибках.

## <a name="setting-folder-permissions-for-elmah"></a>Установка разрешений для папок для Elmah

Как вы помните из предыдущего руководства в этой серии, необходимо убедиться в том, что у приложения есть разрешения на запись для папки в приложении, где Elmah хранит файлы журнала ошибок. При развертывании в IIS локально на компьютере, можно задать эти разрешения вручную. В этом разделе вы узнаете, как задать разрешения на Cytanium. (Некоторые поставщики услуг размещения не могут предоставлять вам возможность сделать это, они могут предлагать одну или несколько предопределенных папок с разрешениями на запись. В этом случае вам пришлось бы изменить приложение, чтобы использовать указанные папки.)

На панели управления Cytanium можно задать разрешения для папки. Перейдите к элементу управления панели URL-адрес и выберите **диспетчер файлов**.

[![Cytanium_Control_Panel_with_File_Manager_selected](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image35.png)](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image34.png)

В **диспетчер файлов** выберите **contosouniversity.com** и затем **wwwrooot** в корневой папке приложения. Щелкните значок с изображением замка рядом **Elmah**.

[![Cytanium_Control_Panel_File_Manager_at_root_folder](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image37.png)](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image36.png)

В **файл**/**разрешения для папки** выберите **чтения** и **записи** флажки для  **contosouniversity.com** и нажмите кнопку **Set Permissions**.

[![Cytanium_Control_Panel_File_Folder_Permissions_Elmah](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image39.png)](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image38.png)

Убедитесь, что Elmah имеет доступ на запись к *Elmah* папку, что вызывает ошибку и затем отображение отчета об ошибках Elmah. Недопустимый URL-адрес, например запроса *Studentsxxx.aspx*. Как и раньше, вы увидите *GenericErrorPage.aspx* страницы. Нажмите кнопку **Выход** ссылку, а затем запустите *Elmah.axd*. Вы получаете **вход** странице во-первых, проверяющее, что *Web.config* преобразование успешно добавлен Elmah авторизации. После входа вы увидите отчет, отображающий ошибку, просто связано.

[![ELMAH.axd_Prod](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image41.png)](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image40.png)

## <a name="testing-in-the-production-environment"></a>Тестирование в рабочей среде

Запустите **учащихся** страницы. Приложение пытается получить доступ к базе данных School в первый раз, которое инициирует Code First Migrations для создания базы данных. Когда отобразится страница после задержки в любой момент, он показывает, что учащиеся не.

[![Students_page_Prod](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image43.png)](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image42.png)

Запустите **преподавателей** страницу, чтобы убедиться, что начальные данные успешно вставлены преподавателя данных в базе данных.

[![Instructors_page_Prod](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image45.png)](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image44.png)

Как это делалось в тестовой среде, вы хотите проверить, что обновления базы данных, работать в рабочей среде, но обычно не требуется вводить тестовые данные в рабочую базу данных. В этом руководстве вы используете тот же метод, вы уже делали в тесте. Но в реальном приложении может потребоваться найти метод, который проверяет эту базу данных обновления выполнены успешно без нарушения тестовых данных в рабочей базе данных. В некоторых приложениях может быть удобно добавить команду, а затем удалить его.

Добавление учащегося, а затем просмотрите данные, введенные в **учащихся** страницу, чтобы убедиться, что можно обновлять данные в базе данных.

[![Add_Students_page_Prod](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image47.png)](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image46.png)

[![Students_page_with_new_student_Prod](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image49.png)](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image48.png)

Проверить, правила авторизации работает правильно, выбрав **обновления кредиты** из **курсы** меню. **Вход** откроется страница. Введите учетные данные учетной записи администратора, нажмите кнопку **вход**и **обновления кредиты** откроется страница.

[![Log_In_page_Prod](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image51.png)](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image50.png)

Если вход выполнен успешно, **обновления кредиты** откроется страница. Это означает, что базе данных членства ASP.NET (с учетной записью администратора единого) был успешно развернут.

[![Update_Credits_page_Prod](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image53.png)](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image52.png)

Вы успешно развернут и тестирование веб-узла, и он доступен публично через Интернет.

## <a name="creating-a-more-reliable-test-environment"></a>Создание более надежного тестовой среды

Как описано в [развертывание в среде тестирования](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12.md) учебник, самый надежный тестовая среда будет вторую учетную запись у поставщика услуг размещения, который имеет так же, как рабочую учетную запись. Это может оказаться дороже, чем использование локального IIS в качестве тестовой среды, так как вам пришлось бы зарегистрироваться в второй учетной записи размещения. Но если он не позволяет удалить сайт производственных ошибок или сбоев, может решить, что имеет смысл стоимость.

Большую часть процесса создания и развертывания для тестовой учетной записи похож на что вы уже выполнили развертывание в рабочей среде:

- Создание *Web.config* файл преобразования.
- Создайте учетную запись у поставщика услуг размещения.
- Создать новый профиль публикации и развернуть в тестовой учетной записью.

### <a name="preventing-public-access-to-the-test-site"></a>Блокирует общий доступ к сайту теста

Особенно важно для тестовой учетной записи — что оно станет доступно в Интернете, но вы не хотите пользоваться его. Для обеспечения конфиденциальности на сайте можно использовать один или несколько из следующих методов:

- Обратитесь к поставщику услуг размещения задать правила брандмауэра, разрешающие доступ к сайту тестирования только с IP-адресов, используемых для тестирования.
- Маскировка URL-адрес, чтобы это не похоже на URL-адрес общедоступного сайта.
- Используйте *robots.txt* убедитесь, что поисковые системы не будет обходить ссылок сайта и отчет теста к нему в результатах поиска.

Первый из этих методов — очевидно, что наиболее безопасным, но к процедуре, характерное для каждого поставщика услуг размещения, а не рассматриваются в этом руководстве. Если упорядочить с помощью вашего поставщика услуг размещения, чтобы разрешить только IP-адрес для перехода к URL-адрес учетной записи теста, теоретически не нужно беспокоиться об обходе его поисковые системы. Но даже в этом случае развертывание *robots.txt* файл имеет смысл как резервную копию в случае, если это правило брандмауэра случайно находится в отключенном состоянии.

*Robots.txt* файл находится в папке проекта и должен содержать следующий текст:

[!code-console[Main](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/samples/sample1.cmd)]

`User-agent` Укажет поисковых систем, в файле правила применяются ко всем web обходчикам (роботов), и `Disallow` строка указывает, что следует выполнить обход нет страниц на сайте.

Вероятно, вы хотите поисковыми каталога на рабочем сайте, поэтому необходимо исключить этот файл из развертывания в рабочей среде. Чтобы сделать это, см. в разделе **можно ли исключить определенные файлы или папки из развертывания?** в [вопросы и ответы ASP.NET Web Application проекта развертывания](https://msdn.microsoft.com/library/ee942158.aspx#can_i_exclude_specific_files_or_folders_from_deployment). Убедитесь, что указать исключения только в том случае, для производства профиль публикации.

Создание второй учетной записи размещения является подходом к работе с тестовую среду, которая не является обязательным, но может иметь смысл дополнительную нагрузку. В следующих учебниках вы перейдете для использования IIS в качестве тестовой среды.

В следующем учебном курсе вы обновите код приложения и развертывание изменения в тестовой и рабочей средах.

> [!div class="step-by-step"]
> [Назад](deployment-to-a-hosting-provider-setting-folder-permissions-6-of-12.md)
> [Вперед](deployment-to-a-hosting-provider-deploying-a-code-only-update-8-of-12.md)
