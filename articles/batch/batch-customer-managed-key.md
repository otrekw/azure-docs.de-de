---
title: Konfigurieren von kundenseitig verwalteten Schlüsseln für Ihr Azure Batch-Konto mit Azure Key Vault und Verwaltete Identität
description: Es wird beschrieben, wie Sie Batch-Daten mit kundenseitig verwalteten Schlüsseln verschlüsseln.
author: pkshultz
ms.topic: how-to
ms.date: 02/11/2021
ms.author: peshultz
ms.openlocfilehash: d3f10436b95aaeb5eb35a873c2a3862c1492bd47
ms.sourcegitcommit: f28ebb95ae9aaaff3f87d8388a09b41e0b3445b5
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/29/2021
ms.locfileid: "100385063"
---
# <a name="configure-customer-managed-keys-for-your-azure-batch-account-with-azure-key-vault-and-managed-identity"></a>Konfigurieren von kundenseitig verwalteten Schlüsseln für Ihr Azure Batch-Konto mit Azure Key Vault und Verwaltete Identität

Standardmäßig werden von Azure Batch plattformseitig verwaltete Schlüssel verwendet, um alle im Azure Batch-Dienst gespeicherten Kundendaten zu verschlüsseln, wie z. B. Zertifikate, Auftrags-/Taskmetadaten. Optional können Sie eigene Schlüssel (d. h. kundenseitig verwaltete Schlüssel) verwenden, um die in Azure Batch gespeicherten Daten zu verschlüsseln.

Die von Ihnen angegebenen Schlüssel müssen in [Azure Key Vault](../key-vault/general/basic-concepts.md) generiert werden, und der Zugriff darauf muss mit [verwalteten Identitäten für Azure-Ressourcen](../active-directory/managed-identities-azure-resources/overview.md) erfolgen.

