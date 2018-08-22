---
uid: web-forms/overview/older-versions-getting-started/deployment-to-a-hosting-provider/deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12
title: 'Развертывание веб-приложения ASP.NET с SQL Server Compact с помощью Visual Studio или Visual Web Developer: развертывание в IIS в качестве тестовой среды — 5, 12 | Документация Майкрософт'
author: tdykstra
description: Этой серии руководств показано, как развернуть (публикации) ASP.NET проекта веб-приложения, который содержит базу данных SQL Server Compact с помощью Visual Stu...
ms.author: riande
ms.date: 11/17/2011
ms.assetid: 493b2a66-816c-485c-8315-952ed1085ccc
msc.legacyurl: /web-forms/overview/older-versions-getting-started/deployment-to-a-hosting-provider/deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12
msc.type: authoredcontent
ms.openlocfilehash: ba939012e8fb11a50992eeaef70e8ebf61cea851
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 08/16/2018
ms.locfileid: "41839207"
---
<a name="deploying-an-aspnet-web-application-with-sql-server-compact-using-visual-studio-or-visual-web-developer-deploying-to-iis-as-a-test-environment---5-of-12"></a>Развертывание веб-приложения ASP.NET с SQL Server Compact с помощью Visual Studio или Visual Web Developer: развертывание в IIS в качестве тестовой среды — 5, 12
====================
по [том Дайкстра](https://github.com/tdykstra)

[Загрузите начальный проект](http://code.msdn.microsoft.com/Deploying-an-ASPNET-Web-4e31366b)

> Этой серии руководств показано, как развернуть (публикации) ASP.NET проекта веб-приложения, который содержит базу данных SQL Server Compact с помощью Visual Studio 2012 RC или Visual Studio Express 2012 RC для Web. Также можно использовать Visual Studio 2010, если установить обновление веб-публикации. Введение в серии, см. в разделе [в первом учебнике серии](deployment-to-a-hosting-provider-introduction-1-of-12.md).
> 
> Учебник, в котором показаны компоненты развертывания, появившиеся после выпуска версии-КАНДИДАТА Visual Studio 2012, содержит сведения о развертывании выпусков SQL Server, отличных от SQL Server Compact и содержит сведения о развертывании веб-приложения службы приложений Azure, см. в разделе [веб-развертывание ASP.NET с помощью Visual Studio](../../deployment/visual-studio-web-deployment/introduction.md).


## <a name="overview"></a>Обзор

Этом руководстве показано, как развернуть веб-приложения ASP.NET в IIS на локальном компьютере.

При разработке приложения, обычно тестируется путем запуска его в Visual Studio. По умолчанию это означает, что вы используете Visual Studio Development Server (также известном как Cassini). Visual Studio Development Server упрощает процесс тестирования во время разработки в Visual Studio, но он не работает так же, как IIS. Таким образом вполне возможно, что приложения будут выполняться правильно при тестировании в Visual Studio, но сбой при развертывании в IIS в среде размещения.

Приложение можно протестировать надежнее следующими способами:

1. Используйте IIS Express или полноценного сервера IIS вместо Visual Studio Development Server при тестировании в Visual Studio во время разработки. Этот метод обычно эмулирует более точно как сайт будет работать под управлением служб IIS. Тем не менее этот метод не нужно проверить процесс развертывания и проверить, что результат процесса развертывания будет работать неправильно.
2. Разверните приложение в службах IIS на компьютере разработчика, используя тот же процесс, который будет использоваться в дальнейшем для его развертывания в рабочей среде. Этот метод проверяет процесс развертывания в дополнение к проверке, приложение будет работать под управлением служб IIS.
3. Развертывание приложения в тестовой среде, максимально близки к рабочей среды. Поскольку в рабочую среду для этих учебников стороннего поставщика услуг размещения, идеальный тестовая среда будет вторую учетную запись с помощью поставщика услуг размещения. Эта вторая учетная запись используется только для тестирования, но он настраивается так же, как рабочую учетную запись.

Мы продемонстрируем этапы вариант 2. Руководство для варианта 3 предоставляется в конце [развертывание в рабочей среде](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12.md) руководстве и в конце этого руководства находятся ссылки на ресурсы для вариант 1.

Напоминание: Если что-то не работает, как работать с учебником, вы получаете сообщение об ошибке, не забудьте установить флажок [страница устранения неполадок](deployment-to-a-hosting-provider-creating-and-installing-deployment-packages-12-of-12.md).

## <a name="configuring-the-application-to-run-in-medium-trust"></a>Настройка приложения для выполнения в среднем уровне доверия

Перед установкой служб IIS и развертывание к нему, вам предстоит изменить параметра файла Web.config, чтобы сделать узел выполнения более как в обычной общей среде размещения.

Поставщики услуг размещения обычно выполняются веб-сайт в *со средним уровнем доверия*, что означает, что нельзя сделать следующее. Например код приложения нельзя доступа к реестру Windows и не удается прочесть или записывать файлы, которые находятся вне иерархии папок приложения. По умолчанию приложение выполняется в *высоким уровнем доверия* на локальном компьютере, что означает, что приложение может иметь возможность выполнять действия, которые будут выполнены при развертывании в рабочей среде. Таким образом чтобы сделать тестовую среду, которая более точно отражает рабочую среду, вы настроите приложение для запуска в среднем уровне доверия.

В файле Web.config приложения добавьте **доверия** элемент в **system.web** элемента, как показано в следующем примере.

[!code-xml[Main](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12/samples/sample1.xml?highlight=4)]

Приложение теперь будет выполняться в среднем уровне доверия в службах IIS даже на локальном компьютере. Этот параметр позволяет перехватывать любые попытки как можно раньше в коде приложения сделать нечто, вызывавшие сбои в рабочей среде.

> [!NOTE]
> При использовании Entity Framework Code First Migrations, убедитесь, что у вас есть версии 5.0 или более поздней версии. В Entity Framework версии 4.3 миграция требует полного доверия, чтобы обновить схему базы данных.


## <a name="installing-iis-and-web-deploy"></a>Установка IIS и веб-развертывание

Для развертывания в IIS на компьютере разработчика, требуются службы IIS и веб-развертывания установлены. Они не включаются в конфигурации по умолчанию Windows 7. Если вы уже установили службы IIS и веб-развертывания, перейдите к следующему разделу.

С помощью [установщика веб-платформы](https://www.microsoft.com/web/downloads/platform.aspx) является более предпочтительным для установки IIS и веб-развертывания, поскольку установщик веб-платформы устанавливает рекомендуемые конфигурации для служб IIS и она автоматически устанавливает необходимые компоненты для IIS и веб- Развертывание, при необходимости.

Чтобы запустить установщик веб-платформы для установки IIS и веб-развертывания, используйте следующую ссылку. Если вы уже установили IIS, веб-развертывания или любой из необходимых компонентов для них, установщик веб-платформы устанавливает только что здесь отсутствует.

- [Установка служб IIS и веб-развертывания с помощью установщика веб-платформы](https://www.microsoft.com/web/gallery/install.aspx?appsxml=&amp;appid=IIS7;ASPNET;NETFramework4;WDeploy)

## <a name="setting-the-default-application-pool-to-net-4"></a>Установка по умолчанию пула приложений в .NET 4

После установки служб IIS, запустите **диспетчера служб IIS** чтобы убедиться в том, что пул приложений по умолчанию назначается в .NET Framework версии 4.

Из Windows **запустить** меню, выберите **запуска**, введите «inetmgr» и нажмите кнопку **ОК**. (Если **запуска** команды не находится в вашей **запустить** меню, можно нажать клавишу Windows и R, чтобы открыть его. Или щелкните правой кнопкой мыши на панели задач, нажмите кнопку **свойства**выберите **меню "Пуск"** щелкните **Настройка**и выберите **выполните команду**.)

В **подключений** области, разверните узел сервера и выберите **пулы приложений**. В **пулы приложений** области, если **DefaultAppPool** будет назначено .NET framework версии 4, как показано на следующем рисунке, перейдите к следующему разделу.

[![Inetmgr_showing_4.0_app_pools](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12/_static/image2.png)](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12/_static/image1.png)

Если вы видите только два пула приложений, и они задаются в .NET Framework 2.0, необходимо установить ASP.NET 4 в IIS:

- Откройте окно командной строки, щелкнув правой кнопкой мыши **командной** в Windows **запустить** меню и выбрав **Запуск от имени администратора**. Затем запустите [aspnet\_regiis.exe](https://msdn.microsoft.com/library/k6h9cz8h.aspx) установить ASP.NET 4 в IIS, с помощью следующих команд. (В 64-разрядных системах, замените «Платформа» с «Framework64»).

    [!code-console[Main](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12/samples/sample2.cmd)]

    [![aspnet_regiis_installing_ASP.NET_4](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12/_static/image4.png)](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12/_static/image3.png)

    Эта команда создает новые пулы приложений для .NET Framework 4, но пул приложений по умолчанию по-прежнему будет указано значение 2.0. Вы будете развертывать приложения, ориентированном на .NET 4 с этим пулом приложений, поэтому вам нужно изменить пул приложений для .NET 4.

Если вы закрыли **диспетчера служб IIS**, запустите его снова, раскройте узел сервера и нажмите кнопку **пулы приложений** для отображения **пулы приложений** панель снова.

В **пулы приложений** области нажмите кнопку **DefaultAppPool**, а затем в **действия** панели щелкните **основные параметры**.

[![Inetmgr_selecting_Basic_Settings_for_app_pool](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12/_static/image6.png)](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12/_static/image5.png)

В **изменение пула приложений** » диалогового окна «Изменение **версии платформы .NET Framework** для **v4.0.30319 .NET Framework** и нажмите кнопку **ОК**.

[![Selecting_.NET_4_for_DefaultAppPool](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12/_static/image8.png)](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12/_static/image7.png)

Теперь вы готовы к публикации в службах IIS.

## <a name="publishing-to-iis"></a>Публикация в IIS

Существует несколько способов, которые можно развернуть с помощью Visual Studio 2010 и веб-развертывания:

- С помощью Visual Studio, публикация одним щелчком.
- Создание *пакет развертывания* и установить его с помощью пользовательского интерфейса диспетчера IIS. Пакет развертывания состоит из *ZIP-файл* файл, содержащий все файлы и метаданные, необходимые для установки сайта в IIS.
- Создание пакета развертывания и установить его с помощью командной строки.

Процесс, который вы выполнили в предыдущих учебных курсах, чтобы настроить Visual Studio для автоматизации задач развертывания применяется ко всем из трех способов. В этих учебниках используется первый из этих методов. Для получения сведений о пакетах развертывания, см. в разделе [Карта содержимого развертывания ASP.NET](https://msdn.microsoft.com/library/bb386521.aspx).

Перед публикацией, убедитесь, что Visual Studio выполняется в режиме администратора. (В Windows 7 **запустить** меню, щелкните правой кнопкой мыши значок для версии Visual Studio, вы используете и выберите **Запуск от имени администратора**.) Режим администратора является обязательным для публикации, только когда вы публикуете в службах IIS на локальном компьютере.

В **обозревателе решений**, щелкните правой кнопкой мыши проект ContosoUniversity (не проект ContosoUniversity.DAL) и выберите **публикации**.

**Веб-публикации** откроется мастер.

![Publish_Web_wizard_Profile_tab](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12/_static/image9.png)

В раскрывающемся списке выберите  **&lt;New... &gt;**.

В **новый профиль** диалоговое окно, введите «Test» и нажмите кнопку **ОК**.

![New_Profile_dialog_box](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12/_static/image10.png)

Это имя является таким же, как в среднем узел Web.Test.config преобразования файла, который был создан ранее. Это соответствие является, в каком случае Web.Test.config преобразования для применения при публикации с использованием этого профиля.

Мастер автоматически переходит к **подключения** вкладки.

В **URL-адрес службы** введите *localhost*.

В **сайт и приложение** введите *веб-сайт по умолчанию/ContosoUniversity*.

В **URL-адрес назначения** введите `http://localhost/ContosoUniversity`.

**URL-адрес назначения** параметр не требуется. Когда Visual Studio завершит развертывание приложения, автоматически откроется браузер по умолчанию этот URL-адрес. Если вы не хотите в браузере открывается автоматически после развертывания, оставьте это поле пустым.

![Publish_Web_wizard_Connection_tab_Test](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12/_static/image11.png)

Нажмите кнопку **проверить подключение** для убедитесь, что параметры указаны правильно, и можно подключиться к службам IIS на локальном компьютере.

Зеленая галочка проверяет, что подключение выполнено успешно.

![Publish_Web_wizard_Connection_tab_validated](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12/_static/image12.png)

Нажмите кнопку **Далее** для перехода к **параметры** вкладки.

**Конфигурации** раскрывающемся списке указывает конфигурацию сборки для развертывания. Значение по умолчанию — выпуска, которые нужны.

Оставьте **удалять дополнительные файлы в месте назначения** флажок снят. Так как это первый развертывания, не будет все файлы в папке назначения еще.

В **баз данных** введите следующее значение в поле Строка подключения для **SchoolContext**:

[!code-console[Main](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12/samples/sample3.cmd)]

Процесс развертывания будет поместить эту строку подключения в развернутый файл Web.config, поскольку **использовать эту строку подключения во время выполнения** выбран.

Также в разделе **SchoolContext**выберите **применить Code First Migrations**. Этот параметр вызывает завершение процесса развертывания для настройки развернутый файл Web.config для указания `MigrateDatabaseToLatestVersion` инициализатор. Это инициализатор автоматически обновляет базу данных до последней версии, когда приложение получает доступ к базе данных в первый раз после развертывания.

В поле Строка подключения для **DefaultConnection**, введите следующее значение:

[!code-console[Main](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12/samples/sample4.cmd)]

Оставьте **обновления базы данных** очищен. Базы данных членства будет развертываться путем копирования SDF-файл в приложении\_данных и вы не хотите, процесс развертывания, никакие дополнительные действия с этой базой данных.

![Publish_Web_wizard_Settings_tab_Test](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12/_static/image13.png)

Нажмите кнопку **Далее** для перехода к **предварительной версии** вкладки.

В **предварительной версии** щелкните **начать просмотр** для просмотра списка файлов, которые будут скопированы.

![Publish_Web_wizard_Preview_tab_Test](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12/_static/image14.png)

![Publish_Web_wizard_Preview_tab_Test_with_file_list](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12/_static/image15.png)

Нажмите кнопку **Опубликовать**.

Если Visual Studio не находится в режиме администратора, может появиться сообщение об ошибке, указывающее ошибку. В этом случае закройте Visual Studio, откройте его в режиме администратора и повторите попытку публикации.

Если Visual Studio находится в режиме администратора, **вывода** отчеты окно успешной сборки и публикации.

![Output_window_publish_Test](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12/_static/image16.png)

В браузере автоматически откроется на Contoso University домашнюю страницу, на сервере IIS на локальном компьютере.

[![Home_page_Test](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12/_static/image18.png)](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12/_static/image17.png)

## <a name="testing-in-the-test-environment"></a>Тестирование в тестовой среде

Обратите внимание на то, что среда индикатор отображает «(тест)» вместо «(Dev)», который показывает, что *Web.config* выполнено преобразование для индикатора среды.

[![Home_page_Test](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12/_static/image20.png)](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12/_static/image19.png)

Запустите **учащихся** страницу, чтобы убедиться, что развернутая база данных имеет не учащихся. При выборе этой страницы может занять несколько минут, чтобы загрузить, так как Code First создает базу данных и затем запускает `Seed` метод. (Ничего не произошло, когда вы были на домашней странице, так как приложение не пытается получить доступ к базе данных еще.)

[![Students_page_Test](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12/_static/image22.png)](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12/_static/image21.png)

Запустите **преподавателей** страницу, чтобы убедиться, что Code First заполнена начальными значениями базу данных данными преподавателя:

[![Instructors_page_Test](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12/_static/image24.png)](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12/_static/image23.png)

Выберите **добавить учащихся** из **учащихся** меню, добавить учащихся, а затем просмотрите нового учащегося в **учащихся** страницу, чтобы убедиться, что вы можете успешно записи в базу данных :

[![Add_Students_page_Test](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12/_static/image26.png)](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12/_static/image25.png)

[![Students_page_with_new_student_Test](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12/_static/image28.png)](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12/_static/image27.png)

Из **курсы** меню, выберите **обновления кредиты**. **Кредиты обновления** страницы требуются права администратора, поэтому **вход** откроется страница. Введите учетные данные учетной записи администратора, что вы создали ранее («admin» и «Pas$ w0rd»). **Обновления кредиты** отображается страница, которая проверяет, что учетную запись администратора, созданную в предыдущем учебном курсе был правильно развернут в тестовую среду.

[![Log_In_page_Test](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12/_static/image30.png)](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12/_static/image29.png)

[![Update_Credits_page_Test](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12/_static/image32.png)](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12/_static/image31.png)

Убедитесь, что *Elmah* существует папка с файлом заполнитель только в ней.

[![Elmah_folder_Test](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12/_static/image34.png)](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12/_static/image33.png)

<a id="efcfmigrations"></a>

## <a name="reviewing-the-automatic-webconfig-changes-for-code-first-migrations"></a>Просмотрите изменения в автоматической Web.config для Code First Migrations

Откройте *Web.config* файл в развернутом приложении в *C:\inetpub\wwwroot\ContosoUniversity* и вы увидите, где процесс развертывания автоматически настроить Code First Migrations для Обновите базу данных до последней версии.

![](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12/_static/image35.png)

Процесс развертывания, также создается новая строка подключения для Code First Migrations для использования исключительно для обновления схемы базы данных:

![DatabasePublish_connection_string](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12/_static/image36.png)

Эта строка дополнительные подключения можно указать одну учетную запись пользователя для обновления схемы баз данных и учетной записи другого пользователя для доступа к данным приложения. Например, можно назначить db\_роль владельца для Code First Migrations и db\_datareader и db\_datawriter роли для приложения. Это общий шаблон защиты в глубину, не позволяющая потенциально вредоносного кода в приложении изменять схему базы данных. (Например, это может произойти в успешной атаки путем внедрения кода SQL.) Этот шаблон не используется в этих учебниках. Он не применяется к SQL Server Compact, и оно не применяется при миграции в SQL Server в следующем руководстве этой серии. На сайте Cytanium предлагает только одну учетную запись пользователя для доступа к базе данных SQL Server, созданную на сайте Cytanium. Если при реализации этого шаблона в рамках сценария, его можно сделать, выполнив следующие действия:

1. В **параметры** вкладке **веб-публикации** мастер, введите строку подключения, задающий пользователя с помощью разрешений на обновление схемы базы данных полный и очистить **использовать эту строку подключения во время выполнения** "флажок". В развернутый файл Web.config, это становится `DatabasePublish` строку подключения.
2. Создайте преобразование файла Web.config для строки подключения, приложение для использования во время выполнения.

Вы развернули приложение в службах IIS на компьютере разработчика и протестировали его там. Это подтверждает, что процесс развертывания копируются содержимое приложения в нужном направлении (за исключением файлов, которые вы не хотите развернуть), а также этой веб-развертывания настроен IIS во время развертывания. В следующем руководстве вы выполните еще один тест, который находит задачу развертывания, которая еще не было сделано: Установка разрешений для папки на *Elmah* папки.

## <a name="more-information"></a>Дополнительные сведения

Сведения о запуске IIS или IIS Express в Visual Studio см. следующие ресурсы:

- [Обзор IIS Express](https://www.iis.net/learn/extensions/introduction-to-iis-express/iis-express-overview) на веб-узле IIS.net.
- [Знакомство с IIS Express](https://weblogs.asp.net/scottgu/archive/2010/06/28/introducing-iis-express.aspx) в блоге Скотта Гатри.
- [Практическое: укажите веб-сервера для веб-проектов в Visual Studio](https://msdn.microsoft.com/library/ms178108.aspx).
- [Основные различия между IIS и ASP.NET Development Server](../deploying-web-site-projects/core-differences-between-iis-and-the-asp-net-development-server-cs.md) на веб-сайте ASP.NET.
- [Тестирование приложения ASP.NET MVC или приложения веб-форм в службах IIS 7 в 30 секунд](https://blogs.msdn.com/b/rickandy/archive/2011/04/22/test-you-asp-net-mvc-or-webforms-application-on-iis-7-in-30-seconds.aspx) в блоге Рика Андерсона:. Эта запись содержит примеры почему тестирование с помощью Visual Studio Development Server (Cassini) не менее надежно, чем тестирование в IIS Express, и почему тестирование в IIS Express не менее надежно, чем тестирование в службах IIS.

Сведения о проблемах могут возникнуть при запуске приложения в среднем уровне доверия, см. в разделе [размещения приложений ASP.NET в средний уровень доверия](http://www.4guysfromrolla.com/articles/100307-1.aspx) на 4 ребята из Rolla сайта.

> [!div class="step-by-step"]
> [Назад](deployment-to-a-hosting-provider-configuring-project-properties-4-of-12.md)
> [Вперед](deployment-to-a-hosting-provider-setting-folder-permissions-6-of-12.md)
