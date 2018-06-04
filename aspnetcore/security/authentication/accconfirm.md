---
title: Подтверждение учетной записи и пароль восстановления в ASP.NET Core
author: rick-anderson
description: Сведения о создании приложения ASP.NET Core с помощью сброса пароля и подтверждение по электронной почте.
manager: wpickett
ms.author: riande
ms.date: 2/11/2018
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: security/authentication/accconfirm
ms.openlocfilehash: b6dbe234973431448c18d3cc82a6ac98d4f53a3b
ms.sourcegitcommit: 43bd79667bbdc8a07bd39fb4cd6f7ad3e70212fb
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 06/04/2018
ms.locfileid: "34730455"
---
# <a name="account-confirmation-and-password-recovery-in-aspnet-core"></a>Подтверждение учетной записи и пароль восстановления в ASP.NET Core

Авторы: [Рик Андерсон](https://twitter.com/RickAndMSFT) (Rick Anderson) и [Джо Одетт](https://twitter.com/joeaudette) (Joe Audette)

Этого учебника показано, как создать приложение ASP.NET Core с помощью сброса пароля и подтверждение по электронной почте. Этот учебник содержит **не** начало раздела. Вы должны быть знакомы с:

* [ASP.NET Core](xref:tutorials/first-mvc-app/start-mvc)
* [Проверка подлинности](xref:security/authentication/index)
* [Подтверждение учетной записи и восстановление пароля](xref:security/authentication/accconfirm)
* [Entity Framework Core](xref:data/ef-mvc/intro)

В разделе [этот файл PDF](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/authorization/secure-data/asp.net_repo_pdf_1-16-18.pdf) для версии ASP.NET Core MVC 1.1 и 2.x.

## <a name="prerequisites"></a>Предварительные требования

[!INCLUDE [](~/includes/net-core-prereqs.md)]

## <a name="create-a-new-aspnet-core-project-with-the-net-core-cli"></a>Создайте новый проект ASP.NET Core с .NET Core CLI

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

::: moniker range=">= aspnetcore-2.1"

```console
dotnet new webapp --auth Individual -o WebPWrecover
cd WebPWrecover
```

::: moniker-end

::: moniker range="= aspnetcore-2.0"

```console
dotnet new razor --auth Individual -o WebPWrecover
cd WebPWrecover
```

::: moniker-end

* `--auth Individual` Указывает шаблон проекта отдельных учетных записей пользователей.
* В Windows, добавить `-uld` параметр. Он указывает, что LocalDB должен использоваться вместо SQLite.
* Запустите `new mvc --help` для получения справки по этой команде.

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

Если вы используете CLI или SQLite, в окно командной строки выполните следующую команду:

```console
dotnet new mvc --auth Individual
```

* `--auth Individual` Указывает шаблон проекта отдельных учетных записей пользователей.
* В Windows, добавить `-uld` параметр. Он указывает, что LocalDB должен использоваться вместо SQLite.
* Запустите `new mvc --help` для получения справки по этой команде.

---

Кроме того можно создать новый проект ASP.NET Core с помощью Visual Studio:

* В Visual Studio создайте новый **веб-приложение** проекта.
* Выберите **ASP.NET Core 2.0**. **.NET core** установлен на следующем рисунке, но вы можете выбрать **.NET Framework**.
* Выберите **изменить аутентификацию** и задайте **индивидуальные учетные записи**.
* Оставьте значение по умолчанию **хранилище учетные записи в приложении**.

![Отображение «Radio отдельных учетных записей пользователей» выбран диалоговое окно нового проекта](accconfirm/_static/2.png)

## <a name="test-new-user-registration"></a>Регистрация нового пользователя теста

Запустите приложение, выберите **зарегистрировать** ссылку и регистрации пользователя. Следуйте инструкциям для выполнения миграции Entity Framework Core. На этом этапе является проверку только в электронном письме с [[EmailAddress]](/dotnet/api/system.componentmodel.dataannotations.emailaddressattribute) атрибута. После прохождения процедуры регистрации, вы вошли в приложение. Далее в этом учебнике код обновляется, таким образом, новые пользователи не могут войти пока не был проверен электронной почте.

## <a name="view-the-identity-database"></a>Представление базы данных удостоверений

В разделе [работать с SQLite в проект ASP.NET Core MVC](xref:tutorials/first-mvc-app-xplat/working-with-sql) инструкции о том, как просмотреть базу данных SQLite.

Для Visual Studio:

* Из **представление** последовательно выберите пункты **обозреватель объектов SQL Server** (SSOX).
* Перейдите к **(localdb) (SQL Server 13) MSSQLLocalDB**. Щелкните правой кнопкой мыши **dbo. AspNetUsers** > **просмотра данных**:

![Контекстное меню таблицы AspNetUsers в обозревателе объектов SQL Server](accconfirm/_static/ssox.png)

Обратите внимание, таблицы `EmailConfirmed` поле является `False`.

Можно использовать это сообщение электронной почты снова на следующем шаге, когда приложение отправляет сообщение с подтверждением. Щелкните правой кнопкой мыши в строке и выберите **удалить**. Удаление псевдонимов электронной почты помогает в следующих шагах.

---

## <a name="require-https"></a>Требовать использования протокола HTTPS

В разделе [обязательного использования протокола HTTPS](xref:security/enforcing-ssl).

<a name="prevent-login-at-registration"></a>
## <a name="require-email-confirmation"></a>Требовать подтверждения по электронной почте

Рекомендуется для подтверждения адреса электронной почты Регистрация нового пользователя. Отправить по электронной почте подтверждение помогает проверить, они не использовании олицетворения кто-то другой (то есть они еще не зарегистрированы с помощью электронной почты другого пользователя). Предположим, что имеется дискуссионный форум и избежать "yli@example.com«от регистрации в качестве»nolivetto@contoso.com». Без подтверждения по электронной почте»nolivetto@contoso.com» может получать нежелательные сообщения от приложения. Предположим, что пользователь случайно зарегистрированы как «ylo@example.com» и не заметили ошибочное «yli». Они не смогут использовать пароль восстановления, так как у приложения нет электронной почте правильно. Подтверждение по электронной почте предоставляет ограниченную защиту от программы-роботы. Подтверждение по электронной почте не обеспечивает защиты от злоумышленников с многих учетные записи электронной почты.

Как правило, требуется запретить пользователям новых учетных данных для веб-узла до того, как они подтверждения по электронной почте.

Обновление `ConfigureServices` требовать подтверждения по электронной почте:

[!code-csharp[](accconfirm/sample/WebPWrecover/Startup.cs?name=snippet1&highlight=12-17)]

`config.SignIn.RequireConfirmedEmail = true;` зарегистрированные пользователю войти в систему, пока не будет подтверждена электронной почте.

### <a name="configure-email-provider"></a>Настройка поставщика услуг электронной почты

В этом учебнике SendGrid используется для отправки электронной почты. Требуется учетной записи SendGrid и ключ для отправки электронной почты. Можно использовать другие поставщики электронной почты. Включает 2.x ASP.NET Core `System.Net.Mail`, позволяющий отправлять электронную почту из приложения. Мы рекомендуем использовать SendGrid или другая служба электронной почты для отправки электронной почты. Для защиты и правильно настроить сложно SMTP.

[Параметры шаблона](xref:fundamentals/configuration/options) используется для доступа к параметрам учетной записи и ключа пользователя. Дополнительные сведения см. в разделе [конфигурации](xref:fundamentals/configuration/index).

Создание класса для получения ключа защиты электронной почты. Для этого образца `AuthMessageSenderOptions` класса создается в *Services/AuthMessageSenderOptions.cs* файла:

[!code-csharp[](accconfirm/sample/WebPWrecover/Services/AuthMessageSenderOptions.cs?name=snippet1)]

Задать `SendGridUser` и `SendGridKey` с [секрет диспетчера](xref:security/app-secrets). Пример:

```console
C:\WebAppl\src\WebApp1>dotnet user-secrets set SendGridUser RickAndMSFT
info: Successfully saved SendGridUser = RickAndMSFT to the secret store.
```

В Windows, диспетчер секрет хранит пары ключей и значений в *secrets.json* файл `%APPDATA%/Microsoft/UserSecrets/<WebAppName-userSecretsId>` каталог.

Содержимое *secrets.json* файл не зашифрован. *Secrets.json* ниже приведен файл ( `SendGridKey` значение был удален.)

 ```json
  {
    "SendGridUser": "RickAndMSFT",
    "SendGridKey": "<key removed>"
  }
  ```

### <a name="configure-startup-to-use-authmessagesenderoptions"></a>Настройка запуска для использования AuthMessageSenderOptions

Добавить `AuthMessageSenderOptions` в контейнер службы, в конце `ConfigureServices` метод в *файла Startup.cs* файла:

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x/)

