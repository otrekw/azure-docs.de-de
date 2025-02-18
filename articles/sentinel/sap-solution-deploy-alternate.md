---
title: Lokales Bereitstellen des Azure Sentinel-Datenconnectors für SAP | Microsoft-Dokumentation
description: Erfahren Sie, wie Sie den Azure Sentinel-Datenconnector für SAP-Umgebungen mithilfe eines lokalen Computers bereitstellen.
author: batamig
ms.author: bagol
ms.service: azure-sentinel
ms.topic: how-to
ms.custom: mvc
ms.date: 05/19/2021
ms.subservice: azure-sentinel
ms.openlocfilehash: fc045d4b6c185b9e27573a1dd97c1194239f7463
ms.sourcegitcommit: 80d311abffb2d9a457333bcca898dfae830ea1b4
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/26/2021
ms.locfileid: "110466457"
---
# <a name="deploy-the-azure-sentinel-sap-data-connector-on-premises"></a>Lokales Bereitstellen des Azure Sentinel-Datenconnectors für SAP

In diesem Artikel wird beschrieben, wie Sie den Azure Sentinel-Datenconnector für SAP in einem Experten- oder benutzerdefinierten Prozess bereitstellen, z. B. mithilfe eines lokalen Computers und einer Azure Key Vault-Instanz zum Speichern Ihrer Anmeldeinformationen.

> [!NOTE]
> Der standardmäßige und am meisten empfohlene Bereitstellungsprozess für den Azure Sentinel-Datenconnector für SAP ist [die Verwendung einer Azure-VM](sap-deploy-solution.md). Dieser Artikel richtet sich an fortgeschrittene Benutzer.

