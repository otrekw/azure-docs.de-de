---
title: include file
description: include file
services: storage
author: fauhse
ms.service: storage
ms.topic: include
ms.date: 2/20/2020
ms.author: fauhse
ms.custom: include file
ms.openlocfilehash: ffa5c899686596256622a6ea334e486c52ec710d
ms.sourcegitcommit: ba8f0365b192f6f708eb8ce7aadb134ef8eda326
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/08/2021
ms.locfileid: "109645306"
---
In diesem Schritt ermitteln Sie, wie viele Azure-Dateifreigaben Sie benötigen. Eine einzelne Windows Server-Instanz (oder ein Cluster) kann bis zu 30 Azure-Dateifreigaben synchronisieren.

Möglicherweise verfügen Sie über weitere Ordner auf Ihren Volumes, die Sie derzeit lokal als SMB-Freigaben für Ihre Benutzer und Apps freigeben. Am einfachsten können Sie sich dieses Szenario vorstellen, wenn Sie sich eine lokale Freigabe vorstellen, die sich 1:1 zu einer Azure-Dateifreigabe zuordnen lässt. Bei einer ausreichend geringen Anzahl von Freigaben (weniger als 30 für eine einzelne Windows Server-Instanz) wird eine 1:1-Zuordnung empfohlen.

Wenn Sie über mehr als 30 Freigaben verfügen, ist es häufig nicht nötig, eine lokale Freigabe 1:1 einer Azure-Dateifreigabe zuzuordnen. Ziehen Sie folgende Möglichkeiten in Betracht.

#### <a name="share-grouping"></a>Gruppierung von Freigaben

Wenn Ihre Personalabteilung z. B. über 15 Freigaben verfügt, können Sie in Erwägung ziehen, alle Personaldaten in einer einzelnen Azure-Dateifreigabe zu speichern. Wenn Sie mehrere lokale Freigaben in einer Azure-Dateifreigabe speichern, können Sie trotzdem die üblichen 15 SMB-Freigaben auf Ihrer lokalen Windows Server-Instanz erstellen. Die Gruppierung der Freigaben bedeutet lediglich, dass Sie die Stammordner dieser 15 Freigaben als Unterordner in einem gemeinsamen Ordner organisieren. Anschließend synchronisieren Sie diesen gemeinsamen Ordner mit einer Azure-Dateifreigabe. Dadurch wird für diese Gruppe lokaler Freigaben nur eine einzige Azure-Dateifreigabe in der Cloud benötigt.

#### <a name="volume-sync"></a>Volumesynchronisierung

Die Azure-Dateisynchronisierung unterstützt das Synchronisieren eines Volumestamms mit einer Azure-Dateifreigabe. Wenn Sie den Volumestamm synchronisieren, werden alle Unterordner und Dateien in derselben Azure-Dateifreigabe abgelegt.

Das Synchronisieren des Volumestamms ist daher nicht immer empfehlenswert. Das Synchronisieren mehrerer Speicherorte hat auch Vorteile. Auf diese Weise lässt sich beispielsweise die Anzahl von Elementen pro Synchronisierungsvorgang reduzieren. Die Tests von Azure-Dateifreigaben und der Azure-Dateisynchronisierung erfolgen mit 100 Millionen Elementen (Dateien und Ordner) pro Freigabe. Es empfiehlt sich jedoch häufig, in einer einzelnen Freigabe maximal 20 bis 30 Millionen Elemente zu verwenden. Das Einrichten der Azure-Dateisynchronisierung mit einer geringeren Anzahl von Elementen ist nicht nur für die Dateisynchronisierung von Vorteil. Von einer geringeren Anzahl von Elementen profitieren Sie auch in Szenarien wie den folgenden:

* Die anfängliche Überprüfung des Cloudinhalts kann schneller durchgeführt werden, was wiederum die Wartezeit für die Anzeige des Namespace auf einem Server mit aktivierter Azure-Dateisynchronisierung verringert.
* Die cloudseitige Wiederherstellung aus einer Azure-Dateifreigabe-Momentaufnahme kann schneller durchgeführt werden.
* Die Notfallwiederherstellung eines lokalen Servers kann erheblich beschleunigt werden.
* Änderungen, die direkt in einer Azure-Dateifreigabe vorgenommen wurden (außerhalb der Synchronisierung), können schneller erkannt und synchronisiert werden.