[!code-csharp[](accconfirm/sample/WebPWrecover/Startup.cs?name=snippet2&highlight=28)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x/)

[!code-csharp[](accconfirm/sample/WebApp1/Startup.cs?name=snippet1&highlight=26)]

---

### <a name="configure-the-authmessagesender-class"></a>Настроить класс AuthMessageSender

Этот учебник демонстрирует добавление уведомления по электронной почте через [SendGrid](https://sendgrid.com/), однако вы можете отправлять электронную почту с помощью SMTP и другие механизмы.

Установить `SendGrid` пакет NuGet:

* Из командной строки:

    `dotnet add package SendGrid`

* В консоли диспетчера пакетов введите следующую команду:

  `Install-Package SendGrid`

В разделе [начните бесплатно с помощью SendGrid](https://sendgrid.com/free/) для регистрации бесплатной учетной записи SendGrid.

#### <a name="configure-sendgrid"></a>Настройка SendGrid

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x/)

Чтобы настроить SendGrid, добавьте код, аналогичный следующему *Services/EmailSender.cs*:

[!code-csharp[](accconfirm/sample/WebPWrecover/Services/EmailSender.cs)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x/)

* Добавьте код в *Services/MessageServices.cs* аналогичный приведенному ниже, чтобы настроить SendGrid:

[!code-csharp[](accconfirm/sample/WebApp1/Services/MessageServices.cs)]

---

## <a name="enable-account-confirmation-and-password-recovery"></a>Включить учетную запись, пароль и Подтверждение восстановления

Шаблон содержит код для восстановления и подтверждение пароля учетной записи. Найти `OnPostAsync` метод в *Pages/Account/Register.cshtml.cs*.

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x/)

