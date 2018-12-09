---
title: Добавить, загрузки и удаления данных пользователя для удостоверения в проекте ASP.NET Core
author: rick-anderson
description: Узнайте, как добавить пользовательские данные для удостоверения в проекте ASP.NET Core. Удаление данных в соответствии с GDPR.
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.date: 6/16/2018
ms.custom: seodec18
uid: security/authentication/add-user-data
ms.openlocfilehash: 5465117e5db880e8298e6c2075a27699e4081894
ms.sourcegitcommit: 49faca2644590fc081d86db46ea5e29edfc28b7b
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 12/09/2018
ms.locfileid: "53121105"
---
# <a name="add-download-and-delete-custom-user-data-to-identity-in-an-aspnet-core-project"></a>Добавление, скачивание и удаление пользовательских данных для удостоверений в проекте ASP.NET Core

Автор: [Рик Андерсон](https://twitter.com/RickAndMSFT) (Rick Anderson)

В этой статье показано, как:

* Добавьте пользовательские данные веб-приложение ASP.NET Core.
* Украшение модели пользовательских данных с [PersonalData](/dotnet/api/microsoft.aspnetcore.identity.personaldataattribute?view=aspnetcore-2.1) атрибут, чтобы оно автоматически доступно для загрузки и удаления. Возможность загрузки и удаления данных помогает обеспечить соответствие [GDPR](xref:security/gdpr) требования.

В примере проекта создается на основе веб-приложения Razor Pages, но инструкции одинаковы для веб-приложения ASP.NET Core MVC.

[Просмотреть или скачать образец кода](https://github.com/aspnet/Docs/tree/live/aspnetcore/security/authentication/add-user-data/sample) ([как скачивать](xref:index#how-to-download-a-sample))

## <a name="prerequisites"></a>Предварительные требования

[!INCLUDE [](~/includes/2.1-SDK.md)]

## <a name="create-a-razor-web-app"></a>Создание веб-приложения Razor

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

* В меню **Файл** Visual Studio откройте меню **Создать** > **Проект**. Назовите проект **WebApp1** Если вы хотите его совпадать с пространством имен [загрузить образец](https://github.com/aspnet/Docs/tree/live/aspnetcore/security/authentication/add-user-data/sample) кода.
* Выберите **веб-приложение ASP.NET Core** > **ОК**
* Выберите **ASP.NET Core 2.1** в раскрывающемся списке
* Выберите **веб-приложение**  > **ОК**
* Постройте и запустите проект.

# <a name="net-core-clitabnetcore-cli"></a>[Интерфейс командной строки .NET Core](#tab/netcore-cli)

```cli
dotnet new webapp -o WebApp1
```

---

## <a name="run-the-identity-scaffolder"></a>Запустите шаблон удостоверений

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

* Из **обозревателе решений**, правой кнопкой мыши проект > **добавить** > **создать шаблонный элемент**.
* В области слева от **Добавление шаблона** диалоговое окно, выберите **удостоверений** > **добавить**.
* В **удостоверение ADD** диалоговом окне следующие параметры:
  * Выберите существующий файл макета *~/Pages/Shared/_Layout.cshtml*
  * Выберите следующие файлы для переопределения:
    * **Учетная запись: регистрация**
    * **Учетная запись и управление/индексов**
  * Выберите **+** кнопку, чтобы создать новый **класс контекста данных**. Выберите тип (**WebApp1.Models.WebApp1Context** Если проект называется **WebApp1**).
  * Выберите **+** кнопку, чтобы создать новый **класс пользователя**. Выберите тип (**WebApp1User** Если проект называется **WebApp1**) > **добавить**.
* Выберите **добавить**.

# <a name="net-core-clitabnetcore-cli"></a>[Интерфейс командной строки .NET Core](#tab/netcore-cli)

Если вы еще не установлен шаблон ASP.NET Core, установите его:

```cli
dotnet tool install -g dotnet-aspnet-codegenerator
```

Добавьте ссылку на пакет [Microsoft.VisualStudio.Web.CodeGeneration.Design](https://www.nuget.org/packages/Microsoft.VisualStudio.Web.CodeGeneration.Design/) в файл проекта (csproj). Выполните следующую команду в каталоге проекта:

```cli
dotnet add package Microsoft.VisualStudio.Web.CodeGeneration.Design
dotnet restore
```

Выполните следующую команду, чтобы получить список вариантов шаблон удостоверений:

```cli
dotnet aspnet-codegenerator identity -h
```

В папке проекта запустите шаблон удостоверений:

```cli
dotnet aspnet-codegenerator identity -u WebApp1User -fi Account.Register;Account.Manage.Index
```

-------------

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

[!code-csharp[Main](add-user-data/sample/Areas/Identity/Data/WebApp1User.cs)]

Свойства с атрибутом [PersonalData](/dotnet/api/microsoft.aspnetcore.identity.personaldataattribute?view=aspnetcore-2.1) , атрибут:

* Удалено, когда *Areas/Identity/Pages/Account/Manage/DeletePersonalData.cshtml* страница Razor вызывает `UserManager.Delete`.
* Включенные в загруженные данные по *Areas/Identity/Pages/Account/Manage/DownloadPersonalData.cshtml* страницу Razor.

### <a name="update-the-accountmanageindexcshtml-page"></a>Обновление страницы Account/Manage/Index.cshtml

Обновление `InputModel` в *Areas/Identity/Pages/Account/Manage/Index.cshtml.cs* на следующий выделенный код:

[!code-csharp[Main](add-user-data/sample/Areas/Identity/Pages/Account/Manage/Index.cshtml.cs?name=snippet&highlight=28-36,63-64,87-95,120)]

Обновление *Areas/Identity/Pages/Account/Manage/Index.cshtml* с выделенную ниже разметку:

[!code-html[Main](add-user-data/sample/Areas/Identity/Pages/Account/Manage/Index.cshtml?highlight=34-41)]

### <a name="update-the-accountregistercshtml-page"></a>Обновление страницы Account/Register.cshtml

Обновление `InputModel` в *Areas/Identity/Pages/Account/Register.cshtml.cs* на следующий выделенный код:

[!code-csharp[Main](add-user-data/sample/Areas/Identity/Pages/Account/Register.cshtml.cs?name=snippet&highlight=8-16,43,44)]

Обновление *Areas/Identity/Pages/Account/Register.cshtml* с выделенную ниже разметку:

[!code-html[Main](add-user-data/sample/Areas/Identity/Pages/Account/Register.cshtml?highlight=16-25)]

Выполните построение проекта.

### <a name="add-a-migration-for-the-custom-user-data"></a>Добавьте миграцию для пользовательских данных

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

В Visual Studio **консоль диспетчера пакетов**:

```PMC
Add-Migration CustomUserData
Update-Database
```

# <a name="net-core-clitabnetcore-cli"></a>[Интерфейс командной строки .NET Core](#tab/netcore-cli)

```cli
dotnet ef migrations add CustomUserData
dotnet ef database update
```

------

## <a name="test-create-view-download-delete-custom-user-data"></a>Тест создание, просмотр, загрузка, удалить пользовательские данные

Проверьте работу приложения:

* Регистрация нового пользователя.
* Просмотр пользовательских данных на `/Identity/Account/Manage` страницы.
* Скачать и просмотреть личные данные пользователей из `/Identity/Account/Manage/PersonalData` страницы.
