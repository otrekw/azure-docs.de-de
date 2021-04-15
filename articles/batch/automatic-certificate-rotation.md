---
title: Aktivieren der automatischen Zertifikatrotation in einem Batch-Pool
description: Sie können einen Batch-Pool mit einer verwalteten Identität und einem Zertifikat erstellen, das automatisch erneuert wird.
ms.topic: conceptual
ms.date: 03/23/2021
ms.custom: references_regions
ms.openlocfilehash: e8bea49b2980deb8f20258ab7ea5619ece8cd2bf
ms.sourcegitcommit: f28ebb95ae9aaaff3f87d8388a09b41e0b3445b5
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/30/2021
ms.locfileid: "104962303"
---
# <a name="enable-automatic-certificate-rotation-in-a-batch-pool"></a>Aktivieren der automatischen Zertifikatrotation in einem Batch-Pool

 Sie können einen Batch-Pool mit einem Zertifikat erstellen, das automatisch erneuert wird. Zu diesem Zweck muss der Pool mit einer [benutzerseitig zugewiesenen verwalteten Identität](managed-identity-pools.md) erstellt werden, die in [Azure Key Vault](../key-vault/general/overview.md) auf das Zertifikat zugreifen kann.

> [!IMPORTANT]
> Die Unterstützung von Azure Batch-Pools mit benutzerseitig zugewiesenen verwalteten Identitäten befindet sich derzeit für die folgenden Regionen in der öffentlichen Vorschau: USA, Westen 2; USA, Süden-Mitte; USA, Osten; US Gov Arizona; US Gov Virginia.
> Diese Vorschauversion wird ohne Vereinbarung zum Servicelevel bereitgestellt und ist nicht für Produktionsworkloads vorgesehen. Manche Features werden möglicherweise nicht unterstützt oder sind nur eingeschränkt verwendbar.
> Weitere Informationen finden Sie unter [Zusätzliche Nutzungsbestimmungen für Microsoft Azure-Vorschauen](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).

## <a name="create-a-user-assigned-identity"></a>Erstellen einer benutzerseitig zugewiesenen Identität

