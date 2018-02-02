# <a name="custom-webhost-service-sample"></a>Образец пользовательского WebHost службы

В этом примере показан способ, рекомендуемый для размещения приложения ASP.NET Core в Windows без использования IIS в качестве службы Windows. В этом примере демонстрируется функции, описанные в [размещения приложения ASP.NET Core в службе Windows](https://docs.microsoft.com/aspnet/core/host-and-deploy/windows-service).

## <a name="instructions"></a>Инструкции

Пример приложения является простой веб-приложения MVC изменить в соответствии с инструкциями в [размещения приложения ASP.NET Core в службе Windows](https://docs.microsoft.com/aspnet/core/host-and-deploy/windows-service).

Чтобы запустить приложение в службе, выполните следующие действия.

1. Создать папку на *c:\svc*.

1. Опубликуйте приложение в папку с `dotnet publish --configuration Release --output c:\\svc`. Команда переместит активы приложения для папки, включая необходимые `appsettings.json` файла и `wwwroot` папки с его содержимым.

1. Откройте **администратора** командной оболочке.

1. Выполните следующие команды:

   ```console
   sc create MyService binPath="c:\svc\aspnetcoreservice.exe"
   sc start MyService
   ```

1. В браузере, перейдите к `http://localhost:5000` чтобы убедиться, что служба запущена.

1. Чтобы остановить службу, используйте команду:

   ```console
   sc stop MyService
   ```

Если приложение не запускается до должным образом при запуске в службе, быстрый способ сделать доступной сообщения об ошибках является добавление регистратора, таких как [поставщик журнала событий Windows](https://docs.microsoft.com/aspnet/core/fundamentals/logging/index#eventlog). Другой вариант — Проверьте журнал событий приложений, с помощью средства просмотра событий в системе. Например вот необработанное исключение для появление ошибки в журнале событий приложений.

```console
Application: AspNetCoreService.exe
Framework Version: v4.0.30319
Description: The process was terminated due to an unhandled exception.
Exception Info: System.IO.FileNotFoundException
   at Microsoft.Extensions.Configuration.FileConfigurationProvider.Load(Boolean)
   at Microsoft.Extensions.Configuration.ConfigurationRoot..ctor(System.Collections.Generic.IList`1<Microsoft.Extensions.Configuration.IConfigurationProvider>)
   at Microsoft.Extensions.Configuration.ConfigurationBuilder.Build()
   ...
```