Запретить пользователям вновь зарегистрированного выполняется автоматический вход в систему, преобразуйте следующую строку:

```csharp
await _signInManager.SignInAsync(user, isPersistent: false);
```

С измененной выделенной строке показан полный метод.

[!code-csharp[](accconfirm/sample/WebPWrecover/Pages/Account/Register.cshtml.cs?highlight=16&name=snippet_Register)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x/)

Чтобы включить подтверждение учетной записи, раскомментируйте следующий код:

[!code-csharp[](accconfirm/sample/WebApp1/Controllers/AccountController.cs?highlight=16-25&name=snippet_Register)]

**Примечание:** код препятствует вновь зарегистрированного пользователя выполняется автоматический вход в систему, преобразуйте следующую строку:

```csharp
//await _signInManager.SignInAsync(user, isPersistent: false);
```

Включить восстановление пароля Раскомментировать код в `ForgotPassword` действие *Controllers/AccountController.cs*:

[!code-csharp[](accconfirm/sample/WebApp1/Controllers/AccountController.cs?highlight=17-23&name=snippet_ForgotPassword)]

Раскомментируйте элемента form в *Views/Account/ForgotPassword.cshtml*. Может потребоваться удалить `<p> For more information on how to enable reset password ... </p>` элемент, который содержит ссылку на эту статью.

