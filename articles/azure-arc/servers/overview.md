---
title: Übersicht über Azure Arc für Server (Vorschauversion)
description: Hier erfahren Sie, wie Sie außerhalb von Azure gehostete Computer mithilfe von Azure Arc für Server wie eine Azure-Ressource verwalten.
services: azure-arc
ms.service: azure-arc
ms.subservice: azure-arc-servers
author: mgoedtel
ms.author: magoedte
keywords: Azure Automation, DSC, PowerShell, Desired State Configuration, Updateverwaltung, Änderungsnachverfolgung, Bestand, Runbooks, Python, grafisch, Hybrid
ms.date: 03/24/2020
ms.topic: overview
ms.openlocfilehash: 5fa39028f1041a063bab295adabf8145a8b46ae4
ms.sourcegitcommit: 7e04a51363de29322de08d2c5024d97506937a60
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/14/2020
ms.locfileid: "81308785"
---
# <a name="what-is-azure-arc-for-servers-preview"></a>Was ist Azure Arc für Server (Vorschauversion)?

Mit Azure Arc für Server (Vorschauversion) können Sie Windows- und Linux-Computer, die außerhalb von Azure in Ihrem Unternehmensnetzwerk oder von einem anderen Cloudanbieter gehostet werden, auf ähnliche Weise verwalten wie native virtuelle Azure-Computer. Wenn ein Hybridcomputer mit Azure verbunden wird, wird er zu einem verbundenen Computer und in Azure wie eine Ressource behandelt. Jeder verbundene Computer verfügt über eine Ressourcen-ID, wird als Teil einer Ressourcengruppe unter einem Abonnement verwaltet und profitiert von Azure-Standardkonstrukten wie Azure Policy und Tagging.

Um diese Möglichkeit für Ihre außerhalb von Azure gehosteten Hybridcomputer nutzen zu können, muss auf jedem Computer, den Sie mit Azure verbinden möchten, der Azure Connected Machine-Agent installiert werden. Dieser Agent bietet keine weiteren Funktionen und ist kein Ersatz für den [Log Analytics-Agent](../../azure-monitor/platform/log-analytics-agent.md) von Azure. Der Log Analytics-Agent für Windows und Linux ist erforderlich, wenn Sie das Betriebssystem und die Workloads auf dem Computer proaktiv überwachen, den Computer mithilfe von Automation-Runbooks oder Lösungen wie der Updateverwaltung verwalten oder andere Azure-Dienste wie [Azure Security Center](../../security-center/security-center-intro.md) verwenden möchten.

>[!NOTE]
>Dieses Vorschaurelease dient lediglich zur Evaluierung und sollte nicht für die Verwaltung wichtiger Produktionscomputer verwendet werden.
>

## <a name="supported-scenarios"></a>Unterstützte Szenarios

Von Azure Arc für Server (Vorschauversion) werden folgende Szenarien mit verbundenen Computern unterstützt:

