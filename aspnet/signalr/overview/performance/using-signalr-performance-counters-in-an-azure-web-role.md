---
uid: signalr/overview/performance/using-signalr-performance-counters-in-an-azure-web-role
title: "Использование счетчиков производительности SignalR в веб-роли Azure | Документы Microsoft"
author: guardrex
description: "Описывается установка и использование счетчиков производительности SignalR в веб-роли Azure."
keywords: "Счетчик ASP.NET,SignalR,Performance, веб-роли azure"
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/11/2017
ms.topic: article
ms.assetid: 2a127d3b-21ed-4cc9-bec0-cdab4e742a25
ms.technology: dotnet-signalr
ms.prod: .net-framework
msc.legacyurl: /signalr/overview/performance/using-signalr-performance-counters-in-an-azure-web-role
msc.type: authoredcontent
ms.openlocfilehash: 2f6c6feb030fc17f95e7862c39029569f3d8c5dc
ms.sourcegitcommit: d8aa1d314891e981460b5e5c912afb730adbb3ad
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 02/05/2018
---
# <a name="using-signalr-performance-counters-in-an-azure-web-role"></a>Использование счетчиков производительности SignalR в веб-роли Azure

Автор [Люк Латэм](https://github.com/guardrex) (Luke Latham)

SignalR счетчики производительности используются для наблюдения за производительностью приложения в веб-роли Azure. Счетчики отслеживаются с помощью системы диагностики Microsoft Azure. Установка счетчиков производительности SignalR в Azure с *signalr.exe*, то же самое средство, используемое для изолированного или локальных приложений. Поскольку роли Azure являются временными, настройке приложения для установки и регистрации счетчиков производительности SignalR при запуске.

## <a name="prerequisites"></a>Предварительные требования

* [Visual Studio 2015](https://www.visualstudio.com/vs/visual-studio-express/)
* [Microsoft Azure SDK для Visual Studio 2015 (VS2015)](https://azure.microsoft.com/downloads/) **Примечание: Перезагрузите компьютер после установки пакета SDK.**
* Подписка на Microsoft Azure: подписаться на бесплатную пробную учетную запись Azure, в разделе [бесплатная пробная версия Azure](https://azure.microsoft.com/free/).

## <a name="creating-an-azure-web-role-application-that-exposes-signalr-performance-counters"></a>Создание приложения веб-роли Azure, предоставляет счетчиков производительности SignalR

1. Откройте Visual Studio 2015.

2. В Visual Studio 2015 выберите **файл** > **New** > **проекта**.

3. В **шаблоны** области **новый проект** под **Visual C#** выберите **облака** , а затем выберите **Облачной службы azure** шаблона. Присвойте приложению имя **SignalRPerfCounters** и выберите **ОК**.

   ![Новое приложение в облаке](using-signalr-performance-counters-in-an-azure-web-role/_static/image1.png)
    
4. В **новой облачной службы Microsoft Azure** диалогового окна выберите **веб-роли ASP.NET** и выберите > кнопку, чтобы добавить роль в проект. Нажмите кнопку **ОК**.

   ![Добавить веб-роль ASP.NET](using-signalr-performance-counters-in-an-azure-web-role/_static/image2.png)
    
5. В **новый веб-приложение ASP.NET — WebRole1** диалогового окна выберите **MVC** шаблона, а затем выберите **ОК**.

   ![Добавить MVC и веб-API](using-signalr-performance-counters-in-an-azure-web-role/_static/image3.png)
    
6. В **обозревателе решений**откройте *diagnostics.wadcfgx* файл **WebRole1**.

   ![Diagnostics.wadcfgx обозревателя решений](using-signalr-performance-counters-in-an-azure-web-role/_static/image4.png)
    
7. Замените содержимое файла со следующей конфигурацией и сохраните файл:

   [!code-xml[Main](using-signalr-performance-counters-in-an-azure-web-role/samples/sample1.xml)]
    
8. Откройте **консоль диспетчера пакетов** из **средства** > **диспетчера пакетов NuGet**. Введите следующие команды, чтобы установить последнюю версию SignalR и служебные программы пакета SignalR.

   [!code-powershell[Main](using-signalr-performance-counters-in-an-azure-web-role/samples/sample2.ps1)]
    
9. Настройка приложения для установки счетчиков производительности SignalR в экземпляре роли при его запуске и перезапускается. В **обозревателе решений**, щелкните правой кнопкой мыши **WebRole1** проект и выберите **добавить** > **новую папку**. Имя новой папки *запуска*.

   ![Добавьте папку при запуске](using-signalr-performance-counters-in-an-azure-web-role/_static/image5.png)
    
10. Копировать *signalr.exe* файла (добавлены с классом **Microsoft.AspNet.SignalR.Utils** пакета) из \<папки проекта > / SignalRPerfCounters/packages/Microsoft.AspNet.SignalR.Utils.\< версия > / средств для *запуска* папку, созданную на предыдущем шаге.

11. В **обозревателе решений**, щелкните правой кнопкой мыши *запуска* папку и выберите **добавить** > **существующий элемент**. В диалоговом окне, которое отображается, выберите *signalr.exe* и выберите **добавить**.

    ![Добавление signalr.exe проект](using-signalr-performance-counters-in-an-azure-web-role/_static/image6.png)
    
12. Щелкните правой кнопкой мыши *запуска* созданную папку. Выберите **Добавить** > **Новый объект**. Выберите **Общие** выберите **текстовый файл**и назовите новый элемент *SignalRPerfCounterInstall.cmd*. Этот командный файл устанавливает счетчиков производительности SignalR в веб-роли.

    ![Создание файла пакета установки счетчиков производительности SignalR](using-signalr-performance-counters-in-an-azure-web-role/_static/image7.png)
     
13. Если Visual Studio создает *SignalRPerfCounterInstall.cmd* файл, он автоматически открывается в главном окне. Замените содержимое файла следующий сценарий, а затем сохраните и закройте файл. Этот скрипт выполняется *signalr.exe*, который добавляет счетчиков производительности SignalR экземпляра роли.

    [!code-console[Main](using-signalr-performance-counters-in-an-azure-web-role/samples/sample3.cmd)]
    
14. Выберите *signalr.exe* файла в **обозревателе решений**. В файле **свойства**, задайте **Копировать в выходной каталог** для **всегда Копировать**.

    ![Задать Копировать в выходной каталог, чтобы всегда Копировать](using-signalr-performance-counters-in-an-azure-web-role/_static/image8.png)
    
15. Повторите предыдущий шаг для *SignalRPerfCounterInstall.cmd* файла.

    
16. Щелкните правой кнопкой мыши *SignalRPerfCounterInstall.cmd* файла и выберите **открыть с помощью**. В диалоговом окне, которое отображается, выберите **двоичный редактор** и выберите **ОК**.

    ![Открыть двоичный редактор](using-signalr-performance-counters-in-an-azure-web-role/_static/image9.png)
    
17. В двоичном редакторе выберите все начальные байты в файле и удалите их. Сохраните и закройте файл.

    ![Удалить начальные байт](using-signalr-performance-counters-in-an-azure-web-role/_static/image10.png)
    
18. Откройте *ServiceDefinition.csdef* и добавить задачу запуска, которая выполняет *SignalrPerfCounterInstall.cmd* файл при запуске службы:

    [!code-xml[Main](using-signalr-performance-counters-in-an-azure-web-role/samples/sample4.xml?highlight=4-7)]
    
19. Откройте `Views/Shared/_Layout.cshtml` и удалить сценарий пакет jQuery с конца файла.

    [!code-cshtml[Main](using-signalr-performance-counters-in-an-azure-web-role/samples/sample5.cshtml)]
    
20. Добавить клиент JavaScript, который постоянно вызывает `increment` метод на сервере. Откройте `Views/Home/Index.cshtml` и замените содержимое следующим кодом:

    [!code-cshtml[Main](using-signalr-performance-counters-in-an-azure-web-role/samples/sample6.cshtml)]
    
21. Создайте новую папку в **WebRole1** проект с именем *концентраторов*. Щелкните правой кнопкой мыши *концентраторов* папки в **обозревателе решений**выберите **Web** > **SignalR**и выберите  **Класс концентратора SignalR (v2)**. Назовите новый концентратор *MyHub.cs* и выберите **добавить**.

    ![Добавление класса концентратора SignalR в папку концентраторы в диалоговом окне Добавление нового элемента](using-signalr-performance-counters-in-an-azure-web-role/_static/image13.png)

22. *MyHub.cs* автоматически открывается в главном окне. Замените содержимое следующим кодом, а затем сохраните и закройте файл:

    [!code-csharp[Main](using-signalr-performance-counters-in-an-azure-web-role/samples/sample7.cs)]
    
23. *[Crank.exe](signalr-connection-density-testing-with-crank.md)*  является его плотность подключения тестирование, предоставляемый с SignalR базу кода. Поскольку ручки требуется постоянное подключение, его добавить на сайт для использования при тестировании. Добавить новую папку для **WebRole1** проект с именем *PersistentConnections*. Щелкните правой кнопкой мыши эту папку и выберите **добавить** > **класса**. Назовите новый файл класса *MyPersistentConnections.cs* и выберите **добавить**.

24. Visual Studio откроет *MyPersistentConnections.cs* файл в главном окне. Замените содержимое следующим кодом, а затем сохраните и закройте файл:

    [!code-csharp[Main](using-signalr-performance-counters-in-an-azure-web-role/samples/sample8.cs)]
    
25. С помощью `Startup` класса, объектов SignalR запуск при запуске OWIN. Откройте или создайте *файла Startup.cs* и заменить его содержимое следующим кодом:

    [!code-csharp[Main](using-signalr-performance-counters-in-an-azure-web-role/samples/sample9.cs)]
    
    В представленном выше коде `OwinStartup` помечает этот класс для запуска OWIN. `Configuration` Метод начинает SignalR.
    
26. Протестируйте приложения в эмуляторе Microsoft Azure, нажав клавишу **F5**.

    > [!NOTE]
    > При возникновении **FileLoadException** в **MapSignalR**, измените переадресации привязок в *web.config* следующее:

    [!code-xml[Main](using-signalr-performance-counters-in-an-azure-web-role/samples/sample12.xml?highlight=3,7)]
    
27. Подождите около одной минуты. Откройте окно инструментов Cloud Explorer в Visual Studio (**представление** > **Cloud Explorer**) и разверните путь `(Local)/Storage Accounts/(Development)/Tables`. Дважды щелкните **WADPerformanceCountersTable**. Вы увидите счетчиков SignalR в таблице данных. Если таблица не отображается, может потребоваться повторно введите учетные данные хранилища Azure. Необходимо выбрать **обновление** кнопку, чтобы просмотреть таблицы в **Cloud Explorer** или выберите **обновление** кнопку в окне Открыть таблицу для просмотра данных в таблице.

    ![При выборе таблицы счетчиков производительности WAD в Visual Studio Cloud Explorer](using-signalr-performance-counters-in-an-azure-web-role/_static/image11.png)

    ![Отображение счетчиков собираться в таблице счетчиков производительности WAD](using-signalr-performance-counters-in-an-azure-web-role/_static/image12.png)
    
28. Чтобы протестировать приложение в облаке, обновите **ServiceConfiguration.Cloud.cscfg** и задайте `Microsoft.WindowsAzure.Plugins.Diagnostics.ConnectionString` на допустимую строку подключения учетной записи хранилища Azure.

    [!code-xml[Main](using-signalr-performance-counters-in-an-azure-web-role/samples/sample10.xml)]

29. Развертывание приложения в вашей подписке Azure. Дополнительные сведения о том, как развернуть приложение в Azure см. в разделе [Создание и развертывание облачной службы](https://docs.microsoft.com/azure/cloud-services/cloud-services-how-to-create-deploy).

30. Подождите несколько минут. В **Cloud Explorer**, найдите учетную запись хранения, настроенных выше и найдите `WADPerformanceCountersTable` таблицы в ней. Вы увидите счетчиков SignalR в таблице данных. Если таблица не отображается, может потребоваться повторно введите учетные данные хранилища Azure. Необходимо выбрать **обновление** кнопку, чтобы просмотреть таблицы в **Cloud Explorer** или выберите **обновление** кнопку в окне Открыть таблицу для просмотра данных в таблице.

Благодарности [Ричард Мартин](https://social.msdn.microsoft.com/profile/Martin+Richard) для исходного содержимого, используемые в этом учебнике.
