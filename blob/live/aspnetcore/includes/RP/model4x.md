<a name="scaffold"></a>
### <a name="scaffold-the-movie-model"></a><span data-ttu-id="4f4dd-101">Модель создания фильма</span><span class="sxs-lookup"><span data-stu-id="4f4dd-101">Scaffold the Movie model</span></span>

* <span data-ttu-id="4f4dd-102">Выполните следующую команду в окне командной строки (в папке проекта, содержащей файлы *Program.cs*, *Startup.cs* и *.csproj*).</span><span class="sxs-lookup"><span data-stu-id="4f4dd-102">Run the following from the command line (in the project directory that contains the *Program.cs*, *Startup.cs*, and *.csproj* files):</span></span>

  ```console
  dotnet aspnet-codegenerator razorpage -m Movie -dc MovieContext -udl -outDir Pages/Movies --referenceScriptLibraries
  ```

<span data-ttu-id="4f4dd-103">Если возникает ошибка.</span><span class="sxs-lookup"><span data-stu-id="4f4dd-103">If you get the error:</span></span>
  ```
No executable found matching command "dotnet-aspnet-codegenerator"
  ```

<span data-ttu-id="4f4dd-104">Откройте в командной оболочке папку проекта (папку, содержащую файлы *Program.cs*, *Startup.cs* и *.csproj*).</span><span class="sxs-lookup"><span data-stu-id="4f4dd-104">Open a command shell to the project directory (The directory that contains the *Program.cs*, *Startup.cs*, and *.csproj* files).</span></span>
