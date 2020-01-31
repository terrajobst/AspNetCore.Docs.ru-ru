---
title: Добавить, загрузки и удаления данных пользователя для удостоверения в проекте ASP.NET Core
author: rick-anderson
description: Узнайте, как добавить пользовательские данные для удостоверения в проекте ASP.NET Core. Удаление данных в соответствии с GDPR.
ms.author: riande
ms.date: 01/28/2020
ms.custom: mvc, seodec18
uid: security/authentication/add-user-data
ms.openlocfilehash: e08c02e2e5d4a429aae10c59e7ae3ea48c975067
ms.sourcegitcommit: c81ef12a1b6e6ac838e5e07042717cf492e6635b
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/29/2020
ms.locfileid: "76885555"
---
# <a name="add-download-and-delete-custom-user-data-to-identity-in-an-aspnet-core-project"></a>Добавление, скачивание и удаление пользовательских данных для удостоверений в проекте ASP.NET Core

Автор: [Рик Андерсон](https://twitter.com/RickAndMSFT) (Rick Anderson)

В этой статье показано, как:

* Добавьте пользовательские данные веб-приложение ASP.NET Core.
* Пометьте пользовательскую модель данных атрибутом <xref:Microsoft.AspNetCore.Identity.PersonalDataAttribute>, чтобы она была автоматически доступна для скачивания и удаления. Возможность загрузки и удаления данных помогает обеспечить соответствие [GDPR](xref:security/gdpr) требования.

В примере проекта создается на основе веб-приложения Razor Pages, но инструкции одинаковы для веб-приложения ASP.NET Core MVC.

[Просмотреть или скачать образец кода](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/security/authentication/add-user-data) ([как скачивать](xref:index#how-to-download-a-sample))

## <a name="prerequisites"></a>Prerequisites

::: moniker range=">= aspnetcore-3.0"

[!INCLUDE [](~/includes/3.0-SDK.md)]

::: moniker-end

::: moniker range="< aspnetcore-3.0"

[!INCLUDE [](~/includes/2.2-SDK.md)]

::: moniker-end

## <a name="create-a-razor-web-app"></a>Создание веб-приложения Razor

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

::: moniker range=">= aspnetcore-3.0"

* В меню **Файл** Visual Studio откройте **Создать** > **Проект**. Назовите проект **WebApp1** Если вы хотите его совпадать с пространством имен [загрузить образец](https://github.com/aspnet/AspNetCore.Docs/tree/live/aspnetcore/security/authentication/add-user-data) кода.
* Выберите **ASP.NET Core веб-приложение** > **ОК** .
* Выберите **ASP.NET Core 3,0** в раскрывающемся списке.
* Выберите **веб-приложение** > **ОК** .
* Постройте и запустите проект.

::: moniker-end

::: moniker range="< aspnetcore-3.0"

* В меню **Файл** Visual Studio откройте **Создать** > **Проект**. Назовите проект **WebApp1** Если вы хотите его совпадать с пространством имен [загрузить образец](https://github.com/aspnet/AspNetCore.Docs/tree/live/aspnetcore/security/authentication/add-user-data) кода.
* Выберите **ASP.NET Core веб-приложение** > **ОК** .
* Выберите **ASP.NET Core 2,2** в раскрывающемся списке.
* Выберите **веб-приложение** > **ОК** .
* Постройте и запустите проект.

::: moniker-end


# <a name="net-core-clitabnetcore-cli"></a>[Интерфейс командной строки .NET Core](#tab/netcore-cli)

```dotnetcli
dotnet new webapp -o WebApp1
```

---

## <a name="run-the-identity-scaffolder"></a>Запустите шаблон удостоверений

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

* Из **обозревателе решений**, правой кнопкой мыши проект > **добавить** > **создать шаблонный элемент**.
* В левой области диалогового окна **Добавление шаблона** выберите **удостоверение** > **добавить**.
* В диалоговом окне **Добавление удостоверения** выполните следующие действия.
  * Выберите существующий файл макета *~/Pages/Shared/_Layout.cshtml*
  * Выберите следующие файлы для переопределения:
    * **Учетная запись: регистрация**
    * **Учетная запись и управление/индексов**
  * Выберите **+** кнопку, чтобы создать новый **класс контекста данных**. Выберите тип (**WebApp1.Models.WebApp1Context** Если проект называется **WebApp1**).
  * Выберите **+** кнопку, чтобы создать новый **класс пользователя**. Выберите тип (**WebApp1User** Если проект называется **WebApp1**) > **добавить**.
* Нажмите **Добавить**.

# <a name="net-core-clitabnetcore-cli"></a>[Интерфейс командной строки .NET Core](#tab/netcore-cli)

Если вы еще не установлен шаблон ASP.NET Core, установите его:

```dotnetcli
dotnet tool install -g dotnet-aspnet-codegenerator
```

Добавьте ссылку на пакет [Microsoft.VisualStudio.Web.CodeGeneration.Design](https://www.nuget.org/packages/Microsoft.VisualStudio.Web.CodeGeneration.Design/) в файл проекта (csproj). Выполните следующую команду в каталоге проекта:

```dotnetcli
dotnet add package Microsoft.VisualStudio.Web.CodeGeneration.Design
dotnet restore
```

Выполните следующую команду, чтобы получить список вариантов шаблон удостоверений:

```dotnetcli
dotnet aspnet-codegenerator identity -h
```

В папке проекта запустите шаблон удостоверений:

```dotnetcli
dotnet aspnet-codegenerator identity -u WebApp1User -fi Account.Register;Account.Manage.Index
```

---

Следуя инструкциям из [миграция, UseAuthentication и макет](xref:security/authentication/scaffold-identity#efm) для выполните следующие действия:

* Создание миграции и обновления базы данных.
* Добавьте `UseAuthentication` в `Startup.Configure`.
* Добавление `<partial name="_LoginPartial" />` с файлами макетов.
* Проверьте работу приложения:
  * Регистрация пользователя
  * Выберите новое имя пользователя (рядом с полем **выхода** ссылку). Может потребоваться развернуть окно или выберите значок панели навигации, чтобы отобразить имя пользователя и другие ссылки.
  * Выберите **персональных данных** вкладки.
  * Выберите **загрузить** кнопку и изучить *PersonalData.json* файла.
  * Тест **удалить** кнопку, которая удаляет вошедшего пользователя.

## <a name="add-custom-user-data-to-the-identity-db"></a>Добавить пользовательские данные в базу данных удостоверений

Обновление `IdentityUser` производного класса с пользовательскими свойствами. Если вы с именем проекта WebApp1, этот файл имеет имя *Areas/Identity/Data/WebApp1User.cs*. Обновление файла следующим кодом:

::: moniker range=">= aspnetcore-3.0"

[!code-csharp[](add-user-data/samples/3.x/SampleApp/Areas/Identity/Data/WebApp1User.cs)]

::: moniker-end

::: moniker range="< aspnetcore-3.0"

[!code-csharp[](add-user-data/samples/2.x/SampleApp/Areas/Identity/Data/WebApp1User.cs)]

::: moniker-end

Свойства с атрибутом [персоналдата](/dotnet/api/microsoft.aspnetcore.identity.personaldataattribute) :

* Удалено, когда *Areas/Identity/Pages/Account/Manage/DeletePersonalData.cshtml* страница Razor вызывает `UserManager.Delete`.
* Включенные в загруженные данные по *Areas/Identity/Pages/Account/Manage/DownloadPersonalData.cshtml* страницу Razor.

### <a name="update-the-accountmanageindexcshtml-page"></a>Обновление страницы Account/Manage/Index.cshtml

Обновление `InputModel` в *Areas/Identity/Pages/Account/Manage/Index.cshtml.cs* на следующий выделенный код:

::: moniker range=">= aspnetcore-3.0"

[!code-csharp[](add-user-data/samples/3.x/SampleApp/Areas/Identity/Pages/Account/Manage/Index.cshtml.cs?name=snippet&highlight=24-32,48-49,96-104,106)]

Обновление *Areas/Identity/Pages/Account/Manage/Index.cshtml* с выделенную ниже разметку:

[!code-cshtml[](add-user-data/samples/3.x/SampleApp/Areas/Identity/Pages/Account/Manage/Index.cshtml?highlight=18-25)]

::: moniker-end

::: moniker range="< aspnetcore-3.0"

[!code-csharp[](add-user-data/samples/2.x/SampleApp/Areas/Identity/Pages/Account/Manage/Index.cshtml.cs?name=snippet&highlight=28-36,63-64,98-106,119)]

Обновление *Areas/Identity/Pages/Account/Manage/Index.cshtml* с выделенную ниже разметку:

[!code-chtml[](add-user-data/samples/2.x/SampleApp/Areas/Identity/Pages/Account/Manage/Index.cshtml?highlight=35-42)]

::: moniker-end

### <a name="update-the-accountregistercshtml-page"></a>Обновление страницы Account/Register.cshtml

Обновление `InputModel` в *Areas/Identity/Pages/Account/Register.cshtml.cs* на следующий выделенный код:

::: moniker range=">= aspnetcore-3.0"

[!code-csharp[](add-user-data/samples/3.x/SampleApp/Areas/Identity/Pages/Account/Register.cshtml.cs?name=snippet&highlight=30-38,70-71)]

Обновление *Areas/Identity/Pages/Account/Register.cshtml* с выделенную ниже разметку:

[!code-cshtml[](add-user-data/samples/3.x/SampleApp/Areas/Identity/Pages/Account/Register.cshtml?highlight=16-25)]

::: moniker-end

::: moniker range="< aspnetcore-3.0"

[!code-csharp[](add-user-data/samples/2.x/SampleApp/Areas/Identity/Pages/Account/Register.cshtml.cs?name=snippet&highlight=28-36,67,66)]

Обновление *Areas/Identity/Pages/Account/Register.cshtml* с выделенную ниже разметку:

[!code-chtml[](add-user-data/samples/2.x/SampleApp/Areas/Identity/Pages/Account/Register.cshtml?highlight=16-25)]

::: moniker-end


Постройте проект.

### <a name="add-a-migration-for-the-custom-user-data"></a>Добавьте миграцию для пользовательских данных

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

В Visual Studio **консоль диспетчера пакетов**:

```powershell
Add-Migration CustomUserData
Update-Database
```

# <a name="net-core-clitabnetcore-cli"></a>[Интерфейс командной строки .NET Core](#tab/netcore-cli)

```dotnetcli
dotnet ef migrations add CustomUserData
dotnet ef database update
```

---

## <a name="test-create-view-download-delete-custom-user-data"></a>Тест создание, просмотр, загрузка, удалить пользовательские данные

Проверьте работу приложения:

* Регистрация нового пользователя.
* Просмотр пользовательских данных на `/Identity/Account/Manage` страницы.
* Скачать и просмотреть личные данные пользователей из `/Identity/Account/Manage/PersonalData` страницы.
