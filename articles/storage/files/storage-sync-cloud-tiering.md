---
title: Grundlegendes zum Cloudtiering der Azure-Dateisynchronisierung | Microsoft-Dokumentation
description: Informieren Sie sich über Cloudtiering, ein optionales Feature der Azure-Dateisynchronisierung. Dateien, auf die häufig zugegriffen wird, werden lokal auf dem Server gespeichert. andere werden in Azure Files ausgelagert.
author: roygara
ms.service: storage
ms.topic: conceptual
ms.date: 06/15/2020
ms.author: rogarana
ms.subservice: files
ms.openlocfilehash: e5aafaa02f503582bd0050f8a6389d78b52eaa76
ms.sourcegitcommit: 541bb46e38ce21829a056da880c1619954678586
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/11/2020
ms.locfileid: "91939152"
---
# <a name="cloud-tiering-overview"></a>Übersicht über Cloudtiering
Cloudtiering ist ein optionales Feature der Azure-Dateisynchronisierung, bei dem häufig verwendete Dateien lokal auf dem Server zwischengespeichert werden, während alle anderen Dateien gemäß Richtlinieneinstellungen in Azure Files ausgelagert werden. Beim Tiering einer Datei ersetzt der Azure-Dateisynchronisierungs-Dateisystemfilter (StorageSync.sys) die Datei lokal durch einen Zeiger oder Analysepunkt. Der Analysepunkt stellt eine URL zur Datei in Azure Files dar. Eine per Tiering ausgelagerte Datei weist sowohl das offline-Attribut als auch das in NTFS festgelegte FILE_ATTRIBUTE_RECALL_ON_DATA_ACCESS-Attribut auf, sodass Drittanwendungen Tieringdateien sicher identifizieren können.
 
Wenn ein Benutzer eine Tieringdatei öffnet, ruft die Azure-Dateisynchronisierung nahtlos die Dateidaten aus Azure Files ab, ohne dass der Benutzer wissen muss, dass die Datei in Azure gespeichert ist. 
 
 > [!Important]  
 > Das Cloudtiering wird auf dem Systemvolume von Windows nicht unterstützt.
    
 > [!Important]  
 > Um Dateien mit Tiering abzurufen, muss die Netzwerkbandbreite mindestens 1 Mbit/s betragen. Wenn die Netzwerkbandbreite kleiner als 1 Mbit/s ist, misslingt ggf. das Abrufen von Dateien mit einem Timeoutfehler.

## <a name="cloud-tiering-faq"></a>Cloudtiering – häufig gestellte Fragen

<a id="afs-cloud-tiering"></a>
### <a name="how-does-cloud-tiering-work"></a>Wie funktioniert Cloudtiering?
Der Systemfilter der Azure-Dateisynchronisierung erstellt ein „Wärmebild“ Ihres Namespace auf jedem Serverendpunkt. Er überwacht Zugriffe (Lese- und Schreibvorgänge) im Lauf der Zeit und weist dann jeder Datei basierend auf der Häufigkeit und Aktualität des Zugriffs eine Wärmebewertung zu. Eine häufig verwendete Datei, die vor Kurzem geöffnet wurde, wird als heiß angesehen, während eine Datei, die kaum verwendet wird und auf die seit einiger Zeit nicht zugegriffen wurde, als kalt angesehen wird. Wenn das Dateivolume auf einem Server den festgelegten Schwellenwert für freien Speicherplatz auf dem Volume überschreitet, werden die Dateien mit der niedrigsten Wärmebewertung nach Azure Files ausgelagert, bis der Anteil an freiem Speicherplatz wieder erfüllt ist.

Sie können zusätzlich eine Datumsrichtlinie für jeden Serverendpunkt angeben, durch die unabhängig von der verfügbaren lokalen Speicherkapazität alle Dateien per Tiering ausgelagert werden, auf die innerhalb einer festgelegten Anzahl von Tagen nicht zugegriffen wurde. Dies empfiehlt sich, um vorsorglich Speicherplatz auf dem lokalen Datenträger freizugeben, wenn Sie wissen, dass Dateien auf dem Serverendpunkt nicht über einen gewissen Zeitraum hinaus aufbewahrt werden müssen. Dadurch wird nützliche lokale Festplattenkapazität für andere Endpunkte auf demselben Volume freigegeben, sodass mehr Dateien zwischengespeichert werden können.

