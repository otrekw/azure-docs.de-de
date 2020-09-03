---
title: Konfigurieren von Windows Java-Apps
description: Erfahren Sie, wie Sie Java-Apps so konfigurieren, dass sie auf Windows VM-Instanzen in Azure App Service ausgeführt werden können. In diesem Artikel werden die gängigsten Konfigurationsaufgaben vorgestellt.
keywords: Azure App Service, Web-App, Windows, OSS, Java
author: jasonfreeberg
ms.devlang: java
ms.topic: article
ms.date: 04/12/2019
ms.author: jafreebe
ms.reviewer: cephalin
ms.custom: seodec18, devx-track-java
zone_pivot_groups: app-service-platform-windows-linux
ms.openlocfilehash: 5d94da91428da2270e0f690df4dcd43ae43d8597
ms.sourcegitcommit: 648c8d250106a5fca9076a46581f3105c23d7265
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/27/2020
ms.locfileid: "88961651"
---
# <a name="configure-a-java-app-for-azure-app-service"></a>Konfigurieren einer Java-App für Azure App Service

::: zone pivot="platform-windows"  

Mit Azure App Service können Java-Entwickler ihre Tomcat-Webanwendungen in einem vollständig verwalteten Windows-basierten Dienst schnell erstellen, bereitstellen und skalieren. Stellen Sie Anwendungen mit Maven-Plug-Ins über die Befehlszeile oder in Editoren wie IntelliJ, Eclipse oder Visual Studio Code bereit.

Dieser Leitfaden enthält die wichtigsten Konzepte und Anweisungen für Java-Entwickler, die App Service verwenden. Wenn Sie Azure App Service noch nie verwendet haben, lesen Sie zunächst den [Java-Schnellstart](quickstart-java.md). Allgemeine Fragen zur Verwendung von App Service, die sich nicht speziell auf die Java-Entwicklung beziehen, werden unter [Häufig gestellte Fragen (FAQ) zu App Service unter Windows](faq-configuration-and-management.md) beantwortet.

## <a name="deploying-your-app"></a>Bereitstellen Ihrer App

Sie können das [Azure-Web-App-Plug-In für Maven](/java/api/overview/azure/maven/azure-webapp-maven-plugin/readme) verwenden, um Ihre WAR-Dateien bereitzustellen. Die Bereitstellung mit beliebten IDEs wird außerdem mit [Azure Toolkit für IntelliJ](/azure/developer/java/toolkit-for-intellij/) oder [Azure Toolkit für Eclipse](/azure/developer/java/toolkit-for-eclipse) unterstützt.

In allen anderen Fällen hängt Ihre Bereitstellungsmethode von Ihrem Archivtyp ab:

