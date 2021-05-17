---
title: 'Problembehandlung: Bedingungen für die Azure-Rollenzuweisung (Vorschau)'
description: 'Problembehandlung: Bedingungen für die Azure-Rollenzuweisung (Vorschau)'
services: active-directory
author: rolyon
manager: mtillman
ms.service: role-based-access-control
ms.subservice: conditions
ms.topic: troubleshooting
ms.workload: identity
ms.date: 05/06/2021
ms.author: rolyon
ms.openlocfilehash: eddebb6742a50ce729d436fc19ce4ff32491f75f
ms.sourcegitcommit: 1fbd591a67e6422edb6de8fc901ac7063172f49e
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/07/2021
ms.locfileid: "109489616"
---
# <a name="troubleshoot-azure-role-assignment-conditions-preview"></a>Problembehandlung: Bedingungen für die Azure-Rollenzuweisung (Vorschau)

> [!IMPORTANT]
> Azure RBAC und Azure-Rollenzuweisungsbedingungen befinden sich derzeit in der Vorschauphase.
> Diese Vorschauversion wird ohne Vereinbarung zum Servicelevel bereitgestellt und ist nicht für Produktionsworkloads vorgesehen. Manche Features werden möglicherweise nicht unterstützt oder sind nur eingeschränkt verwendbar.
> Weitere Informationen finden Sie unter [Zusätzliche Nutzungsbestimmungen für Microsoft Azure-Vorschauen](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).

## <a name="symptom---condition-is-not-enforced"></a>Symptom: Bedingung wird nicht erzwungen

**Ursache 1**

Sicherheitsprinzipale verfügen über mindestens eine Rollenzuweisung im gleichen oder einem höheren Bereich.

**Lösung 1**

