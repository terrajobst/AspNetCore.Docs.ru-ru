---
title: Добавить, загрузить и удалить пользовательские данные для удостоверения в проекте ASP.NET Core
author: rick-anderson
description: Дополнительные сведения о добавлении пользовательские данные для удостоверения в проекте ASP.NET Core. Удаление данных в каждом GDPR.
manager: wpickett
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.date: 6/16/2018
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: security/authentication/add-user-data
ms.openlocfilehash: 23fd792c0d93c038f31ce947e7885ad6e36d119e
ms.sourcegitcommit: d4cefc0c63550c64a8040b11867cc05efcfb7e86
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 06/05/2018
ms.locfileid: "34758792"
---
# <a name="add-download-and-delete-custom-user-data-to-identity-in-an-aspnet-core-project"></a>Добавить, загрузить и удалить пользовательские данные для удостоверения в проекте ASP.NET Core

Автор: [Рик Андерсон](https://twitter.com/RickAndMSFT) (Rick Anderson)

В этой статье показано, как:

* Добавьте пользовательские данные для веб-приложение ASP.NET Core.
* Украшение пользовательские модели данных с [PersonalData](/dotnet/api/microsoft.aspnetcore.identity.personaldataattribute?view=aspnetcore-2.1) атрибута, поэтому оно автоматически доступно для загрузки и удаления. Что делает данные могут быть загружены и удалить помогает удовлетворять [GDPR](xref:security/gdpr) требования.

В образце проекта создается на основе страниц Razor веб-приложения, но инструкции похожи для веб-приложения ASP.NET Core MVC.

[Просмотреть или скачать образец кода](https://github.com/aspnet/Docs/tree/live/aspnetcore/security/authentication/add-user-data/sample) ([как скачивать](xref:tutorials/index#how-to-download-a-sample))

## <a name="prerequisites"></a>Предварительные требования

[!INCLUDE [](~/includes/2.1-SDK.md)]

## <a name="create-a-razor-web-app"></a>Создание веб-приложения Razor

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

* В меню **Файл** Visual Studio откройте меню **Создать** > **Проект**. Назовите проект **WebApp1** Если вы хотите его совпадать с пространством имен [загрузить пример](https://github.com/aspnet/Docs/tree/live/aspnetcore/security/authentication/add-user-data/sample) кода.
* Выберите **веб-приложения ASP.NET Core** > **ОК**
* Выберите **ASP.NET Core 2.1** в раскрывающемся списке
* Выберите **веб-приложение**  > **ОК**
* Постройте и запустите проект.

# <a name="net-core-clitabnetcore-cli"></a>[Интерфейс командной строки .NET Core](#tab/netcore-cli)

```cli
dotnet new webapp -o WebApp1
```

------

## <a name="run-the-identity-scaffolder"></a>Запустите scaffolder удостоверений

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

* Из **обозревателе решений**, щелкните правой кнопкой мыши по проекту > **добавить** > **новый элемент формирования шаблонов**.
* В левой области **Добавление формирования шаблонов** диалогового окна выберите **удостоверение** > **добавить**.
* В **добавить удостоверение** диалогового окна, следующие параметры:
  * Выберите существующий файл макета *~/Pages/Shared/_Layout.cshtml*
  * Выберите следующие файлы для переопределения:
    * **Регистрация учетной записи**
    * **Учетная запись или управление и индекса**
  * Выберите **+** кнопку, чтобы создать новый **класс контекста данных**. Принимает тип (**WebApp1.Models.WebApp1Context** Если имя проекта **WebApp1**).
  * Выберите **+** кнопку, чтобы создать новый **класс пользователя**. Принимает тип (**WebApp1User** Если имя проекта **WebApp1**) > **добавить**.
* Выберите **добавить**.

# <a name="net-core-clitabnetcore-cli"></a>[Интерфейс командной строки .NET Core](#tab/netcore-cli)

Если ASP.NET scaffolder ранее не установлен, установите его:

```cli
dotnet tool install -g dotnet-aspnet-codegenerator
```

Добавьте ссылку на пакет [Microsoft.VisualStudio.Web.CodeGeneration.Design](https://www.nuget.org/packages/Microsoft.VisualStudio.Web.CodeGeneration.Design/) в файл проекта (CSPROJ-файл). В каталоге проекта, выполните следующую команду:

```cli
dotnet add package Microsoft.VisualStudio.Web.CodeGeneration.Design
dotnet restore
```

Выполните следующую команду, чтобы список параметров scaffolder удостоверений:

```cli
dotnet aspnet-codegenerator identity -h
```

В папке проекта запустите scaffolder удостоверений:

```cli
dotnet aspnet-codegenerator identity -u WebApp1User -fi Account.Register;Account.Manage.Index
```

-------------

Следуйте инструкциям в [миграции UseAuthentication, а также макет](xref:security/authentication/scaffold-identity#efm) для выполнения следующих действий:

* Создание миграции и обновить базу данных.
* Добавьте `UseAuthentication` в `Startup.Configure`.
* Добавить `<partial name="_LoginPartial" />` файл макета.
* Проверьте работу приложения:
  * Регистрация пользователя
  * Выберите новое имя пользователя (рядом с **выхода** связи). Может потребоваться развернуть окно или выберите значок панели навигации, чтобы отобразить имя пользователя и другие ссылки.
  * Выберите **персональные данные** вкладки.
  * Выберите **загрузки** кнопку и изучить *PersonalData.json* файла.
  * Тест **удалить** кнопку, которая удаляет вошедшего пользователя.

## <a name="add-custom-user-data-to-the-identity-db"></a>Добавить пользовательские данные в базу данных удостоверений

Обновление `IdentityUser` производного класса с пользовательскими свойствами. Если название проекта WebApp1 этот файл имеет имя *Areas/Identity/Data/WebApp1User.cs*. Обновление файла следующим кодом:

[!code-csharp[Main](add-user-data/sample/Areas/Identity/Data/WebApp1User.cs)]

Свойства декорированных [PersonalData](/dotnet/api/microsoft.aspnetcore.identity.personaldataattribute?view=aspnetcore-2.1) являются:

* Удаляются при *Areas/Identity/Pages/Account/Manage/DeletePersonalData.cshtml* Razor страница вызывает `UserManager.Delete`.
* Включенные в загруженные данные по *Areas/Identity/Pages/Account/Manage/DownloadPersonalData.cshtml* страниц Razor.

### <a name="update-the-accountmanageindexcshtml-page"></a>Страница «обновление» Account/Manage/Index.cshtml

Обновление `InputModel` в *Areas/Identity/Pages/Account/Manage/Index.cshtml.cs* на следующий выделенный код:

[!code-csharp[Main](add-user-data/sample/Areas/Identity/Pages/Account/Manage/Index.cshtml.cs?name=snippet&highlight=28-36,63-64,87-95)]

Обновление *Areas/Identity/Pages/Account/Manage/Index.cshtml* с следующую выделенную разметку:

[!code-html[Main](add-user-data/sample/Areas/Identity/Pages/Account/Manage/Index.cshtml?highlight=34-41)]

### <a name="update-the-accountregistercshtml-page"></a>Страница «обновление» Account/Register.cshtml

Обновление `InputModel` в *Areas/Identity/Pages/Account/Register.cshtml.cs* на следующий выделенный код:

[!code-csharp[Main](add-user-data/sample/Areas/Identity/Pages/Account/Register.cshtml.cs?name=snippet&highlight=8-16,43,44)]

Обновление *Areas/Identity/Pages/Account/Register.cshtml* с следующую выделенную разметку:

[!code-html[Main](add-user-data/sample/Areas/Identity/Pages/Account/Register.cshtml?highlight=16-25)]

Выполните построение проекта.

### <a name="add-a-migration-for-the-custom-user-data"></a>Добавить миграции для пользовательские данные

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
* Загрузить и просмотреть личные данные пользователей из `/Identity/Account/Manage/PersonalData` страницы.