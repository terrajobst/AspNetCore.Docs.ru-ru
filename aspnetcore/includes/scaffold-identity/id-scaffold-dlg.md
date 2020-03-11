Запуск шаблона идентификации:

# <a name="visual-studio"></a>[Visual Studio](#tab/visual-studio)

* В **Обозреватель решений**щелкните правой кнопкой мыши проект > **Добавить** > новый шаблонный **элемент**.
* В левой области диалогового окна **Добавление шаблона** выберите **удостоверение** > **добавить**.
* В диалоговом окне **Добавление удостоверения** выберите нужные параметры.
  * Выберите существующую страницу макета, или файл макета будет перезаписан с неверной разметкой. Например `~/Pages/Shared/_Layout.cshtml` для Razor Pages `~/Views/Shared/_Layout.cshtml` для проектов MVC
  * Нажмите кнопку **+** , чтобы создать новый **класс контекста данных**.
* Нажмите кнопку **Добавить**.

# <a name="net-core-cli"></a>[Интерфейс командной строки .NET Core](#tab/netcore-cli)

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

В папке проекта запустите механизм формирования удостоверений с нужными параметрами. Например, чтобы настроить удостоверение с пользовательским интерфейсом по умолчанию и минимальным числом файлов, выполните следующую команду:

```dotnetcli
dotnet aspnet-codegenerator identity --useDefaultUI
```

---
