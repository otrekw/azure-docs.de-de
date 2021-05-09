---
title: Eine verwaltete Identität ihres Azure Automation-Kontos deaktivieren (Vorschau)
description: In diesem Artikel wird das Deaktivieren und Entfernen einer verwalteten Identität für ein Azure Automation erläutert.
services: automation
ms.subservice: process-automation
ms.date: 04/14/2021
ms.topic: conceptual
ms.openlocfilehash: e17e1afda50d9a0263067a77bf26435f53b4f237
ms.sourcegitcommit: db925ea0af071d2c81b7f0ae89464214f8167505
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/15/2021
ms.locfileid: "107519271"
---
# <a name="disable-your-azure-automation-account-managed-identity-preview"></a>Eine verwaltete Identität ihres Azure Automation-Kontos deaktivieren (Vorschau)

Es gibt zwei Möglichkeiten, eine vom System zugewiesene Identität in einer Azure Automation zu deaktivieren. Sie können diese Aufgabe über das Azure-Portal oder mithilfe einer ARM-Vorlage (Azure Resource Manager) abschließen.

## <a name="disable-managed-identity-in-the-azure-portal"></a>Deaktivieren der verwalteten Identität im Azure-Portal

Unabhängig davon, wie die verwaltete Identität ursprünglich eingerichtet wurde, die verwaltete Identität kann mit dem Azure-Portal deaktiviert werden.

1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com) an.

1. Navigieren Sie zu Ihrem Automation-Konto, und wählen Sie **Identität** unter **Kontoeinstellungen** aus.

1. Legen Sie die Option **System zugewiesen** auf **Aus** fest und klicken Sie auf **Speichern**. Wenn Sie aufgefordert werden, die Auswahl zu bestätigen, drücken Sie auf **Ja**.

Die verwaltete Identität wurde entfernt und hat keinen Zugriff mehr auf die Zielressource.

## <a name="disable-using-azure-resource-manager-template"></a>Deaktivieren mithilfe der Azure Resource Manager-Vorlage

Wenn Sie die verwaltete Identität für Ihr Automation-Konto mithilfe einer Azure Resource Manager-Vorlage erstellt haben, können Sie die verwaltete Identität deaktivieren, indem Sie diese Vorlage wiederverwenden und deren Einstellungen ändern. Legen Sie den Typ der untergeordneten Eigenschaft des Identitätsobjekts wie im folgenden Beispiel gezeigt auf **Keine** fest, und führen Sie dann die Vorlage erneut aus.

```json
"identity": { 
   "type": "None" 
} 
```

Bei dieser Methode zum Entfernen einer systemseitig zugewiesenen Identität wird diese auch aus Azure AD gelöscht. Systemseitig zugewiesene Identitäten werden automatisch aus Azure AD entfernt, wenn die ihnen zugewiesene App-Ressource gelöscht wird.

## <a name="next-steps"></a>Nächste Schritte

- Weitere Informationen zum Aktivieren der verwalteten Identität in Azure Automation finden Sie unter [Aktivieren und Verwenden der verwalteten Identität für Automation (Vorschau)](enable-managed-identity-for-automation.md).

- Eine Übersicht über die Automation-Kontosicherheit finden Sie unter [Übersicht über die Automation-Kontoauthentifizierung.](automation-security-overview.md)
