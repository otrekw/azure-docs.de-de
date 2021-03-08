---
title: Microsoft Azure Data Box-Datenträger – Häufig gestellte Fragen | Microsoft-Dokumentation
description: Hier finden Sie häufig gestellte Fragen und Antworten zu Azure Data Box Disk, einer Cloudlösung, mit der Sie große Datenmengen an Azure übertragen können.
services: databox
author: alkohli
ms.service: databox
ms.subservice: disk
ms.topic: conceptual
ms.date: 02/17/2021
ms.author: alkohli
ms.custom: references_regions
ms.openlocfilehash: 7212fc4113c1de0a7aee4c6c02e8fa65f9828680
ms.sourcegitcommit: c27a20b278f2ac758447418ea4c8c61e27927d6a
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/03/2021
ms.locfileid: "101724828"
---
# <a name="azure-data-box-disk-frequently-asked-questions"></a>Azure Data Box Disk: Häufig gestellte Fragen

Mit der Cloudlösung für Microsoft Azure Data Box-Datenträger können Sie Daten im Terabyte-Bereich schnell, kostengünstig und zuverlässig an Azure senden. Diese FAQ-Seite enthält Fragen, die bei der Verwendung von Data Box-Datenträgern im Azure-Portal aufkommen können, und die dazugehörigen Antworten. 

Die Fragen und Antworten sind in folgende Kategorien unterteilt:

- Informationen zum Dienst
- Konfigurieren und Verbinden 
- Nachverfolgen des Status
- Migrieren von Daten 
- Überprüfen und Hochladen von Daten 


## <a name="about-the-service"></a>Informationen zum Dienst

### <a name="q-what-is-azure-data-box-service"></a>Q. Was ist der Azure Data Box-Dienst? 
A.  Der Azure Data Box-Dienst wurde für die Offlineerfassung von Daten konzipiert. Mit diesem Dienst wird ein Array mit Produkten verwaltet, die für den Datentransport für unterschiedliche Speicherkapazitäten ausgelegt sind. 

### <a name="q-what-are-azure-data-box-disks"></a>Q. Was sind Azure Data Box-Datenträger?
A. Die Azure Data Box-Datenträger ermöglichen eine schnelle, kostengünstige und sichere Übertragung von Daten im Terabyte-Bereich für Azure (ein- und ausgehend). Sie erhalten von Microsoft ein bis fünf Datenträger mit einer maximalen Speicherkapazität von 35 TB. Sie können diese Datenträger über den Data Box-Dienst im Azure-Portal leicht konfigurieren, anschließen und entsperren.  

Datenträger werden mithilfe der Microsoft-BitLocker-Laufwerkverschlüsselung verschlüsselt, und Ihre Verschlüsselungsschlüssel werden über das Azure-Portal verwaltet. Sie kopieren die Daten dann von den Servern des Kunden. Im Rechenzentrum migriert Microsoft Ihre Daten vom Laufwerk zur Cloud, indem ein Link für das schnelle Hochladen ins private Netzwerk verwendet wird, und anschließend werden die Daten in Azure hochgeladen.

### <a name="q-when-should-i-use-data-box-disks"></a>Q. Wann sollte ich Data Box-Datenträger nutzen?
A. Sie profitieren von der Verwendung von Data Box-Datenträgern, wenn Sie über 40 TB an Daten (oder weniger) verfügen, die Sie an Azure übertragen möchten.

