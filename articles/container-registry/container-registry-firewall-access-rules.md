---
title: Firewallzugriffsregeln
description: Konfigurieren Sie Regeln für den Zugriff auf eine Azure-Containerregistrierung hinter einer Firewall, indem Sie den Zugriff auf die REST-API („Whitelist“) und die Domänennamen oder dienstspezifischen IP-Adressbereiche des Datenendpunkts zulassen.
ms.topic: article
ms.date: 05/18/2020
ms.openlocfilehash: 679dbcaf30653b855d35825f94e93f87ac68c322
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/09/2020
ms.locfileid: "86246978"
---
# <a name="configure-rules-to-access-an-azure-container-registry-behind-a-firewall"></a>Konfigurieren von Regeln für den Zugriff auf eine Azure-Containerregistrierung von einem Standort hinter einer Firewall

Dieser Artikel erläutert, wie Sie in Ihrer Firewall Regeln konfigurieren, um den Zugriff auf eine Azure-Containerregistrierung zuzulassen. Möglicherweise muss ein Azure IoT Edge-Gerät hinter einer Firewall oder einem Proxyserver auf eine Containerregistrierung zugreifen, um ein Pull für ein Containerimage auszuführen. Oder vielleicht benötigt ein gesperrter Server in einem lokalen Netzwerk Zugriff, um ein Push für ein Image auszuführen.

Wenn Sie stattdessen eingehenden Netzwerkzugriff auf eine Containerregistrierung nur innerhalb eines virtuellen Azure-Netzwerks konfigurieren möchten, finden Sie Informationen dazu unter [Konfigurieren von Azure Private Link für eine Azure-Containerregistrierung](container-registry-private-link.md).

## <a name="about-registry-endpoints"></a>Informationen über Registrierungsendpunkte

Um ein Pull oder ein Push für Images oder andere Artefakt für eine Azure-Containerregistrierung auszuführen, muss ein Client wie z.B. ein Docker-Daemon über HTTPS mit zwei verschiedenen Endpunkten interagieren. Für Clients, die von hinter einer Firewall auf eine Registrierung zugreifen, müssen Sie Zugriffsregeln für beide Endpunkte konfigurieren. Beide Endpunkte werden über Port 443 erreicht.

* **REST-API-Endpunkt der Registrierung**: Vorgänge für die Authentifizierungs- und Registrierungsverwaltung werden über den öffentlichen REST-API-Endpunkt der Registrierung verarbeitet. Dieser Endpunkt ist der Anmeldeservername der Registrierung. Beispiel: `myregistry.azurecr.io`

* **Speicher(daten)endpunkt**: Azure [weist Blobspeicher](container-registry-storage.md) in Azure-Speicherkonten im Namen jeder Registrierung zu, um die Daten für Containerimages und andere Artefakte zu verwalten. Wenn ein Client auf Imageebenen in einer Azure-Containerregistrierung zugreift, erfolgen die Anforderungen über einen Speicherkonto-Endpunkt, der von der Registrierung bereitgestellt wird.

Wenn Ihre Registrierung [georepliziert](container-registry-geo-replication.md) ist, muss ein Client möglicherweise mit dem Datenendpunkt in einer bestimmten Region oder mehreren replizierten Regionen interagieren.

## <a name="allow-access-to-rest-and-data-endpoints"></a>Zulassen des Zugriffs auf REST- und Datenendpunkte

