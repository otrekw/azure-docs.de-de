---
title: Konfigurieren von Exporteinstellungen in Azure API for FHIR
description: In diesem Artikel wird beschrieben, wie Sie Exporteinstellungen in Azure API for FHIR konfigurieren.
author: matjazl
ms.service: healthcare-apis
ms.subservice: fhir
ms.topic: reference
ms.date: 3/5/2020
ms.author: matjazl
ms.openlocfilehash: e4adceea5c2cd2a36d7a867ca9b9d2ad7c33c155
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/09/2020
ms.locfileid: "91529982"
---
# <a name="configure-export-setting-and-set-up-the-storage-account"></a>Konfigurieren von Exporteinstellungen und Einrichten des Speicherkontos

Azure API for FHIR unterstützt den Befehl „$export“, mit dem Sie die Daten aus dem Azure API for FHIR-Konto in ein Speicherkonto exportieren können.

Zum Konfigurieren eines Exports in Azure API for FHIR sind drei Schritte erforderlich:

1. Aktivieren der verwalteten Identität für den Azure API for FHIR-Dienst
2. Erstellen eines Azure-Speicherkontos (falls noch nicht geschehen) und Zuweisen der Berechtigung für das Speicherkonto an Azure API for FHIR
3. Auswählen des Speicherkontos in Azure API for FHIR als Speicherkonto für den Export

## <a name="enabling-managed-identity-on-azure-api-for-fhir"></a>Aktivieren der verwalteten Identität für Azure API for FHIR

Der erste Schritt beim Konfigurieren von Azure API for FHIR für den Export besteht darin, die systemweite verwaltete Identität für den Dienst zu aktivieren. Umfassende Informationen zu verwalteten Identitäten in Azure finden Sie [hier](../active-directory/managed-identities-azure-resources/overview.md).

Navigieren Sie hierzu zum Azure API for FHIR-Dienst, und wählen Sie das Blatt „Identität“ aus. Wenn Sie den Status in „Ein“ ändern, wird die verwaltete Identität im Azure API for FHIR-Dienst aktiviert.

![Aktivieren der verwalteten Identität](media/export-data/fhir-mi-enabled.png)

Nun können Sie mit dem nächsten Schritt fortfahren, in dem Sie ein Speicherkonto erstellen und dem Dienst die Berechtigung zuweisen.

## <a name="adding-permission-to-storage-account"></a>Hinzufügen der Berechtigung für das Speicherkonto

Der nächste Schritt beim Exportieren besteht darin, dem Azure API for FHIR-Dienst die Berechtigung zum Schreiben in das Speicherkonto zuzuweisen.

Nachdem Sie ein Speicherkonto erstellt haben, navigieren Sie im Speicherkonto zum Blatt „Zugriffssteuerung (IAM)“, und wählen Sie „Rollenzuweisungen hinzufügen“ aus.

![Exportieren der Rollenzuweisung](media/export-data/fhir-export-role-assignment.png)

Hier fügen Sie dann dem Dienstnamen die Rolle „Mitwirkender an Storage-Blobdaten“ hinzu.

![Add Role](media/export-data/fhir-export-role-add.png)

Nun kann der nächste Schritt ausgeführt werden, in dem Sie das Speicherkonto in Azure API for FHIR als Standardspeicherkonto für „$export“ auswählen können.

## <a name="selecting-the-storage-account-for-export"></a>Auswählen des Speicherkontos für „$export“

Im letzten Schritt vor dem Aufrufen des Befehls „$export“ wird das Azure-Speicherkonto zugewiesen, in das die Daten von Azure API for FHIR exportiert werden. Navigieren Sie hierzu im Azure-Portal im Azure API for FHIR-Dienst zum Blatt „Integration", und wählen Sie das Speicherkonto aus.

![FHIR-Exportspeicher](media/export-data/fhir-export-storage.png)

Anschließend können Sie die Daten mit dem Befehl „$export“ exportieren.

>[!div class="nextstepaction"]
>[Zusätzliche Einstellungen](azure-api-for-fhir-additional-settings.md)
