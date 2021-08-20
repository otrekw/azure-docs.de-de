---
title: 'Häufig gestellte Fragen: Azure Monitor für SAP-Lösungen | Microsoft-Dokumentation'
description: In diesem Artikel finden Sie Antworten auf häufig gestellte Fragen (FAQs) zu Azure Monitor für SAP-Lösungen.
author: rdeltcheva
ms.service: virtual-machines-sap
ms.subservice: baremetal-sap
ms.topic: article
ms.date: 07/08/2020
ms.author: radeltch
ms.openlocfilehash: dfc28d9ced0ca65b4ebe0e49ade05eb822c59bab
ms.sourcegitcommit: e0ef8440877c65e7f92adf7729d25c459f1b7549
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/09/2021
ms.locfileid: "113567630"
---
# <a name="azure-monitor-for-sap-solutions-faq-preview"></a>Häufig gestellte Fragen zu Azure Monitor für SAP-Lösungen (Vorschauversion)
## <a name="frequently-asked-questions"></a>Häufig gestellte Fragen

Dieser Artikel enthält Antworten auf häufig gestellte Fragen (FAQs) zu Azure Monitor für SAP-Lösungen.  

 - **Ist Azure Monitor für SAP-Lösungen kostenpflichtig?**  
Für Azure Monitor für SAP-Lösungen fallen keine Lizenzgebühren an. Sie sind aber für die Kosten der Komponenten von verwalteten Ressourcengruppen verantwortlich.  

 - **In welchen Regionen ist dieser Dienst als Public Preview verfügbar?**  
Dieser Dienst ist in den Regionen „USA, Osten 2“, „USA, Westen 2“, „USA, Osten“ und „Europa, Westen“ als öffentliche Vorschauversion (Public Preview) verfügbar.  

 - **Muss ich Berechtigungen erteilen, um die Bereitstellung einer verwalteten Ressourcengruppe in meinem Abonnement zuzulassen?**  
Nein. Es sind keine expliziten Berechtigungen erforderlich.  

 - **Wo befindet sich die Sammler-VM?**  
Für die Bereitstellung von Azure Monitor für SAP-Lösungen empfehlen wir Ihnen, dasselbe VNET für Ihre Überwachungsressource wie für Ihren SAP HANA-Server auszuwählen. Daher lautet unsere Empfehlung, dass sich die Sammler-VM in demselben VNET wie der SAP HANA-Server befinden sollte. Bei Verwendung einer anderen Datenbank als einer HANA-Datenbank ist die Sammler-VM in demselben VNET wie diese andere Datenbank angeordnet.  

 - **Welche Versionen von HANA werden unterstützt?**  
HANA 1.0 SPS 12 (ab Rev. 120) und HANA 2.0 SPS03 oder höher werden unterstützt. 

 - **Welche HANA-Bereitstellungskonfigurationen werden unterstützt?**  
Die folgenden Konfigurationen werden unterstützt:
   - Einzelner Knoten (Hochskalieren) und mehrere Knoten (Aufskalieren)  
   - Einzelner Datenbankcontainer (HANA 1.0 SPS 12) und mehrere Datenbankcontainer (HANA 1.0 SPS 12 oder HANA 2.0)
   - Automatisches Hostfailover (n+1) und HSR  

 - **Welche SQL Server-Versionen werden unterstützt?**  
SQL Server 2012 SP4 oder höher  

 - **Welche SQL Server-Konfigurationen werden unterstützt?**  
Die folgenden Konfigurationen werden unterstützt:
   - Standardinstanzen oder benannte eigenständige Instanzen auf einem virtuellen Computer  
   - Gruppierte Instanzen oder Instanzen in einer AlwaysOn-Konfiguration (bei Verwendung des virtuellen Namens der Clusterressource oder des AlwaysOn-Listenernamens)
   - Derzeit werden keine cluster- oder Always On-spezifischen Metriken erfasst    
   - Azure SQL-Datenbank (PaaS) wird derzeit nicht unterstützt  

 - **Was passiert, wenn ich versehentlich die verwaltete Ressourcengruppe lösche?**  
