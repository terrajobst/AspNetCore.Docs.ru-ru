::: moniker range=">= aspnetcore-3.0"

Запуск шаблона идентификации:

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

* В **Обозреватель решений**щелкните правой кнопкой мыши проект > **Добавить** > новый шаблонный **элемент**.
* В левой области диалогового окна **Добавление шаблона** выберите **удостоверение** > **добавить**.
* В диалоговом окне **Добавление удостоверения** выберите нужные параметры.
  * Выберите существующую страницу макета, или файл макета будет перезаписан с неверной разметкой. При выборе существующего файла *\_Layout. cshtml* он **не** перезаписывается.

 Например: `~/Pages/Shared/_Layout.cshtml` для Razor Pages `~/Views/Shared/_Layout.cshtml` для проектов MVC
* Чтобы использовать существующий контекст данных, выберите по меньшей мере один файл для переопределения. Необходимо выбрать по крайней мере один файл для добавления контекста данных.
  * Выберите класс контекста данных.
  * Нажмите **Добавить**.
* Чтобы создать новый контекст пользователя и, возможно, создать настраиваемый класс пользователя для удостоверения:
  * Выберите **+** кнопку, чтобы создать новый **класс контекста данных**.
  * Нажмите **Добавить**.

Примечание. Если вы создаете новый контекст пользователя, вам не нужно выбирать файл для переопределения.

# <a name="net-core-clitabnetcore-cli"></a>[Интерфейс командной строки .NET Core](#tab/netcore-cli)

Если вы еще не установлен шаблон ASP.NET Core, установите его:

```dotnetcli
dotnet tool install -g dotnet-aspnet-codegenerator
```

Добавьте необходимые ссылки на пакет NuGet в файл проекта (\*. csproj). Выполните следующую команду в каталоге проекта:

```dotnetcli
dotnet add package Microsoft.VisualStudio.Web.CodeGeneration.Design
dotnet add package Microsoft.EntityFrameworkCore.Design
dotnet add package Microsoft.AspNetCore.Identity.EntityFrameworkCore
dotnet add package Microsoft.AspNetCore.Identity.UI
dotnet add package Microsoft.EntityFrameworkCore.SqlServer
dotnet add package Microsoft.EntityFrameworkCore.Tools
```

Выполните следующую команду, чтобы получить список вариантов шаблон удостоверений:

```dotnetcli
dotnet aspnet-codegenerator identity -h
```

[!INCLUDE[](~/includes/scaffoldTFM.md)]

В папке проекта запустите механизм формирования удостоверений с нужными параметрами. Например, чтобы настроить удостоверение с пользовательским интерфейсом по умолчанию и минимальным числом файлов, выполните следующую команду. Используйте правильное полное имя для контекста базы данных:

```dotnetcli
dotnet aspnet-codegenerator identity -dc MyWeb.Data.ApplicationDbContext --files "Account.Register;Account.Login"
```

В PowerShell используется точка с запятой в качестве разделителя команд. При использовании PowerShell заключите точку с запятой в список файлов или вставьте список файлов в двойные кавычки. Например:

```dotnetcli
dotnet aspnet-codegenerator identity -dc MyWeb.Data.ApplicationDbContext --files "Account.Register;Account.Login;Account.Logout"
```

Если вы запустите механизм формирования идентификаторов без указания флага `--files` или флага `--useDefaultUI`, в проекте будут созданы все доступные страницы пользовательского интерфейса идентификации.

---

::: moniker-end

::: moniker range="< aspnetcore-3.0"

Запуск шаблона идентификации:

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

* В **Обозреватель решений**щелкните правой кнопкой мыши проект > **Добавить** > новый шаблонный **элемент**.
* В левой области диалогового окна **Добавление шаблона** выберите **удостоверение** > **добавить**.
* В диалоговом окне **Добавление удостоверения** выберите нужные параметры.
  * Выберите существующую страницу макета, или файл макета будет перезаписан с неверной разметкой. При выборе существующего файла *\_Layout. cshtml* он **не** перезаписывается.

 Например: `~/Pages/Shared/_Layout.cshtml` для Razor Pages `~/Views/Shared/_Layout.cshtml` для проектов MVC
* Чтобы использовать существующий контекст данных, выберите по меньшей мере один файл для переопределения. Необходимо выбрать по крайней мере один файл для добавления контекста данных.
  * Выберите класс контекста данных.
  * Нажмите **Добавить**.
* Чтобы создать новый контекст пользователя и, возможно, создать настраиваемый класс пользователя для удостоверения:
  * Выберите **+** кнопку, чтобы создать новый **класс контекста данных**.
  * Нажмите **Добавить**.

Примечание. Если вы создаете новый контекст пользователя, вам не нужно выбирать файл для переопределения.

# <a name="net-core-clitabnetcore-cli"></a>[Интерфейс командной строки .NET Core](#tab/netcore-cli)

Если вы еще не установлен шаблон ASP.NET Core, установите его:

```dotnetcli
dotnet tool install -g dotnet-aspnet-codegenerator
```

Добавьте ссылку на пакет в [Microsoft. VisualStudio. Web. стратегию. Design](https://www.nuget.org/packages/Microsoft.VisualStudio.Web.CodeGeneration.Design/) в файл проекта (\*. csproj). Выполните следующую команду в каталоге проекта:

```dotnetcli
dotnet add package Microsoft.VisualStudio.Web.CodeGeneration.Design
dotnet restore
```

Выполните следующую команду, чтобы получить список вариантов шаблон удостоверений:

```dotnetcli
dotnet aspnet-codegenerator identity -h
```

В папке проекта запустите механизм формирования удостоверений с нужными параметрами. Например, чтобы настроить удостоверение с пользовательским интерфейсом по умолчанию и минимальным числом файлов, выполните следующую команду. Используйте правильное полное имя для контекста базы данных:

```dotnetcli
dotnet aspnet-codegenerator identity -dc MyWeb.Data.ApplicationDbContext --files "Account.Register;Account.Login"
```

В PowerShell используется точка с запятой в качестве разделителя команд. При использовании PowerShell заключите точку с запятой в список файлов или вставьте список файлов в двойные кавычки. Например:

```dotnetcli
dotnet aspnet-codegenerator identity -dc MyWeb.Data.ApplicationDbContext --files "Account.Register;Account.Login;Account.Logout"
```

Если вы запустите механизм формирования идентификаторов без указания флага `--files` или флага `--useDefaultUI`, в проекте будут созданы все доступные страницы пользовательского интерфейса идентификации.

---

::: moniker-end