---
title: Verwalten von Geheimnissen beim Arbeiten mit einem Azure Dev Space
services: azure-dev-spaces
ms.date: 12/03/2019
ms.topic: conceptual
description: Erfahren Sie, wie Sie beim Entwickeln von Anwendungen mit Azure Dev Spaces Kubernetes-Geheimnisse zur Laufzeit oder Erstellungszeit nutzen können.
keywords: Docker, Kubernetes, Azure, AKS, Azure Container Service, Container
ms.custom: devx-track-js
ms.openlocfilehash: 8791480f420dfd76d5291ce82e8ebf7412a41326
ms.sourcegitcommit: d103a93e7ef2dde1298f04e307920378a87e982a
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/13/2020
ms.locfileid: "91972967"
---
# <a name="how-to-manage-secrets-when-working-with-an-azure-dev-space"></a>Verwalten von Geheimnissen beim Arbeiten mit einem Azure Dev Space

[!INCLUDE [Azure Dev Spaces deprecation](../../../includes/dev-spaces-deprecation.md)]

Für Ihre Dienste sind möglicherweise bestimmte Kennwörter, Verbindungszeichenfolgen und andere Geheimnisse erforderlich, z. B. für Datenbanken oder andere sichere Azure-Dienste. Wenn Sie die Werte dieser Geheimnisse in Konfigurationsdateien festlegen, können Sie sie in Ihrem Code als Umgebungsvariablen zur Verfügung stellen.  Gehen Sie umsichtig mit Konfigurationsdateien um, um die Sicherheit der Geheimnisse nicht zu gefährden.

## <a name="storing-and-using-runtime-secrets"></a>Speichern und Verwenden von Laufzeitgeheimnissen

Azure Dev Spaces bietet zwei empfohlene und optimierte Optionen zum Speichern von Geheimnissen in Helm-Charts, die von den Azure Dev Spaces-Clienttools generiert werden: in der Datei `values.dev.yaml` und inline in `azds.yaml`. Es wird davon abgeraten, Geheimnisse in `values.yaml` zu speichern.

> [!NOTE]
> Die folgenden Vorgehensweisen zeigen, wie Sie Geheimnisse für Helm-Diagramme, die von den Clienttools generiert werden, speichern und verwenden. Wenn Sie ein eigenes Helm-Diagramm erstellen, können Sie dieses direkt verwenden, um Geheimnisse zu verwalten und zu speichern.

### <a name="using-valuesdevyaml"></a>Verwenden von „values.dev.yaml“

Erstellen Sie in einem Projekt, das Sie bereits mit Azure Dev Spaces vorbereitet haben, eine `values.dev.yaml`-Datei im selben Ordner wie `azds.yaml`, um die geheimen Schlüssel und Werte zu definieren. Beispiel:

```yaml
secrets:
  redis:
    port: "6380"
    host: "contosodevredis.redis.cache.windows.net"
    key: "secretkeyhere"
```

Überprüfen Sie mithilfe eines `?`, ob die `azds.yaml`-Dateiverweise `values.dev.yaml` optional sind. Beispiel:

```yaml
install:
  values:
  - values.dev.yaml?
  - secrets.dev.yaml?
```

Wenn Sie weitere geheime Dateien haben, können Sie diese ebenfalls hier hinzufügen.

Aktualisieren oder überprüfen Sie, ob Ihr Dienst auf Ihre Geheimnisse als Umgebungsvariablen verweist. Beispiel:

```javascript
var redisPort = process.env.REDIS_PORT
var host = process.env.REDIS_HOST
var theKey = process.env.REDIS_KEY
```
    
Führen Sie die aktualisierten Dienste mit `azds up` aus.

```console
azds up
```
 
Überprüfen Sie mit `kubectl`, ob Ihre Geheimnisse erstellt wurden.

```console
kubectl get secret --namespace default -o yaml 
```

> [!IMPORTANT]
> Es wird davon abgeraten, Geheimnisse in der Quellcodeverwaltung zu speichern. Wenn Sie Git verwenden, fügen Sie der `.gitignore`-Datei `values.dev.yaml` hinzu, um zu vermeiden, dass Geheimnisse in die Quellcodeverwaltung übernommen werden.

### <a name="using-azdsyaml"></a>Verwenden von „azds.yaml“

Fügen Sie in einem Projekt, das Sie bereits mit Azure Dev Spaces vorbereitet haben, geheime Schlüssel und Werte mithilfe der Syntax *$PLACEHOLDER* unter *configurations.develop.install.set* in `azds.yaml` hinzu. Beispiel:

