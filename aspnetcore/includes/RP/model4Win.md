<a name="scaffold"></a>
### <a name="scaffold-the-movie-model"></a>Модель создания фильма

* Выполните следующую команду в окне командной строки (в папке проекта, содержащей файлы *Program.cs*, *Startup.cs* и *.csproj*).

  ```console
  dotnet aspnet-codegenerator razorpage -m Movie -dc MovieContext -udl -outDir Pages\Movies --referenceScriptLibraries
  ```

Если возникает ошибка.
  ```
No executable found matching command "dotnet-aspnet-codegenerator"
  ```

Откройте в командной оболочке папку проекта (папку, содержащую файлы *Program.cs*, *Startup.cs* и *.csproj*).

Если возникает ошибка.
  ```
  The process cannot access the file
 'RazorPagesMovie/bin/Debug/netcoreapp2.0/RazorPagesMovie.dll'
  because it is being used by another process.
  ```

Выйдите из Visual Studio и выполните команду еще раз.
