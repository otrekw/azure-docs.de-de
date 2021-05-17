---
title: Voraussetzungen für Azure-Rollenzuweisungsbedingungen (Vorschau)
description: Voraussetzungen für Azure-Rollenzuweisungsbedingungen (Vorschau).
services: active-directory
author: rolyon
manager: mtillman
ms.service: role-based-access-control
ms.subservice: conditions
ms.topic: conceptual
ms.workload: identity
ms.date: 05/06/2021
ms.author: rolyon
ms.openlocfilehash: e0da284f7ce9ac1c8699a6f22f2144ff6f3f8c80
ms.sourcegitcommit: 1fbd591a67e6422edb6de8fc901ac7063172f49e
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/07/2021
ms.locfileid: "109489647"
---
# <a name="prerequisites-for-azure-role-assignment-conditions-preview"></a>Voraussetzungen für Azure-Rollenzuweisungsbedingungen (Vorschau)

> [!IMPORTANT]
> Azure RBAC und Azure-Rollenzuweisungsbedingungen befinden sich derzeit in der Vorschauphase.
> Diese Vorschauversion wird ohne Vereinbarung zum Servicelevel bereitgestellt und ist nicht für Produktionsworkloads vorgesehen. Manche Features werden möglicherweise nicht unterstützt oder sind nur eingeschränkt verwendbar.
> Weitere Informationen finden Sie unter [Zusätzliche Nutzungsbestimmungen für Microsoft Azure-Vorschauen](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).

Zum Hinzufügen oder Bearbeiten von Bedingungen für die Azure-Rollenzuweisung müssen die folgenden Voraussetzungen erfüllt sein.

## <a name="storage-accounts"></a>Speicherkonten

Für Bedingungen, die Blobindextags verwenden, müssen Sie ein Speicherkonto verwenden, das mit der Blobindexfunktion kompatibel ist. Beispielsweise werden derzeit nur Speicherkonten des Typs „Universell v2“ (GPv2) mit einem deaktivierten hierarchischen Namespace (HNS) unterstützt. Weitere Informationen finden Sie unter [Verwalten und Finden von Azure-Blobdaten mit Blobindextags (Vorschau)](../storage/blobs/storage-manage-find-blobs.md#regional-availability-and-storage-account-support).

## <a name="azure-powershell"></a>Azure PowerShell

Wenn Sie Azure PowerShell zum Hinzufügen oder Aktualisieren von Bedingungen verwenden, müssen Sie die folgenden Versionen verwenden:

- [Az-Modul 5.5.0 oder höher](https://www.powershellgallery.com/packages/Az/5.5.0)
- [Az.Resources-Modul 3.2.1 oder höher](https://www.powershellgallery.com/packages/Az.Resources/3.2.1)
    - Im Az-Modul v5.5.0 und höher enthalten, kann jedoch manuell über den PowerShell-Katalog installiert werden
- [Az.Storage-Vorschaumodul 2.5.2-preview oder höher](https://www.powershellgallery.com/packages/Az.Storage/2.5.2-preview)

## <a name="azure-cli"></a>Azure CLI

Wenn Sie die Azure CLI zum Hinzufügen oder Aktualisieren von Bedingungen verwenden, müssen Sie die folgenden Versionen verwenden:

- [Azure CLI 2.18 oder höher](/cli/azure/install-azure-cli)

## <a name="permissions"></a>Berechtigungen

Genau wie bei Rollenzuweisungen müssen Sie zum Hinzufügen oder Aktualisieren von Bedingungen als Benutzer in Azure angemeldet sein, der über die Berechtigungen `Microsoft.Authorization/roleAssignments/write` und `Microsoft.Authorization/roleAssignments/delete` verfügt, z. B. [Benutzerzugriffsadministrator](built-in-roles.md#user-access-administrator) oder [Besitzer](built-in-roles.md#owner).

## <a name="next-steps"></a>Nächste Schritte

- [Beispiel für Azure-Rollenzuweisungsbedingungen (Vorschau)](../storage/common/storage-auth-abac-examples.md)
- [Tutorial: Hinzufügen einer Rollenzuweisungsbedingung zum Einschränken des Zugriffs auf Blobs mit dem Azure-Portal (Vorschau)](../storage/common/storage-auth-abac-portal.md)