```yaml
configurations:
  develop:
    ...
    install:
      set:
        secrets:
          redis:
            port: "$REDIS_PORT"
            host: "$REDIS_HOST"
            key: "$REDIS_KEY"
```

> [!NOTE]
> Sie können geheime Werte direkt eingeben, ohne die Syntax *$PLACEHOLDER* in `azds.yaml`zu verwenden. Dieser Ansatz wird jedoch nicht empfohlen, da `azds.yaml` in der Quellcodeverwaltung gespeichert wird.
     
Erstellen Sie eine `.env`-Datei im gleichen Ordner wie `azds.yaml`, um die *$PLACEHOLDER*-Werte zu definieren. Beispiel:

```
REDIS_PORT=3333
REDIS_HOST=myredishost
REDIS_KEY=myrediskey
```

> [!IMPORTANT]
> Es wird davon abgeraten, Geheimnisse in der Quellcodeverwaltung zu speichern. Wenn Sie Git verwenden, fügen Sie der `.gitignore`-Datei `.env` hinzu, um zu vermeiden, dass Geheimnisse in die Quellcodeverwaltung übernommen werden.

Aktualisieren oder überprüfen Sie, ob Ihr Dienst auf Ihre Geheimnisse als Umgebungsvariablen verweist. Beispiel:

```javascript
var redisPort = process.env.REDIS_PORT
var host = process.env.REDIS_HOST
var theKey = process.env.REDIS_KEY
```
    
Führen Sie die aktualisierten Dienste mit `azds up` aus.

```console
azds up
```
 
Überprüfen Sie mit `kubectl`, ob Ihre Geheimnisse erstellt wurden.

```console
kubectl get secret --namespace default -o yaml 
```

## <a name="using-secrets-as-build-arguments"></a>Verwenden von Geheimnissen als Buildargumente

In den vorherigen Abschnitten wurde gezeigt, wie Sie Geheimnisse speichern und verwenden, die zur Laufzeit des Containers verwendet werden. Sie können ein Geheimnis auch zur Erstellungszeit des Containers, z. B. ein Kennwort für ein privates NuGet, mithilfe von `azds.yaml` verwenden.

Legen Sie in `azds.yaml` mithilfe der `<variable name>: ${secret.<secret name>.<secret key>}`-Syntax die Erstellungszeit der Geheimnisse in *configurations.develop.build.args* fest. Beispiel:

```yaml
configurations:
  develop:
    build:
      dockerfile: Dockerfile.develop
      useGitIgnore: true
      args:
        BUILD_CONFIGURATION: ${BUILD_CONFIGURATION:-Debug}
        MYTOKEN: ${secret.mynugetsecret.pattoken}
```

Im obigen Beispiel ist *mynugetsecret* ein vorhandenes Geheimnis und *pattoken* ein vorhandener Schlüssel.

>[!NOTE]
> Geheimnisnamen und Schlüssel können das `.`-Zeichen enthalten. Verwenden Sie `\`, um `.` beim Übergeben von Geheimnissen als Buildargumente zu umgehen, z. B. beim Übergeben eines Geheimnisses mit dem Namen *foo.bar* und dem Schlüssel *token*: `MYTOKEN: ${secret.foo\.bar.token}`. Des Weiteren können Geheimnisse mit einem Präfix- und Postfixtext ausgewertet werden. Beispiel: `MYURL: eus-${secret.foo\.bar.token}-version1`. Geheimnisse, die in übergeordneten und über-übergeordneten Bereichen verfügbar sind, können als Buildargumente übergeben werden.

Verwenden Sie in Ihrem Dockerfile die *ARG*-Anweisung, um das Geheimnis zu verwenden. Verwenden Sie dann später im Dockerfile die gleiche Variable. Beispiel:

```dockerfile
...
ARG MYTOKEN
...
ARG NUGET_EXTERNAL_FEED_ENDPOINTS="{'endpointCredentials': [{'endpoint':'PRIVATE_NUGET_ENDPOINT', 'password':'${MYTOKEN}'}]}"
...
```

Aktualisieren Sie die in Ihrem Cluster ausgeführten Dienste mit diesen Änderungen. Führen Sie in der Befehlszeile den folgenden Befehl aus:

```
azds up
```

## <a name="next-steps"></a>Nächste Schritte

Mithilfe dieser Methoden können Sie jetzt eine sichere Verbindung mit einer Datenbank oder einer Azure Cache for Redis-Instanz herstellen oder auf sichere Azure-Dienste zugreifen.
 
