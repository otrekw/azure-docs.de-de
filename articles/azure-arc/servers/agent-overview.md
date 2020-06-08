---
title: Übersicht über den Connected Machine-Agent für Windows
description: Dieser Artikel bietet eine ausführliche Übersicht über die verfügbaren Azure Arc für Server-Agents, die die Überwachung von VMs (virtuelle Computer) unterstützen, die in Hybridumgebungen gehostet werden.
services: azure-arc
ms.service: azure-arc
ms.subservice: azure-arc-servers
author: mgoedtel
ms.author: magoedte
ms.date: 05/18/2020
ms.topic: conceptual
ms.openlocfilehash: 4dbc559e62523a1ea17236a9e8c9666ef48bba33
ms.sourcegitcommit: fdec8e8bdbddcce5b7a0c4ffc6842154220c8b90
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/19/2020
ms.locfileid: "83662528"
---
# <a name="overview-of-azure-arc-for-servers-agent"></a>Übersicht über Azure Arc für Server-Agents

Mit dem Connected Machine-Agent von Azure Arc für Server können Sie Ihre Windows- und Linux-Computer verwalten, die außerhalb von Azure in Ihrem Unternehmensnetzwerk oder von einem anderen Cloudanbieter gehostet werden. Dieser Artikel enthält eine ausführliche Übersicht über den Agent sowie Informationen zu System- und Netzwerkanforderungen und zu den verschiedenen Bereitstellungsmethoden.

## <a name="download-agents"></a>Herunterladen von Agents

Das Paket für den Azure Connected Machine-Agent für Windows und Linux steht an folgenden Orten zum Download bereit:

