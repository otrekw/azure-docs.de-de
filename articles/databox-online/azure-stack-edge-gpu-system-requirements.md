---
title: Systemanforderungen für Microsoft Azure Stack Edge | Microsoft-Dokumentation
description: In diesem Artikel erfahren Sie mehr über die Software- und Netzwerkanforderungen für Ihr Azure Stack Edge.
services: databox
author: alkohli
ms.service: databox
ms.subservice: edge
ms.topic: conceptual
ms.date: 08/28/2020
ms.author: alkohli
ms.openlocfilehash: 7754c57563ec5acb9028c2ace217f318fea5e959
ms.sourcegitcommit: bcda98171d6e81795e723e525f81e6235f044e52
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/01/2020
ms.locfileid: "89256270"
---
# <a name="system-requirements-for-azure-stack-edge-with-gpu"></a>Systemanforderungen für Azure Stack Edge mit GPU 

In diesem Artikel werden die wichtigen Systemanforderungen für Ihre Microsoft Azure Stack Edge-Lösung und die mit Azure Stack Edge verbundenen Clients beschrieben. Es wird empfohlen, dass Sie die Informationen sorgfältig überprüfen, bevor Sie Ihr Azure Stack Edge bereitstellen. Sie können diese Informationen ggf. während der Bereitstellung und des nachfolgenden Betriebs als Referenz nutzen.

Die Systemanforderungen für Azure Stack Edge umfassen Folgendes:

- **Softwareanforderungen für Hosts:** beschreibt die unterstützten Plattformen, Browser für die lokale Konfigurationsbenutzeroberfläche, SMB-Clients und alle weiteren Anforderungen an Clients, die auf das Gerät zugreifen.
- **Netzwerkanforderungen für das Gerät:** enthält Informationen zu den Netzwerkanforderungen für den Betrieb des physischen Geräts.

## <a name="supported-os-for-clients-connected-to-device"></a>Unterstütztes Betriebssystem für Clients, die mit dem Gerät verbunden sind

[!INCLUDE [Supported OS for clients connected to device](../../includes/azure-stack-edge-gateway-supported-client-os.md)]

## <a name="supported-protocols-for-clients-accessing-device"></a>Unterstützte Protokolle für Clients, die auf das Gerät zugreifen

[!INCLUDE [Supported protocols for clients accessing device](../../includes/azure-stack-edge-gateway-supported-client-protocols.md)]

## <a name="supported-storage-accounts"></a>Unterstützte Speicherkonten

[!INCLUDE [Supported storage accounts](../../includes/azure-stack-edge-gateway-supported-storage-accounts.md)]

## <a name="supported-tiered-storage-accounts"></a>Unterstützte mehrstufige Speicherkonten

Falls von Azure Stack verwaltet, werden die folgenden mehrstufigen Speicherkonten mit SMB-/NFS-/REST-Schnittstellen unterstützt.

|type  |Speicherkonto  |Kommentare  |
|---------|---------|---------|
|Standard     |GPv1: Blockblob         |         |
|    |  Blob Storage: Blockblob       | Nur für NAS unterstützt     |

*Seitenblobs und Azure Files werden in Azure Stack derzeit nicht unterstützt.
** Die Speicherebenen „Heiß“ und „Kalt“ gibt es in Azure Stack nicht. Verschieben Sie die Daten mit Azure PowerShell auf die Archivebene, nachdem die Daten hochgeladen wurden. Schritt-für-Schritt-Anweisungen finden Sie unter [Festlegen der Blobebene mithilfe von Azure PowerShell]().

## <a name="supported-storage-types"></a>Unterstützte Speichertypen

[!INCLUDE [Supported storage types](../../includes/azure-stack-edge-gateway-supported-storage-types.md)]


## <a name="supported-browsers-for-local-web-ui"></a>Unterstützte Browser für die lokale Webbenutzeroberfläche

[!INCLUDE [Supported browsers for local web UI](../../includes/azure-stack-edge-gateway-supported-browsers.md)]

## <a name="networking-port-requirements"></a>Anforderungen für den Netzwerkport

### <a name="port-requirements-for-azure-stack-edge"></a>Portanforderungen für Azure Stack Edge

