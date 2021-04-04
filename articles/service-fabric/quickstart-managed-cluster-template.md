---
title: Bereitstellen eines verwalteten Service Fabric-Clusters (Vorschauversion) mit Azure Resource Manager
description: Es wird beschrieben, wie Sie einen verwalteten Service Fabric-Cluster mit einer Azure Resource Manager-Vorlage erstellen.
ms.topic: quickstart
ms.date: 09/28/2020
ms.custom: references_regions
ms.openlocfilehash: 079e8e3e39bd306332abb43fa2ae1f8cc04e305b
ms.sourcegitcommit: f28ebb95ae9aaaff3f87d8388a09b41e0b3445b5
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/29/2021
ms.locfileid: "91410419"
---
# <a name="quickstart-deploy-a-service-fabric-managed-cluster-preview-with-an-azure-resource-manager-template"></a>Schnellstart: Bereitstellen eines verwalteten Service Fabric-Clusters (Vorschauversion) mit einer Azure Resource Manager-Vorlage

Verwaltete Service Fabric-Cluster sind eine Weiterentwicklung des Azure Service Fabric-Clusterressourcenmodells zur Optimierung Ihrer Bereitstellungs- und Clusterverwaltung. Bei einem verwalteten Service Fabric-Cluster handelt es sich um eine vollständig gekapselte Ressource, mit der Sie Service Fabric-Clusterressourcen einzeln bereitstellen können – anstatt alle zugrunde liegenden Ressourcen, aus denen ein Service Fabric-Cluster besteht. In diesem Artikel wird die Bereitstellung eines verwalteten Service Fabric-Clusters in Azure zu Testzwecken mithilfe einer Azure Resource Manager-Vorlage (ARM-Vorlage) beschrieben.

