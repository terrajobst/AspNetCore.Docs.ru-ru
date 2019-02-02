---
uid: aspnet/overview/owin-and-katana/owin-startup-class-detection
title: Определение класса запуска OWIN | Документация Майкрософт
author: Praburaj
description: Этом руководстве демонстрируется настройка загружается какой класс запуска OWIN. Дополнительные сведения о OWIN см. в разделе Обзор проекта Katana. Этот учебник был...
ms.author: riande
ms.date: 01/28/2019
ms.assetid: 08257f55-36f4-4e39-9c88-2a5602838c79
msc.legacyurl: /aspnet/overview/owin-and-katana/owin-startup-class-detection
msc.type: authoredcontent
ms.openlocfilehash: 0b34cca8b48383dbb028106651758dff889ed614
ms.sourcegitcommit: ed76cc752966c604a795fbc56d5a71d16ded0b58
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 02/02/2019
ms.locfileid: "55667301"
---
<a name="owin-startup-class-detection"></a>Определение класса запуска OWIN
====================

> Этом руководстве демонстрируется настройка загружается какой класс запуска OWIN. Дополнительные сведения о OWIN, см. в разделе [Обзор проекта Katana](an-overview-of-project-katana.md). Это руководство было написано с Рик Андерсон ( [ @RickAndMSFT ](https://twitter.com/#!/RickAndMSFT) ), Praburaj Thiagarajan и Говард Дайеркинг ( [ @howard \_Дайеркинг](https://twitter.com/howard_dierking) ).
>
> ## <a name="prerequisites"></a>Предварительные требования
>
> [Visual Studio 2017](https://visualstudio.microsoft.com/downloads/)


## <a name="owin-startup-class-detection"></a>Определение класса запуска OWIN

 Каждое приложение OWIN имеет класс запуска, где указываются компоненты конвейера приложения. Существует несколько способов, вы можете подключиться классе запуска в среде выполнения, в зависимости от модели размещения выберите (OwinHost, IIS и IIS Express). Класс startup, в этом учебнике используется в каждое приложение размещения. Класс startup для соединения с размещения среды выполнения с помощью, один из этих приближается к:

1. **Соглашение об именовании**: Katana ищет класс с именем `Startup` в пространстве имен, имя сборки или глобальное пространство имен.
2. **Атрибут OwinStartup**: Именно этот подход, большинство разработчиков потребуется указать класс startup. Следующий атрибут будет присвоено класс startup `TestStartup` в класс `StartupDemo` пространства имен.

    [!code-csharp[Main](owin-startup-class-detection/samples/sample1.cs)]

   `OwinStartup` Атрибут переопределяет соглашение об именовании. Также можно указать понятное имя с этим атрибутом, тем не менее, используя понятное имя требует также использования `appSetting` элемент в файле конфигурации.
3. **В файле конфигурации элемент appSetting**: `appSetting` Переопределяет `OwinStartup` атрибут и соглашение об именовании. Можно иметь несколько классов запуска (каждый с помощью `OwinStartup` атрибут) и настройки, какой класс запуска будут загружены в файл конфигурации, используя разметку, аналогичную следующей:

    [!code-xml[Main](owin-startup-class-detection/samples/sample2.xml)]

   Также можно использовать следующий раздел, который явно указывает класс startup и сборки:

    [!code-xml[Main](owin-startup-class-detection/samples/sample3.xml)]

   Следующий код XML в файле конфигурации задает имя класса понятное запуска `ProductionConfiguration`.

    [!code-xml[Main](owin-startup-class-detection/samples/sample4.xml)]

   Приведенная выше разметка должен использоваться со следующими `OwinStartup` атрибут, который задает понятное имя и вызывает `ProductionStartup2` класса для запуска.

    [!code-csharp[Main](owin-startup-class-detection/samples/sample5.cs?highlight=1,16)]
4. Чтобы отключить обнаружение запуска OWIN добавьте `appSetting owin:AutomaticAppStartup` со значением `"false"` в файле web.config.

    [!code-xml[Main](owin-startup-class-detection/samples/sample6.xml)]

## <a name="create-an-aspnet-web-app-using-owin-startup"></a>Создать веб-приложение ASP.NET, с помощью запуска OWIN

1. Создайте пустое веб-приложение Asp.Net и назовите его **StartupDemo**. -Установить `Microsoft.Owin.Host.SystemWeb` с помощью диспетчера пакетов NuGet. Из **средства** меню, выберите **диспетчер пакетов NuGet**, а затем **консоль диспетчера пакетов**. Введите следующую команду:

    [!code-powershell[Main](owin-startup-class-detection/samples/sample7.ps1)]
2. Добавьте класс запуска OWIN. В Visual Studio 2017, щелкните правой кнопкой мыши проект и выберите **Добавление класса**. — в **Добавление нового элемента** диалогового окна введите *OWIN* в поле поиска и измените имя на Startup.cs, а затем выберите **добавить**.

     ![](owin-startup-class-detection/_static/image1.png)

   В следующий раз, вы хотите добавить *класс запуска Owin*, будет доступен из **добавить** меню.

     ![](owin-startup-class-detection/_static/image2.png)

   Кроме того, можно правой кнопкой мыши проект и выберите **добавить**, а затем выберите **новый элемент**, а затем выберите **класс запуска Owin**.

     ![](owin-startup-class-detection/_static/image3.png)

- Замените сгенерированный код в *Startup.cs* файл со следующими:

    [!code-csharp[Main](owin-startup-class-detection/samples/sample8.cs?highlight=5,7,15-28,31-34)]

  `app.Use` Лямбда-выражение используется для регистрации компонента указанного по промежуточного слоя в конвейер OWIN. В этом случае мы собираемся настроить ведение журнала входящих запросов, прежде чем отвечать на входящий запрос. `next` Параметр является делегатом ( [Func](https://msdn.microsoft.com/library/bb534960(v=vs.100).aspx) &lt; [задачи](https://msdn.microsoft.com/library/dd321424(v=vs.100).aspx) &gt; ) к следующему компоненту в конвейере. `app.Run` Лямбда-выражение подключает конвейер для входящих запросов и предоставляет механизм ответа.
     > [!NOTE]
     > В приведенном выше коде мы закомментирован `OwinStartup` атрибут и мы полагаетесь на соглашению под управлением с именем класса `Startup` .-Press ***F5*** для запуска приложения. Несколько раз нажмите кнопку обновления.

    ![](owin-startup-class-detection/_static/image4.png) Примечание. Значение, показанное в образы в этом руководстве не будет соответствовать номера, который отображается. Миллисекунды строка используется для отображения нового ответа при обновлении страницы.
  Вы увидите сведения о трассировке в **вывода** окна.

    ![](owin-startup-class-detection/_static/image5.png)

## <a name="add-more-startup-classes"></a>Добавить дополнительные классы запуска

В этом разделе мы добавим еще один класс запуска. Можно добавить несколько класс запуска OWIN в приложение. Например можно создать классы запуска для разработки, тестирования и эксплуатации.

1. Создайте новый класс запуска OWIN и назовите его `ProductionStartup`.
2. Замените сгенерированный код следующим кодом:

    [!code-csharp[Main](owin-startup-class-detection/samples/sample9.cs?highlight=14-18)]
3. Нажмите клавишу F5 элемента управления, чтобы запустить приложение. `OwinStartup` Атрибут указывает, выполняется класс startup рабочей среде.

    ![](owin-startup-class-detection/_static/image6.png)
4. Создайте еще один класс запуска OWIN и назовите его `TestStartup`.
5. Замените сгенерированный код следующим кодом:

    [!code-csharp[Main](owin-startup-class-detection/samples/sample10.cs?highlight=6,14-18)]

   `OwinStartup` Перегрузка атрибут выше указывает `TestingConfiguration` как *понятное* имя класса Startup.
6. Откройте *web.config* файл и добавьте ключ запуска приложения OWIN, который указывает понятное имя в классе Startup:

    [!code-xml[Main](owin-startup-class-detection/samples/sample11.xml?highlight=3-5)]
7. Нажмите клавишу F5 элемента управления, чтобы запустить приложение. Элемент параметров приложения имеет высокий приоритет и тест запускается конфигурации.

    ![](owin-startup-class-detection/_static/image7.png)
8. Удалить *понятное* имя из `OwinStartup` атрибут в `TestStartup` класса.

    [!code-csharp[Main](owin-startup-class-detection/samples/sample12.cs)]
9. Замените ключ запуска приложения OWIN в *web.config* файл со следующими:

    [!code-xml[Main](owin-startup-class-detection/samples/sample13.xml)]
10. Отменить `OwinStartup` атрибута этих классов в код атрибута по умолчанию, созданный средой Visual Studio:

    [!code-csharp[Main](owin-startup-class-detection/samples/sample14.cs)]

    Каждый ключ запуска приложения OWIN ниже приведет к производственного класса для запуска.

    [!code-xml[Main](owin-startup-class-detection/samples/sample15.xml)]

    Ключ последнего запуска способ конфигурации запуска. Следующий раздел запуска приложения OWIN позволяет изменить имя класса конфигурации для `MyConfiguration` .

    [!code-xml[Main](owin-startup-class-detection/samples/sample16.xml)]

## <a name="using-owinhostexe"></a>С помощью Owinhost.exe

1. Замените файл Web.config следующую разметку:

    [!code-xml[Main](owin-startup-class-detection/samples/sample17.xml?highlight=3-6)]

   Ключ последнего wins, поэтому в данном случае `TestStartup` указан.
2. Установка Owinhost из PMC:

    [!code-console[Main](owin-startup-class-detection/samples/sample18.cmd)]
3. Перейдите в папку приложения (в папку, содержащую *Web.config* файл) и в командную строку и введите:

    [!code-console[Main](owin-startup-class-detection/samples/sample19.cmd)]

   Командное окно будет показано следующее:

    [!code-console[Main](owin-startup-class-detection/samples/sample20.cmd)]
4. Окно браузера с URL-адрес `http://localhost:5000/`.

    ![](owin-startup-class-detection/_static/image8.png)

   OwinHost соблюдаться соглашения, запуска, перечисленные выше.
5. В окне командной строки нажмите клавишу ВВОД для выхода OwinHost.
6. В `ProductionStartup` добавьте следующий атрибут OwinStartup, который указывает понятное имя *ProductionConfiguration*.

    [!code-csharp[Main](owin-startup-class-detection/samples/sample21.cs)]
7. В командную строку и введите:

    [!code-console[Main](owin-startup-class-detection/samples/sample22.cmd)]

   Класс startup рабочей загрузки.
    ![](owin-startup-class-detection/_static/image9.png)

   Наше приложение имеет несколько классов запуска, а в этом примере мы имеют отсроченные какой класс startup для загрузки только во время выполнения.
8. Проверьте следующие параметры запуска среды выполнения:

    [!code-console[Main](owin-startup-class-detection/samples/sample23.cmd)]