Die verwaltete Ressourcengruppe ist standardmäßig gesperrt. Die Wahrscheinlichkeit, dass die verwaltete Ressourcengruppe versehentlich gelöscht wird, ist daher äußerst gering. Falls Sie die verwaltete Ressourcengruppe löschen, funktioniert Azure Monitor für SAP-Lösungen nicht mehr. Sie müssen eine neue Ressource vom Typ „Azure Monitor für SAP-Lösungen“ bereitstellen und von vorn beginnen.  

 - **Welche Rollen benötige ich in meinem Azure-Abonnement für die Bereitstellung einer Ressource vom Typ „Azure Monitor für SAP-Lösungen“?**  
Die Rolle „Mitwirkender“.  

 - **Welche SLA gilt für dieses Produkt?**  
Vorschauversionen sind von Vereinbarungen zum Servicelevel ausgeschlossen. Lesen Sie sich die gesamten Lizenzbedingungen zum Marketplace-Image von Azure Monitor für SAP-Lösungen durch.  

 - **Kann ich mit dieser Lösung meine gesamte Umgebung überwachen?**  
In der öffentlichen Vorschauversion können Sie derzeit Folgendes überwachen:
- HANA-Datenbank
- Zugrunde liegende Infrastruktur
- Hochverfügbarkeitscluster
- Microsoft SQL Server
- Verfügbarkeit von SAP NetWeaver
- Metriken zur Verfügbarkeit der SAP-Anwendungsinstanz

 - **Wird SAP Solution Manager durch diesen Dienst ersetzt?**  
Nein. Kunden können SAP Solution Manager weiterhin für die Geschäftsprozessüberwachung verwenden.  

 - **Welchen zusätzlichen Nutzen bietet dieser Dienst im Vergleich zu herkömmlichen Lösungen wie SAP HANA Cockpit/Studio?**  
Azure Monitor für SAP-Lösungen ist nicht auf HANA-Datenbanken beschränkt. Auch AnyDB wird von Azure Monitor für SAP-Lösungen unterstützt.  

- **Welche SAP NetWeaver-Versionen werden unterstützt?**  
SAP NetWeaver 7.0 oder höher.  

- **Welche SAP NetWeaver-Konfigurationen werden unterstützt?**  
Es werden ABAP- und Java-Konfigurationen und Konfigurationen des SAP NetWeaver-Anwendungsservers (Dual Stack) unterstützt.

- **Warum muss ich den Schutz von Methoden für die SAP NetWeaver-Anwendungsüberwachung aufheben?**  
In SAP-Versionen ab 7.3 sind die meisten Webdienstmethoden standardmäßig geschützt. Um durch das Aufrufen dieser Methoden Verfügbarkeits- und Leistungsmetriken abrufen zu können, müssen Sie den Schutz für die folgenden Methoden aufheben: „GetQueueStatistic“, „ABAPGetWPTable“, „GetProcessList“, „EnqGetStatistic“ und „GetSystemInstancelist“.

- **Ist das Aufheben des Schutzes von SAPCONTROL-Webmethoden mit Risiken verbunden?**  
Im Allgemeinen können Sie den Schutz von SAPCONTROL-Webmethoden aufheben, [ohne dass ein Sicherheitsrisiko entsteht](https://launchpad.support.sap.com/#/notes/1439348). Sie können den Zugriff auf ungeschützte Webmethoden über die Serverports 5XX13 und 5XX14 von „sapstartsrv“ einschränken. Fügen Sie hierzu in der SAP-Zugriffssteuerungsliste einen Filter hinzu. Unter dem [OSS-Hinweis](https://service.sap.com/sap/support/notes/1495075) ist die erforderliche Konfiguration beschrieben. 

- **Muss ich meine SAP-Instanzen neu starten, nachdem ich Systemkonfigurationen zum Einrichten des SAP NetWeaver-Anbieters vorgenommen habe?**  
Ja. Sobald Sie den Schutz von Methoden durch SAP-Konfigurationsänderungen aufgehoben haben, müssen Sie die entsprechenden SAP-Systeme neu starten, um sicherzustellen, dass die Konfigurationsänderungen aktualisiert werden.  

## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen zu Azure Monitor für SAP-Lösungen:

> [!div class="nextstepaction"]
> [Azure Monitor für SAP-Lösungen](azure-monitor-overview.md)
