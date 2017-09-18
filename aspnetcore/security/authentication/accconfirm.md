---
title: "Подтверждение учетной записи и пароль восстановления в ASP.NET Core"
author: rick-anderson
description: "Показано, как создать приложение ASP.NET Core с помощью сброса пароля и подтверждение по электронной почте."
keywords: "Сброс пароля ASP.NET Core, подтверждение по электронной почте, безопасность"
ms.author: riande
manager: wpickett
ms.date: 07/19/2017
ms.topic: article
ms.technology: aspnet
ms.prod: asp.net-core
uid: security/authentication/accconfirm
ms.openlocfilehash: 8fe21b1a1ccb93c124dbd12a540b195400d45ef6
ms.sourcegitcommit: f8f6b5934bd071a349f5bc1e389365c52b1c00fa
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 09/14/2017
---
# <a name="account-confirmation-and-password-recovery-in-aspnet-core"></a>Подтверждение учетной записи и пароль восстановления в ASP.NET Core

Автор: [Рик Андерсон](https://twitter.com/RickAndMSFT) (Rick Anderson)

Этого учебника показано, как создать приложение ASP.NET Core с помощью сброса пароля и подтверждение по электронной почте.

## <a name="create-a-new-aspnet-core-project"></a>Создание проекта ASP.NET Core

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

Этот шаг применяется Visual Studio в Windows. В следующем разделе инструкции CLI.

Учебник по требуется 2017 г предварительной версии Visual Studio 2 или более поздней версии.

* В Visual Studio создайте новый проект веб-приложения.
* Выберите **ASP.NET Core 2.0**. На следующем рисунке show **.NET Core** установлен, но вы можете выбрать **.NET Framework**.
* Выберите **изменить аутентификацию** и задайте **индивидуальные учетные записи**.
* Оставьте значение по умолчанию **хранилище учетные записи в приложении**.

![Отображение «Radio отдельных учетных записей пользователей» выбран диалоговое окно нового проекта](accconfirm/_static/2.png)

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

Учебник по требуется Visual Studio 2017 или более поздней версии.

* В Visual Studio создайте новый проект веб-приложения.
* Выберите **изменить аутентификацию** и задайте **индивидуальные учетные записи**.

![Отображение «Radio отдельных учетных записей пользователей» выбран диалоговое окно нового проекта](accconfirm/_static/indiv.png)

---

### <a name="net-core-cli-project-creation-for-macos-and-linux"></a>Создание проекта .NET core CLI для macOS и Linux

Если вы используете CLI или SQLite, в окно командной строки выполните следующую команду:

```console
dotnet new mvc --auth Individual
```

* `--auth Individual`Задает шаблон отдельных учетных записей пользователей.
* В Windows, добавить `-uld` параметр. `-uld` Параметр создает строку подключения LocalDB, а не базы данных SQLite.
* Запустите `new mvc --help` для получения справки по этой команде.

## <a name="test-new-user-registration"></a>Регистрация нового пользователя теста

Запустите приложение, выберите **зарегистрировать** ссылку и регистрации пользователя. Следуйте инструкциям для выполнения миграции Entity Framework Core. На этом этапе является проверку только в электронном письме с [[EmailAddress]](https://docs.microsoft.com/dotnet/api/system.componentmodel.dataannotations.emailaddressattribute) атрибута. После отправки регистрации вы вошли в приложение. Далее в этом учебнике мы изменим это, не может входить новых пользователей до проверила электронной почте.

## <a name="view-the-identity-database"></a>Представление базы данных удостоверений

# <a name="sql-servertabsql-server"></a>[SQL Server](#tab/sql-server)

* Из **представление** последовательно выберите пункты **обозреватель объектов SQL Server** (SSOX). 
* Перейдите к **(localdb) (SQL Server 13) MSSQLLocalDB**. Щелкните правой кнопкой мыши **dbo. AspNetUsers** > **просмотра данных**:

![Контекстное меню таблицы AspNetUsers в обозревателе объектов SQL Server](accconfirm/_static/ssox.png)

Примечание `EmailConfirmed` поле является `False`.

Можно использовать это сообщение электронной почты снова на следующем шаге, когда приложение отправляет сообщение с подтверждением. Щелкните правой кнопкой мыши в строке и выберите **удалить**. Удаление сообщения электронной почты теперь псевдоним облегчит в следующих шагах.

# <a name="sqlitetabsqlite"></a>[SQLite](#tab/sqlite)

В разделе [работа SQLite в проект ASP.NET Core MVC](xref:tutorials/first-mvc-app-xplat/working-with-sql) инструкции о том, как просмотреть базу данных SQLite. 

---

## <a name="require-ssl-and-setup-iis-express-for-ssl"></a>Обязательное использование SSL и настроить IIS Express для SSL

В разделе [применения SSL](xref:security/enforcing-ssl).

<a name="prevent-login-at-registration"></a>
## <a name="require-email-confirmation"></a>Требовать подтверждения по электронной почте

Рекомендуется для подтверждения адреса электронной почты Регистрация нового пользователя для проверки того, они не являются олицетворения кто-то другой (то есть они еще не зарегистрированы с помощью электронной почты другого пользователя). Предположим, что имеется дискуссионный форум и избежать "yli@example.com«от регистрации, как»nolivetto@contoso.com.» Без подтверждения по электронной почте»nolivetto@contoso.com» удалось получить нежелательных электронной почты из приложения. Предположим, что пользователь случайно зарегистрированы как «ylo@example.com» и не заметили неверное написание из «yli», они не смогут использовать пароль восстановления, так как у приложения нет электронной почте правильно. Предоставляет ограниченную защиту от программы-роботы подтверждение по электронной почте и не обеспечивает защиту от определяется нежелательных сообщений, которые имеют много рабочей электронной почты псевдонимы, которые они могут использовать для регистрации.

Как правило, требуется запретить пользователям новых учетных данных для веб-узла до того, как они подтверждения по электронной почте. 

Обновление `ConfigureServices` требовать подтверждения по электронной почте:

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

[!code-csharp[Main](accconfirm/sample/WebPW/Startup.cs?name=snippet1&highlight=6-9)]


# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

[!code-csharp[Main](accconfirm/sample/WebApp1/Startup.cs?name=snippet1&highlight=13-16)]

---

 
```csharp
config.SignIn.RequireConfirmedEmail = true;
```
Предыдущая строка запрещает зарегистрированных регистрируются до подтверждения электронной почте. Тем не менее эту строку не запрещает новые пользователи регистрируются после их регистрации. Код по умолчанию осуществляет вход пользователя после их регистрации. Как только они выйдите из системы, они будут снова войти в систему до регистрации. Далее в этом учебнике мы изменим, поэтому заново зарегистрирован код пользователя **не** вход.

### <a name="configure-email-provider"></a>Настройка поставщика услуг электронной почты

В этом учебнике SendGrid используется для отправки электронной почты. Требуется учетной записи SendGrid и ключ для отправки электронной почты. Можно использовать другие поставщики электронной почты. Включает 2.x ASP.NET Core `System.Net.Mail`, позволяющий отправлять электронную почту из приложения. Мы рекомендуем использовать SendGrid или другая служба электронной почты для отправки электронной почты.

[Параметры шаблона](xref:fundamentals/configuration#options-config-objects) используется для доступа к параметрам учетной записи и ключа пользователя. Дополнительные сведения см. в разделе [конфигурации](xref:fundamentals/configuration).

Создание класса для получения ключа защиты электронной почты. Для этого образца `AuthMessageSenderOptions` класса создается в *Services/AuthMessageSenderOptions.cs* файла.

[!code-csharp[Main](accconfirm/sample/WebApp1/Services/AuthMessageSenderOptions.cs?name=snippet1)]

Задать `SendGridUser` и `SendGridKey` с [секрет диспетчера](../app-secrets.md). Пример:

```none
C:\WebAppl\src\WebApp1>dotnet user-secrets set SendGridUser RickAndMSFT
info: Successfully saved SendGridUser = RickAndMSFT to the secret store.
```

В Windows, диспетчер секрет хранит свои пары ключей и значений в *secrets.json* файл в каталоге %APPDATA%/Microsoft/UserSecrets/ < WebAppName userSecretsId >.

Содержимое *secrets.json* файла не шифруются. *Secrets.json* ниже приведен файл ( `SendGridKey` значение был удален.)

  ```json
  {
    "SendGridUser": "RickAndMSFT",
    "SendGridKey": "<key removed>"
  }
  ```

### <a name="configure-startup-to-use-authmessagesenderoptions"></a>Настройка запуска для использования AuthMessageSenderOptions

Добавить `AuthMessageSenderOptions` в контейнер службы, в конце `ConfigureServices` метод в *файла Startup.cs* файла:

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

[!code-csharp[Main](accconfirm/sample/WebPW/Startup.cs?name=snippet1&highlight=18)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)
[!code-csharp[Main](accconfirm/sample/WebApp1/Startup.cs?name=snippet1&highlight=26)]

---

### <a name="configure-the-authmessagesender-class"></a>Настроить класс AuthMessageSender

Этот учебник демонстрирует добавление уведомления по электронной почте через [SendGrid](https://sendgrid.com/), однако вы можете отправлять электронную почту с помощью SMTP и другие механизмы.

* Установить `SendGrid` пакет NuGet. Из консоли диспетчера пакетов введите следующую команду:

  `Install-Package SendGrid`

* В разделе [начните бесплатно с помощью SendGrid](https://sendgrid.com/free/) для регистрации бесплатной учетной записи SendGrid.

#### <a name="configure-sendgrid"></a>Настройка SendGrid

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

* Добавьте код в *Services/EmailSender.cs* аналогичный приведенному ниже, чтобы настроить SendGrid:

[!code-csharp[Main](accconfirm/sample/WebPW/Services/EmailSender.cs)]


# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)
* Добавьте код в *Services/MessageServices.cs* аналогичный приведенному ниже, чтобы настроить SendGrid:

[!code-csharp[Main](accconfirm/sample/WebApp1/Services/MessageServices.cs)]

---

## <a name="enable-account-confirmation-and-password-recovery"></a>Включить учетную запись, пароль и Подтверждение восстановления

Шаблон содержит код для восстановления и подтверждение пароля учетной записи. Найти `[HttpPost] Register` метод в *AccountController.cs* файла.

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

Запретить пользователям вновь зарегистрированного выполняется автоматический вход в систему, преобразуйте следующую строку:

```csharp 
await _signInManager.SignInAsync(user, isPersistent: false);
```

С измененной выделенной строке показан полный метод.

[!code-csharp[Main](accconfirm/sample/WebPW/Controllers/AccountController.cs?highlight=19&name=snippet_Register)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

Раскомментируйте код, чтобы включить подтверждение учетной записи.

[!code-csharp[Main](accconfirm/sample/WebApp1/Controllers/AccountController.cs?highlight=16-25&name=snippet_Register)]

Примечание: Мы также запрет зарегистрированный пользователю выполняется автоматический вход в систему, преобразуйте следующую строку:

```csharp 
//await _signInManager.SignInAsync(user, isPersistent: false);
```

Включить восстановление пароля Раскомментировать код в `ForgotPassword` действия в *Controllers/AccountController.cs* файла.

[!code-csharp[Main](accconfirm/sample/WebApp1/Controllers/AccountController.cs?highlight=17-23&name=snippet_ForgotPassword)]

Раскомментируйте элемента form в *Views/Account/ForgotPassword.cshtml*. Может потребоваться удалить `<p> For more information on how to enable reset password ... </p>` элемент, который содержит ссылку на эту статью.

[!code-html[Main](accconfirm/sample/WebApp1/Views/Account/ForgotPassword.cshtml?highlight=7-10,12,28)]

---

## <a name="register-confirm-email-and-reset-password"></a>Регистрация, подтверждение по электронной почте и сброс пароля

Запустите веб-приложения и тестов подтверждение учетной записи и пароль восстановления потока.

* Запустите приложение и регистрации нового пользователя

 ![Веб-приложения, зарегистрируйте учетную запись-представление](accconfirm/_static/loginaccconfirm1.png)

* Проверьте свою почту ссылку для подтверждения учетной записи. В разделе [Отладка электронной почты](#debug) Если вы не получаете сообщение электронной почты.
* Щелкните ссылку, чтобы подтвердить ваш адрес электронной почты.
* Войдите в систему электронной почты и пароль.
* Выйдите из системы.

### <a name="view-the-manage-page"></a>Страница управления представления

Выберите имя пользователя в браузере: ![окно браузера с именем пользователя](accconfirm/_static/un.png)

Может потребоваться развернуть панель навигации, чтобы увидеть имя пользователя.

![панель переходов](accconfirm/_static/x.png)

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

Страница управления отображается с **профиль** с выбранной вкладкой. **Электронной почты** показывает подтвердила типа "флажок", указывающее, сообщения электронной почты. 

![страница «Управление»](accconfirm/_static/rick2.png)


# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

Далее в этом учебнике говорится об этой странице.
![страница «Управление»](accconfirm/_static/rick2.png)

---

### <a name="test-password-reset"></a>Сброс пароля для тестирования

* Если вы выполнили вход, выберите **выхода**.  
* Выберите **входа** ссылку и выберите **забыли пароль?** ссылку.
* Введите адрес электронной почты, который использовался для регистрации учетной записи.
* Будет отправлено сообщение электронной почты со ссылкой для сброса пароля. Проверьте свою почту и щелкните ссылку, чтобы сбросить пароль.  После успешного сброса пароля можно выполнить вход с помощью электронной почты и новый пароль.

<a name="debug"></a>

### <a name="debug-email"></a>Отладка электронной почты

Если не удается получить рабочей электронной почты:

* Просмотрите [действия электронной почты](https://sendgrid.com/docs/User_Guide/email_activity.html) страницы.
* Проверьте папку нежелательной почты.
* Попробуйте другой псевдоним электронной почты на другой поставщик (Microsoft, Yahoo, Gmail, и т. д.)
* Создание [консольного приложения для отправки электронной почты](https://sendgrid.com/docs/Integrate/Code_Examples/v2_Mail/csharp.html).
* Повторите отправку учетным записям другое сообщение.

**Примечание:** безопасности рекомендуется не использовать секретные данные производства в тестирования и разработки. При публикации приложения в Azure, секреты SendGrid можно задать как параметры приложения на портале веб-приложения Azure. Система конфигурации настроен прочитать ключи из переменных среды.

## <a name="prevent-login-at-registration"></a>Запретить имени входа при регистрации

С шаблонами текущего пользователя по завершении формы регистрации, они регистрируются в (проверку подлинности). Как правило, требуется подтверждение перед ее регистрации электронной почте. В следующем разделе мы изменить код, чтобы требовать новых пользователей имеют подтверждения по электронной почте, прежде чем они регистрируются в. Обновление `[HttpPost] Login` действия в *AccountController.cs* файл с выделенной следующие изменения.

[!code-csharp[Main](accconfirm/sample/WebApp1/Controllers/AccountController.cs?highlight=11-21&name=snippet_Login)]

**Примечание:** безопасности рекомендуется не использовать секретные данные производства в тестирования и разработки. При публикации приложения в Azure, секреты SendGrid можно задать как параметры приложения на портале веб-приложения Azure. Система конфигурации настроен прочитать ключи из переменных среды.


## <a name="combine-social-and-local-login-accounts"></a>Объединение социальных сетей и локальных учетных записей

Примечание: Этот раздел относится только к ASP.NET Core 1.x. Для ASP.NET Core 2.x см. в разделе [это](https://github.com/aspnet/Docs/issues/3753) проблему.

Для выполнения в этом разделе, необходимо сначала включить внешнего поставщика аутентификации. В разделе [Включение проверки подлинности с использованием Facebook, Google и других внешних поставщиков](social/index.md).

Можно объединять учетные записи локальных и социальных сетей, щелкнув ссылку по электронной почте. В следующей последовательности «RickAndMSFT@gmail.com» сначала создается как локальное имя входа; тем не менее, можно сначала создать учетную запись входа на социальных сетей, а затем добавить локальный вход.

![Веб-приложения: RickAndMSFT@gmail.com пользователь прошел проверку подлинности](accconfirm/_static/rick.png)

Щелкните **управление** ссылку. Примечание внешних 0 (социальных имена входа), связанные с этой учетной записи.

![Управление представления](accconfirm/_static/manage.png)

Щелкните ссылку на другую службу входа в систему и принимать запросы приложений. В приведенном ниже рисунке Facebook — внешнего поставщика проверки подлинности:

![Управление листинг Facebook Просмотр внешних имен входа](accconfirm/_static/fb.png)

Объединены две учетные записи. Можно войти в систему с любой учетной записи. Может потребоваться пользователям добавлять локальные учетные записи, в случае их социальных вход службы проверки подлинности не работает, или что более вероятно, они утрачен доступ к учетной записи социальных сетей.
