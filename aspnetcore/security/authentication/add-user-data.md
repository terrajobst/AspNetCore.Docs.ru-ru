---
title: Добавление, скачивание и удаление данных пользователя в удостоверении в проекте ASP.NET Core
author: rick-anderson
description: Узнайте, как добавить пользовательские данные пользователя в удостоверение в проекте ASP.NET Core. Удаление данных на GDPR.
ms.author: riande
ms.date: 06/18/2019
ms.custom: mvc, seodec18
uid: security/authentication/add-user-data
ms.openlocfilehash: 6daca5776930f80eec8d81132b5a5c4d4d5c13ad
ms.sourcegitcommit: 0dd224b2b7efca1fda0041b5c3f45080327033f6
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 12/02/2019
ms.locfileid: "74681166"
---
# <a name="add-download-and-delete-custom-user-data-to-identity-in-an-aspnet-core-project"></a>Добавление, скачивание и удаление настраиваемых данных пользователя для идентификации в проекте ASP.NET Core

Автор: [Рик Андерсон](https://twitter.com/RickAndMSFT)

В этой статье показано, как:

* Добавление настраиваемых пользовательских данных в веб-приложение ASP.NET Core.
* Добавьте пользовательскую модель данных с атрибутом <xref:Microsoft.AspNetCore.Identity.PersonalDataAttribute>, чтобы он автоматически был доступен для скачивания и удаления. Обеспечение возможности загрузки и удаления данных помогает удовлетворить требования [GDPR](xref:security/gdpr) .

Пример проекта создается на основе веб-приложения Razor Pages, но эти инструкции похожи на ASP.NET Core веб-приложения MVC.

[Просмотреть или скачать образец кода](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/security/authentication/add-user-data) ([как скачивать](xref:index#how-to-download-a-sample))

## <a name="prerequisites"></a>Необходимые компоненты

::: moniker range=">= aspnetcore-3.0"

[!INCLUDE [](~/includes/3.0-SDK.md)]

::: moniker-end

::: moniker range="< aspnetcore-3.0"

[!INCLUDE [](~/includes/2.2-SDK.md)]

::: moniker-end

## <a name="create-a-razor-web-app"></a>Создание веб-приложения Razor

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

::: moniker range=">= aspnetcore-3.0"

* В меню **Файл** Visual Studio откройте меню **Создать** > **Проект**. Присвойте проекту имя **APP1** , если вы хотите, чтобы оно соответствовало пространству имен примера кода для [скачивания](https://github.com/aspnet/AspNetCore.Docs/tree/live/aspnetcore/security/authentication/add-user-data) .
* Выберите **ASP.NET Core веб-приложение** > **ОК** .
* Выберите **ASP.NET Core 3,0** в раскрывающемся списке.
* Выберите **веб-приложение** > **ОК** .
* Постройте и запустите проект.

::: moniker-end

::: moniker range="< aspnetcore-3.0"

* В меню **Файл** Visual Studio откройте меню **Создать** > **Проект**. Присвойте проекту имя **APP1** , если вы хотите, чтобы оно соответствовало пространству имен примера кода для [скачивания](https://github.com/aspnet/AspNetCore.Docs/tree/live/aspnetcore/security/authentication/add-user-data) .
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

## <a name="run-the-identity-scaffolder"></a>Запуск шаблона удостоверений

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

* В **Обозреватель решений**щелкните правой кнопкой мыши проект > **Добавить** > новый шаблонный **элемент**.
* В левой области диалогового окна **Добавление шаблона** выберите **удостоверение** > **добавить**.
* В диалоговом окне **Добавление удостоверения** выполните следующие действия.
  * Выберите существующий файл макета *~/пажес/шаред/_layout. cshtml*
  * Выберите следующие файлы для переопределения:
    * **Учетная запись или регистр**
    * **Учетная запись/управление/индекс**
  * Нажмите кнопку **+** , чтобы создать новый **класс контекста данных**. Примите тип ("имя_проекта **. Models. WebApp1Context** ", если проект называется " **APP1**").
  * Нажмите кнопку **+** , чтобы создать новый **класс пользователя**. Примите тип (**WebApp1User** , если проект называется "имя_проекта **") >** **добавить**.
* Нажмите кнопку **Добавить**.

# <a name="net-core-clitabnetcore-cli"></a>[Интерфейс командной строки .NET Core](#tab/netcore-cli)

Если вы ранее не устанавливали шаблон ASP.NET Core, установите его сейчас:

```dotnetcli
dotnet tool install -g dotnet-aspnet-codegenerator
```

Добавьте ссылку на пакет в [Microsoft. VisualStudio. Web. стратегию. Design](https://www.nuget.org/packages/Microsoft.VisualStudio.Web.CodeGeneration.Design/) в файл проекта (с расширением CSPROJ). Выполните следующую команду в каталоге проекта:

```dotnetcli
dotnet add package Microsoft.VisualStudio.Web.CodeGeneration.Design
dotnet restore
```

Выполните следующую команду, чтобы вывести список параметров шаблона удостоверений:

```dotnetcli
dotnet aspnet-codegenerator identity -h
```

В папке проекта запустите механизм формирования идентификаторов:

```dotnetcli
dotnet aspnet-codegenerator identity -u WebApp1User -fi Account.Register;Account.Manage.Index
```

---

Следуйте инструкциям в разделе [миграция, усеаусентикатион и макет](xref:security/authentication/scaffold-identity#efm) , чтобы выполнить следующие действия.

* Создайте миграцию и обновите базу данных.
* Добавьте `UseAuthentication` в `Startup.Configure`.
* Добавьте `<partial name="_LoginPartial" />` в файл макета.
* Проверьте работу приложения:
  * Регистрация пользователя
  * Выберите новое имя пользователя (рядом с ссылкой для **выхода** ). Может потребоваться развернуть окно или выбрать значок панели навигации, чтобы отобразить имя пользователя и другие ссылки.
  * Перейдите на вкладку **личные данные** .
  * Нажмите кнопку **скачать** и рассмотрели файл *персоналдата. JSON* .
  * Протестируйте кнопку **Удалить** , которая удаляет пользователя, выполнившего вход в систему.

## <a name="add-custom-user-data-to-the-identity-db"></a>Добавление настраиваемых данных пользователя в базу данных удостоверений

Обновление производного класса `IdentityUser` с помощью пользовательских свойств. Если вы назвали имя проекта Project, файл будет называться *Areas/Identity/Data/WebApp1User. CS*. Обновите файл, используя следующий код:

::: moniker range=">= aspnetcore-3.0"

[!code-csharp[](add-user-data/samples/3.x/SampleApp/Areas/Identity/Data/WebApp1User.cs)]

::: moniker-end

::: moniker range="< aspnetcore-3.0"

[!code-csharp[](add-user-data/samples/2.x/SampleApp/Areas/Identity/Data/WebApp1User.cs)]

::: moniker-end

Свойства, отмеченные атрибутом [персоналдата](/dotnet/api/microsoft.aspnetcore.identity.personaldataattribute) :

* Удаляется, когда страница Razor *Areas/Identity/Pages/Account/Manage/делетеперсоналдата. cshtml* вызывает `UserManager.Delete`.
* Включается в Скачанные данные на странице Razor *Areas/Identity/Pages/Account/Manage/довнлоадперсоналдата. cshtml* .

### <a name="update-the-accountmanageindexcshtml-page"></a>Обновление страницы "учетная запись/управление/индекс. cshtml"

Обновите `InputModel` в *области/удостоверение/страницы/учетная запись/управление/index. cshtml. CS* со следующим выделенным кодом:

::: moniker range=">= aspnetcore-3.0"

[!code-csharp[](add-user-data/samples/3.x/SampleApp/Areas/Identity/Pages/Account/Manage/Index.cshtml.cs?name=snippet&highlight=24-32,48-49,96-104,106)]

Обновите *области, идентификаторы, страницы, учетные записи, а также управление/index. cshtml* с помощью следующей выделенной разметки:

[!code-cshtml[](add-user-data/samples/3.x/SampleApp/Areas/Identity/Pages/Account/Manage/Index.cshtml?highlight=18-25)]

::: moniker-end

::: moniker range="< aspnetcore-3.0"

[!code-csharp[](add-user-data/samples/2.x/SampleApp/Areas/Identity/Pages/Account/Manage/Index.cshtml.cs?name=snippet&highlight=28-36,63-64,98-106,119)]

Обновите *области, идентификаторы, страницы, учетные записи, а также управление/index. cshtml* с помощью следующей выделенной разметки:

[!code-chtml[](add-user-data/samples/2.x/SampleApp/Areas/Identity/Pages/Account/Manage/Index.cshtml?highlight=35-42)]

::: moniker-end

### <a name="update-the-accountregistercshtml-page"></a>Обновление страницы "учетная запись/регистрация. cshtml"

Обновите `InputModel` в *области, Identity, Pages/Account/Register. cshtml. CS* со следующим выделенным кодом:

::: moniker range=">= aspnetcore-3.0"

[!code-csharp[](add-user-data/samples/3.x/SampleApp/Areas/Identity/Pages/Account/Register.cshtml.cs?name=snippet&highlight=30-38,70-71)]

Обновите *области, идентификаторы, страницы, учетную запись или Register. cshtml* со следующей выделенной разметкой:

[!code-cshtml[](add-user-data/samples/3.x/SampleApp/Areas/Identity/Pages/Account/Register.cshtml?highlight=16-25)]

::: moniker-end

::: moniker range="< aspnetcore-3.0"

[!code-csharp[](add-user-data/samples/2.x/SampleApp/Areas/Identity/Pages/Account/Register.cshtml.cs?name=snippet&highlight=28-36,67,66)]

Обновите *области, идентификаторы, страницы, учетную запись или Register. cshtml* со следующей выделенной разметкой:

[!code-chtml[](add-user-data/samples/2.x/SampleApp/Areas/Identity/Pages/Account/Register.cshtml?highlight=16-25)]

::: moniker-end


Постройте проект.

### <a name="add-a-migration-for-the-custom-user-data"></a>Добавление миграции для настраиваемых данных пользователя

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

В **консоли диспетчера пакетов**Visual Studio:

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

## <a name="test-create-view-download-delete-custom-user-data"></a>Тестирование создания, просмотра, загрузки, удаления пользовательских данных пользователя

Проверьте работу приложения:

* Регистрация нового пользователя.
* Просмотр настраиваемых данных пользователя на странице `/Identity/Account/Manage`.
* Скачайте и просмотрите персональные данные пользователей на странице `/Identity/Account/Manage/PersonalData`.
