---
title: Verschlüsselte Konnektivität mit TLS/SSL in Azure Database for MySQL – Flexible Server
description: Hier finden Sie Anweisungen und Informationen zum Herstellen einer Verbindung unter Verwendung von TLS/SSL in Azure Database for MySQL – Flexible Server.
author: ambhatna
ms.author: ambhatna
ms.service: mysql
ms.topic: conceptual
ms.date: 09/21/2020
ms.openlocfilehash: 24a8dd4d21cb6ab6edeb985db4e6e6a1349a758d
ms.sourcegitcommit: 53acd9895a4a395efa6d7cd41d7f78e392b9cfbe
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/22/2020
ms.locfileid: "90931749"
---
# <a name="encrypted-connectivity-using-transport-layer-security-tls-12-in-azure-database-for-mysql---flexible-server"></a>Verschlüsselte Konnektivität mit der Transport Layer Security (TLS 1.2) in Azure Database for MySQL – Flexible Server

> [!IMPORTANT]
> Azure Database for MySQL Flexible Server befindet sich aktuell in der öffentlichen Vorschau.

Azure Database for MySQL Flexible Server unterstützt das Herstellen einer Verbindung zwischen Ihren Clientanwendungen und dem MySQL-Dienst über TLS (Transport Layer Security), ehemals als SSL (Secure Sockets Layer) bezeichnet. TLS ist ein Standardprotokoll der Branche, das verschlüsselte Netzwerkverbindungen zwischen dem Datenbankserver und Clientanwendungen gewährleistet, sodass Sie Konformitätsanforderungen einhalten können.

Azure Database for MySQL Flexible Server unterstützt nur verschlüsselte Verbindungen mit Transport Layer Security (TLS 1.2), und alle eingehenden Verbindungen mit TLS 1.0 und TLS 1.1 werden verweigert. Für alle Flexible Server-Instanzen ist die Durchsetzung von TLS-Verbindungen aktiviert, und Sie können TLS/SSL für die Verbindung zur Flexible Server-Instanz nicht deaktivieren.

