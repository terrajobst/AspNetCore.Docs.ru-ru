# <a name="how-to-buildrun-secure-user-data-sample"></a><span data-ttu-id="72702-101">Как построение и запуск образца данных безопасности пользователей</span><span class="sxs-lookup"><span data-stu-id="72702-101">How to build/run Secure user data sample</span></span>

* <span data-ttu-id="72702-102">Задать пароль с помощью средства диспетчера секрет:</span><span class="sxs-lookup"><span data-stu-id="72702-102">Set password with the Secret Manager tool:</span></span>

  `dotnet user-secrets set SeedUserPW <pw>`

* <span data-ttu-id="72702-103">Обновление базы данных:</span><span class="sxs-lookup"><span data-stu-id="72702-103">Update the database:</span></span>

    `dotnet ef database update`

* <span data-ttu-id="72702-104">Протокол SSL включен в проект</span><span class="sxs-lookup"><span data-stu-id="72702-104">Enable SSL in the project</span></span>