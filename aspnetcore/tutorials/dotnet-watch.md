---
title: "Разработка приложений ASP.NET Core с использованием dotnet watch"
author: rick-anderson
description: "В этом учебнике показано, как установить и использовать наблюдатель за файлами .NET Core CLI (dotnet watch) в приложении ASP.NET Core."
ms.author: riande
manager: wpickett
ms.date: 10/05/2017
ms.topic: article
ms.technology: aspnet
ms.prod: asp.net-core
uid: tutorials/dotnet-watch
ms.openlocfilehash: cadd4a6a78c29e2213c39a02729b5c32a2b93ebd
ms.sourcegitcommit: 3e303620a125325bb9abd4b2d315c106fb8c47fd
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/19/2018
---
# <a name="developing-aspnet-core-apps-using-dotnet-watch"></a>Разработка приложений ASP.NET Core с использованием dotnet watch

Авторы: [Рик Андерсон](https://twitter.com/RickAndMSFT) (Rick Anderson) и [Виктор Хурдугачи](https://twitter.com/victorhurdugaci) (Victor Hurdugaci)

`dotnet watch` — это средство, которое запускает команду [.NET Core CLI](/dotnet/core/tools) при изменении исходных файлов. Например, в результате изменения файла может выполняться компиляция, тестирование или развертывание.

В этом руководстве используется уже существующее приложение веб-API с двумя конечными точками, одна из которых возвращает сумму, а другая — произведение. В методе product присутствует ошибка, которая будет исправлена в рамках этого руководства.

Скачайте [образец приложения](https://github.com/aspnet/Docs/tree/master/aspnetcore/tutorials/dotnet-watch/sample). Он содержит два проекта: *WebApp* (веб-API ASP.NET Core) и *WebAppTests* (модульные тесты для веб-API).

В командной оболочке перейдите в папку *WebApp* и запустите следующую команду:

```console
dotnet run
```

В выходных данных в консоли будут показаны аналогичные следующим сообщения, которые свидетельствуют о том, что приложение выполняется и ожидает запросов:

```console
$ dotnet run
Hosting environment: Development
Content root path: C:/Docs/aspnetcore/tutorials/dotnet-watch/sample/WebApp
Now listening on: http://localhost:5000
Application started. Press Ctrl+C to shut down.
```

В браузере перейдите на адрес `http://localhost:<port number>/api/math/sum?a=4&b=5`. Должен появиться результат `9`.

Перейдите к API продукта (`http://localhost:<port number>/api/math/product?a=4&b=5`). Он возвращает `9`, а не `20`, как ожидалось. Эта ошибка будет исправлена далее в этом руководстве.

## <a name="add-dotnet-watch-to-a-project"></a>Добавление `dotnet watch` в проект

1. Добавьте ссылку на пакет `Microsoft.DotNet.Watcher.Tools` в *CSPROJ*-файл:

    ```xml
    <ItemGroup>
        <DotNetCliToolReference Include="Microsoft.DotNet.Watcher.Tools" Version="2.0.0" />
    </ItemGroup> 
    ```

1. Установите пакет `Microsoft.DotNet.Watcher.Tools`, запустив следующую команду:
    
    ```console
    dotnet restore
    ```

## <a name="running-net-core-cli-commands-using-dotnet-watch"></a>Запуск команд .NET Core CLI с помощью `dotnet watch`

Любую [команду .NET Core CLI](/dotnet/core/tools#cli-commands) можно запустить с `dotnet watch`. Пример:

| Команда | Команда с контрольным значением |
| ---- | ----- |
| dotnet run | dotnet watch run |
| dotnet run -f netcoreapp2.0 | dotnet watch run -f netcoreapp2.0 |
| dotnet run -f netcoreapp2.0 -- --arg1 | dotnet watch run -f netcoreapp2.0 -- --arg1 |
| dotnet test | dotnet watch test |

Запустите `dotnet watch run` в папке *WebApp*. В выходных данных консоли будет указано, что запущен `watch`.

## <a name="making-changes-with-dotnet-watch"></a>Внесение изменений с использованием `dotnet watch`

Убедитесь, что `dotnet watch` выполняется.

Исправьте ошибку в методе `Product` для *MathController.cs* так, чтобы он возвращал произведение, а не сумму чисел.

```csharp
public static int Product(int a, int b)
{
  return a * b;
} 
```

Сохраните файл. В выходных данных консоли будет показано, что `dotnet watch` обнаружил изменение файла и перезапустил приложение.

Убедитесь, что `http://localhost:<port number>/api/math/product?a=4&b=5` возвращает правильный результат.

## <a name="running-tests-using-dotnet-watch"></a>Выполнение тестов с использованием `dotnet watch`

1. Снова измените метод `Product` для *MathController.cs* так, чтобы он возвращал сумму чисел, после чего сохраните файл.
1. В командной оболочке перейдите в папку *WebAppTests*.
1. Запустите `dotnet restore`.
1. Запустите `dotnet watch test`. В выходных данных будет указано, что проверка не пройдена и наблюдатель ожидает изменений в файле:

     ```console
     Total tests: 2. Passed: 1. Failed: 1. Skipped: 0.
     Test Run Failed.
     ```

1. Исправьте код метода `Product` так, чтобы он возвращал произведение. Сохраните файл.

`dotnet watch` обнаруживает изменения в файле и повторно запускает тесты. В выходных данных консоли будет указано, что проверка пройдена.

## <a name="dotnet-watch-in-github"></a>dotnet-watch на сайте GitHub

dotnet-watch входит в состав [репозитория DotNetTools](https://github.com/aspnet/DotNetTools/tree/dev/src/dotnet-watch) на сайте GitHub.

В [разделе MSBuild](https://github.com/aspnet/DotNetTools/tree/dev/src/dotnet-watch#msbuild) файла [ReadMe для dotnet-watch](https://github.com/aspnet/DotNetTools/blob/dev/src/dotnet-watch/README.md) поясняется, как настроить dotnet-watch из файла проекта MSBuild, для которого осуществляется контроль. Файл [ReadMe для dotnet-watch](https://github.com/aspnet/DotNetTools/blob/dev/src/dotnet-watch/README.md) содержит сведения о dotnet-watch, которые отсутствуют в этом руководстве.
