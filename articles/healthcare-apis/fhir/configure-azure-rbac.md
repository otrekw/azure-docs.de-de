---
title: Konfigurieren der rollenbasierten Zugriffssteuerung in Azure (Azure RBAC) für Azure API for FHIR
description: In diesem Artikel wird beschrieben, wie Sie Azure RBAC für die Azure API for FHIR-Datenebene konfigurieren.
author: matjazl
ms.service: healthcare-apis
ms.subservice: fhir
ms.topic: reference
ms.date: 03/15/2020
ms.author: matjazl
ms.reviewer: dseven
ms.openlocfilehash: fb57b689b77ec4cc6205c8885c2a2e062c469efb
ms.sourcegitcommit: 225e4b45844e845bc41d5c043587a61e6b6ce5ae
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/11/2021
ms.locfileid: "103018663"
---
# <a name="configure-azure-rbac-for-fhir"></a>Konfigurieren von Azure RBAC für FHIR 

In diesem Artikel erfahren Sie, wie Sie mithilfe der [rollenbasierten Zugriffssteuerung in Azure (Azure Role-Based Access Control, Azure RBAC)](../../role-based-access-control/index.yml) Zugriff auf die Azure API for FHIR-Datenebene zuweisen. Azure RBAC ist die bevorzugte Methode zum Zuweisen des Zugriffs auf die Datenebene, wenn Benutzer der Datenebene in dem Azure Active Directory-Mandanten verwaltet werden, der Ihrem Azure-Abonnement zugeordnet ist. Wenn Sie einen externen Azure Active Directory-Mandanten verwenden, finden Sie weitere Informationen in der [Referenz zum Zuweisen der lokalen RBAC](configure-local-rbac.md).

## <a name="confirm-azure-rbac-mode"></a>Überprüfen des Azure RBAC-Modus

Um Azure RBAC verwenden zu können, muss Azure API for FHIR so konfiguriert sein, dass Ihr Azure-Abonnementmandant für die Datenebene verwendet wird, und es sollten keine zugewiesenen Identitätsobjekt-IDs vorhanden sein. Sie können Ihre Einstellungen auf dem Blatt **Authentifizierung** von Azure API for FHIR überprüfen:

:::image type="content" source="media/rbac/confirm-azure-rbac-mode.png" alt-text="Überprüfen des Azure RBAC-Modus":::

Die **Autoritative Stelle** muss auf den Azure Active Directory-Mandanten festgelegt sein, der Ihrem Abonnement zugeordnet ist, und es dürfen keine GUIDs in dem Feld **Zulässige Objekt-IDs** vorhanden sein. Zudem ist das Kontrollkästchen deaktiviert, und eine Beschriftung gibt an, dass Azure RBAC zum Zuweisen von Datenebenenrollen verwendet werden soll.

## <a name="assign-roles"></a>Zuweisen von Rollen

Wenn Sie Benutzern, Dienstprinzipalen oder Gruppen Zugriff auf die FHIR-Datenebene gewähren möchten, klicken Sie auf **Zugriffssteuerung (IAM)** , dann auf **Rollenzuweisungen** und anschließend auf **+ Hinzufügen**:

:::image type="content" source="media/rbac/add-azure-rbac-role-assignment.png" alt-text="Hinzufügen einer Azure-Rollenzuweisung":::

Suchen Sie im Auswahlfeld **Rollen** nach einer der integrierten Rollen für die FHIR-Datenebene:

:::image type="content" source="media/rbac/built-in-fhir-data-roles.png" alt-text="Integrierte FHIR-Datenrollen":::

Sie können zwischen folgenden Rollen auswählen:

* FHIR-Datenleser: Kann FHIR-Daten lesen (und durchsuchen).
* FHIR-Datenschreiber: Kann FHIR-Daten lesen, schreiben und vorläufig löschen.
* FHIR-Datenexportierer: Kann Daten lesen und exportieren (`$export`-Operator).
* Mitwirkender an FHIR-Daten: Kann alle Vorgänge auf Datenebene durchführen.

Wenn diese Rollen für Ihre Zwecke nicht ausreichend sind, können Sie auch [benutzerdefinierte Rollen erstellen](../../role-based-access-control/tutorial-custom-role-powershell.md).

Suchen Sie im Feld **Auswählen** nach einem Benutzer, einem Dienstprinzipal oder einer Gruppe, dem bzw. der Sie die Rolle zuweisen möchten.

## <a name="caching-behavior"></a>Verhalten beim Zwischenspeichern

Die Azure API for FHIR nimmt eine Zwischenspeicherung von Entscheidungen für bis zu fünf Minuten vor. Wenn Sie einem Benutzer Zugriff auf den FHIR-Server gewähren, indem Sie ihn der Liste der zulässigen Objekt-IDs hinzufügen, oder wenn Sie ihn aus der Liste entfernen, sollten Sie davon ausgehen, dass es bis zu fünf Minuten dauert, bis die Berechtigungsänderungen weitergegeben sind.

## <a name="next-steps"></a>Nächste Schritte

In diesem Artikel haben Sie erfahren, wie Sie Azure-Rollen für die FHIR-Datenebene zuweisen. Weitere Informationen zu den zusätzlichen Einstellungen für Azure API for FHIR finden Sie unter:
 
>[!div class="nextstepaction"]
>[Zusätzliche Einstellungen für Azure API for FHIR](azure-api-for-fhir-additional-settings.md)