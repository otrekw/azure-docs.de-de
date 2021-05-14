---
title: 'Verwaltung von Drittanbieter-App-Lizenzen über Microsoft: Azure'
description: Hier erfahren Sie mehr über das Verwalten von Drittanbieter-App-Lizenzen über Microsoft.
ms.service: marketplace
ms.subservice: partnercenter-marketplace-publisher
ms.topic: conceptual
author: mingshen-ms
ms.author: mingshen
ms.reviewer: dannyevers
ms.date: 04/30/2021
ms.openlocfilehash: de39afe0b88c78b75948fc7c702f0ae06916e2ff
ms.sourcegitcommit: dd425ae91675b7db264288f899cff6add31e9f69
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/01/2021
ms.locfileid: "108331500"
---
# <a name="third-party-app-license-management-through-microsoft"></a>Verwaltung von Drittanbieter-App-Lizenzen über Microsoft

> [!IMPORTANT]
> Diese Funktion ist derzeit als Public Preview verfügbar.

Gilt für den folgenden Angebotstyp:

- Dynamics 365 for Customer Engagement und Power Apps

Die _Verwaltung von Drittanbieter-App-Lizenzen über Microsoft_ ermöglicht es unabhängigen Softwareherstellern (Independent Software Vendors, ISVs), die Lösungen mithilfe der Dynamics 365-Produktsuite erstellen, Lizenzen für ihre Lösungen mithilfe von Systemen zu verwalten und durchzusetzen, die von Microsoft bereitgestellt werden. Dieser Ansatz macht Folgendes möglich:

- Ermöglichen Sie es Ihren Kunden, die Lizenzen Ihrer Lösung mithilfe vertrauter Tools zuzuweisen bzw. deren Zuweisung aufzuheben. Ein Beispiel für ein solches Tool wäre etwa Microsoft 365 Admin Center, das sie für die Verwaltung von Office- und Dynamics-verwenden.
- Lassen Sie Ihre Lizenzen zur Laufzeit von Power Platform erzwingen, um sicherzustellen, dass nur lizenzierte Benutzer auf Ihre Lösung zugreifen können
- Sparen Sie sich den Aufwand für das Erstellen und Pflegen eines eigenen Systems für die Lizenzverwaltung und -erzwingung.


> [!NOTE]
> Die Verwaltung von Drittanbieter-App-Lizenzen über Microsoft ist nur für ISVs verfügbar, die an ISV Connect Program teilnehmen. Microsoft ist nicht am Verkauf von Lizenzen beteiligt.

## <a name="prerequisites"></a>Voraussetzungen

Sie müssen die folgenden Voraussetzungen erfüllen, um Ihre App-Lizenzen über Microsoft zu verwalten:

1. Sie verfügen über ein gültiges [Microsoft Partner Network-Konto](/partner-center/mpn-create-a-partner-center-account).
1. Sie müssen für das Programm „Kommerzieller Marketplace“ registriert sein. Weitere Informationen finden Sie unter [Erstellen eines Kontos im kommerziellen Marketplace in Partner Center](create-account.md).
1. Sie müssen für [ISV Connect Program](https://partner.microsoft.com/solutions/business-applications/isv-overview) registriert sein. Weitere Informationen finden Sie im [Onboardingleitfaden für Microsoft Business Applications ISV Connect Program](business-applications-isv-program.md).
1. Ihr Entwicklerteam verfügt über die Entwicklungsumgebungen und Tools, die zum Erstellen von Dataverse-Lösungen erforderlich sind. Ihre Dataverse-Lösung muss modellgesteuerte Anwendungen enthalten. (Dies sind derzeit die einzigen Typen von Lösungskomponenten, die über die Lizenzverwaltungsfunktion unterstützt werden.)

## <a name="high-level-process"></a>Allgemeiner Prozess

In dieser Tabelle wird der allgemeine Prozess zum Verwalten von Drittanbieter-Apps über Microsoft veranschaulicht:

| Schritt | Details |
| ------------ | ------------- |
| Schritt 1: Erstellen eines Angebots | Der ISV erstellt ein Angebot in Partner Center und entscheidet sich für dieses Angebot für die Verwaltung von Lizenzen über Microsoft. Dazu gehört auch das Definieren von mindestens einem Lizenzplan für das Angebot. |
| Schritt 2: Aktualisieren des Pakets | Der ISV erstellt ein Lösungspaket für das Angebot, das Lizenzplaninformationen als Metadaten enthält, und lädt es in Partner Center hoch, um es in Microsoft AppSource zu veröffentlichen. Weitere Informationen finden Sie unter [Hinzufügen von Lizenzmetadaten zu Ihrer Lösung](https://go.microsoft.com/fwlink/?linkid=2162161&clcid=0x409). |
| Schritt 3: Erwerben von Lizenzen | Kunden finden das Angebot des ISV in AppSource oder direkt auf der Website des ISV. Kunden erwerben Lizenzen für die gewünschten Pläne direkt über den ISV. (Diese Angebote können derzeit nicht über AppSource erworben werden.) |
| Schritt 4: Registrieren des Geschäftsabschlusses | Der ISV registriert den Kauf bei Microsoft in Partner Center. Im Rahmen der [Registrierung des Geschäftsabschlusses](/partner-center/csp-commercial-marketplace-licensing) gibt der ISV den Typ und die Menge der einzelnen Lizenzierungspläne an, die vom Kunden erworben wurden. |
| Schritt 5: Verwalten von Lizenzen | Die Lizenzpläne werden im Microsoft 365 Admin Center angezeigt, damit der Kunde sie Benutzern oder Gruppen in seiner Organisation zuweisen kann. Der Kunde kann die Anwendung auch über das Power Platform Admin Center in seinem Mandanten installieren. |
| Schritt 6: Durchführen der Lizenzüberprüfung | Wenn ein Benutzer innerhalb der Organisation des Kunden versucht, eine Anwendung auszuführen, überprüft Microsoft vor dem Zulassen der Ausführung, ob der Benutzer über eine Lizenz verfügt. Verfügt er nicht über eine Lizenz, wird dem Benutzer eine Meldung mit dem Hinweis angezeigt, dass er für eine Lizenz einen Administrator kontaktieren muss. |
| Schritt 7: Anzeigen von Berichten | ISVs können Informationen zu bereitgestellten und zugewiesenen Lizenzen über einen bestimmten Zeitraum und nach Geografie anzeigen. |
|||

## <a name="enabling-app-license-management-through-microsoft"></a>Aktivieren der App-Lizenzverwaltung über Microsoft

Beim Erstellen eines Angebots gibt es zwei Kontrollkästchen auf der Registerkarte „Angebotseinrichtung“, mit denen die App-Lizenzverwaltung für ein Angebot aktiviert wird.

### <a name="enable-app-license-management-through-microsoft-check-box"></a>Kontrollkästchen „Enable app license management through Microsoft“ (App-Lizenzverwaltung über Microsoft aktivieren)

Funktionsweise:

- Nachdem Sie das Kontrollkästchen **Enable app license management through Microsoft** (App-Lizenzverwaltung über Microsoft aktivieren) aktiviert haben, können Sie Lizenzierungspläne für Ihr Angebot definieren.
- Kunden wird auf der Seite „Angebotsliste“ in AppSource die Schaltfläche **Jetzt herunterladen** angezeigt. Kunden können diese Schaltfläche auswählen, um sich mit Ihnen in Verbindung zu setzen und Lizenzen für die App zu erwerben.

### <a name="allow-customers-to-install-my-app-even-if-licenses-are-not-assigned-check-box"></a>Kontrollkästchen „Allow customers to install my app even if licenses are not assigned“ (Kunden die Installation meiner App erlauben, auch wenn keine Lizenzen zugewiesen sind)

Nachdem Sie das erste Kontrollkästchen aktiviert haben, wird das Kontrollkästchen **Allow customers to install my app even if licenses are not assigned** (Kunden die Installation meiner App erlauben, auch wenn keine Lizenzen zugewiesen sind) angezeigt. Diese Option ist nützlich, wenn Sie eine Freemium-Lizenzierungsstrategie verwenden, bei der Sie einige grundlegende Features Ihrer Lösung für alle Benutzer kostenlos anbieten und Premium-Features in Rechnung stellen möchten. Wenn Sie umgekehrt sicherstellen möchten, dass nur Mandanten, die derzeit Lizenzen für Ihr Produkt besitzen, es aus AppSource herunterladen können, wählen Sie diese Option nicht aus.

> [!NOTE]
> Wenn Sie diese Option auswählen, müssen Sie Ihr Lösungspaket so konfigurieren, dass keine Lizenz erforderlich ist.

Funktionsweise:

- Allen AppSource-Benutzern wird auf der Seite „Angebotsliste“ die Schaltfläche **Jetzt herunterladen** zusammen mit der Schaltfläche **Kontakt mit mir aufnehmen** angezeigt, und sie können Ihr Angebot herunterladen und installieren.
- Wenn Sie diese Option nicht auswählen, überprüft AppSource, ob der Mandant des Benutzers über mindestens eine Lizenz für Ihre Lösung verfügt, bevor die Schaltfläche **Jetzt herunterladen** angezeigt wird. Wenn im Mandanten des Benutzers keine Lizenz vorhanden ist, wird nur die Schaltfläche **Kontakt mit mir aufnehmen** angezeigt.

Weitere Informationen zum Konfigurieren eines Angebots finden Sie unter [Erstellen eines Angebots für Dynamics 365 for Customer Engagement und Power Apps](dynamics-365-customer-engage-offer-setup.md).

## <a name="offer-listing-page-on-appsource"></a>Seite „Angebotsliste“ in AppSource

Nach der Veröffentlichung Ihres Angebots bestimmen die von Ihnen ausgewählten Optionen, welche Schaltflächen den Benutzern angezeigt werden. Der folgende Screenshot zeigt die Seite „Angebotsliste“ in AppSource mit den Schaltflächen **Jetzt herunterladen** und **Kontakt mit mir aufnehmen**:

:::image type="content" source="./media/third-party-license/f365.png" alt-text="Screenshot: Seite „Angebotsliste“ in AppSource. Die Schaltflächen „Jetzt herunterladen“ und „Kontakt mit mir aufnehmen“ werden angezeigt.":::

***Abbildung 1: Seite „Angebotsliste“ in Microsoft AppSource***

## <a name="next-steps"></a>Nächste Schritte

- [Planen eines Dynamics 365-Angebots](marketplace-dynamics-365.md)
- [Erstellen eines Angebots für Dynamics 365 for Customer Engagement und Power Apps](dynamics-365-customer-engage-offer-setup.md)
