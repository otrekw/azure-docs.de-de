---
title: Verschlüsseln von Bereitstellungsdaten
description: Hier finden Sie Informationen zur Verschlüsselung gespeicherter Daten für Ihre Containerinstanzressourcen, und Sie erfahren, wie Sie die Daten mit einem kundenseitig verwalteten Schlüssel verschlüsseln.
ms.topic: article
ms.date: 01/10/2020
ms.author: danlep
ms.openlocfilehash: 146effd7f1a7ad1ddd94886d1a79e2914bd1c94b
ms.sourcegitcommit: 3eb0cc8091c8e4ae4d537051c3265b92427537fe
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/11/2020
ms.locfileid: "75903691"
---
# <a name="encrypt-deployment-data"></a>Verschlüsseln von Bereitstellungsdaten

Wenn Sie ACI-Ressourcen (Azure Container Instances) in der Cloud ausführen, sammelt und speichert der ACI-Dienst Daten im Zusammenhang mit Ihren Containern. Diese Daten werden von ACI automatisch verschlüsselt, wenn sie in der Cloud gespeichert werden. Diese Verschlüsselung schützt Ihre Daten und unterstützt Sie bei der Einhaltung der Sicherheits- und Complianceanforderungen Ihrer Organisation. ACI ermöglicht zudem die Verschlüsselung dieser Daten mit Ihrem eigenen Schlüssel. Dadurch haben Sie mehr Kontrolle über die Daten im Zusammenhang mit Ihren ACI-Bereitstellungen.

## <a name="about-aci-data-encryption"></a>Informationen zur ACI-Datenverschlüsselung 

Daten in ACI werden mittels 256-Bit-AES-Verschlüsselung ver- und entschlüsselt. Die Verschlüsselung ist für alle ACI-Bereitstellungen aktiviert, und Sie müssen Ihre Bereitstellung oder Container nicht ändern, um die Verschlüsselung zu nutzen. Dies schließt Metadaten zur Bereitstellung, Umgebungsvariablen, die Übergabe von Schlüsseln an Ihre Container sowie Protokolle ein, die auch nach Beendigung Ihrer Container beibehalten werden, um sie weiterhin anzeigen zu können. Die Verschlüsselung hat keine Auswirkungen auf die Leistung Ihrer Containergruppe, und es fallen keine zusätzlichen Kosten für die Verschlüsselung an.

## <a name="encryption-key-management"></a>Verwaltung von Verschlüsselungsschlüsseln

Sie können entweder von Microsoft verwaltete Schlüssel für die Verschlüsselung Ihrer Containerdaten nutzen oder die Verschlüsselung mit Ihren eigenen Schlüsseln verwalten. In der folgenden Tabelle werden diese beiden Optionen miteinander verglichen: 

|    |    Von Microsoft verwaltete Schlüssel     |     Vom Kunden verwaltete Schlüssel     |
|----|----|----|
|    Verschlüsselungs-/Entschlüsselungsvorgänge    |    Azure    |    Azure    |
|    Schlüsselspeicher    |    Microsoft-Schlüsselspeicher    |    Azure-Schlüsseltresor    |
|    Verantwortlich für die Schlüsselrotation    |    Microsoft    |    Kunde    |
|    Schlüsselzugriff    |    Nur Microsoft    |    Microsoft, Kunde    |

Im restlichen Dokument werden die Schritte beschrieben, die ausgeführt werden müssen, um Ihre ACI-Bereitstellungsdaten mit Ihrem Schlüssel (kundenseitig verwalteter Schlüssel) zu verschlüsseln. 

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

## <a name="encrypt-data-with-a-customer-managed-key"></a>Verschlüsseln von Daten mit einem kundenseitig verwalteten Schlüssel

### <a name="create-service-principal-for-aci"></a>Erstellen eines Dienstprinzipals für ACI

