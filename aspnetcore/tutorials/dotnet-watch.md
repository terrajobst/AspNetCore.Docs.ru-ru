---
title: "Разработка приложений ASP.NET Core с использованием dotnet watch"
author: rick-anderson
description: "Показывает способы использования dotnet watch."
keywords: "ASP.NET Core, использование dotnet watch"
ms.author: riande
manager: wpickett
ms.date: 03/09/2017
ms.topic: article
ms.assetid: 563ffb3f-d369-4aa5-bf0a-7300b4e7832c
ms.technology: aspnet
ms.prod: asp.net-core
uid: tutorials/dotnet-watch
ms.openlocfilehash: 30e0d07bdfbd16a475e03c1a21cdd10220bd1630
ms.sourcegitcommit: 0b6c8e6d81d2b3c161cd375036eecbace46a9707
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 08/11/2017
---
# <a name="developing-aspnet-core-apps-using-dotnet-watch"></a>Разработка приложений ASP.NET Core с использованием dotnet watch


Авторы: [Рик Андерсон](https://twitter.com/RickAndMSFT) (Rick Anderson) и [Виктор Хурдугачи](https://twitter.com/victorhurdugaci) (Victor Hurdugaci)

Средство `dotnet watch` выполняет команду `dotnet` при изменении файлов исходного кода. Например, в результате изменения файла может выполняться компиляция, тестирование или развертывание.

В этом руководстве мы используем существующее приложение веб-API с двумя контрольными точками, одна из которых возвращает сумму, а другая произведение. В методе product присутствует ошибка, которая будет исправлена в рамках этого руководства.

Скачайте [образец приложения](https://github.com/aspnet/Docs/tree/master/aspnetcore/tutorials/dotnet-watch/sample). В нем содержатся два проекта: `WebApp` (веб-приложение) и `WebAppTests` (модульные тесты для веб-приложения).

В консоли перейдите в папку WebApp и выполните следующие команды:

- `dotnet restore`
- `dotnet run`

В выходных данных в консоли будут показаны аналогичные следующим сообщения, которые свидетельствуют о том, что приложение выполняется и ожидает запросов:

```console
$ dotnet run
Hosting environment: Production
Content root path: C:/Docs/aspnetcore/tutorials/dotnet-watch/sample/WebApp
Now listening on: http://localhost:5000
Application started. Press Ctrl+C to shut down.
```

В веб-браузере перейдите по адресу `http://localhost:5000/api/math/sum?a=4&b=5`. Должен появиться результат `9`.

Перейдите к API product (`http://localhost:5000/api/math/product?a=4&b=5`), который возвращает значение `9`, а не ожидаемое `20`. Эта ошибка будет исправлена далее в этом руководстве.

## <a name="add-dotnet-watch-to-a-project"></a>Добавление `dotnet watch` в проект

- Добавьте `Microsoft.DotNet.Watcher.Tools` в файл *.csproj*:
 ```xml
 <ItemGroup>
   <DotNetCliToolReference Include="Microsoft.DotNet.Watcher.Tools" Version="1.0.0" />
 </ItemGroup> 
 ```

- Запустите `dotnet restore`.

## <a name="running-dotnet-commands-using-dotnet-watch"></a>Выполнение команд `dotnet` с помощью `dotnet watch`

Любые команды `dotnet` можно запускать с помощью `dotnet watch`, например:

| Команда | Команда с контрольным значением |
| ---- | ----- |
| dotnet run | dotnet watch run |
| dotnet run -f net451 | dotnet watch run -f net451 |
| dotnet run -f net451 -- --arg1 | dotnet watch run -f net451 -- --arg1 |
| dotnet test | dotnet watch test |

Выполните `dotnet watch run` в папке `WebApp`. В выходных данных консоли будет указано, что выполнен запуск `watch`.

## <a name="making-changes-with-dotnet-watch"></a>Внесение изменений с использованием `dotnet watch`

Убедитесь, что `dotnet watch` выполняется.

Исправьте ошибку в методе `Product` для `MathController` так, чтобы он возвращал произведение, а не сумму чисел.

```csharp
public static int Product(int a, int b)
{
  return a * b;
} 
```

Сохраните файл. В выходных данных консоли будут показаны сообщения, указывающие, что `dotnet watch` обнаружил изменение файла и перезапустил приложение.

Убедитесь, что `http://localhost:5000/api/math/product?a=4&b=5` возвращает правильный результат.

## <a name="running-tests-using-dotnet-watch"></a>Выполнение тестов с использованием `dotnet watch`

- Снова измените метод `Product` для `MathController` так, чтобы он возвращал сумму чисел, после чего сохраните файл.
- В окне командной строки перейдите в папку `WebAppTests`.
- Запуск `dotnet restore`
- Запустите `dotnet watch test`. В выходных данных появятся сообщения о том, что проверка завершилась неудачно и модуль наблюдения ожидает изменений в файле:

 ```console
 Total tests: 2. Passed: 1. Failed: 1. Skipped: 0.
 Test Run Failed.
  ```
- Исправьте код метода `Product` так, чтобы он возвращал произведение. Сохраните файл.

`dotnet watch` обнаруживает изменения в файле и повторно запускает тесты. В выходных данных консоли появляются сообщения об успешном прохождении тестов.

## <a name="dotnet-watch-in-github"></a>dotnet-watch на сайте GitHub

dotnet-watch входит в состав [репозитория DotNetTools](https://github.com/aspnet/DotNetTools/tree/dev/src/Microsoft.DotNet.Watcher.Tools) на сайте GitHub.

В [разделе MSBuild](https://github.com/aspnet/DotNetTools/blob/dev/src/Microsoft.DotNet.Watcher.Tools/README.md#msbuild) файла [ReadMe для dotnet-watch](https://github.com/aspnet/DotNetTools/blob/dev/src/Microsoft.DotNet.Watcher.Tools/README.md) поясняется, как настроить dotnet-watch из файла проекта MSBuild, для которого осуществляется контроль. Файл [ReadMe для dotnet-watch](https://github.com/aspnet/DotNetTools/blob/dev/src/Microsoft.DotNet.Watcher.Tools/README.md) содержит сведения о dotnet-watch, которые отсутствуют в этом руководстве.
