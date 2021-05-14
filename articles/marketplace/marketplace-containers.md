---
title: Leitfaden für die Veröffentlichung von Containerangeboten im Azure Marketplace
description: In diesem Artikel werden die Anforderungen für die Veröffentlichung von Containerangeboten im Azure Marketplace beschrieben.
services: Azure, Marketplace, Compute, Storage, Networking, Blockchain, Security
ms.service: marketplace
ms.subservice: partnercenter-marketplace-publisher
ms.topic: conceptual
author: keferna
ms.author: keferna
ms.date: 03/30/2021
ms.openlocfilehash: 43c569204f879e865eb5bd4f040c96e907e63abb
ms.sourcegitcommit: 4a54c268400b4158b78bb1d37235b79409cb5816
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/28/2021
ms.locfileid: "108144707"
---
# <a name="plan-an-azure-container-offer"></a>Planen eines Azure-Containerangebots

Azure-Containerangebote erleichtern Ihnen die Veröffentlichung Ihres Containerimages im Azure Marketplace. Informieren Sie sich in diesem Handbuch über die Anforderungen für diesen Angebotstyp.

Azure-Containerangebote sind Transaktionsangebote, die über den Azure Marketplace bereitgestellt und abgerechnet werden. Den Benutzern wird die Listenoption **Jetzt herunterladen** angezeigt.

Verwenden Sie den Angebotstyp Azure Container, wenn Ihre Lösung ein Docker-Container-Image ist, das als Kubernetes-basierte Azure Container-Instanz eingerichtet wird.

> [!NOTE]
> Eine Azure-Containerinstanz ist eine Laufzeit-Docker-Instanz, mit der sich Container in Azure besonders schnell und einfach ausführen lassen, ohne dass Sie dazu VMs verwalten oder einen übergeordneten Dienst einführen müssen. Container-Instanzen können direkt in Azure bereitgestellt oder von Azure Kubernetes Services oder Azure Kubernetes Service Engine orchestriert werden.  

## <a name="licensing-options"></a>Lizenzierungsoptionen

Für Azure-Containerangebote sind die folgenden Lizenzierungsoptionen verfügbar:

| Lizenzierungsoption | Transaktionsprozess |
| --- | --- |
| Kostenlos | Listen Sie Ihr Angebot für Kunden als kostenlos auf. |
| BYOL | Die Option „Bring Your Own License“ ermöglicht es Ihren Kunden, bereits vorhandene Softwarelizenzen in Azure zu übernehmen.\* |
|

\* Als Herausgeber sind Sie für alle Aspekte der Softwarelizenztransaktion verantwortlich, einschließlich (unter anderem) Auftrag, Auftragserfüllung, Messung, Abrechnung, Rechnungsstellung, Zahlung und Eintreibung.

## <a name="customer-leads"></a>Kundenleads

Wenn Sie ein Angebot mithilfe von Partner Center im kommerziellen Marketplace veröffentlichen, sollten Sie es mit Ihrem CRM-System (Customer Relationship Management) verbinden. Dadurch empfangen Sie Kontaktinformationen zum Kunden, sobald jemand sein Interesse an Ihrem Produkt bekundet oder es verwendet. Das Herstellen einer Verbindung mit einem CRM-System ist erforderlich, wenn Sie eine Testversion aktivieren möchten, andernfalls ist dieser Schritt optional. Partner Center unterstützt Azure Table Storage, Dynamics 365 Customer Engagement, HTTPS-Endpunkte, Marketo und Salesforce.

## <a name="legal-contracts"></a>Verträge

Zur Vereinfachung des Beschaffungsprozesses für Kunden und zur Reduzierung der rechtlichen Komplexität für Softwareanbieter stellt Microsoft eine Vorlage für Standardverträge bereit, die Sie für Ihre Angebote im kommerziellen Marketplace verwenden können. Wenn Sie Ihre Software unter dem Standardvertrag anbieten, müssen Kunden ihn nur einmal lesen und akzeptieren, und Sie müssen keine benutzerdefinierten Geschäftsbedingungen erstellen.

Sie können anstelle des Standardvertrags Ihre eigenen Geschäftsbedingungen angeben. Kunden müssen diesen Bestimmungen zustimmen, bevor sie Ihr Angebot testen können.

## <a name="offer-listing-details"></a>Details zur Angebotsauflistung