In der folgenden Tabelle sind die Ports aufgeführt, die in der Firewall für SMB-, Cloud- oder Verwaltungsdatenverkehr geöffnet werden müssen. In dieser Tabelle bezieht sich *ein* oder *eingehend* auf die Richtung, aus der eingehende Clientanforderungen auf das Gerät zugreifen. Entsprechend bezieht sich *aus* oder *ausgehend* auf die Richtung, in der Ihr Azure Stack Edge-Gerät Daten über die Bereitstellung hinaus an externe Ziele sendet, z. B. ausgehende Verbindungen mit dem Internet.

[!INCLUDE [Port configuration for device](../../includes/azure-stack-edge-gateway-port-config.md)]

### <a name="port-requirements-for-iot-edge"></a>Portanforderungen für IoT Edge

Azure IoT Edge ermöglicht die ausgehende Kommunikation von einem lokalen Edgegerät mit der Azure-Cloud über unterstützte IoT Hub-Protokolle. Die eingehende Kommunikation ist nur für bestimmte Szenarien erforderlich, in denen Azure IoT Hub Nachrichten an das Azure IoT Edge-Gerät (z.B. C2D-Nachrichten) pushen muss.

Die folgende Tabelle enthält die Portkonfiguration für Server, die die Azure IoT Edge-Runtime hosten:

| Port-Nr. | ein oder aus | Portbereich | Erforderlich | Anleitungen |
|----------|-----------|------------|----------|----------|
| TCP 443 (HTTPS)| aus       | WAN        | Ja      | Ausgehend offen für die IoT Edge-Bereitstellung. Diese Konfiguration ist bei Verwendung manueller Skripts oder des Azure IoT Device Provisioning-Diensts (Device Provisioning Service, DPS) erforderlich.|

