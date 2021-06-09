---
title: Konfigurieren von Exporteinstellungen in Azure API for FHIR
description: In diesem Artikel wird beschrieben, wie Sie Exporteinstellungen in Azure API for FHIR konfigurieren.
author: CaitlinV39
ms.service: healthcare-apis
ms.subservice: fhir
ms.topic: reference
ms.date: 5/11/2021
ms.author: cavoeg
ms.openlocfilehash: b245fcaecde13c4a751926e9b7bd639a43b36701
ms.sourcegitcommit: a434cfeee5f4ed01d6df897d01e569e213ad1e6f
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 06/09/2021
ms.locfileid: "111809044"
---
# <a name="configure-export-setting-and-set-up-the-storage-account"></a>Konfigurieren von Exporteinstellungen und Einrichten des Speicherkontos

Azure API for FHIR unterstützt den Befehl „$export“, mit dem Sie die Daten aus dem Azure API for FHIR-Konto in ein Speicherkonto exportieren können.

Zum Konfigurieren eines Exports in Azure API for FHIR sind drei Schritte erforderlich:

1. Aktivieren Sie die verwaltete Identität für Azure API for FHIR Service.
2. Erstellen eines Azure-Speicherkontos (sofern noch nicht geschehen) und Zuweisen der Berechtigung Azure API for FHIR dem Speicherkonto.
3. Wählen Sie das Speicherkonto in Azure API for FHIR als Exportspeicherkonto aus.

## <a name="enabling-managed-identity-on-azure-api-for-fhir"></a>Aktivieren der verwalteten Identität für Azure API for FHIR

Der erste Schritt beim Konfigurieren von Azure API for FHIR für den Export besteht darin, die systemweite verwaltete Identität für den Dienst zu aktivieren. Weitere Informationen zu verwalteten Identitäten in Azure finden Sie unter [Informationen zu verwalteten Identitäten für Azure-Ressourcen.](../../active-directory/managed-identities-azure-resources/overview.md)

Wechseln Sie dazu zum Dienst Azure API for FHIR, und wählen Sie **Identität aus.** Wenn Sie den Status in **Ein** ändern, wird die verwaltete Identität im Azure API for FHIR aktiviert.

![Aktivieren der verwalteten Identität](media/export-data/fhir-mi-enabled.png)

Nun können Sie zum nächsten Schritt wechseln, indem Sie ein Speicherkonto erstellen und dem Dienst Berechtigungen zuweisen.

## <a name="adding-permission-to-storage-account"></a>Hinzufügen der Berechtigung für das Speicherkonto

Der nächste Schritt beim Exportieren von Daten besteht im Zuweisen der Berechtigung für Azure API for FHIR Dienst zum Schreiben in das Speicherkonto.

Nachdem Sie ein Speicherkonto erstellt haben, wechseln Sie zum Access Control **(IAM)** im Speicherkonto, und wählen Sie dann **Rollenzuweisung hinzufügen aus.** 

Weitere Informationen zum Zuweisen von Rollen im Azure-Portal finden Sie unter [Integrierte Azure-Rollen.](../../role-based-access-control/role-assignments-portal.md)

Hier fügen Sie unserem Dienstnamen die Rolle Mitwirkender an [Storage-Blobdaten](https://docs.microsoft.com/azure/role-based-access-control/built-in-roles#storage-blob-data-contributor) hinzu, und wählen Sie dann **Speichern aus.**

![Seite „Rollenzuweisung hinzufügen“](../../../includes/role-based-access-control/media/add-role-assignment-page.png)

Nun können Sie das Speicherkonto in Azure API for FHIR als Standardspeicherkonto für die $export.

## <a name="selecting-the-storage-account-for-export"></a>Auswählen des Speicherkontos für „$export“

Der letzte Schritt besteht im Zuweisen des Azure-Speicherkontos, in Azure API for FHIR die Daten exportiert werden. Wechseln Sie hierzu zu **Integration** in Azure API for FHIR Service, und wählen Sie das Speicherkonto aus.

![FHIR-Exportspeicher](media/export-data/fhir-export-storage.png)

Nachdem Sie diesen letzten Schritt abgeschlossen haben, können Sie die Daten nun mithilfe des befehls $export exportieren.

> [!Note]
> Nur Speicherkonten im gleichen Abonnement wie für Azure API for FHIR können als Ziel für $export registriert werden.

Weitere Informationen zum Konfigurieren von Datenbankeinstellungen, Zugriffssteuerung, Aktivieren der Diagnoseprotokollierung und Verwenden benutzerdefinierter Header zum Hinzufügen von Daten zu Überwachungsprotokollen finden Sie unter:

>[!div class="nextstepaction"]
>[Zusätzliche Einstellungen](azure-api-for-fhir-additional-settings.md)