- [Windows Installer-Paket für den Windows-Agent](https://aka.ms/AzureConnectedMachineAgent) im Microsoft Download Center
- Das Paket für den Linux-Agent wird über das [Paketrepository](https://packages.microsoft.com/) von Microsoft im bevorzugten Paketformat für die Distribution (RPM oder DEB) verteilt.

>[!NOTE]
>Im Rahmen dieser Vorschau wurde nur ein einzelnes, für Ubuntu 16.04 oder 18.04 geeignetes Paket veröffentlicht.

Der Azure Connected Machine-Agent für Windows und Linux kann abhängig von Ihren Anforderungen manuell oder automatisch auf das neueste Release aktualisiert werden. Weitere Informationen finden Sie [hier](manage-agent.md).

## <a name="windows-agent-installation-details"></a>Installationsdetails zu Windows-Agents

Der Connected Machine-Agent für Windows kann mit einer der folgenden drei Methoden installiert werden:

* Doppelklicken Sie auf die Datei `AzureConnectedMachineAgent.msi`.
* Führen Sie das Windows Installer-Paket `AzureConnectedMachineAgent.msi` manuell über die Befehlsshell aus.
* Verwenden Sie eine Skriptmethode in einer PowerShell-Sitzung.

Nach der Installation des Connected Machine-Agents für Windows werden die folgenden zusätzlichen systemweiten Konfigurationsänderungen angewendet.

* Die folgenden Installationsordner werden während des Setups erstellt.

    |Ordner |BESCHREIBUNG |
    |-------|------------|
    |C:\Programme\AzureConnectedMachineAgent |Dieser Ordner entspricht dem Standardinstallationspfad der Unterstützungsdateien für den Agent.|
    |%ProgramData%\AzureConnectedMachineAgent |Dieser Ordner enthält die Konfigurationsdateien des Agents.|
    |%ProgramData%\AzureConnectedMachineAgent\Tokens |Dieser Ordner enthält die abgerufenen Token.|
    |%ProgramData%\AzureConnectedMachineAgent\Config |Dieser Ordner enthält die Agent-Konfigurationsdatei `agentconfig.json`, die die Registrierungsinformationen mit dem Dienst aufzeichnet.|
    |%ProgramData%\GuestConfig |Dieser Ordner enthält die Dateien im Zusammenhang mit den (angewendeten) Azure-Richtlinien.|

* Die folgenden Windows-Dienste werden während der Installation des Agents auf dem Zielcomputer installiert.

    |Dienstname |`Display name` |Prozessname |BESCHREIBUNG |
    |-------------|-------------|-------------|------------|
    |himds |Azure Hybrid Instance Metadata Service |himds.exe |Dieser Dienst implementiert Azure Instance Metadata Service (IMDS) zum Nachverfolgen des Computers.|
    |DscService |Gastkonfigurationsdienst |dsc_service.exe |Hierbei handelt es sich um die DSC v2-Codebasis (Desired State Configuration), die in Azure zum Implementieren von Gastrichtlinien verwendet wird.|

* Die folgenden Umgebungsvariablen werden während der Installation des Agents erstellt.

    |Name |Standardwert |BESCHREIBUNG |
    |-----|--------------|------------|
    |IDENTITY_ENDPOINT |http://localhost:40342/metadata/identity/oauth2/token ||
    |IMDS_ENDPOINT |http://localhost:40342 ||
    
* Es stehen vier Protokolldateien für die Problembehandlung zur Verfügung. Diese werden in der folgenden Tabelle beschrieben.

    |Log |BESCHREIBUNG |
    |----|------------|
    |%ProgramData%\AzureConnectedMachineAgent\Log\himds.log |Dieses Protokoll erfasst Informationen zum Dienst des Agents (himds) und zur Interaktion mit Azure.|
    |%ProgramData%\AzureConnectedMachineAgent\Log\azcmagent.log |Dieses Protokoll enthält die Ausgabe der Befehle des azcmagent-Tools, wenn das Argument „verbose“ (-v) verwendet wird.|
    |%ProgramData%\GuestConfig\gc_agent_logs\gc_agent.log |Dieses Protokoll erfasst Informationen zur DSC-Dienstaktivität,<br> insbesondere die Konnektivität zwischen dem himds-Dienst und Azure Policy.|
    |%ProgramData%\GuestConfig\gc_agent_logs\gc_agent_telemetry.txt |Dieses Protokoll erfasst Informationen über die DSC-Diensttelemetrie und die ausführliche Protokollierung.|

* Die lokale Sicherheitsgruppe **Hybrid agent extension applications** wird erstellt. 

* Die folgenden Artefakte werden bei der Deinstallation des Agents nicht gelöscht.

    * C:\Programme\AzureConnectedMachineAgent\Logs
    * %ProgramData%\AzureConnectedMachineAgent und untergeordnete Verzeichnisse
    * %ProgramData%\GuestConfig

## <a name="linux-agent-installation-details"></a>Linux-Agent-Installationsdetails

Der Linux-Agent für verbundene Computer wird im bevorzugten Paketformat für die Distribution (RPM oder DEB) bereitgestellt, die im [Paketrepository](https://packages.microsoft.com/) von Microsoft gehostet wird. Der Agent wird installiert und mit dem Shellskriptpaket [Install_linux_azcmagent.sh](https://aka.ms/azcmagent) konfiguriert. 

Nach der Installation des Connected Machine-Agents für Linux werden die folgenden zusätzlichen systemweiten Konfigurationsänderungen angewendet.

* Die folgenden Installationsordner werden während des Setups erstellt.

    |Ordner |BESCHREIBUNG |
    |-------|------------|
    |/var/opt/azcmagent/ |Dieser Ordner entspricht dem Standardinstallationspfad der Unterstützungsdateien für den Agent.|
    |/opt/azcmagent/ |
    |/opt/DSC/ |
    |/var/opt/azcmagent/tokens |Dieser Ordner enthält die abgerufenen Token.|
    |/var/lib/GuestConfig |Dieser Ordner enthält die Dateien im Zusammenhang mit den (angewendeten) Azure-Richtlinien.|

* Die folgenden Daemons werden während der Installation des Agents auf dem Zielcomputer installiert.

    |Dienstname |`Display name` |Prozessname |BESCHREIBUNG |
    |-------------|-------------|-------------|------------|
    |himdsd.service |Azure Hybrid Instance Metadata Service |/opt/azcmagent/bin/himds |Dieser Dienst implementiert Azure Instance Metadata Service (IMDS) zum Nachverfolgen des Computers.|
    |dscd.service |Gastkonfigurationsdienst |/opt/DSC/dsc_linux_service |Hierbei handelt es sich um die DSC v2-Codebasis (Desired State Configuration), die in Azure zum Implementieren von Gastrichtlinien verwendet wird.|

* Es stehen vier Protokolldateien für die Problembehandlung zur Verfügung. Diese werden in der folgenden Tabelle beschrieben.

    |Log |BESCHREIBUNG |
    |----|------------|
    |/var/opt/azcmagent/log/himds.log |Dieses Protokoll erfasst Informationen zum Dienst des Agents (himds) und zur Interaktion mit Azure.|
    |/var/opt/azcmagent/log/azcmagent.log |Dieses Protokoll enthält die Ausgabe der Befehle des azcmagent-Tools, wenn das Argument „verbose“ (-v) verwendet wird.|
    |/opt/logs/dsc.log |Dieses Protokoll erfasst Informationen zur DSC-Dienstaktivität,<br> insbesondere die Konnektivität zwischen dem himds-Dienst und Azure Policy.|
    |/opt/logs/dsc.telemetry.txt |Dieses Protokoll erfasst Informationen über die DSC-Diensttelemetrie und die ausführliche Protokollierung.|

* Die folgenden Umgebungsvariablen werden während der Installation des Agents erstellt. Die folgenden Variablen werden in `/lib/systemd/system.conf.d/azcmagent.conf` festgelegt.

    |Name |Standardwert |BESCHREIBUNG |
    |-----|--------------|------------|
    |IDENTITY_ENDPOINT |http://localhost:40342/metadata/identity/oauth2/token ||
    |IMDS_ENDPOINT |http://localhost:40342 ||

* Die folgenden Artefakte werden bei der Deinstallation des Agents nicht gelöscht.

    * /var/opt/azcmagent
    * /opt/logs

## <a name="prerequisites"></a>Voraussetzungen

### <a name="supported-operating-systems"></a>Unterstützte Betriebssysteme

Für den Azure Connected Machine-Agent werden offiziell folgende Windows- und Linux-Versionen unterstützt: 

- Windows Server 2012 R2 und höher (einschließlich Windows Server Core)
- Ubuntu 16.04 und 18.04
- CentOS Linux 7
- SUSE Linux Enterprise Server (SLES) 15
- Red Hat Enterprise Linux 7 (RHEL)
- Amazon Linux 2

>[!NOTE]
>Von diesem Vorschaurelease des Connected Machine-Agents für Windows wird nur Windows Server in englischer Sprache unterstützt.
>

### <a name="required-permissions"></a>Erforderliche Berechtigungen

- Zum Durchführen des Onboardings von Computern müssen Sie der Rolle **Onboarding verbundener Azure-Computer** angehören.

- Zum Lesen, Ändern, erneuten Onboarding oder Löschen eines Computers müssen Sie der Rolle **Ressourcenadministrator für verbundene Azure-Computer** angehören. 

### <a name="azure-subscription-and-service-limits"></a>Einschränkungen von Azure-Abonnements und -Diensten

Bevor Sie Ihre Computer mit Azure Arc für Server (Vorschauversion) konfigurieren, sollten Sie sich mit den [Abonnementgrenzwerten](../../azure-resource-manager/management/azure-subscription-service-limits.md#subscription-limits) von Azure Resource Manager sowie mit den [Grenzwerten für Ressourcengruppen](../../azure-resource-manager/management/azure-subscription-service-limits.md#resource-group-limits) vertraut machen, um die Anzahl der zu verbindenden Computer zu planen.

## <a name="tls-12-protocol"></a>TLS 1.2-Protokoll

Um die Sicherheit von Daten bei der Übertragung an Azure zu gewährleisten, wird dringend empfohlen, den Computer so zu konfigurieren, dass er TLS 1.2 (Transport Layer Security) verwendet. Bei älteren Versionen von TLS/Secure Sockets Layer (SSL) wurde ein Sicherheitsrisiko festgestellt. Sie funktionieren aus Gründen der Abwärtskompatibilität zwar noch, werden jedoch **nicht empfohlen**. 

|Plattform/Sprache | Support | Weitere Informationen |
| --- | --- | --- |
|Linux | Linux-Distributionen greifen zur Unterstützung von TLS 1.2 tendenziell auf [OpenSSL](https://www.openssl.org) zurück. | Überprüfen Sie anhand des [OpenSSL-Änderungsprotokolls](https://www.openssl.org/news/changelog.html), ob Ihre Version von OpenSSL unterstützt wird.|
| Windows Server 2012 R2 und höhere Versionen | Wird unterstützt und ist standardmäßig aktiviert. | Zur Bestätigung, dass Sie weiterhin die [Standardeinstellungen](https://docs.microsoft.com/windows-server/security/tls/tls-registry-settings) verwenden.|

### <a name="networking-configuration"></a>Netzwerkkonfiguration

Der Connected Machine-Agent für Linux und Windows kommuniziert ausgehend auf sichere Weise über den TCP-Port 443 mit Azure Arc. Wenn der Computer für die Kommunikation über das Internet eine Firewall oder einen Proxyserver durchlaufen muss, sehen Sie sich die weiter unten angegebenen Anforderungen an, um sich mit der erforderlichen Netzwerkkonfiguration vertraut zu machen.

Sollte die ausgehende Konnektivität durch Ihre Firewall oder Ihren Proxyserver eingeschränkt sein, stellen Sie sicher, dass die unten aufgeführten URLs nicht blockiert werden. Wenn Sie nur die IP-Adressbereiche oder Domänennamen zulassen, die der Agent für die Kommunikation mit dem Dienst benötigt, müssen Sie auch den Zugriff auf die folgenden Diensttags und URLs zulassen.

Diensttags:

- AzureActiveDirectory
- AzureTrafficManager

URLs:

| Agent-Ressource | BESCHREIBUNG |
|---------|---------|
|management.azure.com|Azure Resource Manager|
|login.windows.net|Azure Active Directory|
|dc.services.visualstudio.com|Application Insights|
|agentserviceapi.azure-automation.net|Gastkonfiguration|
|*-agentservice-prod-1.azure-automation.net|Gastkonfiguration|
|*.his.arc.azure.com|Hybrididentitätsdienst|

Eine Liste der IP-Adressen für die einzelnen Diensttags/Regionen finden Sie in der JSON-Datei unter [Azure-IP-Bereiche und -Diensttags – öffentliche Cloud](https://www.microsoft.com/download/details.aspx?id=56519). Microsoft veröffentlicht wöchentliche Updates zu den einzelnen Azure-Diensten und den dafür genutzten IP-Adressbereichen. Weitere Informationen finden Sie unter [Diensttags](https://docs.microsoft.com/azure/virtual-network/security-overview#service-tags).

Die URLs in der obigen Tabelle werden zusätzlich zu den Informationen zum Diensttag-IP-Adressbereich benötigt, da die meisten Dienste derzeit nicht über eine Diensttagregistrierung verfügen. Die IP-Adressen können sich also ändern. Falls IP-Adressbereiche für Ihre Firewallkonfiguration erforderlich sind, sollte das Diensttag **AzureCloud** verwendet werden, um den Zugriff auf alle Azure-Dienste zuzulassen. Deaktivieren Sie weder die Sicherheitsüberwachung noch die Überprüfung dieser URLs. Lassen Sie sie wie anderen Internetdatenverkehr zu.

### <a name="register-azure-resource-providers"></a>Registrieren von Azure-Ressourcenanbietern

Azure Arc für Server (Vorschauversion) ist auf folgende Azure-Ressourcenanbieter in Ihrem Abonnement angewiesen, um diesen Dienst nutzen zu können:

- **Microsoft.HybridCompute**
- **Microsoft.GuestConfiguration**

Sollten sie nicht registriert sein, können Sie sie mithilfe der folgenden Befehle registrieren:

Azure PowerShell:

```azurepowershell-interactive
Login-AzAccount
Set-AzContext -SubscriptionId [subscription you want to onboard]
Register-AzResourceProvider -ProviderNamespace Microsoft.HybridCompute
Register-AzResourceProvider -ProviderNamespace Microsoft.GuestConfiguration
```

Azure-Befehlszeilenschnittstelle:

```azurecli-interactive
az account set --subscription "{Your Subscription Name}"
az provider register --namespace 'Microsoft.HybridCompute'
az provider register --namespace 'Microsoft.GuestConfiguration'
```

Sie können die Ressourcenanbieter auch über das Azure-Portal registrieren, indem Sie die Schritte unter [Azure-Portal](../../azure-resource-manager/management/resource-providers-and-types.md#azure-portal) ausführen.


## <a name="installation-and-configuration"></a>Installation und Konfiguration

Für Computer in Ihrer Hybridumgebung kann abhängig von Ihren Anforderungen auf verschiedene Weise eine Direktverbindung mit Azure hergestellt werden. Anhand der folgenden Tabelle können Sie ermitteln, welche der Methoden für Ihre Organisation am besten geeignet ist:

| Methode | BESCHREIBUNG |
|--------|-------------|
| Interaktiv | Installieren Sie den Agent manuell auf einem einzelnen Computer oder auf einigen wenigen Computern. Eine entsprechende Anleitung finden Sie unter [Verbinden von Hybridcomputern mit Azure über das Azure-Portal](onboard-portal.md).<br> Im Azure-Portal können Sie ein Skript generieren und dieses auf dem Computer ausführen, um die Installations- und Konfigurationsschritte des Agents zu automatisieren.|
| Skalierbar | Installieren und konfigurieren Sie den Agent für mehrere Computer. Eine entsprechende Anleitung finden Sie unter [Schnellstart: Verbinden von Computern mit Azure mithilfe von Azure Arc für Server – PowerShell](onboard-service-principal.md).<br> Mit dieser Methode wird ein Dienstprinzipal für die nicht interaktive Verbindungsherstellung mit Computern erstellt.|
| Skalierbar | Installieren und konfigurieren Sie den Agent für mehrere Computer. Eine entsprechende Anleitung finden Sie unter [Installieren des Connected Machine-Agent mithilfe von Windows PowerShell DSC](onboard-dsc.md).<br> Mit dieser Methode wird ein Dienstprinzipal für die nicht interaktive Verbindungsherstellung mit Computern mithilfe von PowerShell DSC erstellt. |

## <a name="next-steps"></a>Nächste Schritte

Im Artikel [Verbinden von Hybridcomputern mit Azure über das Azure-Portal](onboard-portal.md) erfahren Sie, wie Sie mit der Evaluierung von Azure Arc für Server (Vorschauversion) beginnen.
