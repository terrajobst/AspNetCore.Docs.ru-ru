---
uid: aspnet/overview/owin-and-katana/owin-startup-class-detection
title: "Определение класса запуска OWIN | Документы Microsoft"
author: Praburaj
description: "Этого учебника показано, как настроить класс запуска OWIN, который загружается. Дополнительные сведения о OWIN в разделе Обзор Katana проекта. Этот учебник был..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 10/17/2013
ms.topic: article
ms.assetid: 08257f55-36f4-4e39-9c88-2a5602838c79
ms.technology: 
ms.prod: .net-framework
msc.legacyurl: /aspnet/overview/owin-and-katana/owin-startup-class-detection
msc.type: authoredcontent
ms.openlocfilehash: 618f8fa23630dcf9821a54415766dc015694e535
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/24/2018
---
<a name="owin-startup-class-detection"></a>Определение класса запуска OWIN
====================
по [Praburaj Thiagarajan](https://github.com/Praburaj), [Рик Андерсон](https://github.com/Rick-Anderson)

> Этого учебника показано, как настроить класс запуска OWIN, который загружается. Дополнительные сведения о OWIN см. в разделе [Обзор проекта Katana](an-overview-of-project-katana.md). Это руководство было написано с Рик Андерсон ( [ @RickAndMSFT ](https://twitter.com/#!/RickAndMSFT) ), Praburaj Thiagarajan и Говард Дайеркинг ( [ @howard \_Дайеркинг](https://twitter.com/howard_dierking) ).
> 
> ## <a name="prerequisites"></a>Предварительные требования
> 
> [Visual Studio 2013](https://www.microsoft.com/visualstudio/eng/2013-downloads)


## <a name="owin-startup-class-detection"></a>Определение класса запуска OWIN

 Каждое приложение OWIN имеет класс запуска, где указываются компоненты для конвейера приложения. Существуют различные способы запуска класса могут подключаться со средой выполнения, в зависимости от модели размещения выберите (OwinHost, IIS и IIS Express). Класс startup, показанные в этом учебнике используется в каждое приложение размещения. Класс startup подключиться с размещения среды выполнения, используя одну из этих подходы:  

1. **Соглашение об именовании**: Katana ищет класс с именем `Startup` в пространстве имен, имя сборки или глобального пространства имен.
2. **Атрибут OwinStartup**: именно этот подход, укажите класс startup большинство разработчиков будет выполняться. Следующий атрибут будет присвоено класс startup `TestStartup` класса в `StartupDemo` пространства имен. 

    [!code-csharp[Main](owin-startup-class-detection/samples/sample1.cs)]

 `OwinStartup` Атрибут замещает соглашение об именовании. С помощью этого атрибута также можно указать понятное имя, однако с помощью понятного имени требует также использования `appSetting` элемент в файле конфигурации.
3. **В файле конфигурации элемент appSetting**: `appSetting` переопределяет `OwinStartup` атрибут и соглашение об именовании. Можно иметь несколько классов запуска (каждый из которых использует `OwinStartup` атрибут) и настройте класс, который при запуске будет загружаться в файл конфигурации с помощью разметки следующего вида:  

    [!code-xml[Main](owin-startup-class-detection/samples/sample2.xml)]

 Также можно использовать следующий раздел, явным образом указывающий запуска класса и сборки: 

    [!code-xml[Main](owin-startup-class-detection/samples/sample3.xml)]

 Следующий XML-код в файле конфигурации задает имя класса понятное запуска `ProductionConfiguration`.  

    [!code-xml[Main](owin-startup-class-detection/samples/sample4.xml)]

 Выше разметки должен использоваться со следующими `OwinStartup` атрибут, который указывает понятное имя и вызывает `ProductionStartup2` для выполнения.

    [!code-csharp[Main](owin-startup-class-detection/samples/sample5.cs?highlight=1,16)]
4. Чтобы отключить обнаружение запуска OWIN добавьте `appSetting owin:AutomaticAppStartup` со значением `"false"` в файле web.config.

    [!code-xml[Main](owin-startup-class-detection/samples/sample6.xml)]

## <a name="create-an-aspnet-web-app-using-owin-startup"></a>Создать веб-приложение ASP.NET с помощью запуска OWIN

1. Создайте пустое веб-приложение Asp.Net и назовите его **StartupDemo**. -Установить `Microsoft.Owin.Host.SystemWeb` с помощью диспетчера пакетов NuGet. Из **средства** последовательно выберите пункты **диспетчер пакетов библиотеки**, а затем **консоль диспетчера пакетов**. Введите следующую команду:  

    [!code-powershell[Main](owin-startup-class-detection/samples/sample7.ps1)]
- Добавьте класс запуска OWIN. В Visual Studio 2013 правой кнопкой мыши проект и выберите **добавить класс**. — в **Добавление нового элемента** диалогового окна введите *OWIN* в поле поиска и измените имя на файле Startup.cs, затем **добавить**.  
  
    ![](owin-startup-class-detection/_static/image1.png)   
  
 В следующий раз, необходимо добавить *класс запуска Owin*, будет доступен из **добавить** меню.  
   
    ![](owin-startup-class-detection/_static/image2.png)  
  
 Кроме того, щелкните правой кнопкой мыши проект и выберите **добавить**, а затем выберите **новый элемент**и выберите **запуска Owin класса**.  
  
    ![](owin-startup-class-detection/_static/image3.png)  
  
- Замените код, созданный в *файла Startup.cs* файл со следующим:  

    [!code-csharp[Main](owin-startup-class-detection/samples/sample8.cs?highlight=5,7,15-28,31-34)]
  
 `app.Use` Лямбда-выражение используется для регистрации компонента указанного по промежуточного слоя в конвейере OWIN. В этом случае настройка ведения журнала входящих запросов перед ответом на входящего запроса. `next` Параметр является делегатом ( [Func](https://msdn.microsoft.com/library/bb534960(v=vs.100).aspx) &lt; [задачи](https://msdn.microsoft.com/library/dd321424(v=vs.100).aspx) &gt; ) к следующему компоненту в конвейере. `app.Run` Лямбда-выражение подключается конвейера на входящие запросы и предоставляет механизм ответа.
     > [!NOTE]
     > В приведенном выше коде мы закомментирован `OwinStartup` атрибут и мы полагаетесь на соглашение выполнения класс с именем `Startup` .-клавишу ***F5*** для запуска приложения. Попадание обновления несколько раз.  
  
    ![](owin-startup-class-detection/_static/image4.png)  
Примечание: Значение, показанное в образы в этом учебнике не будет соответствовать число см. Строка миллисекунды используется для отображения новый ответ при обновлении страницы.  
 Можно просмотреть информацию трассировки в **вывода** окна.  
  
    ![](owin-startup-class-detection/_static/image5.png)

## <a name="add-more-startup-classes"></a>Добавить дополнительные классы запуска

В этом разделе мы добавим другого класса при запуске. Можно добавить несколько класс запуска OWIN в приложение. Например можно создать классы запуска для разработки, тестирования и эксплуатации.

1. Создайте новый класс запуска OWIN и назовите его `ProductionStartup`.
2. Замените сгенерированный код следующим кодом:

    [!code-csharp[Main](owin-startup-class-detection/samples/sample9.cs?highlight=14-18)]
3. Нажмите клавишу F5 элемента управления, чтобы запустить приложение. `OwinStartup` Атрибут указывает, запущена класс startup производства.  
  
    ![](owin-startup-class-detection/_static/image6.png)
4. Создайте еще один класс запуска OWIN и назовите его `TestStartup`.
5. Замените сгенерированный код следующим кодом:  

    [!code-csharp[Main](owin-startup-class-detection/samples/sample10.cs?highlight=6,14-18)]

 `OwinStartup` Выше перегрузка атрибут задает `TestingConfiguration` как *понятное* класса при запуске.
6. Откройте *web.config* и добавьте ключ запуска приложения OWIN, который указывает понятное имя класса при запуске файла:

    [!code-xml[Main](owin-startup-class-detection/samples/sample11.xml?highlight=3-5)]
7. Нажмите клавишу F5 элемента управления, чтобы запустить приложение. Элемент параметры приложения принимает высокий приоритет и тестового запуска конфигурации.  
  
    ![](owin-startup-class-detection/_static/image7.png)
8. Удалить *понятное* имя из `OwinStartup` атрибута в `TestStartup` класса.

    [!code-csharp[Main](owin-startup-class-detection/samples/sample12.cs)]
9. Замените на ключ запуска приложения OWIN в *web.config* файл со следующим:

    [!code-xml[Main](owin-startup-class-detection/samples/sample13.xml)]
10. Восстановить `OwinStartup` атрибута в каждом классе в код атрибута по умолчанию, созданную Visual Studio:  

    [!code-csharp[Main](owin-startup-class-detection/samples/sample14.cs)]

 Каждый ключ запуска приложения OWIN ниже вызовет производственного класса для выполнения. 

    [!code-xml[Main](owin-startup-class-detection/samples/sample15.xml)]

 Ключ последнего запуска указывает метод конфигурации запуска. Следующий ключ запуска приложения OWIN позволяет изменить имя класса конфигурации `MyConfiguration` .

    [!code-xml[Main](owin-startup-class-detection/samples/sample16.xml)]

## <a name="using-owinhostexe"></a>С помощью Owinhost.exe

1. Замените файл Web.config следующую разметку:  

    [!code-xml[Main](owin-startup-class-detection/samples/sample17.xml?highlight=3-6)]

 Ключ последнего wins, поэтому в данном случае `TestStartup` указано.
2. Установка Owinhost из PMC: 

    [!code-console[Main](owin-startup-class-detection/samples/sample18.cmd)]
3. Перейдите в папку приложения (папку, содержащую *Web.config* файл) и в командную строку и введите: 

    [!code-console[Main](owin-startup-class-detection/samples/sample19.cmd)]

 Командное окно будет показано следующее: 

    [!code-console[Main](owin-startup-class-detection/samples/sample20.cmd)]
4. Запустить браузер с URL-адрес `http://localhost:5000/`.  
  
    ![](owin-startup-class-detection/_static/image8.png)  
  
 OwinHost соблюдаться соглашения запуска, перечисленных выше.
5. В окне командной строки нажмите клавишу ВВОД, чтобы закрыть OwinHost.
6. В `ProductionStartup` класса, добавьте следующий атрибут OwinStartup, который указывает понятное имя *ProductionConfiguration*.

    [!code-csharp[Main](owin-startup-class-detection/samples/sample21.cs)]
7. В командную строку и введите: 

    [!code-console[Main](owin-startup-class-detection/samples/sample22.cmd)]

 Класс запуска рабочей загрузки.  
    ![](owin-startup-class-detection/_static/image9.png)  
 Наше приложение имеет несколько классов запуска, а в этом примере имеют отсроченные загружаемый класс запуска до времени выполнения.
8. Проверьте следующие параметры запуска среды выполнения:

    [!code-console[Main](owin-startup-class-detection/samples/sample23.cmd)]
