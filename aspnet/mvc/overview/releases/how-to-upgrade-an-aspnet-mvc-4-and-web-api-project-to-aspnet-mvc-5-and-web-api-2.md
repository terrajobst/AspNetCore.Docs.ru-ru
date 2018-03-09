---
uid: mvc/overview/releases/how-to-upgrade-an-aspnet-mvc-4-and-web-api-project-to-aspnet-mvc-5-and-web-api-2
title: "Процедура обновления ASP.NET MVC 4 и веб-проекта ASP.NET MVC 5 и веб-API 2 API | Документы Microsoft"
author: Rick-Anderson
description: "ASP.NET MVC 5 и веб-API 2 перевести целый ряд новых функций, включая маршрутизацией атрибутов, фильтры проверки подлинности и многое другое."
ms.author: aspnetcontent
manager: wpickett
ms.date: 10/17/2013
ms.topic: article
ms.assetid: db0d02d9-58e8-4a0b-8d7d-b8df8ea97b88
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/releases/how-to-upgrade-an-aspnet-mvc-4-and-web-api-project-to-aspnet-mvc-5-and-web-api-2
msc.type: authoredcontent
ms.openlocfilehash: 05a3189cf105d1230b96e90b46ea5ab60fef1bf1
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 11/10/2017
---
<a name="how-to-upgrade-an-aspnet-mvc-4-and-web-api-project-to-aspnet-mvc-5-and-web-api-2"></a>Как обновить ASP.NET MVC 4 и API веб-проекта ASP.NET MVC 5 и веб-API 2
====================
По [Рик Андерсон](https://github.com/Rick-Anderson)

> ASP.NET MVC 5 и веб-API 2 перевести целый ряд новых функций, включая маршрутизацией атрибутов, фильтры проверки подлинности и многое другое. В разделе [https://www.asp.net/vnext](https://www.asp.net/core) для получения дополнительных сведений.
> 
> В этом пошаговом руководстве поможет вам с помощью действий, необходимых для обновления до последней версии приложения.  
> 
> > [!NOTE]
> > См. в разделе [ASP.NET и веб-инструменты Visual Studio 2013 заметки о выпуске](../../../visual-studio/overview/2013/release-notes.md) сведения о критических изменениях в MVC 4 и веб-API до следующей версии.
> 
>   
> 
> В этой статье было написано с Youngjune Гонконг и Рик Андерсон ( [ @RickAndMSFT ](https://twitter.com/#!/RickAndMSFT) )


## <a name="upgrade-steps"></a>Действия по обновлению

1. Создайте резервную копию проекта. В этом пошаговом руководстве потребуется внести изменения в файл проекта, конфигурацию пакета и файлов web.config.
2. Для обновления с веб-API для веб-API 2 в файле global.asax, измените:

    [!code-csharp[Main](how-to-upgrade-an-aspnet-mvc-4-and-web-api-project-to-aspnet-mvc-5-and-web-api-2/samples/sample1.cs)]

 в

    [!code-csharp[Main](how-to-upgrade-an-aspnet-mvc-4-and-web-api-project-to-aspnet-mvc-5-and-web-api-2/samples/sample2.cs)]
3. Убедитесь, что все пакеты, использующие проекты совместимы с MVC 5 и веб-API 2. Перечисленные ниже таблице показаны MVC 4 и веб-API, связанные с пакетами не должны быть изменены. Если у вас есть пакет, который зависит от одного из перечисленных ниже пакетов, обратитесь в службу издателей, чтобы получить более новых версий, совместимых с MVC 5 и веб-API 2. Если у вас есть исходный код для этих пакетов, необходимо повторно компилировать их с новыми сборками MVC 5 и веб-API 2.   

    | **Идентификатор пакета** | **Старая версия** | **Новая версия** |
    | --- | --- | --- |
    | Microsoft.AspNet.Razor | 2.0.x.x | 3.0.0 |
    | Microsoft.AspNet.WebPages | 2.0.x.x | 3.0.0 |
    | Microsoft.AspNet.WebPages.WebData | 2.0.x.x | 3.0.0 |
    | Microsoft.AspNet.WebPages.OAuth | 2.0.x.x | 3.0.0 |
    | Microsoft.AspNet.Mvc | программ 4.0.x.x | 5.0.0 |
    | Microsoft.AspNet.Mvc.Facebook | программ 4.0.x.x | 5.0.0 |
    | Microsoft.AspNet.WebApi.Core | программ 4.0.x.x | 5.0.0 |
    | Microsoft.AspNet.WebApi.SelfHost | программ 4.0.x.x | 5.0.0 |
    | Microsoft.AspNet.WebApi.Client | программ 4.0.x.x | 5.0.0 |
    | Microsoft.AspNet.WebApi.OData | программ 4.0.x.x | 5.0.0 |
    | Microsoft.AspNet.WebApi | программ 4.0.x.x | 5.0.0 |
    | Microsoft.AspNet.WebApi.WebHost | программ 4.0.x.x | 5.0.0 |
    | Microsoft.AspNet.WebApi.Tracing | программ 4.0.x.x | 5.0.0 |
    | Microsoft.AspNet.WebApi.HelpPage | программ 4.0.x.x | 5.0.0 |
    | Microsoft.Net.Http | 2.0.x. | 2.2.x. |
    | Microsoft.Data.OData | 5.2.x | 5.6.x |
    | System.Spatial | 5.2.x | 5.6.x |
    | Microsoft.Data.Edm | 5.2.x | 5.6.x |
    | Microsoft.AspNet.Mvc.FixedDisplayModes | < o: p >< / o: p > | Удаленная |
    | Microsoft.AspNet.WebPages.Administration | < o: p >< / o: p > | Удаленная |
    | Microsoft Web вспомогательных методов. | < o: p >< / o: p > | Microsoft.AspNet.WebHelpers |

    > [!NOTE]
    > Microsoft Web вспомогательные методы были заменены Microsoft.AspNet.WebHelpers. Сначала удалите старый пакет и установите его более новой.   
    >   
    > Совместимость не перекрестного версии среди основных пакетов ASP.NET не существует. Например MVC 5 совместим только с Razor 3 и не Razor 2.
4. Откройте проект в Visual Studio 2013.
5. Удалите следующие пакеты ASP.NET NuGet, которые установлены. С помощью консоли диспетчера пакетов (PMC) будет удалена. Чтобы открыть PMC, выберите **средства** меню и выберите **диспетчер библиотеки пакетов** выберите **консоль диспетчера пакетов**. Проект может включать не все из них.

    1. `Microsoft.AspNet.WebPages.Administration`  
 Как правило, этот пакет добавляется при обновлении MVC 3 до MVC 4. Чтобы удалить его, выполните следующую команду в PMC:  
        `Uninstall-Package -Id Microsoft.AspNet.WebPages.Administration`
    2. `Microsoft-Web-Helpers`   
 Этот пакет были бренд `Microsoft.AspNet.WebHelpers`. Чтобы удалить его, выполните следующую команду в PMC:  
        `Uninstall-Package -Id Microsoft-Web-Helpers`
    3. `Microsoft.AspNet.Mvc.FixedDisplayMode`  
 Этот пакет содержит обходной путь для ошибок в MVC 4, что ошибка исправлена в MVC 5. Чтобы удалить его, выполните следующую команду в PMC:  
        `Uninstall-Package -Id Microsoft.AspNet.Mvc.FixedDisplayModes`
6. Обновите все пакеты ASP.NET NuGet, с помощью PMC. В PMC выполните следующую команду:  
    `Update-Package`  
 `Update-Package` Команду без параметров будут обновлены все пакеты. Пакеты можно обновлять по отдельности с помощью аргумента идентификатора. Дополнительные сведения о команде обновления запустите `get-help update-package` .

## <a name="update-the-application-webconfig-file"></a>Обновление приложения *web.config* файла

Убедитесь, что для внесения этих изменений в приложении *web.config* файл, не *web.config* файла в *представления* папки.

Найдите `<runtime>/<assemblyBinding>` раздела и внесите следующие изменения:

1. В элементах с именем атрибута «System.Web.Mvc» измените номер версии «4.0.0.0» для «5.0.0.0». (Два изменения в этот элемент).
2. В элементах с именем атрибута &quot;System.Web.Helpers» и &quot;System.Web.WebPages&quot; изменить номер версии на «2.0.0.0» для «3.0.0.0». Четыре будут внесены изменения, два — в каждом из элементов.

    [!code-xml[Main](how-to-upgrade-an-aspnet-mvc-4-and-web-api-project-to-aspnet-mvc-5-and-web-api-2/samples/sample3.xml?highlight=6,10,14)]
3. Найдите `<appSettings>` статьи и обновить webpages:version из 2.0.0.0.0 для 3.0.0.0, как показано ниже:

    [!code-xml[Main](how-to-upgrade-an-aspnet-mvc-4-and-web-api-project-to-aspnet-mvc-5-and-web-api-2/samples/sample4.xml?highlight=2)]
4. Удалите все уровни доверия Кроме Full. Пример:

    [!code-xml[Main](how-to-upgrade-an-aspnet-mvc-4-and-web-api-project-to-aspnet-mvc-5-and-web-api-2/samples/sample5.xml?highlight=2)]

## <a name="update-the-webconfig-files-under-the-views-folder"></a>Обновление *web.config* файлы в папке «представления»

Если приложение использует области, будет необходимо также обновить каждый *web.config* файла в *представления* вложенная папка папки каждой области.

1. Обновите все элементы, содержащие «System.Web.Mvc» версии «4.0.0.0» версии «5.0.0.0».  

    [!code-xml[Main](how-to-upgrade-an-aspnet-mvc-4-and-web-api-project-to-aspnet-mvc-5-and-web-api-2/samples/sample6.xml?highlight=2)]

    [!code-xml[Main](how-to-upgrade-an-aspnet-mvc-4-and-web-api-project-to-aspnet-mvc-5-and-web-api-2/samples/sample7.xml?highlight=4-6,8)]
2. Обновите все элементы, содержащие «System.Web.WebPages.Razor» версии «2.0.0.0» версии «3.0.0.0». Если этот раздел содержит «System.Web.WebPages», обновите эти элементы из версии «2.0.0.0» версии «3.0.0.0»  

    [!code-xml[Main](how-to-upgrade-an-aspnet-mvc-4-and-web-api-project-to-aspnet-mvc-5-and-web-api-2/samples/sample8.xml?highlight=3-5)]
3. Если вы удалили `Microsoft-Web-Helpers` Установка пакета NuGet в предыдущем шаге, `Microsoft.AspNet.WebHelpers` с помощью следующей команды в PMC:  
    `Install-Package -Id Microsoft.AspNet.WebHelpers`
4. Если ваше приложение использует [User.IsInRole()](https://msdn.microsoft.com/en-us/library/system.web.security.roleprincipal.isinrole(v=vs.110).aspx) метод, добавьте следующий код в *Web.config* файла.

    [!code-xml[Main](how-to-upgrade-an-aspnet-mvc-4-and-web-api-project-to-aspnet-mvc-5-and-web-api-2/samples/sample9.xml)]

## <a name="final-steps"></a>Последние действия

Построение и тестирование приложения.

Удалите тип проекта MVC 4 GUID из файлов проекта.

1. В обозревателе решений щелкните правой кнопкой мыши имя проекта, а затем выберите **выгрузить проект**.
2. Щелкните правой кнопкой мыши проект и выберите команду Изменить имя_проекта.csproj.
3. Найдите `ProjectTypeGuids` элемент, а затем удалить MVC 4 проект GUID, `{E3E379DF-F4C6-4180-9B81-6769533ABE47}`.
4. Сохраните и закройте файл открыть проект.
5. Щелкните правой кнопкой мыши проект и выберите **перезагрузить проект**.