> [!NOTE]
> Der Inhalt der Angebotsauflistung muss nicht in englischer Sprache verfasst werden, sofern die Angebotsbeschreibung mit dem Satz „Diese Anwendung ist nur auf [nicht englische Sprache] erhältlich“ beginnt.

Um das Angebot einfacher zu gestalten, können Sie diese Elemente vorab vorbereiten. Sofern nicht anders angegeben sind alle erforderlich.

- **Name:** Dieser Name wird als Titel Ihres Angebots im kommerziellen Marketplace angezeigt. Der Name ist möglicherweise markenrechtlich geschützt. Er darf keine Emojis enthalten (außer diese sind Teil der Markenzeichen oder Copyrightsymbole) und maximal 50 Zeichen lang sein.
- **Zusammenfassung der Suchergebnisse:** Geben Sie den Zweck oder die Funktion des Angebots in einem Satz ohne Zeilenumbrüche und mit höchstens 100 Zeichen an. Diese Zusammenfassung wird in den Suchergebnissen verwendet, die aus Auflistungen im kommerziellen Marketplace erfasst werden.
- **Kurzbeschreibung:** Geben Sie den Zweck oder die Funktion des Angebots als Text ohne Formatierungen oder Zeilenumbrüche an. Diese Informationen werden auf der Detailseite Ihres Angebots angezeigt.
- **Beschreibung:** Diese Beschreibung wird in der Übersicht der Auflistungen im kommerziellen Marketplace angezeigt. Sie können z. B ein Wertversprechen, wichtige Vorteile, Zielgruppe, Kategorie- oder Branchenzuordnungen, Möglichkeiten für In-App-Käufe, erforderliche Veröffentlichungen und einen Link zu weiteren Informationen eingeben. Dieses Textfeld enthält Rich-Text-Editor-Steuerelemente, mit denen Sie die Beschreibung ansprechender gestalten können. Verwenden Sie optional HTML-Tags für die Formatierung.
- **Link zur Datenschutzrichtlinie:** URL zur Datenschutzrichtlinie Ihres Unternehmens. Es liegt in Ihrer Verantwortung sicherzustellen, dass Ihre App den Gesetzen und Regelungen zum Datenschutz entspricht.
- **Nützliche Links** (optional): Links für Benutzer Ihres Angebots zu verschiedenen Ressourcen. Beispiele: Foren, FAQ und Versionshinweise.
- **Kontaktinformationen**
  - **Supportkontakt:** Geben Sie den Namen, die Telefonnummer und die E-Mail-Adresse für Microsoft-Partner an. Diese werden dann von Kunden bei der Eröffnung von Supporttickets verwendet. Schließen Sie auch die URL für Ihre Supportwebsite ein.
  - **Technischer Ansprechpartner:** Geben Sie den Namen, die Telefonnummer und die E-Mail-Adresse an, die Microsoft direkt verwenden kann, wenn es Probleme mit Ihrem Angebot gibt. Diese Kontaktinformationen sind nicht im kommerziellen Marketplace aufgeführt.
  - **Ansprechpartner für CSP-Programm** (optional): Geben Sie den Namen, die Telefonnummer und die E-Mail-Adresse an, wenn Sie sich für das CSP-Programm entscheiden, damit Sie bei Fragen von Partnern kontaktiert werden können. Sie können auch eine URL zu Ihren Marketingmaterialien hinzufügen.
- **Medien**
    - **Logos:** Stellen Sie eine PNG-Datei für das **große** Logo bereit. Dieses wird in Partner Center verwendet, um die anderen erforderlichen Logogrößen zu erstellen. Sie können diese Logos später durch andere Bilder ersetzen.
    - **Screenshots:** Fügen Sie mindestens einen und bis zu fünf Screenshots hinzu, die zeigen, wie Ihr Angebot funktioniert. Bilder müssen 1280 × 720 Pixel im PNG-Format aufweisen und eine Beschriftung enthalten.
    - **Videos** (optional): Fügen Sie bis zu vier Videos hinzu, die Ihr Angebot vorstellen. Geben Sie einen Namen, eine URL für YouTube oder Vimeo sowie eine PNG-Miniaturansicht mit 1280 × 720 Pixel an.

