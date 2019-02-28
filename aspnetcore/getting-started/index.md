---
title: Начало работы с ASP.NET Core
author: rick-anderson
description: 'Краткий учебник, в котором с помощью ASP.NET Core создается и запускается простое приложение Hello World.'
ms.author: riande
ms.custom: mvc
ms.date: 01/15/2019
uid: getting-started
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

![Диалоговое окно "Предупреждение о безопасности"](~/getting-started/_static/cert.png)

Выберите **Да**, если согласны доверять сертификату разработки.

# <a name="macostabmacos"></a>[macOS](#tab/macos)

```console
dotnet dev-certs https --trust
```

Приведенная выше команда отображает следующее сообщение.

*Запрошено доверие к сертификату разработки HTTPS. Если сертификат не является доверенным, выполните следующую команду:* `'sudo security add-trusted-cert -d -r trustRoot -k /Library/Keychains/System.keychain <<certificate>>'`.
 
Эта команда может запросить пароль для установки сертификата в системной цепочке ключей. Введите пароль, если согласны доверять сертификату разработки.

# <a name="linuxtablinux"></a>[Linux](#tab/linux)

Просмотрите документацию по дистрибутиву Linux, чтобы узнать, как установить отношение доверия к сертификату разработки HTTPS.

---

Дополнительные сведения см. в разделе [Trust the ASP.NET Core HTTPS development certificate](xref:security/enforcing-ssl#trust-the-aspnet-core-https-development-certificate-on-windows-and-macos) (Настройка доверия к сертификату разработки HTTPS ASP.NET Core).

## <a name="run-the-app"></a>Запуск приложения

Выполните следующие команды:

```console
cd aspnetcoreapp
dotnet run
```

Когда интерпретатор команд покажет, что приложение запущено, откройте страницу [https://localhost:5001](https://localhost:5001). Щелкните **Принять**, чтобы принять политику конфиденциальности и использования файлов cookie. Это приложение не хранит персональные данные.

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

Дополнительные сведения об ASP.NET Core см. в разделе рекомендуемой схемы обучения в вводной статье:

> [!div class="nextstepaction"]
> <xref:index#recommended-learning-path>
