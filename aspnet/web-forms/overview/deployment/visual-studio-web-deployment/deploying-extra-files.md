---
uid: web-forms/overview/deployment/visual-studio-web-deployment/deploying-extra-files
title: 'Веб-развертывание ASP.NET с помощью Visual Studio: развертывание Extra Files | Документация Майкрософт'
author: tdykstra
description: В этой серии руководств показано, как развернуть ASP.NET (публикации) веб-приложения, веб-приложениях службы приложений Azure или у стороннего поставщика размещения, Пол...
ms.author: aspnetcontent
manager: wpickett
ms.date: 03/23/2015
ms.topic: article
ms.assetid: 1cd91055-84bc-42c6-9d80-646f41429d4d
ms.technology: dotnet-webforms
msc.legacyurl: /web-forms/overview/deployment/visual-studio-web-deployment/deploying-extra-files
msc.type: authoredcontent
ms.openlocfilehash: 41bea625a53afbf7b39c03a2e8a92a03ce144289
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 07/03/2018
ms.locfileid: "37371824"
---
<a name="aspnet-web-deployment-using-visual-studio-deploying-extra-files"></a>Веб-развертывание ASP.NET с помощью Visual Studio: развертывание дополнительных файлов
====================
по [том Дайкстра](https://github.com/tdykstra)

[Загрузите начальный проект](http://go.microsoft.com/fwlink/p/?LinkId=282627)

> В этой серии руководств показано, как развернуть ASP.NET (публикации) веб-приложения для веб-приложениях службы приложений Azure или стороннего поставщика услуг размещения, с помощью Visual Studio 2012 или Visual Studio 2010. Сведения об этой серии см. в разделе [в первом учебнике серии](introduction.md).


## <a name="overview"></a>Обзор

Этом руководстве показано, как расширить возможности Visual Studio, веб-публикация конвейер, чтобы выполнить дополнительную задачу во время развертывания. Задача заключается в копировании лишние файлы, которые не находятся в папке проекта веб-узел назначения.

В этом руководстве вы скопируете один дополнительный файл: *robots.txt*. Необходимо развернуть этот файл в промежуточную среду, но не в рабочей среде. В [развертывание в рабочей среде](deploying-to-production.md) учебника, добавить этот файл в проект и настроить производства профиль, чтобы исключить его публикации. В этом руководстве вы увидите альтернативный метод для обработки этой ситуации, будут полезны для всех файлов, которые требуется развернуть, но не требуется включать в проекте.

## <a name="move-the-robotstxt-file"></a>Переместите файл robots.txt

Чтобы подготовиться к другой метод обработки *robots.txt*, в этом разделе руководства переместить файл в папку, не включенные в проект и удалении *robots.txt* из промежуточных Среда. Это необходимо для удаления файла из промежуточной, можно проверить, правильно ли работает новый метод развертывания файла в этой среде.

1. В **обозревателе решений**, щелкните правой кнопкой мыши *robots.txt* файл и нажмите кнопку **исключить из проекта**.
2. С помощью проводника Windows, создайте новую папку в папке решения и назовите его *ExtraFiles*.
3. Переместить *robots.txt* файла из *ContosoUniversity* папку проекта для *ExtraFiles* папки.

    ![Папка ExtraFiles](deploying-extra-files/_static/image1.png)
4. С помощью инструмента FTP, удалить *robots.txt* файл из промежуточного веб-сайта.

    Кроме того, можно выбрать **удалять дополнительные файлы в месте назначения** под **параметры публикации файлов** на **параметры** вкладке профиля публикации для промежуточного хранения, и повторно опубликуйте в промежуточную среду.

## <a name="update-the-publish-profile-file"></a>Обновить файл профиля публикации

Требуется только *robots.txt* в промежуточной среде, чтобы промежуточных только профиль публикации, необходимо обновить, чтобы развернуть его.

1. В Visual Studio откройте *Staging.pubxml*.
2. В конце файла, перед закрывающей скобкой `</Project>` , добавьте следующую разметку:

    [!code-xml[Main](deploying-extra-files/samples/sample1.xml)]

    Этот код создает новый *целевой* , будет собирать дополнительные файлы для развертывания. Целевой объект состоит из одного или несколько задач, которые будут выполнять MSBuild в зависимости от указанных вами условий.

    `Include` Атрибут указывает, что папка, в которой для поиска файлов *ExtraFiles*, который находится на том же уровне, что и папка проекта. MSBuild будет собирать все файлы в этой папке и рекурсивно из вложенных (двойные звездочки указывает рекурсивных вложенных папок). Этот код можно поместить несколько файлов и файлов во вложенных папках внутри *ExtraFiles* папку и все будет развертываться.

    `DestinationRelativePath` Элемент указывает, что файлы и папки должны быть скопированы в корневую папку веб-узла назначения, в ту же структуру файлов и папок, так как они находятся в *ExtraFiles* папки. Если вы хотели скопировать *ExtraFiles* саму папку `DestinationRelativePath` малейший *ExtraFiles\%(RecursiveDir)%(Filename)%(Extension)*.
3. В конце файла, перед закрывающей скобкой `</Project>` , добавьте следующую разметку, указывающее, когда следует выполнить новый целевой объект.

    [!code-xml[Main](deploying-extra-files/samples/sample2.xml)]

    Этот код вызывает новый `CustomCollectFiles` целевой объект для выполнения всякий раз, когда выполняется целевой объект, который копирует файлы в папке назначения. Существует отдельный целевой для публикации и создания пакета развертывания, и новый целевой объект внедряется в оба целевых объекта, в случае, если вы решили развернуть с помощью пакета развертывания вместо публикации.

    *.Pubxml* файл теперь выглядит как в следующем примере:

    [!code-xml[Main](deploying-extra-files/samples/sample3.xml?highlight=53-71)]
4. Сохраните и закройте *Staging.pubxml* файла.

## <a name="publish-to-staging"></a>Публикации для промежуточного развертывания

Одним щелчком для публикации или из командной строки, публикации приложения с помощью профиля промежуточного хранения.

При использовании быстрой публикации, вы можете проверить в **предварительной версии** окна, *robots.txt* будут скопированы. В противном случае используйте средства FTP, чтобы убедиться, что *robots.txt* файл находится в корневой папке веб-сайта после развертывания.

## <a name="summary"></a>Сводка

На этом завершается этой серии руководств по развертыванию веб-приложению ASP.NET для стороннего поставщика услуг размещения. Дополнительные сведения о всех подразделах, рассматриваемых в этих учебниках, см. в разделе [Карта содержимого развертывания ASP.NET](https://go.microsoft.com/fwlink/p/?LinkId=282413).

## <a name="more-information"></a>Дополнительные сведения

Если вы знаете, как работать с файлами MSBuild, много других задач развертывания можно автоматизировать путем написания кода в *.pubxml* файлов (в целях профиле соответствующего) или проект *. wpp.targets* файл (для задачи, применяется для всех профилей). Дополнительные сведения о *.pubxml* и *. wpp.targets* файлы, см. в разделе [как: изменение параметров развертывания в файлах профиля публикации (.pubxml) и. wpp.targets файл в Visual Studio Web Проекты](https://msdn.microsoft.com/library/ff398069). Основные сведения о MSBuild кода, см. в разделе **Анатомия файл проекта** в [Корпоративное развертывание, часть: основные сведения о файле проекта](../web-deployment-in-the-enterprise/understanding-the-project-file.md). Чтобы научиться работать с файлами MSBuild для выполнения задач для собственных сценариев, см. в этой книге: [Inside the Microsoft Build Engine: использование MSBuild и Team Foundation Build](http://msbuildbook.com) Саид Хашими Ibraham и William Bartholomew.

## <a name="acknowledgements"></a>Благодарности

Я бы хотел поблагодарить следующих специалистов, которые разработали вклад в содержимое этой серии руководств:

- [Альберто Poblacion, обладатель звания MVP &amp; MCT, Испания](https://mvp.microsoft.com/mvp/Alberto%20Poblacion%20Bolano-36772)
- Фергюсон Jarod, MVP разработки платформы данных, США
- Harsh Миттал, Microsoft
- [Джон Гэллоуэй](https://weblogs.asp.net/jgalloway) (twitter: [ @jongalloway ](http://twitter.com/jongalloway))
- [Олсон Kristina, Microsoft](https://blogs.iis.net/krolson/default.aspx)
- [Майк эти протоколы, Microsoft](http://www.mikepope.com/blog/DisplayBlog.aspx)
- Мохи Srivastava, Microsoft
- [Raffaele Rialdi, Италия](http://www.iamraf.net/)
- [Рик Андерсон, Microsoft](https://blogs.msdn.com/b/rickandy/)
- [Саид Хашими, Microsoft](http://sedodream.com/default.aspx)(twitter: [ @sayedihashimi ](http://twitter.com/sayedihashimi))
- [Скотт Хансельман](http://www.hanselman.com/blog/) (twitter: [ @shanselman ](http://twitter.com/shanselman))
- [Скотт Хантер, Microsoft](https://blogs.msdn.com/b/scothu/) (twitter: [ @coolcsh ](http://twitter.com/coolcsh))
- [Srđan Božović, Сербия](http://msforge.net/blogs/zmajcek/)
- [Vishal Joshi, Microsoft](http://vishaljoshi.blogspot.com/) (twitter: [ @vishalrjoshi ](http://twitter.com/vishalrjoshi))

> [!div class="step-by-step"]
> [Назад](command-line-deployment.md)
> [Вперед](troubleshooting.md)
