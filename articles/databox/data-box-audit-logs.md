---
title: Überwachungsprotokolle für Azure Data Box- und Azure Data Box Heavy-Ereignisse| Microsoft-Dokumentation
description: In diesem Artikel werden die vollständigen Überwachungsprotokolle für Data Box behandelt, die in den verschiedenen Phasen Ihres Azure Data Box- und Azure Data Box Heavy-Auftrags erfasst werden.
services: databox
author: alkohli
ms.service: databox
ms.subservice: pod
ms.topic: article
ms.date: 07/10/2020
ms.author: alkohli
ms.openlocfilehash: 50dbe9ab649a708fb36b1c9e4fe89bccadc7ea90
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/09/2020
ms.locfileid: "86209056"
---
# <a name="audit-logs-for-your-azure-data-box-and-azure-data-box-heavy"></a>Überwachungsprotokolle für Azure Data Box und Azure Data Box Heavy

Protokolle sind unveränderliche, mit einem Zeitstempel versehene Aufzeichnungen von einzelnen Ereignissen, die im Laufe der Zeit aufgetreten sind. Die Protokolle enthalten Diagnose-, Überwachungs- und Sicherheitsinformationen von Ihrem Gerät.  

Ein Data Box- oder Data Box Heavy-Auftrag durchläuft bei der Ausführung die folgenden Schritte: Bestellen, Einrichten, Kopieren von Daten, Zurücksenden, Hochladen in Azure und Überprüfen und Löschen von Daten. Bei jedem dieser Schritte werden alle Ereignisse überwacht und protokolliert.

Dieser Artikel enthält Informationen zu den Data Box-Überwachungsprotokollen einschließlich der Arten von Protokollen und der erfassten Informationen sowie des jeweiligen Speicherorts der Protokolle. 

Die Informationen in diesem Artikel gelten sowohl für Data Box als auch für Data Box Heavy. In den folgenden Abschnitten gelten alle Verweise auf Data Box auch für Data Box Heavy. Die Protokolle, die über den in Azure ausgeführten Data Box-Dienst erfasst werden, werden in diesem Artikel nicht behandelt. 


## <a name="about-audit-logs"></a>Informationen zu Überwachungsprotokollen 

Auf Ihrer Data Box werden die folgenden Protokolle erfasst:

- **Systemprotokolle:** Da die Data Box ein Windows-basiertes Gerät ist, werden alle Hardware-, Software- und Systemereignisse protokolliert. Ein Satz dieser Ereignisse wird erfasst und in den Systemüberwachungsprotokollen gemeldet. 

- **Sicherheit:** Da die Data Box ein Windows-basiertes Gerät ist, werden alle sicherheitsrelevanten Ereignisse protokolliert. Ein Satz dieser Ereignisse wird erfasst und in den Sicherheitsüberwachungsprotokollen gemeldet. 

- **Anwendung:** Diese Protokolle beziehen sich nur auf Data Box. Sie enthalten alle Ereignisse, die als Reaktion auf die ausgeführten Data Box-Dienste auf dem Gerät generiert werden.

Die einzelnen Protokolle werden im folgenden Abschnitt behandelt.

### <a name="system-logs"></a>Systemprotokolle

Die folgenden Systemprotokollereignis-IDs werden als Systemüberwachungsprotokolle auf Ihrer Data Box erfasst:

|Name des Ereignisanbieters     |Erfasste Ereignis-ID   |Ereignisbeschreibung   |
|-------------------|----------|----------------|
|Microsoft-Windows-Kernel-General|12  |UTC-Zeit, zu der das Betriebssystem neu gestartet wurde   |
|                                |13  |UTC-Zeit, zu der das Betriebssystem heruntergefahren wurde |
|    |                              |
|Microsoft-Windows-Kernel-Power  |41  |Das System wurde ohne sauberes Herunterfahren neu gestartet.| 
|    |                              |
|Microsoft-Windows-BitLocker-Driver|All|    |

