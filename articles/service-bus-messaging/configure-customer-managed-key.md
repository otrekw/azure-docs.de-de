---
title: Konfigurieren Ihres eigenen Schlüssels zum Verschlüsseln ruhender Azure Service Bus-Daten
description: Dieser Artikel enthält Informationen dazu, wie Sie einen eigenen Schlüssel für die Verschlüsselung ruhender Azure Service Bus-Daten konfigurieren.
services: service-bus-messaging
ms.service: service-bus
documentationcenter: ''
author: axisc
ms.topic: conceptual
ms.date: 11/15/2019
ms.author: aschhab
ms.openlocfilehash: 6d20d4031f0ed4d1be4dddf9e33946251d6dd523
ms.sourcegitcommit: 3eb0cc8091c8e4ae4d537051c3265b92427537fe
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/11/2020
ms.locfileid: "75903317"
---
# <a name="configure-customer-managed-keys-for-encrypting-azure-service-bus-data-at-rest-by-using-the-azure-portal"></a>Konfigurieren von kundenseitig verwalteten Schlüsseln für die Verschlüsselung ruhender Azure Service Bus-Daten mithilfe des Azure-Portals
Azure Service Bus Premium ermöglicht die Verschlüsselung ruhender Daten mit der Azure-Speicherdienstverschlüsselung (Azure Storage Service Encryption, SSE). Service Bus Premium verwendet Azure Storage zum Speichern der Daten. Standardmäßig werden alle mit Azure Storage gespeicherten Daten durch von Microsoft verwaltete Schlüssel verschlüsselt. 

## <a name="overview"></a>Übersicht
Azure Service Bus unterstützt jetzt die Option zum Verschlüsseln ruhender Daten mit von Microsoft oder vom Kunden verwalteten Schlüsseln (Bring Your Own Key, BYOK). Mit dieser Funktion können Sie kundenseitig verwaltete Schlüssel, die zum Verschlüsseln ruhender Azure Service Bus-Daten verwendet werden, erstellen, rotieren, deaktivieren und den Zugriff darauf widerrufen.

Die Aktivierung der BYOK-Funktion ist ein einmaliger Setupvorgang für Ihren Namespace.

> [!NOTE]
> Für die dienstseitige Verschlüsselung bestehen einige Einschränkungen für vom Kunden verwaltete Schlüssel. 
>   * Dieses Feature wird auf der [Azure Service Bus Premium](service-bus-premium-messaging.md)-Ebene unterstützt. Es kann nicht für Service Bus-Namespaces auf Standardebene aktiviert werden.
>   * Die Verschlüsselung kann nur für neue oder leere Namespaces aktiviert werden. Wenn der Namespace Daten enthält, tritt beim Verschlüsselungsvorgang ein Fehler auf.

Verwenden Sie Azure Key Vault, um Ihre Schlüssel zu verwalten und die Schlüsselverwendung zu überwachen. Sie können entweder Ihre eigenen Schlüssel erstellen und in einem Schlüsseltresor speichern oder mit den Azure Key Vault-APIs Schlüssel generieren. Weitere Informationen zum Azure-Schlüsseltresor finden Sie unter [Was ist der Azure-Schlüsseltresor?](../key-vault/key-vault-overview.md)

In diesem Artikel erfahren Sie, wie Sie einen Schlüsseltresor mit kundenseitig verwalteten Schlüsseln mithilfe des Azure-Portals konfigurieren. Informationen zum Erstellen eines Schlüsseltresors über das Azure-Portal finden Sie unter [Schnellstart: Festlegen eines Geheimnisses und Abrufen des Geheimnisses aus Azure Key Vault mithilfe des Azure-Portals](../key-vault/quick-create-portal.md).

> [!IMPORTANT]
> Für die Verwendung kundenseitig verwalteter Schlüssel mit Azure Service Bus müssen für den Schlüsseltresor zwei Eigenschaften konfiguriert werden. Sie lauten wie folgt:  **Vorläufiges Löschen** und **Do Not Purge** (Nicht bereinigen). Diese Eigenschaften sind standardmäßig aktiviert, wenn Sie im Azure-Portal einen neuen Schlüsseltresor erstellen. Wenn Sie diese Eigenschaften jedoch für einen vorhandenen Schlüsseltresor aktivieren möchten, müssen Sie PowerShell oder die Azure-Befehlszeilenschnittstelle verwenden.

