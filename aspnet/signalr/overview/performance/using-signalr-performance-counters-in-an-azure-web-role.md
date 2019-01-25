---
uid: signalr/overview/performance/using-signalr-performance-counters-in-an-azure-web-role
title: Использование счетчиков производительности SignalR в веб-роли Azure | Документация Майкрософт
author: guardrex
description: Описывается установка и использование счетчиков производительности SignalR в веб-роли Azure.
keywords: Счетчик ASP.NET,SignalR,Performance, веб-роли azure
ms.author: bradyg
ms.date: 10/03/2018
ms.assetid: 2a127d3b-21ed-4cc9-bec0-cdab4e742a25
msc.legacyurl: /signalr/overview/performance/using-signalr-performance-counters-in-an-azure-web-role
msc.type: authoredcontent
ms.openlocfilehash: 8e17e945bc144731dd149bd7ddfc9e29160eaf0b
ms.sourcegitcommit: ebf4e5a7ca301af8494edf64f85d4a8deb61d641
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/24/2019
ms.locfileid: "54836493"
---
# <a name="using-signalr-performance-counters-in-an-azure-web-role"></a>Использование счетчиков производительности SignalR в веб-роли Azure

Автор [Люк Латэм](https://github.com/guardrex) (Luke Latham)

[!INCLUDE [Consider ASP.NET Core SignalR](~/includes/signalr/signalr-version-disambiguation.md)]

Счетчики производительности SignalR используются для отслеживания производительности приложения в веб-роли Azure. Счетчики регистрируются с помощью системы диагностики Microsoft Azure. Установка счетчиков производительности SignalR в Azure с помощью *signalr.exe*, тот же инструмент, используемое для изолированными, так и локальных приложений. Так как роли Azure являются временными, Настройка приложения для установки и регистрации счетчиков производительности SignalR при запуске.

## <a name="prerequisites"></a>Предварительные требования

* Visual Studio 2015 или [2017 г.](https://visualstudio.microsoft.com/downloads/?utm_medium=microsoft&utm_source=docs.microsoft.com&utm_campaign=button+cta&utm_content=download+vs2017)
* [Microsoft Azure SDK для Visual Studio](https://azure.microsoft.com/downloads/) **Примечание: Перезагрузите компьютер после установки пакета SDK.**
* Подписка Microsoft Azure: Чтобы зарегистрироваться для получения бесплатной пробной учетной записи Azure, см. в разделе [бесплатной пробной версии Azure](https://azure.microsoft.com/free/).

## <a name="creating-an-azure-web-role-application-that-exposes-signalr-performance-counters"></a>Создание приложения веб-роли Azure, предоставляет счетчики производительности SignalR

1. Запустите Visual Studio.

2. В Visual Studio последовательно выберите **Файл** > **Создать** > **Проект**.

3. В **новый проект** выберите **Visual C#** > **Cloud** категории с левой стороны экрана, а затем выберите **облачной службы Azure** шаблона. Присвойте приложению имя **SignalRPerfCounters** и выберите **ОК**.

   ![Новое Облачное приложение](using-signalr-performance-counters-in-an-azure-web-role/_static/image1.png)

   > [!NOTE]
   > Если вы не видите **Cloud** Категория шаблона или **облачной службы Azure** шаблона, необходимо установить **разработки Azure** рабочей нагрузки для Visual Studio 2017. Выберите **откройте установщик Visual Studio** ссылку в левой нижней части **новый проект** диалоговое окно, чтобы открыть установщик Visual Studio. Выберите **разработки Azure** рабочей нагрузки, а затем выберите **изменить** чтобы начать установку рабочей нагрузки.
   >
   > ![Рабочую нагрузку разработки Azure в Visual Studio Installer](using-signalr-performance-counters-in-an-azure-web-role/_static/azure-development-workload.png)

4. В **новая облачная служба Microsoft Azure** диалоговом окне выберите **веб-роль ASP.NET** и выберите >, чтобы добавить роль к проекту. Нажмите кнопку **ОК**.

   ![Добавление веб-роль ASP.NET](using-signalr-performance-counters-in-an-azure-web-role/_static/image2.png)

5. В **новый веб-приложение ASP.NET — WebRole1** диалоговом окне выберите **MVC** шаблона, а затем выберите **ОК**.

   ![Добавление MVC и веб-API](using-signalr-performance-counters-in-an-azure-web-role/_static/image3.png)

6. В **обозревателе решений**откройте *diagnostics.wadcfgx* файл **WebRole1**.

   ![Diagnostics.wadcfgx обозревателя решений](using-signalr-performance-counters-in-an-azure-web-role/_static/image4.png)

7. Замените содержимое файла со следующей конфигурацией и сохраните файл:

   [!code-xml[Main](using-signalr-performance-counters-in-an-azure-web-role/samples/sample1.xml)]

8. Откройте **консоль диспетчера пакетов** из **средства** > **диспетчер пакетов NuGet**. Введите следующие команды, чтобы установить последнюю версию SignalR и служебные программы пакета SignalR.

   [!code-powershell[Main](using-signalr-performance-counters-in-an-azure-web-role/samples/sample2.ps1)]

9. Настройте приложение для установки счетчиков производительности SignalR в экземпляр роли при его запуске и перезапускается. В **обозревателе решений**, щелкните правой кнопкой мыши **WebRole1** проекта и выберите **добавить** > **новую папку**. Назовите новую папку *запуска*.

   ![Добавление папка "Автозагрузка"](using-signalr-performance-counters-in-an-azure-web-role/_static/image5.png)

10. Копировать *signalr.exe* файл (добавлены с классом **Microsoft.AspNet.SignalR.Utils** пакета) из \<папки проекта > / SignalRPerfCounters/packages/Microsoft.AspNet.SignalR.Utils.\< версии > / средств *запуска* папку, созданную на предыдущем шаге.

11. В **обозревателе решений**, щелкните правой кнопкой мыши *запуска* папку и выберите **добавить** > **существующий элемент**. В появившемся диалоговом окне выберите *signalr.exe* и выберите **добавить**.

    ![Добавление signalr.exe проект](using-signalr-performance-counters-in-an-azure-web-role/_static/image6.png)

12. Щелкните правой кнопкой мыши *запуска* созданной папке. Выберите **Добавить** > **Новый объект**. Выберите **Общие** выберите **текстовый файл**и назовите новый элемент *SignalRPerfCounterInstall.cmd*. Этот командный файл устанавливает счетчики производительности SignalR в веб-роли.

    ![Создание файла пакета установки счетчиков производительности SignalR](using-signalr-performance-counters-in-an-azure-web-role/_static/image7.png)

13. Когда Visual Studio создает *SignalRPerfCounterInstall.cmd* файл, он автоматически открывается в главном окне. Замените содержимое файла следующий сценарий, а затем сохраните и закройте файл. Этот сценарий выполняет *signalr.exe*, который добавляет счетчики производительности SignalR в экземпляр роли.

    [!code-console[Main](using-signalr-performance-counters-in-an-azure-web-role/samples/sample3.cmd)]

14. Выберите *signalr.exe* файл **обозревателе решений**. В файле **свойства**, задайте **Копировать в выходной каталог** для **всегда Копировать**.

    ![Переключите копию в выходной каталог, чтобы всегда Копировать](using-signalr-performance-counters-in-an-azure-web-role/_static/image8.png)

15. Повторите предыдущий шаг для *SignalRPerfCounterInstall.cmd* файла.


16. Щелкните правой кнопкой мыши *SignalRPerfCounterInstall.cmd* файл и выберите **открыть с помощью**. В появившемся диалоговом окне выберите **двоичный редактор** и выберите **ОК**.

    ![Открыть двоичный редактор](using-signalr-performance-counters-in-an-azure-web-role/_static/image9.png)

17. В двоичном редакторе выберите все начальные байты в файле и удалите их. Сохраните и закройте файл.

    ![Удаление начальных байтов](using-signalr-performance-counters-in-an-azure-web-role/_static/image10.png)

18. Откройте *ServiceDefinition.csdef* и добавить задачу запуска, который выполняет *SignalrPerfCounterInstall.cmd* файл при запуске службы:

    [!code-xml[Main](using-signalr-performance-counters-in-an-azure-web-role/samples/sample4.xml?highlight=4-7)]

19. Откройте `Views/Shared/_Layout.cshtml` и удалить сценарий jQuery пакета из конца файла.

    [!code-cshtml[Main](using-signalr-performance-counters-in-an-azure-web-role/samples/sample5.cshtml)]

20. Добавьте клиент JavaScript, который постоянно вызывает `increment` метод на сервере. Откройте `Views/Home/Index.cshtml` и замените его содержимое следующим кодом:

    [!code-cshtml[Main](using-signalr-performance-counters-in-an-azure-web-role/samples/sample6.cshtml)]

21. Создайте новую папку в **WebRole1** проект с именем *концентраторов*. Щелкните правой кнопкой мыши *концентраторов* папку в **обозревателе решений** и выберите **добавить** > **новый элемент**. В **Добавление нового элемента** выберите **Web** > **SignalR** категории, а затем выберите **класс концентратора SignalR (v2)** шаблона элемента. Назовите новый концентратор *MyHub.cs* и выберите **добавить**.

    ![Добавляемый класс концентратора SignalR в папку концентраторов в диалоговом окне Добавление нового элемента](using-signalr-performance-counters-in-an-azure-web-role/_static/image13.png)

22. *MyHub.cs* автоматически открывается в главном окне. Замените содержимое следующим кодом, а затем сохраните и закройте файл:

    [!code-csharp[Main](using-signalr-performance-counters-in-an-azure-web-role/samples/sample7.cs)]

23. *[Crank.exe](signalr-connection-density-testing-with-crank.md)*  является тестирование, предоставляемый с SignalR codebase плотности подключений. Так как Crank требуется постоянное подключение, его добавить на сайт для использования при тестировании. Добавьте новую папку для **WebRole1** проект с именем *PersistentConnections*. Щелкните правой кнопкой мыши эту папку и выберите **добавить** > **класс**. Назовите новый файл класса *MyPersistentConnections.cs* и выберите **добавить**.

24. Visual Studio открывает *MyPersistentConnections.cs* файл в главном окне. Замените содержимое следующим кодом, а затем сохраните и закройте файл:

    [!code-csharp[Main](using-signalr-performance-counters-in-an-azure-web-role/samples/sample8.cs)]

25. С помощью `Startup` класс, объекты SignalR запуск при запуске OWIN. Откройте или создайте *Startup.cs* и замените его содержимое следующим кодом:

    [!code-csharp[Main](using-signalr-performance-counters-in-an-azure-web-role/samples/sample9.cs)]

    В приведенном выше коде `OwinStartup` атрибут помечает этот класс для запуска OWIN. `Configuration` Метод начинает SignalR.

26. Протестируйте приложение в эмуляторе Microsoft Azure, нажав клавишу **F5**.

    > [!NOTE]
    > При возникновении **FileLoadException** в **MapSignalR**, изменить переадресации привязок в *web.config* следующим:

    [!code-xml[Main](using-signalr-performance-counters-in-an-azure-web-role/samples/sample12.xml?highlight=3,7)]

27. Подождите около одной минуты. Откройте окно инструментов Cloud Explorer в Visual Studio (**представление** > **Cloud Explorer**) и разверните путь `(Local)/Storage Accounts/(Development)/Tables`. Дважды щелкните **WADPerformanceCountersTable**. Вы должны увидеть счетчиков SignalR в таблице данных. Если вы не видите таблицу, может потребоваться повторно ввести учетные данные хранилища Azure. Необходимо выбрать **обновить** кнопку, чтобы просмотреть таблицу в **Cloud Explorer** или выберите **обновить** кнопку в окне Открыть таблицу для просмотра данных в таблице.

    ![Выбор таблицы счетчиков производительности WAD в Visual Studio Cloud Explorer](using-signalr-performance-counters-in-an-azure-web-role/_static/image11.png)

    ![Отображение счетчиков собираются в таблице счетчиков производительности WAD](using-signalr-performance-counters-in-an-azure-web-role/_static/image12.png)

28. Чтобы протестировать приложение в облаке, обновите **ServiceConfiguration.Cloud.cscfg** файл и задайте для `Microsoft.WindowsAzure.Plugins.Diagnostics.ConnectionString` допустимую строку подключения учетной записи хранилища Azure.

    [!code-xml[Main](using-signalr-performance-counters-in-an-azure-web-role/samples/sample10.xml)]

29. Развертывание приложения в вашей подписке Azure. Дополнительные сведения о том, как развернуть приложение в Azure, см. в разделе [как создать и развернуть облачную службу](https://docs.microsoft.com/azure/cloud-services/cloud-services-how-to-create-deploy).

30. Подождите несколько минут. В **Cloud Explorer**, найдите учетную запись хранения, настроенных выше и найти `WADPerformanceCountersTable` таблицы в ней. Вы должны увидеть счетчиков SignalR в таблице данных. Если вы не видите таблицу, может потребоваться повторно ввести учетные данные хранилища Azure. Необходимо выбрать **обновить** кнопку, чтобы просмотреть таблицу в **Cloud Explorer** или выберите **обновить** кнопку в окне Открыть таблицу для просмотра данных в таблице.

Особая благодарность [Ричард Мартин](https://social.msdn.microsoft.com/profile/Martin+Richard) для исходного содержимого, используемые в этом руководстве.
