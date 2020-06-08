---
title: Überwachen des Azure Stack Edge-Geräts | Microsoft-Dokumentation
description: Es wird beschrieben, wie Sie über das Azure-Portal und die lokale Webbenutzeroberfläche Ihr Azure Stack Edge-Gerät überwachen.
services: databox
author: alkohli
ms.service: databox
ms.subservice: edge
ms.topic: article
ms.date: 04/15/2019
ms.author: alkohli
ms.openlocfilehash: ac762450b19f2d3f82b6859de23e07afe2d10629
ms.sourcegitcommit: a9784a3fd208f19c8814fe22da9e70fcf1da9c93
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/22/2020
ms.locfileid: "83780341"
---
# <a name="monitor-your-azure-stack-edge"></a>Überwachen Ihres Azure Stack Edge

In diesem Artikel wird beschrieben, wie Sie Ihr Azure Stack Edge-Gerät überwachen. Zum Überwachen Ihres Geräts können Sie das Azure-Portal oder die lokale Webbenutzeroberfläche verwenden. Verwenden Sie das Azure-Portal, um Geräteereignisse anzuzeigen, Warnungen zu konfigurieren und zu verwalten und Metriken anzuzeigen. Verwenden Sie die lokale Webbenutzeroberfläche auf Ihrem physischen Gerät, um den Hardwarestatus der verschiedenen Gerätekomponenten anzuzeigen.

In diesem Artikel werden folgende Vorgehensweisen behandelt:

> [!div class="checklist"]
>
> * Anzeigen von Geräteereignissen und den entsprechenden Warnungen
> * Anzeigen des Hardwarestatus von Gerätekomponenten
> * Anzeigen von Kapazitäts- und Transaktionsmetriken für Ihr Gerät
> * Konfigurieren und Verwalten von Warnungen

## <a name="view-device-events"></a>Anzeigen von Geräteereignissen

[!INCLUDE [Supported OS for clients connected to device](../../includes/data-box-edge-gateway-view-device-events.md)]

## <a name="view-hardware-status"></a>Anzeigen des Hardwarestatus

Führen Sie die folgenden Schritte auf der lokalen Webbenutzeroberfläche aus, um den Hardwarestatus Ihrer Gerätekomponenten anzuzeigen.

1. Stellen Sie eine Verbindung mit der lokalen Webbenutzeroberfläche Ihres Geräts her.
2. Navigieren Sie zu **Wartung > Hardwarestatus**. Sie können die Integrität der verschiedenen Gerätekomponenten anzeigen.

    ![Anzeigen des Hardwarestatus](media/azure-stack-edge-monitor/view-hardware-status.png)

## <a name="view-metrics"></a>Anzeigen von Metriken

[!INCLUDE [Supported OS for clients connected to device](../../includes/data-box-edge-gateway-view-metrics.md)]

### <a name="metrics-on-your-device"></a>Metriken auf Ihrem Gerät

In diesem Abschnitt werden die Überwachungsmetriken auf Ihrem Gerät beschrieben. Folgende Metriken sind möglich:

* Kapazitätsmetriken. Die Kapazitätsmetriken beziehen sich auf die Kapazität des Geräts.

* Transaktionsmetriken. Die Transaktionsmetriken beziehen sich auf die Lese- und Schreibvorgänge in Azure Storage.

* Edgecomputingmetriken. Die Edgecomputingmetriken beziehen sich auf die Verwendung von Edgecomputing auf Ihrem Gerät.

Eine vollständige Liste der Metriken enthält die folgende Tabelle:

