---
title: Подтверждение учетной записи и восстановление пароля в ASP.NET Core
author: rick-anderson
description: Сведения о создании приложения ASP.NET Core с помощью по электронной почте подтверждение и сброс пароля.
ms.author: riande
ms.date: 2/11/2018
uid: security/authentication/accconfirm
ms.openlocfilehash: 12265903f60ff6d62befc445434db025c244c178
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 07/05/2018
ms.locfileid: "37803276"
---
# <a name="account-confirmation-and-password-recovery-in-aspnet-core"></a>Подтверждение учетной записи и восстановление пароля в ASP.NET Core

Авторы: [Рик Андерсон](https://twitter.com/RickAndMSFT) (Rick Anderson) и [Джо Одетт](https://twitter.com/joeaudette) (Joe Audette)

Этом руководстве показано, как создавать приложения ASP.NET Core с помощью по электронной почте подтверждение и сброс пароля. Это руководство представляет собой **не** начало раздела. Вы должны быть знакомы с:

* [ASP.NET Core](xref:tutorials/first-mvc-app/start-mvc)
* [Проверка подлинности](xref:security/authentication/index)
* [Entity Framework Core](xref:data/ef-mvc/intro)

См. в разделе [этот PDF-файл](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/authorization/secure-data/asp.net_repo_pdf_1-16-18.pdf) для версий ASP.NET Core MVC 1.1 и 2.x.

## <a name="prerequisites"></a>Предварительные требования

[!INCLUDE [](~/includes/net-core-prereqs.md)]

## <a name="create-a-new-aspnet-core-project-with-the-net-core-cli"></a>Создайте новый проект ASP.NET Core и .NET Core CLI

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

::: moniker range=">= aspnetcore-2.1"

```console
dotnet new webapp --auth Individual -o WebPWrecover
cd WebPWrecover
```

[!INCLUDE[](~/includes/webapp-alias-notice.md)]

::: moniker-end

::: moniker range="= aspnetcore-2.0"

```console
dotnet new razor --auth Individual -o WebPWrecover
cd WebPWrecover
```

::: moniker-end

* `--auth Individual` Указывает шаблон проекта учетные записи отдельных пользователей.
* На Windows, добавьте `-uld` параметр. Он указывает, что следует использовать LocalDB вместо SQLite.
* Запустите `new mvc --help` для получения справки по этой команде.

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

При использовании интерфейса командной строки или SQLite, в окне командной строки выполните следующую команду:

```console
dotnet new mvc --auth Individual
```

* `--auth Individual` Указывает шаблон проекта учетные записи отдельных пользователей.
* На Windows, добавьте `-uld` параметр. Он указывает, что следует использовать LocalDB вместо SQLite.
* Запустите `new mvc --help` для получения справки по этой команде.

---

Кроме того можно создать новый проект ASP.NET Core с помощью Visual Studio:

* В Visual Studio создайте новое **веб-приложение** проекта.
* Выберите **ASP.NET Core 2.0**. **.NET core** выбран на следующем рисунке, но вы можете выбрать **.NET Framework**.
* Выберите **изменить способ проверки подлинности** и присвоено **учетные записи отдельных пользователей**.
* Сохраните значение по умолчанию **Store учетных записей пользователей в приложении**.

![Отображение «Учетные записи отдельных пользователей radio» выбран диалоговое окно нового проекта](accconfirm/_static/2.png)

## <a name="test-new-user-registration"></a>Регистрация нового пользователя для тестирования

Запустите приложение, выберите **зарегистрировать** связать и регистрации пользователя. Следуйте инструкциям для выполнения миграции Entity Framework Core. На этом этапе является только проверка на адрес электронной почты с [[EmailAddress]](/dotnet/api/system.componentmodel.dataannotations.emailaddressattribute) атрибута. После отправки регистрации, вы вошли в приложение. Далее в этом руководстве код обновляется, поэтому новые пользователи не могут войти, пока не проверен доступ к электронной почте.

## <a name="view-the-identity-database"></a>Представление базы данных удостоверений

См. в разделе [работа с SQLite в проекте ASP.NET Core MVC](xref:tutorials/first-mvc-app-xplat/working-with-sql) инструкции о том, как просмотреть базу данных SQLite.

Для Visual Studio:

* Из **представление** меню, выберите **обозреватель объектов SQL Server** (SSOX).
* Перейдите к **(localdb) (SQL Server 13) MSSQLLocalDB**. Щелкните правой кнопкой мыши **dbo. AspNetUsers** > **просмотра данных**:

![Контекстные меню в таблице AspNetUsers в обозревателе объектов SQL Server](accconfirm/_static/ssox.png)

Обратите внимание, таблицы `EmailConfirmed` поле является `False`.

Вы можете использовать этот адрес электронной почты снова на следующем шаге, когда приложение отправляет сообщение электронной почты с подтверждением. Щелкните правой кнопкой мыши в строке и выберите **удалить**. Удаление псевдонима электронной почты упрощает в следующих шагах.

---

## <a name="require-https"></a>Требование HTTPS

См. в разделе [обязательного использования протокола HTTPS](xref:security/enforcing-ssl).

<a name="prevent-login-at-registration"></a>
## <a name="require-email-confirmation"></a>Требуется подтверждение по электронной почте

Рекомендуется подтвердить адрес электронной почты новой регистрации пользователя. Отправить по электронной почте подтверждение помогает проверить, они не олицетворения кто-то другой (то есть они еще не зарегистрировано другого пользователя по электронной почте). Предположим, что имеется дискуссионный форум и избежать "yli@example.com«из регистрации в качестве»nolivetto@contoso.com«. Без подтверждение по электронной почте "nolivetto@contoso.com" могли получать нежелательные сообщения электронной почты из вашего приложения. Предположим, что пользователь случайно зарегистрирован как "ylo@example.com" и не заметили опечатку «yli». Они не смогут использовать пароль восстановления, так как у приложения нет их правильный адрес электронной почты. Подтверждение по электронной почте предоставляет ограниченную защиту от программ-роботов. Подтверждение по электронной почте не обеспечивают защиту от пользователей-злоумышленников с многих учетных записей электронной почты.

Обычно требуется запретить новым пользователям из учета данных для веб-сайт, прежде чем они получат подтвержден по электронной почте.

Обновление `ConfigureServices` требуется подтвержден по электронной почте:

[!code-csharp[](accconfirm/sample/WebPWrecover/Startup.cs?name=snippet1&highlight=12-17)]

`config.SignIn.RequireConfirmedEmail = true;` запрещает вход до подтверждения их по электронной почте зарегистрированным пользователям.

### <a name="configure-email-provider"></a>Настройка поставщика услуг электронной почты

В этом руководстве SendGrid используется для отправки электронной почты. Требуется учетная запись SendGrid и ключ для отправки электронной почты. Можно использовать другие поставщики электронной почты. ASP.NET Core 2.x включает `System.Net.Mail`, который позволяет отправлять электронную почту из приложения. Мы рекомендуем использовать SendGrid или другая служба электронной почты для отправки электронной почты. Для защиты и правильно настроить сложно SMTP.

[Шаблон параметров](xref:fundamentals/configuration/options) используется для доступа к параметрам и ключ учетной записи пользователя. Дополнительные сведения см. в разделе [конфигурации](xref:fundamentals/configuration/index).

Создание класса для извлечения ключа защиты электронной почты. В этом примере `AuthMessageSenderOptions` класс создается в *Services/AuthMessageSenderOptions.cs* файла:

[!code-csharp[](accconfirm/sample/WebPWrecover/Services/AuthMessageSenderOptions.cs?name=snippet1)]

Задайте `SendGridUser` и `SendGridKey` с [средство secret manager](xref:security/app-secrets). Пример:

```console
C:\WebAppl\src\WebApp1>dotnet user-secrets set SendGridUser RickAndMSFT
info: Successfully saved SendGridUser = RickAndMSFT to the secret store.
```

На Windows, Secret Manager хранит пары ключей и значений в *secrets.json* файл `%APPDATA%/Microsoft/UserSecrets/<WebAppName-userSecretsId>` каталог.

Содержание *secrets.json* файл не зашифрован. *Secrets.json* ниже приведен файл ( `SendGridKey` значение удаляется.)

 ```json
  {
    "SendGridUser": "RickAndMSFT",
    "SendGridKey": "<key removed>"
  }
  ```

### <a name="configure-startup-to-use-authmessagesenderoptions"></a>Настройка запуска для использования AuthMessageSenderOptions

Добавить `AuthMessageSenderOptions` в контейнер службы, в конце `ConfigureServices` метод в *Startup.cs* файла:

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x/)

