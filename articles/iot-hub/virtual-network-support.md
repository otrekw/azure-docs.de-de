---
title: Azure IoT Hub-Unterstützung für virtuelle Netzwerke
description: Hier erfahren Sie mehr über die Verwendung von Konnektivitätsmustern für virtuelle Netzwerke mit IoT Hub.
services: iot-hub
author: jlian
ms.service: iot-fundamentals
ms.topic: conceptual
ms.date: 04/28/2020
ms.author: jlian
ms.openlocfilehash: c0d01ae6507864373a79282476846d6f96adf83b
ms.sourcegitcommit: 58faa9fcbd62f3ac37ff0a65ab9357a01051a64f
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/28/2020
ms.locfileid: "82231440"
---
# <a name="iot-hub-support-for-virtual-networks"></a>IoT Hub-Unterstützung für virtuelle Netzwerke

Dieser Artikel bietet eine Einführung in das VNET-Konnektivitätsmuster und erläutert, wie Sie über ein Azure-VNET eines Benutzers eine private Konnektivitätsverbindung mit einem IoT-Hub einrichten.

> [!NOTE]
> Die in diesem Artikel beschriebenen IoT Hub-Features sind derzeit für IoT-Hubs verfügbar, die mit [verwalteter Dienstidentität](#create-an-iot-hub-with-managed-service-identity) in den folgenden Regionen erstellt wurden: „USA, Osten“, „USA, Süden-Mitte“ und „USA, Westen 2“.


## <a name="introduction"></a>Einführung

Standardmäßig werden IoT Hub-Hostnamen einem öffentlichen Endpunkt mit einer öffentlich routingfähigen IP-Adresse über das Internet zugeordnet. Wie in der Abbildung unten gezeigt, wird dieser öffentliche IoT Hub-Endpunkt von Hubs gemeinsam genutzt, die sich im Besitz verschiedener Kunden befinden, und auf die IoT-Geräte über WANs (Wide-Area Networks) und lokale Netzwerke zugreifen können.

Mehrere IoT Hub-Features wie das [Nachrichtenrouting](./iot-hub-devguide-messages-d2c.md), der [Dateiupload](./iot-hub-devguide-file-upload.md) und [das Importieren und Exportieren von Geräten per Massenvorgang](./iot-hub-bulk-identity-mgmt.md) erfordern auf ähnliche Weise eine Konnektivität zwischen IoT Hub und einer kundeneigenen Azure-Ressource über den öffentlichen Endpunkt. Wie im Folgenden dargestellt, bilden diese Konnektivitätspfade kollektiv den ausgehenden Datenverkehr von IoT Hub zu Kundenressourcen.
![Öffentlicher IoT Hub-Endpunkt](./media/virtual-network-support/public-endpoint.png)


Aus verschiedenen Gründen möchten Kunden möglicherweise die Konnektivität zu ihren Azure-Ressourcen (einschließlich IoT Hub) über ein VNET einschränken, das sie besitzen und betreiben. Zu den Gründen gehören die folgenden:

* Einführung von zusätzlichen Sicherheitsebenen über die Isolation auf Netzwerkebene für Ihren IoT-Hub, indem die Offenlegung der Konnektivität mit Ihrem Hub über das öffentliche Internet verhindert wird

* Aktivieren einer privaten Konnektivitätsumgebung von Ihren lokalen Netzwerkressourcen, um sicherzustellen, dass Ihre Daten und der Datenverkehr direkt an das Azure-Backbone-Netzwerk übertragen wird

* Verhindern der Exfiltration von Angriffen von vertraulichen, lokalen Netzwerken 

* Befolgen bewährter Azure-Konnektivitätsmuster mithilfe [privater Endpunkte](../private-link/private-endpoint-overview.md)


In diesem Artikel wird beschrieben, wie Sie diese Ziele mithilfe von [privaten Endpunkten](../private-link/private-endpoint-overview.md) für eingehende Verbindungen mit IoT Hub erreichen. Dabei werden vertrauenswürdige Azure-Erstanbieterdienste mit der Ausnahme für ausgehende Konnektivität von IoT Hub zu anderen Azure-Ressourcen verwendet.


## <a name="ingress-connectivity-to-iot-hub-using-private-endpoints"></a>Eingehende Konnektivität mit IoT Hub mithilfe privater Endpunkte

Ein privater Endpunkt ist eine private IP-Adresse, die in einem kundeneigenen VNET zugeordnet ist, über das eine Azure-Ressource erreichbar ist. Wenn Sie über einen privaten Endpunkt für Ihren IoT-Hub verfügen, können Sie zulassen, dass Dienste innerhalb Ihres VNET IoT Hub erreichen, ohne dass Datenverkehr an den öffentlichen IoT Hub-Endpunkt gesendet werden muss. Ebenso können Geräte, die in Ihrer lokalen Umgebung arbeiten, ein [virtuelles privates Netzwerk (VPN)](https://docs.microsoft.com/azure/vpn-gateway/vpn-gateway-about-vpngateways) oder privates [ExpressRoute-Peering](https://azure.microsoft.com/services/expressroute/) verwenden, um eine Verbindung mit Ihrem VNET in Azure und anschließend mit Ihrem IoT-Hub (über den privaten Endpunkt) herzustellen. Demzufolge können Kunden, die die Konnektivität zu den öffentlichen Endpunkten Ihres IoT-Hubs einschränken (oder möglicherweise vollständig blockieren) möchten, dieses Ziel erreichen, indem sie [IoT Hub-Firewallregeln](./iot-hub-ip-filtering.md) verwenden und dabei die Konnektivität mit dem Hub über den privaten Endpunkt beibehalten.

> [!NOTE]
> Der Schwerpunkt dieses Setups liegt auf Geräten, die sich in einem lokalen Netzwerk befinden. Dieses Setup wird nicht für Geräte empfohlen, die in einem WAN (Wide Area Network) bereitgestellt werden.

![Öffentlicher IoT Hub-Endpunkt](./media/virtual-network-support/virtual-network-ingress.png)

Vergewissern Sie sich, dass die folgenden Voraussetzungen erfüllt sind, bevor Sie fortfahren:

* Ihr IoT-Hub muss mit einer [verwalteten Dienstidentität](#create-an-iot-hub-with-managed-service-identity) bereitgestellt werden.

* Ihr IoT-Hub muss in einer der [unterstützten Regionen](#regional-availability-private-endpoints) bereitgestellt werden.

* Sie haben ein Azure-VNET mit einem Subnetz bereitgestellt, in dem der private Endpunkt erstellt wird. Weitere Informationen finden Sie unter [Schnellstart: Erstellen eines virtuellen Netzwerks mithilfe der Azure-Befehlszeilenschnittstelle](../virtual-network/quick-create-cli.md).

* Richten Sie in Ihrem Azure-VNET für Geräte, die in lokalen Netzwerken ausgeführt werden, ein [virtuelles privates Netzwerk (VPN)](https://docs.microsoft.com/azure/vpn-gateway/vpn-gateway-about-vpngateways) oder das private [ExpressRoute-Peering](https://azure.microsoft.com/services/expressroute/) ein.


### <a name="regional-availability-private-endpoints"></a>Regionale Verfügbarkeit (private Endpunkte)

Private Endpunkte, die in IoT-Hubs unterstützt werden, die in den folgenden Regionen erstellt wurden:

* East US

* USA Süd Mitte

* USA, Westen 2


### <a name="set-up-a-private-endpoint-for-iot-hub-ingress"></a>Einrichten eines privaten Endpunkts für eingehenden IoT Hub-Datenverkehr

Führen Sie die folgenden Schritte aus, um einen privaten Endpunkt einzurichten:

1. Führen Sie den folgenden Azure CLI-Befehl aus, um einen Azure IoT Hub-Anbieter noch mal bei Ihrem Abonnement zu registrieren:

    ```azurecli-interactive
    az provider register --namespace Microsoft.Devices --wait --subscription  <subscription-name>
    ```

2. Navigieren Sie im IoT Hub-Portal zur Registerkarte **Private Endpunktverbindungen** (diese Registerkarte ist nur für IoT-Hubs in den [unterstützten Regionen verfügbar](#regional-availability-private-endpoints)), und klicken Sie auf das Zeichen **+** , um einen neuen privaten Endpunkt hinzuzufügen.

3. Geben Sie das Abonnement, die Ressourcengruppe, den Namen und die Region an, in der der neue private Endpunkt erstellt werden soll. (Im Idealfall sollte der private Endpunkt in der gleichen Region wie Ihr Hub erstellt werden. Weitere Informationen finden Sie im Abschnitt [Regionale Verfügbarkeit (private Endpunkte)](#regional-availability-private-endpoints).)

4. Klicken Sie auf **Weiter: Ressource**, und geben Sie das Abonnement für die IoT Hub-Ressource an. Wählen Sie dann **"Microsoft.Devices/IotHubs"** als Ressourcentyp, Ihren IoT Hub-Namen als **Ressource** und **iotHub** als Zielunterressource aus.

5. Klicken Sie auf **Weiter: Konfiguration**, und stellen Sie Ihr virtuelles Netzwerk und Subnetz bereit, in dem der private Endpunkt erstellt werden soll. Klicken Sie bei Bedarf auf die Option für die Integration in eine private Azure-DNS-Zone.

6. Klicken Sie auf **Weiter: Tags**, und stellen Sie optional Tags für Ihre Ressource bereit.

7. Klicken Sie auf **Überprüfen und erstellen**, um die private Endpunktressource zu erstellen.


### <a name="pricing-private-endpoints"></a>Preise (private Endpunkte)

Ausführliche Preisinformationen finden Sie unter [Azure Private Link – Preise](https://azure.microsoft.com/pricing/details/private-link).


## <a name="egress-connectivity-from-iot-hub-to-other-azure-resources"></a>Ausgehende Konnektivität von IoT Hub zu anderen Azure-Ressourcen

IoT Hub benötigt für das [Nachrichtenrouting](./iot-hub-devguide-messages-d2c.md), den [Dateiupload](./iot-hub-devguide-file-upload.md) und das [Importieren und Exportieren von Geräten per Massenvorgang](./iot-hub-bulk-identity-mgmt.md) Zugriff auf Azure Blob Storage, Event Hubs und Service Bus-Ressourcen. Diese Vorgänge werden in der Regel über den öffentlichen Endpunkt von Ressourcen ausgeführt. Wenn Sie Ihr Speicherkonto, Event Hubs oder Service Bus-Ressourcen an ein VNET binden, wird die Konnektivität zur Ressource durch die empfohlene Konfiguration standardmäßig blockiert. Da IoT Hub auf diese Ressourcen zugreifen muss, wird die Funktionalität des Diensts beeinträchtigt.

Sie müssen die Konnektivität von Ihrer IoT Hub-Ressource mit Ihren Speicherkonten, Event Hubs oder Service Bus-Ressourcen über die Option **Azure first party trusted services** (Vertrauliche Azure-Erstanbieterdienste) aktivieren, um dieses Problem zu beheben.

Die Voraussetzungen lauten wie folgt:

* Ihr IoT-Hub muss in einer der [unterstützten Regionen](#regional-availability-trusted-microsoft-first-party-services) bereitgestellt werden.

* Ihrem IoT-Hub muss zur Bereitstellung des Hubs eine verwaltete Dienstidentität zugewiesen werden. Befolgen Sie die Anweisungen zum [Erstellen eines Hubs mit verwalteter Dienstidentität](#create-an-iot-hub-with-managed-service-identity).


### <a name="regional-availability-trusted-microsoft-first-party-services"></a>Regionale Verfügbarkeit (vertrauenswürdige Microsoft-Erstanbieterdienste)

Die Ausnahme für vertrauenswürdige Azure-Erstanbieterdienste zum Umgehen von Firewalleinschränkungen für Azure Blob Storage, Event Hubs und Service Bus-Ressourcen wird nur für IoT-Hubs in den folgenden Regionen unterstützt:

* East US

* USA Süd Mitte

* USA, Westen 2


### <a name="pricing-trusted-microsoft-first-party-services"></a>Preise (vertrauenswürdige Microsoft-Erstanbieterdienste)

Das Feature für die Ausnahme vertrauenswürdiger Microsoft-Erstanbieterdienste ist in IoT-Hubs in den [unterstützten Regionen](#regional-availability-trusted-microsoft-first-party-services) kostenlos. Die Gebühren für die bereitgestellten Speicherkonten, Event Hubs oder Service Bus-Ressourcen werden separat berechnet.


### <a name="create-an-iot-hub-with-managed-service-identity"></a>Erstellen eines IoT-Hubs mit verwalteter Dienstidentität

Eine verwaltete Dienstidentität kann Ihrem Hub zum Zeitpunkt der Ressourcenbereitstellung zugewiesen werden (dieses Feature wird derzeit für vorhandene Hubs nicht unterstützt). Dabei muss der IoT-Hub TLS 1.2 als Mindestversion verwenden. Zu diesem Zweck müssen Sie die ARM-Ressourcenvorlage unten verwenden:

```json
{
  "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
  "contentVersion": "1.0.0.0",
  "resources": [
    {
      "type": "Microsoft.Devices/IotHubs",
      "apiVersion": "2020-03-01",
      "name": "<provide-a-valid-resource-name>",
      "location": "<any-of-supported-regions>",
      "identity": {
        "type": "SystemAssigned"
      },
      "properties": {
        "minTlsVersion": "1.2"
      },
      "sku": {
        "name": "<your-hubs-SKU-name>",
        "tier": "<your-hubs-SKU-tier>",
        "capacity": 1
      }
    },
    {
      "type": "Microsoft.Resources/deployments",
      "apiVersion": "2018-02-01",
      "name": "updateIotHubWithKeyEncryptionKey",
      "dependsOn": [
        "<provide-a-valid-resource-name>"
      ],
      "properties": {
        "mode": "Incremental",
        "template": {
          "$schema": "https://schema.management.azure.com/schemas/2019-04-01/deploymentTemplate.json#",
          "contentVersion": "0.9.0.0",
          "resources": [
            {
              "type": "Microsoft.Devices/IotHubs",
              "apiVersion": "2020-03-01",
              "name": "<provide-a-valid-resource-name>",
              "location": "<any-of-supported-regions>",
              "identity": {
                "type": "SystemAssigned"
              },
              "properties": {
                "minTlsVersion": "1.2"
              },
              "sku": {
                "name": "<your-hubs-SKU-name>",
                "tier": "<your-hubs-SKU-tier>",
                "capacity": 1
              }
            }
          ]
        }
      }
    }
  ]
}
```

Nachdem Sie die Werte für Ihre Ressourcen `name`, `location`, `SKU.name` und `SKU.tier` ersetzt haben, können Sie die Ressource mithilfe der Azure CLI in einer vorhandenen Ressourcengruppe bereitstellen, indem Sie Folgendes verwenden:

```azurecli-interactive
az deployment group create --name <deployment-name> --resource-group <resource-group-name> --template-file <template-file.json>
```

Nachdem die Ressource erstellt wurde, können Sie die verwaltete Dienstidentität, die Ihrem Hub zugewiesen ist, mithilfe der Azure CLI abrufen:

```azurecli-interactive
az resource show --resource-type Microsoft.Devices/IotHubs --name <iot-hub-resource-name> --resource-group <resource-group-name>
```

Nachdem IoT Hub mit einer verwalteten Dienstidentität bereitgestellt wurde, führen Sie die Schritte im entsprechenden Abschnitt aus, um Routingendpunkte zu [Speicherkonten](#egress-connectivity-to-storage-account-endpoints-for-routing), [Event Hubs](#egress-connectivity-to-event-hubs-endpoints-for-routing) und [Service Bus-Ressourcen](#egress-connectivity-to-service-bus-endpoints-for-routing) einzurichten oder den [Dateiupload](#egress-connectivity-to-storage-accounts-for-file-upload) und das [Importieren und Exportieren von Geräten per Massenvorgang](#egress-connectivity-to-storage-accounts-for-bulk-device-importexport) zu konfigurieren.


### <a name="egress-connectivity-to-storage-account-endpoints-for-routing"></a>Ausgehende Konnektivität zu Speicherkontoendpunkten für das Routing

IoT Hub kann so konfiguriert werden, dass Nachrichten an ein Speicherkonto im Besitz von Kunden weitergeleitet werden. Damit die Routingfunktion trotz Firewalleinschränkungen auf ein Speicherkonto zugreifen kann, muss Ihr IoT-Hub über eine verwaltete Dienstidentität verfügen (siehe [Erstellen eines IoT-Hubs mit verwalteter Dienstidentität](#create-an-iot-hub-with-managed-service-identity)). Nachdem eine verwaltete Dienstidentität bereitgestellt wurde, führen Sie die folgenden Schritte aus, um der Ressourcenidentität Ihres Hubs die RBAC-Berechtigung für den Zugriff auf Ihr Speicherkonto zu erteilen.

1. Navigieren Sie im Azure-Portal zur Registerkarte **Zugriffssteuerung (IAM)** Ihres Speicherkontos, und klicken Sie im Abschnitt **Rollenzuweisung hinzufügen** auf **Hinzufügen**.

2. Wählen Sie **Mitwirkender an Storage-Blobdaten** als **Rolle** und **Azure AD user, group, or service principal** (Azure AD-Benutzer, -Gruppe oder -Dienstprinzipal) für **Assigning access to** (Zugriff für) aus, und klicken Sie dann auf den Ressourcennamen Ihres IoT-Hubs in der Dropdownliste. Klicken Sie auf die Schaltfläche **Save** .

3. Navigieren Sie in Ihrem Speicherkonto zur Registerkarte **Firewalls und virtuelle Netzwerke**, und aktivieren Sie die Option **Allow access from selected networks** (Zugriff von ausgewählten Netzwerken zulassen). Aktivieren Sie unter **Ausnahmen** das Kontrollkästchen für **Vertrauenswürdigen Microsoft-Diensten den Zugriff auf dieses Speicherkonto erlauben**. Klicken Sie auf die Schaltfläche **Save** .

4. Navigieren Sie auf der Seite Ihrer IoT Hub-Ressource zur Registerkarte **Nachrichtenrouting**.

5. Navigieren Sie zum Abschnitt **Benutzerdefinierte Endpunkte**, und klicken Sie auf **Hinzufügen**. Wählen Sie **Storage** als Endpunkttyp aus.

6. Geben Sie auf der angezeigten Seite einen Namen für den Endpunkt ein, wählen Sie den Container aus, den Sie in Blob Storage verwenden möchten, und geben Sie das Codierungs- und Dateinamenformat an. Wählen Sie **Vom System zugewiesen** als **Authentifizierungstyp** für Ihren Speicherendpunkt aus. Klicken Sie auf die Schaltfläche **Erstellen** .

Nun ist Ihr benutzerdefinierter Speicherendpunkt für die Verwendung der vom System zugewiesenen Identität des Hubs eingerichtet und verfügt trotz der Firewalleinschränkungen über die Berechtigung, auf Ihre Speicherressource zuzugreifen. Sie können diesen Endpunkt jetzt zum Einrichten einer Routingregel verwenden.


### <a name="egress-connectivity-to-event-hubs-endpoints-for-routing"></a>Ausgehende Konnektivität zu Event Hub-Endpunkten für das Routing

IoT Hub kann so konfiguriert werden, dass Nachrichten an einen Event Hub-Namespace im Besitz von Kunden weitergeleitet werden. Damit die Routingfunktion trotz Firewalleinschränkungen auf eine Event Hub-Ressource zugreifen kann, muss Ihr IoT-Hub über eine verwaltete Dienstidentität verfügen (siehe [Erstellen eines IoT-Hubs mit verwalteter Dienstidentität](#create-an-iot-hub-with-managed-service-identity)). Nachdem eine verwaltete Dienstidentität bereitgestellt wurde, führen Sie die folgenden Schritte aus, um der Ressourcenidentität Ihres Hubs die RBAC-Berechtigung für den Zugriff auf Event Hubs zu erteilen.

1. Navigieren Sie im Azure-Portal zur Registerkarte **Zugriffssteuerung (IAM)** Ihres Event Hubs, und klicken Sie im Abschnitt **Rollenzuweisung hinzufügen** auf **Hinzufügen**.

2. Wählen Sie **Azure Event Hubs-Datensender** als **Rolle** und **Azure AD user, group, or service principal** (Azure AD-Benutzer, -Gruppe oder -Dienstprinzipal) für **Assigning access to** (Zugriff für) aus, und klicken Sie dann auf den Ressourcennamen Ihres IoT-Hubs in der Dropdownliste. Klicken Sie auf die Schaltfläche **Save** .

3. Navigieren Sie in Ihren Event Hubs zur Registerkarte **Firewalls und virtuelle Netzwerke**, und aktivieren Sie die Option **Allow access from selected networks** (Zugriff von ausgewählten Netzwerken zulassen). Aktivieren Sie unter **Ausnahmen** das Kontrollkästchen für **Allow trusted Microsoft services to access event hubs** (Vertrauenswürdigen Microsoft-Diensten den Zugriff auf Event Hubs erlauben). Klicken Sie auf die Schaltfläche **Save** .

4. Navigieren Sie auf der Seite Ihrer IoT Hub-Ressource zur Registerkarte **Nachrichtenrouting**.

5. Navigieren Sie zum Abschnitt **Benutzerdefinierte Endpunkte**, und klicken Sie auf **Hinzufügen**. Wählen Sie **Event Hubs** als Endpunkttyp aus.

6. Geben Sie auf der angezeigten Seite einen Namen für den Endpunkt ein, wählen Sie den Event Hub-Namespace und die Instanz aus, und klicken Sie auf die Schaltfläche **Erstellen**.

Nun ist Ihr benutzerdefinierter Event Hub-Endpunkt für die Verwendung der vom System zugewiesenen Identität des Hubs eingerichtet und verfügt trotz der Firewalleinschränkungen über die Berechtigung, auf Ihre Event Hubs zuzugreifen. Sie können diesen Endpunkt jetzt zum Einrichten einer Routingregel verwenden.


### <a name="egress-connectivity-to-service-bus-endpoints-for-routing"></a>Ausgehende Konnektivität zu Service Bus-Endpunkten für das Routing

IoT Hub kann so konfiguriert werden, dass Nachrichten an einen Service Bus-Namespace im Besitz von Kunden weitergeleitet werden. Damit die Routingfunktion trotz Firewalleinschränkungen auf eine Service Bus-Ressource zugreifen kann, muss Ihr IoT-Hub über eine verwaltete Dienstidentität verfügen (siehe [Erstellen eines IoT-Hubs mit verwalteter Dienstidentität](#create-an-iot-hub-with-managed-service-identity)). Nachdem eine verwaltete Dienstidentität bereitgestellt wurde, führen Sie die folgenden Schritte aus, um der Ressourcenidentität Ihres Hubs die RBAC-Berechtigung für den Zugriff auf Service Bus zu erteilen.

1. Navigieren Sie im Azure-Portal zur Registerkarte **Zugriffssteuerung (IAM)** Ihres Service Bus, und klicken Sie im Abschnitt **Rollenzuweisung hinzufügen** auf **Hinzufügen**.

2. Wählen Sie **Azure Service Bus-Datensender** als **Rolle** und **Azure AD user, group, or service principal** (Azure AD-Benutzer, -Gruppe oder -Dienstprinzipal) für **Assigning access to** (Zugriff für) aus, und klicken Sie dann auf den Ressourcennamen Ihres IoT-Hubs in der Dropdownliste. Klicken Sie auf die Schaltfläche **Save** .

3. Navigieren Sie in Ihrem Service Bus zur Registerkarte **Firewalls und virtuelle Netzwerke**, und aktivieren Sie die Option **Allow access from selected networks** (Zugriff von ausgewählten Netzwerken zulassen). Aktivieren Sie unter **Ausnahmen** das Kontrollkästchen für **Allow trusted Microsoft services to access this service bus** (Vertrauenswürdigen Microsoft-Diensten den Zugriff auf Service Bus erlauben). Klicken Sie auf die Schaltfläche **Save** .

4. Navigieren Sie auf der Seite Ihrer IoT Hub-Ressource zur Registerkarte **Nachrichtenrouting**.

5. Navigieren Sie zum Abschnitt **Benutzerdefinierte Endpunkte**, und klicken Sie auf **Hinzufügen**. Wählen Sie **Service Bus-Warteschlange** oder **Service Bus-Thema** (sofern zutreffend) als Endpunkttyp aus.

6. Geben Sie auf der angezeigten Seite einen Namen für den Endpunkt ein, und wählen Sie den Namespace, die Warteschlange oder das Thema (sofern zutreffend) Ihres Service Bus aus. Klicken Sie auf die Schaltfläche **Erstellen** .

Nun ist Ihr benutzerdefinierter Service Bus-Endpunkt für die Verwendung der vom System zugewiesenen Identität des Hubs eingerichtet und verfügt trotz der Firewalleinschränkungen über die Berechtigung, auf Ihre Service Bus-Ressource zuzugreifen. Sie können diesen Endpunkt jetzt zum Einrichten einer Routingregel verwenden.


### <a name="egress-connectivity-to-storage-accounts-for-file-upload"></a>Ausgehende Konnektivität zu Speicherkonten für den Dateiupload

Das IoT Hub-Feature für den Dateiupload ermöglicht Geräten das Hochladen von Dateien in ein Speicherkonto, das im Besitz von Kunden ist. Sowohl Geräte als auch IoT Hub müssen eine Verbindung mit dem Speicherkonto herstellen, damit der Dateiuploadvorgang funktioniert. Wenn Firewalleinschränkungen für das Speicherkonto vorhanden sind, müssen Ihre Geräte einen der unterstützten Mechanismen des Speicherkontos verwenden (z. B. [private Endpunkte](../private-link/create-private-endpoint-storage-portal.md), [Dienstendpunkte](../virtual-network/virtual-network-service-endpoints-overview.md) oder eine [direkte Firewallkonfiguration](../storage/common/storage-network-security.md)), um die Verbindung herzustellen. Wenn Firewalleinschränkungen im Speicherkonto vorhanden sind, muss IoT Hub entsprechend konfiguriert werden, um über die Ausnahme für vertrauenswürdige Microsoft-Dienste auf die Speicherressource zuzugreifen. Zu diesem Zweck muss Ihr IoT-Hub über eine verwaltete Dienstidentität verfügen (siehe [Erstellen eines IoT-Hubs mit verwalteter Dienstidentität](#create-an-iot-hub-with-managed-service-identity)). Nachdem eine verwaltete Dienstidentität bereitgestellt wurde, führen Sie die folgenden Schritte aus, um der Ressourcenidentität Ihres Hubs die RBAC-Berechtigung für den Zugriff auf Ihr Speicherkonto zu erteilen.

1. Navigieren Sie im Azure-Portal zur Registerkarte **Zugriffssteuerung (IAM)** Ihres Speicherkontos, und klicken Sie im Abschnitt **Rollenzuweisung hinzufügen** auf **Hinzufügen**.

2. Wählen Sie **Mitwirkender an Storage-Blobdaten** als **Rolle** und **Azure AD user, group, or service principal** (Azure AD-Benutzer, -Gruppe oder -Dienstprinzipal) für **Assigning access to** (Zugriff für) aus, und klicken Sie dann auf den Ressourcennamen Ihres IoT-Hubs in der Dropdownliste. Klicken Sie auf die Schaltfläche **Save** .

3. Navigieren Sie in Ihrem Speicherkonto zur Registerkarte **Firewalls und virtuelle Netzwerke**, und aktivieren Sie die Option **Allow access from selected networks** (Zugriff von ausgewählten Netzwerken zulassen). Aktivieren Sie unter **Ausnahmen** das Kontrollkästchen für **Vertrauenswürdigen Microsoft-Diensten den Zugriff auf dieses Speicherkonto erlauben**. Klicken Sie auf die Schaltfläche **Save** .

4. Navigieren Sie auf der Seite Ihrer IoT Hub-Ressource zur Registerkarte **Dateiupload**.

5. Wählen Sie auf der angezeigten Seite den Container aus, den Sie in Blob Storage verwenden möchten, und konfigurieren Sie die **Dateibenachrichtigungseinstellungen**, **SAS TTL**, **Standard-TTL** und die **Anzahl maximaler Zustellungen** nach Bedarf. Wählen Sie **Vom System zugewiesen** als **Authentifizierungstyp** für Ihren Speicherendpunkt aus. Klicken Sie auf die Schaltfläche **Erstellen** .

Nun ist Ihr Speicherendpunkt für den Dateiupload für die Verwendung der vom System zugewiesenen Identität des Hubs eingerichtet und verfügt trotz der Firewalleinschränkungen über die Berechtigung, auf Ihre Speicherressource zuzugreifen.


### <a name="egress-connectivity-to-storage-accounts-for-bulk-device-importexport"></a>Ausgehende Konnektivität zu Speicherkonten für das Importieren und Exportieren von Geräten per Massenvorgang

IoT Hub unterstützt die Funktion zum [Importieren und Exportieren](./iot-hub-bulk-identity-mgmt.md) von Geräteinformationen per Massenvorgang von/in vom Kunden bereitgestellte Azure Storage Blob-Instanzen. Sowohl Geräte als auch IoT Hub müssen eine Verbindung mit dem Speicherkonto herstellen, damit das Feature für den Massenimport/-export funktioniert.

Diese Funktion erfordert Konnektivität von IoT Hub mit dem Speicherkonto. Damit trotz Firewalleinschränkungen auf eine Service Bus-Ressource zugegriffen werden kann, muss Ihr IoT-Hub über eine verwaltete Dienstidentität verfügen (siehe [Erstellen eines IoT-Hubs mit verwalteter Dienstidentität](#create-an-iot-hub-with-managed-service-identity)). Nachdem eine verwaltete Dienstidentität bereitgestellt wurde, führen Sie die folgenden Schritte aus, um der Ressourcenidentität Ihres Hubs die RBAC-Berechtigung für den Zugriff auf Service Bus zu erteilen.

1. Navigieren Sie im Azure-Portal zur Registerkarte **Zugriffssteuerung (IAM)** Ihres Speicherkontos, und klicken Sie im Abschnitt **Rollenzuweisung hinzufügen** auf **Hinzufügen**.

2. Wählen Sie **Mitwirkender an Storage-Blobdaten** als **Rolle** und **Azure AD user, group, or service principal** (Azure AD-Benutzer, -Gruppe oder -Dienstprinzipal) für **Assigning access to** (Zugriff für) aus, und klicken Sie dann auf den Ressourcennamen Ihres IoT-Hubs in der Dropdownliste. Klicken Sie auf die Schaltfläche **Save** .

3. Navigieren Sie in Ihrem Speicherkonto zur Registerkarte **Firewalls und virtuelle Netzwerke**, und aktivieren Sie die Option **Allow access from selected networks** (Zugriff von ausgewählten Netzwerken zulassen). Aktivieren Sie unter **Ausnahmen** das Kontrollkästchen für **Vertrauenswürdigen Microsoft-Diensten den Zugriff auf dieses Speicherkonto erlauben**. Klicken Sie auf die Schaltfläche **Save** .

Sie können jetzt die Azure IoT-REST-APIs zum [Erstellen von Import-/Exportaufträgen](https://docs.microsoft.com/rest/api/iothub/service/jobclient/getimportexportjobs) verwenden, um Informationen zur Verwendung der Funktion für Massenimporte/-exporte zu erhalten. Beachten Sie, dass Sie `storageAuthenticationType="identityBased"` im Anforderungstext bereitstellen und `inputBlobContainerUri="https://..."` und `outputBlobContainerUri="https://..."` als Eingabe- und Ausgabe-URLs des Speicherkontos verwenden müssen.


Azure IoT Hub SDKs unterstützen diese Funktion im Registrierungs-Manager des Clients. Der folgende Codeausschnitt zeigt, wie ein Importauftrag bzw. ein Exportauftrag mithilfe des C# SDK ausgelöst werden.

```csharp
// Call an import job on the IoT Hub
JobProperties importJob = 
await registryManager.ImportDevicesAsync(
  JobProperties.CreateForImportJob(inputBlobContainerUri, outputBlobContainerUri, null, StorageAuthenticationType.IdentityBased), 
  cancellationToken);

// Call an export job on the IoT Hub to retrieve all devices
JobProperties exportJob = 
await registryManager.ExportDevicesAsync(
    JobProperties.CreateForExportJob(outputBlobContainerUri, true, null, StorageAuthenticationType.IdentityBased),
    cancellationToken);
```


Führen Sie die folgenden Schritte aus, um diese regionsbeschränkte Version der Azure IoT SDKs mit Unterstützung virtueller Netzwerke für C#, Java und Node.js zu verwenden:

1. Erstellen Sie eine Umgebungsvariable namens `EnableStorageIdentity`, und legen Sie ihren Wert auf `1` fest.

2. Laden Sie das SDK herunter:  [Java](https://aka.ms/vnetjavasdk) | [C#](https://aka.ms/vnetcsharpsdk) | [Node.js](https://aka.ms/vnetnodesdk)
 
Laden Sie für Python die beschränkte Version von GitHub herunter.

1. Navigieren Sie zur [GitHub-Releaseseite](https://aka.ms/vnetpythonsdk).

2. Laden Sie die folgende Datei herunter, die sich unten auf der Releaseseite unter dem Header namens **Assets** befindet.
    > *azure_iot_hub-2.2.0_limited-py2.py3-none-any.whl*

3. Öffnen Sie ein Terminal, und navigieren Sie zum Ordner mit der heruntergeladenen Datei.

4. Führen Sie den folgenden Befehl aus, um das Python-Dienst-SDK mit Unterstützung für virtuelle Netzwerke zu installieren:
    > pip install ./azure_iot_hub-2.2.0_limited-py2.py3-none-any.whl


## <a name="next-steps"></a>Nächste Schritte

Verwenden Sie die unten angegebenen Links, um weitere Informationen zu IoT Hub-Features zu erhalten:

* [Nachrichtenrouting](./iot-hub-devguide-messages-d2c.md)
* [Hochladen von Dateien mit IoT Hub](./iot-hub-devguide-file-upload.md)
* [Importieren und Exportieren von IoT Hub-Geräteidentitäten per Massenvorgang](./iot-hub-bulk-identity-mgmt.md) 