* **REST-Endpunkt**: Erlaubt den Zugriff auf den vollqualifizierten Namen des Registrierungsanmeldeservers, z. B. `<registry-name>.azurecr.io`, oder einen zugeordneten IP-Adressbereich.
* **Speicher(daten)endpunkt**: Erlaubt den Zugriff auf alle Azure-Blobspeicherkonten mithilfe des Platzhalters `*.blob.core.windows.net` oder auf einen zugeordneten IP-Adressbereich.
> [!NOTE]
> Mit Azure Container Registry werden [dedizierte Datenendpunkte](#enable-dedicated-data-endpoints) eingeführt, was es Ihnen erlaubt, den Gültigkeitsbereich von Clientfirewallregeln für Ihren Registrierungsspeicher sehr eng festzulegen. Aktivieren Sie optional Datenendpunkte in allen Regionen, in denen sich die Registrierung befindet bzw. in die sie repliziert wird, indem Sie das Format `<registry-name>.<region>.data.azurecr.io` verwenden.

## <a name="allow-access-by-ip-address-range"></a>Zulassen des Zugriffs nach IP-Adressbereich

Wenn Ihre Organisation über Richtlinien verfügt, um den Zugriff nur auf bestimmte IP-Adressen oder -Adressbereiche zuzulassen, laden Sie [Azure IP Ranges and Service Tags – Public Cloud](https://www.microsoft.com/download/details.aspx?id=56519) (Azure-IP-Bereiche und Diensttags – öffentliche Cloud) herunter.

Suchen Sie in der JSON-Datei nach **AzureContainerRegistry**, um die IP-Adressbereiche für ACR-REST-Endpunkte zu finden, für die Sie den Zugriff gewähren müssen.

> [!IMPORTANT]
> Die IP-Adressbereiche für Azure-Dienste können sich ändern, Updates werden wöchentlich veröffentlicht. Laden Sie die JSON-Datei regelmäßig herunter, und aktualisieren Sie Ihre Zugriffsregeln, sofern erforderlich. Wenn Ihr Szenario das Konfigurieren von Netzwerksicherheitsgruppen-Regeln in einem virtuellen Azure-Netzwerk umfasst, oder wenn Sie Azure Firewall verwenden, verwenden Sie stattdessen das [Diensttag](#allow-access-by-service-tag) **AzureContainerRegistry**.
>

### <a name="rest-ip-addresses-for-all-regions"></a>REST-IP-Adressen für alle Regionen

```json
{
  "name": "AzureContainerRegistry",
  "id": "AzureContainerRegistry",
  "properties": {
    "changeNumber": 10,
    "region": "",
    "platform": "Azure",
    "systemService": "AzureContainerRegistry",
    "addressPrefixes": [
      "13.66.140.72/29",
    [...]
```

### <a name="rest-ip-addresses-for-a-specific-region"></a>REST-IP-Adressen für eine bestimmte Region

Suchen Sie nach der gewünschten Region, z.B. **AzureContainerRegistry.AustraliaEast**.

```json
{
  "name": "AzureContainerRegistry.AustraliaEast",
  "id": "AzureContainerRegistry.AustraliaEast",
  "properties": {
    "changeNumber": 1,
    "region": "australiaeast",
    "platform": "Azure",
    "systemService": "AzureContainerRegistry",
    "addressPrefixes": [
      "13.70.72.136/29",
    [...]
```

### <a name="storage-ip-addresses-for-all-regions"></a>Speicher-IP-Adressen für alle Regionen

```json
{
  "name": "Storage",
  "id": "Storage",
  "properties": {
    "changeNumber": 19,
    "region": "",
    "platform": "Azure",
    "systemService": "AzureStorage",
    "addressPrefixes": [
      "13.65.107.32/28",
    [...]
```

### <a name="storage-ip-addresses-for-specific-regions"></a>Speicher-IP-Adressen für eine bestimmte Region

Suchen Sie nach einer bestimmten Region, z.B. **Storage.AustraliaCentral**.

```json
{
  "name": "Storage.AustraliaCentral",
  "id": "Storage.AustraliaCentral",
  "properties": {
    "changeNumber": 1,
    "region": "australiacentral",
    "platform": "Azure",
    "systemService": "AzureStorage",
    "addressPrefixes": [
      "52.239.216.0/23"
    [...]
```

## <a name="allow-access-by-service-tag"></a>Zulassen des Zugriffs nach Diensttag

Verwenden Sie in einem virtuellen Azure-Netzwerk Netzwerksicherheitsregeln, um den Datenverkehr von einer Ressource wie z.B. einem virtuellen Computer zu einer Containerregistrierung zu filtern. Um die Erstellung der Azure-Netzwerkregeln zu vereinfachen, verwenden Sie das [Diensttag](../virtual-network/security-overview.md#service-tags) **AzureContainerRegistry**. Ein Diensttag repräsentiert eine Gruppe von IP-Adresspräfixen, mit denen global oder pro Azure-Region auf einen Azure-Dienst zugegriffen werden kann. Das Tag wird automatisch aktualisiert, wenn sich Adressen ändern. 

Erstellen Sie z.B. eine ausgehende Netzwerksicherheitsgruppen-Regel mit dem Ziel **AzureContainerRegistry**, um Datenverkehr zu einer Azure-Containerregistrierung zuzulassen. Um den Zugriff auf das Diensttag nur in einer bestimmten Region zuzulassen, geben Sie die Region in folgendem Format an: **AzureContainerRegistry**.[*Regionsname*].

## <a name="enable-dedicated-data-endpoints"></a>Aktivieren dedizierter Datenendpunkte 

> [!WARNING]
> Wenn Sie zuvor den Clientfirewallzugriff auf die vorhandenen `*.blob.core.windows.net`-Endpunkte konfiguriert haben, wirkt sich der Wechsel zu dedizierten Datenendpunkten auf die Clientkonnektivität aus und führt zu Pullfehlern. Um sicherzustellen, dass Clients über konsistenten Zugriff verfügen, fügen Sie die neuen Datenendpunktregeln zu den Clientfirewallregeln hinzu. Aktivieren Sie nach Abschluss des Vorgangs dedizierte Datenendpunkte für Ihre Registrierungen mithilfe der Azure CLI oder anderer Tools.

Dedizierte Datenendpunkte sind eine optionale Funktion des Containerregistrierungstarifs **Premium**. Informationen zu den Tarifen des Registrierungsdiensts und zu den Einschränkungen finden Sie unter [Azure Container Registry-Tarife](container-registry-skus.md). 

Sie können dedizierte Datenendpunkte über das Azure-Portal oder die Azure CLI aktivieren. Die Datenendpunkte folgen einem regionalen Muster, `<registry-name>.<region>.data.azurecr.io`. In einer georeplizierten Registrierung werden durch das Aktivieren von Datenendpunkten Endpunkte in allen Replikatregionen aktiviert.

### <a name="portal"></a>Portal

So aktivieren Sie Datenendpunkte über das Portal

1. Navigieren Sie zu Ihrer Containerregistrierung.
1. Wählen Sie **Netzwerk** > **Öffentlicher Zugriff** aus.
1. Aktivieren Sie das Kontrollkästchen **Dedizierten Datenendpunkt aktivieren**.
1. Wählen Sie **Speichern** aus.

Die Datenendpunkte werden im Portal angezeigt.

:::image type="content" source="media/container-registry-firewall-access-rules/dedicated-data-endpoints-portal.png" alt-text="Dedizierte Datenendpunkte im Portal":::

### <a name="azure-cli"></a>Azure CLI

Um Datenendpunkte mit der Azure CLI zu aktivieren, verwenden Sie Azure CLI Version 2.4.0 oder höher. Informationen zum Durchführen einer Installation oder eines Upgrades finden Sie bei Bedarf unter [Installieren der Azure CLI](/cli/azure/install-azure-cli).

Der folgende Befehl [az acr update][az-acr-update] aktiviert dedizierte Datenendpunkte in einer Registrierung namens *myregistry*. 

```azurecli
az acr update --name myregistry --data-endpoint-enabled
```

Um die Datenendpunkte anzuzeigen, verwenden Sie den Befehl [az acr show-endpoints][az-acr-show-endpoints]:

```azurecli
az acr show-endpoints --name myregistry
```

Ausgabe zu Demonstrationszwecken, die zwei regionale Endpunkte zeigt

```
{
    "loginServer": "myregistry.azurecr.io",
    "dataEndpoints": [
        {
            "region": "eastus",
            "endpoint": "myregistry.eastus.data.azurecr.io",
        },
        {
            "region": "westus",
            "endpoint": "myregistry.westus.data.azurecr.io",
        }
    ]
}
```

Nachdem Sie dedizierte Datenendpunkte für Ihre Registrierung eingerichtet haben, können Sie Clientfirewall-Zugriffsregeln für die Datenendpunkte aktivieren. Aktivieren Sie die Zugriffsregeln für den Datenendpunkt für alle erforderlichen Registrierungsregionen.

## <a name="configure-client-firewall-rules-for-mcr"></a>Konfigurieren von Clientfirewallregeln für MCR

Wenn Sie von hinter einer Firewall auf Microsoft Container Registry (MCR) zugreifen müssen, lesen Sie den Leitfaden zum Konfigurieren von [MCR-Clientfirewallregeln](https://github.com/microsoft/containerregistry/blob/master/client-firewall-rules.md). MCR ist die primäre Registrierung für alle von Microsoft veröffentlichten Docker-Images, z. B. Windows Server-Images.

## <a name="next-steps"></a>Nächste Schritte

* Informationen zu [bewährten Methoden für die Netzwerksicherheit](../security/fundamentals/network-best-practices.md)

* Weitere Informationen zu [Sicherheitsgruppen](../virtual-network/security-overview.md) in einem virtuellen Azure-Netzwerk

* Erfahren Sie mehr über das Einrichten von [Private Link](container-registry-private-link.md) für eine Containerregistrierung.

* Erfahren Sie mehr über [dedizierte Datenendpunkte](https://azure.microsoft.com/blog/azure-container-registry-mitigating-data-exfiltration-with-dedicated-data-endpoints/) für Azure Container Registry.



<!-- IMAGES -->

<!-- LINKS - External -->

<!-- LINKS - Internal -->

[az-acr-update]: /cli/azure/acr#az-acr-update
[az-acr-show-endpoints]: /cli/azure/acr#az-acr-show-endpoints