[!code-cshtml[](accconfirm/sample/WebApp1/Views/Account/ForgotPassword.cshtml?highlight=7-10,12,28)]

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

Это упоминается далее в этом учебнике.
![страница «Управление»](accconfirm/_static/rick2.png)

---

### <a name="test-password-reset"></a>Сброс пароля для тестирования

* Если вы выполнили вход, выберите **выхода**.
* Выберите **входа** ссылку и выберите **забыли пароль?** ссылку.
* Введите адрес электронной почты, который использовался для регистрации учетной записи.
* Отправляется сообщение электронной почты со ссылкой для сброса пароля. Проверьте свою почту и щелкните ссылку, чтобы сбросить пароль. После успешного сброса пароля можно войти электронной почты и новый пароль.

<a name="debug"></a>

### <a name="debug-email"></a>Отладка электронной почты

Если не удается получить рабочей электронной почты:

* Создание [консольного приложения для отправки электронной почты](https://sendgrid.com/docs/Integrate/Code_Examples/v2_Mail/csharp.html).
* Просмотрите [действия электронной почты](https://sendgrid.com/docs/User_Guide/email_activity.html) страницы.
* Проверьте папку нежелательной почты.
* Попробуйте другой псевдоним электронной почты на другой поставщик (Microsoft, Yahoo, Gmail, и т. д.)
* Повторите отправку учетным записям другое сообщение.

**Рекомендации по безопасности** — **не** используйте секреты производства в тестирования и разработки. При публикации приложения в Azure, секреты SendGrid можно задать как параметры приложения на портале веб-приложения Azure. Система конфигурации настраивается для чтения ключи из переменных среды.

## <a name="combine-social-and-local-login-accounts"></a>Объединение социальных сетей и локальных учетных записей

Для выполнения в этом разделе, необходимо сначала включить внешнего поставщика аутентификации. В разделе [Facebook, Google и внешнего поставщика проверки подлинности](xref:security/authentication/social/index).

Можно объединять учетные записи локальных и социальных сетей, щелкнув ссылку по электронной почте. В следующей последовательности «RickAndMSFT@gmail.com» сначала создается как локальное имя входа; тем не менее, можно сначала создать учетную запись входа на социальных сетей, а затем добавить локальный вход.

![Веб-приложения: RickAndMSFT@gmail.com пользователь прошел проверку подлинности](accconfirm/_static/rick.png)

Щелкните **управление** ссылку. Примечание внешних 0 (социальных имена входа), связанные с этой учетной записи.

![Управление представления](accconfirm/_static/manage.png)

Щелкните ссылку на другую службу входа в систему и принимать запросы приложений. На следующем рисунке Facebook является внешнего поставщика проверки подлинности:

![Управление листинг Facebook Просмотр внешних имен входа](accconfirm/_static/fb.png)

Объединены две учетные записи. Имеется возможность войти в систему с любой учетной записи. Может потребоваться пользователям добавлять локальные учетные записи, в случае их службы проверки подлинности имени входа социальных сетей не работает или более вероятно, они потеряли доступ к учетной записи социальных сетей.

## <a name="enable-account-confirmation-after-a-site-has-users"></a>Включить подтверждение учетной записи после сайт имеет пользователей

Включение подтверждение учетной записи на узле с пользователями блокирует работу всех существующих пользователей. Существующие пользователи заблокированы, так как учетные записи не подтверждено. Чтобы обойти существующие блокировки пользователя, используйте один из следующих подходов:

* Обновите базу данных для пометки всех существующих пользователей как подтверждения.
* Подтвердите для существующих пользователей. Например пакет отправки сообщения электронной почты с подтверждением ссылки.