> [!TIP]
> Wenn Sie nicht wissen, wie viele Dateien und Ordner Sie besitzen, kann z. B. das Tool TreeSize von der JAM Software GmbH für Sie hilfreich sein.

#### <a name="a-structured-approach-to-a-deployment-map"></a>Strukturierte Vorgehensweise für eine Bereitstellungszuordnung

Vor der Bereitstellung von Cloudspeicher in einem späteren Schritt ist es wichtig, eine Zuordnung zwischen lokalen Ordnern und Azure-Dateifreigaben zu erstellen. Diese Zuordnung informiert darüber, wie viele und welche Ressourcen in der *Synchronisierungsgruppe* für die Azure-Dateisynchronisierung bereitgestellt werden. Eine Synchronisierungsgruppe bindet die Azure-Dateifreigabe an den Ordner auf dem Server und stellt eine Synchronisierungsverbindung her.

Um zu entscheiden, wie viele Azure-Dateifreigaben benötigt werden, sehen Sie sich die folgenden Grenzwerte und bewährten Methoden an. Auf diese Weise können Sie die Zuordnung optimieren.

* Ein Server, auf dem der Azure-Dateisynchronisierungs-Agent installiert ist, kann eine Synchronisierung mit bis zu 30 Azure-Dateifreigaben durchführen.
* Eine Azure-Dateifreigabe wird in einem Speicherkonto bereitgestellt. Das macht das Speicherkonto zu einem Skalierungsziel für Leistungswerte wie IOPS und Durchsatz.

  Eine Azure-Dateifreigabe vom Standardtyp kann theoretisch die maximale Leistung abdecken, die ein Speicherkonto bereitstellen kann. Das Platzieren mehrerer Freigaben in einem einzelnen Speicherkonto bedeutet, dass Sie einen freigegebenen Pool mit IOPS und Durchsatz für diese Freigaben erstellen. Wenn Sie die Azure-Dateisynchronisierung nur an diese Dateifreigaben anfügen möchten, wird durch das Gruppieren mehrerer Azure-Dateifreigaben im selben Speicherkonto kein Problem verursacht. Prüfen Sie die Leistungsziele der Azure-Dateifreigabe, um präzisere Erkenntnisse über die relevanten Metriken zu erhalten. Diese Einschränkungen gelten nicht für Storage Premium, bei dem die Leistung für jede Freigabe explizit bereitgestellt und garantiert wird.

  Wenn Sie beabsichtigen, eine App in Azure zu verschieben, die die Azure-Dateifreigabe nativ verwendet, benötigen Sie möglicherweise mehr Leistung von der Azure-Dateifreigabe. Wenn diese Art der Verwendung eine Möglichkeit (auch in Zukunft) darstellt, ist die Erstellung einer einzelnen Azure-Dateifreigabe vom Typ „Standard“ in einem eigenen Speicherkonto die beste Lösung.
* Es gilt ein Grenzwert von 250 Speicherkonten pro Abonnement und Azure-Region.

> [!TIP]
> Auf Grundlage dieser Informationen ist es oft erforderlich, mehrere Ordner der obersten Ebene auf Ihren Volumes in einem neuen gemeinsamen Stammverzeichnis zu gruppieren. Anschließend synchronisieren Sie dieses neue Stammverzeichnis und alle darin gruppierten Ordner mit einer einzelnen Azure-Dateifreigabe. Diese Vorgehensweise ermöglicht es Ihnen, den Grenzwert für die Synchronisierung von 30 Azure-Dateifreigaben pro Server einzuhalten.
>
> Diese Gruppierung unter einem gemeinsamen Stamm hat keine Auswirkung auf den Datenzugriff. Ihre Zugriffssteuerungslisten (ACLs) bleiben unverändert. Sie müssen lediglich Freigabepfade (wie SMB- oder NFS-Freigaben) anpassen, die möglicherweise für die lokalen Serverordner bestehen und die Sie nun in einen gemeinsamen Stamm geändert haben. Ansonsten ändert sich nichts.

