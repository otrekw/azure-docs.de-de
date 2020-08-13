---
title: QnA Maker-Verschlüsselung für ruhende Daten
titleSuffix: Azure Cognitive Services
description: Microsoft bietet von Microsoft verwaltete Verschlüsselungsschlüssel an und ermöglicht Ihnen auch die Verwaltung Ihrer Cognitive Services-Abonnements mit Ihren eigenen Schlüsseln, den so genannten kundenseitig verwalteten Schlüsseln (Customer Managed Keys, CMK). In diesem Artikel erfahren Sie mehr über die Datenverschlüsselung im Ruhezustand für QnA Maker und wie Sie CMK aktivieren und verwalten können.
author: erindormier
manager: venkyv
ms.service: cognitive-services
ms.subservice: qna-maker
ms.topic: conceptual
ms.date: 07/10/2020
ms.author: egeaney
ms.openlocfilehash: 1ca0dda046329e95c649540fd42f96ca43838c85
ms.sourcegitcommit: 3d79f737ff34708b48dd2ae45100e2516af9ed78
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/23/2020
ms.locfileid: "87086704"
---
# <a name="qna-maker-encryption-of-data-at-rest"></a>QnA Maker-Verschlüsselung für ruhende Daten

QnA Maker verschlüsselt automatisch Daten, wenn sie in der Cloud persistent gespeichert werden, und unterstützt Sie so bei der Einhaltung der Sicherheits- und Complianceziele Ihrer Organisation.

## <a name="about-encryption-key-management"></a>Informationen zur Verwaltung von Verschlüsselungsschlüsseln

Standardmäßig verwendet Ihr Abonnement von Microsoft verwaltete Verschlüsselungsschlüssel. Es gibt auch eine Option zum Verwalten Ihres Abonnements mit ihren eigenen Schlüsseln. Kundenseitig verwaltete Schlüssel (CMK) bieten größere Flexibilität beim Erstellen, Rotieren, Deaktivieren und Widerrufen von Zugriffssteuerungen. Außerdem können Sie die zum Schutz Ihrer Daten verwendeten Verschlüsselungsschlüssel überwachen.

QnA Maker verwendet die CMK-Unterstützung von Azure Search. Sie müssen [mithilfe von Azure Key Vault einen CMK in Azure Search ](https://docs.microsoft.com/azure/search/search-security-manage-encryption-keys)erstellen. Diese Azure-Instanz muss dem QnA Maker-Dienst zugeordnet sein, damit sie CMK-fähig ist.

> [!IMPORTANT]
> Die Azure Search-Dienstressource muss nach Januar 2019 erstellt worden sein und darf sich nicht im Free-Tarif (freigegeben) befinden. Die Konfiguration von kundenseitig verwalteten Schlüsseln wird im Azure-Portal nicht unterstützt.

## <a name="enable-customer-managed-keys"></a>Aktivieren von vom Kunden verwalteten Schlüsseln

Der QnA Maker-Dienst verwendet CMKs aus dem Azure Search-Dienst. Führen Sie die folgenden Schritte zum Aktivieren von CMKs aus:

1. Erstellen Sie eine neue Azure Search Instanz, und aktivieren Sie die Voraussetzungen, die in [Verschlüsselung ruhender Inhalte in Azure Cognitive Search mit von Kunden verwalteten Schlüsseln in Azure Key Vault unter „Voraussetzungen“](https://docs.microsoft.com/azure/search/search-security-manage-encryption-keys#prerequisites) aufgeführt sind.

   ![Verschlüsselungseinstellungen anzeigen](../media/cognitive-services-encryption/qna-encryption-1.png)

2. Wenn Sie eine QnA Maker-Ressource erstellen, wird sie automatisch einer Azure Search-Instanz zugeordnet. So ist die Nutzung von CMKs nicht möglich. Zur Verwendung von CMKs müssen Sie die neu erstellte Instanz von Azure Search aus Schritt 1 zuordnen. Insbesondere müssen Sie `AzureSearchAdminKey` und `AzureSearchName` in der QnA Maker-Ressource aktualisieren.

   ![Verschlüsselungseinstellungen anzeigen](../media/cognitive-services-encryption/qna-encryption-2.png)

3. Als nächstes erstellen Sie eine neue Anwendungseinstellung:
   * **Name**: Auf `CustomerManagedEncryptionKeyUrl`
   * **Value**: Dies ist der Wert, den Sie in Schritt 1 beim Erstellen der Azure Search-Instanz erhalten haben.

   ![Verschlüsselungseinstellungen anzeigen](../media/cognitive-services-encryption/qna-encryption-3.png)

4. Wenn Sie fertig sind, starten Sie die Runtime neu. Ihr QnA Maker-Dienst ist nun CMK-fähig.

## <a name="regional-availability"></a>Regionale Verfügbarkeit

Kundenseitig verwaltete Schlüssel sind in allen Azure Search-Regionen verfügbar.

## <a name="encryption-of-data-in-transit"></a>Verschlüsselung von Daten während der Übertragung

Das QnA Maker-Portal wird im Browser des Benutzers ausgeführt. Jede Aktion löst einen direkten Aufruf der entsprechenden Cognitive Services-API aus. Daher ist QnA Maker mit Daten während der Übertragung kompatibel.
Der QnA Maker-Portaldienst wird jedoch in der Region „USA, Westen“ gehostet und ist daher nicht ideal für Kunden außerhalb der USA. 

## <a name="next-steps"></a>Nächste Schritte

* [Verschlüsselung in Azure Search mithilfe von CMKs in Azure Key Vault](https://docs.microsoft.com/azure/search/search-security-manage-encryption-keys)
* [Datenverschlüsselung ruhender Daten](https://docs.microsoft.com/azure/security/fundamentals/encryption-atrest)
* [Weitere Informationen zu Azure Key Vault](https://docs.microsoft.com/azure/key-vault/key-vault-overview)