> [!Note]
> Ihr Angebot muss für die Veröffentlichung im kommerziellen Marketplace die allgemeinen [Zertifizierungsrichtlinien für den kommerziellen Marketplace](/legal/marketplace/certification-policies#100-general) erfüllen.

## <a name="preview-audience"></a>Vorschauzielgruppe

Eine Vorschauzielgruppe erhält vor der Liveveröffentlichung in den Onlineshops Zugriff auf Ihr Angebot, um die End-to-End-Funktionalität vorab zu testen. Auf der Seite **Vorschauzielgruppe** können Sie eine eingeschränkte Vorschauzielgruppe definieren.

Sie können Einladungen an Azure-Abonnement-IDs senden. Fügen Sie bis zu 10 IDs manuell hinzu, oder importieren Sie über eine CSV-Datei bis zu 100 IDs. Wenn Ihr Angebot bereits live geschaltet ist, können Sie dennoch eine Vorschauzielgruppe für das Testen von Änderungen oder für Aktualisierungen Ihres Angebots definieren.

## <a name="plans-and-pricing"></a>Pläne und Preise

Für Containerangebote ist mindestens ein Plan erforderlich. Ein Plan definiert den Umfang und die Einschränkungen einer Lösung. Sie können mehrere Pläne für Ihr Angebot erstellen, um Ihren Kunden verschiedene technische und Lizenzoptionen zu bieten. 

Für Container werden zwei Lizenzierungsmodelle unterstützt: Free oder Bring Your Own License (BYOL). BYOL bedeutet, dass Sie mit Ihren Kunden direkt abrechnen und Microsoft Ihnen keine Gebühren berechnet. Microsoft wickelt nur die Nutzungsgebühren für Azure-Infrastruktur ab. Weitere Informationen finden Sie unter [Transaktionsfunktionen im kommerziellen Marketplace](marketplace-commercial-transaction-capabilities-and-considerations.md).

## <a name="additional-sales-opportunities"></a>Zusätzliche Verkaufschancen

Sie haben die Möglichkeit, sich für von Microsoft unterstützte Marketing- und Vertriebskanäle zu entscheiden. Wenn Sie Ihr Angebot im Partner Center erstellen, werden gegen Ende des Prozesses zwei Registerkarten angezeigt:

- **Verkauf über CSPs:** Erlauben Sie Microsoft CSP-Partnern (Cloud Solution Provider), Ihre Lösung als Teil eines Bundleangebots weiterzuverkaufen. Weitere Informationen zu diesem Programm finden Sie unter [Cloud Solution Provider-Programm](cloud-solution-providers.md).
- **Co-Selling mit Microsoft:** Microsoft-Verkaufsteams können Ihre für IP-Co-Selling in Frage kommende Lösung in Betracht ziehen, wenn sie Kundenanforderungen evaluieren. Einzelheiten zur Berechtigung für das Co-Selling finden Sie unter [Requirements for Co-sell Status](/legal/marketplace/certification-policies) (Anforderungen für den Co-Selling-Status). Ausführliche Informationen dazu, wie Sie Ihr Angebot auf die Auswertung vorbereiten, finden Sie unter [Co-Selling-Option in Partner Center](./co-sell-configure.md).

## <a name="container-offer-requirements"></a>Anforderungen für Containerangebote

| Anforderung | Details |  
|:--- |:--- |  
| Abrechnung und Messung | Es wird entweder das kostenlose oder BYOL-Abrechnungsmodell unterstützt. |
| Auf Grundlage einer Dockerfile erstelltes Image | Containerimages müssen auf der Docker-Imagespezifikation basieren und auf der Grundlage einer Dockerfile-Datei erstellt werden. Weitere Informationen zum Entwickeln von Docker-Images finden Sie im Abschnitt zur Verwendung in der [Dockerfile-Referenz](https://docs.docker.com/engine/reference/builder/#usage). |
| Hosten in einem Azure Container Registry-Repository | Containerimages müssen in einem Azure Container Registry-Repository gehostet werden. Weitere Informationen zum Arbeiten mit Azure Container Registry finden Sie unter [Schnellstart: Erstellen einer privaten Containerregistrierung über das Azure-Portal](../container-registry/container-registry-get-started-portal.md).<br><br> |
| Imagemarkierung | Containerimages müssen mindestens ein Tag enthalten (maximale Taganzahl: 16). Weitere Informationen zum Markieren eines Images finden Sie auf der Seite `docker tag` auf der Website zur [Docker-Dokumentation](https://docs.docker.com/engine/reference/commandline/tag). |

## <a name="next-steps"></a>Nächste Schritte

- [Vorbereiten technischer Ressourcen](azure-container-technical-assets.md)