## <a name="applications-that-require-certificate-verification-for-tlsssl-connectivity"></a>Anwendungen, die eine Zertifikatüberprüfung für TLS/SSL-Verbindungen erfordern
In einigen Fällen erfordern Anwendungen eine lokale Zertifikatdatei, die auf der Grundlage der Zertifikatdatei einer vertrauenswürdigen Zertifizierungsstelle (Certificate Authority, CA) generiert wurde, um eine sichere Verbindung herzustellen. Azure Database for MySQL Flexible Server verwendet *DigiCert Global Root CA*. Laden Sie dieses für die Kommunikation über SSL erforderliche Zertifikat von [DigiCert Global Root CA](https://dl.cacerts.digicert.com/DigiCertGlobalRootCA.crt.pem) herunter, und speichern Sie die Zertifikatsdatei an Ihrem bevorzugten Speicherort. In diesem Tutorial wird beispielsweise `c:\ssl` verwendet.

### <a name="connect-using-mysql-command-line-client-with-tlsssl"></a>Herstellen einer Verbindung mithilfe des mysql-Befehlszeilenclients mit TLS/SSL

Wenn Sie Ihre Flexible Server-Instanz mit *Privater Zugriff (VNET-Integration)* erstellt haben, müssen Sie eine Verbindung mit Ihrem Server über eine Ressource innerhalb desselben virtuellen Netzwerks herstellen. Sie können einen virtuellen Computer erstellen und zum virtuellen Netzwerk hinzufügen, das mit Ihrer Flexible Server-Instanz erstellt wurde.

Wenn Sie Ihre Flexible Server-Instanz mit *Öffentlicher Zugriff (zulässige IP-Adressen)* erstellt haben, können Sie die lokale IP-Adresse der Liste der Firewallregeln auf Ihrem Server hinzufügen.

Sie können [mysql.exe](https://dev.mysql.com/doc/refman/8.0/en/mysql.html) oder [MySQL Workbench](./connect-workbench.md) auswählen, um aus Ihrer lokalen Umgebung eine Verbindung mit dem Server herzustellen. 

Das folgende Beispiel zeigt, wie Sie einen Server mithilfe der mysql-Befehlszeilenschnittstelle verbinden können. Verwenden Sie die Einstellung `--ssl-mode=REQUIRED` für die Verbindungszeichenfolge, um die Überprüfung des TLS-/SSL-Zertifikats zu erzwingen. Übergeben Sie den Pfad der lokalen Zertifikatdatei an den Parameter `--ssl-ca`. Ersetzen Sie die Werte durch den tatsächlichen Servernamen und das Kennwort. 

```bash
 mysql.exe -h mydemoserver.mysql.database.azure.com -u myadmin -p --ssl-mode=REQUIRED --ssl-ca=c:\ssl\DigiCertGlobalRootCA.crt.pem
```
> [!Note]
> Vergewissern Sie sich, dass der Wert, den Sie an `--ssl-ca` übergeben haben, mit dem Dateipfad für das von Ihnen gespeicherte Zertifikat übereinstimmt.

### <a name="verify-the-tlsssl-connection"></a>Überprüfen der TLS/SSL-Verbindung

Führen Sie die MySQL-Befehl **status** aus, um sicherzustellen, dass die Verbindung mit dem MySQL-Server über TLS/SSL hergestellt wurde:

```dos
mysql> status
```
Um sicherzustellen, dass die Verbindung verschlüsselt ist, überprüfen Sie die Ausgabe, die Folgendes anzeigen sollte:  **SSL: Verwendete Verschlüsselung ist AES256-SHA**. Diese Verschlüsselungssammlung zeigt ein Beispiel und basierend auf dem Client können Sie eine andere Verschlüsselungssammlung sehen.

## <a name="ensure-your-application-or-framework-supports-tls-connections"></a>Sicherstellen, dass die Anwendung oder das Framework TLS-Verbindungen unterstützt

Einige Anwendungsframeworks, die MySQL für ihre Datenbankdienste verwenden, aktivieren TLS nicht standardmäßig während der Installation. Ihr MySQL-Server erzwingt TLS-Verbindungen. Wenn die Anwendung aber nicht für TLS konfiguriert wurde, kann die Anwendung unter Umständen keine Verbindung mit Ihrem Datenbankserver herstellen. Lesen Sie in der Dokumentation Ihrer Anwendung nach, wie TLS-Verbindungen aktiviert werden.

## <a name="sample-code"></a>Beispielcode
Entsprechend enthalten Verbindungszeichenfolgen, die im Azure-Portal unter dem Server auf der Seite „Verbindungszeichenfolgen“ vorab definiert sind, die erforderlichen Parameter für gängige Sprachen für die Verbindung mit dem Datenbankserver mithilfe von TLS/SSL. Der TLS/SSL-Parameter variiert je nach Connector. Beispiel: "useSSL=true", "sslmode=required" oder "ssl_verify_cert=true" und andere Variationen.

Sie können die folgenden Codebeispiele nutzen, um in Ihrer Anwendung über TLS/SSL eine verschlüsselte Verbindung mit Ihrer Flexible Server-Instanz einzurichten:

### <a name="php"></a>PHP

```php
$conn = mysqli_init();
mysqli_ssl_set($conn,NULL,NULL, "/var/www/html/DigiCertGlobalRootCA.crt.pem", NULL, NULL);
mysqli_real_connect($conn, 'mydemoserver.mysql.database.azure.com', 'myadmin', 'yourpassword', 'quickstartdb', 3306, MYSQLI_CLIENT_SSL);
if (mysqli_connect_errno($conn)) {
die('Failed to connect to MySQL: '.mysqli_connect_error());
}
```

### <a name="php-using-pdo"></a>PHP (mit PDO)

```phppdo
$options = array(
    PDO::MYSQL_ATTR_SSL_CA => '/var/www/html/DigiCertGlobalRootCA.crt.pem'
);
$db = new PDO('mysql:host=mydemoserver.mysql.database.azure.com;port=3306;dbname=databasename', 'myadmin', 'yourpassword', $options);
```

### <a name="python-mysqlconnector-python"></a>Python (MySQLConnector Python)

```python
try:
    conn = mysql.connector.connect(user='myadmin',
                                   password='yourpassword',
                                   database='quickstartdb',
                                   host='mydemoserver.mysql.database.azure.com',
                                   ssl_ca='/var/www/html/DigiCertGlobalRootCA.crt.pem')
except mysql.connector.Error as err:
    print(err)
```

### <a name="python-pymysql"></a>Python (PyMySQL)

```python
conn = pymysql.connect(user='myadmin',
                       password='yourpassword',
                       database='quickstartdb',
                       host='mydemoserver.mysql.database.azure.com',
                       ssl={'ca': '/var/www/html/DigiCertGlobalRootCA.crt.pem'})
```

### <a name="django-pymysql"></a>Django (PyMySQL)

```python
DATABASES = {
    'default': {
        'ENGINE': 'django.db.backends.mysql',
        'NAME': 'quickstartdb',
        'USER': 'myadmin',
        'PASSWORD': 'yourpassword',
        'HOST': 'mydemoserver.mysql.database.azure.com',
        'PORT': '3306',
        'OPTIONS': {
            'ssl': {'ca': '/var/www/html/DigiCertGlobalRootCA.crt.pem'}
        }
    }
}
```

### <a name="ruby"></a>Ruby

```ruby
client = Mysql2::Client.new(
        :host     => 'mydemoserver.mysql.database.azure.com',
        :username => 'myadmin',
        :password => 'yourpassword',
        :database => 'quickstartdb',
        :sslca => '/var/www/html/DigiCertGlobalRootCA.crt.pem'
    )
```

### <a name="golang"></a>Golang

```go
rootCertPool := x509.NewCertPool()
pem, _ := ioutil.ReadFile("/var/www/html/DigiCertGlobalRootCA.crt.pem")
if ok := rootCertPool.AppendCertsFromPEM(pem); !ok {
    log.Fatal("Failed to append PEM.")
}
mysql.RegisterTLSConfig("custom", &tls.Config{RootCAs: rootCertPool})
var connectionString string
connectionString = fmt.Sprintf("%s:%s@tcp(%s:3306)/%s?allowNativePasswords=true&tls=custom",'myadmin' , 'yourpassword', 'mydemoserver.mysql.database.azure.com', 'quickstartdb')
db, _ := sql.Open("mysql", connectionString)
```

### <a name="java-mysql-connector-for-java"></a>Java (MySQL-Connector für Java)

```java
# generate truststore and keystore in code
String importCert = " -import "+
    " -alias mysqlServerCACert "+
    " -file " + ssl_ca +
    " -keystore truststore "+
    " -trustcacerts " +
    " -storepass password -noprompt ";
String genKey = " -genkey -keyalg rsa " +
    " -alias mysqlClientCertificate -keystore keystore " +
    " -storepass password123 -keypass password " +
    " -dname CN=MS ";
sun.security.tools.keytool.Main.main(importCert.trim().split("\\s+"));
sun.security.tools.keytool.Main.main(genKey.trim().split("\\s+"));

# use the generated keystore and truststore
System.setProperty("javax.net.ssl.keyStore","path_to_keystore_file");
System.setProperty("javax.net.ssl.keyStorePassword","password");
System.setProperty("javax.net.ssl.trustStore","path_to_truststore_file");
System.setProperty("javax.net.ssl.trustStorePassword","password");

url = String.format("jdbc:mysql://%s/%s?serverTimezone=UTC&useSSL=true", 'mydemoserver.mysql.database.azure.com', 'quickstartdb');
properties.setProperty("user", 'myadmin');
properties.setProperty("password", 'yourpassword');
conn = DriverManager.getConnection(url, properties);
```

### <a name="java-mariadb-connector-for-java"></a>Java (MariaDB-Connector für Java)

```java
# generate truststore and keystore in code
String importCert = " -import "+
    " -alias mysqlServerCACert "+
    " -file " + ssl_ca +
    " -keystore truststore "+
    " -trustcacerts " +
    " -storepass password -noprompt ";
String genKey = " -genkey -keyalg rsa " +
    " -alias mysqlClientCertificate -keystore keystore " +
    " -storepass password123 -keypass password " +
    " -dname CN=MS ";
sun.security.tools.keytool.Main.main(importCert.trim().split("\\s+"));
sun.security.tools.keytool.Main.main(genKey.trim().split("\\s+"));

# use the generated keystore and truststore
System.setProperty("javax.net.ssl.keyStore","path_to_keystore_file");
System.setProperty("javax.net.ssl.keyStorePassword","password");
System.setProperty("javax.net.ssl.trustStore","path_to_truststore_file");
System.setProperty("javax.net.ssl.trustStorePassword","password");

url = String.format("jdbc:mariadb://%s/%s?useSSL=true&trustServerCertificate=true", 'mydemoserver.mysql.database.azure.com', 'quickstartdb');
properties.setProperty("user", 'myadmin');
properties.setProperty("password", 'yourpassword');
conn = DriverManager.getConnection(url, properties);
```

### <a name="net-mysqlconnector"></a>.NET (MySqlConnector)

```csharp
var builder = new MySqlConnectionStringBuilder
{
    Server = "mydemoserver.mysql.database.azure.com",
    UserID = "myadmin",
    Password = "yourpassword",
    Database = "quickstartdb",
    SslMode = MySqlSslMode.VerifyCA,
    SslCa = "DigiCertGlobalRootCA.crt.pem",
};
using (var connection = new MySqlConnection(builder.ConnectionString))
{
    connection.Open();
}
```

## <a name="next-steps"></a>Nächste Schritte
- [Verwenden von MySQL Workbench zum Herstellen einer Verbindung und zum Abfragen von Daten in Azure Database for MySQL Flexible Server](./connect-workbench.md)
- [Verwenden von PHP zum Herstellen einer Verbindung und zum Abfragen von Daten in Azure Database for MySQL Flexible Server](./connect-php.md)
- [Erstellen und Verwalten von virtuellen Netzwerken für Azure Database for MySQL Flexible Server mit der Azure CLI](./how-to-manage-virtual-network-cli.md)
- Erfahren Sie mehr über [Netzwerke in Azure Database for MySQL Flexible Server](./concepts-networking.md).
- Erfahren Sie mehr über [Firewallregeln für Azure Database for MySQL Flexible Server](./concepts-networking.md#public-access-allowed-ip-addresses).
