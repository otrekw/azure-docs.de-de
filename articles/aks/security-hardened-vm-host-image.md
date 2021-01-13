---
title: Sicherheitshärtung bei AKS-Hosts für virtuelle Computer
description: Hier erfahren Sie mehr über die Sicherheitshärtung beim AKS-Hostbetriebssystem für virtuelle Computer.
services: container-service
author: mlearned
ms.topic: article
ms.date: 09/11/2019
ms.author: mlearned
ms.custom: mvc
ms.openlocfilehash: 84b826ce33b5395db5bd38e883b3a0fb3425725b
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/09/2020
ms.locfileid: "86244037"
---
# <a name="security-hardening-for-aks-agent-node-host-os"></a>Sicherheitshärtung beim AKS-Hostbetriebssystem für Agent-Knoten

Azure Kubernetes Service (AKS) ist ein sicherer Dienst, der die Anforderungen der SOC-, ISO-, PCI-DSS- und HIPAA-Standards erfüllt. Dieser Artikel befasst sich mit der Sicherheitshärtung, die auf AKS-Hosts für virtuelle Computer angewendet wird. Weitere Informationen zur AKS-Sicherheit finden Sie unter [Sicherheitskonzepte für Anwendungen und Cluster in Azure Kubernetes Service (AKS)](./concepts-security.md).

> [!Note]
> Dieses Dokument bezieht sich nur auf Linux-Agents in AKS.

AKS-Cluster werden auf virtuellen Hostcomputern bereitgestellt, die ein sicherheitsoptimiertes Betriebssystem ausführen, das für in AKS ausgeführte Container verwendet wird. Dieses Hostbetriebssystem basiert auf einem **Ubuntu 16.04.LTS**-Image, auf das zusätzliche Sicherheitshärtung und Optimierungen angewendet werden (siehe „Details der Sicherheitshärtung“).

Das Ziel des sicherheitsgehärteten Hostbetriebssystems ist es, die Angriffsfläche zu verringern und die Bereitstellung von Containern auf sichere Weise zu optimieren.

> [!Important]
> Das sicherheitsgehärtete Betriebssystem weist KEINE CIS-Benchmark auf. Obwohl es Überschneidungen mit CIS-Benchmarks gibt, ist das Ziel nicht, CIS-kompatibel zu sein. Das Ziel für die Härtung des Hostbetriebssystems ist es, eine Sicherheitsstufe zu erreichen, die den eigenen internen Microsoft-Sicherheitsstandards für Hosts entspricht.

## <a name="security-hardening-features"></a>Features für die Sicherheitshärtung

* AKS stellt standardmäßig ein sicherheitsoptimiertes Hostbetriebssystem bereit. Es gibt keine Option zur Auswahl eines alternativen Betriebssystems.

* Azure wendet tägliche Patches (einschließlich Sicherheitspatches) auf AKS-Hosts für virtuelle Computer an. Für einige dieser Patches ist ein Neustart erforderlich, für andere hingegen nicht. Es ist Ihre Aufgabe, die Neustarts von AKS-VM-Hosts bedarfsgerecht zu planen. Anleitungen zum Automatisieren von AKS-Patches finden Sie unter [Patchen von AKS-Knoten](./node-updates-kured.md).

## <a name="what-is-configured"></a>Was wird konfiguriert?