Es gibt zwei Arten von verwalteten Identitäten: [*systemseitig* und *benutzerseitig*](../active-directory/managed-identities-azure-resources/overview.md#managed-identity-types) zugewiesene Identitäten.

Sie können Ihr Batch-Konto mit einer systemseitig zugewiesenen verwalteten Identität erstellen oder eine separate benutzerseitig zugewiesene verwaltete Identität erstellen, für die Zugriff auf die kundenseitig verwalteten Schlüssel besteht. Sehen Sie sich die [Vergleichstabelle](../active-directory/managed-identities-azure-resources/overview.md#managed-identity-types) an, um sich mit den Unterschieden vertraut zu machen und zu ermitteln, welche Option für Ihre Lösung am besten geeignet ist. Wenn Sie beispielsweise dieselbe verwaltete Identität nutzen möchten, um auf mehrere Azure-Ressourcen zuzugreifen, benötigen Sie eine benutzerseitig zugewiesene verwaltete Identität. Andernfalls reicht ggf. eine systemseitig zugewiesene verwaltete Identität aus, die Ihrem Batch-Konto zugeordnet ist. Bei Verwendung einer benutzerseitig zugewiesenen verwalteten Identität haben Sie auch die Möglichkeit, bei der Erstellung des Batch-Kontos kundenseitig verwaltete Schlüssel zu erzwingen. Dies wird [unten im Beispiel](#create-a-batch-account-with-user-assigned-managed-identity-and-customer-managed-keys) veranschaulicht.

## <a name="create-a-batch-account-with-system-assigned-managed-identity"></a>Erstellen eines Batch-Kontos mit systemseitig zugewiesener verwalteter Identität

Falls Sie keine separate benutzerseitig zugewiesene verwaltete Identität benötigen, können Sie beim Erstellen Ihres Batch-Kontos eine systemseitig zugewiesene verwaltete Identität aktivieren.

### <a name="azure-portal"></a>Azure-Portal

Wählen Sie im [Azure-Portal](https://portal.azure.com/) beim Erstellen von Batch-Konten auf der Registerkarte **Erweitert** als Identitätstyp **Systemseitig zugewiesen** aus.

![Screenshot: Neues Batch-Konto mit Identitätstyp „Systemseitig zugewiesen“](./media/batch-customer-managed-key/create-batch-account.png)

Nachdem das Konto erstellt wurde, ist im Abschnitt **Eigenschaften** im Feld **Identitätsprinzipal-ID** eine eindeutige GUID enthalten. Als **Identitätstyp** wird `System assigned` angezeigt.

![Screenshot: Eindeutige GUID im Feld „Identitätsprinzipal-ID“](./media/batch-customer-managed-key/linked-batch-principal.png)

Sie benötigen diesen Wert, um für dieses Batch-Konto Zugriff auf die Key Vault-Instanz zu gewähren.

### <a name="azure-cli"></a>Azure CLI

Wenn Sie ein neues Batch-Konto erstellen, geben Sie `SystemAssigned` für den `--identity`-Parameter an.

```azurecli
resourceGroupName='myResourceGroup'
accountName='mybatchaccount'

az batch account create \
    -n $accountName \
    -g $resourceGroupName \
    --locations regionName='West US 2' \
    --identity 'SystemAssigned'
```

Nach dem Erstellen des Kontos können Sie überprüfen, ob die vom System zugewiesene verwaltete Identität für dieses Konto aktiviert wurde. Notieren Sie sich den Wert für `PrincipalId`, da Sie ihn benötigen, um dem Batch-Konto Zugriff auf die Key Vault-Instanz zu gewähren.

```azurecli
az batch account show \
    -n $accountName \
    -g $resourceGroupName \
    --query identity
```

> [!NOTE]
> Die vom System zugewiesene verwaltete Identität, die in einem Batch-Konto erstellt wurde, wird nur zum Abrufen von kundenseitig verwalteten Schlüsseln aus dem Key Vault verwendet. Diese Identität ist für Batch-Pools nicht verfügbar. Um eine benutzerseitig zugeordnete verwaltete Identität in einem Pool zu verwenden, lesen Sie [Konfigurieren verwalteter Identitäten in Azure Batch-Pools](managed-identity-pools.md).

## <a name="create-a-user-assigned-managed-identity"></a>Erstellen einer benutzerseitig zugewiesenen verwalteten Identität

Bei Bedarf können Sie auch eine [benutzerseitig zugewiesene verwaltete Identität erstellen](../active-directory/managed-identities-azure-resources/how-to-manage-ua-identity-portal.md#create-a-user-assigned-managed-identity), die für den Zugriff auf Ihre kundenseitig verwalteten Schlüssel verwendet werden kann.

Sie benötigen den Wert unter **Client-ID** für diese Identität, um dafür den Zugriff auf die Key Vault-Instanz zu ermöglichen.

## <a name="configure-your-azure-key-vault-instance"></a>Konfigurieren Ihrer Azure Key Vault-Instanz

Die Azure Key Vault-Instanz, auf der Ihre Schlüssel generiert werden, muss auf demselben Mandanten wie Ihr Batch-Konto erstellt werden. Hierfür ist es nicht erforderlich, dass sie sich in derselben Ressourcengruppe oder unter demselben Abonnement befindet.

### <a name="create-an-azure-key-vault"></a>Erstellen einer Azure Key Vault-Instanz

Stellen Sie beim [Erstellen einer Azure Key Vault-Instanz](../key-vault/general/quick-create-portal.md) mit kundenseitig verwalteten Schlüsseln für Azure Batch sicher, dass die Optionen **Vorläufiges Löschen** und **Löschschutz** aktiviert sind.

![Screenshot: Bildschirm für Key Vault-Erstellung](./media/batch-customer-managed-key/create-key-vault.png)

### <a name="add-an-access-policy-to-your-azure-key-vault-instance"></a>Hinzufügen einer Zugriffsrichtlinie zu Ihrer Azure Key Vault-Instanz

Fügen Sie im Azure-Portal nach der Erstellung der Key Vault-Instanz in **Zugriffsrichtlinie** unter **Einstellung** den Zugriff auf das Batch-Konto mithilfe der verwalteten Identität hinzu. Wählen Sie unter **Schlüsselberechtigungen** die Berechtigungen **Abrufen**, **Schlüssel packen** und **Schlüssel entpacken** aus.

![Screenshot: Bildschirm zum Hinzufügen der Zugriffsrichtlinie.](./media/batch-customer-managed-key/key-permissions.png)

Machen Sie im Feld **Auswählen** unter **Prinzipal** eine der folgenden Angaben:

- Für systemseitig zugewiesene verwaltete Identität: Geben Sie die zuvor abgerufene `principalId` oder den Namen des Batch-Kontos ein.
- Für benutzerseitig zugewiesene verwaltete Identität: Geben Sie die **Client-ID**, die Sie zuvor abgerufen haben, oder den Namen der benutzerseitig zugewiesenen verwalteten Identität ein.

![Screenshot: Bildschirm „Prinzipal“](./media/batch-customer-managed-key/principal-id.png)

### <a name="generate-a-key-in-azure-key-vault"></a>Generieren eines Schlüssels in Azure Key Vault

Wechseln Sie im Azure-Portal zur Key Vault-Instanz im Abschnitt **Schlüssel**, und wählen Sie **Generieren/importieren** aus. Wählen Sie für den **Schlüsseltyp** die Option `RSA` und für die **RSA-Schlüsselgröße** mindestens `2048` aus. `EC`-Schlüsseltypen werden derzeit als kundenseitig verwalteter Schlüssel in einem Batch-Konto nicht unterstützt.

![Erstellen eines Schlüssels](./media/batch-customer-managed-key/create-key.png)

Klicken Sie nach dem Erstellen des Schlüssels auf den neu erstellten Schlüssel und die aktuelle Version, und kopieren Sie den **Schlüsselbezeichner** im Abschnitt **Eigenschaften**.  Vergewissern Sie sich, dass unter **Zulässige Vorgänge** die Optionen **Schlüssel packen** und **Schlüssel entpacken** aktiviert sind.

## <a name="enable-customer-managed-keys-on-a-batch-account"></a>Aktivieren von kundenseitig verwalteten Schlüsseln für ein Batch-Konto

Nachdem Sie die obigen Schritte ausgeführt haben, können Sie kundenseitig verwaltete Schlüssel für Ihr Batch-Konto aktivieren.

### <a name="azure-portal"></a>Azure-Portal

Navigieren Sie im [Azure-Portal](https://portal.azure.com/) zur Batch-Konto-Seite. Aktivieren Sie im Abschnitt **Verschlüsselung** die Option **Kundenseitig verwalteter Schlüssel**. Sie können den Schlüsselbezeichner direkt verwenden, oder sie können den Schlüsseltresor auswählen und dann auf **Schlüsseltresor und Schlüssel auswählen** klicken.

![Screenshot: Abschnitt „Verschlüsselung“ und Option zum Aktivieren des kundenseitig verwalteten Schlüssels](./media/batch-customer-managed-key/encryption-page.png)

### <a name="azure-cli"></a>Azure CLI

Nachdem das Batch-Konto mit der vom System zugewiesenen verwalteten Identität erstellt wurde und der Zugriff auf Key Vault erteilt wurde, aktualisieren Sie das Batch-Konto mit der URL `{Key Identifier}` unter dem `keyVaultProperties`-Parameter. Legen Sie außerdem **encryption_key_source** auf `Microsoft.KeyVault` fest.

```azurecli
az batch account set \
    -n $accountName \
    -g $resourceGroupName \
    --encryption_key_source Microsoft.KeyVault \
    --encryption_key_identifier {YourKeyIdentifier} 
```

## <a name="create-a-batch-account-with-user-assigned-managed-identity-and-customer-managed-keys"></a>Erstellen eines Batch-Kontos mit benutzerseitig zugewiesener verwalteter Identität und kundenseitig verwalteten Schlüsseln

Mit dem Batch Management .NET-Client können Sie ein Batch-Konto erstellen, das über eine benutzerseitig zugewiesene verwaltete Identität und kundenseitig verwaltete Schlüssel verfügt.

```c#
EncryptionProperties encryptionProperties = new EncryptionProperties()
{
    KeySource = KeySource.MicrosoftKeyVault,
    KeyVaultProperties = new KeyVaultProperties()
    {
        KeyIdentifier = "Your Key Azure Resource Manager Resource ID"
    }
};

BatchAccountIdentity identity = new BatchAccountIdentity()
{
    Type = ResourceIdentityType.UserAssigned,
    UserAssignedIdentities = new Dictionary<string, BatchAccountIdentityUserAssignedIdentitiesValue>
    {
            ["Your Identity Azure Resource Manager ResourceId"] = new BatchAccountIdentityUserAssignedIdentitiesValue()
    }
};
var parameters = new BatchAccountCreateParameters(TestConfiguration.ManagementRegion, encryption:encryptionProperties, identity: identity);

var account = await batchManagementClient.Account.CreateAsync("MyResourceGroup",
    "mynewaccount", parameters); 
```

## <a name="update-the-customer-managed-key-version"></a>Aktualisieren der Version des kundenseitig verwalteten Schlüssels

Wenn Sie eine neue Version eines Schlüssels erstellen, aktualisieren Sie das Batch-Konto, damit dieses die neue Version nutzt. Folgen Sie diesen Schritten:

1. Navigieren Sie im Azure-Portal zu Ihrem Batch-Konto, und zeigen Sie die Einstellungen für „Verschlüsselung“ an.
2. Geben Sie den URI für die neue Schlüsselversion ein. Alternativ können Sie die Key Vault-Instanz und den Schlüssel erneut auswählen, um die Version zu aktualisieren.
3. Speichern Sie die Änderungen.

Sie können die Version auch über die Azure CLI aktualisieren.

```azurecli
az batch account set \
    -n $accountName \
    -g $resourceGroupName \
    --encryption_key_identifier {YourKeyIdentifierWithNewVersion} 
```

## <a name="use-a-different-key-for-batch-encryption"></a>Verwenden eines anderen Schlüssels für die Batch Verschlüsselung

Gehen Sie wie folgt vor, um den für die Batch-Verschlüsselung verwendeten Schlüssel zu ändern:

1. Navigieren Sie zu Ihrem Batch-Konto, und zeigen Sie die Einstellungen für „Verschlüsselung“ an.
2. Geben Sie den URI für den neuen Schlüssel ein. Alternativ können Sie die Key Vault-Instanz und einen neuen Schlüssel auswählen.
3. Speichern Sie die Änderungen.

Sie können auch mithilfe der Azure CLI einen anderen Schlüssel verwenden.

```azurecli
az batch account set \
    -n $accountName \
    -g $resourceGroupName \
    --encryption_key_identifier {YourNewKeyIdentifier} 
```

## <a name="frequently-asked-questions"></a>Häufig gestellte Fragen

- **Werden kundenseitig verwaltete Schlüssel für vorhandene Batch-Konten unterstützt?** Nein. Kundenseitig verwaltete Schlüssel werden nur für neue Batch-Konten unterstützt.
- **Kann ich RSA-Schlüsselgrößen über 2048 Bits auswählen?** Ja, die RSA-Schlüsselgrößen `3072` und `4096` werden ebenfalls unterstützt.
- **Welche Vorgänge sind nach dem Sperren/Widerrufen eines kundenseitig verwalteten Schlüssels verfügbar?** Wenn Batch den Zugriff auf den kundenseitig verwalteten Schlüssel verliert, ist der einzige zulässige Vorgang das Löschen des Kontos.
- **Wie kann ich den Zugriff auf mein Batch-Konto wiederherstellen, wenn ich den Key Vault-Schlüssel versehentlich lösche?** Da Löschschutz und vorläufiges Löschen aktiviert sind, können Sie die vorhandenen Schlüssel wiederherstellen. Weitere Informationen finden Sie unter [Wiederherstellen einer Azure Key Vault-Instanz](../key-vault/general/key-vault-recovery.md).
- **Kann ich kundenseitig verwaltete Schlüssel deaktivieren?** Sie können jederzeit den Verschlüsselungstyp des Batch-Kontos auf „Von Microsoft verwalteter Schlüssel“ zurücksetzen. Anschließend können Sie den Schlüssel löschen oder ändern.
- **Wie kann ich meine Schlüssel rotieren?** Kundenseitig verwaltete Schlüssel werden nicht automatisch rotiert. Zum Rotieren des Schlüssels müssen Sie den Schlüsselbezeichner aktualisieren, dem das Konto zugeordnet ist.
- **Wie lange dauert es nach dem Wiederherstellen des Zugriffs, bis das Batch-Konto wieder funktioniert?** Nach dem Wiederherstellen des Zugriffs kann es bis zu 10 Minuten dauern, bis das Konto wieder zugänglich ist.
- **Was geschieht mit meinen Ressourcen, während das Batch-Konto nicht verfügbar ist?** Alle Pools, die ausgeführt werden, wenn der Batch-Zugriff auf kundenseitig verwaltete Schlüssel verloren geht, werden weiterhin ausgeführt. Die Knoten werden jedoch in den Zustand „Nicht verfügbar“ versetzt, und die Ausführung von Tasks wird beendet (und sie werden erneut in die Warteschlange gestellt). Sobald der Zugriff wieder hergestellt wurde, werden die Knoten wieder verfügbar, und die Tasks werden neu gestartet.
- **Gilt dieser Verschlüsselungsmechanismus für VM-Datenträger in einem Batch-Pool?** Nein. Bei mit der Clouddienstkonfiguration erstellten Pools wird keine Verschlüsselung auf das Betriebssystem und den temporären Datenträger angewendet. Bei mit der VM-Konfiguration erstellten Pools werden das Betriebssystem und die angegebenen Datenträger standardmäßig mit einem von der Microsoft-Plattform verwalteten Schlüssel verschlüsselt. Derzeit können Sie keinen eigenen Schlüssel für diese Datenträger angeben. Um den temporären Datenträger von VMs für einen Batch-Pool mit einem von der Microsoft-Plattform verwalteten Schlüssel zu verschlüsseln, müssen Sie die Eigenschaft [diskEncryptionConfiguration](/rest/api/batchservice/pool/add#diskencryptionconfiguration) im Pool der [Konfiguration der virtuellen Maschine](/rest/api/batchservice/pool/add#virtualmachineconfiguration) aktivieren. Für höchst vertrauliche Daten empfehlen wir, die Verschlüsselung temporärer Datenträger zu aktivieren und das Speichern von vertraulichen Daten auf Betriebssystemdatenträgern und Datenträgern für Daten zu vermeiden. Weitere Informationen finden Sie unter [Erstellen eines Pools mit aktivierter Datenträgerverschlüsselung](./disk-encryption.md).
- **Ist die vom System zugewiesene verwaltete Identität für das Batch-Konto auf den Computeknoten verfügbar?** Nein. Die systemseitig zugewiesene verwaltete Identität wird derzeit nur für den Zugriff auf Azure Key Vault für den kundenseitig verwalteten Schlüssel verwendet. Um eine benutzerseitig zugeordnete verwaltete Identität auf Computeknoten zu verwenden, lesen Sie [Konfigurieren verwalteter Identitäten in Azure Batch-Pools](managed-identity-pools.md).

## <a name="next-steps"></a>Nächste Schritte

- Informieren Sie sich über die [bewährten Methoden in Bezug auf die Sicherheit in Azure Batch](security-best-practices.md).
- Lesen Sie die weiteren Informationen zu [Azure Key Vault](../key-vault/general/basic-concepts.md).
