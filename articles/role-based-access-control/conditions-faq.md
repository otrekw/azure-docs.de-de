---
title: FAQ Bedingungen für Azure-Rollenzuweisungsbedingungen (Vorschau)
description: Häufig gestellte Fragen zu Bedingungen für die Azure-Rollenzuweisung (Vorschauversion)
services: active-directory
author: rolyon
manager: mtillman
ms.service: role-based-access-control
ms.subservice: conditions
ms.topic: conceptual
ms.workload: identity
ms.date: 05/06/2021
ms.author: rolyon
ms.openlocfilehash: 93eb1afb061182105766ce4d7c864601883ec0cc
ms.sourcegitcommit: 1fbd591a67e6422edb6de8fc901ac7063172f49e
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/07/2021
ms.locfileid: "109489841"
---
# <a name="faq-for-azure-role-assignment-conditions-preview"></a>FAQ Bedingungen für Azure-Rollenzuweisungsbedingungen (Vorschau)

> [!IMPORTANT]
> Azure RBAC und Azure-Rollenzuweisungsbedingungen befinden sich derzeit in der Vorschauphase.
> Diese Vorschauversion wird ohne Vereinbarung zum Servicelevel bereitgestellt und ist nicht für Produktionsworkloads vorgesehen. Manche Features werden möglicherweise nicht unterstützt oder sind nur eingeschränkt verwendbar.
> Weitere Informationen finden Sie unter [Zusätzliche Nutzungsbestimmungen für Microsoft Azure-Vorschauen](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).

## <a name="frequently-asked-questions"></a>Häufig gestellte Fragen

**Kann man die Speichercontainernamen oder den Blobpfad im Bedingungs-Generator in der Azure-Portal auswählen?**

Sie müssen den Namen des Speichercontainers, den Blobpfad, den Tagnamen oder die Werte in die Bedingung schreiben. Es gibt keine Auswahlmöglichkeit für die Attributwerte.

**Kann Ausdrücke gruppieren?**

Wenn Sie drei oder mehr Ausdrücke für eine zielorientierte Aktion hinzufügen, müssen Sie die logische Gruppierung dieser Ausdrücke im Code-Editor, im Azure PowerShell oder im Azure CLI definieren. Eine logische Gruppierung von `a AND b OR c` kann entweder `(a AND b) OR c` oder `a AND (b OR c )` sein.

**Kann man Bedingungen für Rollenzuweisungen im Verwaltungsgruppenbereich hinzufügen?**
  
Die Azure-Portal ermöglicht es Ihnen nicht, eine Bedingung im Verwaltungsgruppenbereich zu bearbeiten oder anzeigen. Die Spalte **Bedingung** wird für den Verwaltungsgruppenbereich nicht angezeigt. Mit Azure PowerShell und Azure CLI können Sie Bedingungen im Verwaltungsgruppenbereich hinzufügen.

**Werden Bedingungen über Azure AD Privileged Identity Management (PIM) für Azure-Ressourcen in der Vorschau unterstützt?**

Nein.

**Werden Bedingungen für klassische Administratoren unterstützt?**

Nein. 

**Kann man benutzerdefinierten Rollenzuweisungen Bedingungen hinzufügen?**

Ja, solange die benutzerdefinierte Rolle [Aktionen enthält, die Bedingungen unterstützen](conditions-format.md#actions).
 
**Erhöhen die Bedingungen die Latenz für den Zugriff auf Speicherblobs?**

Nein. Basierend auf unseren Benchmarktests ist nicht zu erwarten, dass Bedingungen eine spürbare Latenz hinzufügen.

**Welche neuen Eigenschaften wurden im Rollenzuweisungsschema eingeführt, um Bedingungen zu unterstützen?**

Dies sind die neuen Bedingungseigenschaften:

- `condition`: Bedingungsaufstellung, die mit einer oder mehreren Aktionen aus Rollendefinition und Attributen erstellt wurde.
- `conditionVersion`: Eine Versionsnummer der Bedingung. Der Standardwert ist 2.0 und ist die einzige öffentlich unterstützte Version.

Es gibt auch eine neue Beschreibungseigenschaft für Rollenzuweisungen:

- `description`: Die Beschreibung der Rollenzuweisung, die zum Beschreiben der Bedingung verwendet werden kann.

**Wird eine Bedingung auf die gesamte Rollenzuweisung oder bestimmte Aktionen angewendet?**

Eine Bedingung wird nur auf die spezifischen Zielaktionen angewendet.

**Welche Grenzwerte gelten für eine Bedingung?**

Eine Bedingung kann bis zu 8 KB lang sein.

**Welche Grenzwerte gelten für eine Beschreibung?**

Eine Beschreibung kann bis zu 2 KB lang sein.

**Ist es möglich, eine Rollenzuweisung mit und ohne Bedingung zu erstellen, aber das gleiche Tupel aus Sicherheitsprinzipal, Rollendefinition und Bereich zu verwenden?**

Nein, wenn Sie versuchen, diese Rollenzuweisung zu erstellen, wird ein Fehler angezeigt.

**Bieten Bedingungen in Rollenzuweisungen eine explizite Verweigerungswirkung?**

Nein, Bedingungen in Rollenzuweisungen bieten keine explizite Verweigerungswirkung. Bedingungen in Rollenzuweisungen filtern den Zugriff, der in einer Rollenzuweisung gewährt wird, was dazu führen kann, dass der Zugriff nicht zulässig ist. Die explizite Verweigerungswirkung ist Teil von Verweigerungszuweisungen.

## <a name="next-steps"></a>Nächste Schritte

- [Format und Syntax von Azure-Rollenzuweisungsbedingungen (Vorschau)](conditions-format.md)
- [Problembehandlung: Bedingungen für die Azure-Rollenzuweisung (Vorschau)](conditions-troubleshoot.md)
