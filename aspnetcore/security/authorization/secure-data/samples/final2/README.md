# <a name="how-to-buildrun-secure-user-data-sample"></a>Как построение и запуск образца данных безопасности пользователей

* Задать пароль с помощью средства диспетчера секрет:

  `dotnet user-secrets set SeedUserPW <pw>`

* Обновление базы данных:

    `dotnet ef database update`

* Протокол SSL включен в проект