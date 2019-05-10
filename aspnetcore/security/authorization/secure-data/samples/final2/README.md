# <a name="how-to-buildrun-secure-user-data-sample"></a>Как построение и запуск образца данных безопасности пользователей

* Задайте пароль с помощью диспетчера секретов:

  `dotnet user-secrets set SeedUserPW <pw>`

* Обновление базы данных:

  `dotnet ef database update`

* Включение протокола HTTPS в проекте
