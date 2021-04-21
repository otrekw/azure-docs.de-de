---
title: Konfigurieren von Azure Functions mit einem virtuellen Netzwerk
description: Dieser Artikel zeigt Ihnen, wie Sie bestimmte virtuelle Netzwerkaufgaben für Azure Functions durchführen können.
ms.topic: conceptual
ms.date: 3/13/2021
ms.custom: template-how-to
ms.openlocfilehash: c123b20e163731f9a872a969f2f1564479b6e308
ms.sourcegitcommit: 79c9c95e8a267abc677c8f3272cb9d7f9673a3d7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/19/2021
ms.locfileid: "107718429"
---
# <a name="how-to-configure-azure-functions-with-a-virtual-network"></a>Konfigurieren von Azure Functions mit einem virtuellen Netzwerk

In diesem Artikel erfahren Sie, wie Sie Aufgaben im Zusammenhang mit der Konfiguration Ihrer Funktions-App zum Herstellen einer Verbindung und zum Ausführen in einem virtuellen Netzwerk ausführen. Weitere Informationen zu Azure Functions und Netzwerken erhalten Sie unter [ Netzwerkoptionen von Azure Functions](functions-networking-options.md).

## <a name="restrict-your-storage-account-to-a-virtual-network"></a>Einschränken Ihres Speicherkontos auf ein virtuelles Netzwerk 

Beim Erstellen einer Funktions-App müssen Sie ein allgemeines Azure Storage-Konto erstellen oder verknüpfen, das Blob-, Queue- und Table Storage unterstützt. Sie können dieses Speicherkonto durch eines ersetzen, das mit Dienstendpunkten oder einem privaten Endpunkt geschützt ist. 

> [!NOTE]  
> Diese Funktion funktioniert zurzeit für alle von Windows Virtual Network unterstützten SKUs im Dedicated (App Service) Tarif und bei den Premium-Tarifen. Der Verbrauchsplan wird nicht unterstützt. 

Einrichten einer Funktion mit einem auf ein privates Netzwerk beschränkten Speicherkonto:

1. Erstellen Sie eine Funktion mit einem Speicherkonto, für das keine Dienstendpunkte aktiviert sind.

1. Konfigurieren Sie die Funktion so, dass eine Verbindung zum virtuellen Netzwerk hergestellt wird.

1. Erstellen oder konfigurieren Sie ein anderes Speicherkonto.  Dabei handelt es sich um das Speicherkonto, in dem die Dienstendpunkte abgesichert und die Verbindung zur Funktion hergestellt werden sollen.

1. [Erstellen Sie eine Dateifreigabe](../storage/files/storage-how-to-create-file-share.md#create-a-file-share) im abgesicherten Speicherkonto.

1. Aktivieren Sie Dienstendpunkte oder einen privaten Endpunkt für das Speicherkonto.  
    * Wenn Sie private Endpunktverbindungen verwenden, benötigt das Speicherkonto einen privaten Endpunkt für die `file` und `blob` Unterressourcen.  Wenn bestimmte Funktionen wie Durable Functions verwendet werden, muss der Zugriff auf `queue` und `table` außerdem über eine Verbindung mit einem privaten Endpunkt möglich sein.
    * Wenn Sie Dienstendpunkte verwenden, aktivieren Sie das für ihre Funktions-Apps dedizierte Subnetz für Speicherkonten.

1. Kopieren Sie die Datei und den Blobinhalt aus dem Speicherkonto der Funktions-App in das geschützte Speicherkonto und die Dateifreigabe.

1. Kopieren Sie die Verbindungszeichenfolge für dieses Speicherkonto.

1. Aktualisieren Sie die **Anwendungseinstellungen** unter **Konfiguration** für die Funktions-App folgendermaßen:

    | Einstellungsname | Wert | Comment |
    |----|----|----|
    | `AzureWebJobsStorage`| Speicherverbindungszeichenfolge | Dies ist die Verbindungszeichenfolge für ein gesichertes Speicherkonto. |
    | `WEBSITE_CONTENTAZUREFILECONNECTIONSTRING` |  Speicherverbindungszeichenfolge | Dies ist die Verbindungszeichenfolge für ein gesichertes Speicherkonto. |
    | `WEBSITE_CONTENTSHARE` | Dateifreigabe | Der Name der Dateifreigabe, die im gesicherten Speicherkonto erstellt wurde und in der sich die Projektbereitstellungsdateien befinden. |
    | `WEBSITE_CONTENTOVERVNET` | 1 | Neue Einstellung |
    | `WEBSITE_VNET_ROUTE_ALL` | 1 | Erzwingt den gesamten ausgehenden Datenverkehr über das virtuelle Netzwerk. Erforderlich, wenn das Speicherkonto private Endpunktverbindungen verwendet. |
    | `WEBSITE_DNS_SERVER` | `168.63.129.16` | Der von der App verwendete DNS-Server. Erforderlich, wenn das Speicherkonto private Endpunktverbindungen verwendet. |

1. Wählen Sie **Speichern**, um die Anwendungseinstellungen zu speichern. Das Ändern von App-Einstellungen führt zu einem Neustart der App.  

Nachdem die Funktions-App neu gestartet wurde, ist sie nun mit einem gesicherten Speicherkonto verbunden.

## <a name="next-steps"></a>Nächste Schritte

> [!div class="nextstepaction"]
> [Netzwerkoptionen von Azure Functions](functions-networking-options.md)