- Um WAR-Dateien in Tomcat bereitzustellen, verwenden Sie den `/api/wardeploy/`-Endpunkt, um Ihre Archivdatei mit POST zu veröffentlichen. Weitere Informationen zu dieser API finden Sie in [dieser Dokumentation](./deploy-zip.md#deploy-war-file).
- Um JAR-Dateien in Java SE bereitzustellen, verwenden Sie den `/api/zipdeploy/`-Endpunkt der Kudu-Website. Weitere Informationen zu dieser API finden Sie in [dieser Dokumentation](./deploy-zip.md#rest).

Stellen Sie Ihre WAR- oder JAR-Dateien nicht mithilfe von FTP bereit. Der FTP-Tool dient zum Hochladen von Startskripts, Abhängigkeiten oder anderen Laufzeitdateien. Es ist nicht die optimale Wahl für die Bereitstellung von Web-Apps.

## <a name="logging-and-debugging-apps"></a>Protokollieren und Debuggen von Apps

Leistungsberichte, Datenverkehrsvisualisierungen und Integritätsprüfungen für die einzelnen Apps stehen über das Azure-Portal zur Verfügung. Weitere Informationen hierzu finden Sie unter [Azure App Service-Pläne – Diagnoseübersicht](overview-diagnostics.md).

### <a name="use-flight-recorder"></a>Verwenden von Flight Recorder

Alle Java-Laufzeiten in App Service, die Azul-JVMs verwenden, enthalten Zulu Flight Recorder. Sie können damit Ereignisse auf JVM-, System- und Java-Ebene aufzeichnen, um das Verhalten der Java-Anwendungen zu überwachen und Probleme bei diesen zu beheben.

Um eine zeitgesteuerte Aufzeichnung zu erstellen, benötigen Sie die PID (Prozess-ID) der Java-Anwendung. Öffnen Sie zum Suchen der PID in einem Browser die SCM-Website Ihrer Web-App unter https://<Name Ihrer Website>.scm.azurewebsites.net/ProcessExplorer/. Auf dieser Seite werden die in Ihrer Web-App ausgeführten Prozesse angezeigt. Suchen Sie in der Tabelle den Prozess mit dem Namen „Java“, und kopieren Sie die entsprechende PID (Prozess-ID).

Öffnen Sie dann auf der oberen Symbolleiste der SCM-Website die **Debugging-Konsole**, und führen Sie den folgenden Befehl aus. Ersetzen Sie `<pid>` durch die Prozess-ID, die Sie zuvor kopiert haben. Dieser Befehl startet eine 30-Sekunden-Profileraufzeichnung der Java-Anwendung und generiert eine Datei mit dem Namen `timed_recording_example.jfr` im Verzeichnis `D:\home`.

```
jcmd <pid> JFR.start name=TimedRecording settings=profile duration=30s filename="D:\home\timed_recording_example.JFR"
```

Weitere Informationen finden Sie in der [Jcmd-Befehlsreferenz](https://docs.oracle.com/javacomponents/jmc-5-5/jfr-runtime-guide/comline.htm#JFRRT190).

#### <a name="analyze-jfr-files"></a>Analysieren von `.jfr`-Dateien

Verwenden Sie [FTPS](deploy-ftp.md) zum Herunterladen Ihrer JFR-Datei auf Ihren lokalen Computer. Laden Sie zum analysieren der JFR-Datei [Zulu Mission Control](https://www.azul.com/products/zulu-mission-control/) herunter und installieren Sie es. Weitere Informationen zu Zulu Mission Control finden Sie in [„Azul-Dokumentation“](https://docs.azul.com/zmc/) und den [Installationsanweisungen](/java/azure/jdk/java-jdk-flight-recorder-and-mission-control).

### <a name="stream-diagnostic-logs"></a>Streamen von Diagnoseprotokollen

[!INCLUDE [Access diagnostic logs](../../includes/app-service-web-logs-access-no-h.md)]

Weitere Informationen finden Sie unter [Streamen von Protokollen in Cloud Shell](troubleshoot-diagnostic-logs.md#in-cloud-shell).

### <a name="app-logging"></a>App-Protokollierung

Aktivieren Sie die [Anwendungsprotokollierung](troubleshoot-diagnostic-logs.md#enable-application-logging-windows) über das Azure-Portal oder die [Azure-Befehlszeilenschnittstelle](/cli/azure/webapp/log#az-webapp-log-config), um App Service so zu konfigurieren, dass die Streams mit der Standardkonsolenausgabe und den Standardkonsolenfehlern Ihrer Anwendung in das lokale Dateisystem oder Azure Blob Storage geschrieben werden. Die Protokollierung in der lokalen App Service-Dateisysteminstanz wird 12 Stunden nach der Konfiguration deaktiviert. Wenn Sie eine längere Beibehaltung benötigen, konfigurieren Sie die Anwendung für die Ausgabe in einen Blob Storage-Container. Ihre Java- und Tomcat-App-Protokolle befinden sich im Verzeichnis */LogFiles/Application/* .

Wenn Ihre Anwendung [Logback](https://logback.qos.ch/) oder [Log4j](https://logging.apache.org/log4j) für die Ablaufverfolgung verwendet, können Sie diese Ablaufverfolgungen mithilfe der Konfigurationsanweisungen für das Protokollierungsframework unter [Untersuchen von Java-Ablaufverfolgungsprotokollen in Application Insights](../azure-monitor/app/java-trace-logs.md) zur Überprüfung an Azure Application Insights weiterleiten.


## <a name="customization-and-tuning"></a>Anpassung und Optimierung

Azure App Service unterstützt eine sofort verfügbare Optimierung und Anpassung über das Azure-Portal und die CLI. Lesen Sie die folgenden Artikel zur nicht Java-spezifischen Web-App-Konfiguration:

- [Konfigurieren von App-Einstellungen](configure-common.md#configure-app-settings)
- [Einrichten einer benutzerdefinierten Domäne](app-service-web-tutorial-custom-domain.md)
- [Konfigurieren von TLS-Bindungen](configure-ssl-bindings.md)
- [Hinzufügen eines CDN](../cdn/cdn-add-to-web-app.md)
- [Konfigurieren der Kudu-Website](https://github.com/projectkudu/kudu/wiki/Configurable-settings)

### <a name="set-java-runtime-options"></a>Festlegen von Java-Runtimeoptionen

Um reservierten Arbeitsspeicher oder andere JVM-Runtimeoptionen festzulegen, erstellen Sie mit den Optionen eine [App-Einstellung](configure-common.md#configure-app-settings) mit dem Namen `JAVA_OPTS`. App Service übergibt diese Einstellung beim Start als Umgebungsvariable an die Java-Runtime.

Erstellen Sie im Azure-Portal unter **Anwendungseinstellungen** für die Web-App eine neue App-Einstellung namens `JAVA_OPTS`, welche die zusätzlichen Einstellungen enthält, z.B. `-Xms512m -Xmx1204m`.

Um die App-Einstellung über das Maven-Plug-In zu konfigurieren, fügen Sie Einstellungs-/Werttags im Azure-Plug-In-Abschnitt hinzu. Im folgenden Beispiel werden bestimmte Mindest- und Höchstwerte für die Java-Heapgröße festgelegt:

```xml
<appSettings>
    <property>
        <name>JAVA_OPTS</name>
        <value>-Xms512m -Xmx1204m</value>
    </property>
</appSettings>
```

Entwickler, die eine einzige Anwendung mit einem Bereitstellungsslot in ihrem App Service-Plan ausführen, können die folgenden Optionen verwenden:

- B1- und S1-Instanzen: `-Xms1024m -Xmx1024m`
- B2- und S2-Instanzen: `-Xms3072m -Xmx3072m`
- B3- und S3-Instanzen: `-Xms6144m -Xmx6144m`

Überprüfen Sie beim Optimieren Ihrer Anwendungsheapeinstellungen Ihre App Service-Plandetails, und berücksichtigen Sie mehrere Anwendungs- und Bereitstellungsslotanforderungen, um die optimale Zuordnung von Arbeitsspeicher zu ermitteln.

### <a name="turn-on-web-sockets"></a>Aktivieren von Websockets

Aktivieren Sie die Unterstützung für Websockets im Azure-Portal in den **Anwendungseinstellungen** für die Anwendung. Sie müssen die Anwendung neu starten, damit die Einstellung in Kraft tritt.

Aktivieren Sie die Websocketunterstützung über die Azure CLI mithilfe des folgenden Befehls:

```azurecli-interactive
az webapp config set --name <app-name> --resource-group <resource-group-name> --web-sockets-enabled true
```

Starten Sie Ihre Anwendung anschließend neu:

```azurecli-interactive
az webapp stop --name <app-name> --resource-group <resource-group-name>
az webapp start --name <app-name> --resource-group <resource-group-name>
```

### <a name="set-default-character-encoding"></a>Festlegen der Standardzeichencodierung

Erstellen Sie im Azure-Portal unter **Anwendungseinstellungen** für die Web-App eine neue App-Einstellung namens `JAVA_OPTS` mit dem Wert `-Dfile.encoding=UTF-8`.

Alternativ können Sie die App-Einstellung mit dem App Service-Maven-Plug-In konfigurieren. Fügen Sie die name- und value-Tags der Einstellung in der Plug-In-Konfiguration hinzu:

```xml
<appSettings>
    <property>
        <name>JAVA_OPTS</name>
        <value>-Dfile.encoding=UTF-8</value>
    </property>
</appSettings>
```

### <a name="pre-compile-jsp-files"></a>Vorkompilieren von JSP-Dateien

Zur Verbesserung der Leistung von Tomcat-Anwendungen können Sie die JSP-Dateien kompilieren, bevor sie in App Service bereitgestellt werden. Sie können das von Apache Sling bereitgestellte [Maven-Plug-In](https://sling.apache.org/components/jspc-maven-plugin/plugin-info.html) oder diese [Ant-Builddatei](https://tomcat.apache.org/tomcat-9.0-doc/jasper-howto.html#Web_Application_Compilation) verwenden.

## <a name="secure-applications"></a>Sichere Anwendungen

Für in App Service ausgeführte Java-Anwendungen gelten dieselben [bewährten Sicherheitsmethoden](../security/fundamentals/paas-applications-using-app-services.md) wie für andere Anwendungen.

### <a name="authenticate-users-easy-auth"></a>Authentifizieren von Benutzern (einfache Authentifizierung)

Richten Sie die App-Authentifizierung im Azure-Portal über die Option **Authentifizierung und Autorisierung** ein. Von dort aus können Sie die Authentifizierung über Azure Active Directory oder soziale Netzwerke wie Facebook, Google oder GitHub aktivieren. Die Konfiguration des Azure-Portals funktioniert nur, wenn Sie einen einzelnen Authentifizierungsanbieter konfigurieren. Weitere Informationen finden Sie unter [Konfigurieren Ihrer App Service-App zur Verwendung der Azure Active Directory-Anmeldung](configure-authentication-provider-aad.md) und in den entsprechenden Artikeln für andere Identitätsanbieter. Wenn Sie mehrere Anmeldungsanbieter aktivieren müssen, befolgen Sie die Anweisungen im Artikel [Anpassen der Authentifizierung und Autorisierung in Azure App Service](app-service-authentication-how-to.md).

#### <a name="tomcat"></a>Tomcat

Ihre Tomcat-Anwendung kann direkt aus dem Servlet auf die Ansprüche des Benutzers zugreifen, indem sie das Principal-Objekt in ein Map-Objekt umwandelt. Das Map-Objekt ordnet jeden Anspruchstyp einer Sammlung der Ansprüche für diesen Typ zu. Im folgenden Code ist `request` eine Instanz von `HttpServletRequest`.

```java
Map<String, Collection<String>> map = (Map<String, Collection<String>>) request.getUserPrincipal();
```

Nun können Sie das `Map`-Objekt auf bestimmte Ansprüche überprüfen. Der folgende Codeausschnitt durchläuft beispielsweise alle Anspruchstypen und gibt den Inhalt der einzelnen Sammlungen aus.

```java
for (Object key : map.keySet()) {
        Object value = map.get(key);
        if (value != null && value instanceof Collection {
            Collection claims = (Collection) value;
            for (Object claim : claims) {
                System.out.println(claims);
            }
        }
    }
```

Verwenden Sie den `/.auth/ext/logout`-Pfad, um Benutzer abzumelden. Lesen Sie die Dokumentation unter [Erweiterte Verwendung der Authentifizierung und Autorisierung in Azure App Service](./app-service-authentication-how-to.md), um andere Aktionen auszuführen. Es gibt auch offizielle Dokumentation zur Tomcat [HttpServletRequest-Schnittstelle](https://tomcat.apache.org/tomcat-5.5-doc/servletapi/javax/servlet/http/HttpServletRequest.html) und ihren Methoden. Die folgenden Servletmethoden werden auch basierend auf Ihrer App Service-Konfiguration aktualisiert:

```java
public boolean isSecure()
public String getRemoteAddr()
public String getRemoteHost()
public String getScheme()
public int getServerPort()
```

Um dieses Feature zu deaktivieren, erstellen Sie eine Anwendungseinstellung mit dem Namen `WEBSITE_AUTH_SKIP_PRINCIPAL` und dem Wert `1`. Um alle Servletfilter zu deaktivieren, die von App Service hinzugefügt wurden, erstellen Sie eine Einstellung namens `WEBSITE_SKIP_FILTERS` mit dem Wert `1`.

### <a name="configure-tlsssl"></a>Konfigurieren von TLS/SSL

Befolgen Sie die Anweisungen unter [Schützen eines benutzerdefinierten DNS-Namens mit einer TLS-Bindung in Azure App Service](configure-ssl-bindings.md), um ein vorhandenes TLS/SSL-Zertifikat hochzuladen und dieses an den Domänennamen Ihrer Anwendung zu binden. Standardmäßig lässt Ihre Anwendung dennoch HTTP-Verbindungen zu. Befolgen Sie die spezifischen Schritte im Tutorial, um SSL und TLS zu erzwingen.

### <a name="use-keyvault-references"></a>Verwenden von KeyVault-Verweisen

[Azure KeyVault](../key-vault/general/overview.md) bietet eine zentrale Verwaltung von Geheimnissen mit Zugriffsrichtlinien und Überprüfungsverlauf. Sie können in KeyVault Geheimnisse (wie Kennwörter oder Verbindungszeichenfolgen) speichern und über Umgebungsvariablen auf diese Geheimnisse in Ihrer Anwendung zugreifen.

Befolgen Sie zunächst die Anweisungen zum [Gewähren des Zugriffs auf KeyVault für Ihre App](app-service-key-vault-references.md#granting-your-app-access-to-key-vault) und zum [Erstellen eines KeyVault-Verweises auf Ihr Geheimnis in einer Anwendungseinstellung](app-service-key-vault-references.md#reference-syntax). Sie können überprüfen, ob der Verweis auf das Geheimnis aufgelöst wird, indem Sie die Umgebungsvariable ausgeben, während Sie remote auf das App Service-Terminal zugreifen.

Um diese Geheimnisse in Ihre Spring- oder Tomcat-Konfigurationsdatei einzufügen, verwenden Sie die Syntax der Umgebungsvariablen (`${MY_ENV_VAR}`). Informationen zu Spring-Konfigurationsdateien finden Sie in dieser Dokumentation zu [externalisierten Konfigurationen](https://docs.spring.io/spring-boot/docs/current/reference/html/boot-features-external-config.html).


## <a name="configure-apm-platforms"></a>Konfigurieren von APM-Plattformen

In diesem Abschnitt wird veranschaulicht, wie Sie Java-Anwendungen, die in Azure App Service für Linux bereitgestellt werden, mit den Plattformen NewRelic und AppDynamics für die Überwachung der Anwendungsleistung (Application Performance Monitoring, APM) verbinden.

### <a name="configure-new-relic"></a>Konfigurieren von NewRelic

1. Erstellen Sie unter [NewRelic.com](https://newrelic.com/signup) ein New Relic-Konto.
2. Laden Sie den Java-Agent von NewRelic herunter. Der Dateiname hat etwa das Format *newrelic-java-x.x.x.zip*.
3. Kopieren Sie Ihren Lizenzschlüssel. Sie benötigen ihn später zum Konfigurieren des Agents.
4. Verwenden Sie die [Kudu-Konsole](https://github.com/projectkudu/kudu/wiki/Kudu-console), um ein neues Verzeichnis namens */home/site/wwwroot/apm* zu erstellen.
5. Laden Sie die entpackten Dateien für den New Relic-Java-Agent in ein Verzeichnis unter */home/site/wwwroot/apm* hoch. Die Dateien für Ihren Agenten sollten Sie unter */home/site/wwwroot/apm/newrelic* finden.
6. Ändern Sie die YAML-Datei unter */home/site/wwwroot/apm/newrelic/newrelic.yml*, und ersetzen Sie den Platzhalter-Lizenzwert durch Ihren eigenen Lizenzschlüssel.
7. Navigieren Sie im Azure-Portal zu Ihrer Anwendung in App Service, und erstellen Sie eine neue Anwendungseinstellung.
    - Wenn für Ihre App **Java SE** verwendet wird, sollten Sie die Umgebungsvariable `JAVA_OPTS` mit dem Wert `-javaagent:/home/site/wwwroot/apm/newrelic/newrelic.jar` erstellen.
    - Wenn **Tomcat** verwendet wird, sollten Sie die Umgebungsvariable `CATALINA_OPTS` mit dem Wert `-javaagent:/home/site/wwwroot/apm/newrelic/newrelic.jar` erstellen.

### <a name="configure-appdynamics"></a>Konfigurieren von AppDynamics

1. Erstellen Sie unter [AppDynamics.com](https://www.appdynamics.com/community/register/) ein AppDynamics-Konto.
2. Laden Sie den Java-Agent von der AppDynamics-Website herunter. Der Dateiname hat etwa das Format *AppServerAgent-x.x.x.xxxxx.zip*.
3. Verwenden Sie die [Kudu-Konsole](https://github.com/projectkudu/kudu/wiki/Kudu-console), um ein neues Verzeichnis namens */home/site/wwwroot/apm* zu erstellen.
4. Laden Sie die Dateien für den Java-Agent in ein Verzeichnis unter */home/site/wwwroot/apm* herunter. Die Dateien für Ihren Agenten sollten Sie unter */home/site/wwwroot/apm/appdynamics* finden.
5. Navigieren Sie im Azure-Portal zu Ihrer Anwendung in App Service, und erstellen Sie eine neue Anwendungseinstellung.
    - Erstellen Sie bei Verwendung von **Java SE** die Umgebungsvariable `JAVA_OPTS` mit dem Wert `-javaagent:/home/site/wwwroot/apm/appdynamics/javaagent.jar -Dappdynamics.agent.applicationName=<app-name>`, wobei `<app-name>` für Ihren App Service-Namen steht.
    - Erstellen Sie bei Verwendung von **Tomcat** die Umgebungsvariable `CATALINA_OPTS` mit dem Wert `-javaagent:/home/site/wwwroot/apm/appdynamics/javaagent.jar -Dappdynamics.agent.applicationName=<app-name>`, wobei `<app-name>` für Ihren App Service-Namen steht.

>  Falls Sie bereits über eine Umgebungsvariable für `JAVA_OPTS` oder `CATALINA_OPTS` verfügen, können Sie die Option `-javaagent:/...` am Ende des aktuellen Werts anhängen.

## <a name="data-sources"></a>Datenquellen

### <a name="tomcat"></a>Tomcat

Diese Anweisungen gelten für alle Datenbankverbindungen. Für die Platzhalter müssen der Treiberklassenname und die JAR-Datei der gewählten Datenbank angegeben werden. In der folgenden Tabelle finden Sie Klassennamen und Treiberdownloads für gängige Datenbanken:

| Datenbank   | Treiberklassenname                             | JDBC-Treiber                                                                      |
|------------|-----------------------------------------------|------------------------------------------------------------------------------------------|
| PostgreSQL | `org.postgresql.Driver`                        | [Download](https://jdbc.postgresql.org/download.html)                                    |
| MySQL      | `com.mysql.jdbc.Driver`                        | [Download](https://dev.mysql.com/downloads/connector/j/) (Wählen Sie „Platform Independent“ (Plattformunabhängig) aus.) |
| SQL Server | `com.microsoft.sqlserver.jdbc.SQLServerDriver` | [Download](/sql/connect/jdbc/download-microsoft-jdbc-driver-for-sql-server?view=sql-server-2017#download)                                                           |

Wenn Sie Tomcat für die Verwendung von Java Database Connectivity (JDBC) oder der Java-Persistenz-API (JPA) konfigurieren möchten, passen Sie zuerst die Umgebungsvariable `CATALINA_OPTS` an, die von Tomcat beim Start eingelesen wird. Diese Werte können mithilfe einer App-Einstellung im [App Service-Maven-Plug-In](https://github.com/Microsoft/azure-maven-plugins/blob/develop/azure-webapp-maven-plugin/README.md) festgelegt werden:

```xml
<appSettings>
    <property>
        <name>CATALINA_OPTS</name>
        <value>"$CATALINA_OPTS -Ddbuser=${DBUSER} -Ddbpassword=${DBPASSWORD} -DconnURL=${CONNURL}"</value>
    </property>
</appSettings>
```

Alternativ können Sie die Umgebungsvariablen im Azure-Portal auf der Seite **Konfiguration** > **Anwendungseinstellungen** festlegen.

Legen Sie als Nächstes fest, ob die Datenquelle nur für eine einzelne Anwendung oder für alle im Tomcat-Servlet ausgeführten Anwendungen verfügbar sein soll.

#### <a name="application-level-data-sources"></a>Datenquellen auf Anwendungsebene

1. Erstellen Sie eine *context.xml*-Datei im Verzeichnis *META-INF/* Ihres Projekts. Erstellen Sie das Verzeichnis *META-INF/* , falls es noch nicht vorhanden ist.

2. Fügen Sie in *context.xml* ein Element vom Typ `Context` hinzu, um die Datenquelle mit einer JNDI-Adresse zu verknüpfen. Ersetzen Sie den Platzhalter `driverClassName` durch den Klassennamen Ihres Treibers aus der obigen Tabelle.

    ```xml
    <Context>
        <Resource
            name="jdbc/dbconnection"
            type="javax.sql.DataSource"
            url="${dbuser}"
            driverClassName="<insert your driver class name>"
            username="${dbpassword}"
            password="${connURL}"
        />
    </Context>
    ```

3. Aktualisieren Sie die Datei *web.xml* Ihrer Anwendung, sodass die Datenquelle in Ihrer Anwendung verwendet wird.

    ```xml
    <resource-env-ref>
        <resource-env-ref-name>jdbc/dbconnection</resource-env-ref-name>
        <resource-env-ref-type>javax.sql.DataSource</resource-env-ref-type>
    </resource-env-ref>
    ```

#### <a name="finalize-configuration"></a>Abschließen der Konfiguration

Platzieren Sie abschließend die JAR-Dateien des Treibers im Tomcat-Klassenpfad, und starten Sie Ihre App Service-Instanz neu. Stellen Sie sicher, dass die JDBC-Treiberdateien für den Tomcat-Klassenladeprogramm verfügbar sind, indem Sie sie im Verzeichnis */home/tomcat/lib* ablegen. (Erstellen Sie dieses Verzeichnis, falls es noch nicht vorhanden ist.) Um diese Dateien in Ihre App Service-Instanz hochzuladen, gehen Sie folgendermaßen vor:

1. Installieren Sie in der [Cloud Shell](https://shell.azure.com) die Web-App-Erweiterung:

    ```azurecli-interactive
    az extension add -–name webapp
    ```

2. Führen Sie den folgenden CLI-Befehl aus, um einen SSH-Tunnel von Ihrem lokalen System zu App Service zu erstellen:

    ```azurecli-interactive
    az webapp remote-connection create --resource-group <resource-group-name> --name <app-name> --port <port-on-local-machine>
    ```

3. Stellen Sie mit Ihrem SFTP-Client eine Verbindung mit dem lokalen Tunnelport her, und laden Sie die Dateien in den Ordner */home/tomcat/lib* hoch.

Alternativ können Sie zum Hochladen des JDBC-Treibers auch einen FTP-Client verwenden. Führen Sie dazu die [Schritte zum Abrufen Ihrer FTP-Anmeldeinformationen](deploy-configure-credentials.md) aus.

## <a name="configuring-tomcat"></a>Konfigurieren von Tomcat

Um die Datei `server.xml` oder andere Konfigurationsdateien von Tomcat zu bearbeiten, notieren Sie zuerst Ihre Tomcat-Hauptversion im Portal.

1. Suchen Sie das Tomcat-Stammverzeichnis für Ihre Version, indem Sie den Befehl `env` ausführen. Suchen Sie nach der Umgebungsvariable, die mit `AZURE_TOMCAT` beginnt und Ihrer Hauptversion entspricht. Beispielsweise verweist `AZURE_TOMCAT85_HOME` auf das Tomcat-Verzeichnis für Tomcat 8.5.
1. Nachdem Sie das Tomcat-Stammverzeichnis für Ihre Version identifiziert haben, kopieren Sie das Konfigurationsverzeichnis nach `D:\home`. Wenn beispielsweise `AZURE_TOMCAT85_HOME` den Wert `D:\Program Files (x86)\apache-tomcat-8.5.37` aufweist, lautet der neue Pfad des kopierten Verzeichnisses `D:\home\apache-tomcat-8.5.37`.

Starten Sie dann Ihre App Service-Instanz neu. Ihre Bereitstellungen sollten wie vorher in `D:\home\site\wwwroot\webapps` ausgeführt werden.

## <a name="configure-java-se"></a>Konfigurieren von Java SE

Wenn Sie eine JAR-Anwendung in Java SE unter Windows ausführen, wird `server.port` beim Start der Anwendung als Befehlszeilenoption übergeben. Der HTTP-Port kann manuell anhand der Umgebungsvariablen `HTTP_PLATFORM_PORT` aufgelöst werden. Der Wert dieser Umgebungsvariablen ist der HTTP-Port, an dem Ihre Anwendung lauschen muss. 

## <a name="java-runtime-statement-of-support"></a>Unterstützungserklärung für die Java-Runtime

### <a name="jdk-versions-and-maintenance"></a>JDK-Versionen und -Wartung

Als Java Development Kit (JDK) wird das von [Azul Systems](https://www.azul.com/) bereitgestellte [Zulu](https://www.azul.com/downloads/azure-only/zulu/) von Azure unterstützt.

Größere Versionsupdates werden durch neue Runtimeoptionen in Azure App Service für Windows bereitgestellt. Kunden führen das Update auf diese neueren Versionen von Java durch die Konfiguration ihrer App Service-Bereitstellung durch und müssen durch Tests sicherstellen, dass das größere Update ihren Anforderungen entspricht.

Unterstützte JDKs werden jedes Vierteljahr im Januar, April, Juli und Oktober automatisch gepatcht. Weitere Informationen zu Java in Azure finden Sie in [diesem Supportdokument](/azure/developer/java/fundamentals/java-jdk-long-term-support).

### <a name="security-updates"></a>Sicherheitsupdates

Patches und Fixes für größere Sicherheitsrisiken werden veröffentlicht, sobald sie von Azul Systems zur Verfügung gestellt werden. Ein „größeres“ Sicherheitsrisiko wird durch eine Gesamtbewertung von 9.0 oder höher im [NIST Common Vulnerability Scoring System, Version 2](https://nvd.nist.gov/vuln-metrics/cvss), definiert.

Tomcat 8.0 hat [am 30. September 2018 das Ende der Lebensdauer (End Of Life, EOL) erreicht](https://tomcat.apache.org/tomcat-80-eol.html). Die Runtime ist zwar noch in Azure App Service verfügbar, von Azure werden jedoch keine Sicherheitsupdates mehr auf Tomcat 8.0 angewendet. Migrieren Sie Ihre Anwendungen nach Möglichkeit zu Tomcat 8.5 oder 9.0. Sowohl Tomcat 8.5 als auch Tomcat 9.0 steht in Azure App Service zur Verfügung. Weitere Informationen finden Sie auf der [offiziellen Tomcat-Website](https://tomcat.apache.org/whichversion.html). 

### <a name="deprecation-and-retirement"></a>Einstellung und Außerbetriebnahme

Wenn eine unterstützte Java-Runtime eingestellt wird, erhalten Azure-Entwickler, die die betreffende Runtime verwenden, mindestens sechs Monate vor Außerbetriebnahme der Runtime eine entsprechende Benachrichtigung.

### <a name="local-development"></a>Lokale Entwicklung

Entwickler können die Production Edition des Azul Zulu Enterprise JDK zur lokalen Entwicklung von der [Azul-Downloadsite](https://www.azul.com/downloads/azure-only/zulu/) herunterladen.

### <a name="development-support"></a>Entwicklungsunterstützung

Produktsupport für das von [Azure unterstützte Azul Zulu JDK](https://www.azul.com/downloads/azure-only/zulu/) ist bei der Entwicklung für Azure oder [Azure Stack](https://azure.microsoft.com/overview/azure-stack/) mit einem [qualifizierten Azure-Supportplan](https://azure.microsoft.com/support/plans/) von Microsoft erhältlich.

### <a name="runtime-support"></a>Laufzeitunterstützung

Entwickler können ein [Problem](../azure-portal/supportability/how-to-create-azure-support-request.md) mit den Azul Zulu JDKs über den Azure-Support melden, wenn sie einen [qualifizierten Supportplan](https://azure.microsoft.com/support/plans/) besitzen.

## <a name="next-steps"></a>Nächste Schritte

Dieses Thema behandelt die Java Runtime-Unterstützungsanweisung für Azure App Service unter Windows.

- Weitere Informationen zum Hosten von Webanwendungen mit Azure App Service finden Sie in der [App Service-Übersicht](overview.md).
- Weitere Informationen zur Java-Entwicklung in Azure finden Sie unter [Azure für Java Dev Center](/java/azure/?view=azure-java-stable).

::: zone-end

::: zone pivot="platform-linux"

Mit Azure App Service für Linux können Java-Entwickler schnell ihre mit Tomcat oder Java Standard Edition (SE) paketierten Webanwendungen in einem vollständig verwalteten Linux-basierten Dienst erstellen, bereitstellen und skalieren. Stellen Sie Anwendungen mit Maven-Plug-Ins über die Befehlszeile oder in Editoren wie IntelliJ, Eclipse oder Visual Studio Code bereit.

Dieser Leitfaden enthält wichtige Konzepte und Anleitungen für Java-Entwickler, die in App Service einen integrierten Linux-Container verwenden. Falls Sie Azure App Service noch nicht verwendet haben, arbeiten Sie zunächst die [Java-Schnellstartanleitung](quickstart-java.md) durch.

## <a name="deploying-your-app"></a>Bereitstellen Ihrer App

Sie können das [Maven-Plug-In für Azure App Service](/java/api/overview/azure/maven/azure-webapp-maven-plugin/readme) zum Bereitstellen von JAR- und WAR-Dateien verwenden. Die Bereitstellung mit beliebten IDEs wird außerdem mit [Azure Toolkit für IntelliJ](/azure/developer/java/toolkit-for-intellij/) oder [Azure Toolkit für Eclipse](/azure/developer/java/toolkit-for-eclipse) unterstützt.

In allen anderen Fällen hängt Ihre Bereitstellungsmethode von Ihrem Archivtyp ab:

- Um WAR-Dateien in Tomcat bereitzustellen, verwenden Sie den `/api/wardeploy/`-Endpunkt, um Ihre Archivdatei mit POST zu veröffentlichen. Weitere Informationen zu dieser API finden Sie in [dieser Dokumentation](./deploy-zip.md#deploy-war-file).
- Um JAR-Dateien in den Java SE-Images bereitzustellen, verwenden Sie den `/api/zipdeploy/`-Endpunkt der Kudu-Website. Weitere Informationen zu dieser API finden Sie in [dieser Dokumentation](./deploy-zip.md#rest).

Stellen Sie Ihre WAR- oder JAR-Dateien nicht mithilfe von FTP bereit. Der FTP-Tool dient zum Hochladen von Startskripts, Abhängigkeiten oder anderen Laufzeitdateien. Es ist nicht die optimale Wahl für die Bereitstellung von Web-Apps.

## <a name="logging-and-debugging-apps"></a>Protokollieren und Debuggen von Apps

Leistungsberichte, Datenverkehrsvisualisierungen und Integritätsprüfungen für die einzelnen Apps stehen über das Azure-Portal zur Verfügung. Weitere Informationen hierzu finden Sie unter [Azure App Service-Pläne – Diagnoseübersicht](overview-diagnostics.md).

### <a name="ssh-console-access"></a>SSH-Konsolenzugriff

[!INCLUDE [Open SSH session in browser](../../includes/app-service-web-ssh-connect-builtin-no-h.md)]

### <a name="stream-diagnostic-logs"></a>Streamen von Diagnoseprotokollen

[!INCLUDE [Access diagnostic logs](../../includes/app-service-web-logs-access-linux-no-h.md)]

Weitere Informationen finden Sie unter [Streamen von Protokollen in Cloud Shell](troubleshoot-diagnostic-logs.md#in-cloud-shell).

### <a name="app-logging"></a>App-Protokollierung

Aktivieren Sie die [Anwendungsprotokollierung](troubleshoot-diagnostic-logs.md#enable-application-logging-windows) über das Azure-Portal oder die [Azure-Befehlszeilenschnittstelle](/cli/azure/webapp/log#az-webapp-log-config), um App Service so zu konfigurieren, dass die Streams mit der Standardkonsolenausgabe und den Standardkonsolenfehlern Ihrer Anwendung in das lokale Dateisystem oder Azure Blob Storage geschrieben werden. Die Protokollierung in der lokalen App Service-Dateisysteminstanz wird 12 Stunden nach der Konfiguration deaktiviert. Wenn Sie eine längere Beibehaltung benötigen, konfigurieren Sie die Anwendung für die Ausgabe in einen Blob Storage-Container. Ihre Java- und Tomcat-App-Protokolle befinden sich im Verzeichnis */home/LogFiles/Application/* .

>[!NOTE]
>Das Deaktivieren der Protokollierung auf dem lokalen App Service-Dateisystem nach 12 Stunden betrifft nur App Services auf Windows-Basis. Die Azure Blob Storage-Protokollierung für App Services auf Linux-Basis kann nur mit [Azure Monitor (Vorschauversion)](./troubleshoot-diagnostic-logs.md#send-logs-to-azure-monitor-preview) konfiguriert werden. 

Wenn Ihre Anwendung [Logback](https://logback.qos.ch/) oder [Log4j](https://logging.apache.org/log4j) für die Ablaufverfolgung verwendet, können Sie diese Ablaufverfolgungen mithilfe der Konfigurationsanweisungen für das Protokollierungsframework unter [Untersuchen von Java-Ablaufverfolgungsprotokollen in Application Insights](../azure-monitor/app/java-trace-logs.md) zur Überprüfung an Azure Application Insights weiterleiten.

### <a name="troubleshooting-tools"></a>Problembehandlungstools

Die integrierten Java-Images basieren auf dem [Alpine Linux](https://alpine-linux.readthedocs.io/en/latest/getting_started.html)-Betriebssystem. Verwenden Sie den `apk`-Paket-Manager zur Installation von Tools oder Befehlen zur Problembehandlung.

### <a name="flight-recorder"></a>Flight Recorder

In allen Linux Java-Images auf App Service ist Zulu Flight Recorder installiert. So können Sie sich ganz einfach mit der JVM verbinden und eine Profileraufnahme erstellen oder einen Heapdump generieren.

#### <a name="timed-recording"></a>Zeitgesteuerte Aufzeichnung

Stellen Sie zuerst eine SSH-Verbindung mit Ihrem App Service her und führen Sie den `jcmd`-Befehl aus, um eine Liste aller laufenden Java-Prozesse zu sehen. Neben jcmd sollten Sie auch Ihre ausgeführte Java-Anwendung mit einer Prozess-ID (pid) sehen.

```shell
078990bbcd11:/home# jcmd
Picked up JAVA_TOOL_OPTIONS: -Djava.net.preferIPv4Stack=true
147 sun.tools.jcmd.JCmd
116 /home/site/wwwroot/app.jar
```

Führen Sie den folgenden Befehl aus, um eine 30-sekündige Aufnahme der JVM zu beginnen. Damit wird die JVM profiliert und eine JFR-Datei namens *jfr_example.jfr* im Stammverzeichnis erstellt. (Ersetzen Sie 116 mit der pid Ihrer Java-App.)

```shell
jcmd 116 JFR.start name=MyRecording settings=profile duration=30s filename="/home/jfr_example.jfr"
```

Während des 30-sekündigen Intervalls können Sie durch Ausführung von `jcmd 116 JFR.check` überprüfen, ob die Aufnahme wirklich stattfindet. Daraufhin werden alle Aufzeichnungen für den entsprechenden Java-Prozess gezeigt.

#### <a name="continuous-recording"></a>Fortlaufende Aufzeichnung

Sie können Zulu Flight Recorder einsetzen, um Ihre Java-Anwendung mit minimalen Auswirkungen auf die Laufzeitleistung zu profilieren ([Quelle](https://assets.azul.com/files/Zulu-Mission-Control-data-sheet-31-Mar-19.pdf)). Führen Sie dafür den folgenden Azure CLI-Befehl aus, um eine App-Einstellung namens JAVA_OPTS mit der notwendigen Konfiguration zu erschaffen. Der Inhalt der JAVA_OPTS-App-Einstellung wird an den `java`-Befehl übermittelt, wenn Ihre App gestartet wird.

```azurecli
az webapp config appsettings set -g <your_resource_group> -n <your_app_name> --settings JAVA_OPTS=-XX:StartFlightRecording=disk=true,name=continuous_recording,dumponexit=true,maxsize=1024m,maxage=1d
```

Nachdem die Aufzeichnung gestartet wurde, können Sie die aktuellen Aufzeichnungsdaten jederzeit mithilfe des `JFR.dump`-Befehls sichern.

```shell
jcmd <pid> JFR.dump name=continuous_recording filename="/home/recording1.jfr"
```

Weitere Informationen finden Sie in der [Jcmd-Befehlsreferenz](https://docs.oracle.com/javacomponents/jmc-5-5/jfr-runtime-guide/comline.htm#JFRRT190).

### <a name="analyzing-recordings"></a>Analysieren der Aufzeichnungen

Verwenden Sie [FTPS](deploy-ftp.md) zum Herunterladen Ihrer JFR-Datei auf Ihren lokalen Computer. Laden Sie zum analysieren der JFR-Datei [Zulu Mission Control](https://www.azul.com/products/zulu-mission-control/) herunter und installieren Sie es. Weitere Informationen zu Zulu Mission Control finden Sie in [„Azul-Dokumentation“](https://docs.azul.com/zmc/) und den [Installationsanweisungen](/java/azure/jdk/java-jdk-flight-recorder-and-mission-control).

## <a name="customization-and-tuning"></a>Anpassung und Optimierung

Azure App Service für Linux unterstützt eine sofort verfügbare Optimierung und Anpassung über das Azure-Portal und die CLI. Lesen Sie die folgenden Artikel zur nicht Java-spezifischen Web-App-Konfiguration:

- [Konfigurieren von App-Einstellungen](configure-common.md#configure-app-settings)
- [Einrichten einer benutzerdefinierten Domäne](app-service-web-tutorial-custom-domain.md)
- [Konfigurieren von SSL-Bindungen](configure-ssl-bindings.md)
- [Hinzufügen eines CDN](../cdn/cdn-add-to-web-app.md)
- [Konfigurieren der Kudu-Website](https://github.com/projectkudu/kudu/wiki/Configurable-settings#linux-on-app-service-settings)

### <a name="set-java-runtime-options"></a>Festlegen von Java-Runtimeoptionen

Um reservierten Arbeitsspeicher oder andere JVM-Runtimeoptionen in den Tomcat- und Java SE-Umgebungen festzulegen, erstellen Sie mit den Optionen eine [App-Einstellung](configure-common.md#configure-app-settings) mit dem Namen `JAVA_OPTS`. App Service für Linux übergibt diese Einstellung beim Start als Umgebungsvariable an die Java-Runtime.

Erstellen Sie im Azure-Portal unter **Anwendungseinstellungen** für die Web-App eine neue App-Einstellung namens `JAVA_OPTS`, welche die zusätzlichen Einstellungen enthält, z.B. `-Xms512m -Xmx1204m`.

Um die App-Einstellung über das Maven-Plug-In zu konfigurieren, fügen Sie Einstellungs-/Werttags im Azure-Plug-In-Abschnitt hinzu. Im folgenden Beispiel werden bestimmte Mindest- und Höchstwerte für die Java-Heapgröße festgelegt:

```xml
<appSettings>
    <property>
        <name>JAVA_OPTS</name>
        <value>-Xms512m -Xmx1204m</value>
    </property>
</appSettings>
```

Entwickler, die eine einzige Anwendung mit einem Bereitstellungsslot in ihrem App Service-Plan ausführen, können die folgenden Optionen verwenden:

- B1- und S1-Instanzen: `-Xms1024m -Xmx1024m`
- B2- und S2-Instanzen: `-Xms3072m -Xmx3072m`
- B3- und S3-Instanzen: `-Xms6144m -Xmx6144m`

Überprüfen Sie beim Optimieren Ihrer Anwendungsheapeinstellungen Ihre App Service-Plandetails, und berücksichtigen Sie mehrere Anwendungs- und Bereitstellungsslotanforderungen, um die optimale Zuordnung von Arbeitsspeicher zu ermitteln.

Wenn Sie eine JAR-Anwendung bereitstellen, sollte sie den Namen *app.jar* tragen, damit das integrierte Image Ihre App ordnungsgemäß identifizieren kann. (Das Maven-Plug-In nimmt diese Umbenennung automatisch vor.) Wenn Sie Ihre JAR-Datei nicht in *app.jar* umbenennen möchten, können Sie ein Shellskript mit dem Befehl zum Ausführen Ihrer JAR-Datei hochladen. Fügen Sie anschließend den vollständigen Pfad zu diesem Skript in das Textfeld [Startdatei](faq-app-service-linux.md#built-in-images) im Abschnitt „Konfiguration“ des Portals ein. Das Startskript wird nicht aus dem Verzeichnis ausgeführt, in dem es abgelegt wurde. Verwenden Sie daher immer absolute Pfade, um auf Dateien in Ihrem Startskript zu verweisen (z. B.: `java -jar /home/myapp/myapp.jar`).

### <a name="turn-on-web-sockets"></a>Aktivieren von Websockets

Aktivieren Sie die Unterstützung für Websockets im Azure-Portal in den **Anwendungseinstellungen** für die Anwendung. Sie müssen die Anwendung neu starten, damit die Einstellung in Kraft tritt.

Aktivieren Sie die Websocketunterstützung über die Azure CLI mithilfe des folgenden Befehls:

```azurecli-interactive
az webapp config set --name <app-name> --resource-group <resource-group-name> --web-sockets-enabled true
```

Starten Sie Ihre Anwendung anschließend neu:

```azurecli-interactive
az webapp stop --name <app-name> --resource-group <resource-group-name>
az webapp start --name <app-name> --resource-group <resource-group-name>
```

### <a name="set-default-character-encoding"></a>Festlegen der Standardzeichencodierung

Erstellen Sie im Azure-Portal unter **Anwendungseinstellungen** für die Web-App eine neue App-Einstellung namens `JAVA_OPTS` mit dem Wert `-Dfile.encoding=UTF-8`.

Alternativ können Sie die App-Einstellung mit dem App Service-Maven-Plug-In konfigurieren. Fügen Sie die name- und value-Tags der Einstellung in der Plug-In-Konfiguration hinzu:

```xml
<appSettings>
    <property>
        <name>JAVA_OPTS</name>
        <value>-Dfile.encoding=UTF-8</value>
    </property>
</appSettings>
```

### <a name="adjust-startup-timeout"></a>Anpassen des Timeoutwerts für den Start

Wenn Ihre Java-Anwendung besonders groß ist, sollten Sie das Zeitlimit für den Start heraufsetzen. Erstellen Sie zu diesem Zweck eine Anwendungseinstellung `WEBSITES_CONTAINER_START_TIME_LIMIT`, und legen Sie sie auf die Anzahl Sekunden fest, die App Service vor dem Timeout warten soll. Der Maximalwert beträgt `1800` Sekunden.

### <a name="pre-compile-jsp-files"></a>Vorkompilieren von JSP-Dateien

Zur Verbesserung der Leistung von Tomcat-Anwendungen können Sie die JSP-Dateien kompilieren, bevor sie in App Service bereitgestellt werden. Sie können das von Apache Sling bereitgestellte [Maven-Plug-In](https://sling.apache.org/components/jspc-maven-plugin/plugin-info.html) oder diese [Ant-Builddatei](https://tomcat.apache.org/tomcat-9.0-doc/jasper-howto.html#Web_Application_Compilation) verwenden.

## <a name="secure-applications"></a>Sichere Anwendungen

Für App Service für Linux ausgeführte Java-Anwendungen gelten dieselben [bewährten Sicherheitsmethoden](../security/fundamentals/paas-applications-using-app-services.md) wie für andere Anwendungen.

### <a name="authenticate-users-easy-auth"></a>Authentifizieren von Benutzern (einfache Authentifizierung)

Richten Sie die App-Authentifizierung im Azure-Portal über die Option **Authentifizierung und Autorisierung** ein. Von dort aus können Sie die Authentifizierung über Azure Active Directory oder soziale Netzwerke wie Facebook, Google oder GitHub aktivieren. Die Konfiguration des Azure-Portals funktioniert nur, wenn Sie einen einzelnen Authentifizierungsanbieter konfigurieren. Weitere Informationen finden Sie unter [Konfigurieren Ihrer App Service-App zur Verwendung der Azure Active Directory-Anmeldung](configure-authentication-provider-aad.md) und in den entsprechenden Artikeln für andere Identitätsanbieter. Wenn Sie mehrere Anmeldungsanbieter aktivieren müssen, befolgen Sie die Anweisungen im Artikel [Anpassen der Authentifizierung und Autorisierung in Azure App Service](app-service-authentication-how-to.md).

#### <a name="tomcat"></a>Tomcat

Ihre Tomcat-Anwendung kann direkt aus dem Servlet auf die Ansprüche des Benutzers zugreifen, indem sie das Principal-Objekt in ein Map-Objekt umwandelt. Das Map-Objekt ordnet jeden Anspruchstyp einer Sammlung der Ansprüche für diesen Typ zu. Im folgenden Code ist `request` eine Instanz von `HttpServletRequest`.

```java
Map<String, Collection<String>> map = (Map<String, Collection<String>>) request.getUserPrincipal();
```

Nun können Sie das `Map`-Objekt auf bestimmte Ansprüche überprüfen. Der folgende Codeausschnitt durchläuft beispielsweise alle Anspruchstypen und gibt den Inhalt der einzelnen Sammlungen aus.

```java
for (Object key : map.keySet()) {
        Object value = map.get(key);
        if (value != null && value instanceof Collection {
            Collection claims = (Collection) value;
            for (Object claim : claims) {
                System.out.println(claims);
            }
        }
    }
```

Verwenden Sie den `/.auth/ext/logout`-Pfad, um Benutzer abzumelden. Lesen Sie die Dokumentation unter [Erweiterte Verwendung der Authentifizierung und Autorisierung in Azure App Service](./app-service-authentication-how-to.md), um andere Aktionen auszuführen. Es gibt auch offizielle Dokumentation zur Tomcat [HttpServletRequest-Schnittstelle](https://tomcat.apache.org/tomcat-5.5-doc/servletapi/javax/servlet/http/HttpServletRequest.html) und ihren Methoden. Die folgenden Servletmethoden werden auch basierend auf Ihrer App Service-Konfiguration aktualisiert:

```java
public boolean isSecure()
public String getRemoteAddr()
public String getRemoteHost()
public String getScheme()
public int getServerPort()
```

Um dieses Feature zu deaktivieren, erstellen Sie eine Anwendungseinstellung mit dem Namen `WEBSITE_AUTH_SKIP_PRINCIPAL` und dem Wert `1`. Um alle Servletfilter zu deaktivieren, die von App Service hinzugefügt wurden, erstellen Sie eine Einstellung namens `WEBSITE_SKIP_FILTERS` mit dem Wert `1`.

#### <a name="spring-boot"></a>Spring Boot

Spring Boot-Entwickler können die [Azure Active Directory-Startoption für Spring Boot](/java/azure/spring-framework/configure-spring-boot-starter-java-app-with-azure-active-directory?view=azure-java-stable) verwenden, um Anwendungen mithilfe von vertrauten Spring Security-Anmerkungen und -APIs absichern. Achten Sie darauf, dass Sie die maximale Headergröße in Ihrer *application.properties*-Datei erhöhen. Wir empfehlen den Wert `16384`.

### <a name="configure-tlsssl"></a>Konfigurieren von TLS/SSL

Befolgen Sie die Anweisungen unter [Schützen eines benutzerdefinierten DNS-Namens mit einer SSL-Bindung in Azure App Service](configure-ssl-bindings.md), um ein vorhandenes SSL-Zertifikat hochzuladen und dieses an den Domänennamen Ihrer Anwendung zu binden. Standardmäßig lässt Ihre Anwendung dennoch HTTP-Verbindungen zu. Befolgen Sie die spezifischen Schritte im Tutorial, um SSL und TLS zu erzwingen.

### <a name="use-keyvault-references"></a>Verwenden von KeyVault-Verweisen

[Azure KeyVault](../key-vault/general/overview.md) bietet eine zentrale Verwaltung von Geheimnissen mit Zugriffsrichtlinien und Überprüfungsverlauf. Sie können in KeyVault Geheimnisse (wie Kennwörter oder Verbindungszeichenfolgen) speichern und über Umgebungsvariablen auf diese Geheimnisse in Ihrer Anwendung zugreifen.

Befolgen Sie zunächst die Anweisungen zum [Gewähren des Zugriffs auf KeyVault für Ihre App](app-service-key-vault-references.md#granting-your-app-access-to-key-vault) und zum [Erstellen eines KeyVault-Verweises auf Ihr Geheimnis in einer Anwendungseinstellung](app-service-key-vault-references.md#reference-syntax). Sie können überprüfen, ob der Verweis auf das Geheimnis aufgelöst wird, indem Sie die Umgebungsvariable ausgeben, während Sie remote auf das App Service-Terminal zugreifen.

Um diese Geheimnisse in Ihre Spring- oder Tomcat-Konfigurationsdatei einzufügen, verwenden Sie die Syntax der Umgebungsvariablen (`${MY_ENV_VAR}`). Informationen zu Spring-Konfigurationsdateien finden Sie in dieser Dokumentation zu [externalisierten Konfigurationen](https://docs.spring.io/spring-boot/docs/current/reference/html/boot-features-external-config.html).

### <a name="using-the-java-key-store"></a>Verwenden des Java-Keystores

Standardmäßig werden alle öffentlichen oder privaten Zertifikate, die [in App Service Linux hochgeladen](configure-ssl-certificate.md) wurden, beim Start des Containers in den jeweiligen Java-Keystore geladen. Nach dem Hochladen Ihres Zertifikats müssen Sie Ihre App Service-Instanz neu starten, damit es in den Java-Keystore geladen wird. Öffentliche Zertifikate werden unter `$JAVA_HOME/jre/lib/security/cacerts` in den Schlüsselspeicher geladen, und private Zertifikate werden unter `$JAVA_HOME/lib/security/client.jks` gespeichert.

Zur Verschlüsselung Ihrer JDBC-Verbindung mit Zertifikaten im Java-Schlüsselspeicher sind möglicherweise zusätzliche Konfigurationsschritte erforderlich. Weitere Informationen finden Sie in der Dokumentation für den gewählten JDBC-Treiber.

- [PostgreSQL](https://jdbc.postgresql.org/documentation/head/ssl-client.html)
- [SQL Server](/sql/connect/jdbc/connecting-with-ssl-encryption?view=sql-server-ver15)
- [MySQL](https://dev.mysql.com/doc/connector-j/5.1/en/connector-j-reference-using-ssl.html)
- [MongoDB](https://mongodb.github.io/mongo-java-driver/3.4/driver/tutorials/ssl/)
- [Cassandra](https://docs.datastax.com/en/developer/java-driver/4.3/)

#### <a name="initializing-the-java-key-store"></a>Initialisieren des Java-Schlüsselspeichers

Zum Initialisieren des `import java.security.KeyStore`-Objekts laden Sie die Schlüsselspeicherdatei mit dem Kennwort. Das Standardkennwort für beide Schlüsselspeicher lautet „changeit“.

```java
KeyStore keyStore = KeyStore.getInstance("jks");
keyStore.load(
    new FileInputStream(System.getenv("JAVA_HOME")+"/lib/security/cacets"),
    "changeit".toCharArray());

KeyStore keyStore = KeyStore.getInstance("pkcs12");
keyStore.load(
    new FileInputStream(System.getenv("JAVA_HOME")+"/lib/security/client.jks"),
    "changeit".toCharArray());
```

#### <a name="manually-load-the-key-store"></a>Manuelles Laden des Schlüsselspeichers

Sie können Zertifikate manuell in den Schlüsselspeicher laden. Erstellen Sie die App-Einstellung `SKIP_JAVA_KEYSTORE_LOAD` mit dem Wert `1`, damit die Zertifikate nicht automatisch von App Service in den Keystore geladen werden. Alle öffentlichen Zertifikate, die über das Azure-Portal in App Service hochgeladen werden, werden unter `/var/ssl/certs/` gespeichert. Private Zertifikate werden unter `/var/ssl/private/` gespeichert.

Sie können mit dem Java-Schlüsseltool interagieren oder Debuggingschritte ausführen, indem Sie eine [SSH-Verbindung](configure-linux-open-ssh-session.md) mit Ihrer App Service-Instanz herstellen und den Befehl `keytool`ausführen. Eine Liste mit Befehlen finden Sie in der [Dokumentation des Schlüsseltools](https://docs.oracle.com/javase/8/docs/technotes/tools/unix/keytool.html). Weitere Informationen zur Keystore-API finden Sie in der [offiziellen Dokumentation](https://docs.oracle.com/javase/8/docs/api/java/security/KeyStore.html).

## <a name="configure-apm-platforms"></a>Konfigurieren von APM-Plattformen

In diesem Abschnitt wird veranschaulicht, wie Sie Java-Anwendungen, die in Azure App Service für Linux bereitgestellt werden, mit den Plattformen NewRelic und AppDynamics für die Überwachung der Anwendungsleistung (Application Performance Monitoring, APM) verbinden.

### <a name="configure-new-relic"></a>Konfigurieren von NewRelic

1. Erstellen Sie ein NewRelic-Konto unter [NewRelic.com](https://newrelic.com/signup).
2. Laden Sie den Java-Agent von NewRelic herunter. Der Dateiname hat etwa das Format *newrelic-java-x.x.x.zip*.
3. Kopieren Sie Ihren Lizenzschlüssel. Sie benötigen ihn später zum Konfigurieren des Agents.
4. [Stellen Sie eine SSH-Verbindung mit Ihrer App Service-Instanz her](configure-linux-open-ssh-session.md), und erstellen Sie das neue Verzeichnis */home/site/wwwroot/apm*.
5. Laden Sie die entpackten Dateien für den NewRelic-Java-Agent in ein Verzeichnis unter */home/site/wwwroot/apm* herunter. Die Dateien für Ihren Agenten sollten Sie unter */home/site/wwwroot/apm/newrelic* finden.
6. Ändern Sie die YAML-Datei unter */home/site/wwwroot/apm/newrelic/newrelic.yml*, und ersetzen Sie den Platzhalter-Lizenzwert durch Ihren eigenen Lizenzschlüssel.
7. Navigieren Sie im Azure-Portal zu Ihrer Anwendung in App Service, und erstellen Sie eine neue Anwendungseinstellung.
    - Wenn für Ihre App **Java SE** verwendet wird, sollten Sie die Umgebungsvariable `JAVA_OPTS` mit dem Wert `-javaagent:/home/site/wwwroot/apm/newrelic/newrelic.jar` erstellen.
    - Wenn **Tomcat** verwendet wird, sollten Sie die Umgebungsvariable `CATALINA_OPTS` mit dem Wert `-javaagent:/home/site/wwwroot/apm/newrelic/newrelic.jar` erstellen.

### <a name="configure-appdynamics"></a>Konfigurieren von AppDynamics

1. Erstellen Sie unter [AppDynamics.com](https://www.appdynamics.com/community/register/) ein AppDynamics-Konto.
2. Laden Sie den Java-Agent von der AppDynamics-Website herunter. Der Dateiname hat etwa das Format *AppServerAgent-x.x.x.xxxxx.zip*.
3. [Stellen Sie eine SSH-Verbindung mit Ihrer App Service-Instanz her](configure-linux-open-ssh-session.md), und erstellen Sie das neue Verzeichnis */home/site/wwwroot/apm*.
4. Laden Sie die Dateien für den Java-Agent in ein Verzeichnis unter */home/site/wwwroot/apm* herunter. Die Dateien für Ihren Agenten sollten Sie unter */home/site/wwwroot/apm/appdynamics* finden.
5. Navigieren Sie im Azure-Portal zu Ihrer Anwendung in App Service, und erstellen Sie eine neue Anwendungseinstellung.
    - Erstellen Sie bei Verwendung von **Java SE** die Umgebungsvariable `JAVA_OPTS` mit dem Wert `-javaagent:/home/site/wwwroot/apm/appdynamics/javaagent.jar -Dappdynamics.agent.applicationName=<app-name>`, wobei `<app-name>` für Ihren App Service-Namen steht.
    - Erstellen Sie bei Verwendung von **Tomcat** die Umgebungsvariable `CATALINA_OPTS` mit dem Wert `-javaagent:/home/site/wwwroot/apm/appdynamics/javaagent.jar -Dappdynamics.agent.applicationName=<app-name>`, wobei `<app-name>` für Ihren App Service-Namen steht.

    > [!NOTE]
    > Falls Sie bereits über eine Umgebungsvariable für `JAVA_OPTS` oder `CATALINA_OPTS` verfügen, können Sie die Option `-javaagent:/...` am Ende des aktuellen Werts anhängen.
    
## <a name="configure-jar-applications"></a>Konfigurieren von JAR-Anwendungen

### <a name="starting-jar-apps"></a>Starten von JAR-Apps

Standardmäßig erwartet App Service, dass Ihre JAR-Anwendung den Namen *app.jar* hat. Wenn sie diesen Namen aufweist, wird sie automatisch ausgeführt. Für Maven-Benutzer können Sie den JAR-Namen festlegen, indem Sie in den Abschnitt `<build>` der Datei *pom.xml* die Zeichenfolge `<finalName>app</finalName>` einfügen. [Sie können dies auch in Gradle durchführen](https://docs.gradle.org/current/dsl/org.gradle.api.tasks.bundling.Jar.html#org.gradle.api.tasks.bundling.Jar:archiveFileName), indem Sie die `archiveFileName`-Eigenschaft festlegen.

Falls Sie einen anderen Namen für Ihre JAR-Anwendung wählen, müssen Sie auch den [Startbefehl](faq-app-service-linux.md#built-in-images) angeben, mit dem Ihre JAR-Datei ausgeführt wird. Beispiel: `java -jar my-jar-app.jar`. Sie können den Wert für Ihren Startbefehl im Portal festlegen, indem Sie zu „Konfiguration“ > „Allgemeine Einstellungen“ navigieren oder die Anwendungseinstellung `STARTUP_COMMAND` verwenden.

### <a name="server-port"></a>Serverport

Da App Service für Linux eingehende Anforderungen an Port 80 leitet, sollte Ihre Anwendung auch an Port 80 lauschen. Sie können dies in der Konfiguration Ihrer Anwendung (z. B. Datei*application.properties*  von Spring) oder über Ihren Startbefehl (z. B. `java -jar spring-app.jar --server.port=80`) durchführen. Die folgende Dokumentation enthält Informationen zu gängigen Java-Frameworks:

- [Spring Boot](https://docs.spring.io/spring-boot/docs/current/reference/html/howto-properties-and-configuration.html#howto-use-short-command-line-arguments)
- [SparkJava](http://sparkjava.com/documentation#embedded-web-server)
- [Micronaut](https://docs.micronaut.io/latest/guide/index.html#runningSpecificPort)
- [Play Framework](https://www.playframework.com/documentation/2.6.x/ConfiguringHttps#Configuring-HTTPS)
- [Vertx](https://vertx.io/docs/vertx-core/java/#_start_the_server_listening)
- [Quarkus](https://quarkus.io/guides/application-configuration-guide)

## <a name="data-sources"></a>Datenquellen

### <a name="tomcat"></a>Tomcat

Diese Anweisungen gelten für alle Datenbankverbindungen. Für die Platzhalter müssen der Treiberklassenname und die JAR-Datei der gewählten Datenbank angegeben werden. In der folgenden Tabelle finden Sie Klassennamen und Treiberdownloads für gängige Datenbanken:

| Datenbank   | Treiberklassenname                             | JDBC-Treiber                                                                      |
|------------|-----------------------------------------------|------------------------------------------------------------------------------------------|
| PostgreSQL | `org.postgresql.Driver`                        | [Download](https://jdbc.postgresql.org/download.html)                                    |
| MySQL      | `com.mysql.jdbc.Driver`                        | [Download](https://dev.mysql.com/downloads/connector/j/) (Wählen Sie „Platform Independent“ (Plattformunabhängig) aus.) |
| SQL Server | `com.microsoft.sqlserver.jdbc.SQLServerDriver` | [Download](/sql/connect/jdbc/download-microsoft-jdbc-driver-for-sql-server?view=sql-server-2017#download)                                                           |

Wenn Sie Tomcat für die Verwendung von Java Database Connectivity (JDBC) oder der Java-Persistenz-API (JPA) konfigurieren möchten, passen Sie zuerst die Umgebungsvariable `CATALINA_OPTS` an, die von Tomcat beim Start eingelesen wird. Diese Werte können mithilfe einer App-Einstellung im [App Service-Maven-Plug-In](https://github.com/Microsoft/azure-maven-plugins/blob/develop/azure-webapp-maven-plugin/README.md) festgelegt werden:

```xml
<appSettings>
    <property>
        <name>CATALINA_OPTS</name>
        <value>"$CATALINA_OPTS -Ddbuser=${DBUSER} -Ddbpassword=${DBPASSWORD} -DconnURL=${CONNURL}"</value>
    </property>
</appSettings>
```

Alternativ können Sie die Umgebungsvariablen im Azure-Portal auf der Seite **Konfiguration** > **Anwendungseinstellungen** festlegen.

Legen Sie als Nächstes fest, ob die Datenquelle nur für eine einzelne Anwendung oder für alle im Tomcat-Servlet ausgeführten Anwendungen verfügbar sein soll.

#### <a name="application-level-data-sources"></a>Datenquellen auf Anwendungsebene

1. Erstellen Sie eine *context.xml*-Datei im Verzeichnis *META-INF/* Ihres Projekts. Erstellen Sie das Verzeichnis *META-INF/* , falls es noch nicht vorhanden ist.

2. Fügen Sie in *context.xml* ein Element vom Typ `Context` hinzu, um die Datenquelle mit einer JNDI-Adresse zu verknüpfen. Ersetzen Sie den Platzhalter `driverClassName` durch den Klassennamen Ihres Treibers aus der obigen Tabelle.

    ```xml
    <Context>
        <Resource
            name="jdbc/dbconnection"
            type="javax.sql.DataSource"
            url="${dbuser}"
            driverClassName="<insert your driver class name>"
            username="${dbpassword}"
            password="${connURL}"
        />
    </Context>
    ```

3. Aktualisieren Sie die Datei *web.xml* Ihrer Anwendung, sodass die Datenquelle in Ihrer Anwendung verwendet wird.

    ```xml
    <resource-env-ref>
        <resource-env-ref-name>jdbc/dbconnection</resource-env-ref-name>
        <resource-env-ref-type>javax.sql.DataSource</resource-env-ref-type>
    </resource-env-ref>
    ```

#### <a name="shared-server-level-resources"></a>Gemeinsam verwendete Ressourcen auf Serverebene

Wenn Sie eine gemeinsam genutzte Datenquelle auf Serverebene hinzufügen, müssen Sie die Datei „server.xml“ von Tomcat bearbeiten. Laden Sie zunächst ein [Startskript](faq-app-service-linux.md#built-in-images) hoch, und legen Sie unter **Konfiguration** > **Startbefehl** den Pfad des Skripts fest. Das Startskript kann per [FTP](deploy-ftp.md) hochgeladen werden.

Ihr Startskript führt eine [XSL-Transformation](https://www.w3schools.com/xml/xsl_intro.asp) für die Datei „server.xml“ durch und gibt die resultierende XML-Datei unter `/usr/local/tomcat/conf/server.xml` aus. Das Startskript muss „libxslt“ per APK installieren. Die XSL-Datei und das Startskript können per FTP hochgeladen werden. Im Anschluss finden Sie ein Beispiel für ein Startskript:

```sh
# Install libxslt. Also copy the transform file to /home/tomcat/conf/
apk add --update libxslt

# Usage: xsltproc --output output.xml style.xsl input.xml
xsltproc --output /home/tomcat/conf/server.xml /home/tomcat/conf/transform.xsl /usr/local/tomcat/conf/server.xml
```

Im Anschluss folgt ein Beispiel für eine XSL-Datei. Die XSL-Beispieldatei fügt der Datei „server.xml“ von Tomcat einen neuen Connectorknoten hinzu.

```xml
<xsl:stylesheet version="1.0" xmlns:xsl="http://www.w3.org/1999/XSL/Transform">
  <xsl:output method="xml" indent="yes"/>

  <xsl:template match="@* | node()" name="Copy">
    <xsl:copy>
      <xsl:apply-templates select="@* | node()"/>
    </xsl:copy>
  </xsl:template>

  <xsl:template match="@* | node()" mode="insertConnector">
    <xsl:call-template name="Copy" />
  </xsl:template>

  <xsl:template match="comment()[not(../Connector[@scheme = 'https']) and
                                 contains(., '&lt;Connector') and
                                 (contains(., 'scheme=&quot;https&quot;') or
                                  contains(., &quot;scheme='https'&quot;))]">
    <xsl:value-of select="." disable-output-escaping="yes" />
  </xsl:template>

  <xsl:template match="Service[not(Connector[@scheme = 'https'] or
                                   comment()[contains(., '&lt;Connector') and
                                             (contains(., 'scheme=&quot;https&quot;') or
                                              contains(., &quot;scheme='https'&quot;))]
                                  )]
                      ">
    <xsl:copy>
      <xsl:apply-templates select="@* | node()" mode="insertConnector" />
    </xsl:copy>
  </xsl:template>

  <!-- Add the new connector after the last existing Connnector if there is one -->
  <xsl:template match="Connector[last()]" mode="insertConnector">
    <xsl:call-template name="Copy" />

    <xsl:call-template name="AddConnector" />
  </xsl:template>

  <!-- ... or before the first Engine if there is no existing Connector -->
  <xsl:template match="Engine[1][not(preceding-sibling::Connector)]"
                mode="insertConnector">
    <xsl:call-template name="AddConnector" />

    <xsl:call-template name="Copy" />
  </xsl:template>

  <xsl:template name="AddConnector">
    <!-- Add new line -->
    <xsl:text>&#xa;</xsl:text>
    <!-- This is the new connector -->
    <Connector port="8443" protocol="HTTP/1.1" SSLEnabled="true" 
               maxThreads="150" scheme="https" secure="true" 
               keystroreFile="${{user.home}}/.keystore" keystorePass="changeit"
               clientAuth="false" sslProtocol="TLS" />
  </xsl:template>
  
</xsl:stylesheet>
```

#### <a name="finalize-configuration"></a>Abschließen der Konfiguration

Platzieren Sie abschließend die JAR-Dateien des Treibers im Tomcat-Klassenpfad, und starten Sie Ihre App Service-Instanz neu.

1. Stellen Sie sicher, dass die JDBC-Treiberdateien für den Tomcat-Klassenladeprogramm verfügbar sind, indem Sie sie im Verzeichnis */home/tomcat/lib* ablegen. (Erstellen Sie dieses Verzeichnis, falls es noch nicht vorhanden ist.) Um diese Dateien in Ihre App Service-Instanz hochzuladen, gehen Sie folgendermaßen vor:

    1. Installieren Sie in der [Cloud Shell](https://shell.azure.com) die Web-App-Erweiterung:

      ```azurecli-interactive
      az extension add -–name webapp
      ```

    2. Führen Sie den folgenden CLI-Befehl aus, um einen SSH-Tunnel von Ihrem lokalen System zu App Service zu erstellen:

      ```azurecli-interactive
      az webapp remote-connection create --resource-group <resource-group-name> --name <app-name> --port <port-on-local-machine>
      ```

    3. Stellen Sie mit Ihrem SFTP-Client eine Verbindung mit dem lokalen Tunnelport her, und laden Sie die Dateien in den Ordner */home/tomcat/lib* hoch.

    Alternativ können Sie zum Hochladen des JDBC-Treibers auch einen FTP-Client verwenden. Führen Sie dazu die [Schritte zum Abrufen Ihrer FTP-Anmeldeinformationen](deploy-configure-credentials.md) aus.

2. Wenn Sie eine Datenquelle auf Serverebene erstellt haben, starten Sie die App Service-Linux-Anwendung neu. Tomcat setzt `CATALINA_BASE` auf `/home/tomcat` zurück und verwendet die aktualisierte Konfiguration.

### <a name="spring-boot"></a>Spring Boot

Für das Verbinden mit Datenquellen in Spring Boot-Anwendungen wird empfohlen, Verbindungszeichenfolgen zu erstellen und in Ihre Datei *application.properties* einzufügen.

1. Legen Sie auf der App Service-Seite im Abschnitt „Konfiguration“ einen Namen für die Zeichenfolge fest, fügen Sie Ihre JDBC-Verbindungszeichenfolge in das Feld für den Wert ein, und legen Sie den Typ auf „Benutzerdefiniert“ fest. Sie können diese Verbindungszeichenfolge optional als Sloteinstellung festlegen.

    Diese Verbindungszeichenfolge ist für die Anwendung als die Umgebungsvariable `CUSTOMCONNSTR_<your-string-name>` verfügbar. Die Verbindungszeichenfolge, die Sie oben erstellt haben, erhält z. B. den Namen `CUSTOMCONNSTR_exampledb`.

2. Verweisen Sie in Ihrer Datei *application.properties* über den Namen der Umgebungsvariable auf diese Verbindungszeichenfolge. In unserem Beispiel würden wir Folgendes verwenden.

    ```yml
    app.datasource.url=${CUSTOMCONNSTR_exampledb}
    ```

Weitere Informationen zu diesem Thema finden Sie in der Spring Boot-Dokumentation unter [Data Access](https://docs.spring.io/spring-boot/docs/current/reference/html/howto-data-access.html) (Datenzugriff) und [Externalized Configuration](https://docs.spring.io/spring-boot/docs/current/reference/html/boot-features-external-config.html) (Externalisierte Konfiguration).

## <a name="use-redis-as-a-session-cache-with-tomcat"></a>Verwenden von Redis als Sitzungscache mit Tomcat

Sie können Tomcat zur Verwendung eines externen Sitzungsspeichers konfigurieren, z. B. [Azure Cache for Redis](../azure-cache-for-redis/index.yml). Dies ermöglicht es Ihnen, den Benutzersitzungszustand (z. B. Warenkorbdaten) beizubehalten, wenn ein Benutzer zu einer anderen Instanz Ihrer App übertragen wird, z. B. wenn es zur automatischen Skalierung, einem Neustart oder einem Failover kommt.

Um Tomcat mit Redis zu verwenden, müssen Sie Ihre App für die Verwendung einer [PersistentManager](https://tomcat.apache.org/tomcat-8.5-doc/config/manager.html)-Implementierung konfigurieren. In den folgenden Schritten wird dieser Prozess erläutert, wobei [Pivotal-Sitzungs-Manager:redis-store](https://github.com/pivotalsoftware/session-managers/tree/master/redis-store) als Beispiel verwendet wird.

1. Öffnen Sie in Bash-Terminal, und verwenden Sie `<variable>=<value>`, um jede der folgenden Umgebungsvariablen festzulegen.

    | Variable                 | Wert                                                                      |
    |--------------------------|----------------------------------------------------------------------------|
    | RESOURCEGROUP_NAME       | Der Name der Ressourcengruppe, die Ihre App Service-Instanz enthält.       |
    | WEBAPP_NAME              | Der Name Ihrer App Service-Instanz.                                     |
    | WEBAPP_PLAN_NAME         | Der Name Ihres App Service-Plans.                                         |
    | REGION                   | Der Name der Region, in der Ihre App gehostet wird.                           |
    | REDIS_CACHE_NAME         | Der Name Ihrer Azure Cache for Redis-Instanz.                           |
    | REDIS_PORT               | Der SSL-Port, an dem Ihre Redis-Cache lauscht.                             |
    | REDIS_PASSWORD           | Der primäre Zugriffsschlüssel für Ihre Instanz.                                  |
    | REDIS_SESSION_KEY_PREFIX | Ein Wert, den Sie angeben, um Sitzungsschlüssel zu identifizieren, die von Ihrer App stammen. |

    ```bash
    RESOURCEGROUP_NAME=<resource group>
    WEBAPP_NAME=<web app>
    WEBAPP_PLAN_NAME=<App Service plan>
    REGION=<region>
    REDIS_CACHE_NAME=<cache>
    REDIS_PORT=<port>
    REDIS_PASSWORD=<access key>
    REDIS_SESSION_KEY_PREFIX=<prefix>
    ```

    Sie finden den Namen, den Port und die Zugriffsschlüsselinformationen im Azure-Portal, indem Sie in den Abschnitten **Eigenschaften** oder **Zugriffsschlüssel** Ihrer Dienstinstanz nachsehen.

2. Erstellen oder aktualisieren Sie die Datei *src/main/webapp/META-INF/context.xml* Ihrer App mit folgendem Inhalt:

    ```xml
    <?xml version="1.0" encoding="UTF-8"?>
    <Context path="">
        <!-- Specify Redis Store -->
        <Valve className="com.gopivotal.manager.SessionFlushValve" />
        <Manager className="org.apache.catalina.session.PersistentManager">
            <Store className="com.gopivotal.manager.redis.RedisStore"
                   connectionPoolSize="20"
                   host="${REDIS_CACHE_NAME}.redis.cache.windows.net"
                   port="${REDIS_PORT}"
                   password="${REDIS_PASSWORD}"
                   sessionKeyPrefix="${REDIS_SESSION_KEY_PREFIX}"
                   timeout="2000"
            />
        </Manager>
    </Context>
    ```

    Diese Datei gibt die Sitzungs-Manager-Implementierung für Ihre App an und konfiguriert diese. Sie verwendet die Umgebungsvariablen, die Sie im vorherigen Schritt festgelegt haben, um Ihre Kontoinformationen aus ihren Quelldateien herauszuhalten.

3. Verwenden Sie FTP, um die JAR-Datei des Sitzungs-Managers in Ihre App Service-Instanz hochzuladen, indem Sie sie im Verzeichnis */home/tomcat/lib* ablegen. Weitere Informationen finden Sie unter [Bereitstellen der App in Azure App Service mit FTP/S](./deploy-ftp.md).

4. Deaktivieren Sie das [Sitzungsaffinitätscookie](https://azure.microsoft.com/blog/disabling-arrs-instance-affinity-in-windows-azure-web-sites/) für Ihre App Service-Instanz. Sie können dies über das Azure-Portal tun, indem Sie zu Ihrer App navigieren und dann **Konfiguration > Allgemeine Einstellungen > ARR-Affinität** auf **Aus** festlegen. Alternativ können Sie den folgenden Befehl verwenden:

    ```azurecli
    az webapp update -g <resource group> -n <webapp name> --client-affinity-enabled false
    ```

    Standardmäßig werden in App Service Sitzungsaffinitätscookies verwendet, um sicherzustellen, dass Clientanforderungen mit vorhandenen Sitzungen an die gleiche Instanz der Anwendung weitergeleitet werden. Dieses Standardverhalten erfordert keine Konfiguration, kann aber den Benutzersitzungszustand nicht beibehalten, wenn Ihre App-Instanz neu gestartet oder Datenverkehr zu einer anderen Instanz umgeleitet wird. Wenn Sie [die vorhandene ARR-Instanzaffinitäts](https://azure.microsoft.com/blog/disabling-arrs-instance-affinity-in-windows-azure-web-sites/)konfiguration deaktivieren, um die cookiebasierte Weiterleitung der Sitzung zu deaktivieren, ermöglichen Sie, dass der konfigurierte Sitzungsspeicher ohne Beeinträchtigung ausgeführt wird.

5. Navigieren Sie zum Abschnitt **Eigenschaften** Ihrer App Service-Instanz, und suchen Sie **Zusätzliche ausgehende IP-Adressen**. Diese repräsentieren alle möglichen ausgehenden IP-Adressen für Ihre App. Kopieren Sie sie, um sie im nächsten Schritt zu verwenden.

6. Erstellen Sie für jede IP-Adresse in ihrer Azure Cache for Redis-Instanz eine Firewallregel. Dies können Sie im Azure-Portal im Abschnitt **Firewall** Ihrer Redis-Instanz durchführen. Geben Sie einen eindeutigen Namen für jede Regel an, und legen Sie die Werte für **Start-IP-Adresse** und **End-IP-Adresse** auf dieselbe IP-Adresse fest.

7. Navigieren Sie zum Abschnitt **Erweiterte Einstellungen** Ihrer Redis-Instanz, und legen Sie **Nur Zugriff über SSL zulassen** auf **Nein** fest. Dadurch kann Ihre App Service Instanz über die Azure-Infrastruktur mit Ihrem Redis Cache kommunizieren.

8. Aktualisieren Sie die `azure-webapp-maven-plugin`-Konfiguration in der Datei *pom.xml* Ihrer App so, dass sie auf Ihre Redis-Kontoinformationen verweist. Diese Datei verwendet die Umgebungsvariablen, die Sie zuvor festgelegt haben, um Ihre Kontoinformationen aus ihren Quelldateien herauszuhalten.

    Ändern Sie, falls notwendig, `1.9.1` in die aktuelle Version des [Maven-Plug-Ins für Azure App Service](/java/api/overview/azure/maven/azure-webapp-maven-plugin/readme).

    ```xml
    <plugin>
        <groupId>com.microsoft.azure</groupId>
        <artifactId>azure-webapp-maven-plugin</artifactId>
        <version>1.9.1</version>
        <configuration>            
            <!-- Web App information -->
            <schemaVersion>v2</schemaVersion>
            <resourceGroup>${RESOURCEGROUP_NAME}</resourceGroup>
            <appServicePlanName>${WEBAPP_PLAN_NAME}-${REGION}</appServicePlanName>
            <appName>${WEBAPP_NAME}-${REGION}</appName>
            <region>${REGION}</region>            
            <runtime>
                <os>linux</os>
                <javaVersion>jre8</javaVersion>
                <webContainer>tomcat 9.0</webContainer>
            </runtime>

            <appSettings>
                <property>
                    <name>REDIS_CACHE_NAME</name>
                    <value>${REDIS_CACHE_NAME}</value>
                </property>
                <property>
                    <name>REDIS_PORT</name>
                    <value>${REDIS_PORT}</value>
                </property>
                <property>
                    <name>REDIS_PASSWORD</name>
                    <value>${REDIS_PASSWORD}</value>
                </property>
                <property>
                    <name>REDIS_SESSION_KEY_PREFIX</name>
                    <value>${REDIS_SESSION_KEY_PREFIX}</value>
                </property>
                <property>
                    <name>JAVA_OPTS</name>
                    <value>-Xms2048m -Xmx2048m -DREDIS_CACHE_NAME=${REDIS_CACHE_NAME} -DREDIS_PORT=${REDIS_PORT} -DREDIS_PASSWORD=${REDIS_PASSWORD} IS_SESSION_KEY_PREFIX=${REDIS_SESSION_KEY_PREFIX}</value>
                </property>

            </appSettings>

        </configuration>
    </plugin>
    ```

9. Erstellen Sie Ihre App neu, und stellen Sie sie erneut bereit.

    ```bash
    mvn package -DskipTests azure-webapp:deploy
    ```

Ihre App verwendet nun Ihren Redis Cache für die Sitzungsverwaltung.

Ein Beispiel, das Sie verwenden können, um diese Anweisungen zu testen, finden Sie im Repository [scaling-stateful-java-web-app-on-azure](https://github.com/Azure-Samples/scaling-stateful-java-web-app-on-azure) auf GitHub.

## <a name="docker-containers"></a>Docker-Container

Wenn Sie das von Azure unterstützte Zulu JDK in Ihren Containern verwenden möchten, rufen Sie die auf der [Downloadseite für Azul Zulu Enterprise for Azure](https://www.azul.com/downloads/azure-only/zulu/) aufgeführten vorgefertigten Images per Pull ab, oder nutzen Sie die `Dockerfile`-Beispiele aus dem [Microsoft Java-GitHub-Repository](https://github.com/Microsoft/java/tree/master/docker).

## <a name="statement-of-support"></a>Unterstützungserklärung

### <a name="runtime-availability"></a>Runtimeverfügbarkeit

App Service für Linux unterstützt zwei Runtimes zum verwalteten Hosten von Java-Webanwendungen:

- [Tomcat-Servlet-Container](https://tomcat.apache.org/) zum Ausführen von Anwendungen, die als Webarchivdateien (WAR) paketiert wurden. Unterstützt werden die Versionen 8.5 und 9.0.
- Java SE Runtime Environment zum Ausführen von Anwendungen, die als Java-Archivdateien (JAR) paketiert wurden. Unterstützt werden die Versionen Java 8 und 11.

### <a name="jdk-versions-and-maintenance"></a>JDK-Versionen und -Wartung

Bei Azul Zulu Enterprise-Builds von OpenJDK handelt es sich um eine kostenlose, plattformübergreifende und produktionsbereite Distribution von OpenJDK für Azure und Azure Stack, die von Microsoft und Azul Systems unterstützt wird. Sie enthält alle Komponenten, die zum Erstellen und Ausführen von Java SE-Anwendungen benötigt werden. Sie können die JDK aus der [Java JDK-Installation](https://aka.ms/azure-jdks) heraus installieren.

Unterstützte JDKs werden jedes Vierteljahr im Januar, April, Juli und Oktober automatisch gepatcht.

### <a name="security-updates"></a>Sicherheitsupdates

Patches und Fixes für größere Sicherheitsrisiken werden veröffentlicht, sobald sie von Azul Systems zur Verfügung gestellt werden. Ein „größeres“ Sicherheitsrisiko wird durch eine Gesamtbewertung von 9.0 oder höher im [NIST Common Vulnerability Scoring System, Version 2](https://nvd.nist.gov/cvss.cfm), definiert.

### <a name="deprecation-and-retirement"></a>Einstellung und Außerbetriebnahme

Wenn eine unterstützte Java-Runtime eingestellt wird, erhalten Azure-Entwickler, die die betreffende Runtime verwenden, mindestens sechs Monate vor Außerbetriebnahme der Runtime eine entsprechende Benachrichtigung.

[!INCLUDE [robots933456](../../includes/app-service-web-configure-robots933456.md)]

## <a name="next-steps"></a>Nächste Schritte

Besuchen Sie das Center [Azure für Java-Entwickler](/java/azure/), um Azure-Schnellstarts, Tutorials und Java-Referenzdokumentation zu finden.

Allgemeine Fragen zur Verwendung von App Service für Linux, die sich nicht speziell auf die Java-Entwicklung beziehen, werden unter [Häufig gestellte Fragen (FAQ) zu Azure App Service unter Linux](faq-app-service-linux.md) beantwortet.

::: zone-end