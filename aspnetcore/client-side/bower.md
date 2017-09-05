---
title: "С помощью Bower в ASP.NET Core"
author: rick-anderson
description: "Управление пакетами стороне клиента с помощью Bower."
keywords: ASP.NET Core, bower
ms.author: riande
manager: wpickett
ms.date: 02/14/2017
ms.topic: article
ms.assetid: df7c43da-280e-4df6-86cb-eecec8f12bfc
ms.technology: aspnet
ms.prod: asp.net-core
uid: client-side/bower
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 409f7afba8dd7d03b7b9aa27d93ec9167252b965
ms.sourcegitcommit: 4e84d8bf5f404bb77f3d41665cf7e7374fc39142
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 09/05/2017
---
# <a name="manage-client-side-packages-with-bower-in-aspnet-core"></a>Управление пакетами стороне клиента с помощью Bower в ASP.NET Core

По [Рик Андерсон](https://twitter.com/RickAndMSFT), [Риса Ноэл](http://blog.falafel.com/author/noel-rice/), и [Скотт Addie](https://scottaddie.com) 

[Bower](https://bower.io/) вызывает саму себя «Диспетчер пакетов для Интернета.» В экосистеме .NET он заполняет void влево на невозможность NuGet доставки статического содержимого файлов. Для проектов ASP.NET Core эти статические файлы, присущие клиентские библиотеки, такие как [jQuery](http://jquery.com/) и [начальной загрузки](http://getbootstrap.com/). Для библиотеки .NET, по-прежнему использовать [NuGet](https://nuget.org/) диспетчера пакетов.

Процесс создания новых проектов, созданных с помощью шаблонов проектов ASP.NET Core, Настройка на стороне клиента. [jQuery](http://jquery.com/) и [начальной загрузки](http://getbootstrap.com/) установлены, и поддерживается Bower.

Клиентские пакеты отображаются в *bower.json* файла. Шаблоны проектов ASP.NET Core настраивает *bower.json* с jQuery, jQuery проверки и начальной загрузки.

В этом учебнике мы добавили поддержку для [шрифта здорово](http://fontawesome.io). Можно установить пакеты bower с **управление пакетами Bower** пользовательского интерфейса или вручную в *bower.json* файла.

### <a name="installation-via-manage-bower-packages-ui"></a>Установка с помощью Bower управление пакеты пользовательского интерфейса

* Создание нового приложения ASP.NET Core Web с **веб-приложения ASP.NET Core (.NET Core)** шаблона. Выберите **веб-приложение** и **без проверки подлинности**.

* Щелкните правой кнопкой мыши проект в обозревателе решений и выберите **управление пакетами Bower** (также в главном меню **проекта** > **управление пакетами Bower**).

* В **Bower: \<имя проекта\>**  окно, перейдите на вкладку «Просмотр» и отфильтруйте список пакетов, введя `font-awesome` в поле поиска:

 ![Управление пакетами bower](bower/_static/manage-bower-packages.png)

* Убедитесь, что «сохранить изменения в *bower.json*» установлен флажок. Выберите версию в раскрывающемся списке и нажмите кнопку **установить** кнопки. **Выходной** в окне отображаются сведения об установке.

### <a name="manual-installation-in-bowerjson"></a>Установка вручную в bower.json

Откройте *bower.json* и добавьте «шрифта здорово» зависимостям. IntelliSense отображает доступные пакеты. При выборе пакета отображаются доступные версии. Изображения ниже являются более старыми и не будет соответствовать вы видите.

![IntelliSense bower пакета обозревателя](bower/_static/add-package.png)

![версия bower IntelliSense](bower/_static/version-intelliSense.png)

Bower использует [семантического управления версиями](http://semver.org/) организовать зависимостей. Семантического управления версиями, также известный как SemVer определяет пакеты с формирование \<основной >.\< дополнительный номер >. \<исправление >. IntelliSense упрощает семантического управления версиями, отображая только несколько распространенных вариантов. Верхний элемент в списке IntelliSense (4.6.3 в приведенном выше примере) считается последняя стабильная версия пакета. Знак вставки (^) соответствует самой последней основной номер версии, а тильда (~) соответствует последнему дополнительному номеру версии.

Сохранить *bower.json* файла. Visual Studio отслеживает *bower.json* изменения в файле. При сохранении, *bower установки* выполняется команда. См. в окне вывода **Bower или npm** представление для точного выполняемой команды.

Откройте *.bowerrc* файл *bower.json*. `directory` Свойству *wwwroot/lib* указывающая расположение Bower установит пакет средств.

```json
{
 "directory": "wwwroot/lib"
}
```

Поле поиска в обозревателе решений можно использовать для поиска и отображения шрифта здорово пакета.

Откройте *одну\_Layout.cshtml* и добавьте здорово шрифта CSS-файл в среде [вспомогательный тег](xref:mvc/views/tag-helpers/intro) для `Development`. В обозревателе решений, перетаскивание *шрифта awesome.css* внутри `<environment names="Development">` элемента.

[!code-html[Main](bower/sample/_Layout.cshtml?highlight=4&range=9-13)]

В рабочем приложении необходимо добавить *шрифта awesome.min.css* помощникам тега среды для `Staging,Production`.

Замените содержимое *Views\Home\About.cshtml* файл Razor с следующую разметку:

[!code-html[Main](bower/sample/About.cshtml)]

Запустите приложение и перейдите в представление о программе порядок проверки работы пакета здорово шрифта.

## <a name="exploring-the-client-side-build-process"></a>Обзор процесса сборки на стороне клиента

Большинство шаблонов проектов ASP.NET Core уже настроены для использования Bower. Это Далее Пошаговое руководство начинается с пустой проект ASP.NET Core и добавляет каждый фрагмент вручную, то можно понять, как Bower используется в проекте. Вы видите, можно, что происходит в структуре проекта и выполняется вывод каждого изменения конфигурации среды выполнения.

Указаны общие шаги для использования процесса построения на стороне клиента с помощью Bower.

* Определите пакеты, используемой в проекте. <!-- once defined, you don't need to download them, VS does -->
* Справочник по пакетов из веб-страниц.

### <a name="define-packages"></a>Определение пакетов

После списка пакетов в *bower.json* их загружается файл, Visual Studio. В следующем примере используется Bower для загрузки jQuery и начальной загрузки для *wwwroot* папки.

* Создание нового приложения ASP.NET Core Web с **веб-приложения ASP.NET Core (.NET Core)** шаблона. Выберите **пустой** шаблон проекта и нажмите кнопку **ОК**.

* В обозревателе решений щелкните правой кнопкой мыши проект > **Добавление нового элемента** и выберите **файла конфигурации для Bower**. Примечание: A *.bowerrc* также будет добавлен файл.

* Откройте *bower.json*, добавление jquery и начальной загрузки для `dependencies` раздела. Итоговый *bower.json* файла будет выглядеть как в следующем примере. Версии будет меняться со временем и может не совпадать на рисунке ниже.

[!code-json[Main](bower/sample/bower.json?highlight=5,6)]

* Сохранить *bower.json* файла.

 Проверить проект включает *начальной загрузки* и *jQuery* каталогов в *wwwroot/lib*. Bower использует *.bowerrc* файл для установки средств в *wwwroot/lib*.

 Примечание: «Управление пакетами Bower» пользовательского интерфейса является альтернативой редактирование файлов вручную.

### <a name="enable-static-files"></a>Включение статических файлов

* Добавить `Microsoft.AspNetCore.StaticFiles` пакет NuGet для проекта.
* Включение статических файлов предоставляется с [по промежуточного слоя статических файлов](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.builder.staticfileextensions). Добавьте вызов [UseStaticFiles](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.builder.staticfileextensions) для `Configure` метод `Startup`.

[!code-csharp[Main](bower/sample/Startup.cs?highlight=9)]

### <a name="reference-packages"></a>Справочник по пакетам

В этом разделе вы создадите на HTML-странице, чтобы убедиться, что он может обращаться к развернутых пакетов.

* Добавьте новую HTML-страницу с именем *Index.html* для *wwwroot* папки. Примечание: Необходимо добавить HTML-файла для *wwwroot* папки. По умолчанию статического содержимого не может быть выдана за пределами *wwwroot*. В разделе [работа с файлами статического](xref:fundamentals/static-files) для получения дополнительной информации.

 Замените содержимое *Index.html* следующей разметкой:

[!code-html[Main](bower/sample/Index.html)]

* Запустите приложение и перейдите к `http://localhost:<port>/Index.html`. Кроме того, с *Index.html* открыт, нажмите клавишу `Ctrl+Shift+W`. Проверка применения стилей jumbotron, код jQuery реагирует при нажатии кнопки и что начальной загрузки кнопка меняет состояние.

 ![Стиль jumbotron](bower/_static/jumbotron.png)
