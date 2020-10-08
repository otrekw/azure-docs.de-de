---
title: 'Versionshinweise: Azure Synapse Analytics (Arbeitsbereichsvorschau)'
description: Versionshinweise für Azure Synapse Analytics (Arbeitsbereichsvorschau)
services: synapse-analytics
author: julieMSFT
ms.service: synapse-analytics
ms.topic: overview
ms.subservice: overview
ms.date: 04/15/2020
ms.author: jrasnick
ms.reviewer: jrasnick
ms.openlocfilehash: f2050bf671af35df4faec8b0b1d53f4c10a9075e
ms.sourcegitcommit: eb6bef1274b9e6390c7a77ff69bf6a3b94e827fc
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/05/2020
ms.locfileid: "90031669"
---
# <a name="azure-synapse-analytics-workspaces-preview-release-notes"></a>Versionshinweise für Azure Synapse Analytics (Arbeitsbereichsvorschau)

In diesem Artikel werden Einschränkungen von und Probleme mit Azure Synapse Analytics (Arbeitsbereiche) beschrieben. Weitere Informationen finden Sie unter [Was ist Azure Synapse Analytics (Arbeitsbereiche)?](overview-what-is.md).

[!INCLUDE [preview](includes/note-preview.md)]

## <a name="azure-cli"></a>Azure CLI

- Problem und Kundenbeeinträchtigung: Mit dem SDK erstellte Arbeitsbereiche können Synapse Studio nicht starten.

- Problemumgehung: Führen Sie die folgenden Schritte aus: 
  1.    Erstellen Sie den Arbeitsbereich, indem Sie `az synapse workspace create` ausführen.
  2.    Extrahieren Sie die ID der verwalteten Identität, indem Sie `$identity=$(az synapse workspace show --name {workspace name}  --resource-group {resource group name} --query "identity.principalId")` ausführen.
  3.    Fügen Sie einen Arbeitsbereich als Rolle zum Speicherkonto hinzu, indem Sie ` az role assignment create --role "Storage Blob Data Contributor" --assignee-object-id {identity } --scope {storage account resource id}` ausführen.
  4.    Fügen Sie eine Firewallregel hinzu, indem Sie ` az synapse firewall-rule create --name allowAll --start-ip-address 0.0.0.0 --end-ip-address 255.255.255.255 ` ausführen.

## <a name="next-steps"></a>Nächste Schritte

* [Was ist Azure Synapse?](overview-what-is.md)
* [Erste Schritte](get-started.md)
* [Häufig gestellte Fragen](overview-faq.md)
