---
uid: web-forms/overview/older-versions-getting-started/deployment-to-a-hosting-provider/deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12
title: 'Развертывание веб-приложения ASP.NET с SQL Server Compact с помощью Visual Studio или Visual Web Developer: развертывание в IIS, в тестовой среде - 5, 12 | Документы Microsoft'
author: tdykstra
description: Ряд учебниках показано развертывание ASP.NET (публикации) проекта веб-приложения, который содержит базу данных SQL Server Compact с помощью Visual Stu...
ms.author: aspnetcontent
manager: wpickett
ms.date: 11/17/2011
ms.topic: article
ms.assetid: 493b2a66-816c-485c-8315-952ed1085ccc
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/older-versions-getting-started/deployment-to-a-hosting-provider/deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12
msc.type: authoredcontent
ms.openlocfilehash: 16050455c161c8ced1f954bfce9c2d9a44c522b4
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/06/2018
---
<a name="deploying-an-aspnet-web-application-with-sql-server-compact-using-visual-studio-or-visual-web-developer-deploying-to-iis-as-a-test-environment---5-of-12"></a>Развертывание веб-приложения ASP.NET с SQL Server Compact с помощью Visual Studio или Visual Web Developer: развертывание в IIS, в тестовой среде - 5, 12
====================
по [Tom Dykstra](https://github.com/tdykstra)

[Загрузите начальный проект](http://code.msdn.microsoft.com/Deploying-an-ASPNET-Web-4e31366b)

> Ряд учебниках показано развертывание ASP.NET (публикации) проекта веб-приложения, который содержит базу данных SQL Server Compact с помощью Visual Studio 2012 RC или Visual Studio Express 2012 RC для Web. Также можно использовать Visual Studio 2010, при установке обновления публикации Web. Введение ряда см. в разделе [в первом учебнике ряда](deployment-to-a-hosting-provider-introduction-1-of-12.md).
> 
> Учебник, в котором показаны возможности развертывания, появившиеся после выпуска версии-КАНДИДАТА Visual Studio 2012, показано, как развернуть выпусках SQL Server, отличных от SQL Server Compact и показано, как для развертывания на веб-приложениях службы приложений Azure, в разделе [развертывания веб-приложения ASP.NET с помощью Visual Studio](../../deployment/visual-studio-web-deployment/introduction.md).


## <a name="overview"></a>Обзор

Этот учебник содержит описание развертывания веб-приложение ASP.NET в IIS на локальном компьютере.

При разработке приложения обычно протестировать, запустив его в Visual Studio. По умолчанию это означает, что вы используете Visual Studio Development Server (также известном как Cassini). На сервере разработки Visual Studio позволяет легко проверить во время разработки в Visual Studio, но он не работает так же, как службы IIS. В результате возможно, что приложение будет работать при тестировании в Visual Studio, но вызывает сбой при развертывании служб IIS в среде размещения.

Приложение можно протестировать надежнее следующими способами:

1. Используйте IIS Express и полноценного сервера IIS вместо Visual Studio Development Server, при тестировании в Visual Studio во время разработки. Этот метод обычно эмулирует более точно способ запуска веб-узла в службах IIS. Тем не менее этот метод не нужно проверить процесс развертывания и проверки правильной работы в результате процесса развертывания.
2. Развертывание приложения в IIS на компьютере разработчика с помощью того же процесса, будет использоваться позже развернуть его в рабочей среде. Этот метод проверяет процесс развертывания в дополнение к проверке, приложение будет работать в службах IIS.
3. Развертывание приложения в тестовой среде, как можно в рабочей среде. Поскольку в рабочую среду для этих учебников стороннего поставщика услуг размещения, идеальным тестовой среде будет вторую учетную запись с помощью поставщика услуг размещения. Эта вторая учетная запись будет использовать только для тестирования, но будет настроить так же, как учетная запись рабочей среде.

Этот учебник показаны шаги вариант 2. Для параметра 3 указаний в конце [развертывание в рабочей среде](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12.md) учебника, и в конце этого учебника, ссылки на ресурсы для вариант 1.

Напоминание: Если вы получаете сообщение об ошибке или что-то не работает, как пройти учебник, не забудьте проверить [страницу устранения неполадок](deployment-to-a-hosting-provider-creating-and-installing-deployment-packages-12-of-12.md).

## <a name="configuring-the-application-to-run-in-medium-trust"></a>Настройка приложения для выполнения на среднем уровне доверия

Перед установкой служб IIS и развертывание на него, вы измените файл Web.config параметр для запуска сайта более как в обычной общей среде размещения.

Поставщики услуг размещения обычно выполняются веб-сайте *среднего уровня доверия*, что означает, что нельзя сделать следующее. Например код приложения не может получить доступ к реестру Windows и не удается прочесть или записывать файлы, которые находятся вне иерархии папок приложения. По умолчанию приложение запускается *высокого уровня доверия* на локальном компьютере, что означает, что приложение можно попытаться выполнить действия, которые будут выполнены при развертывании в рабочей среде. Таким образом чтобы сделать тестовую среду, которая более точного отражения в рабочей среде, следует настроить приложение для запуска на среднем уровне доверия.

В файле Web.config приложения добавьте **доверия** элемент в **system.web** элемента, как показано в следующем примере.

[!code-xml[Main](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12/samples/sample1.xml?highlight=4)]

Приложение будет работать со средним уровнем доверия в службах IIS даже на локальном компьютере. Этот параметр позволяет перехватывать попыток, как можно раньше в коде приложения, к каким-либо, произойдет сбой в рабочей среде.

> [!NOTE]
> При использовании Entity Framework Code First Migrations, убедитесь в том, что версии 5.0 или более поздней версии. В Entity Framework версии 4.3 миграции требует полного доверия, чтобы обновить схему базы данных.


## <a name="installing-iis-and-web-deploy"></a>Установка служб IIS и веб-развертывания

Чтобы развернуть службы IIS на компьютере разработчика, необходимо иметь IIS и веб-развертывания установлен. Они не включаются в конфигурации по умолчанию Windows 7. Если вы уже установили службы IIS и веб-развертывания, перейдите к следующему разделу.

С помощью [Web Platform Installer](https://www.microsoft.com/web/downloads/platform.aspx) является предпочтительным способом для установки служб IIS и веб-развертывания, поскольку установщик веб-платформы устанавливает рекомендуемые конфигурации для служб IIS и автоматически устанавливает необходимые компоненты для IIS и веб- Развертывание, при необходимости.

Чтобы запустить установщик веб-платформы для установки IIS и веб-развертывания, используйте следующую ссылку. Если вы уже установили IIS, веб-развертывания или любые их необходимые компоненты, установщик веб-платформы устанавливает только то, что отсутствует.

- [Установка служб IIS и веб-развертывания с помощью установщика веб-платформы](https://www.microsoft.com/web/gallery/install.aspx?appsxml=&amp;appid=IIS7;ASPNET;NETFramework4;WDeploy)

## <a name="setting-the-default-application-pool-to-net-4"></a>Параметр по умолчанию пула приложений для .NET Framework 4

После установки служб IIS, запустите **диспетчера служб IIS** чтобы убедиться в том, что .NET Framework версии 4 назначен пул приложений по умолчанию.

С Windows **запустить** последовательно выберите пункты **запуска**, введите «inetmgr» и нажмите кнопку **ОК**. (Если **запуска** команда не находится в вашей **запустить** меню, можно нажать клавишу Windows и R, чтобы открыть его. Или щелкните правой кнопкой мыши на панели задач, нажмите кнопку **свойства**выберите **меню "Пуск"** щелкните **Настройка**и выберите **выполните команду**.)

В **подключений** разверните узел сервера и выберите **пулы приложений**. В **пулы приложений** области, если **DefaultAppPool** — назначено платформа .NET framework версии 4, как показано на следующем рисунке, перейдите к следующему разделу.

[![Inetmgr_showing_4.0_app_pools](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12/_static/image2.png)](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12/_static/image1.png)

Если вы видите только два пула приложений, а они устанавливаются в .NET Framework 2.0, необходимо установить ASP.NET 4 в службах IIS:

- Откройте окно командной строки, щелкнув правой кнопкой мыши **командной строки** в Windows **запустить** , выберите в меню **Запуск от имени администратора**. Затем запустите [aspnet\_regiis.exe](https://msdn.microsoft.com/library/k6h9cz8h.aspx) для установки ASP.NET 4 в IIS с помощью следующих команд. (В 64-разрядных системах замените «Framework» с «Framework64»).

    [!code-console[Main](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12/samples/sample2.cmd)]

    [![aspnet_regiis_installing_ASP.NET_4](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12/_static/image4.png)](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12/_static/image3.png)

    Эта команда создает новые пулы приложений для .NET Framework 4, но пул приложений будет по-прежнему присвоено 2.0. Вы будете развертывать приложения, ориентированном на .NET 4 с этим пулом приложений, необходимо изменить пул приложений .NET 4.

Если вы закрыли **диспетчера служб IIS**, запустите его еще раз, разверните узел сервера и нажмите кнопку **пулы приложений** для отображения **пулы приложений** области еще раз.

В **пулы приложений** области, нажмите кнопку **DefaultAppPool**, а затем в **действия** щелкните **основные параметры**.

[![Inetmgr_selecting_Basic_Settings_for_app_pool](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12/_static/image6.png)](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12/_static/image5.png)

В **изменение пула приложений** диалоговом изменение **версии платформы .NET Framework** для **v4.0.30319 .NET Framework** и нажмите кнопку **ОК**.

[![Selecting_.NET_4_for_DefaultAppPool](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12/_static/image8.png)](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12/_static/image7.png)

Теперь все готово для публикации в IIS.

## <a name="publishing-to-iis"></a>Публикация в IIS

С помощью Visual Studio 2010 и веб-развертывания можно развернуть несколькими способами:

- Используйте Visual Studio, публикация одним щелчком.
- Создание *пакета развертывания* и установите его с помощью пользовательского интерфейса диспетчера IIS. Пакет развертывания состоит из *.zip* файл, содержащий файлы и метаданные, необходимые для установки сайта в IIS.
- Создание пакета развертывания и установите его с помощью командной строки.

Процесс, который вы выполнили на предыдущих уроках, чтобы настроить Visual Studio для автоматизации задач развертывания применяется ко всем из трех способов. В этих учебниках используется первый из этих методов. Сведения об использовании пакетов развертывания см. в разделе [Карта содержимого развертывания ASP.NET](https://msdn.microsoft.com/library/bb386521.aspx).

Перед публикацией убедитесь, что Visual Studio выполняется с правами администратора. (В Windows 7 **запустить** меню, щелкните правой кнопкой мыши значок для используемой версии Visual Studio, вы используете и выберите **Запуск от имени администратора**.) Режим администратора является обязательным для публикации только при публикации в IIS на локальном компьютере.

В **обозревателе решений**, щелкните правой кнопкой мыши проект ContosoUniversity (не проект ContosoUniversity.DAL) и выберите **публикации**.

**Веб-публикация** откроется окно мастера.

![Publish_Web_wizard_Profile_tab](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12/_static/image9.png)

В раскрывающемся списке выберите  **&lt;создать... &gt;**.

В **новый профиль** диалоговое окно, введите «Test» и нажмите кнопку **ОК**.

![New_Profile_dialog_box](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12/_static/image10.png)

Это имя является таким же, как в среднем узел Web.Test.config преобразования файла, которое было создано ранее. Это соответствие является в каком случае преобразования Web.Test.config, применяемый при публикации с помощью этого профиля.

Мастер автоматически перейдет к **подключения** вкладки.

В **URL-адрес службы** введите *localhost*.

В **сайт и приложение** введите *веб-сайта по умолчанию или ContosoUniversity*.

В **URL-адрес назначения** введите `http://localhost/ContosoUniversity`.

**URL-адрес назначения** параметр не является обязательным. По завершении развертывание приложения Visual Studio автоматически откроется браузер по умолчанию для этого URL-адреса. Если вы не хотите браузер, чтобы автоматически открыть после развертывания, оставьте это поле пустым.

![Publish_Web_wizard_Connection_tab_Test](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12/_static/image11.png)

Нажмите кнопку **проверить подключение** Чтобы проверить правильность параметров и можно подключиться к службам IIS на локальном компьютере.

Зеленая галочка проверяет, что соединение установлено успешно.

![Publish_Web_wizard_Connection_tab_validated](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12/_static/image12.png)

Нажмите кнопку **Далее** для продвижения **параметры** вкладки.

**Конфигурации** раскрывающийся список указывает конфигурацию сборки для развертывания. Значение по умолчанию — выпуска, которые нужны.

Оставить **удалить дополнительные файлы в месте назначения** флажок снят. Поскольку это первый развертывания, не будет существовать все файлы в папке назначения еще.

В **баз данных** введите следующее значение в поле Строка соединения для **SchoolContext**:

[!code-console[Main](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12/samples/sample3.cmd)]

Процесс развертывания будет поместить эту строку подключения в развернутом файле Web.config, так как **использовать эту строку подключения во время выполнения** выбран.

Также в разделе **SchoolContext**выберите **применить Code First Migrations**. Этот параметр вызывает процесс развертывания для настройки в развернутый файл Web.config для указания `MigrateDatabaseToLatestVersion` инициализатора. Это инициализатора автоматически обновляет базу данных до последней версии, когда приложение обращается к базе данных в первый раз после развертывания.

В поле Строка соединения для **DefaultConnection**, введите следующее значение:

[!code-console[Main](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12/samples/sample4.cmd)]

Оставить **обновление базы данных** очищен. База данных членства будет развертываться путем копирования SDF-файл в приложении\_данных и вы не хотите процесс развертывания необходимости выполнять дополнительные действия с этой базой данных.

![Publish_Web_wizard_Settings_tab_Test](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12/_static/image13.png)

Нажмите кнопку **Далее** для продвижения **предварительного просмотра** вкладки.

В **предварительного просмотра** щелкните **начать просмотр** для просмотра списка файлов, которые будут скопированы.

![Publish_Web_wizard_Preview_tab_Test](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12/_static/image14.png)

![Publish_Web_wizard_Preview_tab_Test_with_file_list](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12/_static/image15.png)

Нажмите кнопку **Опубликовать**.

Если Visual Studio не находится в режиме администратора, может появиться сообщение об ошибке, указывающее, ошибки разрешений. В этом случае закройте Visual Studio, откройте его в режиме администратора и повторите попытку публикации.

Если Visual Studio с правами администратора **вывода** отчеты окно успешного построения и публикации.

![Output_window_publish_Test](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12/_static/image16.png)

Автоматически откроется в браузере на Contoso университета домашнюю страницу, работающего в IIS на локальном компьютере.

[![Home_page_Test](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12/_static/image18.png)](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12/_static/image17.png)

## <a name="testing-in-the-test-environment"></a>Тестирование в тестовой среде

Обратите внимание, что индикатор среды показывает «(Test)» вместо «(Dev)», который показывает, что *Web.config* успешно преобразование для индикатора среды.

[![Home_page_Test](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12/_static/image20.png)](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12/_static/image19.png)

Запустите **учащихся** страницу, чтобы убедиться, что развернутая база данных имеет не учащихся. При выборе этой страницы она может занять несколько минут, чтобы загрузить, так как Code First создает базу данных и затем запускает `Seed` метод. (Он не сделать, если были на домашней странице, так как приложение не пытается получить доступ к базе данных еще).

[![Students_page_Test](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12/_static/image22.png)](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12/_static/image21.png)

Запустите **инструкторов** страницу, чтобы убедиться, что Code First заполнена данными инструктора базы данных:

[![Instructors_page_Test](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12/_static/image24.png)](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12/_static/image23.png)

Выберите **добавьте учащихся** из **учащихся** меню, добавить студент, а затем просмотрите нового студента в **учащихся** страницу, чтобы убедиться, что вы успешно может записывать в базу данных :

[![Add_Students_page_Test](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12/_static/image26.png)](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12/_static/image25.png)

[![Students_page_with_new_student_Test](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12/_static/image28.png)](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12/_static/image27.png)

Из **курсы** последовательно выберите пункты **обновление кредиты**. **Обновления кредиты** страницы требуются разрешения администратора, поэтому **входа в** отображается страница. Введите учетные данные учетной записи администратора, созданную ранее («admin» и «АП $w0rd»). **Обновление кредиты** отображается страница, проверяет, что учетная запись администратора, созданную в предыдущем учебнике правильно была развернута в тестовую среду.

[![Log_In_page_Test](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12/_static/image30.png)](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12/_static/image29.png)

[![Update_Credits_page_Test](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12/_static/image32.png)](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12/_static/image31.png)

Убедитесь, что *Elmah* существует папка с файлом заполнитель только в ней.

[![Elmah_folder_Test](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12/_static/image34.png)](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12/_static/image33.png)

<a id="efcfmigrations"></a>

## <a name="reviewing-the-automatic-webconfig-changes-for-code-first-migrations"></a>Просмотр изменений автоматического Web.config для миграции Code First

Откройте *Web.config* файла в развернутом приложении по *C:\inetpub\wwwroot\ContosoUniversity* , чтобы увидеть, где процесс развертывания автоматически настроить Code First Migrations в Обновите базу данных до последней версии.

![](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12/_static/image35.png)

Процесс развертывания также создается строка подключения для миграции Code First, предназначенного исключительно для обновления схемы базы данных:

![DatabasePublish_connection_string](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12/_static/image36.png)

Эта строка дополнительные подключения можно указать одну учетную запись пользователя для обновления схемы базы данных, а другой учетной записью для доступа к данным приложения. Например, можно назначить db\_роль владельца для миграции Code First и db\_datareader и db\_datawriter ролей для приложения. Это распространенный подход глубокой обороны, предотвращающее потенциально вредоносный код приложения, изменения схемы базы данных. (Например, это может произойти в успешных атак путем внедрения кода SQL.) Этот шаблон не используется в этих учебниках. Он не применяется к SQL Server Compact, а не применяется при миграции в SQL Server позднее в этом учебнике этой серии. Сайт Cytanium обеспечивает лишь одну учетную запись для доступа к базе данных SQL Server, созданную на сайте Cytanium. Если удается реализовать этот шаблон в сценарий, его можно сделать, выполнив следующие действия:

1. В **параметры** вкладке **веб-публикация** мастера введите строку подключения, который указывает пользователя с полной базы данных разрешения на обновление схемы и снимите **использовать эту строку подключения во время выполнения** флажок. В развернутом файле Web.config, это свойство становится `DatabasePublish` строку подключения.
2. Создайте преобразование файла Web.config для строки подключения, требуется приложение для использования во время выполнения.

Теперь развертывания приложения в IIS на компьютере разработчика и она протестирована. Проверяет, что процесс развертывания копируются содержимое приложения правильное местоположение (за исключением файлов, которые вы не хотите развернуть), а также что веб-развертывания IIS правильность настройки во время развертывания. В следующем уроке вы будете запускать один дополнительные тест, который находит задачу развертывания, которая еще не было сделано: Установка разрешений для папки на *Elmah* папки.

## <a name="more-information"></a>Дополнительные сведения

Сведения о запуске IIS или IIS Express в Visual Studio см. следующие ресурсы:

- [IIS Express Обзор](https://www.iis.net/learn/extensions/introduction-to-iis-express/iis-express-overview) на сайте IIS.net.
- [Знакомство с IIS Express](https://weblogs.asp.net/scottgu/archive/2010/06/28/introducing-iis-express.aspx) в блоге Скотта Гатри.
- [Как: укажите веб-сервера для веб-проектов в Visual Studio](https://msdn.microsoft.com/library/ms178108.aspx).
- [Основные различия между IIS и ASP.NET Development Server](../deploying-web-site-projects/core-differences-between-iis-and-the-asp-net-development-server-cs.md) на веб-сайте ASP.NET.
- [Тестирование приложения ASP.NET MVC или веб-приложение форм в службах IIS 7 за 30 секунд](https://blogs.msdn.com/b/rickandy/archive/2011/04/22/test-you-asp-net-mvc-or-webforms-application-on-iis-7-in-30-seconds.aspx) в блоге Рика Андерсона. Эта запись содержит примеры почему тестирование с помощью Visual Studio Development Server (Cassini) не является надежной, как и тестирование в IIS Express, и почему тестирование в IIS Express не является надежной, как и тестирование в службах IIS.

Сведения о какие проблемы могут возникнуть при запуске приложения на среднем уровне доверия, см. в разделе [размещение приложений ASP.NET в средний уровень доверия](http://www.4guysfromrolla.com/articles/100307-1.aspx) на 4 Guys Rolla сайта.

> [!div class="step-by-step"]
> [Назад](deployment-to-a-hosting-provider-configuring-project-properties-4-of-12.md)
> [Вперед](deployment-to-a-hosting-provider-setting-folder-permissions-6-of-12.md)
