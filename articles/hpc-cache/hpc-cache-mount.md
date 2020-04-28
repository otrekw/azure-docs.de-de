---
title: Einbinden einer Azure HPC Cache-Instanz
description: Herstellen einer Verbindung von Clients mit einem Azure HPC Cache-Dienst
author: ekpgh
ms.service: hpc-cache
ms.topic: conceptual
ms.date: 04/15/2020
ms.author: v-erkel
ms.openlocfilehash: a44232f06b455e20530271723e816c2117b339a0
ms.sourcegitcommit: b55d7c87dc645d8e5eb1e8f05f5afa38d7574846
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/16/2020
ms.locfileid: "81458363"
---
# <a name="mount-the-azure-hpc-cache"></a>Einbinden einer Azure HPC Cache-Instanz

Nachdem der Cache erstellt wurde, können NFS-Clients mit einem einfachen `mount`-Befehl darauf zugreifen. Der Befehl verbindet einen bestimmten Speicherzielpfad auf der Azure HPC Cache-Instanz mit einem lokalen Verzeichnis auf dem Clientcomputer.

Der mount-Befehl besteht aus den folgenden Elementen:

* Eine der Einbindungsadressen des Cache (aufgelistet auf der Übersichtsseite des Cache).
* Der Pfad des virtuellen Namespace, den Sie beim Erstellen des Speicherziels festgelegt haben.
* Dem lokalen Pfad, der auf dem Client verwendet werden soll.
* Befehlsparametern zur Erfolgsoptimierung dieser Art von NFS-Einbindung.

