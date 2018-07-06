---
uid: web-forms/overview/deployment/web-deployment-in-the-enterprise/setting-up-the-contact-manager-solution
title: Настройка решения диспетчера контактов | Документация Майкрософт
author: jrjlee
description: В этом разделе описывается скачивание и настройка решения диспетчера контактов для локального запуска на компьютере разработчика.
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/04/2012
ms.topic: article
ms.assetid: 200b973c-776b-4a9b-9e82-39fda6120a52
ms.technology: dotnet-webforms
msc.legacyurl: /web-forms/overview/deployment/web-deployment-in-the-enterprise/setting-up-the-contact-manager-solution
msc.type: authoredcontent
ms.openlocfilehash: a7675a793909ec4d95164ee47a3a43f73600c5bc
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 07/03/2018
ms.locfileid: "37366693"
---
<a name="setting-up-the-contact-manager-solution"></a>Настройка решения диспетчера контактов
====================
по [Джейсон Lee](https://github.com/jrjlee)

[Загрузить PDF-файл](https://msdnshared.blob.core.windows.net/media/MSDNBlogsFS/prod.evol.blogs.msdn.com/CommunityServer.Blogs.Components.WeblogFiles/00/00/00/63/56/8130.DeployingWebAppsInEnterpriseScenarios.pdf)

> В этом разделе описывается скачивание и настройка решения диспетчера контактов для локального запуска на компьютере разработчика.


## <a name="system-requirements"></a>Требования к системе

Для локального запуска решения диспетчера контактов и выполнения других задач, описанных в этом руководстве, необходимо установить это программное обеспечение на рабочей станции разработчика:

- Visual Studio 2010 с пакетом 1, Premium или Ultimate Edition
- Internet Information Services (IIS) 7.5 Express
- SQL Server Express 2008 R2
- IIS средство веб-развертывания (Web Deploy) 2.1 или более поздней версии
- ASP.NET 4.0
- ASP.NET MVC 3
- .NET Framework 4
- .NET Framework 3.5 SP1

За исключением Visual Studio 2010, можно загрузить и установить последние версии всех этих продуктов и компонентов с помощью [установщика веб-платформы](https://go.microsoft.com/?linkid=9805118).

## <a name="download-and-extract-the-solution"></a>Загрузите и извлеките решения

Можно загрузить пример приложения диспетчера контактов из коллекции кода MSDN [здесь](https://code.msdn.microsoft.com/Deploying-Web-Applications-9d9093c0).

## <a name="configure-and-run-the-solution"></a>Настройка и запуск решения

Чтобы настроить и запустить решение диспетчера контактов на локальном компьютере, необходимо выполнить следующие действия:

1. Если вы не сделали, создайте локальной базы данных служб приложения ASP.NET с членством и ролями управления функциями.
2. Изменение строк подключения в *web.config* файлы, чтобы он указывал на локальный экземпляр SQL Server Express.
3. Запустите решение из Visual Studio 2010.

В оставшейся части этого раздела предоставляет дополнительные рекомендации о том, как выполнить каждую из этих задач.

**Для создания базы данных служб приложений**

1. Откройте командную строку Visual Studio 2010. Для этого на **запустить** последовательно выберите пункты **все программы**, нажмите кнопку **Microsoft Visual Studio 2010**, нажмите кнопку **средств Visual Studio**, а затем Нажмите кнопку **Командная строка Visual Studio (2010)**.
2. В командной строке введите следующую команду и нажмите клавишу ВВОД:

    [!code-console[Main](setting-up-the-contact-manager-solution/samples/sample1.cmd)]

    1. Используйте **– C** параметр, чтобы указать строку подключения для сервера базы данных.
    2. Используйте **** для указания функций, которые вы хотите добавить в базу данных служб приложения. В этом случае **m** указывает, что вы хотите добавить поддержку для поставщика членства и **r** указывает, что вы хотите добавить поддержку диспетчера ролей.
    3. Используйте **– d** параметр, чтобы указать имя для вашей базы данных служб приложений. Если параметр не задан, программа создаст базу данных с именем по умолчанию **aspnetdb**.
3. Когда база данных создана успешно, командной строке будет отображаться подтверждение.

    ![](setting-up-the-contact-manager-solution/_static/image1.png)

> [!NOTE]
> Дополнительные сведения о aspnet\_regsql служебной программы, см. в разделе [средство регистрации ASP.NET SQL Server (Aspnet\_regsql.exe)](https://msdn.microsoft.com/library/ms229862(v=vs.100).aspx).


Следующим шагом является убедитесь, что строки подключения в решение диспетчера контактов указывают на локальном экземпляре SQL Server Express.

**Чтобы обновить строки соединения**

1. Откройте решение диспетчера контактов в Visual Studio 2010.
2. В **обозревателе решений** окне разверните **ContactManager.Mvc** проекта, а затем дважды щелкните **Web.config** узла.

    > [!NOTE]
    > ContactManager.Mvc проект включает два *web.config* файлов. Необходимо изменить файл проекта.

    ![](setting-up-the-contact-manager-solution/_static/image2.png)
3. В **connectionStrings** элемента, убедитесь, что строка подключения с именем **ApplicationServices** указывает локальную базу данных служб приложения ASP.NET.

    [!code-xml[Main](setting-up-the-contact-manager-solution/samples/sample2.xml)]
4. В **обозревателе решений** окне разверните **ContactManager.Service** проекта, а затем дважды щелкните **Web.config** узла.

    ![](setting-up-the-contact-manager-solution/_static/image3.png)
5. В **connectionStrings** элемента в строке подключения с именем **ContactManagerContext**, убедитесь, что **источника данных** свойство имеет значение локального экземпляра SQL Server Express. Не нужно изменять что-нибудь еще в строке подключения.

    [!code-xml[Main](setting-up-the-contact-manager-solution/samples/sample3.xml)]
6. Сохраните все открытые файлы.

Теперь можно все готово к запуску решения диспетчера контактов на локальном компьютере.

> [!NOTE]
> Если вы выполните следующие действия без предварительного создания базы данных служб приложений, ASP.NET создаст базу данных первой попытке создать пользователя. Тем не менее вручную создав базу данных дает гораздо больший контроль над набора средств служб приложения, которые требуется поддерживать.


**Чтобы запустить решение диспетчера контактов**

1. В Visual Studio 2010 нажмите клавишу F5.
2. Internet Explorer запускается и запрашивает URL-адреса приложения Contact Manager ASP.NET MVC 3. По умолчанию приложение отображает **все контакты** страницы.

    ![](setting-up-the-contact-manager-solution/_static/image4.png)
3. Добавить несколько контактов и убедитесь, что приложение работает должным образом.

    ![](setting-up-the-contact-manager-solution/_static/image5.png)
4. Перейдите к `http://localhost:50114/Account/Register` (Настройка URL-адрес, если вы размещаете приложение на другой порт). Добавьте имя пользователя, адрес электронной почты и пароль и убедитесь, что вы можете зарегистрировать учетную запись успешно.

    ![](setting-up-the-contact-manager-solution/_static/image6.png)
5. Перейдите к `http://localhost:50114/Account/LogOn` (Настройка URL-адрес, если вы размещаете приложение на другой порт). Убедитесь, что вы можете выполнить вход с использованием учетной записи, которую вы только что создали.

    ![](setting-up-the-contact-manager-solution/_static/image7.png)
6. Закройте Internet Explorer, чтобы остановить отладку.

## <a name="conclusion"></a>Заключение

На этом этапе решение диспетчера контактов должен быть полностью настроен для запуска на локальном компьютере. Решение можно использовать как ссылка, при работе через другие подразделы в этом руководстве.

Следующий раздел, [основные сведения о файле проекта](understanding-the-project-file.md), объясняется, как можно использовать пользовательские файлы проекта Microsoft Build Engine (MSBuild) в рамках решения диспетчера контактов для управления процессом развертывания.

> [!div class="step-by-step"]
> [Назад](the-contact-manager-solution.md)
> [Вперед](understanding-the-project-file.md)