[!code-csharp[](accconfirm/sample/WebPWrecover/Startup.cs?name=snippet2&highlight=28)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x/)

[!code-csharp[](accconfirm/sample/WebApp1/Startup.cs?name=snippet1&highlight=26)]

---

### <a name="configure-the-authmessagesender-class"></a>Настроить класс AuthMessageSender

Этот учебник демонстрирует добавление уведомлений по электронной почте через [SendGrid](https://sendgrid.com/), но вы можете отправлять электронную почту с помощью SMTP и другие механизмы.

Установить `SendGrid` пакет NuGet:

* Из командной строки:

    `dotnet add package SendGrid`

* В консоли диспетчера пакетов введите следующую команду:

  `Install-Package SendGrid`

См. в разделе [приступить к работе с SendGrid бесплатно](https://sendgrid.com/free/) зарегистрироваться для получения бесплатной учетной записи SendGrid.

#### <a name="configure-sendgrid"></a>Настройка SendGrid

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x/)

Чтобы настроить SendGrid, добавьте код, аналогичный следующему *Services/EmailSender.cs*:

[!code-csharp[](accconfirm/sample/WebPWrecover/Services/EmailSender.cs)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x/)

* Добавьте код в *Services/MessageServices.cs* Настройка SendGrid следующего вида:

[!code-csharp[](accconfirm/sample/WebApp1/Services/MessageServices.cs)]

---

## <a name="enable-account-confirmation-and-password-recovery"></a>Включить учетную запись, пароль и Подтверждение восстановления

Шаблон содержит код для восстановления и подтверждение пароля учетной записи. Найти `OnPostAsync` метод в *Pages/Account/Register.cshtml.cs*.

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x/)