Der SKU-Cluster vom Typ „Basic“ mit drei Knoten, der in diesem Tutorial bereitgestellt wird, ist nur für Schulungszwecke bestimmt (und nicht für Produktionsworkloads). Weitere Informationen finden Sie unter [SKUs für verwaltete Service Fabric-Cluster](overview-managed-cluster.md#service-fabric-managed-cluster-skus).

## <a name="prerequisites"></a>Voraussetzungen

Bevor Sie mit dieser Schnellstartanleitung beginnen:

* Wenn Sie kein Azure-Abonnement besitzen, erstellen Sie ein [kostenloses Konto](https://azure.microsoft.com/free/?WT.mc_id=A261C142F).

## <a name="review-the-template"></a>Überprüfen der Vorlage

Die in dieser Schnellstartanleitung verwendete Vorlage stammt aus [Azure-Beispiele: Service Fabric-Clustervorlagen](https://github.com/Azure-Samples/service-fabric-cluster-templates/tree/master/SF-Managed-Basic-SKU-1-NT).

## <a name="create-a-client-certificate"></a>Erstellen eines Clientzertifikats

Für verwaltete Service Fabric-Cluster wird ein Clientzertifikat als Schlüssel für die Zugriffssteuerung verwendet. Wenn Sie bereits über ein Clientzertifikat verfügen, das Sie für die Zugriffssteuerung für Ihren Cluster verwenden möchten, können Sie diesen Schritt überspringen.

Befolgen Sie die Schritte unter [Festlegen und Abrufen eines Zertifikats aus Azure Key Vault](../key-vault/certificates/quick-create-portal.md), falls Sie ein neues Clientzertifikat erstellen müssen.

Notieren Sie sich den Zertifikatfingerabdruck, da Sie ihn im nächsten Schritt zum Bereitstellen der Vorlage benötigen.

## <a name="deploy-the-template"></a>Bereitstellen der Vorlage

1. Klicken Sie auf das folgende Bild, um sich bei Azure anzumelden und eine Vorlage zu öffnen.

      [![In Azure bereitstellen](../media/template-deployments/deploy-to-azure.svg)](https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure-Samples%2Fservice-fabric-cluster-templates%2Fmaster%2FSF-Managed-Basic-SKU-1-NT%2Fazuredeploy.json)

2. Wählen Sie die folgenden Werte aus, bzw. geben Sie sie ein.

    Geben Sie in dieser Schnellstartanleitung Ihre eigenen Werte für die folgenden Vorlagenparameter an:

    * **Abonnement**: Wählen Sie ein Azure-Abonnement aus.
    * **Ressourcengruppe**: Wählen Sie **Neu erstellen**. Geben Sie einen eindeutigen Namen für die Ressourcengruppe an, z. B. *myResourceGroup*, und wählen Sie dann **OK** aus.
    * **Standort**: Wählen Sie einen Standort aus, z. B. **eastus2**. Zu den unterstützten Regionen für verwaltete Service Fabric-Cluster (Vorschauversion) zählen `centraluseuap`, `eastus2euap`, `eastasia`, `northeurope`, `westcentralus` und `eastus2`.
    * **Clustername**: Geben Sie einen eindeutigen Namen für Ihren Cluster ein, z. B. *mysfcluster*.
    * **Administratorbenutzername**: Geben Sie einen Administratornamen ein, der auf den zugrunde liegenden VMs des Clusters für RDP verwendet werden soll.
    * **Administratorkennwort**: Geben Sie ein Administratorkennwort ein, das auf den zugrunde liegenden VMs des Clusters für RDP verwendet werden soll.
    * **Clientzertifikat-Fingerabdruck**: Geben Sie den Fingerabdruck des Clientzertifikats an, das Sie für den Zugriff auf Ihren Cluster verwenden möchten. Falls Sie nicht über ein Zertifikat verfügen, sollten Sie die Anleitungen unter [Festlegen und Abrufen eines Zertifikats](../key-vault/certificates/quick-create-portal.md) befolgen, um ein selbstsigniertes Zertifikat zu erstellen.
    * **Name des Knotentyps**: Geben Sie einen eindeutigen Namen für Ihren Knotentyp ein, z. B. *nt1*.
    * **Ich stimme den oben genannten Geschäftsbedingungen zu**: Aktivieren Sie dieses Kontrollkästchen, um zuzustimmen. 

3. Wählen Sie die Option **Kaufen**.

4. Es dauert einige Minuten, bis die Bereitstellung Ihres verwalteten Service Fabric-Clusters abgeschlossen ist. Warten Sie, bis die Bereitstellung erfolgreich abgeschlossen wurde, bevor Sie mit den nächsten Schritten fortfahren.

## <a name="validate-the-deployment"></a>Überprüfen der Bereitstellung

### <a name="review-deployed-resources"></a>Überprüfen der bereitgestellten Ressourcen

Suchen Sie nach Abschluss der Bereitstellung in der Ausgabe nach dem Service Fabric Explorer-Wert, und öffnen Sie die Adresse in einem Webbrowser, um Ihren Cluster im Service Fabric Explorer anzuzeigen. Wenn Sie zur Eingabe eines Zertifikats aufgefordert werden, verwenden Sie das Zertifikat, für das der Clientfingerabdruck in der Vorlage bereitgestellt wurde.

> [!NOTE]
> Sie finden die Ausgabe der Bereitstellung im Azure-Portal auf der Registerkarte „Bereitstellungen von Ressourcengruppen“.

## <a name="clean-up-resources"></a>Bereinigen von Ressourcen

Löschen Sie die Ressourcengruppe für Ihren verwalteten Service Fabric-Cluster, wenn Sie sie nicht mehr benötigen. So löschen Sie die Ressourcengruppe über das Portal:

1. Geben Sie den Namen Ihrer Ressourcengruppe in das *Suchfeld* am oberen Rand des Portals ein. Klicken Sie in den Suchergebnissen auf die Ressourcengruppe aus dieser Schnellstartanleitung.
2. Wählen Sie die Option **Ressourcengruppe löschen**.
3. Geben Sie im Feld **GEBEN SIE DEN RESSOURCENGRUPPENNAMEN EIN:** den Namen der Ressourcengruppe ein, und klicken Sie anschließend auf **Löschen**.

## <a name="next-steps"></a>Nächste Schritte

In dieser Schnellstartanleitung haben Sie einen verwalteten Service Fabric-Cluster bereitgestellt. Weitere Informationen zum Skalieren eines Clusters finden Sie unter:

> [!div class="nextstepaction"]
> [Aufskalieren eines verwalteten Service Fabric-Clusters](tutorial-managed-cluster-scale.md)