Ausführliche Informationen finden Sie unter [Firewall- und Portkonfigurationsregeln für die IoT Edge-Bereitstellung](https://docs.microsoft.com/azure/iot-edge/troubleshoot).

## <a name="url-patterns-for-firewall-rules"></a>URL-Muster für Firewallregeln

Netzwerkadministratoren können häufig erweiterte, auf den URL-Mustern basierende Firewallregeln konfigurieren, die zum Filtern des eingehenden und ausgehenden Verkehrs verwendet werden. Ihr Azure Stack Edge-Gerät und der Dienst sind abhängig von anderen Microsoft-Anwendungen wie Azure Service Bus, Azure Active Directory Access Control, Speicherkonten und Microsoft Update-Servern. Die URL-Muster, die diesen Anwendungen zugeordnet sind, können verwendet werden, um Firewallregeln zu konfigurieren. Es ist wichtig, zu verstehen, dass sich diese den Anwendungen zugeordneten URL-Muster ändern können. Diese Änderungen erfordern, dass der Netzwerkadministrator die Firewallregeln für Ihr Azure Stack Edge nach Bedarf überwachen und aktualisieren muss.

Es wird empfohlen, dass Sie Ihre Firewallregeln für den ausgehenden Verkehr basierend auf den festen IP-Adressen für Azure Stack Edge in den meisten Fällen recht locker festlegen. Sie können jedoch die folgenden Informationen verwenden, um erweiterte Firewallregeln festzulegen, die erforderlich sind, um sichere Umgebungen zu erstellen.

> [!NOTE]
> - Die Geräte-IPs (Quell-IPs) sollten immer für alle cloudaktivierten Netzwerkschnittstellen eingerichtet sein.
> - Die Ziel-IPs sollten auf die [IP-Bereiche des Azure-Datencenters](https://www.microsoft.com/download/confirmation.aspx?id=41653) festgelegt werden.

### <a name="url-patterns-for-gateway-feature"></a>URL-Muster für Gateway-Feature

[!INCLUDE [URL patterns for firewall](../../includes/azure-stack-edge-gateway-url-patterns-firewall.md)]

### <a name="url-patterns-for-compute-feature"></a>URL-Muster für Compute-Feature

| URL-Muster                      | Komponente oder Funktion                     |   
|----------------------------------|---------------------------------------------|
| https:\//mcr.microsoft.com<br></br>https://\*.cdn.mscr.io | Microsoft-Containerregistrierung (erforderlich)               |
| https://\*.azurecr.io                     | Persönliche Containerregistrierungen und Containerregistrierungen von Drittanbietern (optional) | 
| https://\*.azure-devices.net              | IoT Hub-Zugriff (erforderlich)                             | 

### <a name="url-patterns-for-gateway-for-azure-government"></a>URL-Muster für das Gateway für Azure Government

[!INCLUDE [Azure Government URL patterns for firewall](../../includes/azure-stack-edge-gateway-gov-url-patterns-firewall.md)]

### <a name="url-patterns-for-compute-for-azure-government"></a>URL-Muster für Compute für Azure Government

| URL-Muster                      | Komponente oder Funktion                     |  
|----------------------------------|---------------------------------------------|
| https:\//mcr.microsoft.com<br></br>https://\*.cdn.mscr.com | Microsoft-Containerregistrierung (erforderlich)               |
| https://\*.azure-devices.us              | IoT Hub-Zugriff (erforderlich)           |
| https://\*.azurecr.us                    | Persönliche Containerregistrierungen und Containerregistrierungen von Drittanbietern (optional) | 

## <a name="internet-bandwidth"></a>Internetbandbreite

[!INCLUDE [Internet bandwidth](../../includes/azure-stack-edge-gateway-internet-bandwidth.md)]

## <a name="compute-sizing-considerations"></a>Berechnen von Überlegungen zur Größenanpassung

Nutzen Sie Ihre Erfahrung bei der Entwicklung und dem Testen Ihrer Lösung, um sicherzustellen, dass auf Ihrem Azure Stack Edge-Gerät ausreichend Kapazität vorhanden ist und Sie die optimale Leistung Ihres Geräts erhalten.

Zu den zu berücksichtigenden Faktoren gehören:

- **Containerdetails**: Bedenken Sie Folgendes.

    - Wie hoch ist Ihr Containerspeicherbedarf? Wie viel Arbeitsspeicher, Speicherplatz und CPU belegt Ihr Container?
    - Wie viele Container umfasst Ihre Workload? Sie könnten eine Vielzahl von leichten Containern anstelle von einigen ressourcenintensiven Containern verwenden.
    - Welche Ressourcen sind diesen Containern zugeteilt im Vergleich zu den von ihnen belegten Ressourcen (der Speicherbedarf)?
    - Wie viele Ebenen werden von den Containern gemeinsam genutzt? Containerimages sind ein Bündel von Dateien, die in einem Stapel von Ebenen angeordnet sind. Bestimmen Sie für Ihr Containerimage die Anzahl der Ebenen und deren jeweilige Größe, um den Ressourcenbedarf zu berechnen.
    - Gibt es nicht verwendete Container? Ein angehaltener Container belegt weiterhin Speicherplatz.
    - In welcher Sprache sind Ihre Container geschrieben?
- **Umfang der verarbeiteten Daten**: Wie viele Daten werden Ihre Container verarbeiten? Werden diese Daten Speicherplatz verbrauchen oder werden die Daten im Arbeitsspeicher verarbeitet?
- **Erwartete Leistung**: Was sind die gewünschten Leistungsmerkmale Ihrer Lösung? 

Um die Leistung Ihrer Lösung zu verstehen und zu optimieren, können Sie Folgendes verwenden:

- Die im Azure-Portal verfügbaren Computemetriken. Wechseln Sie zu Ihrer Azure Stack Edge-Ressource und dann zu **Überwachung > Metriken**. Betrachten Sie **Edgecomputing – Arbeitsspeichernutzung** und **Edgecomputing – CPU in Prozent**, um die verfügbaren Ressourcen zu verstehen und wie die Ressourcen verbraucht werden.
- Informationen zur Überwachung und Problembehandlung von Computemodulen finden Sie unter [Debuggen von Kubernetes-Problemen](azure-stack-edge-gpu-connect-powershell-interface.md#debug-kubernetes-issues-related-to-iot-edge).

Stellen Sie abschließend sicher, dass Sie Ihre Lösung für Ihr Dataset überprüfen und die Leistung auf Azure Stack Edge quantifizieren, bevor Sie sie in der Produktionsumgebung bereitstellen.

## <a name="next-step"></a>Nächster Schritt

- [Bereitstellen Ihres Azure Stack Edge-Geräts](azure-stack-edge-gpu-deploy-prep.md)
