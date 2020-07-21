---
title: Konfigurieren von Windows-Node.js-Apps
description: Informationen zum Konfigurieren von Node.js-Apps in die nativen Windows-Instanzen von App Service In diesem Artikel werden die gängigsten Konfigurationsaufgaben vorgestellt.
ms.devlang: nodejs
ms.topic: article
ms.date: 06/02/2020
ms.openlocfilehash: 9f4ccdd04b8d57784f452dc28fa4507fb7ea94c2
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/02/2020
ms.locfileid: "84907925"
---
# <a name="configure-a-windows-nodejs-app-for-azure-app-service"></a>Konfigurieren einer Windows-Node.js-App für Azure App Service

Node.js-Apps müssen mit allen erforderlichen NPM-Abhängigkeiten bereitgestellt werden. Die App Service-Bereitstellungs-Engine führt automatisch `npm install --production` für Sie aus, wenn Sie ein [Git-Repository](deploy-local-git.md) oder ein [ZIP-Paket](deploy-zip.md) mit aktivierter Buildautomatisierung bereitstellen. Wenn Sie Ihre Dateien jedoch mit [FTP/S](deploy-ftp.md) bereitstellen, müssen Sie die erforderlichen Pakete manuell hochladen. Informationen zu Linux-Apps finden Sie unter [Konfigurieren einer Linux-PHP-App für Azure App Service](containers/configure-language-nodejs.md).

Diese Anleitung enthält die wichtigsten Konzepte und Anweisungen für Node.js-Entwickler, die in App Service bereitstellen. Wenn Sie Azure App Service noch nie verwendet haben, befolgen Sie zunächst den [Node.js-Schnellstart](app-service-web-get-started-nodejs.md) und das Tutorial [Node.js mit dem MongoDB](app-service-web-tutorial-nodejs-mongodb-app.md).

## <a name="show-nodejs-version"></a>Anzeigen der Node.js-Version

