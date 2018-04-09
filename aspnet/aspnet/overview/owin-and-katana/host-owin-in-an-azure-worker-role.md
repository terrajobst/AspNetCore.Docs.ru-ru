---
uid: aspnet/overview/owin-and-katana/host-owin-in-an-azure-worker-role
title: Разместить OWIN в рабочей роли Azure | Документы Microsoft
author: MikeWasson
description: Этот учебник демонстрирует резидентной OWIN в рабочей роли Microsoft Azure. Откройте веб-интерфейс .NET (OWIN) определяет абстракцию между .NET веб-сервера...
ms.author: aspnetcontent
manager: wpickett
ms.date: 04/11/2014
ms.topic: article
ms.assetid: 07aa855a-92ee-4d43-ba66-5bfd7de20ee6
ms.technology: ''
ms.prod: .net-framework
msc.legacyurl: /aspnet/overview/owin-and-katana/host-owin-in-an-azure-worker-role
msc.type: authoredcontent
ms.openlocfilehash: 13bccc4b2d6f1b22c94446deaf6795dab766275b
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/06/2018
---
<a name="host-owin-in-an-azure-worker-role"></a>Узел OWIN в рабочей роли Azure
====================
по [Mike Wasson](https://github.com/MikeWasson)

> Этот учебник демонстрирует резидентной OWIN в рабочей роли Microsoft Azure.
> 
> [Открыть веб-интерфейс .NET](http://owin.org/) (OWIN) определяет абстракцию между .NET веб-серверов и веб-приложений. OWIN разрывает связь веб-приложения с сервера, что делает OWIN идеальной для резидентного размещения веб-приложения в свой собственный процесс, за пределами служб IIS — например, внутри рабочей роли Azure.
> 
> В этом учебнике вы узнаете, как для резидентных приложений OWIN в рабочей роли Microsoft Azure. Дополнительные сведения о рабочих ролей см. в разделе [модели выполнения Azure](https://azure.microsoft.com/documentation/articles/fundamentals-application-models/#CloudServices).
> 
> ## <a name="software-versions-used-in-the-tutorial"></a>Версии программного обеспечения, используемая в этом учебнике
> 
> 
> - [Visual Studio 2013](https://www.microsoft.com/visualstudio/eng/2013-downloads)
> - [Azure SDK для .NET 2.3](https://azure.microsoft.com/downloads/)
> - [Microsoft.Owin.Selfhost 2.1.0](http://www.nuget.org/packages/Microsoft.Owin.SelfHost/2.1.0)


## <a name="create-a-microsoft-azure-project"></a>Создание проекта Microsoft Azure

Запустите Visual Studio с правами администратора. Для отладки приложения локально, с помощью эмулятора вычислений Azure требуются права администратора.

На **файл** меню, нажмите кнопку **New**, нажмите кнопку **проекта**. Из **установленные шаблоны**, в Visual C#, нажмите кнопку **облака** и нажмите кнопку **облачных служб Windows Azure**. Назовите проект «AzureApp» и нажмите кнопку **ОК**.

[![](host-owin-in-an-azure-worker-role/_static/image2.png)](host-owin-in-an-azure-worker-role/_static/image1.png)

В **новой облачной службы Windows Azure** диалоговое окно, дважды щелкните **рабочей роли**. Оставьте имя по умолчанию («WorkerRole1»). Этот шаг добавляет рабочей роли в решение. Нажмите кнопку **ОК**.

[![](host-owin-in-an-azure-worker-role/_static/image4.png)](host-owin-in-an-azure-worker-role/_static/image3.png)

Решение Visual Studio, которая создается содержит два проекта:

- &quot;AzureApp&quot; определяет роли и конфигурации для приложения Azure.
- &quot;WorkerRole1&quot; содержит код для рабочей роли.

Как правило приложение Azure может содержать несколько ролей, несмотря на то, что в этом учебнике используется одна роль.

![](host-owin-in-an-azure-worker-role/_static/image5.png)

## <a name="add-the-owin-self-host-packages"></a>Добавление пакетов резидентной OWIN

Из **средства** меню, нажмите кнопку **диспетчер пакетов библиотеки**, нажмите кнопку **консоль диспетчера пакетов**.

В окне консоли диспетчера пакетов введите следующую команду:

[!code-console[Main](host-owin-in-an-azure-worker-role/samples/sample1.cmd)]

## <a name="add-an-http-endpoint"></a>Добавить конечную точку HTTP

В обозревателе решений разверните проект AzureApp. Разверните узел роли, щелкните правой кнопкой мыши WorkerRole1 и выберите **свойства**.

![](host-owin-in-an-azure-worker-role/_static/image6.png)

Нажмите кнопку **конечные точки**, а затем нажмите кнопку **добавить конечную точку**.

В **протокола** раскрывающемся списке выберите «http». В **общий порт** и **частный порт**, введите 80. Эти номера портов могут быть разными. Открытый порт является то, что клиенты будут использовать при отправке запроса к роли.

[![](host-owin-in-an-azure-worker-role/_static/image8.png)](host-owin-in-an-azure-worker-role/_static/image7.png)

## <a name="create-the-owin-startup-class"></a>Создайте класс запуска OWIN

В обозревателе решений щелкните правой кнопкой мыши проект WorkerRole1 и выберите **добавить** / **класса** для добавления нового класса. Присвойте классу имя `Startup`.

Замените все стандартный код следующим кодом:

[!code-csharp[Main](host-owin-in-an-azure-worker-role/samples/sample2.cs)]

`UseWelcomePage` Метод расширения добавляет простой HTML-страницы для приложения, чтобы проверить работы сайта.

## <a name="start-the-owin-host"></a>Запустить узел OWIN

Откройте файл WorkerRole.cs. Этот класс определяет код, который выполняется при остановке и запуске рабочей роли.

Добавьте следующий код с помощью инструкции:

[!code-csharp[Main](host-owin-in-an-azure-worker-role/samples/sample3.cs)]

Добавить **IDisposable** члена `WorkerRole` класса:

[!code-csharp[Main](host-owin-in-an-azure-worker-role/samples/sample4.cs)]

В `OnStart` метода, добавьте следующий код для запуска узла:

[!code-csharp[Main](host-owin-in-an-azure-worker-role/samples/sample5.cs?highlight=5)]

**WebApp.Start** метод запускает узел OWIN. Имя `Startup` класса является параметром типа метода. По соглашению, узел будет вызывать `Configure` метода этого класса.

Переопределить `OnStop` для удаления  *\_приложения* экземпляр:

[!code-csharp[Main](host-owin-in-an-azure-worker-role/samples/sample6.cs)]

Ниже приведен полный код для WorkerRole.cs:

[!code-csharp[Main](host-owin-in-an-azure-worker-role/samples/sample7.cs)]

Выполните сборку решения и нажмите клавишу F5 для запуска приложения локально в эмуляторе вычислений Azure. В зависимости от настроек брандмауэра может потребоваться разрешить эмулятор через брандмауэр.

Эмулятор вычислений назначает локальный IP-адрес конечной точки. IP-адрес можно найти, просмотрев пользовательский Интерфейс эмулятора вычислений. Щелкните правой кнопкой мыши значок эмулятора в задаче панели области уведомлений и выберите **Показать пользовательский Интерфейс эмулятора вычислений**.

[![](host-owin-in-an-azure-worker-role/_static/image10.png)](host-owin-in-an-azure-worker-role/_static/image9.png)

Поиск IP-адреса в развертываниях службы развертывания [id], сведения о службе. Откройте веб-браузер и перейдите к http://<em>адрес</em>, где <em>адрес</em> IP-адрес, назначенный эмулятор вычислений; например, `http://127.0.0.1:80`. Вы увидите страницу приветствия OWIN:

![](host-owin-in-an-azure-worker-role/_static/image11.png)

## <a name="deploy-to-azure"></a>Развертывание в Azure

Для выполнения этого шага необходимо иметь учетную запись Azure. Если у вас еще нет один, можно создать бесплатную пробную учетную запись всего за несколько минут. Дополнительные сведения см. в разделе [бесплатная пробная версия Microsoft](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A261C142F).

В обозревателе решений щелкните правой кнопкой мыши проект AzureApp. Нажмите **Публиковать**.

![](host-owin-in-an-azure-worker-role/_static/image12.png)

Если вы не вошли учетную запись Azure, щелкните **входа**.

[![](host-owin-in-an-azure-worker-role/_static/image14.png)](host-owin-in-an-azure-worker-role/_static/image13.png)

После входа, выберите подписку и нажмите кнопку **Далее**.

[![](host-owin-in-an-azure-worker-role/_static/image16.png)](host-owin-in-an-azure-worker-role/_static/image15.png)

Введите имя для облачной службы и выберите область. Нажмите кнопку **Создать**.

![](host-owin-in-an-azure-worker-role/_static/image17.png)

Нажмите кнопку **Опубликовать**.

[![](host-owin-in-an-azure-worker-role/_static/image19.png)](host-owin-in-an-azure-worker-role/_static/image18.png)

В окне Журнал действий Azure отображается ход выполнения развертывания. При развертывании приложения, перейдите к `http://appname.cloudapp.net/`, где *appname* имя облачной службы.

## <a name="additional-resources"></a>Дополнительные ресурсы

- [Обзор проекта Katana](an-overview-of-project-katana.md)
- [Katana проекта на GitHub](https://github.com/aspnet/AspNetKatana/)