Запретить пользователям только что зарегистрированное автоматически входит в систему, закомментируйте следующую строку:

```csharp
await _signInManager.SignInAsync(user, isPersistent: false);
```

С помощью измененные строки выделены показан полный метод:

[!code-csharp[](accconfirm/sample/WebPWrecover/Pages/Account/Register.cshtml.cs?highlight=16&name=snippet_Register)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x/)

Чтобы включить подтверждение учетной записи, раскомментируйте следующий код:

[!code-csharp[](accconfirm/sample/WebApp1/Controllers/AccountController.cs?highlight=16-25&name=snippet_Register)]

**Примечание:** код предотвращает вновь зарегистрированного пользователя автоматически входит в систему, закомментируйте следующую строку:

```csharp
//await _signInManager.SignInAsync(user, isPersistent: false);
```

Включить восстановление пароля Раскомментировать код в `ForgotPassword` действие *Controllers/AccountController.cs*:

[!code-csharp[](accconfirm/sample/WebApp1/Controllers/AccountController.cs?highlight=17-23&name=snippet_ForgotPassword)]

Раскомментируйте элемент формы в *Views/Account/ForgotPassword.cshtml*. Может потребоваться удалить `<p> For more information on how to enable reset password ... </p>` элемент, который содержится ссылка в этой статье.

[!code-cshtml[](accconfirm/sample/WebApp1/Views/Account/ForgotPassword.cshtml?highlight=7-10,12,28)]

---

## <a name="register-confirm-email-and-reset-password"></a>Регистрация, подтверждение электронной почты и сброс пароля

Запустите веб-приложение и протестируйте подтверждение учетной записи и процесс восстановления пароля.

* Запустите приложение и зарегистрируйте нового пользователя

  ![Веб-приложения, зарегистрируйте учетную запись-представление](accconfirm/_static/loginaccconfirm1.png)

