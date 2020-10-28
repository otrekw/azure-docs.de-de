---
title: Auswählen der richtigen PostgreSQL-Option in Azure
description: Enthält Richtlinien für die Auswahl der richtigen PostgreSQL-Serveroption für Ihre Bereitstellungen.
author: sunilagarwal
ms.author: sunila
ms.custom: mvc
ms.service: postgresql
ms.topic: overview
ms.date: 09/21/2020
ms.openlocfilehash: 8bc303f619d145cc280e6caab65781bd42d1b314
ms.sourcegitcommit: 3bcce2e26935f523226ea269f034e0d75aa6693a
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/23/2020
ms.locfileid: "92489692"
---
# <a name="choose-the-right-postgresql-server-option-in-azure"></a>Auswählen der richtigen PostgreSQL-Option in Azure

Mit Azure können Ihre PostgreSQL-Serverworkloads auf einem gehosteten virtuellen IaaS-Computer (Infrastructure-as-a-Service) oder als gehostete PaaS-Instanz (Platform-as-a-Service) ausgeführt werden. PaaS bietet mehrere Bereitstellungsoptionen mit jeweils mehreren Dienstebenen. Bei der Auswahl zwischen IaaS und PaaS müssen Sie entscheiden, ob Sie Ihre Datenbank verwalten, Patches anwenden und Sicherungen erstellen oder diese Vorgänge an Azure delegieren möchten.

Wählen Sie zwischen den folgenden drei Optionen in PaaS aus, oder entscheiden Sie sich für eine Ausführung auf Azure-VMs (IaaS).
- [Azure Database for PostgreSQL Single Server](./overview-single-server.md)
- [Azure Database for PostgreSQL Flexible Server](./flexible-server/overview.md)
- [Azure Database for PostgreSQL Hyperscale (Citus)]()

Die Option **PostgreSQL auf Azure-VMs** fällt in die Branchenkategorie IaaS. Mit diesem Dienst können Sie PostgreSQL-Serverinstanzen auf einem vollständig verwalteten virtuellen Computers auf der Azure-Cloudplattform ausführen. Alle aktuellen Versionen und Editionen von PostgreSQL können auf einem virtuellen IaaS-Computer installiert werden. Der wichtigste Unterschied zu Azure Database for PostgreSQL ist, dass PostgreSQL auf Azure-VMs die Kontrolle über die Datenbank-Engine ermöglicht. Diese Kontrolle ist aber mit der Verantwortung für die Verwaltung der VMs und vielen Datenbankverwaltungsaufgaben (DBA) verbunden. Zu diesen Aufgaben gehören die Wartung und das Patchen von Datenbankservern, die Datenbankwiederherstellung und der Entwurf für Hochverfügbarkeit.

Die Hauptunterschiede zwischen diesen Optionen werden in der folgenden Tabelle aufgeführt:

| **Attribut** | **Postgres auf Azure-VMs** | **PostgreSQL als PaaS** |
| ----- | ----- | ----- |
| <B> SLA zur Verfügbarkeit |– 99,99 % mit Verfügbarkeitsgruppen <br> – 99,95 % mit einzelnen VMs | – Single Server: 99,99 % <br> – Flexible Server: während der Vorschau nicht verfügbar <br> – Hyperscale (Citus): 99,95 % (bei aktivierter Hochverfügbarkeit)|
| <B> Patchen von Betriebssystemen und PostgreSQL | – Vom Kunden verwaltet | – Single Server: automatisch <br> – Flexible Server: automatisch mit optionalem vom Kunden verwaltetem Fenster <br> – Hyperscale (Citus): automatisch |
| <B> Hochverfügbarkeit | – Kunden übernehmen den Entwurf, die Implementierung, das Testen und die Wartung der Hochverfügbarkeit. Zu den Funktionen können Clustering, Replikation usw. zählen. | – Single Server: integriert <br> – Flexible Server: integriert <br> – Hyperscale (Citus): erstellt mit Standby |
| <B> Zonenredundanz | – Azure-VMs können für die Ausführung in unterschiedlichen Verfügbarkeitszonen eingerichtet werden. Für eine lokale Lösung müssen Kunden ihr eigenes sekundäres Rechenzentrum erstellen, verwalten und warten. | – Single Server: Nein <br> – Flexible Server: Ja <br> – Hyperscale (Citus) Nein |
| <B> Hybridszenario | – Vom Kunden verwaltet |– Single Server: Lesereplikat <br> – Flexible Server: Während der Vorschau nicht verfügbar <br> – Hyperscale (Citus) Nein |
| <B> Sichern und Wiederherstellen | – Vom Kunden verwaltet | – Single Server: integrierter Server mit Benutzerkonfiguration für lokale Replikation und Georeplikation <br> – Flexible Server: integrierter Server mit Benutzerkonfiguration für zonenredundantem Speicher <br> – Hyperscale (Citus): integriert |
| <B> Überwachen von Datenbankvorgängen | – Vom Kunden verwaltet | – Single Server, Flexible Server und Hyperscale (Citus): Ermöglichen Kunden das Festlegen von Warnungen für den Datenbankvorgang und das Reagieren beim Erreichen von Schwellenwerten. |
| <B> Advanced Threat Protection | – Kunden müssen diesen Schutz selbst einrichten. |– Single Server: Ja <br> – Flexible Server: Während der Vorschau nicht verfügbar <br> – Hyperscale (Citus) Nein |
| <B> Notfallwiederherstellung | – Vom Kunden verwaltet | – Single Server: Georedundante Sicherung und regionsübergreifende Lesereplikate <br> – Flexible Server: Während der Vorschau nicht verfügbar <br> – Hyperscale (Citus) Nein |
| <B> Intelligente Leistung | – Vom Kunden verwaltet | – Single Server: Ja <br> – Flexible Server: Während der Vorschau nicht verfügbar <br> – Hyperscale (Citus) Nein |

