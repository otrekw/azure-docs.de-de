---
title: Konfigurieren von Exporteinstellungen in Azure API for FHIR
description: In diesem Artikel wird beschrieben, wie Sie Exporteinstellungen in Azure API for FHIR konfigurieren.
author: matjazl
ms.service: healthcare-apis
ms.subservice: fhir
ms.topic: reference
ms.date: 5/11/2021
ms.author: matjazl
ms.openlocfilehash: 20c08851d082702342b43bad12409cc71a0cae46
ms.sourcegitcommit: 17345cc21e7b14e3e31cbf920f191875bf3c5914
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 05/19/2021
ms.locfileid: "110062018"
---
# <a name="configure-export-setting-and-set-up-the-storage-account"></a>Konfigurieren von Exporteinstellungen und Einrichten des Speicherkontos

Azure API for FHIR unterstützt den Befehl „$export“, mit dem Sie die Daten aus dem Azure API for FHIR-Konto in ein Speicherkonto exportieren können.

Zum Konfigurieren eines Exports in Azure API for FHIR sind drei Schritte erforderlich:

1. Aktivieren Sie die verwaltete Identität für Azure API for FHIR-Dienst.
2. Erstellen eines Azure-Speicherkontos (falls noch nicht geschehen) und Zuweisen der Berechtigung zum Azure API for FHIR zum Speicherkonto.
3. Wählen Sie das Speicherkonto in Azure API for FHIR als Exportspeicherkonto aus.

## <a name="enabling-managed-identity-on-azure-api-for-fhir"></a>Aktivieren der verwalteten Identität für Azure API for FHIR

Der erste Schritt beim Konfigurieren von Azure API for FHIR für den Export besteht darin, die systemweite verwaltete Identität für den Dienst zu aktivieren. Weitere Informationen zu verwalteten Identitäten in Azure finden Sie unter Informationen zu [verwalteten Identitäten für Azure-Ressourcen.](../../active-directory/managed-identities-azure-resources/overview.md)

Wechseln Sie hierzu zum Azure API for FHIR-Dienst, und wählen Sie **Identität** aus. Wenn Sie den Status in **Ein** ändern, wird die verwaltete Identität in Azure API for FHIR Service aktiviert.

![Aktivieren der verwalteten Identität](media/export-data/fhir-mi-enabled.png)

Nun können Sie mit dem nächsten Schritt fortschreiten, indem Sie ein Speicherkonto erstellen und unserem Dienst berechtigungen zuweisen.

## <a name="adding-permission-to-storage-account"></a>Hinzufügen der Berechtigung für das Speicherkonto

Der nächste Schritt beim Exportieren von Daten besteht darin, Azure API for FHIR Dienst die Berechtigung zum Schreiben in das Speicherkonto zuzuweisen.

Nachdem Sie ein Speicherkonto erstellt haben, wechseln Sie zum **Access Control (IAM)** im Speicherkonto, und wählen Sie dann **Rollenzuweisung hinzufügen** aus. 

Weitere Informationen zum Zuweisen von Rollen im Azure-Portal finden Sie unter [Integrierte Azure-Rollen.](../../role-based-access-control/role-assignments-portal.md)

Hier fügen Sie die Rolle [Mitwirkender an Storage-Blobdaten](https://docs.microsoft.com/azure/role-based-access-control/built-in-roles#storage-blob-data-contributor) zu unserem Dienstnamen hinzu und wählen dann **Speichern** aus.

![Seite „Rollenzuweisung hinzufügen“](../../../includes/role-based-access-control/media/add-role-assignment-page.png)

Nun können Sie das Speicherkonto in Azure API for FHIR als Standardspeicherkonto für $export auswählen.

## <a name="selecting-the-storage-account-for-export"></a>Auswählen des Speicherkontos für „$export“

Der letzte Schritt besteht darin, das Azure-Speicherkonto zuzuweisen, in das Azure API for FHIR die Daten exportieren. Wechseln Sie hierzu zu **Integration** in Azure API for FHIR Dienst, und wählen Sie das Speicherkonto aus.

![FHIR-Exportspeicher](media/export-data/fhir-export-storage.png)

Nachdem Sie diesen letzten Schritt abgeschlossen haben, können Sie die Daten jetzt mit $export Befehl exportieren.

> [!Note]
> Nur Speicherkonten im selben Abonnement wie für Azure API for FHIR dürfen als Ziel für $export Vorgänge registriert werden.

Weitere Informationen zum Konfigurieren von Datenbankeinstellungen, Zugriffssteuerung, Aktivieren der Diagnoseprotokollierung und Verwenden von benutzerdefinierten Headern zum Hinzufügen von Daten zu Überwachungsprotokollen finden Sie unter:

>[!div class="nextstepaction"]
>[Zusätzliche Einstellungen](azure-api-for-fhir-additional-settings.md)