Das Wärmebild für das Cloudtiering ist im Wesentlichen eine sortierte Liste aller Dateien, die synchronisiert werden und sich an einem Speicherort mit aktiviertem Cloudtiering befinden. Zur Bestimmung der relativen Position einer einzelnen Datei im Wärmebild legt das System den höchsten der folgenden Zeitstempel in der angegebenen Reihenfolge zugrunde: MAX(Zeitpunkt des letzten Zugriffs, Zeitpunkt der letzten Änderung, Erstellungszeitpunkt). In der Regel wird der Zeitpunkt des letzten Zugriffs nachverfolgt und verfügbar gemacht. Wenn jedoch ein neuer Serverendpunkt bei aktiviertem Cloudtiering erstellt wird, ist zunächst nicht genügend Zeit zum Überwachen des Dateizugriffs vergangen. Wenn der Zeitpunkt des letzten Zugriffs nicht verfügbar ist, wird der Zeitpunkt der letzten Änderung verwendet, um die relative Position im Wärmebild zu ermitteln. Der gleiche Fallback gilt für die Datumsrichtlinie. Wenn der Zeitpunkt des letzten Zugriffs nicht verfügbar ist, zieht die Datumsrichtlinie den Zeitpunkt der letzten Änderung heran. Sollte dieser nicht verfügbar sein, wird auf die Erstellungszeit einer Datei zurückgegriffen. Im Laufe der Zeit verfolgt das System mehr und mehr Dateizugriffsanforderungen und verwendet zunehmend die selbst verfolgte Zeit des letzten Zugriffs.

Das Cloudtiering ist nicht von der NTFS-Funktion für die Überwachung des Zeitpunkts des letzten Zugriffs abhängig. Diese NTFS-Funktion ist standardmäßig deaktiviert. Um die Leistung nicht zu beeinträchtigen, wird empfohlen, diese Funktion nicht manuell zu aktivieren. Das Cloudtiering verfolgt den Zeitpunkt des letzten Zugriffs separat und sehr effizient.

<a id="tiering-minimum-file-size"></a>
### <a name="what-is-the-minimum-file-size-for-a-file-to-tier"></a>Welche Mindestdateigröße gilt für Dateien, für die ein Tiering durchgeführt werden soll?

Die Mindestdateigröße für Dateien, für die ein Tiering durchgeführt werden soll, basiert ab den Agent-Versionen 9.x auf der Größe des Dateisystemclusters. Die Mindestdateigröße für Cloudtiering wird durch Verdopplung der Clustergröße berechnet und muss mindestens 8 KB betragen. In der folgenden Tabelle sind die Mindestdateigrößen auf Grundlage der Volumeclustergröße aufgeführt, für die ein Tiering möglich ist:

|Volumeclustergröße (Byte) |Mindestdateigröße für Tiering  |
|----------------------------|---------|
|4 KB oder kleiner (4096)      | 8 KB    |
|8 KB (8192)                 | 16 KB   |
|16 KB (16384)               | 32 KB   |
|32 KB (32768)               | 64 KB   |
|64 KB (65536)    | 128 KB  |

Aktuell werden Clustergrößen bis zu 64 KB unterstützt, für größere Größen ist Cloudtiering jedoch nicht verfügbar.

Alle von Windows verwendeten Dateisysteme organisieren Ihre Festplatte auf Grundlage der Clustergröße (auch als Größe der Zuordnungseinheiten bezeichnet). Die Clustergröße stellt die kleinste Menge an Speicherplatz dar, die zum Speichern einer Datei verwendet werden kann. Wenn Dateigrößen kein gerades Vielfaches der Clustergröße ergeben, muss zum Speichern der Datei zusätzlicher Speicherplatz verwendet werden – bis zum nächsten Vielfachen der Clustergröße.

Die Azure-Dateisynchronisierung wird auf NTFS-Volumes mit Windows Server 2012 R2 und höher unterstützt. In der folgenden Tabelle sind die Standardclustergrößen für das Erstellen eines neuen NTFS-Volumes aufgeführt. 

|Volumegröße    |Windows Server 2012R2 und höher |
|---------------|---------------|
|7 MB–16 TB   | 4 KB          |
|16 TB–32 TB   | 8 KB          |
|32 TB–64 TB   | 16 KB         |
|64 TB–128 TB  | 32 KB         |
|128 TB–256 TB | 64 KB         |
|> 256 TB       | Nicht unterstützt |

