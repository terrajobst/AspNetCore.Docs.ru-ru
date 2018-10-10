---
uid: aspnet/overview/owin-and-katana/host-owin-in-an-azure-worker-role
title: Размещение OWIN в рабочей роли Azure | Документация Майкрософт
author: MikeWasson
description: Этом руководстве показано, как для саморазмещения OWIN в рабочей роли Microsoft Azure. Откройте веб-интерфейс для .NET (OWIN) определяет абстракции между .NET веб-сервер...
ms.author: riande
ms.date: 04/11/2014
ms.assetid: 07aa855a-92ee-4d43-ba66-5bfd7de20ee6
msc.legacyurl: /aspnet/overview/owin-and-katana/host-owin-in-an-azure-worker-role
msc.type: authoredcontent
ms.openlocfilehash: dbf0964695dd2592d063b05c0778923edffe8e2e
ms.sourcegitcommit: a4dcca4f1cb81227c5ed3c92dc0e28be6e99447b
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/10/2018
ms.locfileid: "48910781"
---
<a name="host-owin-in-an-azure-worker-role"></a>Размещение OWIN в рабочей роли Azure
====================
по [Майк Уоссон](https://github.com/MikeWasson)

> Этом руководстве показано, как для саморазмещения OWIN в рабочей роли Microsoft Azure.
>
> [Открыть веб-интерфейс .NET](http://owin.org/) (OWIN) определяет абстракции между веб-серверов .NET и веб-приложений. OWIN отделяет веб-приложения на сервер, который идеально OWIN для резидентного размещения веб-приложения в собственном процессе, за пределами IIS — например, внутри рабочей роли Azure.
>
> В этом руководстве вы узнаете, как для резидентного размещения приложений OWIN в рабочей роли Microsoft Azure. Дополнительные сведения о рабочих ролей, см. в разделе [модели выполнения Azure](https://azure.microsoft.com/documentation/articles/fundamentals-application-models/#CloudServices).
>
> ## <a name="software-versions-used-in-the-tutorial"></a>Версии программного обеспечения, используемые в этом руководстве
>
>
> - [Visual Studio 2013](https://my.visualstudio.com/Downloads?q=visual%20studio%202013)
> - [Пакет Azure SDK для .NET 2.3](https://azure.microsoft.com/downloads/)
> - [Microsoft.Owin.Selfhost 2.1.0](http://www.nuget.org/packages/Microsoft.Owin.SelfHost/2.1.0)


## <a name="create-a-microsoft-azure-project"></a>Создание проекта Microsoft Azure

Запустите Visual Studio с правами администратора. Для отладки приложения в локальной среде, с помощью эмулятора вычислений Azure требуются права администратора.

На **файл** меню, щелкните **New**, затем нажмите кнопку **проекта**. Из **установленные шаблоны**, в разделе Visual C#, щелкните **Cloud** и нажмите кнопку **облачной службы Windows Azure**. Присвойте проекту имя «AzureApp» и нажмите кнопку **ОК**.

[![](host-owin-in-an-azure-worker-role/_static/image2.png)](host-owin-in-an-azure-worker-role/_static/image1.png)

В **новая облачная служба Windows Azure** диалоговое окно, дважды щелкните **рабочей роли**. Оставьте имя по умолчанию («WorkerRole1»). Этот шаг добавляет рабочей роли в решение. Нажмите кнопку **ОК**.

[![](host-owin-in-an-azure-worker-role/_static/image4.png)](host-owin-in-an-azure-worker-role/_static/image3.png)

Решение Visual Studio, который создается содержит два проекта:

- &quot;AzureApp&quot; определяет роли и конфигурации для приложения Azure.
- &quot;WorkerRole1&quot; содержит код для рабочей роли.

Как правило приложение Azure может содержать несколько ролей, несмотря на то, что в этом руководстве используется одна роль.

![](host-owin-in-an-azure-worker-role/_static/image5.png)

## <a name="add-the-owin-self-host-packages"></a>Добавьте пакеты резидентного размещения OWIN

Из **средства** меню, щелкните **диспетчер пакетов NuGet**, затем нажмите кнопку **консоль диспетчера пакетов**.

В окне консоли диспетчера пакетов введите следующую команду:

[!code-console[Main](host-owin-in-an-azure-worker-role/samples/sample1.cmd)]

## <a name="add-an-http-endpoint"></a>Добавить конечную точку HTTP

В обозревателе решений разверните проект AzureApp. Разверните узел роли, щелкните правой кнопкой мыши WorkerRole1 и выберите **свойства**.

![](host-owin-in-an-azure-worker-role/_static/image6.png)

Нажмите кнопку **конечные точки**, а затем нажмите кнопку **добавить конечную точку**.

В **протокола** раскрывающегося списка выберите «http». В **общий порт** и **частный порт**, введите 80. Эти номера портов могут отличаться. Общий порт — новые клиенты будут использовать при отправке запроса к роли.

[![](host-owin-in-an-azure-worker-role/_static/image8.png)](host-owin-in-an-azure-worker-role/_static/image7.png)

## <a name="create-the-owin-startup-class"></a>Создать класс запуска OWIN

В обозревателе решений щелкните правой кнопкой мыши проект WorkerRole1 и выберите **добавить** / **класс** для добавления нового класса. Присвойте классу имя `Startup`.

Замените весь код шаблона следующим кодом:

[!code-csharp[Main](host-owin-in-an-azure-worker-role/samples/sample2.cs)]

`UseWelcomePage` Добавляет метод расширения простую HTML-страницу приложения, чтобы проверить, работает сайт.

## <a name="start-the-owin-host"></a>Запустить узел OWIN

Откройте файл WorkerRole.cs. Этот класс определяет код, выполняемый при рабочей роли запущено и остановлено.

Добавьте следующий оператор using:

[!code-csharp[Main](host-owin-in-an-azure-worker-role/samples/sample3.cs)]

Добавить **IDisposable** члена `WorkerRole` класса:

[!code-csharp[Main](host-owin-in-an-azure-worker-role/samples/sample4.cs)]

В `OnStart` метод, добавьте следующий код для запуска узла:

[!code-csharp[Main](host-owin-in-an-azure-worker-role/samples/sample5.cs?highlight=5)]

**WebApp.Start** метод начинает хоста OWIN. Имя `Startup` класс является параметром типа метода. По соглашению, главное приложение выполняет вызов `Configure` метод этого класса.

Переопределить `OnStop` для удаления  *\_приложения* экземпляр:

[!code-csharp[Main](host-owin-in-an-azure-worker-role/samples/sample6.cs)]

Ниже приведен полный код для WorkerRole.cs:

[!code-csharp[Main](host-owin-in-an-azure-worker-role/samples/sample7.cs)]

Выполните сборку решения и нажмите клавишу F5 для запуска приложения локально в эмуляторе вычислений Azure. В зависимости от настроек брандмауэра может потребоваться предоставление эмулятору через брандмауэр.

Эмулятор вычислений назначает локальный IP-адрес конечной точки. IP-адрес можно найти, просмотрев пользовательский Интерфейс эмулятора вычислений. Щелкните правой кнопкой мыши значок эмулятора в задаче области уведомлений и выберите **Показать пользовательский Интерфейс эмулятора вычислений**.

[![](host-owin-in-an-azure-worker-role/_static/image10.png)](host-owin-in-an-azure-worker-role/_static/image9.png)

Найти IP-адрес в группе развертывания служб, развертывания [id], сведения о службе. Откройте веб-браузер и перейдите на http://<em>адрес</em>, где <em>адрес</em> IP-адрес, назначенный с помощью эмулятора вычислений; например, `http://127.0.0.1:80`. Вы должны увидеть страницу приветствия OWIN:

![](host-owin-in-an-azure-worker-role/_static/image11.png)

## <a name="deploy-to-azure"></a>Развертывание в Azure

Для выполнения этого шага необходимо иметь учетную запись Azure. Если ее еще нет, можно создать бесплатную пробную учетную запись всего за несколько минут. Дополнительные сведения см. в разделе [бесплатной пробной версии Microsoft Azure](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A261C142F).

В обозревателе решений щелкните правой кнопкой мыши проект AzureApp. Нажмите **Публиковать**.

![](host-owin-in-an-azure-worker-role/_static/image12.png)

Если вы не вошли учетную запись Azure, щелкните **Sign In**.

[![](host-owin-in-an-azure-worker-role/_static/image14.png)](host-owin-in-an-azure-worker-role/_static/image13.png)

После входа, выберите подписку и нажмите кнопку **Далее**.

[![](host-owin-in-an-azure-worker-role/_static/image16.png)](host-owin-in-an-azure-worker-role/_static/image15.png)

Введите имя для облачной службы и выбрать регион. Нажмите кнопку **Создать**.

![](host-owin-in-an-azure-worker-role/_static/image17.png)

Нажмите кнопку **Опубликовать**.

[![](host-owin-in-an-azure-worker-role/_static/image19.png)](host-owin-in-an-azure-worker-role/_static/image18.png)

Окно журнала действий Azure показывает ход выполнения развертывания. При развертывании приложения, перейдите к `http://appname.cloudapp.net/`, где *appname* имя облачной службы.

## <a name="additional-resources"></a>Дополнительные ресурсы

- [Обзор проекта Katana](an-overview-of-project-katana.md)
- [Проект Katana на GitHub](https://github.com/aspnet/AspNetKatana/)
