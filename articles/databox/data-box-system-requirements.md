---
title: Systemanforderungen für Microsoft Azure Data Box | Microsoft-Dokumentation
description: Hier erfahren Sie etwas über wichtige Systemanforderungen für Ihre Azure Data Box und für Clients, die eine Verbindung mit der Data Box herstellen.
services: databox
author: alkohli
ms.service: databox
ms.subservice: pod
ms.topic: article
ms.date: 12/23/2020
ms.author: alkohli
ms.openlocfilehash: 0190e295c2ab206242ab8a44a09ffb42746d75bd
ms.sourcegitcommit: f5b8410738bee1381407786fcb9d3d3ab838d813
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/14/2021
ms.locfileid: "98209548"
---
# <a name="azure-data-box-system-requirements"></a>Systemanforderungen für Azure Data Box

In diesem Artikel werden wichtige Systemanforderungen für Ihre Microsoft Azure Data Box und die Clients beschrieben, die eine Verbindung mit der Data Box herstellen. Sie sollten die Informationen sorgfältig lesen, bevor Sie Ihre Data Box bereitstellen. Dann können Sie bei Bedarf während der Bereitstellung und des Betriebs als Referenz darauf zurückgreifen.

Systemanforderungen:

* **Softwareanforderungen**: Für Hosts, die eine Verbindung mit der Data Box herstellen, werden die unterstützten Betriebssysteme, Dateiübertragungsprotokolle, Speicherkonten, Speichertypen und Browser für die lokale Webbenutzeroberfläche beschrieben.
* **Netzwerkanforderungen**: Für die Data Box werden Anforderungen an Netzwerkverbindungen und Ports für den optimalen Betrieb der Data Box beschrieben.


## <a name="software-requirements"></a>Softwareanforderungen

Die Softwareanforderungen umfassen unterstützte Betriebssysteme, Dateiübertragungsprotokolle, Speicherkonten, Speichertypen und Browser für die lokale Webbenutzeroberfläche.

### <a name="supported-operating-systems-for-clients"></a>Unterstützte Betriebssysteme für Clients

[!INCLUDE [data-box-supported-os-clients](../../includes/data-box-supported-os-clients.md)]


### <a name="supported-file-transfer-protocols-for-clients"></a>Unterstützte Dateiübertragungsprotokolle für Clients

[!INCLUDE [data-box-supported-file-systems-clients](../../includes/data-box-supported-file-systems-clients.md)]

> [!IMPORTANT] 
> Die Verbindung mit Data Box-Freigaben wird über REST für Exportaufträge nicht unterstützt. 

### <a name="supported-storage-accounts"></a>Unterstützte Speicherkonten

[!INCLUDE [data-box-supported-storage-accounts](../../includes/data-box-supported-storage-accounts.md)]

### <a name="supported-storage-types"></a>Unterstützte Speichertypen

[!INCLUDE [data-box-supported-storage-types](../../includes/data-box-supported-storage-types.md)]

### <a name="supported-web-browsers"></a>Unterstützte Webbrowser

[!INCLUDE [data-box-supported-web-browsers](../../includes/data-box-supported-web-browsers.md)]

## <a name="networking-requirements"></a>Netzwerkanforderungen

Ihr Datencenter verfügt über ein Hochgeschwindigkeitsnetzwerk. Wir empfehlen dringend mindestens eine 10-GbE-Verbindung. Falls keine 10-GbE-Verbindung zur Verfügung steht, können Sie eine 1-GbE-Datenverbindung verwenden. Allerdings wird dadurch die Geschwindigkeit für Kopiervorgänge beeinträchtigt.

### <a name="port-requirements"></a>Portanforderungen

In der folgenden Tabelle sind die Ports aufgeführt, die in der Firewall für SMB- oder NFS-Datenverkehr geöffnet werden müssen. In dieser Tabelle bezieht sich *ein* (*eingehend*) auf die Richtung, aus der eingehende Clientanforderungen auf das Gerät zugreifen. *Aus* (oder *ausgehend*) bezieht sich auf die Richtung, in der Ihr Data Box-Gerät Daten aus der Bereitstellung heraus an externe Ziele sendet. Beispielsweise könnte das Ziel für ausgehende Daten das Internet sein.

[!INCLUDE [data-box-port-requirements](../../includes/data-box-port-requirements.md)]


## <a name="next-steps"></a>Nächste Schritte

* [Bereitstellen Ihrer Azure Data Box](data-box-deploy-ordered.md)
