---
title: Konfigurieren von Exporteinstellungen in Azure API for FHIR
description: In diesem Artikel wird beschrieben, wie Sie Exporteinstellungen in Azure API for FHIR konfigurieren.
author: matjazl
ms.service: healthcare-apis
ms.subservice: fhir
ms.topic: reference
ms.date: 3/18/2021
ms.author: matjazl
ms.openlocfilehash: ee110420c697afb6ecad857ba823c61d03c6be6c
ms.sourcegitcommit: ed7376d919a66edcba3566efdee4bc3351c57eda
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/24/2021
ms.locfileid: "105046986"
---
# <a name="configure-export-setting-and-set-up-the-storage-account"></a>Konfigurieren von Exporteinstellungen und Einrichten des Speicherkontos

Azure API for FHIR unterstützt den Befehl „$export“, mit dem Sie die Daten aus dem Azure API for FHIR-Konto in ein Speicherkonto exportieren können.

Zum Konfigurieren eines Exports in Azure API for FHIR sind drei Schritte erforderlich:

1. Aktivieren Sie die verwaltete Identität auf der Azure-API für den Dienst
2. Erstellen eines Azure-Speicher Kontos (falls noch nicht geschehen) und Zuweisen der Berechtigung für die Azure-API für fhir für das Speicherkonto.
3. Wählen Sie das Speicherkonto in der Azure-API für das Speicherkonto exportieren als Speicherkonto aus.

## <a name="enabling-managed-identity-on-azure-api-for-fhir"></a>Aktivieren der verwalteten Identität für Azure API for FHIR

Der erste Schritt beim Konfigurieren von Azure API for FHIR für den Export besteht darin, die systemweite verwaltete Identität für den Dienst zu aktivieren. Weitere Informationen zu verwalteten Identitäten in Azure finden Sie unter Informationen [zu verwalteten Identitäten für Azure-Ressourcen](../../active-directory/managed-identities-azure-resources/overview.md).

Wechseln Sie zu diesem Zweck zum Dienst Azure-API für fhir, und wählen Sie **Identität** aus. Wenn Sie den Status in on ändern, wird die verwaltete Identität in Azure-API für **den** fhir-Dienst aktiviert.

![Aktivieren der verwalteten Identität](media/export-data/fhir-mi-enabled.png)

Nun können Sie mit dem nächsten Schritt fortfahren, indem Sie ein Speicherkonto erstellen und dem Dienst die Berechtigung zuweisen.

## <a name="adding-permission-to-storage-account"></a>Hinzufügen der Berechtigung für das Speicherkonto

Der nächste Schritt beim Export besteht darin, die Berechtigung für den Azure-API für den fhir-Dienst zum Schreiben in das Speicherkonto zuzuweisen.

Nachdem Sie ein Speicherkonto erstellt haben, navigieren Sie zu **Access Control (IAM)** im Speicherkonto, und wählen Sie **Rollenzuweisung hinzufügen** aus.

![Exportieren der Rollenzuweisung](media/export-data/fhir-export-role-assignment.png)

Hier fügen Sie die Rollen Speicher-BLOB-Daten, die dem Dienstnamen **Mitwirkender** , hinzu und wählen dann **Speichern** aus.

![Add Role](media/export-data/fhir-export-role-add.png)

Nun können Sie das Speicherkonto in der Azure-API für fhir als Standard Speicherkonto für $Export auswählen.

## <a name="selecting-the-storage-account-for-export"></a>Auswählen des Speicherkontos für „$export“

Der letzte Schritt besteht darin, das Azure-Speicherkonto zuzuweisen, das von der Azure-API für fhir zum Exportieren der Daten in verwendet wird. Navigieren Sie hierzu zu **Integration** in Azure API for fhir Service, und wählen Sie das Speicherkonto aus.

![FHIR-Exportspeicher](media/export-data/fhir-export-storage.png)

Nachdem Sie den letzten Schritt abgeschlossen haben, können Sie die Daten mit $Export Befehl exportieren.

> [!Note]
> Es können nur Speicher Konten, die sich in demselben Abonnement wie für die Azure-API für fhir befinden, als Ziel für $Export Vorgänge registriert werden.

Weitere Informationen zum Konfigurieren von Datenbankeinstellungen, Zugriffs Steuerung, Aktivieren der Diagnoseprotokollierung und Verwenden von benutzerdefinierten Headern zum Hinzufügen von Daten zu Überwachungsprotokollen finden Sie unter:

>[!div class="nextstepaction"]
>[Zusätzliche Einstellungen](azure-api-for-fhir-additional-settings.md)
