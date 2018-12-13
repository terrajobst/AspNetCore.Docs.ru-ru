---
title: Начало работы с ASP.NET Core
author: rick-anderson
description: Краткий учебник, в котором с помощью ASP.NET Core создается и запускается простое приложение Hello World.
ms.author: riande
ms.custom: mvc
ms.date: 12/11/2018
uid: getting-started
ms.openlocfilehash: cf9e731f7638687b3f40b42864ef7ee8f5522b39
ms.sourcegitcommit: b34b25da2ab68e6495b2460ff570468f16a9bf0d
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 12/12/2018
ms.locfileid: "53284361"
---
# <a name="tutorial-get-started-with-aspnet-core"></a>Учебник. Начало работы с ASP.NET Core

В этом руководстве показано, как использовать интерфейс командной строки .NET Core для создания веб-приложения ASP.NET Core.

Вы научитесь:

> [!div class="checklist"]
> * создавать проект веб-приложения;
> * включать локальный HTTPS;
> * Запустите приложение.
> * редактировать страницу Razor.

В итоге вы получите рабочее веб-приложение на локальном компьютере.

![Домашняя страница веб-приложения](_static/home-page.png)

## <a name="prerequisites"></a>Предварительные требования

* [Пакет SDK для .NET Core 2.2](https://www.microsoft.com/net/download/all)

## <a name="create-a-web-app-project"></a>Создание проекта веб-приложения

Откройте окно командной оболочки и введите следующую команду:

```console
dotnet new webapp -o aspnetcoreapp
```

## <a name="enable-local-https"></a>Включение локального HTTPS

Установите доверие к сертификату разработки HTTPS.

# <a name="windowstabwindows"></a>[Windows](#tab/windows)

```console
dotnet dev-certs https --trust
```

Приведенная выше команда отображает следующее диалоговое окно.

![Диалоговое окно "Предупреждение о безопасности"](_static/cert.png)

Выберите **Да**, если согласны доверять сертификату разработки.

# <a name="macostabmacos"></a>[macOS](#tab/macos)

```console
dotnet dev-certs https --trust
```

Приведенная выше команда отображает следующее сообщение.

*Запрошено доверие к сертификату разработки HTTPS. Если сертификат не является доверенным, выполните следующую команду:* `'sudo security add-trusted-cert -d -r trustRoot -k /Library/Keychains/System.keychain <<certificate>>'`.  
* Эта команда может запросить пароль для установки сертификата в системной цепочке ключей.

Пароль: *

Введите пароль, если согласны доверять сертификату разработки.

# <a name="linuxtablinux"></a>[Linux](#tab/linux)

Просмотрите документацию по дистрибутиву Linux, чтобы узнать, как установить отношение доверия к сертификату разработки HTTPS.

---

## <a name="run-the-app"></a>Запуск приложения

Выполните следующие команды:

```console
cd aspnetcoreapp
dotnet run
```

Перейдите по адресу [https://localhost:5001](https://localhost:5001). Щелкните **Принять**, чтобы принять политику конфиденциальности и использования файлов cookie. Это приложение не хранит персональные данные.

## <a name="edit-a-razor-page"></a>Редактирование страницы Razor

Откройте *Pages/Index.cshtml* и измените страницу, добавив выделенное исправление.

[!code-cshtml[](sample/index.cshtml?highlight=9)]

Перейдите к [https://localhost:5001](https://localhost:5001) и проверьте, отобразились ли изменения.

## <a name="next-steps"></a>Следующие шаги

В этом руководстве вы узнали, как:

> [!div class="checklist"]
> * создавать проект веб-приложения;
> * включать локальный HTTPS;
> * Запустите проект.
> * вносить изменения.

Дополнительные сведения об ASP.NET Core см. во введении:

> [!div class="nextstepaction"]
> <xref:index>

> [!NOTE]
> Мы тестируем удобство использования новой предлагаемой структуры оглавления для ASP.NET Core. Если у вас есть несколько минут, и вы хотите попробовать найти семь разных тем в существующей или планируемой структуре оглавления, [щелкните здесь, чтобы принять участие в исследовании](https://dpk4xbh5.optimalworkshop.com/treejack/aa11wn82).
