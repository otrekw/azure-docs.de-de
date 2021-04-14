---
title: 'Tutorial: Einstieg Hinzufügen eines Administrators'
description: In diesem Tutorial erfahren Sie, wie Sie Ihrem Arbeitsbereich einen weiteren Administrator hinzufügen.
services: synapse-analytics
author: saveenr
ms.author: saveenr
manager: julieMSFT
ms.reviewer: jrasnick
ms.service: synapse-analytics
ms.subservice: workspace
ms.topic: tutorial
ms.date: 04/02/2021
ms.openlocfilehash: 8b854dfcff7dfb4d03038b542308b6f3ebb6d491
ms.sourcegitcommit: b0557848d0ad9b74bf293217862525d08fe0fc1d
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/07/2021
ms.locfileid: "106553497"
---
# <a name="add-an-administrator-to-your-synapse-workspace"></a>Fügen Sie Ihrem Synapse-Arbeitsbereich einen Administrator hinzu.

In diesem Tutorial erfahren Sie, wie Sie Ihrem Synapse-Arbeitsbereich einen Administrator hinzufügen. Dieser Benutzer verfügt über vollständige Kontrolle über den Arbeitsbereich.

## <a name="overview"></a>Übersicht

Bisher haben wir im Leitfaden für die ersten Schritte im Mittelpunkt mit den Aktivitäten *den Aktivitäten die Sie im* Arbeitsbereich machen begonnen. Da Sie den Arbeitsbereich in Schritt 1 erstellt haben, sind Sie Administrator des Arbeitsbereichs Synapse. Nun werden wir ein anderer Benutzer Ryan () zu `ryan@contoso.com` einem Administrator machen. Nachdem der Vorgang abgeschlossen ist, kann Ryan alles ausführen, was Sie im Arbeitsbereich erledigen können.

## <a name="azure-rbac-owner-role-for-the-workspace"></a>Azure RBAC: Besitzer-Rolle für den Arbeitsbereich

Weisen Sie `ryan@contoso.com` der Azure RBAC- **Besitzer** Rolle im Arbeitsbereich zu.

1. Öffnen Sie die Azure-Portal, und öffnen Sie Ihren Synapse-Arbeitsbereich.
1. Wählen Sie auf der linken Seite **Zugriffssteuerung (IAM)** aus.
1. Hinzufügen `ryan@contoso.com` zur **Besitzer** rolle. 
1. Klicken Sie auf **Speichern**.
 
 
## <a name="synapse-rbac-synapse-administrator-role-for-the-workspace"></a>Synapse RBAC: Synapse-Administrator Rolle für den Arbeitsbereich

Weisen Sie `ryan@contoso.com` der Synapse RBAC **Synapse-Administrator** Rolle im Arbeitsbereich zu.

1. Öffnen Sie Ihren Arbeitsbereich in Azure Synapse Studio.
1. Klicken Sie auf der linken Seite auf **Verwalten**, um den Hub verwalten zu öffnen.
1. Klicken Sie unter **Sicherheit** auf **Zugriffs Steuerung**.
1. Klicken Sie auf **Hinzufügen**.
1. Lassen Sie den **Bereich** auf **Arbeitsbereich** eingestellt.
1. Fügen Sie die Rolle `ryan@contoso.com`**Synapse-Administrator** dazu. 
1. Klicken Sie anschließend auf **Übernehmen**.
 
## <a name="azure-rbac-role-assignments-on-the-primary-storage-account"></a>Azure RBAC: Rollenzuweisungen für das primäre Speicherkonto

Weisen Sie `ryan@contoso.com` die Rolle **Besitzer** im primären Speicherkonto des Arbeitsbereichs zu.
Weisen Sie `ryan@contoso.com` zu die **Azure Storage Blob Data Mitwirkender** Rolle im primären Speicherkonto des Arbeitsbereichs.

1. Öffnen Sie das Speicherkonto des primären Arbeitsbereichs im Azure-Portal.
1. Klicken Sie im linken Menü auf **Zugriffssteuerung (IAM)** .
1. Hinzufügen `ryan@contoso.com` zur **Besitzer** rolle. 
1. Weisen Sie  die Rolle `ryan@contoso.com`**Mitwirkender an Azure Storage-Blobdaten** zu.

## <a name="dedicated-sql-pools-db_owner-role"></a>Dedizierte SQL-Pools: db_owner Rolle

Weisen `ryan@contoso.com` Sie den **db_owner** für jeden dedizierten SQL-Pool im Arbeitsbereich zu.

```
CREATE USER [ryan@contoso.com] FROM EXTERNAL PROVIDER; 
EXEC sp_addrolemember 'db_owner', 'ryan@contoso.com'
```

## <a name="next-steps"></a>Nächste Schritte

* [Erste Schritte mit Azure Synapse Analytics](get-started.md)
* [Erstellen eines Arbeitsbereichs](quickstart-create-workspace.md)
* [Verwenden eines serverlosen SQL-Pools](quickstart-sql-on-demand.md)