| CIS  | Überwachungsbeschreibung|
|---|---|
| 1.1.1.1 |Sicherstellen, dass die Einbindung von cramfs-Dateisystemen deaktiviert ist|
| 1.1.1.2 |Sicherstellen, dass die Einbindung von freevxfs-Dateisystemen deaktiviert ist|
| 1.1.1.3 |Sicherstellen, dass die Einbindung von jffs2-Dateisystemen deaktiviert ist|
| 1.1.1.4 |Sicherstellen, dass die Einbindung von HFS-Dateisystemen deaktiviert ist|
| 1.1.1.5 |Sicherstellen, dass die Einbindung von HFS Plus-Dateisystemen deaktiviert ist|
|1.4.3 |Sicherstellen, dass eine Authentifizierung für den Einzelbenutzermodus erforderlich ist |
|1.7.1.2 |Sicherstellen, dass das Warnbanner für lokale Anmeldung richtig konfiguriert ist |
|1.7.1.3 |Sicherstellen, dass das Warnbanner für Remoteanmeldung richtig konfiguriert ist |
|1.7.1.5 |Sicherstellen, dass Berechtigungen für „/etc/issue“ konfiguriert sind |
|1.7.1.6 |Sicherstellen, dass Berechtigungen für „/etc/issue.net“ konfiguriert sind |
|2.1.5 |Sicherstellen, dass „--streaming-connection-idle-timeout“ nicht auf 0 festgelegt ist |
|3.1.2 |Sicherstellen, dass das Senden von Paketumleitungen deaktiviert ist |
|3.2.1 |Sicherstellen, dass an die Quelle geleitete Pakete nicht akzeptiert werden |
|3.2.2 |Sicherstellen, dass ICMP-Umleitungen nicht akzeptiert werden |
|3.2.3 |Sicherstellen, dass sichere ICMP-Umleitungen nicht akzeptiert werden |
|3.2.4 |Sicherstellen, dass verdächtige Pakete protokolliert werden |
|3.3.1 |Sicherstellen, dass IPv6-Routerankündigungen nicht akzeptiert werden |
|3.5.1 |Sicherstellen, dass DCCP deaktiviert ist |
|3.5.2 |Sicherstellen, dass SCTP deaktiviert ist |
|3.5.3 |Sicherstellen, dass RDS deaktiviert ist |
|3.5.4 |Sicherstellen, dass TIPC deaktiviert ist |
|4.2.1.2 |Sicherstellen, dass die Protokollierung konfiguriert ist |
|5.1.2 |Sicherstellen, dass Berechtigungen für „/etc/crontab“ konfiguriert sind |
|5.2.4 |Sicherstellen, dass X11-Forwarding für SSH deaktiviert ist |
|5.2.5 |Sicherstellen, dass „MaxAuthTries“ für SSH auf höchstens 4 festgelegt ist |
|5.2.8 |Sicherstellen, dass Root-Anmeldung für SSH deaktiviert ist |
|5.2.10 |Sicherstellen, dass „PermitUserEnvironment“ für SSH deaktiviert ist |
|5.2.11 |Sicherstellen, dass nur genehmigte MAX-Algorithmen verwendet werden |
|5.2.12 |Sicherstellen, dass das Timeoutintervall für Leerlauf für SSH konfiguriert ist |
|5.2.13 |Sicherstellen, dass „LoginGraceTime“ für SSH auf höchstens eine Minute festgelegt ist |
|5.2.15 |Sicherstellen, dass das Warnbanner für SSH konfiguriert ist |
|5.3.1 |Sicherstellen, dass Anforderungen für die Kennworterstellung konfiguriert sind |
|5.4.1.1 |Sicherstellen, dass der Kennwortablauf höchstens 90 Tage beträgt |
|5.4.1.4 |Sicherstellen, dass die Sperre für inaktive Kennwörter höchstens 30 Tage beträgt |
|5.4.4 |Sicherstellen, dass der Befehl „umask“ für Standardbenutzer 027 oder stärker einschränkend ist |
|5.6 |Sicherstellen, dass der Zugriff auf den Befehl „su“ eingeschränkt ist|

## <a name="additional-notes"></a>Zusätzliche Hinweise
 
* Um die Angriffsfläche weiter zu verringern, wurden einige unnötige Kernelmodultreiber im Betriebssystem deaktiviert.

* Das sicherheitsgehärtete Betriebssystem wird speziell für AKS erstellt und verwaltet, außerhalb der AKS-Plattform aber NICHT unterstützt.

## <a name="next-steps"></a>Nächste Schritte  

Weitere Informationen zur AKS-Sicherheit finden Sie in den folgenden Artikeln: 

[Azure Kubernetes Service (AKS)](./intro-kubernetes.md)

[AKS – Sicherheitshinweise](./concepts-security.md)

[AKS – Bewährte Methoden](./best-practices.md)