Führen Sie in [Cloud Shell](https://shell.azure.com) den folgenden Befehl aus, um die aktuelle Node.js-Version anzuzeigen:

```azurecli-interactive
az webapp config appsettings set --name <app-name> --resource-group <resource-group-name> --query "[?name=='WEBSITE_NODE_DEFAULT_VERSION'].value"
```

Führen Sie in [Cloud Shell](https://shell.azure.com) den folgenden Befehl aus, um alle unterstützten Node.js-Versionen anzuzeigen:

```azurecli-interactive
az webapp list-runtimes | grep node
```

## <a name="set-nodejs-version"></a>Festlegen der Node.js-Version

Führen Sie in [Cloud Shell](https://shell.azure.com) den folgenden Befehl aus, um `WEBSITE_NODE_DEFAULT_VERSION` auf eine unterstützte Version und für Ihre App eine [unterstützte Node.js-Version](#show-nodejs-version) festzulegen:

```azurecli-interactive
az webapp config appsettings set --name <app-name> --resource-group <resource-group-name> --settings WEBSITE_NODE_DEFAULT_VERSION="10.15"
```

Diese Einstellung legt die zu verwendende Version von Node.js fest, sowohl zur Laufzeit als auch während der automatischen Paketwiederherstellung bei der App Service-Buildautomatisierung.

> [!NOTE]
> Sie sollten die Node.js-Version in der `package.json` des Projekts festlegen. Die Bereitstellungs-Engine wird in einem separaten Prozess ausgeführt, der alle unterstützten Node.js-Versionen enthält.

## <a name="access-environment-variables"></a>Zugreifen auf Umgebungsvariablen

In App Service können Sie [App-Einstellungen außerhalb Ihres App-Codes festlegen](configure-common.md). Anschließend können Sie darauf unter Verwendung des Node.js-Standardmusters zugreifen. Verwenden Sie beispielsweise den folgenden Code, um auf eine App-Einstellung namens `NODE_ENV` zuzugreifen:

```javascript
process.env.NODE_ENV
```

## <a name="run-gruntbowergulp"></a>Run Grunt/Bower/Gulp

Standardmäßig führt die App Service-Buildautomatisierung `npm install --production` aus, wenn sie erkennt, dass eine Node.js-App über Git bereitgestellt wird (oder über eine ZIP-Bereitstellung mit aktivierter Buildautomatisierung). Wenn Ihre App eines der beliebten Automationstools wie Grunt, Bower oder Gulp benötigt, müssen Sie ein [benutzerdefiniertes Bereitstellungsskript](https://github.com/projectkudu/kudu/wiki/Custom-Deployment-Script) bereitstellen, um es auszuführen.

Damit Ihr Repository diese Tools ausführen kann, müssen Sie sie zu den Abhängigkeiten in *package.json* hinzufügen. Beispiel:

```json
"dependencies": {
  "bower": "^1.7.9",
  "grunt": "^1.0.1",
  "gulp": "^3.9.1",
  ...
}
```

Wechseln Sie von einem lokalen Terminalfenster aus in das Stammverzeichnis Ihres Repositorys, und führen Sie die folgenden Befehle aus:

```bash
npm install kuduscript -g
kuduscript --node --scriptType bash --suppressPrompt
```

Das Stammverzeichnis Ihres Repositorys enthält jetzt zwei zusätzliche Dateien: *.deployment* und *deploy.sh*.

Öffnen Sie *deploy.sh* und suchen Sie den Abschnitt `Deployment`, der wie folgt aussieht:

```bash
##################################################################################################################################
# Deployment
# ----------
```

Dieser Abschnitt endet mit dem Ausführen von `npm install --production`. Fügen Sie den Codeabschnitt, den Sie zum Ausführen des erforderlichen Tools benötigen, *am Ende* des Abschnitts `Deployment` hinzu:

- [Bower](#bower)
- [Gulp](#gulp)
- [Grunt](#grunt)

Weitere Informationen finden Sie in einem [Beispiel im der MEAN.js-Beispiel](https://github.com/Azure-Samples/meanjs/blob/master/deploy.sh#L112-L135), in dem das Bereitstellungsskript auch einen benutzerdefinierten `npm install`-Befehl ausführt.

### <a name="bower"></a>Bower

Dieser Codeausschnitt führt `bower install` aus.

```bash
if [ -e "$DEPLOYMENT_TARGET/bower.json" ]; then
  cd "$DEPLOYMENT_TARGET"
  eval ./node_modules/.bin/bower install
  exitWithMessageOnError "bower failed"
  cd - > /dev/null
fi
```

### <a name="gulp"></a>Gulp

Dieser Codeausschnitt führt `gulp imagemin` aus.

```bash
if [ -e "$DEPLOYMENT_TARGET/gulpfile.js" ]; then
  cd "$DEPLOYMENT_TARGET"
  eval ./node_modules/.bin/gulp imagemin
  exitWithMessageOnError "gulp failed"
  cd - > /dev/null
fi
```

### <a name="grunt"></a>Grunt

Dieser Codeausschnitt führt `grunt` aus.

```bash
if [ -e "$DEPLOYMENT_TARGET/Gruntfile.js" ]; then
  cd "$DEPLOYMENT_TARGET"
  eval ./node_modules/.bin/grunt
  exitWithMessageOnError "Grunt failed"
  cd - > /dev/null
fi
```

## <a name="detect-https-session"></a>Erkennen einer HTTPS-Sitzung

In App Service erfolgt die [SSL-Terminierung](https://wikipedia.org/wiki/TLS_termination_proxy) in den Modulen für den Netzwerklastenausgleich, sodass alle HTTPS-Anforderungen Ihre App als unverschlüsselte HTTP-Anforderungen erreichen. Wenn Ihre App-Logik überprüfen muss, ob Benutzeranforderungen verschlüsselt sind, können Sie dazu den Header `X-Forwarded-Proto` untersuchen.

Gängige Webframeworks ermöglichen den Zugriff auf die Information `X-Forwarded-*` in Ihrem App-Standardmuster. In [Express](https://expressjs.com/) können Sie [trust proxies](https://expressjs.com/guide/behind-proxies.html) (Proxys vertrauen) verwenden. Beispiel:

```javascript
app.set('trust proxy', 1)
...
if (req.secure) {
  // Do something when HTTPS is used
}
```

## <a name="access-diagnostic-logs"></a>Zugreifen auf Diagnoseprotokolle

[!INCLUDE [Access diagnostic logs](../../includes/app-service-web-logs-access-no-h.md)]

## <a name="troubleshooting"></a>Problembehandlung

Wenn sich eine funktionierende Node.js-App in App Service anders verhält oder Fehler aufweist, versuchen Sie Folgendes:

- [Greifen Sie auf den Protokolldatenstrom zu](#access-diagnostic-logs).
- Testen Sie die App lokal im Produktionsmodus. App Service führt Ihre Node.js-Apps im Produktionsmodus aus, daher müssen Sie sicherstellen, dass Ihr Projekt lokal wie erwartet im Produktionsmodus funktioniert. Beispiel:
    - Abhängig von Ihrer *package.json* können verschiedene Pakete für den Produktionsmodus installiert sein (`dependencies` oder `devDependencies`).
    - Bestimmte Webframeworks können statische Dateien im Produktionsmodus unterschiedlich bereitstellen.
    - Bestimmte Webframeworks können benutzerdefinierte Startskripts verwenden, wenn sie im Produktionsmodus ausgeführt werden.
- Führen Sie Ihre App in App Service im Entwicklungsmodus aus. In [MEAN.js](https://meanjs.org/) können Sie Ihre App z. B. zur Laufzeit in den Entwicklungsmodus versetzen, indem Sie die [App-Einstellung `NODE_ENV` festlegen](configure-common.md).

## <a name="next-steps"></a>Nächste Schritte

> [!div class="nextstepaction"]
> [Tutorial: Node.js-App mit MongoDB](app-service-web-tutorial-nodejs-mongodb-app.md)

