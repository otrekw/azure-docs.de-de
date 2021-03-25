---
title: Verwalten eines Servers – Azure-Portal – Azure Database for PostgreSQL – Flexible Server
description: In diesem Artikel erfahren Sie, wie Sie Azure Database for PostgreSQL – Flexible Server über das Azure-Portal verwalten.
author: mksuni
ms.author: sumuth
ms.service: postgresql
ms.topic: how-to
ms.date: 09/22/2020
ms.custom: mvc
ms.openlocfilehash: 1ac418d855696138341115412dc7e2601d4cf3a1
ms.sourcegitcommit: 772eb9c6684dd4864e0ba507945a83e48b8c16f0
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/19/2021
ms.locfileid: "91961407"
---
# <a name="manage-an-azure-database-for-postgresql---flexible-server-using-the-azure-portal"></a>Verwalten von Azure Database for PostgreSQL – Flexible Server mithilfe des Azure-Portals

> [!IMPORTANT]
> Azure Database for PostgreSQL – Flexible Server befindet sich in der Vorschau.

In diesem Artikel erfahren Sie, wie Sie Ihre Instanz von Azure Database for PostgreSQL – Flexible Server verwalten. Zu den Verwaltungsaufgaben gehören die Compute- und Speicherskalierung, das Zurücksetzen des Administratorkennworts und das Anzeigen von Serverdetails.

## <a name="sign-in"></a>Anmelden

Melden Sie sich beim [Azure-Portal](https://portal.azure.com) an. Navigieren Sie zu Ihrer flexiblen Serverressource im Azure-Portal.

## <a name="scale-compute-and-storage"></a>Skalieren von Compute und Speicher

Nach der Servererstellung können Sie zwischen verschiedenen [Tarifen](https://azure.microsoft.com/pricing/details/postgresql/) skalieren, wenn sich Ihre Anforderungen ändern. Sie können auch Ihre Computeressourcen und den Arbeitsspeicher skalieren, indem Sie die Anzahl der virtuellen Kerne vergrößern oder verkleinern.

> [!NOTE]
> Der Speicher kann nicht auf einen kleineren Wert herunterskaliert werden.

1. Wählen Sie Ihren Server im Azure-Portal aus. Klicken Sie im Abschnitt **Einstellungen** auf **Compute + Storage** (Compute + Speicher).
2. Sie können den **Computetarif** ändern, um den Server hochzuskalieren, oder die Anzahl der **virtuellen Kerne** oder den **Speicher** an den gewünschten Wert anpassen, um innerhalb des gleichen Computetarifs hochzuskalieren.

> [!div class="mx-imgBorder"]
> :::image type="content" source="./media/howto-manage-server-portal/scale-server.png" alt-text="Skalieren des Speichers, Flexible Server":::

> [!Important]
> - Der Speicher kann nicht herunterskaliert werden.
> - Das Skalieren der virtuellen Kerne löst einen Neustart des Servers aus.

3. Wählen Sie **OK** aus, um die Änderungen zu speichern.

## <a name="reset-admin-password"></a>Zurücksetzen des Administratorkennworts

Sie können das Kennwort für die Administratorrolle über das Azure-Portal ändern.

1. Wählen Sie Ihren Server im Azure-Portal aus. Wählen Sie im Fenster **Übersicht** die Option **Kennwort zurücksetzen** aus.
2. Geben Sie ein neues Kennwort ein, und bestätigen Sie es. Im Textfeld werden Sie nach den Anforderungen an die Kennwortkomplexität gefragt.

> [!div class="mx-imgBorder"]
> :::image type="content" source="./media/howto-manage-server-portal/reset-password.png" alt-text="Zurücksetzen Ihres Kennworts für Flexible Server":::

3. Wählen Sie **Speichern** aus, um das neue Kennwort zu speichern.

## <a name="delete-a-server"></a>Löschen eines Servers

Sie können Ihren Server löschen, wenn Sie ihn nicht mehr benötigen.

1. Wählen Sie Ihren Server im Azure-Portal aus. Wählen Sie im Fenster **Übersicht** die Option **Löschen** aus.
2. Geben Sie den Namen des Servers in das Eingabefeld ein, um zu bestätigen, dass Sie den Server löschen möchten.

   :::image type="content" source="./media/howto-manage-server-portal/delete-server.png" alt-text="Löschen des flexiblen Servers":::

   > [!IMPORTANT]
   > Das Löschen eines Servers kann nicht mehr rückgängig gemacht werden.

  > [!div class="mx-imgBorder"]
  > ![Löschen des flexiblen Servers](./media/howto-manage-server-portal/delete-server.png)  

3. Klicken Sie auf **Löschen**.

## <a name="next-steps"></a>Nächste Schritte

- [Grundlegendes zu Sicherungs- und Wiederherstellungskonzepten](concepts-backup-restore.md)
- [Optimieren und Überwachen des Servers](concepts-monitoring.md)
