<a name="scaffold"></a>
### <a name="scaffold-the-movie-model"></a><span data-ttu-id="b99e6-101">Модель создания фильма</span><span class="sxs-lookup"><span data-stu-id="b99e6-101">Scaffold the Movie model</span></span>

* <span data-ttu-id="b99e6-102">Откройте окно командной строки в папке проекта (папке, где находятся файлы *Program.cs*, *Startup.cs* и *.csproj* файлов).</span><span class="sxs-lookup"><span data-stu-id="b99e6-102">Open a command window in the project directory (The directory that contains the *Program.cs*, *Startup.cs*, and *.csproj* files).</span></span>
* <span data-ttu-id="b99e6-103">Выполните следующую команду:</span><span class="sxs-lookup"><span data-stu-id="b99e6-103">Run the following command:</span></span>

  ```console
  dotnet aspnet-codegenerator razorpage -m Movie -dc MovieContext -udl -outDir Pages\Movies --referenceScriptLibraries
  ```

<span data-ttu-id="b99e6-104">Если возникает ошибка.</span><span class="sxs-lookup"><span data-stu-id="b99e6-104">If you get the error:</span></span>
  ```
  The process cannot access the file 
 'RazorPagesMovie/bin/Debug/netcoreapp2.0/RazorPagesMovie.dll' 
  because it is being used by another process.
  ```

<span data-ttu-id="b99e6-105">Выйдите из Visual Studio и выполните команду еще раз.</span><span class="sxs-lookup"><span data-stu-id="b99e6-105">Exit Visual Studio and run the command again.</span></span>