<a name="scaffold"></a>
### <a name="scaffold-the-movie-model"></a>Модель создания фильма

* Откройте окно командной строки в папке проекта (папке, где находятся файлы *Program.cs*, *Startup.cs* и *.csproj* файлов).
* Выполните следующую команду:

  ```console
  dotnet aspnet-codegenerator razorpage -m Movie -dc MovieContext -udl -outDir Pages/Movies --referenceScriptLibraries
  ```
  
Если возникает ошибка.
  ```
No executable found matching command "dotnet-aspnet-codegenerator"
  ```

Откройте окно командной строки в папке проекта (папке, где находятся файлы *Program.cs*, *Startup.cs* и *.csproj* файлов).