Auf der Seite **Einbindungsanweisungen** des Caches werden die Informationen und empfohlenen Optionen gesammelt und der Prototyp eines mount-Befehls erstellt, den Sie kopieren können. Weitere Informationen finden Sie im Abschnitt [Verwenden des Hilfsprogramms „Einbindungsanweisungen“](#use-the-mount-instructions-utility).

## <a name="prepare-clients"></a>Vorbereiten der Clients

Befolgen Sie die Richtlinien in diesem Abschnitt, um sicherzustellen, dass Ihre Clients die Azure HPC Cache-Instanz auch einbinden können.

### <a name="provide-network-access"></a>Bereitstellen von Netzwerkzugriff

Die Clientcomputer müssen über Netzwerkzugriff auf das virtuelle Netzwerk des Caches und das private Subnetz verfügen.

Erstellen Sie beispielsweise Client-VMs innerhalb des gleichen virtuellen Netzwerks, oder verwenden Sie einen Endpunkt, ein Gateway oder eine andere Lösung im virtuellen Netzwerk für den Zugriff von außen. (Denken Sie daran, dass nur der Cache selbst im Subnetz des Caches gehostet werden sollte.)

### <a name="install-utilities"></a>Installieren der Hilfsprogramme

Installieren Sie zur Unterstützung des NFS-mount-Befehls das entsprechende Linux-Hilfsprogramm:

* Für Red Hat Enterprise Linux oder SuSE: `sudo yum install -y nfs-utils`
* Für Ubuntu oder Debian: `sudo apt-get install nfs-common`

### <a name="create-a-local-path"></a>Erstellen eines lokalen Pfads

Erstellen Sie auf jedem Client einen Pfad zum lokalen Verzeichnis, über den eine Verbindung mit dem Cache hergestellt wird. Erstellen Sie einen Pfad für jeden Namespacepfad, den Sie einbinden möchten.

Beispiel: `sudo mkdir -p /mnt/hpc-cache-1/target3`

Die Seite [Einbindungsanweisungen](#use-the-mount-instructions-utility) im Azure-Portal enthält einen Prototypbefehl, den Sie kopieren können.

Wenn Sie den Clientcomputer mit dem Cache verbinden, ordnen Sie diesen Pfad einem virtuellen Namespacepfad zu, der einen Speicherzielexport darstellt. Erstellen Sie Verzeichnisse für jeden der virtuellen Namespacepfade, die der Client verwenden wird.

## <a name="use-the-mount-instructions-utility"></a>Verwenden des Hilfsprogramms „Einbindungsanweisungen“

Sie können die Seite **Einbindungsanweisungen** im Azure-Portal verwenden, um einen kopierbaren Einbindungsbefehl zu erstellen. Öffnen Sie die Seite im Abschnitt **Konfigurieren** der Ansicht „Cache“ im Portal.

Bevor Sie den Befehl auf einem Client verwenden, stellen Sie sicher, dass der Client die Voraussetzungen erfüllt und über die Software verfügt, die erforderlich ist, um den NFS-Befehl `mount` wie oben unter [Vorbereiten der Clients](#prepare-clients) beschrieben, zu verwenden.

![Screenshot einer Azure HPC Cache-Instanz im Azure-Portal mit der Seite „Konfigurieren > Einbindungsanweisungen“](media/mount-instructions.png)

Erstellen Sie anhand dieser Vorgehensweise den Einbindungsbefehl.

1. Passen Sie das Feld **Clientpfad** an. Dieses Feld bietet einen Beispielbefehl, den Sie verwenden können, um einen lokalen Pfad auf dem Client zu erstellen. Der Client greift lokal in diesem Verzeichnis auf den Inhalt von Azure HPC Cache zu.

   Klicken Sie auf das Feld, und bearbeiten Sie den Befehl, um den gewünschten Verzeichnisnamen einzuschließen. Der Name wird am Ende der Zeichenfolge nach `sudo mkdir -p` angezeigt.

   ![Screenshot des Felds „Clientpfad“ mit am Ende positioniertem Cursor.](media/mount-edit-client.png)

   Nachdem Sie das Feld bearbeitet haben, wird der Einbindungsbefehl am unteren Rand der Seite mit dem neuen Clientpfad aktualisiert.

1. Wählen Sie die **Einbindungsadresse des Caches** in der Liste aus. In diesem Menü werden alle [Clienteinbindungspunkte](#find-mount-command-components) des Caches aufgeführt.

   Gleichen Sie die Clientlast über alle verfügbaren Einbindungsadressen hinweg aus, um eine bessere Cacheleistung zu erzielen.

   ![Screenshot des Felds „Einbindungsadresse des Caches“ mit Auswahlfeld, in dem drei IP-Adressen zur Auswahl angezeigt werden.](media/mount-select-ip.png)

1. Wählen Sie den **Pfad des virtuellen Namespace** aus, der für den Client verwendet werden soll. Diese Pfade sind mit Exporten auf dem Back-End-Speichersystem verknüpft.

   ![Screenshot des Felds „Namespacepfade“ mit geöffneter Auswahl.](media/mount-select-target.png)

   Sie können die virtuellen Namespacepfade auf der Portalseite „Speicherziele“ anzeigen. Informationen zur Vorgehensweise hierfür finden Sie unter [Hinzufügen von Speicherzielen](hpc-cache-add-storage.md).

   Weitere Informationen zur aggregierten Namespacefunktion von Azure HPC Cache finden Sie unter [Planen des aggregierten Namespace](hpc-cache-namespace.md).

1. Das Feld **Einbindungsbefehl** in Schritt 3 wird automatisch mit einem angepassten Einbindungsbefehl aufgefüllt, der die Einbindungsadresse, den virtuellen Namespacepfad und den Clientpfad verwendet, die Sie in den vorherigen Feldern festgelegt haben.

   Klicken Sie auf das Kopiersymbol auf der rechten Seite des Felds, um den Befehl automatisch in die Zwischenablage zu kopieren.

   ![Screenshot des Felds „Namespacepfade“ mit geöffneter Auswahl.](media/mount-command-copy.png)

1. Verwenden Sie den kopierten mount-Befehl auf dem Clientcomputer, um eine Verbindung zwischen dem Clientcomputer und dem Azure HPC Cache herzustellen. Sie können den mount-Befehl direkt über die Befehlszeile auf dem Client ausgeben oder ihn in ein Setupskript oder eine -vorlage auf dem Client einfügen.

## <a name="understand-mount-command-syntax"></a>Grundlegendes zur Syntax des mount-Befehls

Der mount-Befehl weist das folgende Format auf:

> sudo mount {*options*} *cache_mount_address*:/*namespace_path* *local_path*

Beispiel:

```bash
root@test-client:/tmp# mkdir hpccache
root@test-client:/tmp# sudo mount -o hard,proto=tcp,mountproto=tcp,retry=30 10.0.0.28:/blob-demo-0722 hpccache
root@test-client:/tmp#
```

Nachdem dieser Befehl erfolgreich ausgeführt wurde, wird der Inhalt des Speicherexports im ``hpccache``-Verzeichnis auf dem Client angezeigt.

### <a name="mount-command-options"></a>Einbindungsbefehlsoptionen

Um eine problemlose Clienteinbindung sicherzustellen, übergeben Sie diese Einstellungen und Argumente in Ihrem Einbindungsbefehl:

> mount -o hard,proto=tcp,mountproto=tcp,retry=30 ${CACHE_IP_ADDRESS}:/${NAMESPACE_PATH} ${LOCAL_FILESYSTEM_MOUNT_POINT}

| Empfohlene Einstellungen für den mount-Befehl | |
--- | ---
``hard`` | Softwareseitige Einbindungen für Azure HPC Cache sind Anwendungsfehlern und möglichen Datenverlusten zugeordnet.
``proto=tcp`` | Diese Option unterstützt eine angemessene Behandlung von NFS-Netzwerkfehlern.
``mountproto=tcp`` | Diese Option unterstützt die angemessene Behandlung von Netzwerkfehlern für Einbindungsvorgänge.
``retry=<value>`` | Legen Sie ``retry=30`` fest, um vorübergehende Einbindungsfehler zu vermeiden. (Bei Einbindungen im Vordergrund wird ein anderer Wert empfohlen.)

### <a name="find-mount-command-components"></a>Suchen nach Komponenten für den mount-Befehl

Wenn Sie einen Einbindungsbefehl ohne die Seite **Einbindungsanweisungen** erstellen möchten, finden Sie die dafür erforderlichen Komponenten auf den folgenden Seiten: die Einbindungsadressen auf der Seite **Übersicht** des Caches und die Pfade zum virtuellen Namespace auf der Seite **Speicherziel**.

![Screenshot der Übersichtsseite einer Azure HPC Cache-Instanz mit einem Hervorhebungskasten um die Einbindungsadressliste rechts unten](media/hpc-cache-mount-addresses.png)

> [!NOTE]
> Die Cache-Einbindungsadressen entsprechen Netzwerkschnittstellen innerhalb des Cache-Subnetzes. Diese NICs werden in einer Ressourcengruppe mit Namen angezeigt, die auf `-cluster-nic-` und einer Zahl enden. Sie dürfen diese Schnittstellen nicht ändern oder löschen, sonst ist der Cache nicht mehr verfügbar.

Die virtuellen Namespacepfade werden auf der Seite „Details“ des jeweiligen Speicherziels angezeigt. Klicken Sie auf den Namen eines einzelnen Speicherziels, um dessen Details anzuzeigen, einschließlich zugeordneter aggregierter Namespacepfade.

![Screenshot der Seite „Details“ eines Speicherziels (Header „Speicherziel aktualisieren“). Ein Eintrag in der Spalte „Pfad des virtuellen Namespace“ der Tabelle weist einen Hervorhebungsrahmen auf.](media/hpc-cache-view-namespace-paths.png)

## <a name="next-steps"></a>Nächste Schritte

* Informationen zum Verschieben von Daten in die Speicherziele des Caches finden Sie unter [Auffüllen des neuen Azure-Blobspeichers](hpc-cache-ingest.md).
