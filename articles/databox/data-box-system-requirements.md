---
title: Systemanforderungen für Microsoft Azure Data Box | Microsoft-Dokumentation
description: Hier werden wichtige Systemanforderungen für Ihre Azure Data Box-Instanz und die Clients beschrieben, die sich mit der Data Box verbinden.
services: databox
author: alkohli
ms.service: databox
ms.subservice: pod
ms.topic: article
ms.date: 10/02/2020
ms.author: alkohli
ms.openlocfilehash: 5f1623ef4dde59e816e3afe5a5f5894c49469580
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/09/2020
ms.locfileid: "91767864"
---
# <a name="azure-data-box-system-requirements"></a>Systemanforderungen für Azure Data Box

In diesem Artikel werden wichtige Systemanforderungen für Ihre Microsoft Azure Data Box und die Clients beschrieben, die eine Verbindung mit der Data Box herstellen. Sie sollten die Informationen sorgfältig lesen, bevor Sie Ihre Data Box bereitstellen. Später können Sie während der Bereitstellung und des Betriebs bei Bedarf als Referenz darauf zurückgreifen.

Systemanforderungen:

* **Softwareanforderungen**: Für Hosts, die eine Verbindung mit der Data Box herstellen, werden die unterstützten Betriebssysteme, Dateiübertragungsprotokolle, Speicherkonten, Speichertypen und Browser für die lokale Webbenutzeroberfläche beschrieben.
* **Netzwerkanforderungen**: Für die Data Box werden Anforderungen an Netzwerkverbindungen und Ports für einen optimalen Betrieb der Data Box beschrieben.


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

Ihr Datencenter verfügt über ein Hochgeschwindigkeitsnetzwerk. Mindestens eine 10-GbE-Verbindung wird dringend empfohlen. Falls keine 10-GbE-Verbindung verfügbar ist, kann zum Kopieren der Daten ein 1-GbE-Datenlink verwendet werden, die Geschwindigkeit der Kopiervorgänge wird dadurch jedoch beeinträchtigt.

### <a name="port-requirements"></a>Portanforderungen

In der folgenden Tabelle sind die Ports aufgeführt, die in der Firewall für SMB- oder NFS-Datenverkehr geöffnet werden müssen. In dieser Tabelle bezieht sich *ein* (*eingehend*) auf die Richtung, aus der eingehende Clientanforderungen auf das Gerät zugreifen. Entsprechend bezieht sich *aus* (*ausgehend*) auf die Richtung, in der das Data Box-Gerät Daten aus der Bereitstellung heraus an externe Ziele sendet, z. B. ausgehende Verbindungen mit dem Internet.

[!INCLUDE [data-box-port-requirements](../../includes/data-box-port-requirements.md)]


## <a name="next-steps"></a>Nächste Schritte

* [Bereitstellen Ihrer Azure Data Box](data-box-deploy-ordered.md)