[Erstellen Sie zunächst Ihre benutzerseitig zugewiesene verwaltete Identität](../active-directory/managed-identities-azure-resources/how-to-manage-ua-identity-portal.md#create-a-user-assigned-managed-identity) im selben Mandanten wie Ihr Azure Batch-Konto. Diese verwaltete Identität muss sich nicht in derselben Ressourcengruppe oder unter demselben Abonnement befindet.

Notieren Sie sich auf jeden Fall die **Client-ID** der benutzerseitig zugewiesenen verwalteten Identität. Sie benötigen diesen Wert später.

:::image type="content" source="media/automatic-certificate-rotation/client-id.png" alt-text="Screenshot der Client-ID einer benutzerseitig zugewiesenen verwalteten Identität im Azure-Portal":::

## <a name="create-your-certificate"></a>Erstellen eines Zertifikats

Als Nächstes müssen Sie ein Zertifikat erstellen und es in Azure Key Vault hinzufügen. Wenn Sie noch keinen Schlüsseltresor erstellt haben, müssen Sie dies zunächst tun. Anweisungen finden Sie unter [Schnellstart: Festlegen und Abrufen eines Zertifikats aus Azure Key Vault über das Azure-Portal](../key-vault/certificates/quick-create-portal.md).

Wenn Sie Ihr Zertifikat erstellen, stellen Sie sicher, dass Sie den **Aktionstyp „Lebensdauer“** auf „Automatisch verlängern“ festlegen, und geben Sie die Anzahl von Tagen an, nach der das Zertifikat verlängert werden soll.

:::image type="content" source="media/automatic-certificate-rotation/certificate.png" alt-text="Screenshot des Zertifikaterstellungsbildschirms im Azure-Portal":::

Notieren Sie sich nach dem Erstellen des Zertifikats den **geheimen Bezeichner**. Sie benötigen diesen Wert später.

:::image type="content" source="media/automatic-certificate-rotation/secret-identifier.png" alt-text="Screenshot des geheimen Bezeichner eines Zertifikats":::

## <a name="add-an-access-policy-in-azure-key-vault"></a>Hinzufügen einer Zugriffsrichtlinie in Azure Key Vault

Weisen Sie in Ihrem Schlüsseltresor eine Key Vault-Zugriffsrichtlinie zu, mit der die benutzerseitig zugewiesene verwaltete Identität auf Geheimnisse und Zertifikate zugreifen kann. Detaillierte Anweisungen finden Sie unter [Zuweisen einer Key Vault-Zugriffsrichtlinie über das Azure-Portal](../key-vault/general/assign-access-policy-portal.md).

## <a name="create-a-batch-pool-with-a-user-assigned-managed-identity"></a>Erstellen eines Batch-Pools mit benutzerseitig zugewiesenen verwalteten Identitäten

Erstellen Sie einen Batch-Pool mit Ihrer verwalteten Identität, indem Sie die [Batch-Verwaltungsbibliothek von .NET](/dotnet/api/overview/azure/batch#management-library) verwenden. Weitere Informationen finden Sie unter [Konfigurieren verwalteter Identitäten in Azure Batch-Pools](managed-identity-pools.md).

Im folgenden Beispiel wird die Batch Management-Rest-API verwendet, um einen Pool zu erstellen. Stellen Sie sicher, dass Sie den **geheimen Bezeichner** für `observedCertificates` und die **Client-ID** der verwalteten Identität für `msiClientId` verwenden und die folgenden Beispieldaten ersetzen.

REST-API-URI

```http
PUT https://management.azure.com/subscriptions/<subscriptionid>/resourceGroups/<resourcegroupName>/providers/Microsoft.Batch/batchAccounts/<batchaccountname>/pools/<poolname>?api-version=2021-01-01
```

Anforderungstext

```json
{
    "name": "test2",
    "type": "Microsoft.Batch/batchAccounts/pools",
    "properties": {
        "vmSize": "STANDARD_DS2_V2",
        "taskSchedulingPolicy": {
            "nodeFillType": "Pack"
        },
        "deploymentConfiguration": {
            "virtualMachineConfiguration": {
                "imageReference": {
                    "publisher": "canonical",
                    "offer": "ubuntuserver",
                    "sku": "18.04-lts",
                    "version": "latest"
                },
                "nodeAgentSkuId": "batch.node.ubuntu 18.04",
                "extensions": [
                    {
                        "name": "KVExtensions",
                        "type": "KeyVaultForLinux",
                        "publisher": "Microsoft.Azure.KeyVault",
                        "typeHandlerVersion": "1.0",
                        "autoUpgradeMinorVersion": true,
                        "settings": {
                            "secretsManagementSettings": {
                                "pollingIntervalInS": "300",
                                "certificateStoreLocation": "/var/lib/waagent/Microsoft.Azure.KeyVault",
                                "requireInitialSync": true,
                                "observedCertificates": [
                                    "https://testkvwestus2s.vault.azure.net/secrets/authcertforumatesting/8f5f3f491afd48cb99286ba2aacd39af"
                                ]
                            },
                            "authenticationSettings": {
                                "msiEndpoint": "http://169.254.169.254/metadata/identity",
                                "msiClientId": "b9f6dd56-d2d6-4967-99d7-8062d56fd84c"
                            }  }, "protectedSettings":{}
                    }                ]            }
        },
        "scaleSettings": {
            "fixedScale": {
                "targetDedicatedNodes": 1,
                "resizeTimeout": "PT15M"
            }
        },
    },
    "identity": {
        "type": "UserAssigned",
        "userAssignedIdentities": {
            "/subscriptions/042998e4-36dc-4b7d-8ce3-a7a2c4877d33/resourceGroups/ACR/providers/Microsoft.ManagedIdentity/userAssignedIdentities/testumaforpools": {}
        }
    }
}

```

## <a name="validate-the-certificate"></a>Überprüfen des Zertifikats

Melden Sie sich beim Serverknoten an, um zu überprüfen, ob das Zertifikat erfolgreich bereitgestellt wurde. Die Ausgabe sollte etwa folgendermaßen aussehen:

```
root@74773db5fe1b42ab9a4b6cf679d929da000000:/var/lib/waagent/Microsoft.Azure.KeyVault.KeyVaultForLinux-1.0.1363.13/status# cat 1.status
[{"status":{"code":0,"formattedMessage":{"lang":"en","message":"Successfully started Key Vault extension service. 2021-03-03T23:12:23Z"},"operation":"Service start.","status":"success"},"timestampUTC":"2021-03-03T23:12:23Z","version":"1.0"}]root@74773db5fe1b42ab9a4b6cf679d929da000000:/var/lib/waagent/Microsoft.Azure.KeyVault.KeyVaultForLinux-1.0.1363.13/status#
```

## <a name="next-steps"></a>Nächste Schritte

- Weitere Informationen zu [verwalteten Identitäten für Azure-Ressourcen](../active-directory/managed-identities-azure-resources/overview.md).
- Erfahren Sie, wie Sie [vom Kunden verwaltete Schlüssel mit benutzerseitig verwalteten Identitäten](batch-customer-managed-key.md) verwenden.