* Проверьте свой адрес электронной почты для ссылку для подтверждения учетной записи. См. в разделе [отладки электронной почты](#debug) Если вы не получили сообщение электронной почты.
* Щелкните ссылку, чтобы подтвердить свой адрес электронной почты.
* Войдите с помощью электронной почты и пароль.
* Выйдите из системы.

### <a name="view-the-manage-page"></a>Просмотреть страницу управление

Выберите свое имя пользователя в браузере: ![окно браузера с именем пользователя](accconfirm/_static/un.png)

Может потребоваться развернуть панель навигации, чтобы увидеть имя пользователя.

![панель переходов](accconfirm/_static/x.png)

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

Управление страница отображается с **профиль** выделенной вкладки. **Электронной почты** показано типа "флажок", указывающее, адрес электронной почты будет подтверждена.

![страница «Управление»](accconfirm/_static/rick2.png)

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

Это упоминается позже в этом руководстве.
![страница «Управление»](accconfirm/_static/rick2.png)

---

### <a name="test-password-reset"></a>Сброс пароля теста

* Если вы выполнили вход в, выберите **выхода**.
* Выберите **вход** ссылку и выберите **забыли пароль?** ссылку.
* Введите адрес электронной почты, которая использовалась для регистрации учетной записи.
* Отправляется сообщение электронной почты со ссылкой для сброса пароля. Проверьте свой адрес электронной почты и щелкните ссылку, чтобы сбросить пароль. После успешного сброса пароля вы можете войти с использованием электронной почты и новый пароль.

<a name="debug"></a>

### <a name="debug-email"></a>Отладка по электронной почте

Если не удается получить рабочий адрес электронной почты:

* Создание [консольное приложение для отправки электронной почты](https://sendgrid.com/docs/Integrate/Code_Examples/v2_Mail/csharp.html).
* Просмотрите [действия "сообщения"](https://sendgrid.com/docs/User_Guide/email_activity.html) страницы.
* Проверьте папку нежелательной почты.
* Попробуйте другой псевдоним электронной почты на другой поставщик электронной почты (Microsoft, Yahoo, Gmail, и т.д.)
* Попробуйте отправить учетными записями электронной почты.

**Рекомендации по безопасности** — **не** используйте секреты производства в разработки и тестирования. При публикации приложения в Azure, секреты SendGrid можно задать как параметры приложения на портале веб-приложения Azure. Система конфигурации предназначена для чтения разделов из переменных среды.

## <a name="combine-social-and-local-login-accounts"></a>Объединить учетные записи социальных сетей и локального имени входа

Для завершения этого раздела необходимо сначала включить внешний поставщик аутентификации. См. в разделе [Facebook, Google и внешнего поставщика проверки подлинности](xref:security/authentication/social/index).

Можно объединить учетные записи локальных и социальных сетей, щелкнув ссылку по электронной почте. В следующей последовательности «RickAndMSFT@gmail.com"сначала создается как локальное имя входа; тем не менее, можно сначала создать учетную запись как имя для входа социальных сетей, а затем добавить локальное имя входа.

![Веб-приложения: RickAndMSFT@gmail.com пользователь прошел проверку подлинности](accconfirm/_static/rick.png)

Щелкните **управление** ссылку. Примечание внешний 0 (имена входа социальных сетей), связанное с этой учетной записи.

![Управления представлением](accconfirm/_static/manage.png)

Щелкните ссылку на другую службу входа в систему и принимать запросы приложений. На следующем рисунке Facebook, — это внешнего поставщика проверки подлинности:

![Управление ваш список внешних имен входа Facebook](accconfirm/_static/fb.png)

Две учетные записи были объединены. Вы сможете выполнить вход с использованием либо учетной записи. Вы можете добавить локальные учетные записи, в случае их учетные записи социальных сетей службы проверки подлинности не работает, или более вероятно, они потеряли доступ к своей учетной записи социальной сети пользователей.

## <a name="enable-account-confirmation-after-a-site-has-users"></a>Включить подтверждение учетной записи, после узла имеет пользователей

Включение подтверждение учетной записи на сайте с пользователями блокирует работу всех существующих пользователей. Существующие пользователи заблокированы, так как учетные записи не подтверждены. Чтобы обойти существующие блокировки пользователя, используйте один из следующих подходов:

* Обновите базу данных для пометки всех существующих пользователей в качестве подтверждения.
* Подтвердите существующих пользователей. Например пакетная Отправка сообщения электронной почты с подтверждением ссылки.
