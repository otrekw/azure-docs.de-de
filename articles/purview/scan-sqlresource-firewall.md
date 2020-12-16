---
title: Überprüfen von Azure SQL-Datenbank hinter einer Firewall
description: Es wird beschrieben, wie Sie über das Azure Purview-Portal Ressourcen überprüfen, die sich hinter einer Firewall befinden.
author: sudhandkumar
ms.author: kumarsud
ms.service: purview
ms.subservice: purview-data-catalog
ms.topic: how-to
ms.date: 10/16/2020
ms.openlocfilehash: 4e1e76aaae2951e6d65fd1ebaaa5798ff417daf9
ms.sourcegitcommit: 65db02799b1f685e7eaa7e0ecf38f03866c33ad1
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 12/03/2020
ms.locfileid: "96550389"
---
# <a name="scan-azure-sql-db-behind-a-firewall-in-azure-purview"></a>Überprüfen von Azure SQL-Datenbank hinter einer Firewall in Azure Purview

In diesem Artikel wird beschrieben, wie Sie Azure Purview zum Überprüfen einer Ressource hinter einer Firewall verwenden.

## <a name="prerequisites"></a>Voraussetzungen

* Ein Azure-Konto mit einem aktiven Abonnement. Sie können [kostenlos ein Konto erstellen](https://azure.microsoft.com/free/?WT.mc_id=A261C142F).

* Ihr eigener [Azure Active Directory-Mandant](../active-directory/fundamentals/active-directory-access-create-new-tenant.md).

## <a name="sign-in-to-azure"></a>Anmelden bei Azure

Melden Sie sich mit Ihrem Azure-Konto beim [Azure-Portal](https://portal.azure.com) an.

## <a name="set-up-sql-storage-behind-a-firewall"></a>Einrichten von SQL-Speicher hinter einer Firewall

Der erste Schritt ist das Hinzufügen des MSI des Katalogs zu einer Azure SQL-Datenbank-Instanz. Informationen hierzu finden Sie unter [Registrieren und Überprüfen von Azure SQL-Datenbank](register-scan-azure-sql-database.md).

## <a name="scan-sql-db-from-purview"></a>Überprüfen von SQL-Datenbank mit Purview

1. Navigieren Sie zur Startseite von Purview.

1. Wählen Sie im linken Navigationsbereich die Option **Verwaltungscenter** aus.

1. Wählen Sie unter **Sources and scanning** (Quellen und Überprüfung) die Option **Datenquellen** aus.

1. Wählen Sie **+ Neu** aus, um die Datenquelle hinzuzufügen.

1. Wählen Sie **Azure SQL-Datenbank** aus.

   :::image type="content" source="./media/scan-resource-firewall/scan-sql-source.png" alt-text="Screenshot: Auswählen von SQL Server":::

1. Geben Sie einen Namen für die neue Datenquelle ein, und wählen Sie die Option **Aus Azure-Abonnement** und dann in der Dropdownliste Ihr Abonnement und den Servernamen aus.

   Wählen Sie **Fertig stellen** aus, um die Registrierung abzuschließen. 

   :::image type="content" source="./media/scan-resource-firewall/scan-sql-register.png" alt-text="Screenshot: Abschließen der Auswahl":::

1. Wählen Sie die Option **Set Up Scan** (Überprüfung einrichten) für den Speicher aus, der sich hinter der Firewall befindet, und testen Sie die Verbindung. Es wird angezeigt, dass die Verbindungsherstellung erfolgreich war. 

   :::image type="content" source="./media/scan-resource-firewall/scan-sql-setscan.png" alt-text="Screenshot: Auswahl von „t0“ zum Einrichten der Überprüfung":::

1. Richten Sie die Überprüfungshäufigkeit ein, und wählen Sie **Weiter** aus.

   :::image type="content" source="./media/scan-resource-firewall/scan-sql-once.png" alt-text="Screenshot: Auswahl zum Starten der SQL-Überprüfung":::

1.  Die Überprüfung wird abgeschlossen, und der Status wird in der Liste mit den Datenquellen aktualisiert.

   :::image type="content" source="./media/scan-resource-firewall/scan-sql-success.png" alt-text="Screenshot: Meldung zum Abschluss der Überprüfung":::
   
## <a name="next-steps"></a>Nächste Schritte

Verwenden Sie als Nächstes die Informationen unter [Erstellen eines Überprüfungsregelsatzes](create-a-scan-rule-set.md), um einen Überprüfungsregelsatz einzurichten und Überprüfungen zu gruppieren.
