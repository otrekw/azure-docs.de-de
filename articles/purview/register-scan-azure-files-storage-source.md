---
title: Durchführen einer Überprüfung für Azure Files
description: In diesem Leitfaden wird beschrieben, wie Sie eine Überprüfung für Azure Files durchführen.
author: SunetraVirdi
ms.author: suvirdi
ms.service: purview
ms.subservice: purview-data-catalog
ms.topic: how-to
ms.date: 05/08/2021
ms.openlocfilehash: c88134e978615d53bdfbde26492096212c3f582f
ms.sourcegitcommit: 3de22db010c5efa9e11cffd44a3715723c36696a
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/10/2021
ms.locfileid: "109656403"
---
# <a name="register-and-scan-azure-files"></a>Registrieren und Überprüfen von Azure Files

## <a name="supported-capabilities"></a>Unterstützte Funktionen

Azure Files unterstützt vollständige und inkrementelle Überprüfungen, um die Metadaten zu erfassen und basierend auf System- und Kundenklassifizierungen entsprechende Klassifizierungen auf die Metadaten anzuwenden.

## <a name="prerequisites"></a>Voraussetzungen

- Erstellen Sie vor dem Registrieren der Datenquellen zunächst ein Azure Purview-Konto. Weitere Informationen zum Erstellen eines Purview-Kontos finden Sie unter [Schnellstart: Erstellen eines Azure Purview-Kontos im Azure-Portal](create-catalog-portal.md).
- Sie müssen ein Datenquellenadministrator sein, um Überprüfungen einrichten und planen zu können. Ausführlichere Informationen hierzu finden Sie unter [Katalogberechtigungen](catalog-permissions.md).

## <a name="register-an-azure-files-storage-account"></a>Registrieren eines Azure Files-Speicherkontos

Gehen Sie wie folgt vor, um in Ihrem Datenkatalog ein neues Azure Files-Konto zu registrieren:

1. Navigieren Sie zu Ihrem Purview-Datenkatalog.
1. Wählen Sie im linken Navigationsbereich die Option **Verwaltungscenter** aus.
1. Wählen Sie unter **Sources and scanning** (Quellen und Überprüfung) die Option **Datenquellen** aus.
1. Wählen Sie **+ Neu** aus.
1. Wählen Sie unter **Register sources** (Quellen registrieren) die Option **Azure Files** aus. Wählen Sie **Weiter**.

:::image type="content" source="media/register-scan-azure-files/register-new-data-source.png" alt-text="Registrieren einer neuen Datenquelle" border="true":::

Gehen Sie auf dem Bildschirm **Register sources (Azure Files)** (Quellen registrieren (Azure Files)) wie folgt vor:

1. Geben Sie unter **Name** einen Namen ein, unter dem die Datenquelle im Katalog aufgeführt werden soll.
2. Wählen Sie Ihr Azure-Abonnement aus, um Azure Storage-Konten zu filtern.
3. Wählen Sie ein Azure Storage-Konto aus.
4. Wählen Sie eine Sammlung aus, oder erstellen Sie eine neue Sammlung (optional).
5. Wählen Sie **Registrieren** aus, um die Datenquelle zu registrieren.

:::image type="content" source="media/register-scan-azure-files/register-sources.png" alt-text="Optionen für die Quellenregistrierung" border="true":::

## <a name="set-up-authentication-for-a-scan"></a>Einrichten der Authentifizierung für eine Überprüfung

Gehen Sie wie folgt vor, um die Authentifizierung für Azure Files-Speicher mit einem Kontoschlüssel einzurichten:

1. Wählen Sie **Kontoschlüssel** als Authentifizierungsmethode aus.
2. Wählen Sie die Option **Aus Azure-Abonnement** aus.
3. Wählen Sie Ihr Azure-Abonnement aus, in dem das Azure Files-Konto enthalten ist.
4. Wählen Sie in der Liste den Namen Ihres Speicherkontos aus.
5. Klicken Sie auf **Fertig stellen**.

[!INCLUDE [create and manage scans](includes/manage-scans.md)]

## <a name="next-steps"></a>Nächste Schritte

- [Browsen im Azure Purview-Datenkatalog](how-to-browse-catalog.md)
- [Suchen im Azure Purview-Datenkatalog](how-to-search-catalog.md)