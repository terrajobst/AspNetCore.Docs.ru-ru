* Настройте доверие сертификату разработки HTTPS с помощью следующей команды:

    ```console
    dotnet dev-certs https --trust
    ```

* Приведенная выше команда отображает следующие выходные данные:

    ```console
    Trusting the HTTPS development certificate was requested. If the certificate 
    is not already trusted we will run the following command:
    'sudo security add-trusted-cert -d -r trustRoot -k /Library/Keychains/System.keychain 
    <<certificate>>'
    This command might prompt you for your password to install the certificate on the 
    system keychain.
    The HTTPS developer certificate was generated successfully.
    ```

* При появлении соответствующего запроса введите имя пользователя и пароль администратора.  Установленный сертификат будет считаться доверенным.

    Дополнительные сведения см. в разделе [Настройка доверия к сертификату разработки HTTPS ASP.NET Core](xref:security/enforcing-ssl#trust-the-aspnet-core-https-development-certificate-on-windows-and-macos).