> [!IMPORTANT]
> Die Azure Sentinel-Lösung für SAP befindet sich derzeit in der PREVIEW-Phase. In den [zusätzlichen Nutzungsbestimmungen für Microsoft Azure-Vorschauen](https://azure.microsoft.com/support/legal/preview-supplemental-terms/) finden Sie weitere rechtliche Bedingungen, die für Azure-Features gelten, die sich in der Beta- oder Vorschauversion befinden oder anderweitig noch nicht zur allgemeinen Verfügbarkeit freigegeben sind.
>

## <a name="prerequisites"></a>Voraussetzungen

Die grundlegenden Voraussetzungen für die Bereitstellung Ihres Azure Sentinel-Datenconnectors für SAP sind unabhängig von Ihrer Bereitstellungsmethode identisch.

Stellen Sie sicher, dass Ihr System die Voraussetzungen erfüllt, die im [Haupttutorial zur Bereitstellung des SAP-Datenconnectors](sap-deploy-solution.md#prerequisites) dokumentiert sind, bevor Sie beginnen.

Weitere Informationen finden Sie unter [Azure Sentinel-Lösung für SAP: detaillierte SAP-Anforderungen (Public Preview)](sap-solution-detailed-requirements.md).

## <a name="create-your-azure-key-vault"></a>Erstellen Ihres Azure-Schlüsseltresors

Erstellen Sie einen Azure-Schlüsseltresor, den Sie für Ihren Azure Sentinel-Datenconnector für SAP reservieren können.

Führen Sie den folgenden Befehl aus, um einen Azure-Schlüsseltresor zu erstellen:

``` azurecli
kvgp=<KVResourceGroup>

kvname=<keyvaultname>

#Create key vault
az keyvault create \
  --name $kvname \
  --resource-group $kvgp
```

Weitere Informationen finden Sie unter [Schnellstart: Erstellen eines Schlüsseltresors mithilfe der Azure CLI](/azure/key-vault/general/quick-create-cli).

## <a name="add-azure-key-vault-secrets"></a>Hinzufügen von Azure Key Vault-Geheimnissen

Um Azure Key Vault-Geheimnisse hinzuzufügen, führen Sie das folgende Skript mit Ihrer eigenen System-ID und den Anmeldeinformationen aus, die Sie hinzufügen möchten:

```azurecli
#Add Abap username
az keyvault secret set \
  --name <SID>-ABAPUSER \
  --value "<abapuser>" \
  --description SECRET_ABAP_USER --vault-name $kvname

#Add Abap Username password
az keyvault secret set \
  --name <SID>-ABAPPASS \
  --value "<abapuserpass>" \
  --description SECRET_ABAP_PASSWORD --vault-name $kvname

#Add java Username
az keyvault secret set \
  --name <SID>-JAVAOSUSER \
  --value "<javauser>" \
  --description SECRET_JAVAOS_USER --vault-name $kvname

#Add java Username password
az keyvault secret set \
  --name <SID>-JAVAOSPASS \
  --value "<javauserpass>" \
  --description SECRET_JAVAOS_PASSWORD --vault-name $kvname

#Add abapos username
az keyvault secret set \
  --name <SID>-ABAPOSUSER \
  --value "<abaposuser>" \
  --description SECRET_ABAPOS_USER --vault-name $kvname

#Add abapos username password
az keyvault secret set \
  --name <SID>-ABAPOSPASS \
  --value "<abaposuserpass>" \
  --description SECRET_ABAPOS_PASSWORD --vault-name $kvname

#Add Azure Log ws ID
az keyvault secret set \
  --name <SID>-LOG_WS_ID \
  --value "<logwsod>" \
  --description SECRET_AZURE_LOG_WS_ID --vault-name $kvname

#Add Azure Log ws public key
az keyvault secret set \
  --name <SID>-LOG_WS_PUBLICKEY \
  --value "<loswspubkey>" \
  --description SECRET_AZURE_LOG_WS_PUBLIC_KEY --vault-name $kvname
```

Weitere Informationen finden Sie in der CLI-Dokumentation unter [az keyvault secret](/cli/azure/keyvault/secret).

## <a name="perform-an-expert--custom-installation"></a>Durchführen einer Experten-/benutzerdefinierten Installation

In diesem Verfahren wird beschrieben, wie Sie den SAP-Datenconnector mithilfe einer Experten- oder benutzerdefinierten Installation bereitstellen, z. B. bei der lokalen Installation. 

Wir empfehlen diesen Vorgang, nachdem Sie einen Schlüsseltresor mit Ihren SAP-Anmeldeinformationen eingerichtet haben.

**So stellen Sie den SAP-Datenconnector bereit**

1. Laden Sie auf Ihrem lokalen Computer das neueste SAP NW RFC SDK von der Website [SAP Launchpad](https://support.sap.com) > **SAP NW RFC SDK** > **SAP NW RFC SDK 7.50** > **nwrfc750X_X-xxxxxxx.zip** herunter.

    > [!NOTE]
    > Sie benötigen Ihre SAP-Benutzeranmeldeinformationen, um auf das SDK zugreifen zu können. Außerdem müssen Sie das SDK herunterladen, das Ihrem Betriebssystem entspricht.
    >
    > Wählen Sie die Option **LINUX ON X86_64** aus.

1. Erstellen Sie auf Ihrem lokalen Computer einen neuen Ordner mit einem aussagekräftigen Namen, und kopieren Sie die ZIP-Datei des SDK in Ihren neuen Ordner.

1. Klonen Sie das GitHub-Repository von Azure Sentinel auf Ihrem lokalen Computer, und kopieren Sie die Datei **systemconfig.ini**  der Azure Sentinel-Lösung für SAP in Ihren neuen Ordner.

    Beispiel:

    ```bash
    mkdir /home/$(pwd)/sapcon/<sap-sid>/
    Cd /home/$(pwd)/sapcon/<sap-sid>/
    Wget  https://raw.githubusercontent.com/Azure/Azure-Sentinel/master/Solutions/SAP/template/systemconfig.inicp <**nwrfc750X_X-xxxxxxx.zip**> /home/$(pwd)/sapcon/<sap-sid>/
    ```

1. Bearbeiten Sie nach Bedarf die Datei **systemconfig.ini**. Orientieren Sie sich dabei an den eingebetteten Kommentaren. Weitere Informationen finden Sie unter [Manuelles Konfigurieren des SAP-Datenconnectors](#manually-configure-the-sap-data-connector).

    Um Ihre Konfiguration zu testen, sollten Sie Benutzer und Kennwort direkt zur Konfigurationsdatei **systemconfig.ini** hinzufügen. Wir empfehlen zwar [Azure Key Vault](#add-azure-key-vault-secrets) zur Speicherung Ihrer Anmeldeinformationen, Sie können aber auch eine Datei des Typs **env.list** bzw. [Docker-Geheimnisse](#manually-configure-the-sap-data-connector) verwenden oder Ihre Anmeldeinformationen direkt in die Datei **systemconfig.ini** einfügen.

1. Legen Sie die Protokolle, die Sie in Azure Sentinel erfassen möchten, anhand der Anweisungen in der Datei **systemconfig.ini** fest. Weitere Informationen finden Sie beispielsweise unter [Bestimmen der an Azure Sentinel gesendeten SAP-Protokolle](#define-the-sap-logs-that-are-sent-to-azure-sentinel).

1. Legen Sie die folgenden Konfigurationen anhand der Anweisungen in der Datei **systemconfig.ini** fest:

    - Ob E-Mail-Adressen von Benutzern in Überwachungsprotokollen enthalten sein sollen
    - Ob fehlgeschlagene API-Aufrufe wiederholt werden sollen
    - Ob cexal-Überwachungsprotokolle enthalten sein sollen
    - Ob ein Zeitintervall zwischen Datenextraktionen gewartet werden soll, insbesondere bei großen Extraktionen

    Weitere Informationen finden Sie unter [Konfigurationen des SAL-Protokollconnectors](#sal-logs-connector-settings).

1. Speichern Sie die aktualisierte Datei **systemconfig.ini** im Verzeichnis **sapcon** auf Ihrem Computer.

1. Wenn Sie sich zum Angeben Ihrer Ihre Anmeldeinformationen in einer Datei des Typs **env.list** entschieden haben, erstellen Sie die temporäre Datei **env.list** mit den erforderlichen Anmeldeinformationen. Sobald Ihr Docker-Container ordnungsgemäß ausgeführt wird, löschen Sie unbedingt diese Datei.

    > [!NOTE]
    > Das folgende Skript enthält alle Docker-Container, die eine Verbindung mit einem bestimmten ABAP-System herstellen können. Ändern Sie bei Bedarf Ihr Skript für Ihre Umgebung.
    >

    Führen Sie Folgendes aus:

    ```bash
    ##############################################################
    ##############################################################
    # env.list template
    SAPADMUSER=<SET_SAPCONTROL_USER>
    SAPADMPASSWORD=<SET_SAPCONTROL_PASS>
    ABAPUSER=SET_ABAP_USER>
    ABAPPASS=<SET_ABAP_PASS>
    JAVAUSER=<SET_JAVA_OS_USER>
    JAVAPASS=<SET_JAVA_OS_USER>
    ##############################################################
    ```

1. Laden Sie das vordefinierte Docker-Image herunter, und führen Sie es bei installiertem SAP-Datenconnector aus.  Führen Sie Folgendes aus:

    ```bash
    docker pull mcr.microsoft.com/azure-sentinel/solution/sapcon:latest-preview
    docker run --env-file=<env.list_location> -d -v /home/$(pwd)/sapcon/<sap-sid>/:/sapcon-app/sapcon/config/system --name sapcon-<sid> sapcon
    rm -f <env.list_location>
    ```

1. Überprüfen Sie, ob der Docker-Container ordnungsgemäß ausgeführt wird. Führen Sie Folgendes aus:

    ```bash
    docker logs –f sapcon-[SID]
    ```

1. Fahren Sie mit der Bereitstellung der Lösung **Azure Sentinel – Kontinuierliche Bedrohungsüberwachung für SAP** fort.

    Durch die Bereitstellung der Lösung wird der SAP-Datenconnector in Azure Sentinel angezeigt, und die SAP-Arbeitsmappe und -Analyseregeln werden bereitgestellt. Wenn Sie fertig sind, fügen Sie Ihre SAP-Watchlists manuell hinzu, und passen Sie sie an.

    Weitere Informationen finden Sie unter [Bereitstellen von SAP-Sicherheitsinhalten](sap-deploy-solution.md#deploy-sap-security-content).

## <a name="manually-configure-the-sap-data-connector"></a>Manuelles Konfigurieren des SAP-Datenconnectors

Der SAP-Datenconnector für Azure Sentinel für wird in der Datei **systemconfig.ini** konfiguriert, die Sie im Rahmen des [Bereitstellungsvorgangs](#perform-an-expert--custom-installation) auf Ihrem Computer mit dem SAP-Datenconnector geklont haben.

Der folgende Code zeigt ein Beispiel der Datei **systemconfig.ini**:

```Python
[Secrets Source]
secrets = '<DOCKER_RUNTIME/AZURE_KEY_VAULT/DOCKER_SECRETS/DOCKER_FIXED>'
keyvault = '<SET_YOUR_AZURE_KEYVAULT>'
intprefix = '<SET_YOUR_PREFIX>'

[ABAP Central Instance]
##############################################################
# Define the following values according to your server configuration.
ashost = <SET_YOUR_APPLICATION_SERVER_HOST>
mshost = <SET_YOUR_MESSAGE_SERVER_HOST> - #In case different then App
##############################################################
group = <SET_YOUR_LOGON_GROUP>
msserv = <SET_YOUR_MS_SERVICE> - #Required only if the message server service is not defined as sapms<SYSID> in /etc/services
sysnr = <SET_YOUR_SYS_NUMBER>
user = <SET_YOUR_USER>
##############################################################
# Enter your password OR your X509 SNC parameters
passwd = <SET_YOUR_PASSWORD>
snc_partnername = <SET_YOUR_SNC_PARTNER_NAME>
snc_lib = <SET_YOUR_SNC_LIBRARY_PATH>
x509cert = <SET_YOUR_X509_CERTIFICATE>
##############################################################
sysid = <SET_YOUR_SYSTEM_ID>
client = <SET_YOUR_CLIENT>

[Azure Credentials]
loganalyticswsid = <SET_YOUR_LOG_ANALYTICS_WORKSPACE_ID>
publickey = <SET_YOUR_PUBLIC_KEY>

[File Extraction ABAP]
osuser = <SET_YOUR_SAPADM_LIKE_USER>
##############################################################
# Enter your password OR your X509 SNC parameters
ospasswd = <SET_YOUR_SAPADM_PASS>
x509pkicert = <SET_YOUR_X509_PKI_CERTIFICATE>
##############################################################
appserver = <SET_YOUR_SAPCTRL_SERVER>
instance = <SET_YOUR_SAP_INSTANCE>
abapseverity = <SET_ABAP_SEVERITY>
abaptz = <SET_ABAP_TZ>

[File Extraction JAVA]
javaosuser = <SET_YOUR_JAVAADM_LIKE_USER>
##############################################################
# Enter your password OR your X509 SNC parameters
javaospasswd = <SET_YOUR_JAVAADM_PASS>
javax509pkicert = <SET_YOUR_X509_PKI_CERTIFICATE>
##############################################################
javaappserver = <SET_YOUR_JAVA_SAPCTRL_SERVER>
javainstance = <SET_YOUR_JAVA_SAP_INSTANCE>
javaseverity = <SET_JAVA_SEVERITY>
javatz = <SET_JAVA_TZ>
```

### <a name="define-the-sap-logs-that-are-sent-to-azure-sentinel"></a>Bestimmen der an Azure Sentinel gesendeten SAP-Protokolle

Fügen Sie der Datei **systemconfig.ini** der Azure Sentinel-Lösung für SAP den folgenden Code hinzu, um die Protokolle zu bestimmen, die an Azure Sentinel gesendet werden.

Weitere Informationen finden Sie unter [Referenz zu Protokollen der Azure Sentinel-Lösung für SAP (öffentliche Vorschau)](sap-solution-log-reference.md).

```Python
##############################################################
# Enter True OR False for each log to send those logs to Azure Sentinel
[Logs Activation Status]
ABAPAuditLog = True
ABAPJobLog = True
ABAPSpoolLog = True
ABAPSpoolOutputLog = True
ABAPChangeDocsLog = True
ABAPAppLog = True
ABAPWorkflowLog = True
ABAPCRLog = True
ABAPTableDataLog = False
# ABAP SAP Control Logs - Retrieved by using SAP Conntrol interface and OS Login
ABAPFilesLogs = False
SysLog = False
ICM = False
WP = False
GW = False
# Java SAP Control Logs - Retrieved by using SAP Conntrol interface and OS Login
JAVAFilesLogs = False
##############################################################
```

### <a name="sal-logs-connector-settings"></a>Einstellungen des SAL-Protokollconnectors

Fügen Sie den folgenden Code der Datei **systemconfig.ini** des Azure Sentinel-Datenconnectors für SAP hinzu, um weitere Einstellungen für in Azure Sentinel erfasste SAP-Protokolle festzulegen.

Weitere Informationen finden Sie unter [Ausführen einer Experten-/benutzerdefinierten Installation des SAP-Datenconnectors](#perform-an-expert--custom-installation).


```Python
##############################################################
[Connector Configuration]
extractuseremail = True
apiretry = True
auditlogforcexal = False
auditlogforcelegacyfiles = False
timechunk = 60
##############################################################
```

In diesem Abschnitt können Sie die folgenden Parameter konfigurieren:

|Parametername  |BESCHREIBUNG  |
|---------|---------|
|**extractuseremail**     |  Bestimmt, ob E-Mail-Adressen von Benutzern in Überwachungsprotokollen enthalten sind.       |
|**apiretry**     |   Bestimmt, ob API-Aufrufe als Failovermechanismus wiederholt werden.      |
|**auditlogforcexal**     |  Bestimmt, ob das System Überwachungsprotokolle für Nicht-SAL-Systeme erzwingt, z. B. für SAP BASIS-Version 7.4.       |
|**auditlogforcelegacyfiles**     |  Bestimmt, ob das System Überwachungsprotokolle mit Legacysystemfunktionen erzwingt, z. B. ab SAP BASIS-Version 7.4 mit niedrigeren Patchebenen.|
|**timechunk**     |   Bestimmt, dass das System eine bestimmte Anzahl von Minuten als Intervall zwischen Datenextraktionen wartet. Verwenden Sie diesen Parameter, wenn eine große Datenmenge erwartet wird. <br><br>Während des ersten Datenladevorgangs in den ersten 24 Stunden sollten Sie z. B. die Datenextraktion nur alle 30 Minuten ausführen lassen, um jeder Datenextraktion genügend Zeit zu geben. Legen Sie in solchen Fällen diesen Wert auf **30** fest.  |
|     |         |


## <a name="next-steps"></a>Nächste Schritte

Nachdem Sie den SAP-Datenconnector installiert haben, können Sie den SAP-bezogenen Sicherheitsinhalt hinzufügen.

Weitere Informationen finden Sie unter [Bereitstellen der SAP-Lösung](sap-deploy-solution.md#deploy-sap-security-content).

Weitere Informationen finden Sie unter

- [Detaillierte SAP-Anforderungen für die Azure Sentinel-Lösung für SAP](sap-solution-detailed-requirements.md)
- [Referenz zu Protokollen der Azure Sentinel-Lösung für SAP](sap-solution-log-reference.md)
- [Azure Sentinel-Lösung für SAP: Referenz zu sicherheitsbezogenen Inhalten](sap-solution-security-content.md)