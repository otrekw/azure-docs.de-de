---
title: Konfigurieren von PHP-Apps
description: Hier erfahren Sie, wie Sie einen vordefinierten PHP-Container für Ihre App konfigurieren. In diesem Artikel werden die gängigsten Konfigurationsaufgaben vorgestellt.
ms.devlang: php
ms.topic: article
ms.date: 03/28/2019
ms.openlocfilehash: 9e87466f810dc4ebf767c36ad74c358cbf6069e5
ms.sourcegitcommit: 31e9f369e5ff4dd4dda6cf05edf71046b33164d3
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/22/2020
ms.locfileid: "81758874"
---
# <a name="configure-a-linux-php-app-for-azure-app-service"></a>Konfigurieren einer PHP-App für Azure App Service

In diesem Leitfaden erfahren Sie, wie Sie die integrierte PHP-Laufzeit für Web-Apps, mobile Back-Ends und API-Apps in Azure App Service konfigurieren.

Dieser Leitfaden enthält wichtige Konzepte und Anleitungen für PHP-Entwickler, die in App Service einen integrierten Linux-Container verwenden. Wenn Sie Azure App Service noch nie verwendet haben, befolgen Sie zunächst den [PHP-Schnellstart](quickstart-php.md) und das [Tutorial zu PHP mit MySQL](tutorial-php-mysql-app.md).

## <a name="show-php-version"></a>Anzeigen der PHP-Version

