---
title: Azure AD-Architektur für die lokale Anwendungsbereitstellung | Microsoft-Dokumentation
description: Die Dokumentation enthält eine Übersicht über die Architektur für die lokale Anwendungsbereitstellung.
services: active-directory
author: billmath
manager: mtillman
ms.service: active-directory
ms.workload: identity
ms.topic: overview
ms.date: 05/28/2021
ms.subservice: hybrid
ms.author: billmath
ms.collection: M365-identity-device-management
ms.openlocfilehash: d996ebf8641b4b2829ecbe7cb70ed50280f89391
ms.sourcegitcommit: 7d63ce88bfe8188b1ae70c3d006a29068d066287
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/22/2021
ms.locfileid: "114451568"
---
# <a name="azure-ad-on-premises-application-provisioning-architecture"></a>Azure AD-Architektur für die lokale Anwendungsbereitstellung

>[!IMPORTANT]
> Die Vorschauversion der lokalen Bereitstellung ist derzeit nur auf Einladung verfügbar. Um den Zugriff auf die Funktion anzufordern, verwenden Sie das [Zugriffsanforderungsformular](https://aka.ms/onpremprovisioningpublicpreviewaccess). Wir öffnen die Vorschauversion im Rahmen der Vorbereitung auf die allgemeine Verfügbarkeit (GA) in den nächsten Monaten für weitere Kunden und Connectors.

## <a name="overview"></a>Übersicht

Das folgende Diagramm enthält eine Übersicht über die Funktionsweise der lokalen Anwendungsbereitstellung.

![Ein Diagramm, das die Architektur für die lokale Anwendungsbereitstellung zeigt.](.\media\on-premises-application-provisioning-architecture\arch-3.png)

Es gibt drei Hauptkomponenten für die Bereitstellung von Benutzern in einer lokalen Anwendung:

- Der Bereitstellungs-Agent ermöglicht die Konnektivität zwischen Azure Active Directory (Azure AD) und Ihrer lokalen Umgebung.
- Der ECMA-Host konvertiert Bereitstellungsanforderungen von Azure AD in Anforderungen, die an Ihre Zielanwendung gesendet werden. Er dient als Gateway zwischen Azure AD und Ihrer Anwendung. Mit dem Host können Sie vorhandene ECMA2-Connectors importieren, die mit dem Microsoft Identity Manager verwendet werden. Der ECMA-Host ist nicht erforderlich, wenn Sie eine SCIM-Anwendung oder ein SCIM-Gateway erstellt haben.
- Der Azure AD-Bereitstellungsdienst dient als Synchronisierungs-Engine.

>[!NOTE]
> Eine Synchronisierung mit dem Microsoft Identity Manager ist nicht erforderlich. Sie können sie aber verwenden, um Ihren ECMA-Connector zu erstellen und zu testen, bevor Sie diesen auf den ECMA-Host importieren.


### <a name="firewall-requirements"></a>Firewallanforderungen

Sie müssen keine eingehenden Verbindungen mit dem Unternehmensnetzwerk öffnen. Für die Bereitstellungs-Agenten werden nur ausgehende Verbindungen mit dem Bereitstellungsdienst verwendet. Dies bedeutet, dass keine Firewallports für eingehende Verbindungen geöffnet werden müssen. Sie benötigen auch kein Umkreisnetzwerk (DMZ), da alle Verbindungen in ausgehender Richtung verlaufen und über einen sicheren Kanal erfolgen.

## <a name="agent-best-practices"></a>Bewährte Methoden für den Agent
- Vergewissern Sie sich, dass für den Azure AD Connect-Bereitstellungs-Agent der Dienst für automatische Aktualisierungen ausgeführt wird. Er ist standardmäßig aktiviert, wenn Sie den Agent installieren. Die automatische Aktualisierung ist erforderlich, damit Microsoft den Support für Ihre Bereitstellung leisten kann.
- Vermeiden Sie alle Arten von Inline-Untersuchungen der ausgehenden TLS-Kommunikation zwischen den Agents und Azure. Diese Art der Inline-Untersuchung führt zu einer Verschlechterung des Kommunikationsflusses.
- Der Agent muss sowohl mit Azure als auch mit Ihren Anwendungen kommunizieren. Daher wirkt sich die Platzierung des Agenten auf die Latenz dieser beiden Verbindungen aus. Sie können die Wartezeit des End-to-End-Datenverkehrs verringern, indem Sie die einzelnen Netzwerkverbindungen optimieren. Jede Verbindung kann wie folgt optimiert werden:
  - Reduzieren Sie den Abstand zwischen den beiden Enden des Hops.
  - Wählen Sie das richtige zu durchlaufende Netzwerk. Wenn beispielsweise, anstelle des öffentlichen Internets beispielsweise ein privates Netzwerk durchlaufen wird, kann dies aufgrund von dedizierten Links schneller gehen.

## <a name="provisioning-agent-questions"></a>Fragen zum Bereitstellungs-Agent
Hier werden einige häufig gestellte Fragen beantwortet.

### <a name="what-is-the-ga-version-of-the-provisioning-agent"></a>Wie lautet die allgemein verfügbare Version des Bereitstellungs-Agenten?

Die neueste GA-Version des Bereitstellungs-Agenten finden Sie unter [Azure AD Connect Bereitstellungs-Agent: Versionsveröffentlichungsverlauf](provisioning-agent-release-version-history.md).

### <a name="how-do-i-know-the-version-of-my-provisioning-agent"></a>Wie kann ich die Version meines Bereitstellungs-Agent ermitteln?

 1. Melden Sie sich bei dem Windows Server an, auf dem der Bereitstellungs-Agent installiert ist.
 1. Wechseln Sie zur **Systemsteuerung** > **Deinstallieren oder Ändern eines Programms**.
 1. Suchen Sie nach der Version für den Eintrag **Microsoft Azure AD Connect-Bereitstellungs-Agent**.

### <a name="does-microsoft-automatically-push-provisioning-agent-updates"></a>Überträgt Microsoft automatisch Updates des Bereitstellungs-Agent per Push?

Ja. Der Bereitstellungs-Agent wird von Microsoft automatisch aktualisiert, wenn der Windows-Dienst Microsoft Azure AD Connect Agent Updater aktiv ist und ausgeführt wird. Die Sicherstellung, dass Ihr Agent auf dem aktuellen Stand ist, ist erforderlich, damit der Support die Problembehandlung durchführen kann.

### <a name="can-i-install-the-provisioning-agent-on-the-same-server-running-azure-ad-connect-or-microsoft-identity-manager"></a>Kann ich den Bereitstellungs-Agent auf demselben Server installieren, auf dem Azure AD Connect oder Microsoft Identity Manager ausgeführt wird?

Ja. Sie können den Bereitstellungs-Agent auf demselben Server installieren, auf dem Azure AD Connect oder der Microsoft Identity Manager ausgeführt wird, aber das ist nicht zwingend erforderlich.

### <a name="how-do-i-configure-the-provisioning-agent-to-use-a-proxy-server-for-outbound-http-communication"></a>Wie konfiguriere ich den Bereitstellungs-Agent, damit dieser einen Proxyserver für die ausgehende HTTP-Kommunikation verwendet?

Der Bereitstellungs-Agent unterstützt die Verwendung von Proxys für ausgehenden Datenverkehr. Das können Sie durch das Bearbeiten der Konfigurationsdatei des Agenten **C:\Programm Dateien\Microsoft Azure AD Connect Bereitstellungs-Agent\AADConnectProvisioningAgent.exe.config** konfigurieren. Fügen Sie in der Datei gegen Ende unmittelbar vor dem schließenden `</configuration>` Tag die folgenden Zeilen ein. Ersetzen Sie die Variablen `[proxy-server]` und `[proxy-port]` durch den Namen Ihres Proxyservers und die Portwerte.

```
    <system.net>
        <defaultProxy enabled="true" useDefaultCredentials="true">
            <proxy
                usesystemdefault="true"
                proxyaddress="http://[proxy-server]:[proxy-port]"
                bypassonlocal="true"
            />
        </defaultProxy>
    </system.net>
```
### <a name="how-do-i-ensure-the-provisioning-agent-can-communicate-with-the-azure-ad-tenant-and-no-firewalls-are-blocking-ports-required-by-the-agent"></a>Wie stelle ich sicher, dass der Bereitstellungs-Agent mit dem Azure AD-Mandanten kommunizieren kann und dass keine Firewalls die vom Agenten benötigten Ports blockieren?

Sie können auch überprüfen, ob alle erforderlichen Ports geöffnet sind.

### <a name="how-do-i-uninstall-the-provisioning-agent"></a>Wie deinstalliere ich den Bereitstellungs-Agent?
1. Melden Sie sich bei dem Windows Server an, auf dem der Bereitstellungs-Agent installiert ist.
1. Wechseln Sie zur **Systemsteuerung** > **Deinstallieren oder Ändern eines Programms**.
1. Deinstallieren Sie die folgenden Programme:
     - Microsoft Azure AD Connect-Bereitstellungs-Agent
     - Microsoft Azure AD Connect Agent Updater
     - Microsoft Azure AD Connect-Bereitstellungs-Agent-Paket


## <a name="next-steps"></a>Nächste Schritte

- [App-Bereitstellung](user-provisioning.md)
- [Azure AD-ECMA-Connectorhost: Voraussetzungen](on-premises-ecma-prerequisites.md)
- [Azure AD-ECMA-Connectorhost: Installation](on-premises-ecma-install.md)
- [Azure AD-ECMA-Connectorhost: Konfiguration](on-premises-ecma-configure.md)