|Kapazitätsmetriken                     |BESCHREIBUNG  |
|-------------------------------------|-------------|
|**Verfügbare Kapazität**               | Bezieht sich auf die Menge der Daten, die auf das Gerät geschrieben werden können. Das bedeutet, dass dies die Kapazität ist, die auf dem Gerät zur Verfügung gestellt werden kann. <br></br>Sie können die Gerätekapazität freigeben, indem Sie die lokale Kopie von Dateien löschen, die sowohl auf dem Gerät als auch in der Cloud über eine Kopie verfügen.        |
|**Gesamtkapazität**                   | Bezieht sich auf die Gesamtzahl der Bytes auf dem Gerät, auf das Daten geschrieben werden sollen. Dies wird auch als Gesamtgröße des lokalen Caches bezeichnet. <br></br> Sie können jetzt die Kapazität eines vorhandenen virtuellen Geräts erhöhen, indem Sie einen Datenträger hinzufügen. Fügen Sie einen Datenträger über die Hypervisor-Verwaltung für den virtuellen Computer hinzu, und starten Sie den virtuellen Computer neu. Der lokale Speicherpool des Gatewaygeräts wird erweitert, um den neu hinzugefügten Datenträger aufzunehmen. <br></br>Weitere Informationen finden Sie unter [Hinzufügen einer Festplatte für virtuelle Hyper-V-Computer](https://www.youtube.com/watch?v=EWdqUw9tTe4). |

|Transaktionsmetriken              | BESCHREIBUNG         |
|-------------------------------------|---------|
|**Hochgeladene Cloudbytes (Gerät)**    | Summe aller Bytes, die über alle Freigaben auf Ihrem Gerät hochgeladen wurden        |
|**Hochgeladene Cloudbytes (Freigabe)**     | Hochgeladene Bytes pro Freigabe Infrage kommt: <br></br> Avg, d. h. (Summe aller hochgeladenen Bytes pro Freigabe/Anzahl der Freigaben)  <br></br>Max, d. h. die maximale Anzahl von Bytes, die von einer Freigabe hochgeladen wurden <br></br>Min, d. h. die minimale Anzahl von Bytes, die von einer Freigabe hochgeladen wurden      |
|**Clouddownloaddurchsatz (Freigabe)**| Heruntergeladene Bytes pro Freigabe Infrage kommt: <br></br> Avg, d. h. (Summe aller von einer Freigabe gelesenen oder heruntergeladenen Bytes/Anzahl der Freigaben) <br></br> Max, d. h. die maximale Anzahl von Bytes, die von einer Freigabe heruntergeladen wurden<br></br> Min, d. h. die minimale Anzahl von Bytes, die von einer Freigabe heruntergeladen wurden  |
|**Cloudlesedurchsatz**            | Summe aller Bytes, die über alle Freigaben auf Ihrem Gerät aus der Cloud gelesen wurden     |
|**Clouduploaddurchsatz**          | Summe aller Bytes, die über alle Freigaben auf Ihrem Gerät in die Cloud geschrieben wurden     |
|**Clouduploaddurchsatz (Freigabe)**  | Summe aller Bytes, die von einer Freigabe in die Cloud geschrieben wurden/Anzahl der Freigaben ist durchschnittlich, maximal und minimal pro Freigabe      |
|**Lesedurchsatz (Netzwerk)**           | Enthält den Systemnetzwerkdurchsatz für alle aus der Cloud gelesenen Bytes Diese Ansicht kann Daten enthalten, die nicht auf Freigaben beschränkt sind. <br></br>Die Aufteilung zeigt den Datenverkehr über alle Netzwerkadapter auf dem Gerät an. Dazu gehören auch Adapter, die nicht verbunden oder aktiviert sind.      |
|**Schreibdurchsatz (Netzwerk)**       | Enthält den Systemnetzwerkdurchsatz für alle in die Cloud geschriebenen Bytes Diese Ansicht kann Daten enthalten, die nicht auf Freigaben beschränkt sind. <br></br>Die Aufteilung zeigt den Datenverkehr über alle Netzwerkadapter auf dem Gerät an. Dazu gehören auch Adapter, die nicht verbunden oder aktiviert sind.          |

| Edgecomputingmetriken              | BESCHREIBUNG         |
|-------------------------------------|---------|
|**Edgecomputing – Arbeitsspeichernutzung**      |           |
|**Edgecomputing – CPU in Prozent**    |         |

## <a name="manage-alerts"></a>Warnungen verwalten

[!INCLUDE [Supported OS for clients connected to device](../../includes/data-box-edge-gateway-manage-alerts.md)]

## <a name="next-steps"></a>Nächste Schritte

Erfahren Sie, wie Sie [Bandbreite verwalten](azure-stack-edge-manage-bandwidth-schedules.md).
