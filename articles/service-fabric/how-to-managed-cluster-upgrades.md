---
title: Upgrade von verwalteten Azure Service Fabric-Clustern
description: Erfahren Sie mehr über die Optionen zum Aktualisieren Ihres verwalteten Azure Service Fabric-Clusters.
ms.topic: how-to
ms.date: 05/10/2021
ms.openlocfilehash: 478b39a6222906c793d826ab69edeeaddbb096bf
ms.sourcegitcommit: c072eefdba1fc1f582005cdd549218863d1e149e
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/10/2021
ms.locfileid: "111960997"
---
# <a name="manage-service-fabric-managed-cluster-upgrades"></a>Verwalten von Service Fabric-Clusterupgrades

Ein Azure Service Fabric-Cluster ist eine Ressource, die Sie besitzen, die jedoch teilweise von Microsoft verwaltet wird. Hier erfahren Sie, wie Sie den Zeitpunkt und die Art der Aktualisierung Ihres Azure Service Fabric-Clusters durch Microsoft verwalten.

## <a name="set-upgrade-mode"></a>Festlegen des Upgrademodus

Verwaltete Azure Service Fabric-Cluster werden standardmäßig so festgelegt, dass sie automatische Service Fabric-Upgrades erhalten, wenn sie von Microsoft mithilfe einer [Wave-Bereitstellungsstrategie](#wave-deployment-for-automatic-upgrades) veröffentlicht werden. Alternativ können Sie Upgrades im manuellen Modus einrichten, indem Sie diese aus einer Liste der derzeit unterstützten Versionen auswählen. Dies kann entweder im Azure-Portal über das Steuerelement *Fabric-Upgrades* oder die `ClusterUpgradeMode`-Einstellung in der Vorlage für die Clusterbereitstellung erfolgen.

## <a name="wave-deployment-for-automatic-upgrades"></a>Bereitstellung in Zyklen für automatische Upgrades

Mit der Bereitstellung in Zyklen können Sie eine Pipeline erstellen, mit der Sie Ihre Test-, Bereitstellungs- und Produktionscluster nacheinander, durch die integrierte „Bake Time“ getrennt aktualisieren können, um anstehende Service Fabric-Versionen zu validieren, bevor die Produktionscluster aktualisiert werden.

>HINWEIS: Standardmäßig werden Cluster auf Wave 0 festgelegt.

Zum Auswählen der zyklischen Bereitstellung für das automatische Upgrade legen Sie zunächst fest, welcher Zyklus Ihrem Cluster zugewiesen werden soll:

* **Wave 0** (`Wave0`): Cluster werden aktualisiert, sobald ein neuer Service Fabric-Build freigegeben wird.
* **Wave 1** (`Wave1`): Cluster werden nach Wave 0 aktualisiert, um Baking-Zeit zu ermöglichen. Dies tritt nach mindestens 7 Tagen nach Wave 0 auf.
* **Wave 2** (`Wave2`): Cluster werden zuletzt aktualisiert, um weitere Baking-Zeit zu ermöglichen. Dies tritt nach mindestens 14 Tagen nach Wave 0 auf.

## <a name="set-the-wave-for-your-cluster"></a>Festlegen des Zyklus für Ihren Cluster

Sie können Ihren Cluster im Azure-Portal entweder über das Steuerelement *Fabric-Upgrades* oder die `ClusterUpgradeMode` Einstellung in Ihrer Clusterbereitstellungsvorlage auf eine der verfügbaren Wellen festlegen.

### <a name="azure-portal"></a>Azure-Portal

Im Azure-Portal wählen Sie beim Erstellen eines neuen Service Fabric-Clusters zwischen den verfügbaren automatischen Zyklen aus.

:::image type="content" source="./media/how-to-managed-cluster-upgrades/portal-new-cluster-upgrade-waves-setting.png" alt-text="Wählen Sie beim Erstellen eines neuen Clusters im Azure-Portal unter Optionen „Erweitert“ zwischen verschiedenen verfügbaren Zyklen":::.

Sie können auch im Abschnitt **Fabric-Upgrades** einer vorhandenen Clusterressource zwischen automatischen und manuellen Upgrades umschalten.

:::image type="content" source="./media/how-to-managed-cluster-upgrades/manage-upgrade-wave-settings.png" alt-text="Wählen Sie im Azure-Portal im Abschnitt „Fabric-Upgrades“ Ihrer Clusterressource zwischen verschiedenen automatischen Zyklen":::.

### <a name="resource-manager-template"></a>Resource Manager-Vorlage

Wenn Sie den Clusterupgrademodus mithilfe einer Resource Manager-Vorlage ändern möchten, geben Sie für die `ClusterUpgradeMode`-Eigenschaft der Ressourcendefinition *Microsoft.ServiceFabric/clusters* entweder *Automatisch* oder *Manuell* an. Wenn Sie manuelle Upgrades auswählen, legen Sie auch `clusterCodeVersion` auf eine zurzeit [unterstützte Fabric-Version](#query-for-supported-cluster-versions) fest.

#### <a name="manual-upgrade"></a>Manuelles Upgrade

```json
{
"apiVersion": "2021-05-01",
"type": "Microsoft.ServiceFabric/managedClusters",
"properties": {
        "ClusterUpgradeMode": "Manual",
        "ClusterCodeVersion": "7.2.457.9590"
        }
}
```

Nach erfolgreicher Bereitstellung der Vorlage werden Änderungen am Clusterupgrademodus angewendet. Wenn sich der Cluster im manuellen Modus befindet, wird das Clusterupgrade automatisch gestartet.

Bei dem Upgrade werden die [Clusterintegritätsrichtlinien](./service-fabric-health-introduction.md#health-policies) (eine Kombination aus Knotenintegrität und Integrität aller im Cluster ausgeführten Anwendungen) berücksichtigt. Wenn die Clusterintegritätsrichtlinien nicht erfüllt sind, wird das Upgrade zurückgesetzt.

Beheben Sie die Probleme, die zu dem Rollback geführt haben, und initiieren Sie das Upgrade erneut, indem Sie die gleichen Schritte ausführen wie zuvor.

#### <a name="automatic-upgrade-with-wave-deployment"></a>Automatisches Upgrade mit Wave-Bereitstellung

Um automatische Upgrades und die Wave-Bereitstellung zu konfigurieren, wird einfach „`ClusterUpgradeMode` Hinzufügen/Validieren“ auf `Automatic` festgelegt und die `upgradeWave`-Eigenschaft wird mit einem der Wave-Werte definiert, die oben in Ihrer Resource Manager-Vorlage aufgeführt sind.

```json
{
"apiVersion": "2021-05-01",
"type": "Microsoft.ServiceFabric/managedClusters",
"properties": {
        "ClusterUpgradeMode": "Automatic",
        "upgradeWave": "Wave1",
        }  
}
```

Nachdem Sie die aktualisierte Vorlage bereitgestellt haben, wird Ihr Cluster im angegebenen Zyklus für den nächsten Upgradezeitraum und danach registriert.

## <a name="custom-policies-for-manual-upgrades"></a>Benutzerdefinierte Richtlinien für manuelle Upgrades

Sie können benutzerdefinierte Integritätsrichtlinien für manuelle Clusterupgrades angeben. Diese Richtlinien werden jedes Mal angewendet, wenn Sie eine neue Runtimeversion auswählen, wodurch im System der Start des Upgrade Ihres Clusters ausgelöst wird. Wenn Sie die Richtlinien nicht überschreiben, werden die Standardwerte verwendet.

Sie können die benutzerdefinierten Integritätsrichtlinien angeben oder die aktuellen Einstellungen im Abschnitt **Fabric-Upgrades** Ihrer Clusterressource im Azure-Portal überprüfen, indem Sie die Option *Benutzerdefiniert* für **Upgraderichtlinie** auswählen.

:::image type="content" source="./media/service-fabric-cluster-upgrade/custom-upgrade-policy.png" alt-text="Wählen Sie die Upgraderichtlinienoption „Benutzerdefiniert“ im Abschnitt „Fabric-Upgrades“ Ihrer Clusterressource im Azure-Portal aus, um während des Upgrades benutzerdefinierte Integritätsrichtlinien festzulegen.":::

## <a name="query-for-supported-cluster-versions"></a>Abfrage nach unterstützten Clusterversionen

Mit der [Azure-REST-API](/rest/api/azure/) können Sie alle verfügbaren Service Fabric-Runtimeversionen ([clusterVersions](/rest/api/servicefabric/sfrp-api-clusterversions_list)) auflisten, die für den angegebenen Speicherort und Ihr Abonnement verfügbar sind.

Sie können auch auf [Service Fabric-Versionen](service-fabric-versions.md) verweisen, um weitere Informationen zu unterstützten Versionen und Betriebssystemen zu erhalten.

```REST
GET https://<endpoint>/subscriptions/{{subscriptionId}}/providers/Microsoft.ServiceFabric/locations/{{location}}/clusterVersions?api-version=2018-02-01

"value": [
  {
    "id": "subscriptions/########-####-####-####-############/providers/Microsoft.ServiceFabric/environments/Windows/clusterVersions/5.0.1427.9490",
    "name": "5.0.1427.9490",
    "type": "Microsoft.ServiceFabric/environments/clusterVersions",
    "properties": {
      "codeVersion": "5.0.1427.9490",
      "supportExpiryUtc": "2016-11-26T23:59:59.9999999",
      "environment": "Windows"
    }
  },
  {
    "id": "subscriptions/########-####-####-####-############/providers/Microsoft.ServiceFabric/environments/Windows/clusterVersions/4.0.1427.9490",
    "name": "5.1.1427.9490",
    "type": " Microsoft.ServiceFabric/environments/clusterVersions",
    "properties": {
      "codeVersion": "5.1.1427.9490",
      "supportExpiryUtc": "9999-12-31T23:59:59.9999999",
      "environment": "Windows"
    }
  },
  {
    "id": "subscriptions/########-####-####-####-############/providers/Microsoft.ServiceFabric/environments/Windows/clusterVersions/4.4.1427.9490",
    "name": "4.4.1427.9490",
    "type": " Microsoft.ServiceFabric/environments/clusterVersions",
    "properties": {
      "codeVersion": "4.4.1427.9490",
      "supportExpiryUtc": "9999-12-31T23:59:59.9999999",
      "environment": "Linux"
    }
  }
]
}
```

`supportExpiryUtc` in der Ausgabe meldet, wenn ein bestimmtes Release abläuft oder abgelaufen ist. Die neueste Version besitzt anstelle eines gültigen Datums den Wert *9999-12-31T23:59:59.9999999*, was bedeutet, dass noch kein Ablaufdatum feststeht.

## <a name="next-steps"></a>Nächste Schritte

* [Anpassen Ihrer Konfiguration für verwaltete Service Fabric-Cluster](how-to-managed-cluster-configuration.md)
* Machen Sie sich mit [Anwendungsupgrades](service-fabric-application-upgrade.md)

<!--Image references-->
[CertificateUpgrade]: ./media/service-fabric-cluster-upgrade/CertificateUpgrade2.png
[AddingProbes]: ./media/service-fabric-cluster-upgrade/addingProbes2.PNG
[AddingLBRules]: ./media/service-fabric-cluster-upgrade/addingLBRules.png
[Upgrade-Wave-Settings]: ./media/service-fabric-cluster-upgrade/manage-upgrade-wave-settings.png
[ARMUpgradeMode]: ./media/service-fabric-cluster-upgrade/ARMUpgradeMode.PNG
[Create_Manualmode]: ./media/service-fabric-cluster-upgrade/Create_Manualmode.PNG
[Manage_Automaticmode]: ./media/service-fabric-cluster-upgrade/Manage_Automaticmode.PNG