### <a name="q-what-is-the-price-of-data-box-disks"></a>Q. Was kosten Data Box-Datenträger?
A. Informationen zum Preis von Data Box-Datenträgern finden Sie auf der [Seite mit den Preisen](https://azure.microsoft.com/pricing/details/databox/disk/).

### <a name="q-how-do-i-get-data-box-disks"></a>Q. Wie erhalte ich Data Box-Datenträger? 
A.  Melden Sie sich als Nächstes am Azure-Portal an, und erstellen Sie einen Data Box-Auftrag für Datenträger, um Azure Data Box-Datenträger zu erhalten. Geben Sie Ihre Kontaktinformationen und Benachrichtigungsdetails an. Nachdem Sie den Auftrag erstellt haben, werden die Datenträger je nach Verfügbarkeit innerhalb von zehn Tagen an Sie gesendet.

### <a name="q-what-is-the-maximum-amount-of-data-i-can-transfer-with-data-box-disks-in-one-instance"></a>Q. Welche Menge von Daten kann ich mit Data Box-Datenträgern auf einmal übertragen?
A. Bei fünf Datenträgern mit jeweils 8 TB Kapazität (7 TB nutzbare Kapazität) beträgt die maximale nutzbare Kapazität 35 TB. So können Sie 35 TB an Daten auf einmal übertragen. Um eine größere Datenmenge zu übertragen, können Sie weitere Datenträger bestellen.

### <a name="q-how-can-i-check-if-data-box-disks-are-available-in-my-region"></a>Q. Wie kann ich prüfen, ob Data Box-Datenträger in meiner Region verfügbar sind? 
A.  Unter [Regionale Verfügbarkeit](data-box-disk-overview.md#region-availability) können Sie sehen, in welchen Regionen Data Box-Datenträger derzeit verfügbar sind.  

### <a name="q-which-regions-can-i-store-data-in-with-data-box-disks"></a>Q. In welchen Regionen kann ich Daten über Data Box-Datenträger speichern?
A. Data Box Disk wird für alle Regionen in den USA, Kanada, Australien, Europa, Westen, und Europa, Norden, sowie Korea und Japan unterstützt. Es werden nur die öffentlichen Azure-Cloudregionen unterstützt. Azure Government oder andere Sovereign Clouds werden nicht unterstützt.

### <a name="q-how-can-i-import-source-data-present-at-my-location-in-one-countryregion-to-an-azure-region-in-a-different-country"></a>Q. Wie kann ich Quelldaten, die sich an einem Standort in einem Land bzw. einer Region befinden, in eine Azure-Region in einem anderen Land importieren?
A. Data Box Disk unterstützt die Datenerfassung nur innerhalb desselben Landes/derselben Region, in dem bzw. der sich auch das Ziel befindet. Internationale Grenzen werden nicht überquert. Die einzige Ausnahme betrifft Bestellungen innerhalb der Europäischen Union. Dort können Data Box Disks in und aus beliebige(n) EU-Länder(n) bzw. EU-Regionen gesendet werden.

Wenn Sie beispielsweise Daten von Ihrem Standort in Kanada in ein Speicherkonto in der Azure-Region „USA, Westen“ verschieben möchten, könnten Sie so vorgehen:

#### <a name="option-1"></a>Option 1: 

Senden Sie einen [unterstützten Datenträger](../import-export/storage-import-export-requirements.md?toc=%2fazure%2fstorage%2fblobs%2ftoc.json#supported-disks) mit Daten über den [Azure Import/Export-Dienst](../import-export/storage-import-export-service.md) vom Quellstandort in Kanada an das Azure-Rechenzentrum in „USA, Westen“.

#### <a name="option-2"></a>Option 2:

1. Bestellen Sie Data Box Disk in Kanada, indem Sie ein Speicherkonto beispielsweise in „Kanada, Mitte“ auswählen. Die SSD-Datenträger werden vom Azure-Rechenzentrum in „Kanada, Mitte“ an die Lieferadresse (in Kanada) gesendet, die bei Auftragserstellung angegeben wurde.

2. Nachdem die Daten von Ihrem lokalen Server auf die Datenträger kopiert wurden, senden Sie die Datenträger zurück an das Azure-Rechenzentrum in Kanada. Verwenden Sie dabei die von Microsoft bereitgestellten Rücksendeetiketten. Die Daten auf den Data Box Disk-Datenträgern werden dann in das Zielspeicherkonto in Azure Kanada hochgeladen, das während der Auftragserstellung ausgewählt wurde.

3. Anschließend können Sie die Daten mithilfe eines Tools wie AzCopy in ein Speicherkonto in „USA, Westen“ kopieren. Bei diesem Schritt fallen Kosten für [Standardspeicher](https://azure.microsoft.com/pricing/details/storage/) und [Bandbreite](https://azure.microsoft.com/pricing/details/bandwidth/) an, die in der Data Box Disk-Abrechnung nicht enthalten sind.

#### <a name="q-does-data-box-disk-store-any-customer-data-outside-of-the-service-region"></a>Q. Speichert Data Box Disk irgendwelche Kundendaten außerhalb der Dienstregion?

A. Nein. Data Box Disk speichert keine Kundendaten außerhalb der Dienstregion. Der Kunde ist der Eigentümer aller seiner Daten und kann sie an einem angegebenen Speicherort speichern, basierend auf dem Speicherkonto, das er während der Auftragserstellung ausgewählt hat.  

Zusätzlich zu den Kundendaten gibt es Data Box Disk-Daten, die Metadaten und Überwachungsprotokolle enthalten. In allen Regionen (mit Ausnahme von Brasilien, Süden, und Asien, Südosten) werden Data Box Disk-Daten über ein georedundantes Speicherkonto im [Regionspaar](../best-practices-availability-paired-regions.md) gespeichert und repliziert, um sie vor Datenverlust zu schützen.  

Aufgrund der [Anforderungen an die Datenresidenz](https://azure.microsoft.com/global-infrastructure/data-residency/#more-information) in Brasilien, Süden, und Asien, Südosten, werden Data Box Disk-Daten in einem zonenredundanten Speicherkonto (ZRS) gespeichert, sodass sie in einer einzigen Region enthalten sind. Für Asien, Südosten, werden alle Data Box Disk-Daten in Singapur und für Brasilien, Süden, in Brasilien gespeichert. 

Bei einer Dienstunterbrechung in Brasilien, Süden, und Asien, Südosten, können Kunden neue Aufträge aus einer anderen Region erstellen. Die neuen Aufträge werden aus der Region bereitgestellt, in der sie erstellt werden, und die Kunden sind für die Lieferung des Data Box Disk-Geräts in beide Richtungen verantwortlich.



### <a name="q-how-can-i-recover-my-data-if-an-entire-region-fails"></a>Q. Wie kann ich meine Daten wiederherstellen, wenn eine ganze Region ausfällt?

A. In extremen Fällen, in denen eine Region durch einen schwerwiegenden Notfall verloren geht, kann Microsoft ein regionales Failover initiieren. In diesem Fall ist keine Aktion Ihrerseits erforderlich. Ihr Auftrag wird über die gesamte Failoverregion ausgeführt, wenn sie sich innerhalb derselben Land- oder Handelsgrenze befindet. Bei einigen Azure-Regionen gibt es jedoch keine Regionspaare in derselben geografischen Grenze oder Handelsgrenze. Wenn in einer dieser Regionen ein Notfall eintritt, müssen Sie den Data Box-Auftrag erneut aus einer anderen verfügbaren Region erstellen und die Daten in der neuen Region in Azure kopieren. Weitere Informationen finden Sie unter [Geschäftskontinuität und Notfallwiederherstellung: Azure-Regionspaare](../best-practices-availability-paired-regions.md).

### <a name="q-whom-should-i-contact-if-i-encounter-any-issues--with-data-box-disks"></a>Q. An wen sollte ich mich wenden, falls Probleme mit Data Box-Datenträgern auftreten?
A. Falls bei Data Box-Datenträgern Probleme auftreten, [wenden Sie sich an den Microsoft-Support](./data-box-disk-contact-microsoft-support.md).

## <a name="order-device"></a>Bestellen des Geräts

### <a name="q-how-do-i-get-data-box-disk"></a>Q. Wie erhalte ich Data Box Disk? 
A.  Wenn Sie Azure Data Box Disk erhalten möchten, melden Sie sich beim Azure-Portal an, und erstellen Sie einen Data Box Disk-Auftrag. Geben Sie Ihre Kontaktinformationen und Benachrichtigungsdetails an. Nachdem Sie einen Auftrag erteilt haben, wird Ihnen Data Box Disk je nach Verfügbarkeit innerhalb von 10 Tagen zugestellt. Weitere Informationen finden Sie unter [Bestellen einer Data Box](data-box-disk-deploy-ordered.md).

### <a name="q-i-couldnt-create-a-data-box-disk-order-in-the-azure-portal-why"></a>Q. Ich konnte im Azure-Portal keinen Data Box Disk-Auftrag erstellen. Warum?
A. Wenn Sie keinen Data Box Disk-Auftrag erstellen können, gibt es entweder bei Ihrem Abonnementtyp oder Ihrem Zugriff ein Problem.

Überprüfen Sie Ihr Abonnement. Data Box Disk steht nur für Kunden mit den Abonnementangeboten Enterprise Agreement (EA) und Cloud Solution Provider (CSP) zur Verfügung. Wenn Sie keinen dieser Abonnementtypen haben, wenden Sie sich an Microsoft-Support, um Ihr Abonnement zu aktualisieren.

Wenn Sie eine für das Abonnement unterstützte Angebotsart haben, überprüfen Sie Ihre Zugriffsebene im Abonnement. Sie müssen Mitwirkender oder Besitzer in Ihrem Abonnement sein, um einen Auftrag erstellen zu können.

### <a name="q-how-long-will-my-order-take-from-order-creation-to-data-uploaded-to-azure"></a>Q. Wie lange dauert mein Auftrag von der Erstellung bis zum Hochladen der Daten in Azure?

A. Anhand der folgenden geschätzten Vorlaufzeiten für jede Phase der Auftragsverarbeitung können Sie sich gut vorstellen, was zu erwarten ist.  

Diese Vorlaufzeiten sind *Schätzwerte*. Die Zeit für jede Phase der Auftragsverarbeitung wird durch die Last auf das Rechenzentrum, gleichzeitige Bestellungen und andere Umgebungsbedingungen beeinträchtigt.

**Geschätzte Vorlaufzeiten für einen Data Box Disk-Auftrag:**

1. Bestellen von Data Box Disk: Einige Minuten, über das Portal
2. Datenträgerzuordnung und -vorbereitung: Bis zu 5 Werktage, abhängig von der Bestandsverfügbarkeit und der Anzahl der zur Verarbeitung ausstehenden Aufträge
3. Versand: 2-3 Werktage
4. Datenkopiervorgang am Kundenstandort: Abhängig von der Art der Daten, der Größe und der Anzahl der Dateien
5. Rücksendung: 2-3 Werktage
6. Verarbeitung im Rechenzentrum und Hochladen in Azure: Das Hochladen der Daten im Rechenzentrum beginnt, sobald die operative Verarbeitung abgeschlossen und der Datenträger verbunden ist. Die Uploadzeit ist abhängig von der Art der Daten, der Größe und der Anzahl der Dateien.

## <a name="configure-and-connect"></a>Konfigurieren und Verbinden
 
### <a name="q-can-i-specify-the-number-of-data-box-disks-in-the-order"></a>Q. Kann ich im Auftrag die Anzahl von Data Box-Datenträgern angeben?
A.  Nein. Sie erhalten je nach Datenumfang und Verfügbarkeit einen oder mehrere Datenträger mit 8 TB Speicherplatz (maximal fünf Datenträger).  

### <a name="q-how-do-i-unlock-the-data-box-disks"></a>Q. Wie entsperre ich die Data Box-Datenträger? 
A.  Navigieren Sie im Azure-Portal zu Ihrem Auftrag für Data Box-Datenträger und dann zu **Gerätedetails**. Kopieren Sie den Hauptschlüssel. Laden Sie das Tool zum Entsperren von Data Box-Datenträgern für Ihr Betriebssystem über das Azure-Portal herunter, und extrahieren Sie es. Führen Sie das Tool auf dem Computer mit den Daten aus, die Sie auf die Datenträger kopieren möchten. Geben Sie den Hauptschlüssel an, um Ihre Datenträger zu entsperren. Alle Datenträger werden mit demselben Hauptschlüssel entsperrt. 

Eine Schritt-für-Schritt-Anleitung finden Sie unter den Informationen zum [Entsperren von Datenträgern auf einem Windows-Client](data-box-disk-deploy-set-up.md#unlock-disks-on-windows-client) bzw. [Entsperren von Datenträgern auf einem Linux-Client](data-box-disk-deploy-set-up.md#unlock-disks-on-linux-client).

### <a name="q-can-i-use-a-linux-host-computer-to-connect-and-copy-the-data-on-to-the-data-box-disks"></a>Q. Kann ich einen Linux-Hostcomputer verwenden, um eine Verbindung herzustellen und die Daten auf die Data Box-Datenträger zu kopieren?
A.  Ja. Sowohl der Linux- als auch der Windows-Client können verwendet werden, um eine Verbindung herzustellen und Daten in Data Box Disk zu kopieren. Weitere Informationen finden Sie in der Liste mit den [unterstützten Betriebssystemen](data-box-disk-system-requirements.md) für Ihren Hostcomputer.

### <a name="q-my-disks-are-dispatched-but-now-i-want-to-cancel-this-order-why-is-the-cancel-button-not-available"></a>Q. Meine Datenträger wurden versendet, aber ich möchte den Auftrag jetzt stornieren. Warum ist die Schaltfläche zum Stornieren nicht verfügbar?
A.  Sie können den Auftrag nur stornieren, wenn die Datenträger bestellt, aber noch nicht versendet wurden. Nachdem die Datenträger versendet wurden, ist eine Stornierung des Auftrags nicht mehr möglich. Sie haben aber die Möglichkeit, Ihre Datenträger gegen eine Gebühr zurückzugeben. 

### <a name="q-can-i-connect-multiple-data-box-disks-at-the-same-to-the-host-computer-to-transfer-data"></a>Q. Kann ich mehrere Data Box-Datenträger gleichzeitig an denselben Hostcomputer anschließen, um Daten zu übertragen?
A. Ja. Mehrere Data Box-Datenträger können an denselben Hostcomputer angeschlossen werden, um Daten zu übertragen, und es können mehrere Kopieraufträge parallel durchgeführt werden.

## <a name="track-status"></a>Nachverfolgen des Status

### <a name="q-how-do-i-track-the-disks-from-when-i-placed-the-order-to-shipping-the-disks-back"></a>Q. Wie kann ich für die Datenträger den Status ab der Erstellung des Auftrags bis zur Rücksendung der Datenträger nachverfolgen? 
A.  Sie können den Status des Auftrags für Data Box-Datenträger im Azure-Portal nachverfolgen. Beim Erstellen des Auftrags werden Sie auch aufgefordert, eine E-Mail-Adresse für Benachrichtigungen anzugeben. Wenn Sie eine E-Mail-Adresse angegeben haben, werden Sie per E-Mail über alle Statusänderungen des Auftrags informiert. [Hier finden Sie weitere Informationen zum Konfigurieren von E-Mail-Benachrichtigungen](data-box-portal-ui-admin.md#edit-notification-details).

### <a name="q-how-do-i-return-the-disks"></a>Q. Wie kann ich die Datenträger zurücksenden? 
A.  Microsoft fügt dem Versandpaket für die Data Box-Datenträger ein Versandetikett bei. Befestigen Sie das Etikett am Versandpaket, und geben Sie das sicher verschlossene Paket beim Paketdienst ab. Falls das Etikett beschädigt oder nicht mehr vorhanden ist, können Sie unter **Übersicht > Versandetikett herunterladen** ein neues Versandetikett erhalten.

### <a name="can-i-pick-up-my-data-box-disk-order-myself-can-i-return-the-disks-via-a-carrier-that-i-choose"></a>Kann ich meine Data Box Disk-Bestellung selbst abholen? Kann ich die Datenträger über einen Netzbetreiber meiner Wahl zurückgeben?
A. Ja. Microsoft bietet auch selbstverwalteten Versand in der Region „Gov (US)“ an. Beim Aufgeben der Data Box Disk-Bestellung können Sie die Option für selbstverwalteten Versand auswählen. Führen Sie die folgenden Schritte aus, um Ihre Data Box Disk-Bestellung abzuholen:
    
1. Nachdem Sie den Auftrag erteilt haben, wird er verarbeitet, und die Datenträger werden vorbereitet. Sie werden per E-Mail benachrichtigt, dass Ihre Bestellung zur Abholung bereit ist. 
2. Wenn die Bestellung bereit für die Abholung ist, rufen Sie im Azure-Portal Ihre Bestellung auf, und navigieren Sie zum Blatt **Übersicht**. 
3. Im Azure-Portal wird eine Benachrichtigung mit einem Code angezeigt. Senden Sie eine E-Mail an das [Azure Data Box Operations-Team](mailto:adbops@microsoft.com), und geben Sie den Code an. Das Team gibt den Standort bekannt und plant ein Abholdatum und eine Uhrzeit. Sie müssen das Team innerhalb von fünf Werktagen nach Erhalt der E-Mail-Benachrichtigung anrufen.

Führen Sie nach Abschluss der Datenkopie und der Überprüfung die folgenden Schritte aus, um den Datenträger zurückzugeben:

1. Trennen Sie die Datenträger vom Computer, nachdem die Überprüfung der Daten abgeschlossen ist. Entfernen Sie die Verbindungskabel.
2. Verpacken Sie alle Datenträger und die Verbindungskabel in Luftpolsterfolie, und legen Sie sie in das Versandpaket. Fehlendes Zubehör wird ggf. in Rechnung gestellt.

    - Verwenden Sie das Verpackungsmaterial aus der ursprünglichen Lieferung. Es empfiehlt sich, die Datenträger sorgfältig in Luftpolsterfolie zu verpacken.
    - Achten Sie auf einen festen Sitz der Teile im Karton, um ein Verrutschen innerhalb der Verpackung zu vermeiden.
3. Rufen Sie das **Übersichtsblatt** für Ihre Bestellung im Azure-Portal auf. Eine Benachrichtigung mit einem Code sollte angezeigt werden.
4. Verwenden Sie diesen Code, und senden Sie eine E-Mail an das [Azure Data Box Operations-Team](mailto:adbops@microsoft.com), und geben Sie den Code an. Sie erhalten die Informationen darüber, wo und wann Sie die Datenträger ablegen müssen.


## <a name="migrate-data"></a>Migrieren von Daten

### <a name="q-what-is-the-maximum-data-size-that-can-be-used-with-data-box-disks"></a>Q. Welche Datenmenge kann für Data Box-Datenträger maximal verwendet werden?  
A.  Bei der Lösung mit Data Box-Datenträgern können bis zu fünf Datenträger mit einer maximalen Kapazität von 35 TB genutzt werden. Die Größe der Datenträger beträgt jeweils 8 TB (7 TB nutzbar).

### <a name="q-what-are-the-maximum-block-blob-and-page-blob-sizes-supported-by-data-box-disks"></a>Q. Welche Größen für Blockblobs und Seitenblobs werden für Data Box-Datenträger maximal unterstützt? 
A.  Die maximale Größe richtet sich nach den Speichergrenzwerten für Azure. Die maximale Größe für Blockblobs beträgt ca. 4,768 TiB, und die maximale Größe für Seitenblobs beträgt 8 TiB. Weitere Informationen finden Sie unter [Skalierbarkeits- und Leistungsziele für Blob Storage](../storage/blobs/scalability-targets.md).

### <a name="q-what-is-the-data-transfer-speed-for-data-box-disks"></a>Q. Wie hoch ist die Datenübertragungsgeschwindigkeit für Data Box-Datenträger?
A. Beim Testen mit Datenträgern, die über USB 3.0 verbunden waren, betrug die Datenträgerleistung bis zu 430 MB/s. Die tatsächlichen Zahlen variieren abhängig von der Größe der verwendeten Datei. Bei kleineren Dateien ist die Leistung unter Umständen etwas reduziert.

### <a name="q-how-do-i-know-that-my-data-is-secure-during-transit"></a>Q. Woher weiß ich, dass meine Daten während der Übertragung geschützt sind? 
A.  Data Box-Datenträger werden per BitLocker-Verschlüsselung (AES-128 Bit) verschlüsselt, und der Hauptschlüssel ist nur über das Azure-Portal verfügbar. Melden Sie sich mit den Anmeldeinformationen für Ihr Konto am Azure-Portal an, um den Hauptschlüssel zu erhalten. Geben Sie diesen Hauptschlüssel an, wenn Sie das Tool zum Entsperren von Data Box-Datenträgern ausführen.

### <a name="q-how-do-i-copy-the-data-to-the-data-box-disks"></a>Q. Wie kopiere ich die Daten auf die Data Box-Datenträger? 
A.  Verwenden Sie ein SMB-Kopiertool, z. B. `Robocopy`, `Diskboss` oder auch Drag & Drop im Windows-Datei-Explorer, um Daten auf Datenträger zu kopieren.

### <a name="q-are-there-any-tips-to-speed-up-the-data-copy"></a>Q. Gibt es Tipps zur Beschleunigung des Datenkopiervorgangs?
A.  Sie können den Kopiervorgang wie folgt beschleunigen:

- Nutzen Sie mehrere Datenströme zum Kopieren von Daten. Verwenden Sie bei `Robocopy` beispielsweise die Option für Multithreading. Weitere Informationen zu den genauen Befehlen, die jeweils verwendet werden, finden Sie unter [Tutorial: Kopieren von Daten auf die Azure Data Box Disk und Durchführen der Überprüfung](data-box-disk-deploy-copy-data.md#copy-data-to-disks).
- Verwenden Sie mehrere Sitzungen.
- Stellen Sie sicher, dass sich die Daten lokal auf dem Computer befinden, an den die Datenträger angeschlossen sind, anstatt den Kopiervorgang über die Netzwerkfreigabe durchzuführen (um Beeinträchtigungen aufgrund der Netzwerkgeschwindigkeit zu vermeiden).
- Während des gesamten Kopiervorgangs müssen Sie USB 3.0 oder höher verwenden. Laden Sie das Tool [`USBView` ](/windows-hardware/drivers/debugger/usbview) herunter, und verwenden Sie es zum Identifizieren der USB-Controller und USB-Geräte, die an den Computer angeschlossen sind.
- Erstellen Sie Benchmarkwerte zur Leistung des Computers, der zum Kopieren der Daten verwendet wird. Laden Sie das [`Bluestop` `FIO`-Tool](https://ci.appveyor.com/project/axboe/fio) herunter, um Benchmarkwerte zur Leistung der Serverhardware zu erhalten. Wählen Sie den aktuellen x86- oder x64-Build und dann die Registerkarte **Artefakte** aus, und laden Sie die MSI herunter.

### <a name="q-how-to-speed-up-the-data-if-the-source-data-has-small-files-kbs-or-few-mbs"></a>Q. Wie kann ich für die Daten eine Beschleunigung erreichen, wenn die Quelldaten kleine Dateien (KB oder nur wenige MB) enthalten?
A.  Sie können den Kopiervorgang wie folgt beschleunigen:

- Erstellen Sie im schnellen Speicher eine lokale VHDx oder eine leere VHD auf der HDD/SSD (langsamer).
- Stellen Sie die Daten auf einer VM bereit.
- Kopieren Sie Dateien auf den Datenträger der VM.

### <a name="q-can-i-use-multiple-storage-accounts-with-data-box-disks"></a>Q. Kann ich mehrere Speicherkonten mit Data Box-Datenträgern verwenden?
A.  Nein. Für Data Box-Datenträger wird derzeit nur ein Speicherkonto (allgemein oder klassisch) unterstützt. Es werden sowohl „heiße“ als auch „kalte“ Blobs unterstützt. Derzeit werden nur die Speicherkonten in den USA, Westeuropa und Nordeuropa unterstützt, die Teil der öffentlichen Azure-Cloud sind.

### <a name="q-what-is-the-toolset-available-for-my-data-with-data-box-disks"></a>Q. Was umfasst das Toolset, das für meine Daten für Data Box-Datenträger verfügbar ist?
A. Das für Data Box Disk verfügbare Toolset enthält drei Tools:
 - **Data Box Disk-Tool zum Entsperren**: Verwenden Sie dieses Tool, um die verschlüsselten Datenträger zu entsperren, die von Microsoft ausgeliefert werden. Beim Entsperren der Datenträger mit dem Tool müssen Sie einen Hauptschlüssel angeben, der in der Data Box Disk-Bestellung im Azure-Portal enthalten ist. 
 - **Data Box Disk-Überprüfungstool**: Verwenden Sie dieses Tool, um Größe, Format und Blobnamen gemäß den Azure-Benennungskonventionen zu überprüfen. Außerdem werden damit Prüfsummen für die kopierten Daten generiert, anhand derer dann die in Azure hochgeladenen Daten überprüft werden.
 - **Data Box Disk-Tool zum Aufteilen/Kopieren**: Verwenden Sie dieses Tool, wenn Sie mehrere Datenträger nutzen und über ein umfangreiches Dataset verfügen, das aufgeteilt und auf alle Datenträger kopiert werden muss. Dieses Tool ist derzeit für Windows verfügbar. Dieses Tool wird für verwaltete Datenträger nicht unterstützt. Weil dieses Tool die Daten während des Kopiervorgangs überprüft, können Sie den Überprüfungsschritt bei Verwendung des Tools überspringen.

Das Toolset steht sowohl für Windows als auch für Linux zur Verfügung. Sie können das Toolset hier herunterladen:
- [Herunterladen des Data Box Disk-Toolsets für Windows](https://aka.ms/databoxdisktoolswin) 
- [Herunterladen des Data Box Disk-Toolsets für Linux](https://aka.ms/databoxdisktoolslinux)
 
### <a name="q-can-i-use-data-box-disk-to-transfer-data-to-azure-files-and-then-use-the-data-with-azure-file-sync"></a>Q. Kann ich mit Data Box Disk Daten an Azure Files übertragen und die Daten anschließend mit der Azure-Dateisynchronisierung verwenden? 
A. Azure Files wird für Data Box Disk unterstützt, funktioniert jedoch nicht gut mit der Azure-Dateisynchronisierung. Metadaten werden nicht beibehalten, wenn die Dateidaten mit der Azure-Dateisynchronisierung verwendet werden.


## <a name="verify-and-upload"></a>Überprüfen und Hochladen

### <a name="q-how-soon-can-i-access-my-data-in-azure-once-ive-shipped-the-disks-back"></a>Q. Wie schnell kann ich in Azure auf meine Daten zugreifen, nachdem ich die Datenträger zurückgesendet habe? 
A.  Nachdem der Auftragsstatus für das Kopieren der Daten als „Abgeschlossen“ angezeigt wird, sollten Sie sofort auf Ihre Daten zugreifen können.

### <a name="q-where-is-my-data-located-in-azure-after-the-upload"></a>Q. Wo befinden sich meine Daten nach dem Upload in Azure?
A.  Wenn Sie die Daten in die Ordner *BlockBlob* und *PageBlob* auf Ihrem Datenträger kopieren, wird unter dem Azure-Speicherkonto für jeden Unterordner von *BlockBlob* und *PageBlob* ein Container erstellt. Falls Sie die Dateien direkt in die Ordner *BlockBlob* und *PageBlob* kopiert haben, befinden sie sich im Standardcontainer *$root* unter dem Azure Storage-Konto. Wenn Sie die Daten in einen Order im Ordner *AzureFile* kopieren, wird eine Dateifreigabe erstellt.

### <a name="q-i-just-noticed-that-i-didnt-follow-the-azure-naming-requirements-for-my-containers-will-my-data-fail-to-upload-to-azure"></a>Q. Ich habe gerade festgestellt, dass ich mich nicht an die Azure-Benennungsanforderungen für meine Container gehalten habe. Können meine Daten deshalb nicht in Azure hochgeladen werden?
A. Alle Großbuchstaben in Ihren Containernamen werden automatisch in Kleinbuchstaben konvertiert. Wenn die Namen nicht auf andere Weise konform sind – beispielsweise Sonderzeichen oder andere Sprachen enthalten – tritt beim Hochladen ein Fehler auf. Weitere Informationen finden Sie unter den [Benennungskonventionen für Azure](data-box-disk-limits.md#azure-block-blob-page-blob-and-file-naming-conventions).

### <a name="q-how-do-i-verify-the-data-i-copied-onto-multiple-data-box-disks"></a>Q. Wie kann ich die Daten überprüfen, die auf mehrere Data Box-Datenträger kopiert wurden?
A.  Nach Abschluss des Kopiervorgangs für die Daten können Sie `DataBoxDiskValidation.cmd` aus dem Ordner *DataBoxDiskImport* ausführen, um Prüfsummen für die Validierung zu generieren. Falls Sie über mehrere Datenträger verfügen, müssen Sie pro Datenträger ein Befehlsfenster öffnen und diesen Befehl ausführen. Beachten Sie, dass dieser Vorgang je nach Größe Ihrer Daten ggf. lange dauern kann (mehrere Stunden).

### <a name="q-what-happens-to-my-data-after-i-have-returned-the-disks"></a>Q. Was passiert mit meinen Daten, nachdem ich die Datenträger zurückgesendet habe?
A.  Nach Abschluss des Kopiervorgangs für die Daten nach Azure werden die Daten gemäß den NIST-Richtlinien (SP 800-88 Revision 1) auf sichere Weise gelöscht.  

### <a name="q-how-is-my-data-protected-during-transit"></a>Q. Wie sind meine Daten während des Versands geschützt? 
A.  Die Data Box-Datenträger werden mit der Microsoft BitLocker-Verschlüsselung AES-128 verschlüsselt, und zum Entsperren aller Datenträger und Zugriffsdaten ist ein einziger Hauptschlüssel erforderlich.

### <a name="q-do-i-need-to-rerun-checksum-validation-if-i-add-more-data-to-the-data-box-disks"></a>Q. Muss ich die Überprüfung der Prüfsumme erneut durchführen, wenn ich den Data Box-Datenträgern weitere Daten hinzufüge?
A. Ja. Wenn Sie sich für die Validierung Ihrer Daten entscheiden (empfohlen), müssen Sie die Validierung erneut durchführen, falls Sie den Datenträgern weitere Daten hinzufügen.

### <a name="q-i-used-all-my-disks-to-transfer-data-and-need-to-order-more-disks-is-there-a-way-to-quickly-place-the-order"></a>Q. Ich habe alle Datenträger zum Übertragen von Daten verwendet und muss weitere Datenträger anfordern. Gibt es eine Möglichkeit, die Bestellung ohne viel Zeitaufwand aufzugeben?
A. Sie können Ihren vorherigen Auftrag klonen. Beim Klonen wird derselbe Auftrag noch einmal erstellt, und Sie müssen nur noch die Auftragsdetails angeben, ohne Adresse, Kontaktinformationen und Benachrichtigungsdetails erneut eingeben zu müssen.

### <a name="q-i-copied-data-to-the-manageddisk-folder-i-dont-see-any-managed-disks-with-the-resource-group-specified-for-managed-disks-was-my-data-uploaded-to-azure-how-can-i-locate-it"></a>Q. Ich habe Daten in den Ordner „ManagedDisk“ kopiert. Ich kann keine verwalteten Datenträger mit der für verwaltete Datenträger angegebenen Ressourcengruppe sehen. Wurden meine Daten in Azure hochgeladen? Wie kann ich sie finden?
A. Ja. Ihre Daten wurden in Azure hochgeladen. Wenn aber keine verwalteten Datenträger mit den angegebenen Ressourcengruppen angezeigt werden, waren die Daten wahrscheinlich nicht gültig. Wenn Seitenblobs, Blockblobs, Azure-Dateien oder verwaltete Datenträger nicht gültig sind, werden sie in den folgenden Ordnern angezeigt:
 - Seitenblobs werden in einem Blockblobcontainer gespeichert, dessen Name mit *databoxdisk-invalid-pb-* beginnt.
 - Azure-Dateien werden in einem Blockblobcontainer gespeichert, dessen Name mit *databoxdisk-invalid-af-* beginnt.
 - Verwaltete Datenträger werden in einem Blockblobcontainer gespeichert, dessen Name mit *databoxdisk-invalid-md-* beginnt.

## <a name="next-steps"></a>Nächste Schritte

- Sehen Sie sich die [Data Box Disk-Systemanforderungen](data-box-disk-system-requirements.md) an.
- Machen Sie sich mit den [Einschränkungen für Data Box-Datenträger](data-box-disk-limits.md) vertraut.
- Informieren Sie sich über die schnelle Bereitstellung von [Azure Data Box-Datenträgern](data-box-disk-quickstart-portal.md) im Azure-Portal.