Führen Sie in [Cloud Shell](https://shell.azure.com) den folgenden Befehl aus, um die aktuelle PHP-Version anzuzeigen:

```azurecli-interactive
az webapp config show --resource-group <resource-group-name> --name <app-name> --query linuxFxVersion
```

Führen Sie in [Cloud Shell](https://shell.azure.com) den folgenden Befehl aus, um alle unterstützten PHP-Versionen anzuzeigen:

```azurecli-interactive
az webapp list-runtimes --linux | grep PHP
```

## <a name="set-php-version"></a>Festlegen der PHP-Version

Führen Sie in [Cloud Shell](https://shell.azure.com) den folgenden Befehl aus, um die PHP-Version auf 7.2 festzulegen:

```azurecli-interactive
az webapp config set --name <app-name> --resource-group <resource-group-name> --linux-fx-version "PHP|7.2"
```

## <a name="customize-build-automation"></a>Anpassen der Buildautomatisierung

Wenn Sie Ihre App mithilfe von Git- oder ZIP-Paketen mit aktivierter Buildautomatisierung bereitstellen, durchläuft die App Service-Buildautomatisierung die Schritte der folgenden Sequenz:

1. Ausführen eines benutzerdefinierten Skripts, falls durch `PRE_BUILD_SCRIPT_PATH` angegeben
1. Führen Sie `php composer.phar install` aus.
1. Ausführen eines benutzerdefinierten Skripts, falls mittels `POST_BUILD_SCRIPT_PATH` angegeben.

`PRE_BUILD_COMMAND` und `POST_BUILD_COMMAND` sind Umgebungsvariablen, die standardmäßig leer sind. Um Präbuildbefehle auszuführen, definieren Sie `PRE_BUILD_COMMAND`. Um Postbuildbefehle auszuführen, definieren Sie `POST_BUILD_COMMAND`.

Im folgenden Beispiel werden die beiden Variablen für eine Reihe von Befehlen angegeben, die durch Kommas getrennt sind.

```azurecli-interactive
az webapp config appsettings set --name <app-name> --resource-group <resource-group-name> --settings PRE_BUILD_COMMAND="echo foo, scripts/prebuild.sh"
az webapp config appsettings set --name <app-name> --resource-group <resource-group-name> --settings POST_BUILD_COMMAND="echo foo, scripts/postbuild.sh"
```

Weitere Umgebungsvariablen zum Anpassen der Buildautomatisierung finden Sie unter [Oryx-Konfiguration](https://github.com/microsoft/Oryx/blob/master/doc/configuration.md).

Weitere Informationen, wie App Service PHP-Apps in Linux ausführt und erstellt, finden Sie unter [Oryx-Dokumentation: Erkennen und Erstellen von PHP-Apps](https://github.com/microsoft/Oryx/blob/master/doc/runtimes/php.md).

## <a name="customize-start-up"></a>Anpassen des Startvorgangs

Standardmäßig führt der integrierte PHP-Container den Apache-Server aus. Beim Start führt er `apache2ctl -D FOREGROUND"` aus. Wenn Sie möchten, können Sie beim Start einen anderen Befehl ausführen, indem Sie den folgenden Befehl in der [Cloud Shell](https://shell.azure.com) ausführen:

```azurecli-interactive
az webapp config set --resource-group <resource-group-name> --name <app-name> --startup-file "<custom-command>"
```

## <a name="access-environment-variables"></a>Zugreifen auf Umgebungsvariablen

In App Service können Sie [App-Einstellungen außerhalb Ihres App-Codes festlegen](../configure-common.md?toc=%2fazure%2fapp-service%2fcontainers%2ftoc.json#configure-app-settings). Anschließend können Sie darauf unter Verwendung des Standardmusters [getenv()](https://secure.php.net/manual/function.getenv.php) zugreifen. Verwenden Sie beispielsweise den folgenden Code, um auf eine App-Einstellung namens `DB_HOST` zuzugreifen:

```php
getenv("DB_HOST")
```

## <a name="change-site-root"></a>Ändern des Sitestammverzeichnisses

Das Webframework Ihrer Wahl verwendet möglicherweise ein Unterverzeichnis als Stammverzeichnis der Site. [Laravel](https://laravel.com/) verwendet beispielsweise das Unterverzeichnis `public/` als Stammverzeichnis der Site.

Das PHP-Standardimage für App Service verwendet Apache, und Sie können das Stammverzeichnis der Site für Ihre App nicht anpassen. Um diese Einschränkung zu umgehen, fügen Sie dem Stammverzeichnis Ihres Repositorys eine *.htaccess*-Datei mit dem folgenden Inhalt hinzu:

```
<IfModule mod_rewrite.c>
    RewriteEngine on

    RewriteRule ^(.*)$ /public/$1 [NC,L,QSA]
</IfModule>
```

Falls Sie die Umschreibung per *.htaccess* nicht nutzen möchten, können Sie Ihre Laravel-Anwendung stattdessen mit einem [benutzerdefinierten Docker-Image](quickstart-docker-go.md) bereitstellen.

## <a name="detect-https-session"></a>Erkennen einer HTTPS-Sitzung

In App Service erfolgt die [SSL-Terminierung](https://wikipedia.org/wiki/TLS_termination_proxy) in den Modulen für den Netzwerklastenausgleich, sodass alle HTTPS-Anforderungen Ihre App als unverschlüsselte HTTP-Anforderungen erreichen. Wenn Ihre App-Logik überprüfen muss, ob Benutzeranforderungen verschlüsselt sind, können Sie dazu den Header `X-Forwarded-Proto` untersuchen.

```php
if (isset($_SERVER['X-Forwarded-Proto']) && $_SERVER['X-Forwarded-Proto'] === 'https') {
  // Do something when HTTPS is used
}
```

Gängige Webframeworks ermöglichen den Zugriff auf die Information `X-Forwarded-*` in Ihrem App-Standardmuster. In [CodeIgniter](https://codeigniter.com/) überprüft [is_https()](https://github.com/bcit-ci/CodeIgniter/blob/master/system/core/Common.php#L338-L365) standardmäßig den Wert von `X_FORWARDED_PROTO`.

## <a name="customize-phpini-settings"></a>Anpassen der „php.ini“-Einstellungen

Wenn Sie Änderungen an Ihrer PHP-Installation vornehmen müssen, können Sie jede der [php.ini-Anweisungen](https://www.php.net/manual/ini.list.php) mit folgenden Schritten ändern.

> [!NOTE]
> Die beste Möglichkeit zum Anzeigen der PHP-Version und der aktuellen *php.ini*-Konfiguration besteht darin, [phpinfo()](https://php.net/manual/function.phpinfo.php) in Ihrer App aufzurufen.
>

### <a name="customize-non-php_ini_system-directives"></a><a name="Customize-non-PHP_INI_SYSTEM directives"></a>Anpassen von Nicht-PHP_INI_SYSTEM-Anweisungen

Zum Anpassen der Anweisungen PHP_INI_USER PHP_INI_PERDIR und PHP_INI_ALL (siehe [php.ini-Anweisungen](https://www.php.net/manual/ini.list.php)), fügen Sie im Stammverzeichnis Ihrer App eine *.htaccess*-Datei hinzu.

Fügen Sie in der *.htaccess*-Datei die Anweisungen hinzu, indem Sie die `php_value <directive-name> <value>`-Syntax verwenden. Beispiel:

```
php_value upload_max_filesize 1000M
php_value post_max_size 2000M
php_value memory_limit 3000M
php_value max_execution_time 180
php_value max_input_time 180
php_value display_errors On
php_value upload_max_filesize 10M
```

Stellen Sie Ihre App mit den Änderungen erneut bereit, und starten Sie sie neu. Wenn Sie sie mit Kudu bereitstellen (z. B. [Git](../deploy-local-git.md?toc=%2fazure%2fapp-service%2fcontainers%2ftoc.json)), wird sie automatisch nach der Bereitstellung neu gestartet.

Als Alternative zur Verwendung von *.htaccess* können Sie [ini_set()](https://www.php.net/manual/function.ini-set.php) in Ihrer App verwenden, um diese Nicht-PHP_INI_SYSTEM-Anweisungen anzupassen.

### <a name="customize-php_ini_system-directives"></a><a name="customize-php_ini_system-directives"></a>Anpassen von PHP_INI_SYSTEM-Anweisungen

Zum Anpassen von PHP_INI_SYSTEM-Anweisungen (siehe [php.ini-Anweisungen](https://www.php.net/manual/ini.list.php)) können Sie nicht den Ansatz mit *.htaccess* verwenden. App Service bietet einen separaten Mechanismus, der die App-Einstellung `PHP_INI_SCAN_DIR` verwendet.

Führen Sie als Erstes den folgenden Befehl in der [Cloud Shell](https://shell.azure.com) aus, um eine App-Einstellung namens `PHP_INI_SCAN_DIR` hinzuzufügen:

```azurecli-interactive
az webapp config appsettings set --name <app-name> --resource-group <resource-group-name> --settings PHP_INI_SCAN_DIR="/usr/local/etc/php/conf.d:/home/site/ini"
```

`/usr/local/etc/php/conf.d` ist das Standardverzeichnis, in dem *php.ini* vorhanden ist. `/home/site/ini` ist das benutzerdefinierte Verzeichnis, in dem Sie eine benutzerdefinierte *ini*-Datei hinzufügen werden. Sie trennen die Werte durch `:`.

Navigieren Sie zu der Web-SSH-Sitzung mit Ihrem Linux-Container (`https://<app-name>.scm.azurewebsites.net/webssh/host`).

Erstellen Sie ein Verzeichnis in `/home/site` namens `ini`, erstellen Sie dann eine *.ini*-Datei im Verzeichnis `/home/site/ini` (z. B. *settings.ini)* mit den Anweisungen, die Sie anpassen möchten. Verwenden Sie dieselbe Syntax, die Sie in einer *php.ini*-Datei verwenden würden. 

> [!TIP]
> In den integrierten Linux-Containern in App Service wird */home* als persistenter freigegebener Speicher verwendet. 
>

Führen Sie beispielsweise zum Ändern des Werts von [expose_php](https://php.net/manual/ini.core.php#ini.expose-php) die folgenden Befehle aus:

```bash
cd /home/site
mkdir ini
echo "expose_php = Off" >> ini/setting.ini
```

Damit die Änderungen wirksam werden, starten Sie die App neu.

## <a name="enable-php-extensions"></a>Aktivieren von PHP-Erweiterungen

Die integrierten PHP-Installationen enthalten die am häufigsten verwendeten Erweiterungen. Sie können zusätzliche Erweiterungen auf dieselbe Weise aktivieren, wie Sie [php.ini-Anweisungen anpassen](#customize-php_ini_system-directives).

> [!NOTE]
> Die beste Möglichkeit zum Anzeigen der PHP-Version und der aktuellen *php.ini*-Konfiguration besteht darin, [phpinfo()](https://php.net/manual/function.phpinfo.php) in Ihrer App aufzurufen.
>

Um zusätzliche Erweiterungen zu aktivieren, führen Sie diese Schritte aus:

Fügen Sie im Stammverzeichnis Ihrer App ein Verzeichnis `bin` hinzu, und legen Sie die `.so`-Erweiterungsdateien darin ab (z. B. *mongodb.so*). Stellen Sie sicher, dass die Erweiterungen mit der PHP-Version in Azure sowie mit VC9 und nts-kompatibel (non-thread-safe, nicht threadsicher) sind.

Stellen Sie Ihre Änderungen bereit.

Befolgen Sie die Schritte in [Anpassen von PHP_INI_SYSTEM-Anweisungen](#customize-php_ini_system-directives), fügen Sie die Erweiterungen in der benutzerdefinierten *.ini*-Datei mit den Anweisungen [extension](https://www.php.net/manual/ini.core.php#ini.extension) oder [zend_extension](https://www.php.net/manual/ini.core.php#ini.zend-extension) hinzu.

```ini
extension=/home/site/wwwroot/bin/mongodb.so
zend_extension=/home/site/wwwroot/bin/xdebug.so
```

Damit die Änderungen wirksam werden, starten Sie die App neu.

## <a name="access-diagnostic-logs"></a>Zugreifen auf Diagnoseprotokolle

[!INCLUDE [Access diagnostic logs](../../../includes/app-service-web-logs-access-no-h.md)]

## <a name="open-ssh-session-in-browser"></a>Öffnen einer SSH-Sitzung im Browser

[!INCLUDE [Open SSH session in browser](../../../includes/app-service-web-ssh-connect-builtin-no-h.md)]

## <a name="troubleshooting"></a>Problembehandlung

Wenn sich eine funktionierende PHP-App in App Service anders verhält oder Fehler aufweist, versuchen Sie Folgendes:

- [Greifen Sie auf den Protokolldatenstrom zu](#access-diagnostic-logs).
- Testen Sie die App lokal im Produktionsmodus. App Service führt Ihre Node.js-Apps im Produktionsmodus aus, daher müssen Sie sicherstellen, dass Ihr Projekt lokal wie erwartet im Produktionsmodus funktioniert. Beispiel:
    - Abhängig von Ihrer *composer.json* können verschiedene Pakete für den Produktionsmodus installiert sein (`require` oder `require-dev`).
    - Bestimmte Webframeworks können statische Dateien im Produktionsmodus unterschiedlich bereitstellen.
    - Bestimmte Webframeworks können benutzerdefinierte Startskripts verwenden, wenn sie im Produktionsmodus ausgeführt werden.
- Führen Sie Ihre App in App Service im Debugmodus aus. Z. B. können Sie in [Laravel](https://meanjs.org/) Ihre App so konfigurieren, dass sie in der Produktion Debugmeldungen ausgibt, indem Sie [die App-Einstellung `APP_DEBUG` auf `true`](../configure-common.md?toc=%2fazure%2fapp-service%2fcontainers%2ftoc.json#configure-app-settings) festlegen.

[!INCLUDE [robots933456](../../../includes/app-service-web-configure-robots933456.md)]

## <a name="next-steps"></a>Nächste Schritte

> [!div class="nextstepaction"]
> [Tutorial: PHP-App mit MySQL](tutorial-php-mysql-app.md)

> [!div class="nextstepaction"]
> [Häufig gestellte Fragen (FAQ) zu Azure App Service unter Linux](app-service-linux-faq.md)