Stellen Sie sicher, dass die Sicherheitsprinzipale nicht über mehrere Rollenzuweisungen (mit oder ohne Bedingungen) verfügen, die Zugriff auf dieselbe Datenaktion gewähren, was zu einer Nichterzwingung von Bedingungen führt. Informationen zur Auswertungslogik finden Sie unter [Wie Azure RBAC ermittelt, ob ein Benutzer Zugriff auf eine Ressource besitzt](overview.md#how-azure-rbac-determines-if-a-user-has-access-to-a-resource).

**Ursache 2**

Ihre Rollenzuweisung weist mehrere Aktionen auf, die eine Berechtigung gewähren, und Ihre Bedingung zielt nicht auf alle Aktionen ab. Sie können z. B. ein Blob erstellen, wenn Sie über `/blobs/write`- oder `/blobs/add/action`-Datenaktionen verfügen. Wenn Ihre Rollenzuweisung beide Datenaktionen enthält und Sie nur eine davon in einer Bedingung berücksichtigen, gewährt die Rollenzuweisung die Berechtigung zum Erstellen von Blobs und umgeht die Bedingung.

**Lösung 2**

Wenn Ihre Rollenzuweisung über mehrere Aktionen verfügt, die eine Berechtigung erteilen, stellen Sie sicher, dass Sie alle relevanten Aktionen als Ziel verwenden.

**Ursache 3**

Wenn Sie einer Rollenzuweisung eine Bedingung hinzufügen, kann es bis zu 5 Minuten dauern, bis die Bedingung erzwungen wird. Wenn Sie eine Bedingung hinzufügen, werden Ressourcenanbieter (z. B. Microsoft.Storage) über die Aktualisierung benachrichtigt. Ressourcenanbieter aktualisieren sofort Updates ihre lokalen Caches, um sicherzustellen, dass sie über die neuesten Rollenzuweisungen verfügen. Dieser Vorgang wird normalerweise in 1 oder 2 Minuten abgeschlossen, kann aber bis zu 5 Minuten dauern.

**Lösung 3**

Warten Sie 5 Minuten, und testen Sie die Bedingung dann erneut.

## <a name="symptom---condition-is-not-valid-error-when-adding-a-condition"></a>Symptom: Bedingung führt zu Fehler des Typs „Ungültig“ beim Hinzufügen einer Bedingung

Wenn Sie versuchen, eine Rollenzuweisung mit einer Bedingung hinzuzufügen, erhalten Sie einen Fehler ähnlich dem folgenden:

`The given role assignment condition is invalid.`

**Ursache**

Ihre Bedingung ist nicht ordnungsgemäß formatiert. 

**Lösung**

Beheben Sie alle Probleme mit dem [Bedingungsformat oder der Syntax](conditions-format.md). Alternativ können Sie die Bedingung mithilfe des [visuellen Editors im Azure-Portal](conditions-role-assignments-portal.md) hinzufügen.

## <a name="symptom---resource-attribute-is-not-valid-error-when-adding-a-condition-using-azure-powershell"></a>Symptom: Das Ressourcenattribut führt zu einem Fehler des Typs „Ungültig“, wenn eine Bedingung mithilfe von Azure PowerShell hinzugefügt wird

Wenn Sie mit Azure PowerShell versuchen, eine Rollenzuweisung mit einer Bedingung hinzuzufügen, erhalten Sie einen Fehler ähnlich dem folgenden:

```
New-AzRoleAssignment : Resource attribute
Microsoft.Storage/storageAccounts/blobServices/containers/blobs/tags:Project<$> is not valid.
```

**Ursache**

Wenn Ihre Bedingung ein Dollarzeichen ($) enthält, müssen Sie ihm einen umgekehrten Schrägstrich (\`) voranstellen.

**Lösung**

Fügen Sie vor jedem Dollarzeichen einen umgekehrten Schrägstrich (\`) hinzu. Die Folgenden wird ein Beispiel gezeigt. Weitere Informationen zu Regeln für Anführungszeichen in PowerShell finden Sie unter [Informationen zu Regeln für Anführungszeichen](/powershell/module/microsoft.powershell.core/about/about_quoting_rules).

```azurepowershell
$condition = "((!(ActionMatches{'Microsoft.Storage/storageAccounts/blobServices/containers/blobs/read'} AND @Request[subOperation] ForAnyOfAnyValues:StringEqualsIgnoreCase {'Blob.Read.WithTagConditions'})) OR (@Resource[Microsoft.Storage/storageAccounts/blobServices/containers/blobs/tags:Project<`$key_case_sensitive`$>] StringEquals 'Cascade'))"
```

## <a name="symptom---resource-attribute-is-not-valid-error-when-adding-a-condition-using-azure-cli"></a>Symptom: Das Ressourcenattribut führt zu einem Fehler des Typs „Ungültig“, wenn eine Bedingung mithilfe der Azure CLI hinzugefügt wird

Wenn Sie mit der Azure CLI versuchen, eine Rollenzuweisung mit einer Bedingung hinzuzufügen, erhalten Sie einen Fehler ähnlich dem folgenden:

```
Resource attribute Microsoft.Storage/storageAccounts/blobServices/containers/blobs/tags:Project<$> is not valid.
```

**Ursache**

Wenn Ihre Bedingung ein Dollarzeichen ($) enthält, müssen Sie ihm einen umgekehrten Schrägstrich (\\) voranstellen.

**Lösung**

Fügen Sie vor jedem Dollarzeichen einen umgekehrten Schrägstrich (\\) hinzu. Die Folgenden wird ein Beispiel gezeigt. Weitere Informationen zu Regeln für Anführungszeichen in Bash finden Sie unter [Doppelte Anführungszeichen](https://www.gnu.org/software/bash/manual/html_node/Double-Quotes.html).

```azurecli
condition="((!(ActionMatches{'Microsoft.Storage/storageAccounts/blobServices/containers/blobs/read'} AND @Request[subOperation] ForAnyOfAnyValues:StringEqualsIgnoreCase {'Blob.Read.WithTagConditions'})) OR (@Resource[Microsoft.Storage/storageAccounts/blobServices/containers/blobs/tags:Project<\$key_case_sensitive\$>] StringEquals 'Cascade'))"
```

## <a name="symptom---error-when-assigning-a-condition-string-to-a-variable-in-bash"></a>Symptom: Fehler beim Zuweisen einer Bedingungszeichenfolge zu einer Variablen in Bash

Wenn Sie versuchen, einer Variablen in Bash eine Bedingungszeichenfolge zuzuweisen, erhalten Sie die Meldung `bash: !: event not found`.

**Ursache**

Wenn Verlaufserweiterung in Bash aktiviert ist, wird die Meldung `bash: !: event not found` möglicherweise aufgrund des Ausrufezeichens (!) angezeigt.

**Lösung**

Deaktivieren Sie die Verlaufserweiterung mit dem Befehl `set +H`. Verwenden Sie `set -H`, um die Verlaufserweiterung erneut zu aktivieren.

## <a name="symptom---unrecognized-arguments-error-when-adding-a-condition-using-azure-cli"></a>Symptom: Fehler des Typs „Unbekannte Argumente“ beim Hinzufügen einer Bedingung mit der Azure CLI

Wenn Sie mit der Azure CLI versuchen, eine Rollenzuweisung mit einer Bedingung hinzuzufügen, erhalten Sie einen Fehler ähnlich dem folgenden:

`az: error: unrecognized arguments: --description {description} --condition {condition} --condition-version 2.0`

**Ursache**

Sie verwenden wahrscheinlich eine frühere Version von Azure CLI, die keine Bedingungsparameter für die Rollenzuweisung unterstützt.

**Lösung**

Führen Sie ein Update auf die neueste Version der Azure CLI (2.18 oder höher) aus. Weitere Informationen finden Sie unter [Installieren der Azure-Befehlszeilenschnittstelle](/cli/azure/install-azure-cli).

## <a name="symptom---condition-not-recognized-in-visual-editor"></a>Symptom: Bedingung wird im visuellen Editor nicht erkannt

Nachdem Sie den Code-Editor verwendet haben, wechseln Sie zum visuellen Editor und erhalten eine Meldung ähnlich der folgenden:

`The current expression cannot be recognized. Switch to the code editor to edit the expression or delete the expression and add a new one.`

**Ursache**

Die Bedingung, die der visuelle Editor nicht analysieren kann, wurde aktualisiert.

**Lösung**

Beheben Sie alle Probleme mit dem [Bedingungsformat oder der Syntax](conditions-format.md). Alternativ können Sie die Bedingung löschen und es erneut versuchen.

## <a name="symptom---error-when-copying-and-pasting-a-condition"></a>Symptom: Fehler beim Kopieren und Einfügen einer Bedingung

**Ursache**

Wenn Sie eine Bedingung aus einem Dokument kopieren, kann diese Sonderzeichen enthalten und Fehler verursachen. Einige Editoren (z. B. Microsoft Word) fügen beim Formatieren von Text, der nicht angezeigt wird, Steuerzeichen hinzu.

**Lösung**

Wenn Sie sicher sind, dass Ihre Bedingung richtig ist, löschen Sie alle Leerzeichen und Zeilenschaltungen, und fügen Sie die relevanten Leerzeichen dann erneut hinzu.

## <a name="next-steps"></a>Nächste Schritte

- [Format und Syntax von Azure-Rollenzuweisungsbedingungen (Vorschau)](conditions-format.md)
- [FAQ Bedingungen für Azure-Rollenzuweisungsbedingungen (Vorschau)](conditions-faq.md)
- [Beispiel für Azure-Rollenzuweisungsbedingungen (Vorschau)](../storage/common/storage-auth-abac-examples.md)
