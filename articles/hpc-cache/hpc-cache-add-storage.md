---
title: Hinzufügen von Speicher zu einem Azure HPC Cache
description: Definieren von Speicherzielen, damit Ihr Azure HPC Cache Ihr lokales NFS-System oder Azure-Blobcontainer für die langfristige Speicherung von Dateien verwenden kann
author: ekpgh
ms.service: hpc-cache
ms.topic: conceptual
ms.date: 12/30/2019
ms.author: rohogue
ms.openlocfilehash: a68bf06bad995f71bedf6a5bdedcb676737a8c61
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/28/2020
ms.locfileid: "79233442"
---
# <a name="add-storage-targets"></a>Hinzufügen von Speicherzielen

*Speicherziele* sind Back-End-Speicher für Dateien, auf die über eine Azure HPC Cache-Instanz zugegriffen wird. Sie können NFS-Speicher hinzufügen, z. B. ein lokales Hardwaresystem, oder Daten in Azure Blob Storage speichern.

Es können bis zu zehn verschiedene Speicherziele für einen Cache definiert werden. Der Cache stellt alle Speicherziele in einem aggregierten Namespace dar.

Beachten Sie, dass der Zugriff auf die Speicherexporte vom virtuellen Netzwerk Ihres Caches aus möglich sein muss. Für einen lokalen Hardwarespeicher müssen Sie möglicherweise einen DNS-Server einrichten, der Hostnamen für den NFS-Speicherzugriff auflösen kann. Weitere Informationen finden Sie unter [DNS-Zugriff](hpc-cache-prereqs.md#dns-access).

Fügen Sie nach dem Erstellen des Caches Speicherziele hinzu. Das Verfahren unterscheidet sich geringfügig, je nachdem, ob Sie Azure Blobspeicher oder einen NFS-Export hinzufügen. Details zu beiden Szenarien finden Sie unten.

## <a name="open-the-storage-targets-page"></a>Öffnen der Seite „Speicherziele“

Öffnen Sie im Azure-Portal Ihre Cache-Instanz, und klicken Sie in der linken Seitenleiste auf **Speicherziele**. Auf der Seite „Speicherziele“ werden alle vorhandenen Ziele und ein Link zum Hinzufügen eines neuen Ziels aufgelistet.

![Screenshot des Links „Speicherziele“ in der Randleiste unter der Überschrift „Konfigurieren“, die sich zwischen den Kategorieüberschriften „Einstellungen“ und „Überwachung“ befindet](media/hpc-cache-storage-targets-sidebar.png)

## <a name="add-a-new-azure-blob-storage-target"></a>Hinzufügen eines neuen Azure Blobspeicherziels

Für ein neues Blobspeicherziel ist ein leerer Blobcontainer oder ein Container erforderlich, der mit Daten im Format des Azure HPC Cache-Clouddateisystems aufgefüllt ist. Weitere Informationen zum Vorabladen eines Blobcontainers finden Sie unter [Daten in Azure-Blobspeicher verschieben](hpc-cache-ingest.md).

Sie können auf dieser Seite unmittelbar vor dem Hinzufügen einen neuen Container erstellen.

Geben Sie diese Informationen ein, um einen Azure Blobcontainer zu definieren.

![Screenshot der Seite „Speicherziel hinzufügen“, aufgefüllt mit Informationen für ein neues Azure-Blobspeicherziel](media/hpc-cache-add-blob.png)

* **Name des Speicherziels**: Legen Sie einen Namen fest, der dieses Speicherziel im Azure HPC Cache identifiziert.
* **Zieltyp**: Wählen Sie **Blob** aus.
* **Speicherkonto**: Wählen Sie das Speicherkonto aus, das Sie verwenden möchten.

  Sie müssen die Cache-Instanz für den Zugriff auf das Speicherkonto autorisieren, wie unter [Hinzufügen der Zugriffsrollen](#add-the-access-control-roles-to-your-account) beschrieben.

  Informationen zur Art des Speicherkontos, das Sie verwenden können, finden Sie unter [Blobspeicheranforderungen](hpc-cache-prereqs.md#blob-storage-requirements).

* **Speichercontainer**: Wählen Sie den Blobcontainer für dieses Ziel aus, oder klicken Sie auf **Neu erstellen**.

  ![Screenshot des Dialogfelds zum Angeben von Name und Zugriffsebene (privat) für neuen Container](media/add-blob-new-container.png)

* **Pfad des virtuellen Namespace**: Legen Sie den clientseitigen Dateipfad für dieses Speicherziel fest. Weitere Informationen zum Feature virtueller Namespace finden Sie unter [Aggregierten Namespace konfigurieren](hpc-cache-namespace.md).

Klicken Sie abschließend auf **OK**, um das Speicherziel hinzuzufügen.

> [!NOTE]
> Wenn Ihre Speicherkontofirewall so festgelegt ist, dass der Zugriff nur auf „ausgewählte Netzwerke“ eingeschränkt wird, verwenden Sie die temporäre Problemumgehung, die unter [Problemumgehung für die Firewalleinstellungen von Blobspeicherkonten](hpc-cache-blob-firewall-fix.md) dokumentiert ist.

### <a name="add-the-access-control-roles-to-your-account"></a>Hinzufügen der Zugriffssteuerungsrollen zu Ihrem Konto

Azure HPC Cache verwendet die [rollenbasierte Zugriffssteuerung (Role-Based Access Control, RBAC)](https://docs.microsoft.com/azure/role-based-access-control/index), um den Cachedienst für den Zugriff auf Ihr Speicherkonto für Azure-Blobspeicherziele zu autorisieren.

Der Besitzer des Speicherkontos muss die Rollen [Speicherkontomitwirkender](https://docs.microsoft.com/azure/role-based-access-control/built-in-roles#storage-account-contributor) und [Mitwirkender an Storage-Blobdaten](https://docs.microsoft.com/azure/role-based-access-control/built-in-roles#storage-blob-data-contributor) für den Benutzer „HPC Cache-Ressourcenanbieter“ explizit hinzufügen.

Sie können dies im Voraus erledigen oder indem Sie auf einen Link auf der Seite klicken, auf der Sie ein Blobspeicherziel hinzufügen. Beachten Sie, dass es bis zu fünf Minuten dauern kann, bis die Rolleneinstellungen in der Azure-Umgebung weitergegeben werden. Daher sollten Sie nach dem Hinzufügen der Rollen einige Minuten warten, bevor Sie ein Speicherziel erstellen.

Schritte zum Hinzufügen der RBAC-Rollen:

1. Öffnen Sie die Seite **Zugriffssteuerung (IAM)** für das Speicherkonto. (Der Link auf der Seite **Speicherziel hinzufügen** öffnet diese Seite für das ausgewählte Konto automatisch.)

1. Klicken Sie auf die **+** oben auf der Seite, und wählen Sie **Rollenzuweisung hinzufügen** aus.

1. Wählen Sie in der Liste die Rolle "Speicherkontomitwirkender" aus.

1. Übernehmen Sie im Feld **Zugriff zuweisen zu** den Standardwert ("Azure AD-Benutzer, -Gruppe oder -Dienstprinzipal").  

1. Suchen Sie im Feld **Auswählen** nach „hpc“.  Diese Zeichenfolge sollte mit einem Dienstprinzipal mit dem Namen "HPC Cache-Ressourcenanbieter" übereinstimmen. Klicken Sie auf diesen Prinzipal, um ihn auszuwählen.

   > [!NOTE]
   > Wenn eine Suche nach „hpc“ nicht funktioniert, versuchen Sie stattdessen, die Zeichenfolge „storagecache“ zu verwenden. Benutzer, die (vor der allgemeinen Verfügbarkeit) an der Vorschau teilgenommen haben, müssen möglicherweise den älteren Namen für den Dienstprinzipal verwenden.

1. Klicken Sie unten auf die Schaltfläche **Speichern**.

1. Wiederholen Sie diesen Vorgang, um die Rolle "Mitwirkender an Storage-Blobdaten" zuzuweisen.  

![Screenshot des GUI zum Hinzufügen von Rollenzuweisungen](media/hpc-cache-add-role.png)

## <a name="add-a-new-nfs-storage-target"></a>Hinzufügen eines neuen NFS-Speicherziels

Ein NFS-Speicherziel verfügt über mehr Felder als das Blobspeicherziel. Diese Felder geben an, wie auf den Speicherexport zugegriffen werden soll und wie seine Daten effizient zwischengespeichert werden sollen. Außerdem können Sie mit einem NFS-Speicherziel mehrere Namespacepfade erstellen, wenn der NFS-Host über mehrere Exporte verfügt.

![Screenshot der Seite zum Hinzufügen von Speicherzielen mit definiertem NFS-Ziel](media/hpc-cache-add-nfs-target.png)

Geben Sie diese Informationen für ein NFS-gestütztes Speicherziel an:

* **Name des Speicherziels**: Legen Sie einen Namen fest, der dieses Speicherziel im Azure HPC Cache identifiziert.

* **Zieltyp**: Wählen Sie **NFS** aus.

* **Hostname**: Geben Sie die IP-Adresse oder den vollqualifizierten Domänennamen Ihres NFS-Speichersystems ein. (Verwenden Sie nur dann einen Domänennamen, wenn Ihr Cache Zugriff auf einen DNS-Server hat, der den Namen auflösen kann.)

* **Nutzungsmodell**: Wählen Sie auf der Grundlage Ihres Workflows eines der Datencacheprofile aus, die unten in [Auswählen eines Nutzungsmodells](#choose-a-usage-model) beschrieben werden.

### <a name="nfs-namespace-paths"></a>NFS-Namespacepfade

Ein NFS-Speicherziel kann mehrere virtuelle Pfade aufweisen, solange jeder Pfad ein anderes Export- oder Unterverzeichnis in demselben Speichersystem darstellt.

Erstellen Sie alle Pfade von einem Speicherziel aus.

Sie können [Namespacepfade](hpc-cache-edit-storage.md) für ein Speicherziel jederzeit hinzufügen und bearbeiten.

Geben Sie diese Werte für die einzelnen Namespacepfade ein:

* **Pfad des virtuellen Namespace**: Legen Sie den clientseitigen Dateipfad für dieses Speicherziel fest. Weitere Informationen zum Feature virtueller Namespace finden Sie unter [Aggregierten Namespace konfigurieren](hpc-cache-namespace.md).

<!--  The virtual path should start with a slash ``/``. -->

* **NFS-Exportpfad**: Geben Sie den Pfad zum NFS-Export ein.

* **Unterverzeichnispfad**: Wenn Sie ein bestimmtes Unterverzeichnis des Exports einbinden möchten, geben Sie es hier ein. Lassen Sie dieses Feld andernfalls leer.

Klicken Sie abschließend auf **OK**, um das Speicherziel hinzuzufügen.

### <a name="choose-a-usage-model"></a>Auswählen eines Nutzungsmodells
<!-- referenced from GUI - update aka.ms link if you change this heading -->

Wenn Sie ein Speicherziel erstellen, das auf ein NFS-Speichersystem verweist, müssen Sie das *Nutzungsmodell* für dieses Ziel auswählen. Dieses Modell bestimmt, wie Ihre Daten zwischengespeichert werden.

Drei Optionen stehen zur Verfügung:

* **Leseintensive, unregelmäßige Schreibvorgänge**: Verwenden Sie diese Option, wenn Sie den Lesezugriff auf Dateien beschleunigen möchten, die statisch sind oder selten geändert werden.

  Mit dieser Option werden von Clients gelesene Dateien zwischengespeichert, die Schreibvorgänge werden jedoch sofort an den Back-End-Speicher übergeben. Im Cache gespeicherte Dateien werden niemals mit den Dateien auf dem NFS-Speichervolume verglichen.

  Verwenden Sie diese Option nicht, wenn das Risiko besteht, dass eine Datei direkt im Speichersystem geändert werden kann, ohne sie zuvor in den Cache zu schreiben. In diesem Fall wird die zwischengespeicherte Version der Datei niemals mit Änderungen aus dem Back-End aktualisiert, und das Dataset kann inkonsistent werden.

* **Mehr als 15 % Schreibvorgänge**: Diese Option beschleunigt die Lese-und Schreibleistung. Wenn diese Option verwendet wird, müssen alle Clients über den Azure HPC-Cache auf Dateien zugreifen, anstatt den Back-End-Speicher direkt einzubinden. Die zwischengespeicherten Dateien enthalten aktuelle Änderungen, die nicht auf dem Back-End gespeichert werden.

  Bei diesem Nutzungsmodell werden Dateien im Cache nicht mit den Dateien im Back-End-Speicher verglichen. Es wird davon ausgegangen, dass die zwischengespeicherte Version der Datei aktueller ist. Eine geänderte Datei im Cache wird nur dann in das Back-End-Speichersystem geschrieben, nachdem sie sich für eine Stunde ohne zusätzliche Änderungen im Cache befunden hat.

* **Clients umgehen den Cache und schreiben in das NFS-Ziel**: Wählen Sie diese Option aus, wenn Clients in Ihrem Workflow Daten direkt in das Speichersystem schreiben, ohne zuvor in den Cache zu schreiben. Die von Clients angeforderten Dateien werden zwischengespeichert, aber alle Änderungen an diesen Dateien vom Client werden sofort an das Back-End-Speichersystem zurückgegeben.

  Bei diesem Nutzungsmodell werden die Dateien im Cache häufig anhand der Back-End-Versionen auf Updates überprüft. Bei dieser Überprüfung können Dateien außerhalb des Caches geändert werden, während die Datenkonsistenz gewahrt bleibt.

In dieser Tabelle werden die Unterschiede im Nutzungsmodell zusammengefasst:

| Nutzungsmodell | Cachemodus | Back-End-Überprüfung | Maximale Zurückschreibverzögerung |
| ---- | ---- | ---- | ---- |
| Leseintensiv, unregelmäßige Schreibvorgänge | Lesen | Nie | Keine |
| Mehr als 15 % Schreibvorgänge | Lesen/Schreiben | Nie | 1 Stunde |
| Clients umgehen den Cache | Lesen | 30 Sekunden | Keine |

## <a name="next-steps"></a>Nächste Schritte

Ziehen Sie nach dem Erstellen von Speicherzielen eine dieser Aufgaben in Erwägung:

* [Einbinden der Azure HPC Cache-Instanz](hpc-cache-mount.md)
* [Daten in Azure-Blobspeicher verschieben](hpc-cache-ingest.md)

Wenn Sie Einstellungen aktualisieren müssen, können Sie ein [Speicherziel bearbeiten](hpc-cache-edit-storage.md).
