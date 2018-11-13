---
title: Управление пакетами на стороне клиента с помощью Bower в ASP.NET Core
author: rick-anderson
description: Управление пакетами на стороне клиента с помощью Bower.
ms.author: riande
ms.custom: H1Hack27Feb2017
ms.date: 08/09/2018
uid: client-side/bower
ms.openlocfilehash: 06edf7ee791aac0984ff71c2f243f61093f0d503
ms.sourcegitcommit: 408921a932448f66cb46fd53c307a864f5323fe5
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 11/12/2018
ms.locfileid: "51570026"
---
# <a name="manage-client-side-packages-with-bower-in-aspnet-core"></a>Управление пакетами на стороне клиента с помощью Bower в ASP.NET Core

По [Рик Андерсон](https://twitter.com/RickAndMSFT), [Райс Ноэл](https://blog.falafel.com/falafel-software-recognized-sitefinity-website-year/), и [Scott Addie](https://scottaddie.com)

> [!IMPORTANT]
> Сохранение Bower, его издатели рекомендуем использовать другое решение. [Диспетчер библиотек](https://blogs.msdn.microsoft.com/webdev/2018/04/18/what-happened-to-bower/) (LibMan для краткости) — средство приобретения новых клиентская библиотека для Visual Studio (Visual Studio требуется выделить 15,8 или более поздней версии). Дополнительные сведения см. в разделе <xref:client-side/libman/index>. Bower поддерживается в Visual Studio с помощью версии 15.5.
>
> Yarn с Webpack является альтернативой популярных, для которого [инструкции по миграции](https://bower.io/blog/2017/how-to-migrate-away-from-bower/) доступны.

[Bower](https://bower.io/) вызывает саму себя «Диспетчер пакетов для веб-». В экосистеме .NET он заполняет этот пробел влево на невозможность NuGet для доставки статического содержимого файлов. Для проектов ASP.NET Core, статическими файлами принадлежат клиентские библиотеки, такие как [jQuery](http://jquery.com/) и [Bootstrap](http://getbootstrap.com/). Для библиотеки .NET, вы по-прежнему использовать [NuGet](https://www.nuget.org/) диспетчера пакетов.

Процесс сборки новые проекты, созданные с помощью шаблонов проекта ASP.NET Core, Настройка на стороне клиента. [jQuery](http://jquery.com/) и [Bootstrap](http://getbootstrap.com/) установлены, и поддерживается Bower.

Клиентские пакеты отображаются в *bower.json* файла. Шаблоны проектов ASP.NET Core настраивает *bower.json* с jQuery, Bootstrap и проверки jQuery.

В этом руководстве мы добавим поддержку [Font Awesome](http://fontawesome.io). Можно установить пакеты bower с **управление пакетами Bower** пользовательского интерфейса или вручную в *bower.json* файла.

### <a name="installation-via-manage-bower-packages-ui"></a>Установка с помощью управление пакетами Bower пользовательского интерфейса

* Создание нового приложения ASP.NET Core Web с **веб-приложение ASP.NET Core (.NET Core)** шаблона. Выберите **веб-приложение** и **без проверки подлинности**.

* Щелкните правой кнопкой мыши проект в обозревателе решений и выберите **управление пакетами Bower** (также в главном меню **проекта** > **управление пакетами Bower**).

* В **Bower: \<имя_проекта\>**  окно, перейдите на вкладку «Обзор» и отфильтруйте список пакетов, введя `font-awesome` в поле поиска:

  ![Управление пакетами bower](bower/_static/manage-bower-packages.png)

* Убедитесь, что «сохранить изменения в *bower.json*"будет установлен флажок. Выберите версию в раскрывающемся списке и нажмите кнопку **установить** кнопки. **Вывода** окне отображаются сведения об установке.

### <a name="manual-installation-in-bowerjson"></a>Установка вручную в bower.json

Откройте *bower.json* и добавьте зависимости «font awesome». IntelliSense отображает доступные пакеты. При выборе пакета отображаются доступные версии. Приведенные ниже изображения являются более старыми и не будет соответствовать результату.

![IntelliSense из обозреватель пакетов bower](bower/_static/add-package.png)

![версия bower IntelliSense](bower/_static/version-intelliSense.png)

Bower использует [семантического управления версиями](http://semver.org/) для упорядочения зависимостей. Семантическое управление версиями, также известный как SemVer, определяет пакеты с формирование \<основных >.\< дополнительный номер >. \<patch >. IntelliSense упрощает семантического управления версиями, отображая только несколько распространенных вариантов действий. Верхний элемент в списке IntelliSense (4.6.3 в приведенном выше примере) считается последняя стабильная версия пакета. Символ крышки (^) соответствует самой последней основной версии и тильда (~) соответствует самой последней дополнительный номер версии.

Сохранить *bower.json* файла. Visual Studio отслеживает *bower.json* файл для изменения. При сохранении, *bower install* выполняется команда. См. в окне вывода **Bower и npm** представление для точного выполняемой команды.

Откройте *.bowerrc* файл *bower.json*. `directory` Свойству *wwwroot/lib* , указывающее расположение Bower установит пакет средств.

```json
{
 "directory": "wwwroot/lib"
}
```

Поле поиска в обозревателе решений можно использовать для поиска и отображения font awesome пакета.

Откройте *Views\Shared\_Layout.cshtml* и добавьте font awesome CSS-файл в среде [вспомогательная функция тега](xref:mvc/views/tag-helpers/intro) для `Development`. Из обозревателя решений перетащите *шрифта awesome.css* внутри `<environment names="Development">` элемент.

[!code-html[](bower/sample/_Layout.cshtml?highlight=4&range=9-13)]

В рабочем приложении необходимо добавить *шрифта awesome.min.css* чтобы вспомогательная функция тега среды для `Staging,Production`.

Замените содержимое файла *Views\Home\About.cshtml* файл Razor, используя следующую разметку:

[!code-html[](bower/sample/About.cshtml)]

Запустите приложение и перейдите к представлению About, чтобы убедитесь, что работает font awesome пакета.

## <a name="exploring-the-client-side-build-process"></a>Изучение процесса сборки на стороне клиента

Большинство шаблоны проектов ASP.NET Core уже настроены на использование Bower. Этот следующего пошагового руководства начинается с пустого проекта ASP.NET Core и добавляет каждый фрагмент вручную, поэтому вы можете почувствовать, как Bower используется в проекте. Вы увидите, что происходит с структуре проекта и выходные данные каждого изменения конфигурации среды выполнения.

Приведены общие шаги, используемые процесса сборки на стороне клиента с помощью Bower.

* Определение пакетов, используемых в проекте. <!-- once defined, you don't need to download them, VS does -->
* Справочник по пакетов из веб-страниц.

### <a name="define-packages"></a>Определение пакетов

После перечисления пакетов в *bower.json* файла, Visual Studio будут загружать их. В следующем примере используется Bower для загрузки, jQuery и начальной загрузки для *wwwroot* папки.

* Создание нового приложения ASP.NET Core Web с **веб-приложение ASP.NET Core (.NET Core)** шаблона. Выберите **пустой** шаблон проекта и нажмите кнопку **ОК**.

* В обозревателе решений щелкните правой кнопкой мыши проект > **Добавление нового элемента** и выберите **файл конфигурации Bower**. Примечание: Объект *.bowerrc* добавляется еще и файл.

* Откройте *bower.json*и добавьте jquery и начальной загрузки для `dependencies` раздел. Полученный в результате *bower.json* файла будет выглядеть как в следующем примере. Версии будут меняться со временем и могут не соответствовать на следующем рисунке.

[!code-json[](bower/sample/bower.json?highlight=5,6)]

* Сохранить *bower.json* файла.

  Проверить проект включает *начальной загрузки* и *jQuery* каталогов в *wwwroot/lib*. Bower использует *.bowerrc* файл, чтобы установить для активов *wwwroot/lib*.

  Примечание: «Управление пакетами Bower» пользовательский Интерфейс представляет собой альтернативу редактирование файлов вручную.

### <a name="enable-static-files"></a>Включение статических файлов

* Добавление `Microsoft.AspNetCore.StaticFiles` пакет NuGet в проект.
* Включить статические файлы обслуживаются с [по промежуточного слоя статических файлов](/dotnet/api/microsoft.aspnetcore.builder.staticfileextensions). Добавьте вызов [UseStaticFiles](/dotnet/api/microsoft.aspnetcore.builder.staticfileextensions) для `Configure` метод `Startup`.

[!code-csharp[](bower/sample/Startup.cs?highlight=9)]

### <a name="reference-packages"></a>Справочник по пакетов

В этом разделе вы создадите страницу HTML, чтобы убедиться, что он может обращаться к развернутых пакетов.

* Добавьте новую страницу HTML с именем *Index.html* для *wwwroot* папки. Примечание: Необходимо добавить HTML-файл для *wwwroot* папки. По умолчанию невозможно обслужить статическое содержимое за пределами *wwwroot*. См. в разделе [статические файлы](xref:fundamentals/static-files) Дополнительные сведения.

  Замените содержимое файла *Index.html* следующей разметкой:

[!code-html[](bower/sample/Index.html)]

* Запустите приложение и перейдите к `http://localhost:<port>/Index.html`. Кроме того, с помощью *Index.html* открыт, нажмите клавишу `Ctrl+Shift+W`. Убедитесь, что применяется в стилях jumbotron, кодом jQuery реагирует при нажатии кнопки и что начальной загрузки кнопки изменяет состояние.

  ![jumbotron стиль.](bower/_static/jumbotron.png)
