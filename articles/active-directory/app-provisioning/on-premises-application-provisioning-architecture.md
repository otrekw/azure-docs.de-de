---
title: Azure AD-Architektur für die lokale Anwendungsbereitstellung | Microsoft-Dokumentation
description: Enthält eine Übersicht über die Architektur für die lokale Anwendungsbereitstellung.
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
ms.openlocfilehash: 696f2f9e54e350b2cce952cbec8d745779e1d00a
ms.sourcegitcommit: ff1aa951f5d81381811246ac2380bcddc7e0c2b0
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/07/2021
ms.locfileid: "111570356"
---
# <a name="azure-ad-on-premises-application-provisioning-architecture"></a>Azure AD-Architektur für die lokale Anwendungsbereitstellung

>[!IMPORTANT]
> Die Vorschauversion der lokalen Bereitstellung ist derzeit nur auf Einladung verfügbar. Sie können [hier](https://aka.ms/onpremprovisioningpublicpreviewaccess) den Zugriff auf die Funktion beantragen. Wir öffnen die Vorschauversion im Rahmen der Vorbereitung auf die allgemeine Verfügbarkeit in den nächsten Monaten für weitere Kunden und Connectors.

## <a name="overview"></a>Übersicht

Das folgende Diagramm enthält eine Übersicht über die Funktionsweise der lokalen Anwendungsbereitstellung.

![Aufbau](.\media\on-premises-application-provisioning-architecture\arch-3.png)

Es gibt drei Hauptkomponenten für die Bereitstellung von Benutzern in einer lokalen Anwendung.

- Der Bereitstellungs-Agent ermöglicht die Konnektivität zwischen Azure AD und Ihrer lokalen Umgebung.
- Der ECMA-Host konvertiert Bereitstellungsanforderungen von Azure AD in Anforderungen, die an Ihre Zielanwendung gesendet werden. Er dient als Gateway zwischen Azure AD und Ihrer Anwendung. Mit dem Host können Sie vorhandene ECMA2-Connectors importieren, die mit Microsoft Identity Manager verwendet werden. Hinweis: Der ECMA-Host ist nicht erforderlich, wenn Sie eine SCIM-Anwendung oder ein SCIM-Gateway erstellt haben.
- Der Azure AD-Bereitstellungsdienst dient als Synchronisierungs-Engine.

>[!NOTE]
> Die MIM-Synchronisierung ist nicht erforderlich. Sie können die MIM-Synchronisierung aber verwenden, um Ihren ECMA-Connector zu erstellen und zu testen, bevor Sie diesen auf dem ECMA-Host importieren.


### <a name="firewall-requirements"></a>Firewallanforderungen

Sie müssen keine eingehenden Verbindungen mit dem Unternehmensnetzwerk öffnen. Für die Bereitstellungs-Agents werden nur ausgehende Verbindungen mit dem Bereitstellungsdienst verwendet. Dies bedeutet, dass keine Firewallports für eingehende Verbindungen geöffnet werden müssen. Sie benötigen auch kein Umkreisnetzwerk (DMZ), da alle Verbindungen in ausgehender Richtung verlaufen und über einen sicheren Kanal erfolgen. 

## <a name="agent-best-practices"></a>Bewährte Methoden für den Agent
- Vergewissern Sie sich, dass für den Azure AD Connect-Bereitstellungs-Agent der Dienst für automatische Aktualisierungen ausgeführt wird. Er wird bei der Installation des Agents standardmäßig aktiviert. Die automatische Aktualisierung ist erforderlich, damit Microsoft den Support für Ihre Bereitstellung leisten kann.
- Vermeiden Sie alle Arten von Inline-Untersuchungen der ausgehenden TLS-Kommunikation zwischen den Agents und Azure. Diese Art der Inline-Untersuchung führt zu einer Verschlechterung des Kommunikationsflusses.
- Der Agent muss sowohl mit Azure als auch mit Ihren Anwendungen kommunizieren. Daher wirkt sich die Platzierung des Agents auf die Latenz dieser beiden Verbindungen aus. Sie können die Wartezeit des End-to-End-Datenverkehrs verringern, indem Sie die einzelnen Netzwerkverbindungen optimieren. Jede Verbindung kann wie folgt optimiert werden:
- Reduzieren Sie den Abstand zwischen den beiden Enden des Hops.
- Wählen Sie das richtige zu durchlaufende Netzwerk. Wenn anstelle des öffentlichen Internet beispielsweise ein privates Netzwerk durchlaufen wird, kann dies aufgrund von dedizierten Links schneller gehen.

## <a name="provisioning-agent-questions"></a>Fragen zum Bereitstellungs-Agent
**Wie lautet die allgemein verfügbare Version des Bereitstellungs-Agents?**

Unter [Azure AD Connect-Bereitstellungs-Agent: Verlauf der Versionsveröffentlichungen](provisioning-agent-release-version-history.md) finden Sie die aktuelle GA-Version des Bereitstellungs-Agents.

**Wie kann ich die Version meines Bereitstellungs-Agents ermitteln?**

 1. Melden Sie sich bei dem Windows Server an, auf dem der Bereitstellungs-Agent installiert ist.
 2. Navigieren Sie zu „Systemsteuerung“ > „Programm deinstallieren oder ändern“.
 3. Suchen Sie nach der Version für den Eintrag Microsoft Azure AD Connect-Bereitstellungs-Agent.

**Überträgt Microsoft automatisch Updates des Bereitstellungs-Agents per Push?**

Ja. Der Bereitstellungs-Agent wird von Microsoft automatisch aktualisiert, wenn der Windows-Dienst Microsoft Azure AD Connect Agent Updater aktiv ist und ausgeführt wird. Die Sicherstellung, dass Ihr Agent auf dem aktuellen Stand ist, ist erforderlich, damit der Support die Problembehandlung durchführen kann.

**Kann ich den Bereitstellungs-Agent auf demselben Server installieren, auf dem Azure AD Connect oder Microsoft Identity Manager (MIM) ausgeführt wird?**

Ja. Sie können den Bereitstellungs-Agent auf demselben Server installieren, auf dem Azure AD Connect oder MIM ausgeführt wird, aber dies ist nicht zwingend erforderlich.

**Wie konfiguriere ich den Bereitstellungs-Agent, damit dieser einen Proxyserver für die ausgehende HTTP-Kommunikation verwendet?**

Der Agent-Bereitstellung unterstützt die Verwendung von Proxys für ausgehenden Datenverkehr. Dies können Sie durch das Konfigurieren der Konfigurationsdatei des Agent **C:\Program Files\Microsoft Azure AD Connect Provisioning Agent\AADConnectProvisioningAgent.exe.config** konfigurieren. Fügen Sie die folgenden Zeilen zum Ende der Datei unmittelbar vor dem </configuration>-Tag ein. Ersetzen Sie der Variablen [[proxy-server] ] und [proxy-port] durch den Namen Ihres Proxyservers und die Portwerte.
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
**Wie stelle ich sicher, dass der Bereitstellungs-Agent mit dem Azure AD-Mandanten kommunizieren kann, und dass keine Firewalls die vom Agenten benötigten Ports blockieren?**

Sie können auch überprüfen, ob alle erforderlichen Ports geöffnet sind.

**Wie deinstalliere ich den Bereitstellungs-Agent?**
1. Melden Sie sich bei dem Windows Server an, auf dem der Bereitstellungs-Agent installiert ist.
2. Navigieren Sie zu „Systemsteuerung“ > „Programm deinstallieren oder ändern“.
3. Deinstallieren Sie die folgenden Programme:
     - Microsoft Azure AD Connect-Bereitstellungs-Agent
     - Microsoft Azure AD Connect Agent Updater
     - Microsoft Azure AD Connect-Bereitstellungs-Agent-Paket


## <a name="next-steps"></a>Nächste Schritte

- [App-Bereitstellung](user-provisioning.md)
- [Azure AD-ECMA-Connectorhost: Voraussetzungen](on-premises-ecma-prerequisites.md)
- [Azure AD-ECMA-Connectorhost: Installation](on-premises-ecma-install.md)
- [Azure AD-ECMA-Connectorhost: Konfiguration](on-premises-ecma-configure.md)
