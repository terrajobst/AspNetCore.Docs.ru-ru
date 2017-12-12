---
uid: web-forms/overview/deployment/visual-studio-web-deployment/deploying-extra-files
title: "ASP.NET веб-развертывания с помощью Visual Studio: развертывание дополнительных файлов | Документы Microsoft"
author: tdykstra
description: "Этот учебник ряд показано развертывание ASP.NET (публикации) веб-приложения для веб-приложениях службы приложений Azure или стороннего поставщика услуг размещения, Пол..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 03/23/2015
ms.topic: article
ms.assetid: 1cd91055-84bc-42c6-9d80-646f41429d4d
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/deployment/visual-studio-web-deployment/deploying-extra-files
msc.type: authoredcontent
ms.openlocfilehash: a34607b25f6cf502f5fbf2fe51bf1937f470159e
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 11/10/2017
---
<a name="aspnet-web-deployment-using-visual-studio-deploying-extra-files"></a>ASP.NET веб-развертывания с помощью Visual Studio: развертывание дополнительных файлов
====================
По [Tom Dykstra](https://github.com/tdykstra)

[Загрузите начальный проект](http://go.microsoft.com/fwlink/p/?LinkId=282627)

> Этот учебник ряд показано развертывание ASP.NET (публикации) веб-приложения для веб-приложениях службы приложений Azure или стороннего поставщика услуг размещения, с помощью Visual Studio 2012 или Visual Studio 2010. Сведения о последовательности см. в разделе [в первом учебнике ряда](introduction.md).


## <a name="overview"></a>Обзор

Этот учебник показывает, как для расширения Visual Studio, веб-публикация конвейера для выполнения дополнительных задач во время развертывания. Задача заключается в копировании лишние файлы, которые не находятся в папке проекта веб-узел назначения.

В этом учебнике предстоит скопировать один дополнительный файл: *robots.txt*. Необходимо развернуть этот файл для промежуточного хранения, но не в рабочей среде. В [развертывание в рабочей среде](deploying-to-production.md) учебника, можно добавить этот файл в проект и настройки производства профиль, чтобы исключить его публикации. В этом учебнике вы увидите альтернативный метод для обработки этой ситуации, в который будут использоваться для всех файлов, которые требуется развернуть, но не требуется включать в проект.

## <a name="move-the-robotstxt-file"></a>Переместите файл robots.txt

Для подготовки к другой метод обработки *robots.txt*, в этом разделе учебника переместить файл в папку, не включенные в проект и удалении *robots.txt* из промежуточного хранения Среда. Это необходимо удалить файл из промежуточного хранения, чтобы мог проверить правильность работы нового метода развертывания файла для этой среды.

1. В **обозревателе решений**, щелкните правой кнопкой мыши *robots.txt* файла и нажмите кнопку **исключить из проекта**.
2. Используя проводник Windows, создайте новую папку в папке решения и назовите его *ExtraFiles*.
3. Переместить *robots.txt* файл из *ContosoUniversity* папку проекта для *ExtraFiles* папки.

    ![Папка ExtraFiles](deploying-extra-files/_static/image1.png)
4. Используя инструмент FTP, удалить *robots.txt* файл из промежуточного веб-сайта.

    В качестве альтернативы можно выбрать **удалить дополнительные файлы в месте назначения** под **параметры публикации файлов** на **параметры** вкладке профиля публикации для промежуточного хранения, и Повторная публикация для промежуточного хранения.

## <a name="update-the-publish-profile-file"></a>Обновить файл профиля публикации

Требуется только *robots.txt* в промежуточной, поэтому промежуточная только профиль публикации, необходимо обновить, чтобы развернуть его.

1. В Visual Studio откройте *Staging.pubxml*.
2. В конце файла перед закрывающей `</Project>` , добавьте следующую разметку:

    [!code-xml[Main](deploying-extra-files/samples/sample1.xml)]

    Этот код создает новый *целевой* , будет собирать дополнительные файлы для развертывания. Цель состоит из одного или несколько задач, MSBuild выполнит на основании указанных вами условий.

    `Include` Атрибут указывает, что папка, в которой для поиска файлов, *ExtraFiles*, расположенную на том же уровне, что и папка проекта. MSBuild будет собирать все файлы в этой папке и рекурсивно из вложенных (двойная звездочка указывает рекурсивные вложенные папки). Этот код можно поместить несколько файлов и файлов во вложенных папках внутри *ExtraFiles* папки и всех будет развертываться.

    `DestinationRelativePath` Элемент указывает, что файлы и папки должны копироваться в корневую папку веб-сайта назначения в ту же структуру файлов и папок найденные в *ExtraFiles* папки. Если нужно скопировать *ExtraFiles* саму папку `DestinationRelativePath` значение будет *ExtraFiles\%(RecursiveDir)%(Filename)%(Extension)*.
3. В конце файла перед закрывающей `</Project>` , добавьте следующую разметку, указывающее, когда следует выполнить новую цель.

    [!code-xml[Main](deploying-extra-files/samples/sample2.xml)]

    Этот код вызывает новый `CustomCollectFiles` целевой должно выполняться каждый раз, когда выполняется целевой объект, который копирует файлы в папку назначения. Существует отдельный целевой объект для публикации и создания пакета развертывания, а новый целевой встраивается в обе целевые объекты в случае, если вы решили развернуть с помощью пакета развертывания вместо публикации.

    *.Pubxml* файлов теперь выглядит как в следующем примере:

    [!code-xml[Main](deploying-extra-files/samples/sample3.xml?highlight=53-71)]
4. Сохраните и закройте *Staging.pubxml* файла.

## <a name="publish-to-staging"></a>Публикация для промежуточного хранения

С помощью одного щелчка мыши опубликовать или из командной строки, публикации приложения с помощью профиля промежуточного хранения.

При использовании одним щелчком публикации, можно проверить в **предварительного просмотра** окна, *robots.txt* будут скопированы. В противном случае использовать средством FTP и убедитесь, что *robots.txt* файл находится в корневой папке веб-сайта после развертывания.

## <a name="summary"></a>Сводка

На этом завершается, серию учебников по развертыванию веб-приложению ASP.NET для стороннего поставщика услуг размещения. Дополнительные сведения об этих подразделах, рассматриваемых в этих учебниках см. в разделе [Карта содержимого развертывания ASP.NET](https://go.microsoft.com/fwlink/p/?LinkId=282413).

## <a name="more-information"></a>Дополнительные сведения

Если вы знаете, как работать с файлами MSBuild, можно автоматизировать многие другие задачи развертывания путем написания кода в *.pubxml* файлов (для задачи, связанные с профилем) или проекта *. wpp.targets* файла (для задачи, применяется ко всем профилям). Дополнительные сведения о *.pubxml* и *. wpp.targets* файлов, в разделе [как: изменение параметров развертывания в файлы профилей публикации (.pubxml) и. wpp.targets файл в Visual Studio Web Проекты](https://msdn.microsoft.com/en-us/library/ff398069). Основные сведения о MSBuild кода, в разделе **составляющие файл проекта** в [корпоративного развертывания ряда: основные сведения о файле проекта](../web-deployment-in-the-enterprise/understanding-the-project-file.md). Чтобы научиться работать с файлами MSBuild для выполнения задач собственные сценарии, см. в данной книге: [внутри Microsoft Build Engine: с помощью MSBuild и Team Foundation Build](http://msbuildbook.com) Sayed Ibraham Hashimi и Уильям Bartholomew.

## <a name="acknowledgements"></a>Благодарности

Я хочу поблагодарить следующих людям, которые вносили свой вклад значительные к содержимому этого учебника ряда:

- [Alberto Poblacion, MVP &amp; MCT, Испания](https://mvp.microsoft.com/en-us/mvp/Alberto%20Poblacion%20Bolano-36772)
- Фергюсон Jarod, MVP разработки платформы данных, Соединенных Штатов
- Резкого Mittal, корпорация Майкрософт
- [Джон Гэллоуэй](https://weblogs.asp.net/jgalloway) (twitter: [ @jongalloway ](http://twitter.com/jongalloway))
- [Олсон Kristina, корпорация Майкрософт](https://blogs.iis.net/krolson/default.aspx)
- [Эти протоколы Майк, корпорация Майкрософт](http://www.mikepope.com/blog/DisplayBlog.aspx)
- Шривастав Mohit, корпорация Майкрософт
- [Raffaele Rialdi, Италия](http://www.iamraf.net/)
- [Рик Андерсон, корпорация Майкрософт](https://blogs.msdn.com/b/rickandy/)
- [Sayed Hashimi, корпорация Майкрософт](http://sedodream.com/default.aspx)(twitter: [ @sayedihashimi ](http://twitter.com/sayedihashimi))
- [Скотт Хансельман](http://www.hanselman.com/blog/) (twitter: [ @shanselman ](http://twitter.com/shanselman))
- [Скотт Хантер, корпорация Майкрософт](https://blogs.msdn.com/b/scothu/) (twitter: [ @coolcsh ](http://twitter.com/coolcsh))
- [Srđan Božović, Сербия](http://msforge.net/blogs/zmajcek/)
- [Вишал Joshi, корпорация Майкрософт](http://vishaljoshi.blogspot.com/) (twitter: [ @vishalrjoshi ](http://twitter.com/vishalrjoshi))

>[!div class="step-by-step"]
[Назад](command-line-deployment.md)
[Вперед](troubleshooting.md)