### <a name="security-logs"></a>Sicherheitsprotokolle

Die folgenden Sicherheitsprotokollereignis-IDs werden als Sicherheitsüberwachungsprotokolle auf Ihrer Data Box erfasst:

|Name des Ereignisanbieters                   |Erfasste Ereignis-ID    |Ereignisbeschreibung       |
|--------------------------------------|------------|----------|
|Microsoft-Windows-Security-Auditing   |4624        |Erfolgreiche Anmeldung |
|                                      |4625        |Fehler bei einer Kontoanmeldung. Unbekannter Benutzername oder falsches Kennwort. |
|                                     

### <a name="application-logs"></a>Anwendungsprotokolle

Die folgenden Anwendungsprotokollereignis-IDs werden als Teil der Paketüberwachungsprotokolle auf Ihrer Data Box erfasst.     

- **Microsoft-Azure-DataBox-OOBE-Auditing:** Enthält die Ereignisse, die in der lokalen Benutzeroberfläche auftreten 
- **Microsoft-Azure-DataBox-Reprovision-Audit:** Enthält Ereignisse im Zusammenhang mit der erneuten Bereitstellung des Data Box-Geräts. Die erneute Bereitstellung der Data Box erfolgt, wenn das Gerät über die lokale Benutzeroberfläche zurückgesetzt wird. Wählen Sie diese Option, wenn Sie die kopierten Daten löschen möchten, indem Sie die vorhandenen Freigaben entfernen und die Freigaben im Rahmen der erneuten Bereitstellung oder Gerätezurücksetzung erneut erstellen.
- **Microsoft-Azure-DataBox-HcsMgmt-Audit:** Enthält Ereignisse, die sich nur auf den Schritt **Für Versand vorbereiten** vor dem Zurücksenden des Geräts an das Azure-Rechenzentrum beziehen 
- **Microsoft-Azure-DataBox-IfxAudit:** Enthält die Nachrichten, die von verschiedenen Entitäten des Produkts über die Aufträge protokolliert wurden, Protokolle mit weiteren Informationen zu dem, was in einigen Flows passiert

In der folgenden Tabelle sind die verschiedenen Ereignisanbieter und die entsprechenden Ereignis-IDs, die jeweils erfasst werden, zusammengefasst.

|Name des Ereignisanbieters    |Ereignis-ID    | Notizen |
|-----------------|-----------------|-------------------|
|Microsoft-Azure-DataBox-OOBE-Auditing |4624        |Erfolgreiche Anmeldung|
|                                      |4625        |Fehler bei einer Kontoanmeldung. Unbekannter Benutzername oder falsches Kennwort.|
|                                     |4634        |Abmeldeereignis|
|                                   |  | |
|Microsoft-Azure-DataBox-Reprovision-Audit    |65001       |Ereignis bei erfolgreicher erneuter Bereitstellung|
|                                                  |65002       |Ereignis bei einem Fehler bei der erneuten Bereitstellung|
|                                                  |                 |         |
|Microsoft-Azure-DataBox-HcsMgmt-Audit        |65003       |Ereignis für Status „Für Versand vorbereiten“:     NotStarted,     InProgress,     Failed,     Cancelled,     Succeeded,     ScanCompletedWithIssues,     SucceededWithWarnings          |
|                                                  |                 |     |
|Microsoft-Azure-DataBox-IfxAudit    |All |Alle Ereignisse werden mit der Überwachungsprotokoll-API im Code protokolliert. |

Hier sehen Sie ein Beispiel für das Überwachungsprotokoll für das Instrumentierungsframework (IFX):

