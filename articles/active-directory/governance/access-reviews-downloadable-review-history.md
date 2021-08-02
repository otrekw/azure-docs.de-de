---
title: Erstellen und Verwalten eines herunterladbaren Verlaufsberichts zur Zugriffsüberprüfung (Vorschau) – Azure Active Directory
description: Mit Azure Active Directory-Zugriffsüberprüfungen können Sie einen Überprüfungsverlauf für Zugriffsüberprüfungen in Ihrer Organisation herunterladen.
services: active-directory
documentationcenter: ''
author: ajburnle
manager: daveba
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.subservice: compliance
ms.date: 03/25/2021
ms.author: ajburnle
ms.openlocfilehash: 6f0d9b92b9aa3bb48533d5270c0fcd31aa8df82c
ms.sourcegitcommit: 7f59e3b79a12395d37d569c250285a15df7a1077
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/02/2021
ms.locfileid: "111026091"
---
# <a name="create-and-manage-downloadable-access-review-history-report-preview-in-azure-ad-access-reviews"></a>Erstellen und Verwalten eines herunterladbaren Verlaufsberichts zur Zugriffsüberprüfung (Vorschau) in Azure AD-Zugriffsüberprüfungen

Mit Azure AD-Zugriffsüberprüfungen (Azure Active Directory) können Sie einen herunterladbaren Überprüfungsverlauf erstellen, damit Ihre Organisation mehr Erkenntnisse erhält. Der Bericht pullt die Entscheidungen, die von Prüfern beim Erstellen eines Berichts getroffen wurden. Diese Berichte können so erstellt werden, dass sie bestimmte Zugriffsüberprüfungen für einen bestimmten Zeitrahmen enthalten, und sie können gefiltert werden, um verschiedene Überprüfungstypen und -ergebnisse einzuschließen.
 
## <a name="who-can-access-and-request-review-history"></a>Wer kann auf den Überprüfungsverlauf zugreifen und diesen anfordern?

Der Überprüfungsverlauf und der Anforderungsüberprüfungsverlauf sind für alle Benutzer verfügbar, die berechtigt sind, Zugriffsüberprüfungen anzuzeigen. Informationen dazu, welche Rollen Zugriffsüberprüfungen anzeigen und erstellen können, finden Sie unter [Welche Ressourcentypen können überprüft werden?](deploy-access-reviews.md#what-resource-types-can-be-reviewed). Die Rollen „Globaler Administrator“ und „Globaler Leser“ können alle Zugriffsüberprüfungen anzeigen. Alle anderen Benutzer dürfen nur Berichte zu Zugriffsüberprüfungen anzeigen, die sie generiert haben.

## <a name="how-to-create-a-review-history-report"></a>Erstellen eines Überprüfungsverlaufsberichts

**Erforderliche Rolle:** alle Benutzer, die zum Anzeigen von Zugriffsüberprüfungen autorisiert sind

1. Wählen Sie im Azure-Portal die Option **Azure Active Directory** und dann **Identity Governance** aus.

1. Wählen Sie im linken Menü unter **Zugriffsüberprüfungen** die Option **Überprüfungsverlauf** aus.
 
1. Wählen Sie **Neuer Bericht** aus. 

1. Geben Sie ein Start- und Enddatum für die Überprüfung an.

1. Wählen Sie die Überprüfungstypen und -ergebnisse aus, die Sie in den Bericht einschließen möchten. 

    ![Zugriffsüberprüfungen – Verlaufsbericht zur Zugriffsüberprüfung – Erstellen](./media/access-reviews-downloadable-review-history/create-review-history.png)

1. Wählen Sie dann **Erstellen** aus, um einen Verlaufsbericht zur Zugriffsüberprüfung zu erstellen.

## <a name="how-to-download-review-history-reports"></a>Herunterladen von Überprüfungsverlaufsberichten

Nachdem ein Überprüfungsverlaufsbericht erstellt wurde, können Sie ihn herunterladen. Alle erstellten Berichte stehen 30 Tage lang im CSV-Format zum Download zur Verfügung.

1. Wählen Sie unter **Identity Governance** die Option **Überprüfungsverlauf** aus. Es sind alle von Ihnen erstellten Berichte zum Überprüfungsverlauf verfügbar. 
1. Wählen Sie den Bericht aus, den Sie herunterladen möchten. 

## <a name="what-is-included-in-a-review-history-report"></a>Was ist in einem Überprüfungsverlaufsbericht enthalten?

Die Berichte enthalten die folgenden benutzerspezifischen Details:

| Elementname | BESCHREIBUNG |
| --- | --- |
| AccessReviewId |  ID des Überprüfungsobjekts |
| ReviewType | Zu den Überprüfungstypen gehören Gruppe, Anwendung, Azure AD-Rolle, Azure-Rolle und Zugriffspaket|
|ResourceDisplayName | Anzeigename der überprüften Ressource |
| resourceId | ID der überprüften Ressource |
| ReviewName |  Name der Überprüfung |
| CreatedDateTime | Datum/Uhrzeit der Erstellung der Überprüfung |
| ReviewStartDate | Startdatum der Überprüfung
| ReviewEndDate | Enddatum der Überprüfung |
| ReviewStatus | Status der Überprüfung. Sie finden alle Überprüfungsstatus in der [Tabelle mit Status der Zugriffsüberprüfung](create-access-review.md). |
| OwnerId | ID des Überprüfungsbesitzers |
| OwnerName | Name des Überprüfungsbesitzers |
| OwnerUPN | Benutzerprinzipalname des Überprüfungsbesitzers |
| PrincipalId | ID des überprüften Prinzipals |
| PrincipalName | Name des überprüften Prinzipals |
| PrincipalUPN | Prinzipalname des überprüften Benutzers |
| PrincipalType | Typ des Prinzipals. Zu den Optionen gehören „Benutzer“, „Gruppe“ und „Dienstprinzipal“. |
| ReviewDate | Datum der Überprüfung |
| ReviewResult | Zu den Überprüfungsergebnissen zählen „Verweigern“, „Genehmigen“ und „Nicht überprüft“. |
|Begründung | Begründung für das vom Prüfer bereitgestellte Überprüfungsergebnis |
| ReviewerId | ID des Prüfers |
| ReviewerName | Name des Prüfers |
| ReviewerUPN | Benutzerprinzipalname des Prüfers |
| ReviewerEmailAddress | E-Mail-Adresse des Prüfers |
| AppliedByName | Name des Benutzers, der das Überprüfungsergebnis angewandt hat |
| AppliedByUPN | Benutzerprinzipalname des Benutzers, der das Überprüfungsergebnis angewandt hat|
| AppliedByEmailAddress | E-Mail-Adresse des Benutzers, der das Überprüfungsergebnis angewandt hat |
| AppliedDate | Datum, an dem das Überprüfungsergebnis angewandt wurde |
| AccessRecommendation | Zu den Systemempfehlungen zählen „Genehmigen“, „Verweigern“ und „Keine Informationen“. |
|SubmissionResult | Zu den Übermittlungsstatus von Überprüfungsergebnissen gehören „Angewendet“ und „Nicht angewendet“. |

## <a name="next-steps"></a>Nächste Schritte
- [Überprüfen des Zugriffs auf Gruppen oder Anwendungen](perform-access-review.md)
- [Überprüfen des eigenen Zugriffs auf Gruppen oder Anwendungen](review-your-access.md)
- [Abschließen einer Zugriffsüberprüfung von Gruppen oder Anwendungen](complete-access-review.md)