## <a name="total-cost-of-ownership-tco"></a>Gesamtkosten

Die Gesamtkosten sind oft der wichtigste Aspekt, der darüber entscheidet, was die beste Lösung für das Hosting von Datenbanken ist. Dies gilt unabhängig davon, ob Sie ein Startup-Unternehmen mit wenig liquiden Mitteln oder ein Team in einem etablierten Unternehmen sind, das unter engen Budgetbeschränkungen arbeitet. In diesem Abschnitt werden die Grundlagen zur Abrechnung und Lizenzierung in Azure in Bezug auf Azure Database for PostgreSQL und PostgreSQL auf Azure-VMs beschrieben.

## <a name="billing"></a>Abrechnung

Azure Database for PostgreSQL ist derzeit als Dienst in verschiedenen Tarifen mit unterschiedlichen Preisen für Ressourcen verfügbar. Alle Ressourcen werden auf Stundenbasis mit einer festen Gebühr abgerechnet. Aktuelle Informationen zu den derzeit unterstützten Dienstebenen, Computegrößen und Speichermengen finden Sie auf der Seite [Preise](https://azure.microsoft.com/pricing/details/postgresql/server/). Sie können die Dienstebenen und Computegrößen dynamisch an die unterschiedlichen Durchsatzanforderungen Ihrer Anwendung anpassen. Ihnen wird der ausgehende Internetdatenverkehr basierend auf den üblichen [Datenübertragungsraten](https://azure.microsoft.com/pricing/details/data-transfers/) berechnet.

Mit Azure Database for PostgreSQL wird die Datenbanksoftware automatisch von Microsoft konfiguriert, gepatcht und aktualisiert. Diese automatisierten Aktionen senken Ihre Verwaltungskosten. Außerdem bietet Azure Database for PostgreSQL [automatisierte Sicherungen](). Mithilfe dieser Funktionen können Sie erhebliche Kosteneinsparungen erzielen, vor allem bei einer großen Anzahl von Datenbanken. Im Gegensatz dazu können Sie mit PostgreSQL auf Azure-VMs eine beliebige PostgreSQL-Version auswählen und ausführen. Für die bereitgestellte VM zahlen Sie für Folgendes: die Speicherkosten für die Daten, den Speicher für die Sicherungen, die Überwachungsdaten und die Protokolle sowie die Kosten für den jeweils verwendeten PostgreSQL-Lizenztyp (sofern vorhanden).

Azure Database for PostgreSQL bietet integrierte Hochverfügbarkeit für jede Art von Unterbrechung auf Knotenebene und bietet gleichzeitig eine SLA-Garantie von 99,99 % für den Dienst. Für Hochverfügbarkeit von Datenbanken auf VMs sollten Sie jedoch Hochverfügbarkeitsoptionen wie die [Streamingreplikation](https://www.postgresql.org/docs/12/warm-standby.html#STREAMING-REPLICATION) verwenden, die für PostgreSQL-Datenbanken zur Verfügung stehen. Durch Verwendung einer unterstützten Hochverfügbarkeitsoption erhalten Sie keine zusätzliche SLA. Allerdings können Sie mit zusätzlichen Kosten und höherem Verwaltungsaufwand eine Datenbankverfügbarkeit von mehr als 99,99 % erzielen.

Weitere Informationen zu Preisen finden Sie in den folgenden Artikeln:
- [Azure Database for PostgreSQL – Preise](https://azure.microsoft.com/pricing/details/postgresql/server/)
- [Preise für virtuelle Computer](https://azure.microsoft.com/pricing/details/virtual-machines/)
- [Azure-Preisrechner](https://azure.microsoft.com/pricing/calculator/)

## <a name="administration"></a>Verwaltung

Bei vielen Unternehmen geht es bei der Entscheidung für den Wechsel zu einem Clouddienst ebenso um eine Vereinfachung der Verwaltung wie um die Kosten.

Bei IaaS übernimmt Microsoft folgende Aufgaben:

- Verwaltet die zugrunde liegende Infrastruktur
- Automatisiertes Patchen der zugrunde liegenden Hardware und der Betriebssysteme

Bei PaaS übernimmt Microsoft folgende Aufgaben:

- Verwaltet die zugrunde liegende Infrastruktur
- Automatisiertes Patchen der zugrunde liegenden Hardware, der Betriebssysteme und des Datenbankmoduls
- Verwaltung der Hochverfügbarkeit der Datenbank
- Automatische Erstellung von Sicherungen und Replikation aller Daten, um eine Notfallwiederherstellung zu ermöglichen
- Verschlüsselung von ruhenden Daten und Daten in Bewegung als Standard
- Überwachung Ihres Servers und Bereitstellung von Features für Statistiken zur Abfrageleistung und Leistungsempfehlungen

Mit Azure Database for PostgreSQL können Sie Ihre Datenbank weiterhin verwalten. Es ist aber keine Verwaltung von Datenbank-Engine, Betriebssystem oder Hardware mehr erforderlich. Hier sind einige Beispiele für Elemente angegeben, die Sie weiterhin verwalten können:

- Datenbanken
- Anmeldung
- Indexoptimierung
- Abfrageoptimierung
- Überwachung
- Sicherheit

Darüber hinaus erfordert die Konfiguration der Hochverfügbarkeit für ein anderes Rechenzentrum nur noch einen minimalen bzw. gar keinen Konfigurations- oder Verwaltungsaufwand mehr.

- Bei PostgreSQL auf Azure-VMs haben Sie die uneingeschränkte Kontrolle über das Betriebssystem und die Konfiguration der PostgreSQL-Serverinstanzen. Bei einer VM entscheiden Sie, wann ein Update oder Upgrade des Betriebssystems und der Datenbanksoftware durchgeführt werden soll und welche Patches angewendet werden sollen. Außerdem entscheiden Sie, wann zusätzliche Software, z. B. eine Antivirenanwendung, installiert werden soll. Durch einige bereitgestellte Automatisierungsfeatures lassen sich Patching, Sicherungen und Hochverfügbarkeit erheblich vereinfachen. Sie können die Größe der VM, die Anzahl von Datenträgern sowie deren Speicherkonfigurationen steuern. Weitere Informationen finden Sie unter [Größen virtueller Computer und Clouddienste für Azure](../virtual-machines/sizes.md).

## <a name="time-to-move-to-azure-postgresql-service-paas"></a>Zeit für den Wechsel zum Azure PostgreSQL-Dienst (PaaS)

- Azure Database for PostgreSQL ist die geeignete Lösung für cloudbasierte Anwendungen, wenn Entwicklerproduktivität und eine schnelle Markteinführung für neue Lösungen entscheidend sind. Aufgrund von programmgesteuerten Funktionen wie DBA ist der Dienst für Cloudarchitekten und Entwickler geeignet, da er den Verwaltungsaufwand für das zugrunde liegende Betriebssystem und die Datenbank verringert.

- Wenn Sie die Zeit und Kosten für den Erwerb neuer lokaler Hardware sparen möchten, ist PostgreSQL auf Azure-VMs die richtige Lösung für Anwendungen, die eine differenzierte Steuerung und Anpassung der PostgreSQL-Engine erfordern, die vom Dienst nicht unterstützt wird oder Zugriff auf das zugrunde liegende Betriebssystem erfordert.

## <a name="next-steps"></a>Nächste Schritte

- Weitere Informationen finden Sie unter [Azure Database for PostgreSQL – Preise](https://azure.microsoft.com/pricing/details/postgresql/server/).
- Legen Sie los, und erstellen Sie Ihren ersten Server.