|     Task/Auftrag/API                              |     Protokollierte Ereignisse                                                                                                              | 
|-----------------------------------------------|------------------------------------------------------------------------------------------------------------------------------|
|     Cleanup                                   |     Die Ereignisse im Zusammenhang mit dem Start oder Abschluss eines Bereinigungsauftrags sowie mit dabei auftretenden Fehlern werden protokolliert. |                                              
|     Vorbereiten des Geräts für den Kundenversand    |     Die Ereignisse im Zusammenhang mit dem Start oder Abschluss des Auftrags zur Vorbereitung des Geräts für den Versand sowie mit dabei auftretenden Fehlern werden protokolliert. |
|     Bereitstellen                                 |     Die Ereignisse im Zusammenhang mit dem Start oder Abschluss eines Gerätebereitstellungsauftrags sowie mit dabei auftretenden Fehlern werden protokolliert.|
|     Überwachungspaketauftrag                       |     Die Ereignisse im Zusammenhang mit dem Start oder Abschluss eines Überwachungspaketauftrags, der Rückverfolgbarkeitsprotokolle erstellt, sowie mit dabei auftretenden Fehlern werden protokolliert.|
|     Überschreiben von Datenträgern                          |     Fehler beim Überschreiben des Datenträgers werden protokolliert.|
|     Aktivieren oder Deaktivieren von Remote-PowerShell     |     Die Ereignisse im Zusammenhang mit dem Aktivieren oder Deaktivieren von Remote-PowerShell auf dem Gerät werden protokolliert. |
|     Abrufen von Details zu Installationsphasen               |     Die Ereignisse im Zusammenhang mit der Installation von Software auf dem Gerät in Phasen werden im Azure-Rechenzentrum protokolliert.|
|     Entsperren oder Sperren von BitLocker-Volumes           |     Die Ereignisse werden protokolliert, um den BitLocker-Status der Volumes *basevolume* und *hcsdata* anzugeben.|
|     Bereinigen des Datenträgers                              |     Die Ereignisse im Zusammenhang mit dem Ausfall physischer Datenträger, die nicht gelöscht werden konnten, und die Ereignisse bei einem erfolgreichen Löschen aller physischen Datenträger auf dem Gerät werden protokolliert. |
|     Aktivieren oder Deaktivieren von lokalen Benutzern               |     Die Ereignisse im Zusammenhang mit dem Aktivieren oder Deaktivieren von lokalen Benutzerkonten für StorSimpleAdmin und PodSupportAdminUser werden protokolliert.| 
|     Kennwortzurücksetzung                          |     Die Ereignisse im Zusammenhang mit einer erfolgreichen oder fehlerhaften Kennwortzurücksetzung für den lokalen StorSimpleAdmin-Benutzer werden protokolliert. |


Neben den IFX-Überwachungsprotokollen werden auch Rückverfolgbarkeitsüberwachungsprotokolle für Data Box erfasst. Diese Protokolle können nicht in Echtzeit angezeigt werden, sondern erst, wenn der Auftrag abgeschlossen wurde und die Daten von den Data Box-Datenträgern gelöscht wurden. Diese Protokolle enthalten eine Teilmenge der Informationen, die in den IFX-Überwachungsprotokollen enthalten sind.

Weitere Informationen zu den Rückverfolgbarkeitsüberwachungsprotokollen finden Sie unter [Abrufen der Rückverfolgbarkeitsprotokolle nach dem Löschen von Daten](data-box-logs.md#get-chain-of-custody-logs-after-data-erasure).

<!-- write a few lines about order history and link out to the detailed section on order history-->

## <a name="access-audit-logs"></a>Zugreifen auf Überwachungsprotokolle

Diese Protokolle werden in Azure gespeichert, und es kann nicht direkt auf sie zugegriffen werden. Wenn Sie Zugriff benötigen, müssen Sie ein Supportticket erstellen. Weitere Informationen finden Sie unter [Kontaktaufnahme mit dem Microsoft-Support](data-box-disk-contact-microsoft-support.md). 

Sobald das Supportticket erstellt wurde, werden die Protokolle von Microsoft heruntergeladen, und Sie erhalten Zugriff darauf.


## <a name="next-steps"></a>Nächste Schritte

- Erfahren Sie mehr über das [Behandeln von Problemen mit Azure Data Box und Azure Data Box Heavy](data-box-troubleshoot.md).