## <a name="enable-customer-managed-keys"></a>Aktivieren von vom Kunden verwalteten Schlüsseln
Um vom Kunden verwaltete Schlüssel im Azure-Portal zu aktivieren, gehen Sie folgendermaßen vor:

1. Navigieren Sie zu Ihrem Service Bus Premium-Namespace.
2. Wählen Sie auf der Seite **Einstellungen** des Service Bus-Namespace die Option **Verschlüsselung** aus.
3. Wählen Sie die **Verschlüsselung im Ruhezustand mit kundenseitig verwalteten Schlüsseln**  wie in der folgenden Abbildung gezeigt aus.

    ![Kundenseitig verwaltete Schlüssel aktivieren](./media/configure-customer-managed-key/enable-customer-managed-key.png)


## <a name="set-up-a-key-vault-with-keys"></a>Einrichten eines Schlüsseltresors mit Schlüsseln

Nachdem Sie kundenseitig verwaltete Schlüssel aktiviert haben, müssen Sie den kundenseitig verwalteten Schlüssel Ihrem Azure Service Bus-Namespace zuordnen. Service Bus unterstützt nur Azure Key Vault. Wenn Sie die Option **Verschlüsselung mit kundenseitig verwaltetem Schlüssel** im vorherigen Abschnitt aktivieren, muss der Schlüssel in Azure Key Vault importiert werden. Außerdem müssen die Eigenschaften **Vorläufiges Löschen** und **Do Not Purge** (Nicht bereinigen) für den Schlüssel konfiguriert sein. Diese Einstellungen können mithilfe von [PowerShell](../key-vault/key-vault-soft-delete-powershell.md) oder der [CLI](../key-vault/key-vault-soft-delete-cli.md#enabling-purge-protection) konfiguriert werden.

1. Befolgen Sie zum Erstellen eines neuen Schlüsseltresors die [Schnellstartanleitung](../key-vault/key-vault-overview.md) für Azure Key Vault. Weitere Informationen zum Importieren vorhandener Schlüssel finden Sie unter [Informationen zu Schlüsseln, Geheimnissen und Zertifikaten](../key-vault/about-keys-secrets-and-certificates.md).
1. Verwenden Sie den Befehl [az keyvault create](/cli/azure/keyvault?view=azure-cli-latest#az-keyvault-create), um beim Erstellen eines Tresors sowohl das vorläufige Löschen als auch den Bereinigungsschutz zu aktivieren.

    ```azurecli-interactive
    az keyvault create --name contoso-SB-BYOK-keyvault --resource-group ContosoRG --location westus --enable-soft-delete true --enable-purge-protection true
    ```    
1. Verwenden Sie den Befehl [az keyvault update](/cli/azure/keyvault?view=azure-cli-latest#az-keyvault-update), um einem vorhandenen Tresor (für den bereits das vorläufige Löschen aktiviert ist) den Bereinigungsschutz hinzuzufügen.

    ```azurecli-interactive
    az keyvault update --name contoso-SB-BYOK-keyvault --resource-group ContosoRG --enable-purge-protection true
    ```
1. Führen Sie diese Schritte durch, um Schlüssel zu erstellen:
    1. Wählen Sie zum Erstellen eines neuen Schlüssels **Generieren/Importieren** im Menü **Schlüssel** unter **Einstellungen** aus.
        
        ![Auswählen der Schaltfläche „Generate/Import“ (Generieren/Importieren)](./media/configure-customer-managed-key/select-generate-import.png)

    1. Legen Sie **Optionen** auf **Generieren** fest, und benennen Sie den Schlüssel.

        ![Erstellen eines Schlüssels](./media/configure-customer-managed-key/create-key.png) 

    1. Sie können diesen Schlüssel jetzt in der Dropdownliste auswählen, um ihn dem Service Bus-Namespace zum Verschlüsseln zuzuordnen. 

        ![Auswählen des Schlüssels aus dem Schlüsseltresor](./media/configure-customer-managed-key/select-key-from-key-vault.png)
        > [!NOTE]
        > Um Redundanz zu gewährleisten, können Sie bis zu drei Schlüssel hinzufügen. Wenn einer der Schlüssel abgelaufen ist oder nicht darauf zugegriffen werden kann, werden die anderen Schlüssel für die Verschlüsselung verwendet.
        
    1. Geben Sie die Details für den Schlüssel ein, und klicken Sie auf **Auswählen**. Dadurch wird die Verschlüsselung von ruhenden Daten im Namespace mit einem kundenseitig verwalteten Schlüssel ermöglicht. 


> [!IMPORTANT]
> Wenn Sie vom Kunden verwaltete Schlüssel zusammen mit der georedundanten Notfallwiederherstellung verwenden möchten, überprüfen Sie Folgendes: 
>
> Um die Verschlüsselung im Ruhezustand mit vom Kunden verwalteten Schlüsseln zu aktivieren, wird eine [Zugriffsrichtlinie](../key-vault/key-vault-secure-your-key-vault.md) für die von Service Bus verwaltete Identität in der angegebenen Azure Key Vault-Instanz eingerichtet. Dies gewährleistet den kontrollierten Zugriff auf Azure Key Vault aus dem Azure Service Bus-Namespace.
>
> Gehen Sie daher folgendermaßen vor:
> 
>   * Wenn die [georedundante Notfallwiederherstellung](service-bus-geo-dr.md) bereits für den Service Bus-Namespace aktiviert ist und Sie den vom Kunden verwalteten Schlüssel aktivieren möchten: 
>     * Heben Sie die Kopplung auf.
>     * [Richten Sie die Zugriffsrichtlinien](../key-vault/managed-identity.md) für die verwaltete Identität sowohl für den primären als auch für den sekundären Namespace auf den Schlüsseltresor ein.
>     * Richten Sie die Verschlüsselung für den primären Namespace ein.
>     * Stellen Sie die Kopplung des primären und des sekundären Namespace wieder her.
> 
>   * Wenn Sie in einem Service Bus-Namespace, in dem bereits vom Kunden verwaltete Schlüssel eingerichtet sind, georedundante Notfallwiederherstellung aktivieren möchten:
>     * [Richten Sie die Zugriffsrichtlinien](../key-vault/managed-identity.md) für die verwaltete Identität für den sekundären Namespace auf den Schlüsseltresor ein.
>     * Koppeln Sie den primären und den sekundären Namespace.


## <a name="rotate-your-encryption-keys"></a>Rotieren der Verschlüsselungsschlüssel

Sie können Ihren Schlüssel im Schlüsseltresor mit dem Rotationsmechanismus von Azure Key Vault rotieren. Weitere Informationen finden Sie unter [Einrichten von Azure Key Vault mit Schlüsselrotation und Überwachung](../key-vault/key-vault-key-rotation-log-monitoring.md). Aktivierungs- und Ablaufdaten können auch festgelegt werden, um die Schlüsselrotation zu automatisieren. Der Service Bus-Dienst erkennt neue Schlüsselversionen und beginnt automatisch mit ihrer Verwendung.

## <a name="revoke-access-to-keys"></a>Widerrufen des Zugriffs auf Schlüssel

Wenn Sie den Zugriff auf die Verschlüsselungsschlüssel widerrufen, werden die Daten dadurch nicht aus Service Bus gelöscht. Es kann jedoch nicht über den Event Hubs-Namespace auf die Daten zugegriffen werden. Sie können den Verschlüsselungsschlüssel mithilfe von Zugriffsrichtlinien oder durch das Löschen des Schlüssels widerrufen. Unter [Sicherer Zugriff auf einen Schlüsseltresor](../key-vault/key-vault-secure-your-key-vault.md) erfahren Sie mehr über Zugriffsrichtlinien und das Sichern Ihres Schlüsseltresors.

Nachdem der Verschlüsselungsschlüssel widerrufen wurde, funktioniert der Service Bus-Dienst im verschlüsselten Namespace nicht mehr. Wenn der Zugriff auf den Schlüssel aktiviert oder der gelöschte Schlüssel wiederhergestellt wird, wählt der Service Bus-Dienst den Schlüssel aus, sodass Sie aus dem verschlüsselten Service Bus-Namespace auf die Daten zugreifen können.

## <a name="next-steps"></a>Nächste Schritte
Weitere Informationen finden Sie in folgenden Artikeln:
- [Übersicht über Service Bus](service-bus-messaging-overview.md)
- [Übersicht über Key Vault](../key-vault/key-vault-overview.md)