> [!IMPORTANT]
> Der wichtigste Skalierungsvektor für die Azure-Dateisynchronisierung ist die Anzahl der Elemente (Dateien und Ordner), die synchronisiert werden müssen. Weitere Informationen finden Sie in [Skalierbarkeitsziele für die Azure-Dateisynchronisierung](../articles/storage/files/storage-files-scale-targets.md#azure-file-sync-scale-targets).

Es wird empfohlen, die Anzahl der Elemente pro Synchronisierungsbereich gering zu halten. Dies ist ein wichtiger Faktor, der bei der Zuordnung von Ordnern zu Azure-Dateifreigaben zu berücksichtigen ist. Die Azure-Dateisynchronisierung wird mit 100 Millionen Elementen (Dateien und Ordner) pro Freigabe getestet. Es empfiehlt sich jedoch häufig, in einer einzelnen Freigabe maximal 20 bis 30 Millionen Elemente zu verwenden. Teilen Sie Ihren Namespace in mehrere Freigaben auf, wenn Sie merken, dass Sie diese Grenze überschreiten. Sie können weiterhin mehrere lokale Freigaben in derselben Azure-Dateifreigabe gruppieren, wenn Sie ungefähr unter dieser Grenze bleiben. Diese Methode bietet Ihnen Raum für Wachstum.

Möglicherweise können in Ihrem Fall mehrere Ordner logisch mit derselben Azure-Dateifreigabe synchronisiert werden (mithilfe des oben beschriebenen neuen, gemeinsamen Stammordners). Trotzdem kann es besser sein, die Ordner so zu gruppieren, dass sie nicht mit einer, sondern mit zwei Azure-Dateifreigaben synchronisiert werden. Mit dieser Methode kann die Anzahl der Dateien und Ordner pro Dateifreigabe auf dem Server ausgeglichen werden. Sie können auch Ihre lokalen Freigaben aufteilen und sie über mehrere lokale Server hinweg synchronisieren, was die Synchronisierung mit 30 weiteren Azure-Dateifreigaben pro zusätzlichem Server ermöglicht.

#### <a name="create-a-mapping-table"></a>Erstellen einer Zuordnungstabelle

:::row:::
    :::column:::
        [![Darstellung eines Beispiels für eine Zuordnungstabelle. Laden Sie die folgende Datei herunter, um den Inhalt dieser Abbildung kennenzulernen und zu verwenden.](media/storage-files-migration-namespace-mapping/namespace-mapping.png)](media/storage-files-migration-namespace-mapping/namespace-mapping-expanded.png#lightbox)
    :::column-end:::
    :::column:::
        Bestimmen Sie anhand der zuvor beschriebenen Informationen, wie viele Azure-Dateifreigaben Sie benötigen und welche Teile Ihrer vorhandenen Daten in welcher Azure-Dateifreigabe platziert werden sollen.
        
        Erstellen Sie eine Tabelle mit den wichtigsten Aspekten, die Sie für Ihre Umgebung berücksichtigen müssen, damit Sie bei Bedarf darauf zugreifen können. Beim Erstellen Ihres Zuordnungsplans sollten Sie organisiert vorgehen, um nicht den Überblick zu verlieren, wenn Sie eine große Anzahl von Azure-Ressourcen gleichzeitig bereitstellen. Laden Sie die folgende Excel-Datei herunter, um sie als Vorlage zum Erstellen Ihrer Zuordnung zu verwenden.

[//]: # (HTML scheint die einzige Möglichkeit zu sein, eine geschachtelte zweispaltige Tabelle mit Arbeitsimageanalyse und Text/Hyperlink in derselben Zeile hinzuzufügen.)

<br>
<table>
    <tr>
        <td>
            <img src="media/storage-files-migration-namespace-mapping/excel.png" alt="Excel icon that sets the context for the download.">
        </td>
        <td>
            <a href="https://download.microsoft.com/download/1/8/D/18DC8184-E7E2-45EF-823F-F8A36B9FF240/Azure File Sync - Namespace Mapping.xlsx">Laden Sie eine Namespace-Zuordnungsvorlage herunter.</a>
        </td>
    </tr>
</table>
    :::column-end:::
:::row-end:::