Als Erstes muss dafür gesorgt werden, dass Ihr [Azure-Mandant](https://docs.microsoft.com/azure/active-directory/develop/quickstart-create-new-tenant) über einen zugewiesenen Dienstprinzipal verfügt, um dem Azure Container Instances-Dienst Berechtigungen zu gewähren. 

Mit dem folgenden CLI-Befehl wird der ACI-Dienstprinzipal in Ihrer Azure-Umgebung eingerichtet:

```azurecli-interactive
az ad sp create --id 6bb8e274-af5d-4df2-98a3-4fd78b4cafd9
```

Die Ausgabe des Befehls sollte ein Dienstprinzipal sein, der mit Folgendem eingerichtet wurde: "displayName": "Azure Container Instance Service".

### <a name="create-a-key-vault-resource"></a>Erstellen einer Key Vault-Ressource

Erstellen Sie eine Azure Key Vault-Ressource. Hierzu können Sie das [Azure-Portal](https://docs.microsoft.com/azure/key-vault/quick-create-portal#create-a-vault), die [Befehlszeilenschnittstelle](https://docs.microsoft.com/azure/key-vault/quick-create-cli) oder [PowerShell](https://docs.microsoft.com/azure/key-vault/quick-create-powershell) verwenden. 

Berücksichtigen Sie bei den Eigenschaften Ihres Schlüsseltresors die folgenden Richtlinien: 
* Name: Es ist ein eindeutiger Name erforderlich. 
* Abonnement: Wählen Sie ein Abonnement aus.
* Wählen Sie unter „Ressourcengruppe“ entweder eine bereits vorhandene Ressourcengruppe aus, oder erstellen Sie eine neue Ressourcengruppe, und geben Sie einen Ressourcengruppennamen ein.
* Wählen Sie im Pulldownmenü „Standort“ einen Standort aus.
* Bei den anderen Optionen können Sie entweder die Standardeinstellungen übernehmen oder sie gemäß Ihren individuellen Anforderungen festlegen.

> [!IMPORTANT]
> Wenn Sie kundenseitig verwaltete Schlüssel zum Verschlüsseln einer ACI-Bereitstellungsvorlage verwenden, empfiehlt es sich, für den Schlüsseltresor die Eigenschaften „Soft Delete“ (Vorläufig löschen) und „Do Not Purge“ (Nicht bereinigen) festzulegen. Diese Eigenschaften sind standardmäßig nicht aktiviert, können aber entweder mithilfe von PowerShell oder per Azure CLI für einen neuen oder vorhandenen Schlüsseltresor aktiviert werden.

### <a name="generate-a-new-key"></a>Generieren eines neuen Schlüssels 

Navigieren Sie nach der Erstellung Ihres Schlüsseltresors zur entsprechenden Ressource im Azure-Portal. Klicken Sie im linken Navigationsmenü des Ressourcenblatts unter „Einstellungen“ auf **Schlüssel**. Klicken Sie in der Ansicht für „Schlüssel“ auf „Generieren/importieren“, um einen neuen Schlüssel zu generieren. Verwenden Sie einen beliebigen eindeutigen Namen für diesen Schlüssel, und legen Sie die gewünschten Einstellungen fest. 

![Generieren eines neuen Schlüssels](./media/container-instances-encrypt-data/generate-key.png)

### <a name="set-access-policy"></a>Festlegen der Zugriffsrichtlinie

Erstellen Sie eine neue Zugriffsrichtlinie, um dem ACI-Dienst den Zugriff auf Ihren Schlüssel zu ermöglichen.

* Klicken Sie nach Abschluss der Schlüsselgenerierung auf dem Ressourcenblatt Ihres Schlüsseltresors unter „Einstellungen“ auf **Zugriffsrichtlinien**.
* Klicken Sie auf der Seite „Zugriffsrichtlinien“ für Ihren Schlüsseltresor auf **Zugriffsrichtlinie hinzufügen**.
* Schließen Sie unter *Schlüsselberechtigungen* die Berechtigungen **Abrufen** und **Schlüssel entpacken** ein. ![Festlegen der Schlüsselberechtigungen](./media/container-instances-encrypt-data/set-key-permissions.png)
* Wählen Sie unter *Prinzipal auswählen* die Option **Azure Container Instance Service** aus.
* Klicken Sie unten auf **Hinzufügen**. 

Die Zugriffsrichtlinie sollte nun in den Zugriffsrichtlinien Ihres Schlüsseltresors angezeigt werden.

![Neue Zugriffsrichtlinie](./media/container-instances-encrypt-data/access-policy.png)

### <a name="modify-your-json-deployment-template"></a>Ändern Ihrer JSON-Bereitstellungsvorlage

> [!IMPORTANT]
> Das Verschlüsseln von Bereitstellungsdaten mit einem kundenseitig verwalteten Schlüssel ist in der neuesten API-Version (2019-12-01) verfügbar, deren Rollout momentan durchgeführt wird. Geben Sie diese API-Version in Ihrer Bereitstellungsvorlage an. Sollten dabei Probleme auftreten, wenden Sie sich an den Azure-Support.

Fügen Sie Ihrer ACI-Bereitstellungsvorlage nach dem Einrichten des Schlüsseltresorschlüssels und der Zugriffsrichtlinie die folgende Eigenschaft hinzu. Weitere Informationen zum Bereitstellen von ACI-Ressourcen mit einer Vorlage finden Sie unter [Tutorial: Bereitstellen einer Gruppe mit mehreren Containern über eine Resource Manager-Vorlage](https://docs.microsoft.com/azure/container-instances/container-instances-multi-container-group). 

Fügen Sie insbesondere im Abschnitt mit den Containergruppeneigenschaften der Bereitstellungsvorlage Verschlüsselungseigenschaften (encryptionProperties) mit folgenden Werten hinzu:
* vaultBaseUrl: Der DNS-Name Ihres Schlüsseltresors. Diesen finden Sie auf dem Übersichtsblatt der Schlüsseltresorressource im Portal.
* keyName: Der Name des zuvor generierten Schlüssels.
* keyVersion: Die aktuelle Version des Schlüssels. Diese Angabe finden Sie, indem Sie direkt auf den Schlüssel klicken (im Abschnitt „Einstellungen“ Ihrer Schlüsseltresorressource unter „Schlüssel“).


```json
"resources": [
    {
        "name": "[parameters('containerGroupName')]",
        "type": "Microsoft.ContainerInstance/containerGroups",
        "apiVersion": "2019-12-01",
        "location": "[resourceGroup().location]",    
        "properties": {
            "encryptionProperties": {
                "vaultBaseUrl": "https://example.vault.azure.net",
                "keyName": "acikey",
                "keyVersion": "xxxxxxxxxxxxxxxx"
            },
            "containers": {
                [...]
            }
        }
    }
]
```

### <a name="deploy-your-resources"></a>Bereitstellen Ihrer Ressourcen

Wenn Sie die Vorlagendatei auf Ihrem Desktop erstellt und bearbeitet haben, können Sie sie in Ihr Cloud Shell-Verzeichnis hochladen, indem Sie sie dorthin ziehen. 

Erstellen Sie mithilfe des Befehls [az group create][az-group-create] eine Ressourcengruppe.

```azurecli-interactive
az group create --name myResourceGroup --location eastus
```

Stellen Sie mit dem Befehl [az group deployment create][az-group-deployment-create] die Vorlage bereit.

```azurecli-interactive
az group deployment create --resource-group myResourceGroup --template-file deployment-template.json
```

Innerhalb weniger Sekunden sollten Sie eine erste Antwort von Azure erhalten. Nach Abschluss der Bereitstellung werden alle bereitstellungsbezogenen Daten, die vom ACI-Dienst gespeichert werden, mit dem bereitgestellten Schlüssel verschlüsselt.

<!-- LINKS - Internal -->
[az-group-create]: /cli/azure/group#az-group-create
[az-group-deployment-create]: /cli/azure/group/deployment#az-group-deployment-create