Möglicherweise haben Sie das Volume bei der Erstellung manuell mit einer anderen Clustergröße formatiert. Wenn das Volume von einer älteren Version von Windows stammt, können die Standardclustergrößen ebenfalls abweichen. [In diesem Artikel finden Sie weitere Informationen zu den Standardclustergrößen.](https://support.microsoft.com/help/140365/default-cluster-size-for-ntfs-fat-and-exfat) Auch wenn Sie eine Clustergröße unter 4 KB auswählen, gilt weiterhin der Grenzwert von 8 KB als kleinste Dateigröße, für die ein Tiering durchgeführt werden kann. (Auch wenn technisch gesehen das Doppelte der Clustergröße weniger als 8 KB wäre.)

Der Grund für diesen absoluten Minimalwert findet sich in der Art und Weise, in der NTFS sehr kleine Dateien speichert, also mit Dateigrößen von 1 bis 4 KB. Je nach den Parametern des Volumes ist es möglich, dass kleine Dateien überhaupt nicht in einem Cluster oder auf einem Datenträger gespeichert werden. Möglicherweise ist es effizienter, solche Dateien direkt im Datensatz der Masterdateitabelle (Master File Table, MFT) des Volumes zu speichern. Der Analysepunkt für das Cloudtiering wird immer auf dem Datenträger gespeichert und belegt genau einen Cluster. Bei so kleinen Dateien spart ein Tiering möglicherweise keinen Speicherplatz. In extremen Fällen könnte es sogar vorkommen, dass ein aktiviertes Tiering zu einer höheren Speicherplatzbelegung führt. Zum Schutz vor solchen Situationen ist die kleinste Dateigröße, für die ein Cloudtiering ausgeführt werden kann, 8 KB in einem Cluster mit 4 KB oder weniger.

<a id="afs-volume-free-space"></a>
### <a name="how-does-the-volume-free-space-tiering-policy-work"></a>Wie funktioniert die Richtlinie für freien Speicherplatz auf dem Volume?
Der freie Speicherplatz auf dem Volume ist die Menge an freiem Speicherplatz auf dem Volume, auf dem sich ein Serverendpunkt befindet, die Sie reservieren möchten. Wenn beispielsweise der freie Speicherplatz auf einem Volume, das einen Serverendpunkt aufweist, auf 20 % festgelegt ist, werden bis zu 80 % des Speicherplatzes auf dem Volume von den zuletzt verwendeten Dateien belegt, und etwaige verbleibende Dateien, die nicht in den Speicherplatz passen, werden in Azure ausgelagert. Der freier Speicherplatz auf dem Volume gilt auf Volumeebene und nicht auf Ebene der einzelnen Verzeichnisse oder Synchronisierungsgruppen. 

<a id="volume-free-space-fastdr"></a>
### <a name="how-does-the-volume-free-space-tiering-policy-work-with-regards-to-new-server-endpoints"></a>Wie funktioniert die Auslagerungsrichtlinie für den freien Speicherplatz auf dem Volume im Hinblick auf neue Serverendpunkte?
Wenn ein Serverendpunkt neu bereitgestellt und mit einer Azure-Dateifreigabe verbunden wird, ruft der Server zuerst den Namespace ab und dann die eigentlichen Dateien, bis der Schwellenwert für freien Speicherplatz auf dem Volume erreicht ist. Dieser Vorgang wird auch als schnelle Notfallwiederherstellung oder schnelle Namespacewiederherstellung bezeichnet.

<a id="afs-effective-vfs"></a>
### <a name="how-is-volume-free-space-interpreted-when-i-have-multiple-server-endpoints-on-a-volume"></a>Wie wird freier Speicherplatz auf dem Volume interpretiert, wenn ich über mehrere Serverendpunkte auf einem Volume verfüge?
Wenn auf einem Volume mehrere Serverendpunkte vorhanden sind, gilt als freier Speicherplatz auf dem Volume der größte freie Speicherplatz auf dem Volume, der für alle Serverendpunkte auf diesem Volume angegeben wurde. Die Dateien werden entsprechend ihrer Verwendungsmuster, aber unabhängig davon, zu welchem Serverendpunkt sie gehören, eingestuft. Wenn Sie z.B. auf einem Volume über die beiden Serverendpunkte Endpoint1 und Endpoint2 verfügen, wobei Endpoint1 über einen Schwellenwert für freien Speicherplatz auf dem Volume von 25 % und Endpoint2 von 50 % verfügt, beträgt der Schwellenwert für freien Speicherplatz das auf dem Volume für beide Serverendpunkte 50 %. 

<a id="date-tiering-policy"></a>
### <a name="how-does-the-date-tiering-policy-work-in-conjunction-with-the-volume-free-space-tiering-policy"></a>Wie funktioniert die datumsbasierte Tieringrichtlinie in Verbindung mit der Tieringrichtlinie für freien Speicherplatz auf dem Volume? 
Wenn Sie Cloudtiering auf einem Serverendpunkt aktivieren, legen Sie eine Richtlinie für freien Speicherplatz auf dem Volume fest. Diese Richtlinie hat immer Vorrang vor allen anderen Richtlinien, einschließlich der Datumsrichtlinie. Optional können Sie eine Datumsrichtlinie für jeden Serverendpunkt auf diesem Volume aktivieren. Diese Richtlinie sorgt dafür, dass nur Dateien lokal aufbewahrt werden, auf die in der in dieser Richtlinie beschriebenen Zeitspanne zugegriffen wird (d. h., es erfolgen Lese- oder Schreibzugriffe auf sie). Dateien, auf die nicht in der angegebenen Anzahl von Tagen zugegriffen wird, werden ausgelagert. 

Beim Cloudtiering wird der Zeitpunkt des letzten Zugriffs verwendet, um festzulegen, welche Dateien ausgelagert werden sollen. Der Cloudtiering-Filtertreiber („storagesync.sys“) verfolgt den Zeitpunkt des letzten Zugriffs und protokolliert die Informationen im Cloudtiering-Wärmespeicher. Sie können den Speicherzustand abrufen und in einer CSV-Datei speichern, indem Sie ein server-local-PowerShell-Cmdlet verwenden.

```powershell
# There is a single heat store for files on a volume / server endpoint / individual file.
# The heat store can get very large. If you only need to retrieve the "coolest" number of items, use -Limit and a number

# Import the PS module:
Import-Module '<SyncAgentInstallPath>\StorageSync.Management.ServerCmdlets.dll'

# VOLUME FREE SPACE: To get the order in which files will be tiered using the volume free space policy:
Get-StorageSyncHeatStoreInformation -VolumePath '<DriveLetter>:\' -ReportDirectoryPath '<FolderPathToStoreResultCSV>' -IndexName LastAccessTimeWithSyncAndTieringOrder

# DATE POLICY: To get the order in which files will be tiered using the date policy:
Get-StorageSyncHeatStoreInformation -VolumePath '<DriveLetter>:\' -ReportDirectoryPath '<FolderPathToStoreResultCSV>' -IndexName LastAccessTimeWithSyncAndTieringOrderV2

# Find the heat store information for a particular file:
Get-StorageSyncHeatStoreInformation -FilePath '<PathToSpecificFile>'
```

> [!IMPORTANT]
> Der Zeitstempel des letzten Zugriffs ist keine Eigenschaft, die von NTFS nachverfolgt wird, und wird daher standardmäßig nicht im Datei-Explorer angezeigt. Verwenden Sie nicht den Zeitstempel der letzten Änderung einer Datei, um zu überprüfen, ob die Datumsrichtlinie erwartungsgemäß funktioniert. Mit diesem Zeitstempel werden nur Schreibvorgänge nachverfolgt, keine Lesevorgänge. Verwenden Sie für diese Auswertung das dargestellte Cmdlet, um den Zeitstempel des letzten Zugriffs zu erhalten.

> [!WARNING]
> Aktivieren Sie nicht das NTFS-Feature für die Überwachung des Zeitstempels des letzten Zugriffs für Dateien und Ordner. Dieses Feature ist standardmäßig deaktiviert, da es starke Auswirkungen auf die Leistung hat. Die Azure-Dateisynchronisierung verfolgt den Zeitpunkt des letzten Zugriffs automatisch und sehr effizient nach und nutzt dieses NTFS-Feature nicht.

Denken Sie daran, dass die Richtlinie für freien Speicherplatz auf dem Volume immer Vorrang hat. Ist auf dem Volume nicht genügend freier Speicherplatz für die gemäß der Datumsrichtlinie zu speichernden Dateien verfügbar, lagert die Azure-Dateisynchronisierung die ältesten Dateien weiter per Tiering aus, bis der erforderliche Prozentsatz an freiem Speicherplatz auf dem Volume erreicht ist.

Angenommen, Sie haben eine datumsbasierte Tieringrichtlinie von 60 Tagen und eine Richtlinie für freien Speicherplatz auf dem Volume von 20 %. Wenn der freie Speicherplatz auf dem Volume nach dem Anwenden der Datumsrichtlinie unter 20 % liegt, setzt die Richtlinie für freien Speicherplatz auf dem Volume die Datumsrichtlinie außer Kraft. Da dadurch mehr Dateien ausgelagert werden, kann die auf dem Server beibehaltene Datenmenge von Daten für 60 Tage auf Daten für 45 Tage reduziert werden. Umgekehrt erzwingt diese Richtlinie auch dann das Tiering von Dateien außerhalb des Zeitbereichs, wenn der Schwellenwert für den freien Speicherplatz nicht erreicht wurde. Eine Datei, die 61 Tage alt ist, wird also selbst dann ausgelagert, wenn das Volume leer ist.

<a id="volume-free-space-guidelines"></a>
### <a name="how-do-i-determine-the-appropriate-amount-of-volume-free-space"></a>Wie lege ich eine geeignete Menge an freiem Speicherplatz auf dem Volume fest?
Die Menge der Daten, die lokal gespeichert werden sollten, wird durch einige Faktoren bestimmt: Ihre Bandbreite, das Zugriffsmuster Ihres Datasets und Ihr Budget. Wenn Sie über eine Verbindung mit geringer Bandbreite verfügen, sollten Sie möglicherweise einen größeren Teil Ihrer Daten lokal speichern, um sicherzustellen, dass nur minimale Verzögerungen für Ihre Benutzer auftreten. Andernfalls können Sie als Baseline die Änderungsrate in einem bestimmten Zeitraum verwenden. Wenn Ihnen z.B. bekannt ist, dass sich jeden Monat ungefähr 10 % Ihres 1-TB-Datasets ändern oder auf dieses aktiv zugegriffen wird, sollten Sie 100 GB lokal speichern, damit Sie nicht zu häufig Dateien abrufen. Wenn das Volume 2 TB groß ist, sollten Sie 5 % (100 GB) lokal speichern, d.h., die verbleibenden 95 % sind der Anteil des freien Speicherplatzes auf dem Volume. Wir empfehlen jedoch, einen Puffer hinzuzufügen, um Zeiträume höherer Änderungsraten zu berücksichtigen. Anders ausgedrückt, sollten Sie mit einem niedrigeren Anteil des freien Speicherplatzes auf dem Volume beginnen und diesen ggf. später noch einmal anpassen. 

Wenn Sie mehr Daten lokal speichern, fallen geringere Kosten für ausgehenden Datenverkehr an, da weniger Dateien aus Azure zurückgerufen werden, Sie müssen jedoch eine größere Menge von lokalem Speicher verwalten, was ebenfalls mit Kosten verbunden ist. Nachdem Sie eine Instanz der Azure-Dateisynchronisierung bereitgestellt haben, können Sie über den ausgehenden Datenverkehr Ihres Speicherkontos abschätzen, ob Ihre Einstellungen für den freien Speicherplatz auf dem Volume für Ihre Zwecke geeignet sind. Ausgehend davon, dass das Speicherkonto nur Ihren Cloudendpunkt für die Azure-Dateisynchronisierung (d.h. Ihre Synchronisierungsfreigabe) enthält, bedeutet ein hoher ausgehender Datenverkehr, dass viele Dateien aus der Cloud zurückgerufen werden. In diesem Fall sollten Sie erwägen, den lokalen Cache zu erhöhen.

<a id="how-long-until-my-files-tier"></a>
### <a name="ive-added-a-new-server-endpoint-how-long-until-my-files-on-this-server-tier"></a>Ich habe einen neuen Serverendpunkt hinzugefügt. Wann werden meine Dateien auf diesem Server per Tiering ausgelagert?

Einmal pro Stunde wird ermittelt, ob gemäß den festgelegten Richtlinien ein Tiering von Dateien erforderlich ist. Beim Erstellen eines neuen Serverendpunkts können zwei Situationen auftreten:

1. Wenn Sie einen neuen Serverendpunkt hinzufügen, sind häufig an diesem Serverspeicherort Dateien vorhanden. Diese müssen zuerst hochgeladen werden, bevor das Cloudtiering erfolgen kann. Die Richtlinie für freien Volumespeicherplatz wird erst wirksam, wenn der erste Upload aller Dateien abgeschlossen ist. Die optionale Datumsrichtlinie wird jedoch auf einzelne Dateien angewendet, sobald eine Datei hochgeladen wurde. Hierbei gilt ebenfalls ein Intervall von einer Stunde. 
2. Wenn Sie einen neuen Serverendpunkt hinzufügen, verbinden Sie möglicherweise einen leeren Serverspeicherort mit einer Azure-Dateifreigabe, auf der sich Ihre Daten befinden. Dabei kann es sich um einen zweiten Server oder die Folgen einer Notfallwiederherstellung handeln. Wenn Sie den Namespace herunterladen und den Inhalt beim ersten Download auf Ihren Server abrufen, werden die Dateien nach dem Abschalten des Namespace auf der Grundlage des Zeitstempels der letzten Änderung abgerufen. Es werden nur so viele Dateien abgerufen, wie es die Richtlinie für freien Volumespeicherplatz und die optionale Datumsrichtlinie erlauben.

<a id="is-my-file-tiered"></a>
### <a name="how-can-i-tell-whether-a-file-has-been-tiered"></a>Woran erkenne ich, ob eine Datei per Tiering ausgelagert wurde?
Es gibt mehrere Möglichkeiten, um zu überprüfen, ob eine Datei per Tiering auf Ihre Azure-Dateifreigabe ausgelagert wurde:
    
   *  **Überprüfen Sie die Dateiattribute in der Datei.**
     Klicken Sie mit der rechten Maustaste auf eine Datei, navigieren Sie zu **Details**, und scrollen Sie dann nach unten zur Eigenschaft **Attribute**. In einer per Tiering ausgelagerten Datei sind folgende Attribute festgelegt:     
        
        | Attributbuchstabe | attribute | Definition |
        |:----------------:|-----------|------------|
        | Ein | Archivieren | Gibt an, dass die Datei durch Sicherungssoftware gesichert werden sollte. Dieses Attribut ist immer festgelegt, unabhängig davon, ob die Datei per Tiering ausgelagert oder vollständig auf dem Datenträger gespeichert wurde. |
        | P | Sparsedatei | Gibt an, dass die Datei eine „Sparsedatei“ (platzsparende Datei) ist. Eine platzsparende Datei ist ein spezieller Typ von Datei, der von NTFS zur effizienten Verwendung bereitgestellt wird, wenn die Datei im Datenträgerdatenstrom nahezu leer ist. Platzsparende Dateien werden von der Azure-Dateisynchronisierung genutzt, da eine Datei entweder vollständig per Tiering ausgelagert oder teilweise zurückgerufen wurde. Bei einer vollständig per Tiering ausgelagerten Datei wird der Dateidatenstrom in der Cloud gespeichert. Bei einer teilweise zurückgerufenen Datei befindet sich dieser Teil der Datei bereits auf dem Datenträger. Wenn eine Datei vollständig auf die Festplatte zurückgerufen wird, konvertiert die Azure-Dateisynchronisierung diese platzsparende Datei in eine normale Datei. Dieses Attribut ist nur unter Windows Server 2016 und früher festgelegt.|
        | M | Abruf beim Datenzugriff | Gibt an, dass die Daten der Datei nicht vollständig im lokalen Speicher vorhanden sind. Das Lesen der Datei bewirkt, dass mindestens ein Teil des Dateiinhalts aus einer Azure-Dateifreigabe abgerufen wird, mit der der Serverendpunkt verbunden ist. Dieses Attribut ist nur unter Windows Server 2019 festgelegt. |
        | L | Analysepunkt | Gibt an, dass die Datei über einen Analysepunkt verfügt. Ein Analysepunkt ist ein spezieller Zeiger für die Verwendung durch einen Dateisystemfilter. Die Azure-Dateisynchronisierung verwendet Analysepunkte, um den Speicherort für den Azure-Dateisynchronisierungs-Dateisystemfilter (StorageSync.sys) zu definieren, an dem die Datei in der Cloud gespeichert ist. Dies erleichtert den nahtlosen Zugriff. Benutzer müssen nicht wissen, dass die Azure-Dateisynchronisierung verwendet wird oder wie der Zugriff auf die Datei in Ihrer Azure-Dateifreigabe möglich ist. Wenn eine Datei vollständig zurückgerufen wird, entfernt die Azure-Dateisynchronisierung den Analysepunkt aus der Datei. |
        | O | Offline | Gibt an, dass ein Teil des Dateiinhalts oder der gesamte Dateiinhalt nicht auf dem Datenträger gespeichert ist. Wenn eine Datei vollständig zurückgerufen wird, entfernt die Azure-Dateisynchronisierung dieses Attribut. |

        ![Dialogfeld „Eigenschaften“ einer Datei mit ausgewählter Registerkarte „Details“](media/storage-files-faq/azure-file-sync-file-attributes.png)
        
        Sie können die Attribute für alle Dateien in einem Ordner anzeigen, indem das Feld **Attribute** der Tabellenanzeige des Datei-Explorers hinzugefügt wird. Klicken Sie hierzu mit der rechten Maustaste auf eine vorhandene Spalte (z.B. **Größe**). Wählen Sie anschließend **Mehr** und dann in der Dropdownliste **Attribute** aus.
        
   * **Verwenden Sie `fsutil`, um nach Analysepunkten in einer Datei zu suchen.**
       Wie für die vorherige Option beschrieben, verfügt eine per Tiering ausgelagerte Datei immer über einen festgelegten Analysepunkt. Ein Analysepunkt ist ein spezieller Zeiger für den Azure-Dateisynchronisierungs-Dateisystemfilter (StorageSync.sys). Führen Sie das Hilfsprogramm `fsutil` an einer Eingabeaufforderung mit erhöhten Rechten oder in einem PowerShell-Fenster aus, um zu überprüfen, ob eine Datei einen Analysepunkt hat:
    
        ```powershell
        fsutil reparsepoint query <your-file-name>
        ```

        Wenn die Datei einen Analysepunkt enthält, sollte **Analysentagwert: 0x8000001e** angezeigt werden. Dieser Hexadezimalwert ist der Analysepunktwert, der der Azure-Dateisynchronisierung zuzuordnen ist. Die Ausgabe enthält außerdem die Analysedaten, die für den Pfad zu Ihrer Datei auf der Azure-Dateifreigabe stehen.

        > [!WARNING]  
        > Der Hilfsprogrammbefehl `fsutil reparsepoint` bietet außerdem die Möglichkeit, einen Analysepunkt zu löschen. Führen Sie diesen Befehl nur aus, wenn Sie vom Engineering-Team für die Azure-Dateisynchronisierung dazu aufgefordert werden. Bei der Ausführung dieses Befehls kann es zu Datenverlusten kommen. 

<a id="afs-recall-file"></a>
### <a name="a-file-i-want-to-use-has-been-tiered-how-can-i-recall-the-file-to-disk-to-use-it-locally"></a>Eine Datei, die ich verwenden möchte, wurde ausgelagert. Wie kann ich die Datei auf den Datenträger zurückrufen, um sie lokal zu verwenden?
Der einfachste Weg zum Zurückrufen einer Datei auf den Datenträger besteht darin, sie zu öffnen. Der Azure-Dateisynchronisierungs-Dateisystemfilter (StorageSync.sys) lädt die Datei nahtlos von Ihrer Azure-Dateifreigabe herunter, ohne dass Sie irgendetwas tun müssen. Bei Dateitypen, die teilweise gelesen werden können (z.B. Multimedia- oder ZIP-Dateien), wird beim Öffnen einer Datei nicht die gesamte Datei heruntergeladen.

Sie können auch PowerShell nutzen, um für eine Datei das Zurückrufen zu erzwingen. Diese Option kann nützlich sein, wenn Sie mehrere Dateien auf einmal zurückrufen möchten, z.B. alle Dateien in einem Ordner. Öffnen Sie eine PowerShell-Sitzung auf dem Serverknoten, auf dem die Azure-Dateisynchronisierung installiert ist, und führen Sie anschließend die folgenden PowerShell-Befehle aus:
    
```powershell
Import-Module "C:\Program Files\Azure\StorageSyncAgent\StorageSync.Management.ServerCmdlets.dll"
Invoke-StorageSyncFileRecall -Path <path-to-to-your-server-endpoint>
```
Optionale Parameter:
* `-Order CloudTieringPolicy` ruft zuerst die Dateien ab, die zuletzt geändert oder auf die zuletzt zugegriffen wurden, wie es die aktuelle Tieringrichtlinie erlaubt. 
    * Wenn die Richtlinie für den freien Volumespeicherplatz konfiguriert ist, werden Dateien abgerufen, bis die Einstellung der Richtlinie für den freien Volumespeicherplatz erreicht ist. Wenn die Einstellung der Richtlinie für den freien Volumespeicherplatz z. B. 20 % beträgt, werden keine Dateien mehr abgerufen, sobald der freie Speicherplatz auf dem Volume 20 % erreicht.  
    * Wenn die Richtlinie für den freien Volumespeicherplatz und die Datumsrichtlinie konfiguriert sind, werden Dateien abgerufen, bis die Einstellung der Richtlinie für den freien Volumespeicherplatz oder der Datumsrichtlinie erreicht ist. Wenn die Einstellung der Richtlinie für den freien Volumenspeicherplatz z. B. 20 % und die Einstellung der Datumsrichtlinie 7 Tage beträgt, werden keine Dateien mehr abgerufen, sobald der freie Speicherplatz auf dem Volume 20 % erreicht wurde oder alle Dateien mit einem Änderungs- oder Zugriffszeitpunkt innerhalb von 7 Tagen lokal gespeichert sind.
* `-ThreadCount` bestimmt, wie viele Dateien parallel abgerufen werden können.
* `-PerFileRetryCount` bestimmt, wie oft ein Abruf einer Datei versucht wird, die derzeit blockiert ist.
* `-PerFileRetryDelaySeconds` bestimmt die Zeit in Sekunden zwischen den Versuchen zum erneuten Abrufen und sollte immer in Kombination mit dem vorherigen Parameter verwendet werden.

Beispiel:
```powershell
Import-Module "C:\Program Files\Azure\StorageSyncAgent\StorageSync.Management.ServerCmdlets.dll"
Invoke-StorageSyncFileRecall -Path <path-to-to-your-server-endpoint> -ThreadCount 8 -Order CloudTieringPolicy -PerFileRetryCount 3 -PerFileRetryDelaySeconds 10
``` 

> [!Note]  
> - Sie können auch das Cmdlet Invoke-StorageSyncFileRecall verwenden, um die Leistung von Dateidownloads zu optimieren, wenn Sie einen neuen Serverendpunkt einer vorhandenen Synchronisierungsgruppe hinzufügen.  
>- Wenn das lokale Volume, das den Server hostet, nicht genügend freien Speicherplatz aufweist, um alle ausgelagerten Daten abzurufen, schlägt das Cmdlet `Invoke-StorageSyncFileRecall` fehl.  

<a id="sizeondisk-versus-size"></a>
### <a name="why-doesnt-the-size-on-disk-property-for-a-file-match-the-size-property-after-using-azure-file-sync"></a>Warum stimmt die Eigenschaft *Größe auf Datenträger* einer Datei nach der Verwendung der Azure-Dateisynchronisierung nicht mit der Eigenschaft *Größe* überein? 
Der Windows-Datei-Explorer verwendet zwei Eigenschaften zur Darstellung einer Dateigröße: **Größe** und **Größe auf Festplatte**. Diese Eigenschaften unterscheiden sich leicht in ihrer Bedeutung. **Größe** steht für die vollständige Größe der Datei. **Größe auf Datenträger** steht für die Größe des Dateidatenstroms, der auf dem Datenträger gespeichert ist. Die Werte dieser Eigenschaften können aus unterschiedlichen Gründen variieren, z.B. aufgrund einer Komprimierung, einer Datendeduplizierung oder eines Cloudtiering mit der Azure-Dateisynchronisierung. Wenn eine Datei per Tiering auf eine Azure-Dateifreigabe ausgelagert wird, ist die Größe auf dem Datenträger gleich null, da der Dateidatenstrom nicht auf dem Datenträger, sondern in Ihrer Azure-Dateifreigabe gespeichert wird. Für eine Datei kann das Tiering auch teilweise durchgeführt werden (bzw. teilweise zurückgerufen werden). Bei einer teilweise per Tiering ausgelagerten Datei befindet sich ein Teil der Datei auf dem Datenträger. Dies kann vorkommen, wenn Dateien von Anwendungen, z.B. Multimedia-Playern oder ZIP-Hilfsprogrammen, teilweise gelesen werden. 

<a id="afs-force-tiering"></a>
### <a name="how-do-i-force-a-file-or-directory-to-be-tiered"></a>Wie kann ich das Tiering einer Datei oder eines Verzeichnisses erzwingen?

> [!NOTE]
> Wenn Sie ein Verzeichnis für das Tiering auswählen, wird es nur auf die Dateien angewendet, die sich derzeit im Verzeichnis befinden. Für alle Dateien, die später erstellt werden, wird kein automatisches Tiering ausgeführt.

Wenn das Cloudtiering-Feature aktiviert ist, wird beim Cloudtiering automatisch basierend auf dem letzten Zugriff und den Änderungszeiten gefiltert, um den auf dem Cloudendpunkt angegebenen freien Volumespeicherplatz zu erzielen. In einigen Fällen kann es auch erforderlich sein, das Tiering für eine Datei manuell zu erzwingen. Dies kann beispielsweise nützlich sein, wenn Sie eine große Datei speichern, die Sie für längere Zeit nicht mehr verwenden werden, und den freien Speicherplatz auf Ihrem Volume nun für andere Dateien und Ordner nutzen möchten. Sie können das Tiering mithilfe der folgenden PowerShell-Befehle erzwingen:

```powershell
Import-Module "C:\Program Files\Azure\StorageSyncAgent\StorageSync.Management.ServerCmdlets.dll"
Invoke-StorageSyncCloudTiering -Path <file-or-directory-to-be-tiered>
```

<a id="afs-image-thumbnail"></a>
### <a name="why-are-my-tiered-files-not-showing-thumbnails-or-previews-in-windows-explorer"></a>Warum werden für meine Tieringdateien keine Miniaturansichten oder Vorschauversionen in Windows-Explorer angezeigt?
Für Tieringdateien sind Miniatur- und Vorschauansichten auf dem Serverendpunkt nicht sichtbar. Dieses Verhalten ist zu erwarten, da das Feature zur Zwischenspeicherung von Miniaturansichten in Windows absichtlich das Lesen von Dateien mit dem Attribut „Offline“ überspringt. Wenn Cloudtiering aktiviert ist, würde das Lesen der Tieringdateien dazu führen, dass diese heruntergeladen (abgerufen) werden.

Dieses Verhalten ist nicht spezifisch für die Azure-Dateisynchronisierung. In Windows-Explorer wird für alle Dateien, die das Attribut „Offline“ aufweisen, ein graues „X“ angezeigt. Dieses „X“ wird beim Zugriff auf Dateien über SMB angezeigt. Eine ausführliche Erläuterung des Verhaltens finden Sie unter [https://blogs.msdn.microsoft.com/oldnewthing/20170503-00/?p=96105](https://blogs.msdn.microsoft.com/oldnewthing/20170503-00/?p=96105).

<a id="afs-tiering-disabled"></a>
### <a name="i-have-cloud-tiering-disabled-why-are-there-tiered-files-in-the-server-endpoint-location"></a>Ich habe Cloudtiering deaktiviert. Warum befinden sich mehrstufige Dateien am Speicherort des Serverendpunkts?

Es gibt zwei Gründe, warum mehrstufige Dateien am Speicherort des Serverendpunkts vorhanden sein können:

- Beim Hinzufügen eines neuen Serverendpunkts zu einer vorhandenen Synchronisierungsgruppe werden zunächst die Metadaten mit dem Server synchronisiert und anschließend die Dateien im Hintergrund auf den Server heruntergeladen. Die Dateien werden als mehrstufige Daten angezeigt, bis Sie lokal heruntergeladen wurden. Um die Leistung von Dateidownloads zu optimieren, wenn Sie einen neuen Server einer Synchronisierungsgruppe hinzufügen, verwenden Sie das Cmdlet [Invoke-StorageSyncFileRecall](storage-sync-cloud-tiering.md#afs-recall-file).

- Wenn das Cloudtiering auf dem Serverendpunkt aktiviert und anschließend deaktiviert wurde, bleiben die Dateien so lange mehrstufig, bis auf Sie zugegriffen wird.


## <a name="next-steps"></a>Nächste Schritte
* [Planung für die Bereitstellung einer Azure-Dateisynchronisierung](storage-sync-files-planning.md)
