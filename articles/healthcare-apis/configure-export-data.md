---
title: Konfigurieren von Exporteinstellungen in Azure API for FHIR
description: In diesem Artikel wird beschrieben, wie Sie Exporteinstellungen in Azure API for FHIR konfigurieren.
author: matjazl
ms.service: healthcare-apis
ms.subservice: fhir
ms.topic: reference
ms.date: 3/5/2020
ms.author: matjazl
ms.openlocfilehash: 46a55b83b38593a514d40a9f75d99739a1efb8a2
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/02/2020
ms.locfileid: "84871020"
---
# <a name="configure-export-setting-and-export-the-data-to-a-storage-account"></a>Konfigurieren von Exporteinstellungen und Exportieren der Daten in ein Speicherkonto

Azure API for FHIR unterstützt den Befehl „$export“, mit dem Sie die Daten aus dem Azure API for FHIR-Konto in ein Speicherkonto exportieren können.

Zum Durchführen eines Exports in Azure API for FHIR sind vier Schritte erforderlich:

1. Aktivieren der verwalteten Identität für den Azure API for FHIR-Dienst
2. Erstellen eines Azure-Speicherkontos (falls noch nicht geschehen) und Zuweisen der Berechtigung für das Speicherkonto an Azure API for FHIR
3. Auswählen des Speicherkontos in Azure API for FHIR als Speicherkonto für den Export
4. Ausführen des Exports durch Aufrufen des Befehls „$export“ in Azure API for FHIR

## <a name="enabling-managed-identity-on-azure-api-for-fhir"></a>Aktivieren der verwalteten Identität für Azure API for FHIR

Der erste Schritt beim Konfigurieren von Azure API for FHIR für den Export besteht darin, die systemweite verwaltete Identität für den Dienst zu aktivieren. Umfassende Informationen zu verwalteten Identitäten in Azure finden Sie [hier](../active-directory/managed-identities-azure-resources/overview.md).

Navigieren Sie hierzu zum Azure API for FHIR-Dienst, und wählen Sie das Blatt „Identität“ aus. Wenn Sie den Status in „Ein“ ändern, wird die verwaltete Identität im Azure API for FHIR-Dienst aktiviert.

![Aktivieren der verwalteten Identität](media/export-data/fhir-mi-enabled.png)

Nun können Sie mit dem nächsten Schritt fortfahren, in dem Sie ein Speicherkonto erstellen und dem Dienst die Berechtigung zuweisen.

## <a name="adding-permission-to-storage-account"></a>Hinzufügen der Berechtigung für das Speicherkonto

Der nächste Schritt beim Exportieren besteht darin, dem Azure API for FHIR-Dienst die Berechtigung zum Schreiben in das Speicherkonto zuzuweisen.

Nachdem Sie ein Speicherkonto erstellt haben, navigieren Sie im Speicherkonto zum Blatt „Zugriffssteuerung (IAM)“, und wählen Sie „Rollenzuweisungen hinzufügen“ aus.

![Aktivieren der verwalteten Identität](media/export-data/fhir-export-role-assignment.png)

Hier fügen Sie dann dem Dienstnamen die Rolle „Mitwirkender an Storage-Blobdaten“ hinzu.

![Aktivieren der verwalteten Identität](media/export-data/fhir-export-role-add.png)

Nun kann der nächste Schritt ausgeführt werden, in dem Sie das Speicherkonto in Azure API for FHIR als Standardspeicherkonto für „$export“ auswählen können.

## <a name="selecting-the-storage-account-for-export"></a>Auswählen des Speicherkontos für „$export“

Im letzte Schritt vor dem Aufrufen des Befehls „$export“ wird das Azure-Speicherkonto zugewiesen, in das die Daten von Azure API for FHIR exportiert werden. Navigieren Sie hierzu im Azure-Portal im Azure API for FHIR-Dienst zum Blatt „Integration", und wählen Sie das Speicherkonto aus. 

![Aktivieren der verwalteten Identität](media/export-data/fhir-export-storage.png)

Anschließend können Sie die Daten mit dem Befehl „$export“ exportieren.

## <a name="exporting-the-data-using-export-command"></a>Exportieren der Daten mit dem Befehl „$export“

Nachdem Sie Azure API for FHIR für den Export konfiguriert haben, können Sie nun den Befehl „$export“ verwenden, um die Daten aus dem Dienst in das angegebene Speicherkonto zu exportieren. Informationen zum Aufrufen des Befehls „$export“ auf dem FHIR-Server finden Sie in der Dokumentation zur Spezifikation von „$export“ unter [https://hl7.org/Fhir/uv/bulkdata/export/index.html](https://hl7.org/Fhir/uv/bulkdata/export/index.html).

> [!IMPORTANT]
> Beachten Sie, dass Azure API for FHIR derzeit nur den Export auf Systemebene unterstützt, wie es in der Exportspezifikation unter [https://hl7.org/Fhir/uv/bulkdata/export/index.html](https://hl7.org/Fhir/uv/bulkdata/export/index.html) definiert ist. Außerdem werden derzeit keine Abfrageparameter bei „$export“ unterstützt.

>[!div class="nextstepaction"]
>[Zusätzliche Einstellungen](azure-api-for-fhir-additional-settings.md)