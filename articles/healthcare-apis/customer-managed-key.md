---
title: Konfigurieren von kundenseitig verwalteten Schlüsseln für Azure API for FHIR
description: Unterstützung von Bring Your Own Key in Azure API for FHIR per Cosmos DB
services: healthcare-apis
author: matjazl
ms.service: healthcare-apis
ms.subservice: fhir
ms.topic: overview
ms.date: 09/28/2020
ms.author: matjazl
ms.openlocfilehash: 3879280f56a4b99d8e6e08a9c9ed852ef2cafa68
ms.sourcegitcommit: 3c8964a946e3b2343eaf8aba54dee41b89acc123
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/25/2021
ms.locfileid: "98747322"
---
# <a name="configure-customer-managed-keys-at-rest"></a>Konfigurieren kundenseitig verwalteter Schlüssel im Ruhezustand

Wenn Sie ein neues Azure API for FHIR-Konto erstellen, werden Ihre Daten standardmäßig mit von Microsoft verwalteten Schlüsseln verschlüsselt. Sie können jetzt eine zweite Verschlüsselungsebene für die Daten hinzufügen, indem Sie einen eigenen Schlüssel verwenden, den Sie selbst auswählen und verwalten.

In Azure wird dies normalerweise mit einem Verschlüsselungsschlüssel in der Azure Key Vault-Instanz des Kunden erreicht. Azure SQL, Azure Storage und Cosmos DB sind Beispiele für Dienste, die derzeit über diese Funktion verfügen. In Azure API for FHIR wird diese Unterstützung über Cosmos DB genutzt. Wenn Sie ein Konto erstellen, haben Sie die Möglichkeit, einen URI für einen Azure Key Vault-Schlüssel anzugeben. Dieser Schlüssel wird an Cosmos DB übergeben, wenn das DB-Konto bereitgestellt wird. Wenn eine FHIR-Anforderung gesendet wird, ruft Cosmos DB Ihren Schlüssel ab und verwendet ihn zum Ver- bzw. Entschlüsseln der Daten. Informationen zu den ersten Schritten finden Sie unter den folgenden Links:

- [Registrieren des Azure Cosmos DB-Ressourcenanbieters für Ihr Azure-Abonnement](../cosmos-db/how-to-setup-cmk.md#register-resource-provider) 
- [Konfigurieren Ihrer Azure Key Vault-Instanz](../cosmos-db/how-to-setup-cmk.md#configure-your-azure-key-vault-instance)
- [Hinzufügen einer Zugriffsrichtlinie zu Ihrer Azure Key Vault-Instanz](../cosmos-db/how-to-setup-cmk.md#add-an-access-policy-to-your-azure-key-vault-instance)
- [Generieren eines Schlüssels in Azure Key Vault](../cosmos-db/how-to-setup-cmk.md#generate-a-key-in-azure-key-vault)

## <a name="specify-the-azure-key-vault-key"></a>Angeben des Azure Key Vault-Schlüssels

Beim Erstellen Ihres Azure API for FHIR-Kontos im Azure-Portal wird auf der Registerkarte „Zusätzliche Einstellungen“ unter „Datenbankeinstellungen“ die Konfigurationsoption „Datenverschlüsselung“ angezeigt. Standardmäßig ist die Option „Dienstseitig verwalteter Schlüssel“ ausgewählt. 

Sie können Ihren Schlüssel in der Schlüsselauswahl auswählen:

:::image type="content" source="media/bring-your-own-key/bring-your-own-key-keypicker.png" alt-text="Schlüsselauswahl":::

Alternativ können Sie hier Ihren Azure Key Vault-Schlüssel angeben, indem Sie die Option „Kundenseitig verwalteter Schlüssel“ auswählen. Sie können den Schlüssel-URI hier eingeben:

:::image type="content" source="media/bring-your-own-key/bring-your-own-key-create.png" alt-text="Erstellen einer Azure API for FHIR-Instanz":::

Für vorhandene FHIR-Konten können Sie die Option für die Schlüsselverschlüsselung (dienst- oder kundenseitig verwalteter Schlüssel) wie unten dargestellt auf dem Blatt „Datenbank“ anzeigen. Die Konfigurationsoption kann nach dem Auswählen nicht mehr geändert werden. Sie können aber Ihren Schlüssel ändern und aktualisieren.

:::image type="content" source="media/bring-your-own-key/bring-your-own-key-database.png" alt-text="Datenbank":::

Darüber hinaus können Sie auch eine neue Version des angegebenen Schlüssels erstellen. Nach der Erstellung werden Ihre Daten mit der neuen Version verschlüsselt, ohne dass es zu einer Dienstunterbrechung kommt. Sie können den Zugriff auf den Schlüssel auch entfernen, damit nicht auf die Daten zugegriffen werden kann. Wenn der Schlüssel deaktiviert ist, tritt bei Abfragen ein Fehler auf. Wird der Schlüssel wieder aktiviert, sind die Abfragen wieder erfolgreich.

## <a name="next-steps"></a>Nächste Schritte

In diesem Artikel haben Sie gelernt, wie Sie kundenseitig verwaltete Schlüssel im Ruhezustand konfigurieren. Als Nächstes können Sie sich mit den häufig gestellten Fragen zu Azure Cosmos DB befassen: 
 
>[!div class="nextstepaction"]
>[Häufig gestellte Fragen](https://docs.microsoft.com/azure/cosmos-db/how-to-setup-cmk#frequently-asked-questions)
