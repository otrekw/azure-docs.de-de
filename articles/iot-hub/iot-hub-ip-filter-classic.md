---
title: Klassischer IP-Filter (veraltet) für Azure IoT Hub | Microsoft-Dokumentation
description: Informationen zum Aktualisieren des klassischen IP-Filters und den damit verbundenen Vorteilen
author: jlian
ms.service: iot-hub
services: iot-hub
ms.topic: conceptual
ms.date: 10/16/2020
ms.author: jlian
ms.openlocfilehash: 6f326bafb311acedc48c5a349c78f1cd6bcebc87
ms.sourcegitcommit: f28ebb95ae9aaaff3f87d8388a09b41e0b3445b5
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/29/2021
ms.locfileid: "101661153"
---
# <a name="iot-hub-classic-ip-filter-and-how-to-upgrade"></a>Klassischer IP-Filter für IoT Hub und Vorgehensweise zum Aktualisieren 

Der aktualisierte IP-Filter für IoT Hub schützt den integrierten Endpunkt und ist standardmäßig sicher. Obwohl wir uns bemühen, niemals Breaking Changes vorzunehmen, ist das erweiterte Sicherheitsmodell des aktualisierten IP-Filters mit dem klassischen IP-Filter nicht kompatibel. Deshalb kündigen wir dessen Außerbetriebnahme an. Weitere Informationen zum neuen aktualisierten IP-Filter finden Sie unter [Neuerungen](#whats-new) und [IP-Filter für IoT Hub](iot-hub-ip-filtering.md).

Zur Vermeidung einer Dienstunterbrechung müssen Sie die geführte Aktualisierung vor dem Migrationsstichtag erledigen, weil die Aktualisierung ab dann automatisch durchgeführt wird. Weitere Informationen zur Migrationszeitachse finden Sie unter [Azure-Update](https://aka.ms/ipfilterv2azupdate).

## <a name="how-to-upgrade"></a>Aktualisieren

1.  Besuchen Sie das Azure-Portal.
2.  Navigieren Sie zu Ihrem IoT Hub.
3.  Wählen Sie im linken Menü die Option **Netzwerk** aus.
4.  Es sollte ein Banner mit der Aufforderung angezeigt werden, Ihren IP-Filter auf das neue Modell zu aktualisieren. Klicken Sie auf **Yes** (Ja), um den Vorgang fortzusetzen.
    :::image type="content" source="media/iot-hub-ip-filter-classic/ip-filter-upgrade-banner.png" alt-text="Bild der Banner-Eingabeaufforderung zum Aktualisieren des klassischen IP-Filters":::
5.  Da der neue IP-Filter alle IP-Adressen standardmäßig blockiert, entfernt das Upgrade Ihre individuellen Regeln für „Ablehnen“, ermöglicht es Ihnen aber, sie vor dem Speichern zu überprüfen. Überprüfen Sie sorgfältig die Regeln, um sicherzustellen, dass Sie bei Ihnen funktionieren.
6.  Folgen Sie den Eingabeaufforderungen, um die Aktualisierung abzuschließen.

## <a name="whats-new"></a>Neues

### <a name="secure-by-default"></a>Standardmäßig sicher

Der klassische IP-Filter erlaubt implizit, dass alle IP-Adressen eine Verbindung mit dem IoT Hub standardmäßig herstellen. Dies entspricht nicht den gängigsten Szenarien für Netzwerksicherheit. Normalerweise möchten Sie, dass nur vertrauenswürdige IP-Adressen eine Verbindung mit Ihrem IoT-Hub herstellen und alles andere ablehnen können. Zur Erreichung dieses Ziels mit klassischem IP-Filter ist ein mehrstufiger Prozess erforderlich. Wenn Sie beispielsweise Datenverkehr nur aus `192.168.100.0/22` zulassen möchten, müssen Sie Folgendes ausführen:

1. Konfigurieren Sie eine einzelne *Zulassen*-Regel für `192.168.100.0/22`.
1. Konfigurieren Sie eine andere *Blockieren*-Regel für `0.0.0.0/0` (die „Alle blockieren“-Regel).
1. Sorgen Sie dafür, dass die Regeln richtig angeordnet sind – die Regel „Zulassen“ über der Regel „Blockieren“.

In der Praxis führt dieser mehrstufige Prozess zu Verwirrung. Weil Benutzer die Regel „Alle blockieren“ nicht konfigurierten oder die Regeln nicht richtig sortierten, kam es zu einer unbeabsichtigten Gefährdung. 

Der neue IP-Filter blockiert standardmäßig alle IP-Adressen. Nur die IP-Adressbereiche, die Sie explizit hinzufügen, dürfen Verbindungen mit IoT Hub herstellen. Im vorstehenden Beispiel sind die Schritte 2 und 3 nicht mehr erforderlich. Durch dieses neue Verhalten wird die Konfiguration vereinfacht und das [Prinzip „standardmäßig sicher“](https://wikipedia.org/wiki/Secure_by_default) eingehalten.

### <a name="protect-the-built-in-event-hub-compatible-endpoint"></a>Schützen des integrierten Event Hub-kompatiblen Endpunkts

Der klassische IP-Filter kann auf den integrierten Endpunkt nicht angewendet werden. Diese Einschränkung bedeutet: Wenn ein Ereignis mit konfigurierter Regel „Alle blockieren“ (`0.0.0.0/0` blockieren) konfiguriert wurde, kann auf den integrierten Endpunkt über jede beliebige IP-Adresse weiterhin zugegriffen werden.

Der neue IP-Filter enthält eine Option zum Anwenden von Regeln auf den integrierten Endpunkt, wodurch die Gefährdung durch Sicherheitsbedrohungen im Netzwerk verringert wird.

:::image type="content" source="media/iot-hub-ip-filter-classic/ip-filter-built-in-endpoint.png" alt-text="Bild der Umschaltfläche zum Anwenden (oder Nichtanwenden) auf den integrierten Endpunkt":::

> [!NOTE]
> Diese Option ist für kostenlose IoT-Hubs (F1) nicht verfügbar. Wenn Sie IP-Filterregeln auf den integrierten Endpunkt anwenden möchten, verwenden Sie einen kostenpflichtigen IoT-Hub.

### <a name="api-impact"></a>API-Auswirkung

Der aktualisierte IP-Filter steht in der IoT Hub-Ressourcen-API über `2020-08-31` (sowie `2020-08-31-preview`) und höher zur Verfügung. Der klassische IP-Filter steht in allen API-Versionen weiterhin zur Verfügung, wird in einer zukünftigen API-Version nahe dem Migrationsstichtag aber entfernt. Weitere Informationen zur Migrationszeitachse finden Sie unter [Azure-Update](https://aka.ms/ipfilterv2azupdate).

## <a name="tip-try-the-changes-before-they-apply"></a>Tipp: Testen Sie die Änderungen, bevor sie angewendet werden.

Da der neue IP-Filter standardmäßig alle IP-Adressen blockiert, sind einzelne Blockierungsregeln nicht mehr kompatibel. Daher entfernt der geführte Aktualisierungsvorgang diese individuellen Blockierungsregeln. 

So testen Sie die Änderung beim klassischen IP-Filter:

1. Öffnen Sie in Ihrem IoT-Hub die Registerkarte **Netzwerk**.
1. Notieren Sie sich Ihre vorhandene Konfiguration des IP-Filters (klassisch) für den Fall, dass Sie ein Rollback ausführen möchten.
1. Wählen Sie neben Regeln mit **Blockieren** das Papierkorbsymbol aus, um sie zu entfernen.
1. Fügen Sie unten mit `0.0.0.0/0` eine weitere Regel hinzu, und wählen Sie **Blockieren** aus.
1. Wählen Sie **Speichern** aus.

Diese Konfiguration imitiert, wie sich der neue IP-Filter nach der Aktualisierung von „klassisch“ verhält. Eine Ausnahme ist der integrierte Endpunktschutz, der mit dem klassischen IP-Filter nicht getestet werden kann. Weil dieses Feature aber optional ist, müssen Sie es nicht verwenden, wenn Sie denken, dass dadurch etwas unterbrochen werden könnte.

## <a name="tip-check-diagnostic-logs-for-all-ip-connections-to-your-iot-hub"></a>Tipp: Überprüfen Sie die Diagnoseprotokolle für alle IP-Verbindungen mit Ihrem IoT-Hub.

Überprüfen Sie zur Sicherstellung eines reibungslosen Übergangs Ihre Diagnoseprotokolle unter der Kategorie „Verbindungen“. Suchen Sie nach der Eigenschaft `maskedIpAddress`, um zu sehen, ob die Bereiche wie erwartet sind. Beachten Sie Folgendes: Der neue IP-Filter wird alle IP-Adressen blockieren, die nicht explizit hinzugefügt wurden.

## <a name="iot-hub-classic-ip-filter-documentation-retired"></a>Dokumentation zum klassischen IP-Filter für IoT Hub (außer Betrieb genommen)

> [!IMPORTANT]
> Im Folgenden finden Sie die Originaldokumentation zum klassischen IP-Filter, der außer Betrieb genommen wird.

Die Sicherheit ist bei jeder IoT-Lösung, die auf Azure IoT Hub basiert, ein wichtiger Aspekt. Manchmal müssen Sie im Rahmen der Sicherheitskonfiguration die IP-Adressen explizit angeben, über die Geräte eine Verbindung herstellen können. Mit dem Feature *IP-Filter* können Sie Regeln konfigurieren, mit denen Datenverkehr von bestimmten IPv4-Adressen abgelehnt oder zugelassen wird.

### <a name="when-to-use"></a>Verwendung

Wenn es hilfreich ist, die IoT Hub-Endpunkte für bestimmte IP-Adressen zu blockieren, gibt es zwei spezifische Anwendungsfälle:

* Der IoT Hub sollte nur Datenverkehr aus einem angegebenen Bereich von IP-Adressen empfangen und den anderen Datenverkehr ablehnen. Ein Beispiel hierfür ist, wenn Sie IoT Hub mit [Azure Express Route](../expressroute/expressroute-faqs.md#supported-services) verwenden, um private Verbindungen zwischen IoT Hub und Ihrer lokalen Infrastruktur zu erstellen.

* Sie müssen Datenverkehr von IP-Adressen ablehnen, die vom IoT Hub-Administrator als verdächtig eingestuft wurden.

### <a name="how-filter-rules-are-applied"></a>Anwenden von Filterregeln

Die IP-Filterregeln werden auf IoT Hub-Dienstebene angewendet. Daher gelten die IP-Filterregeln für alle Verbindungen von Geräten und Back-End-Apps mit allen unterstützten Protokollen. Allerdings sind Clients, die aus dem [integrierten Event Hub-kompatiblen Endpunkt](iot-hub-devguide-messages-read-builtin.md) direkt (und nicht über die IoT Hub-Verbindungszeichenfolge) lesen, nicht an die IP-Filterregeln gebunden. 

Alle Verbindungsversuche über eine IP-Adresse, die für eine IP-Ablehnungsregel in IoT Hub eine Übereinstimmung ergeben, werden mit dem Statuscode „401 – Nicht autorisiert“ und einer Beschreibung versehen. In der Antwortnachricht wird die IP-Regel nicht erwähnt. Das Ablehnen von IP-Adressen kann andere Azure-Dienste wie z. B. Azure Stream Analytics, Azure Virtual Machines oder den Geräte-Explorer im Azure-Portal an der Interaktion mit dem IoT-Hub hindern.

> [!NOTE]
> Wenn Sie Azure Stream Analytics (ASA) verwenden müssen, um Nachrichten aus einem IoT-Hub mit aktiviertem IP-Filter zu lesen, verwenden Sie den Event Hub-kompatiblen Namen und Endpunkt Ihres IoT-Hubs zum manuellen Hinzufügen einer [Event Hubs-Streameingabe](../stream-analytics/stream-analytics-define-inputs.md#stream-data-from-event-hubs) in der ASA.

### <a name="default-setting"></a>Standardeinstellung

Das Raster **IP-Filter** im Portal ist für ein IoT Hub standardmäßig leer. Diese Standardeinstellung bedeutet, dass Ihr Hub Verbindungen von allen IP-Adressen akzeptiert. Die Standardeinstellung entspricht einer Regel, bei der der IP-Adressbereich 0.0.0.0/0 zulässig ist.

Wenn Sie die Seite mit den IP-Filtereinstellungen aufrufen möchten, wählen Sie **Netzwerk**, **Öffentlicher Zugriff** und dann **Ausgewählte IP-Bereiche** aus:

:::image type="content" source="media/iot-hub-ip-filter-classic/ip-filter-default.png" alt-text="Standardeinstellungen der IP-Filterung von IoT Hub":::

### <a name="add-or-edit-an-ip-filter-rule"></a>Hinzufügen oder Bearbeiten einer IP-Filterregel

Zum Hinzufügen einer IP-Filterregel wählen Sie **+ IP-Filterregel hinzufügen** aus.

:::image type="content" source="./media/iot-hub-ip-filter-classic/ip-filter-add-rule.png" alt-text="Hinzufügen einer IP-Filterregel zu einem IoT-Hub":::

Nachdem Sie **IP-Filterregel hinzufügen** ausgewählt haben, füllen Sie die Felder aus.

:::image type="content" source="./media/iot-hub-ip-filter-classic/ip-filter-after-selecting-add.png" alt-text="Nach dem Auswählen von „IP-Filterregel hinzufügen“":::

* Geben Sie einen **Namen** für die IP-Filterregel an. Er muss eine eindeutige alphanumerische Zeichenfolge ohne Beachtung von Groß-/Kleinschreibung sein, die bis zu 128 Zeichen umfassen kann. Nur alphanumerische ASCII 7-Bit-Zeichen und die Zeichen `{'-', ':', '/', '\', '.', '+', '%', '_', '#', '*', '?', '!', '(', ')', ',', '=', '@', ';', '''}` sind zulässig.

* Geben Sie eine einzelne IPv4-Adresse oder einen Block von IP-Adressen in CIDR-Notation ein. In CIDR-Notation steht 192.168.100.0/22 beispielsweise für die 1.024 IPv4-Adressen von 192.168.100.0 bis 192.168.103.255.

* Wählen Sie **Zulassen** oder **Blockieren** als **Aktion** für die IP-Filterregel aus.

Nachdem Sie die Felder ausgefüllt haben, wählen Sie **Speichern** aus, um die Regel zu speichern. Daraufhin wird eine Warnung mit dem Hinweis angezeigt, dass der Updatevorgang läuft.

:::image type="content" source="./media/iot-hub-ip-filter-classic/ip-filter-save-new-rule.png" alt-text="Benachrichtigung über das Speichern einer IP-Filterregel":::

Die Option **Hinzufügen** ist deaktiviert, wenn Sie das Maximum von 10 IP-Filterregeln erreichen.

Wenn Sie eine vorhandene Regel bearbeiten möchten, wählen Sie die zu ändernden Daten aus, nehmen Sie die Änderung vor, und wählen Sie **Speichern** aus, um Ihre Bearbeitung zu speichern.

### <a name="delete-an-ip-filter-rule"></a>Löschen einer IP-Filterregel

Wählen Sie zum Löschen einer IP-Filterregel das Papierkorbsymbol in dieser Zeile und dann **Speichern** aus. Die Regel wird entfernt und die Änderung gespeichert.

:::image type="content" source="./media/iot-hub-ip-filter-classic/ip-filter-delete-rule.png" alt-text="Löschen einer IP-Filterregel für IoT Hub":::

### <a name="retrieve-and-update-ip-filters-using-azure-cli"></a>Abrufen und Aktualisieren von IP-Filtern über die Azure-Befehlszeilenschnittstelle

Die IP-Filter für Ihren IoT Hub können über die [Azure-Befehlszeilenschnittstelle](/cli/azure/) abgerufen und aktualisiert werden.

Wenn Sie die aktuellen IP-Filter für Ihren IoT Hub abrufen möchten, führen Sie Folgendes aus:

```azurecli-interactive
az resource show -n <iothubName> -g <resourceGroupName> --resource-type Microsoft.Devices/IotHubs
```

Damit wird ein JSON-Objekt zurückgegeben, in dem Ihre vorhandenen IP-Filter unter dem Schlüssel `properties.ipFilterRules` aufgelistet sind:

```json
{
...
    "properties": {
        "ipFilterRules": [
        {
            "action": "Reject",
            "filterName": "MaliciousIP",
            "ipMask": "6.6.6.6/6"
        },
        {
            "action": "Allow",
            "filterName": "GoodIP",
            "ipMask": "131.107.160.200"
        },
        ...
        ],
    },
...
}
```

Wenn Sie einen neuen IP-Filter für Ihren IoT Hub hinzufügen möchten, führen Sie aus:

```azurecli-interactive
az resource update -n <iothubName> -g <resourceGroupName> --resource-type Microsoft.Devices/IotHubs --add properties.ipFilterRules "{\"action\":\"Reject\",\"filterName\":\"MaliciousIP\",\"ipMask\":\"6.6.6.6/6\"}"
```

Wenn Sie einen vorhandenen IP-Filter in Ihrem IoT Hub entfernen möchten, führen Sie aus:

```azurecli-interactive
az resource update -n <iothubName> -g <resourceGroupName> --resource-type Microsoft.Devices/IotHubs --add properties.ipFilterRules <ipFilterIndexToRemove>
```

Beachten Sie, dass `<ipFilterIndexToRemove>` der Reihenfolge von IP-Filtern in `properties.ipFilterRules` Ihres IoT Hub entsprechen muss.

### <a name="retrieve-and-update-ip-filters-using-azure-powershell"></a>Abrufen und Aktualisieren von IP-Filtern über Azure PowerShell

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

Die IP-Filter für Ihren IoT Hub können über [Azure PowerShell](/powershell/azure/) abgerufen und festgelegt werden.

```powershell
# Get your IoT Hub resource using its name and its resource group name
$iothubResource = Get-AzResource -ResourceGroupName <resourceGroupNmae> -ResourceName <iotHubName> -ExpandProperties

# Access existing IP filter rules
$iothubResource.Properties.ipFilterRules |% { Write-host $_ }

# Construct a new IP filter
$filter = @{'filterName'='MaliciousIP'; 'action'='Reject'; 'ipMask'='6.6.6.6/6'}

# Add your new IP filter rule
$iothubResource.Properties.ipFilterRules += $filter

# Remove an existing IP filter rule using its name, e.g., 'GoodIP'
$iothubResource.Properties.ipFilterRules = @($iothubResource.Properties.ipFilterRules | Where 'filterName' -ne 'GoodIP')

# Update your IoT Hub resource with your updated IP filters
$iothubResource | Set-AzResource -Force
```

### <a name="update-ip-filter-rules-using-rest"></a>Aktualisieren von IP-Filterregeln mit REST

Möglicherweise müssen Sie den IP-Filter für Ihren IoT Hub auch über den REST-Endpunkt des Azure-Ressourcenanbieters abrufen und ändern. Lesen Sie dazu `properties.ipFilterRules` unter [createorupdate-Methode](/rest/api/iothub/iothubresource/createorupdate).

### <a name="ip-filter-rule-evaluation"></a>Auswertung von IP-Filterregeln

IP-Filterregeln werden der Reihenfolge nach angewendet, und die erste Regel, die eine Übereinstimmung mit der IP-Adresse ergibt, bestimmt die Aktion (Zulassen oder Ablehnen).

Wenn Sie beispielsweise Adressen im Bereich 192.168.100.0/22 zulassen und alle anderen Adressen ablehnen möchten, sollte die erste Regel im Raster lauten, dass der Adressbereich 192.168.100.0/22 zulässig ist. Mit der nächsten Regel sollten alle Adressen abgelehnt werden, indem der Bereich 0.0.0.0/0 verwendet wird.

Sie können die Reihenfolge der IP-Filterregeln im Raster ändern, indem Sie auf die drei vertikal angeordneten Punkte am Anfang der Zeile klicken und Drag & Drop nutzen.

Klicken Sie auf **Speichern**, um die neue Reihenfolge der IP-Filterregeln zu speichern.

:::image type="content" source="media/iot-hub-ip-filter-classic/ip-filter-rule-order.png" alt-text="Ändern der Reihenfolge Ihrer IP-Filterregeln für IoT Hub":::

## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen zu den Funktionen von IoT Hub finden Sie unter:

* [Verwenden von IP-Filtern](iot-hub-ip-filtering.md)