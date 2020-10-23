---
title: Regionale Verfügbarkeit und Datenresidenz
titleSuffix: Azure AD B2C
description: Hier finden Sie Informationen zur regionalen Verfügbarkeit, zur Datenresidenz und zu Azure Active Directory B2C-Vorschaumandanten.
services: active-directory-b2c
author: msmimart
manager: celestedg
ms.service: active-directory
ms.workload: identity
ms.topic: reference
ms.date: 06/06/2020
ms.author: mimart
ms.subservice: B2C
ms.custom: references_regions
ms.openlocfilehash: f8c6f7daecd38babaa4f2961d04a6cd4c3b4dbed
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/09/2020
ms.locfileid: "91840556"
---
# <a name="azure-active-directory-b2c-region-availability--data-residency"></a>Azure Active Directory B2C: Regionale Verfügbarkeit und Datenresidenz

Regionale Verfügbarkeit und Datenresidenz sind zwei sehr unterschiedliche Konzepte, die in Azure AD B2C anders als in den restlichen Komponenten von Azure angewendet werden. In diesem Artikel werden die Unterschiede zwischen diesen beiden Konzepten erläutert und ihre Anwendung in Azure und Azure AD B2C miteinander verglichen.

Azure AD B2C ist **grundsätzlich weltweit verfügbar**, mit der Option für **Datenresidenz** in den **USA, in Europa und in der Region „Asien-Pazifik“** .

[Regionale Verfügbarkeit](#region-availability) gibt an, wo ein Dienst für die Verwendung verfügbar ist.

[Datenresidenz](#data-residency) gibt an, wo die Benutzerdaten gespeichert werden.

## <a name="region-availability"></a>Regionale Verfügbarkeit

Azure AD B2C ist über die öffentliche Azure-Cloud weltweit verfügbar.

Dies unterscheidet sich von dem Modell der meisten anderen Azure-Dienste, in dem *Verfügbarkeit* und *Datenresidenz* gekoppelt sind. Beispiele hierfür finden Sie auf der Seite [Verfügbare Produkte nach Region](https://azure.microsoft.com/regions/services/) und der Seite [Azure Active Directory B2C – Preise](https://azure.microsoft.com/pricing/details/active-directory-b2c/) von Azure.

## <a name="data-residency"></a>Datenresidenz

Benutzerdaten von Azure AD B2C werden entweder in den USA, in Europa oder in der Region „Asien-Pazifik“ gespeichert.

Die Datenresidenz wird durch das Land oder die Region bestimmt, das bzw. die Sie beim [Erstellen eines Azure AD B2C-Mandanten](tutorial-create-tenant.md) auswählen:

![Screenshot des Formulars „Mandanten erstellen“, Auswahl von Land oder Region.](./media/data-residency/data-residency-b2c-tenant.png)

Für die folgenden Länder/Regionen werden die Daten in den **USA** gespeichert:

> USA (US), Kanada (CA), Costa Rica (CR), Dominikanische Republik (DO), El Salvador (SV), Guatemala (GT), Mexiko (MX), Panama (PA), Puerto Rico (PR) sowie Trinidad und Tobago (TT)

Für die folgenden Länder/Regionen werden die Daten in **Europa** gespeichert:

> Algerien (DZ), Österreich (AT), Aserbaidschan (AZ), Bahrain (BH), Belarus (BY), Belgien (BE), Bulgarien (BG), Kroatien (HR), Zypern (CY), Tschechische Republik (CZ), Dänemark (DK), Ägypten (EG), Estland (EE), Finnland (FT), Frankreich (FR), Deutschland (DE), Griechenland (GR), Ungarn (HU), Island (IS), Irland (IE), Israel (IL), Italien (IT), Jordanien (JO), Kasachstan (KZ), Kenia (KE), Kuwait (KW), Lettland (LV), Libanon (LB), Liechtenstein (LI), Litauen (LT), Luxemburg (LU), Nordmazedonien (ML), Malta (MT), Montenegro (ME), Marokko (MA), Niederlande (NL), Nigeria (NG), Norwegen (NO), Oman (OM), Pakistan (PK), Polen (PL), Portugal (PT), Katar (QA), Rumänien (RO), Russland (RU), Saudi-Arabien (SA), Serbien (RS), Slowakei (SK), Slowenien (ST), Südafrika (ZA), Spanien (ES), Schweden (SE), Schweiz (CH), Tunesien (TN), Türkei (TR), Ukraine (UA), Vereinigte Arabische Emirate (AE) und Vereinigtes Königreich (GB)

Für die folgenden Länder/Regionen werden die Daten in der Region **Asien-Pazifik** gespeichert:

> Afghanistan (AF), Hongkong (SAR) (HK), Indien (IN), Indonesien (ID), Japan (JP), Südkorea (KR), Malaysia (MY), Philippinen (PH), Singapur (SG), Sri Lanka (LK), Taiwan (TW) und Thailand (TH)

Derzeit wird die Liste um die folgenden Länder/Regionen ergänzt. Sie können vorerst Azure AD B2C verwenden, indem Sie eines bzw. eine der oben genannten Länder/Regionen auswählen.

> Argentinien, Australien, Brasilien, Chile, Ecuador, Irak, Kolumbien, Neuseeland, Paraguay, Peru, Uruguay und Venezuela.

## <a name="preview-tenant"></a>Vorschaumandant

Wenn Sie während des Vorschauzeitraums von Azure AD B2C einen B2C-Mandanten erstellt haben, ist die Wahrscheinlichkeit hoch, dass der **Mandantentyp** auf **Vorschaumandant** festgelegt ist.

In diesem Fall können Sie Ihren Mandanten NUR für Entwicklungs- und Testzwecke verwenden. Verwenden Sie einen Vorschaumandanten NICHT für Produktionsanwendungen.

Es gibt **keinen Migrationspfad** von einem B2C-Vorschaumandanten zu einem B2C-Produktionsmandanten. Sie müssen einen neuen B2C-Mandanten für Ihre Produktionsanwendungen erstellen.

Beim Löschen eines B2C-Vorschaumandanten und dem Erstellen eines B2C-Produktionsmandanten mit demselben Domänennamen können bekannte Probleme auftreten. *Sie müssen einen B2C-Produktionsmandanten mit einem anderen Domänennamen erstellen*.

![Screenshot eines Mandantentyps als Vorschaumandant.](./media/data-residency/preview-b2c-tenant.png)