- Zuweisen von [Azure Policy-Gastkonfigurationen](../../governance/policy/concepts/guest-configuration.md) mit der gleichen Vorgehensweise wie bei der Richtlinienzuweisung für virtuelle Azure-Computer
- Protokolldaten, die vom Log Analytics-Agent gesammelt und in dem Log Analytics-Arbeitsbereich gespeichert werden, bei dem der Computer registriert ist, enthalten jetzt spezifische Eigenschaften für den Computer (beispielsweise die Ressourcen-ID), die zur Unterstützung des Protokollzugriffs im [Ressourcenkontext](../../azure-monitor/platform/design-logs-deployment.md#access-mode) verwendet werden können.

## <a name="supported-regions"></a>Unterstützte Regionen

Azure Arc für Server (Vorschauversion) wird nur in bestimmten Regionen unterstützt:

- WestUS2
- Europa, Westen
- WestAsia

In den meisten Fällen sollte der Standort, den Sie beim Erstellen des Installationsskripts auswählen, die Azure-Region sein, die dem Standort Ihres Computers geografisch am nächsten ist. Ruhende Daten werden innerhalb der geografischen Azure-Region gespeichert, in der sich die von Ihnen angegebene Region befindet, was ggf. auch Auswirkungen auf die Wahl der Region hat, wenn Data Residency-Anforderungen erfüllt werden müssen. Wenn die Azure-Region, mit der Ihr Computer verbunden ist, von einem Ausfall betroffen ist, wird der verbundene Computer zwar nicht beeinträchtigt, es kann jedoch sein, dass Verwaltungsvorgänge mit Azure nicht abgeschlossen werden können. Wenn Sie über mehrere Standorte mit einem geografisch redundanten Dienst verfügen, empfiehlt es sich, die Computer an den einzelnen Standorten jeweils mit einer anderen Azure-Region zu verbinden, um im Falle eines regionalen Ausfalls von der Resilienz zu profitieren.

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
|*.his.hybridcompute.azure-automation.net|Hybrididentitätsdienst|

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

## <a name="connected-machine-agent"></a>Connected Machine-Agent

Das Paket für den Azure Connected Machine-Agent für Windows und Linux steht an folgenden Orten zum Download bereit:

- [Windows Installer-Paket für den Windows-Agent](https://aka.ms/AzureConnectedMachineAgent) im Microsoft Download Center
- Das Paket für den Linux-Agent wird über das [Paketrepository](https://packages.microsoft.com/) von Microsoft im bevorzugten Paketformat für die Distribution (RPM oder DEB) verteilt.

>[!NOTE]
>Im Rahmen dieser Vorschau wurde nur ein einzelnes, für Ubuntu 16.04 oder 18.04 geeignetes Paket veröffentlicht.

Der Azure Connected Machine-Agent für Windows und Linux kann abhängig von Ihren Anforderungen manuell oder automatisch auf das neueste Release aktualisiert werden. Weitere Informationen finden Sie [hier](manage-agent.md).

### <a name="agent-status"></a>Agent-Status

Der Connected Machine-Agent sendet alle fünf Minuten eine reguläre Heartbeatnachricht an den Dienst. Wenn der Dienst keine solchen Heartbeatnachrichten mehr von einem Computer empfängt, wird dieser Computer als offline betrachtet, und der Status wird im Portal innerhalb von 15 bis 30 Minuten automatisch in **Getrennt** geändert. Wenn wieder eine Heartbeatnachricht des Connected Machine-Agents empfangen wird, ändert sich der Status automatisch in **Verbunden**.

## <a name="install-and-configure-agent"></a>Installieren und Konfigurieren des Agents

Für Computer in Ihrer Hybridumgebung kann abhängig von Ihren Anforderungen auf verschiedene Weise eine Direktverbindung mit Azure hergestellt werden. Anhand der folgenden Tabelle können Sie ermitteln, welche der Methoden für Ihre Organisation am besten geeignet ist:

| Methode | BESCHREIBUNG |
|--------|-------------|
| Interaktiv | Installieren Sie den Agent manuell auf einem einzelnen Computer oder auf einigen wenigen Computern. Eine entsprechende Anleitung finden Sie unter [Verbinden von Hybridcomputern mit Azure über das Azure-Portal](onboard-portal.md).<br> Im Azure-Portal können Sie ein Skript generieren und dieses auf dem Computer ausführen, um die Installations- und Konfigurationsschritte des Agents zu automatisieren.|
| Skalierbar | Installieren und konfigurieren Sie den Agent für mehrere Computer. Eine entsprechende Anleitung finden Sie unter [Schnellstart: Verbinden von Computern mit Azure mithilfe von Azure Arc für Server – PowerShell](onboard-service-principal.md).<br> Mit dieser Methode wird ein Dienstprinzipal für die nicht interaktive Verbindungsherstellung mit Computern erstellt.|
| Skalierbar | Installieren und konfigurieren Sie den Agent für mehrere Computer. Eine entsprechende Anleitung finden Sie unter [Installieren des Connected Machine-Agent mithilfe von Windows PowerShell DSC](onboard-dsc.md).<br> Mit dieser Methode wird ein Dienstprinzipal für die nicht interaktive Verbindungsherstellung mit Computern mithilfe von PowerShell DSC erstellt. |

## <a name="next-steps"></a>Nächste Schritte

- Im Artikel [Verbinden von Hybridcomputern mit Azure über das Azure-Portal](onboard-portal.md) erfahren Sie, wie Sie mit der Evaluierung von Azure Arc für Server (Vorschauversion) beginnen. 
