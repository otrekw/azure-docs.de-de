---
title: Verwenden von NFS-Blobspeicher mit Azure HPC Cache
description: Hier werden Prozeduren und Einschränkungen bei der Verwendung des ADLS-NFS-Blobspeichers mit Azure HPC Cache beschrieben.
author: ekpgh
ms.service: hpc-cache
ms.topic: how-to
ms.date: 03/29/2021
ms.author: v-erkel
ms.openlocfilehash: d861a41d8cdeac548729ff341b3ffe27c0aa8152
ms.sourcegitcommit: 20f8bf22d621a34df5374ddf0cd324d3a762d46d
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/09/2021
ms.locfileid: "107259909"
---
# <a name="use-nfs-mounted-blob-storage-preview-with-azure-hpc-cache"></a>Verwenden von in NFS eingebundenem Blobspeicher (VORSCHAU) mit Azure HPC Cache

Ab März 2021 können Sie in NFS eingebundene Blobcontainer mit Azure HPC Cache nutzen. Weitere Informationen zur [NFS 3.0-Protokollunterstützung in Azure Blobspeicher](../storage/blobs/network-file-system-protocol-support.md) finden Sie auf der Dokumentationswebsite zum Blobspeicher.

> [!NOTE]
> Die NFS 3.0-Protokollunterstützung in Azure Blob Storage ist in der Vorschau verfügbar und sollte nicht in Produktionsumgebungen verwendet werden. In der [Dokumentation zur NFS-Protokollunterstützung](../storage/blobs/network-file-system-protocol-support.md) finden Sie Updates und weitere Informationen.

Azure HPC Cache verwendet NFS-fähigen Blobspeicher im ADLS-NFS-Speicherzieltyp. Diese Speicherziele haben Ähnlichkeiten mit regulären NFS-Speicherzielen, aber auch mit regulären Azure-Blobzielen.

In diesem Artikel werden Strategien und Einschränkungen erläutert, die Sie kennen sollten, wenn Sie ADLS-NFS-Speicherziele verwenden.

Lesen Sie auch die Dokumentation zu NFS-Blobs, insbesondere die folgenden Abschnitte, in denen kompatible und inkompatible Szenarios beschrieben werden:

* [Funktionsübersicht](../storage/blobs/network-file-system-protocol-support.md#applications-and-workloads-suited-for-this-feature)
* [Features, die noch nicht unterstützt werden](../storage/blobs/network-file-system-protocol-support.md#azure-storage-features-not-yet-supported)
* [Leistungsaspekte](../storage/blobs/network-file-system-protocol-support-performance.md)

## <a name="understand-consistency-requirements"></a>Grundlegendes zu Konsistenzanforderungen

HPC Cache erfordert eine starke Konsistenz für ADLS-NFS-Speicherziele. Standardmäßig aktualisiert NFS-fähiger Blobspeicher keine Dateimetadaten. Dadurch kann HPC Cache Dateiversionen nicht genau vergleichen.

Als Lösung deaktiviert Azure HPC Cache automatisch das NFS-Attributzwischenspeichern für jeden NFS-fähigen Blobcontainer, der als Speicherziel verwendet wird.

Diese Einstellung bleibt für die Lebensdauer des Containers erhalten, auch wenn Sie ihn aus dem Cache entfernen.

## <a name="preload-data-with-nfs-protocol"></a>Im Voraus Laden von Daten mit dem NFS-Protokoll

In einem NFS-fähigen Blobcontainer kann *eine Datei nur von dem Protokoll bearbeitet werden, das beim Erstellen verwendet wurde*. Wenn Sie also die Azure REST-API zum Auffüllen eines Containers verwenden, können Sie NFS nicht verwenden, um diese Dateien zu aktualisieren. Es können keine Dateien bearbeitet werden, die mit der Azure REST-API erstellt wurden, da Azure HPC Cache nur NFS verwendet.

Es ist kein Problem für den Cache, wenn Ihr Container leer ist, oder wenn die Dateien mithilfe von NFS erstellt wurden.

Wenn die Dateien in Ihrem Container mit der REST-API des Azure-Blobs anstelle von NFS erstellt wurden, ist Azure HPC Cache auf die folgenden Aktionen für die ursprünglichen Dateien beschränkt:

* Auflisten der Dateien in einem Verzeichnis
* Lesen der Datei (und Speichern im Cache für nachfolgende Lesezugriffe)
* Löschen der Datei
* Leeren der Datei (Kürzen auf 0)
* Eine Kopie der Datei speichern Die Kopie wird als NFS-erstellte Datei markiert und kann mithilfe von NFS bearbeitet werden.

Azure HPC Cache kann den Inhalt einer Datei, die mit REST erstellt wurde, **nicht** bearbeiten. Das bedeutet, dass Sie eine geänderte Datei nicht von einem Client wieder auf dem Speicherziel speichern können.

Diese Einschränkungen müssen Sie sich merken, da sie zu Datenintegritätsproblemen führen kann, wenn Sie Zwischenspeicherungsmodelle für Lese-/Schreibzugriff für Dateien verwenden, die nicht mit NFS erstellt wurden.

> [!TIP]
> Weitere Informationen zum Zwischenspeichern von Lese- und Schreibvorgängen finden Sie unter [Verstehen von Cachenutzungsmodellen](cache-usage-models.md).

### <a name="write-caching-scenarios"></a>Szenarios für das Zwischenspeichern von Schreibvorgängen

Zu diesen Cachenutzungsmodellen gehört das Zwischenspeichern von Schreibvorgängen:

* **Mehr als 15 % Schreibvorgänge**
* **Mehr als 15 % Schreibvorgänge, der Sicherungsserver wird alle 30 Sekunden auf Änderungen überprüft**
* **Mehr als 15 % Schreibvorgänge, der Sicherungsserver wird alle 60 Sekunden auf Änderungen überprüft**
* **Mehr als 15 % Schreibvorgänge, Zurückschreiben auf den Server alle 30 Sekunden**

Verwenden Sie diese Nutzungsmodelle nur zum Bearbeiten von Dateien, die mit NFS erstellt wurden.

Wenn Sie versuchen, das Zwischenspeichern von Schreibvorgängen für mit REST erstellte Dateien zu verwenden, können Ihre Dateiänderungen verlorengehen. Das liegt daran, dass der Cache nicht versucht, Dateiänderungen sofort im Speichercontainer zu speichern.

Wenn Sie versuchen, Schreibvorgänge in mit REST erstellten Dateien zwischenzuspeichern, gefährden Sie Ihre Daten:

1. Der Cache akzeptiert Änderungen von Clients und gibt bei jeder Änderung eine Erfolgsmeldung zurück.
1. Der Cache speichert die geänderte Datei im Speicher und wartet auf weitere Änderungen.
1. Nach einiger Zeit versucht der Cache, die geänderte Datei im Back-End-Container zu speichern. An diesem Punkt wird eine Fehlermeldung angezeigt, da versucht wird, mit NFS in eine mit REST erstellte Datei zu schreiben.

   Es ist zu spät, um den Clientcomputer darüber zu informieren, dass die Änderungen nicht akzeptiert wurden, und der Cache hat keine Möglichkeit, die ursprüngliche Datei zu aktualisieren. Die Änderungen von Clients gehen also verloren.

### <a name="read-caching-scenarios"></a>Szenarios für das Zwischenspeichern von Lesevorgängen

Szenarios für das Zwischenspeichern von Lesevorgängen sind für Dateien geeignet, die entweder mit NFS oder der Azure Blob REST-API erstellt wurden.

In diesen Nutzungsmodellen wird nur das Zwischenspeichern von Lesevorgängen verwendet:

* **Leseintensiv, unregelmäßige Schreibvorgänge**
* **Clients schreiben in das NFS-Ziel unter Umgehung des Caches.**
* **Viele Lesezugriffe, Überprüfung des Sicherungsservers alle 3 Stunden**

Sie können diese Nutzungsmodelle bei Dateien verwenden, die von der REST-API oder von NFS erstellt wurden. Alle NFS-Schreibvorgänge, die von einem Client an den Back-End-Container gesendet werden, schlagen weiterhin fehl. Der Fehlschlag erfolgt jedoch sofort und es wird eine Fehlermeldung an den Client zurückgegeben.

Ein Workflow zum Zwischenspeichern von Lesevorgängen kann weiterhin Dateiänderungen umfassen, solange diese nicht zwischengespeichert werden. Clients können beispielsweise auf Dateien aus dem Container zugreifen, müssen ihre Änderungen jedoch als neue Datei zurückschreiben oder geänderte Dateien an einem anderen Speicherort speichern.

## <a name="recognize-network-lock-manager-nlm-limitations"></a>Erkennen von Einschränkungen bei Network Lock Manager (NLM)

NFS-fähige Blobcontainer unterstützen Network Lock Manager (NLM) nicht. Dabei handelt es sich um ein häufig verwendetes NFS-Protokoll zum Schutz von Dateien vor Konflikten.

Wenn Ihr NFS-Workflow ursprünglich für Hardwarespeichersysteme geschrieben wurde, können Ihre Clientanwendungen NLM-Anforderungen enthalten. Sie können diese Einschränkung umgehen, wenn Sie Ihren Prozess in NFS-fähigen Blobspeicher verschieben. Stellen Sie dazu beim Einbinden des Caches durch Ihre Clients sicher, dass diese NLM deaktivieren.

Verwenden Sie die Option ``-o nolock`` im Befehl ``mount`` Ihrer Clients, um NLM zu deaktivieren. Diese Option verhindert, dass Clients NLM-Sperren anfordern und Fehler als Antwort erhalten.

In bestimmten Fällen bestätigt Azure HPC Cache selbst NLM-Anforderungen vom Client. Das Cachenutzungsmodell **Read heavy, infrequent writes** (Leseintensiv, unregelmäßige Schreibvorgänge) bestätigt NLM-Anforderungen im Auftrag seines Back-End-Speichers. Dieses System verhindert Fehler auf dem Client, erstellt jedoch keine Sperre für den ADLS-NFS-Container.

Wenn Sie das Nutzungsmodell eines ADLS-NFS-Speicherziels auf oder von **Read heavy, infrequent writes** umstellen, müssen Sie alle Clients mithilfe der Option ``nolock`` erneut einbinden.

Weitere Informationen zu NLM, HPC Cache und Nutzungsmodellen finden Sie unter [Verstehen von Cachenutzungsmodellen](cache-usage-models.md#know-when-to-remount-clients-for-nlm).

## <a name="streamline-writes-to-nfs-enabled-containers-with-hpc-cache"></a>Optimieren von Schreibvorgängen in NFS-fähige Container mit HPC Cache

Azure HPC Cache kann die Leistung einer Workload verbessern, die das Schreiben von Änderungen an in ein ADLS-NFS-Speicherziel umfasst.

> [!NOTE]
> Sie müssen NFS verwenden, um Ihren ADLS-NFS-Speichercontainer aufzufüllen, wenn Sie dessen Dateien über Azure HPC Cache bearbeiten möchten.

Im [Artikel zu Überlegungen zur Leistung](../storage/blobs/network-file-system-protocol-support-performance.md) von NFS-fähigen Blobs wird unter anderem die Einschränkung beschrieben, dass ADLS-NFS-Speicher beim Überschreiben vorhandener Dateien nicht sehr effizient ist. Wenn Sie Azure HPC Cache mit NFS-eingebundenen Blobspeichern verwenden, verarbeitet der Cache zeitweilige Rewrites, wenn Clients eine aktive Datei ändern. Die Wartezeit beim Schreiben einer Datei in den Back-End-Container wird für die Clients ausgeblendet.

Beachten Sie die Einschränkungen, die oben unter [Im Voraus Laden von Daten mit dem NFS-Protokoll](#preload-data-with-nfs-protocol) erläutert wurden.

## <a name="next-steps"></a>Nächste Schritte

* Sie können mehr über die [Voraussetzungen für ADLS-NFS-Speicherziele](hpc-cache-prerequisites.md#nfs-mounted-blob-adls-nfs-storage-requirements-preview) lesen.
* Sie können einen [NFS-fähiges Blobspeicherkonto](../storage/blobs/network-file-system-protocol-support-how-to.md) erstellen.
