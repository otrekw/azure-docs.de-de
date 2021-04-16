---
title: 'Azure Certified Device-Programm – Tutorial: Hinzufügen von Gerätedetails'
description: Eine Schritt-für-Schritt-Anleitung zum Hinzufügen von Gerätedetails zu Ihrem Projekt im Azure Certified Device-Portal
author: nkuntjoro
ms.author: nikuntjo
ms.service: certification
ms.topic: tutorial
ms.date: 03/02/2021
ms.custom: template-tutorial
ms.openlocfilehash: f4f3d045a2530fa54d22bec789918454cba80097
ms.sourcegitcommit: b4fbb7a6a0aa93656e8dd29979786069eca567dc
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/13/2021
ms.locfileid: "107310426"
---
# <a name="tutorial-add-device-details"></a>Tutorial: Hinzufügen von Gerätedetails

Sie haben jetzt das Projekt für Ihr Gerät erstellt und können mit dem Zertifizierungsprozess beginnen! Zunächst fügen Sie Ihre Gerätedetails hinzu. Dazu gehören technische Spezifikationen, die Ihre Kunden im Azure Certified Device-Katalog anzeigen können, und die Marketingdetails, die sie für den Kauf verwenden, sobald sie eine Entscheidung getroffen haben.

In diesem Tutorial lernen Sie, wie die folgenden Aufgaben ausgeführt werden:

> [!div class="checklist"]
> * Hinzufügen von Gerätedetails mithilfe der Funktionen für Komponenten und Abhängigkeiten
> * Hochladen eines Leitfadens für die ersten Schritte mit Ihrem Gerät
> * Angeben von Marketingdetails für Kunden, um Ihr Gerät zu erwerben
> * Identifizieren von Branchenzertifizierungen (optional)

## <a name="prerequisites"></a>Voraussetzungen

* Sie müssen angemeldet sein und über ein Projekt für Ihr Gerät im [Azure Certified Device-Portal](https://certify.azure.com) verfügen. Weitere Informationen finden Sie im [Tutorial](tutorial-01-creating-your-project.md).
* Sie müssen über eine Anleitung für die ersten Schritte mit Ihrem Gerät im PDF-Format verfügen. Wir stellen eine Reihe von Vorlagen für die ersten Schritte bereit, die Sie je nach Zertifizierungsprogramm und ihrer bevorzugten Sprache verwenden können. Die Vorlagen finden Sie an unserem GitHub-Speicherort [Get_Started_Templates](https://aka.ms/GSTemplate "Vorlagen für die ersten Schritte").

## <a name="adding-technical-device-details"></a>Hinzufügen von technischen Gerätedetails

Im ersten Abschnitt der Projektseite mit dem Namen „Gerätedetails eingeben“ können Sie Informationen zu den wichtigsten Hardwarefunktionen Ihres Geräts angeben, z. B. Gerätename, Beschreibung, Prozessor, Betriebssystem, Konnektivitätsoptionen, Hardwareschnittstellen, Branchenprotokolle, Abmessungen usw. Viele der Felder sind zwar optional, aber die meisten dieser Informationen werden potenziellen Kunden im Azure Certified Device-Katalog zur Verfügung gestellt, wenn Sie Ihr Gerät nach der Zertifizierung veröffentlichen möchten.

1. Klicken Sie auf der Projektübersichtsseite im Abschnitt „Gerätedetails eingeben“ auf `Add`, um den Abschnitt mit den Gerätedetails zu öffnen. Ihnen werden fünf Abschnitte angezeigt, die Sie ausfüllen können.

![Abbildung: Seite mit Projektdetails](./media/images/device-details-menu.png)

2. Überprüfen Sie die Informationen, die Sie zuvor beim Erstellen des Projekts auf der Registerkarte `Basics` angegeben haben.
1. Überprüfen Sie die Zertifizierungen, die Sie für Ihr Gerät beantragen, auf der Registerkarte `Certifications`.
1. Öffnen Sie die Registerkarte `Product details`, und wählen Sie mindestens ein Betriebssystem aus.
1. Fügen Sie **mindestens** eine diskrete Komponente hinzu, die Ihr Gerät beschreibt. Weitere Anleitungen zur Verwendung von Komponenten finden Sie [hier](how-to-using-the-components-feature.md).
1. Klicken Sie auf `Save`. Anschließend können Sie das Komponentengerät bearbeiten und erweiterte Details hinzufügen.
1. Listen Sie zusätzliche Gerätedetails, die nicht von den Komponentendetails erfasst werden, unter `Additional product details` auf.
1. Wenn Sie in einem der Komponentenfelder `Other` angekreuzt haben oder das Azure-Zertifizierungsteam über einen besonderen Umstand informieren möchten, geben Sie im Abschnitt `Comments for reviewer` einen erklärenden Kommentar ein.
1. Verwenden Sie die Registerkarte `Dependencies`, um alle Abhängigkeiten aufzulisten, wenn Ihr Gerät zusätzliche Hardware oder Dienste benötigt, um Daten an Azure zu senden. Weitere Anleitungen zum Auflisten von Abhängigkeiten finden Sie [hier](how-to-indirectly-connected-devices.md).
1. Wenn Sie mit den angegebenen Informationen zufrieden sind, finden Sie auf der Registerkarte `Review` einen schreibgeschützten Überblick über alle eingegebenen Geräte.
1. Klicken Sie oben auf der Seite auf `Project summary`, um zur Zusammenfassungsseite zurückzukehren.

![Überprüfen der Seite „Projektdetails“](./media/images/sample-device-details.png)

## <a name="uploading-a-get-started-guide"></a>Hochladen eines Leitfadens für die ersten Schritte

Der Leitfaden für die ersten Schritte ist ein PDF-Dokument zum Vereinfachen der Einrichtung, Konfiguration und Verwaltung Ihres Produkts. Ihr Zweck ist es, Kunden die Verbindung und Unterstützung von Geräten auf Azure mit Ihrem Gerät zu erleichtern. Im Rahmen des Zertifizierungsprozesses müssen unsere Partner **einen** Leitfaden für die ersten Schritte für ihr wichtigstes Zertifizierungsprogramm bereitstellen.

1. Vergewissern Sie sich, dass Sie alle erforderlichen Informationen gemäß den bereitgestellten [Vorlagen](https://aka.ms/GSTemplate) in die PDF-Datei des Leitfadens für die ersten Schritte eingegeben haben. Die Vorlage, die Sie verwenden, sollte sich nach dem Zertifizierungsbadge richten, den Sie beantragen. (Beispiel: Für ein IoT Plug & Play-Gerät wird die Vorlage „IoT Plug & Play“ verwendet. Geräte, für die *nur* die Azure Certified Device-Basiszertifizierung beantragt wird, verwenden die Vorlage „Azure Certified Device“).
1. Klicken Sie auf der Seite „Projektzusammenfassung“ im Abschnitt des Leitfadens für die ersten Schritte auf `Add`.

![Abbildung: Leitfaden für die ersten Schritte](./media/images/gsg-menu.png)

2. Klicken Sie auf „Datei auswählen“, um Ihre PDF-Datei hochzuladen.
1. Überprüfen Sie das Dokument in der Vorschau auf die Formatierung.
1. Speichern Sie den Upload, indem Sie auf die Schaltfläche „Speichern“ klicken.
1. Klicken Sie oben auf der Seite auf `Project summary`, um zur Zusammenfassungsseite zurückzukehren.

## <a name="providing-marketing-details"></a>Angeben von Marketingdetails

In diesem Bereich stellen Sie kundenspezifische Marketinginformationen für Ihr Gerät bereit. Diese Felder werden im Azure Certified Device-Katalog angezeigt, wenn Sie sich für die Veröffentlichung ihres zertifizierten Geräts entscheiden.

1. Klicken Sie im Abschnitt „Marketingdetails hinzufügen“ auf `Add`, um die Seite mit den Marketingdetails zu öffnen.

![Abbildung: Abschnitt „Marketingdetails“](./media/images/marketing-details.png)

1. Laden Sie ein Produktfoto im JPEG-oder PNG-Format hoch, das im Katalog verwendet wird.
1. Verfassen Sie eine Kurzbeschreibung Ihres Geräts, die auf der Produktbeschreibungsseite des Katalogs angezeigt wird.
1. Geben Sie die geografische Verfügbarkeit Ihres Geräts an.
1. Geben Sie einen Link zur Marketingseite des Geräteherstellers an. Dabei sollte es sich um einen Link zu einer Website mit weiteren Informationen über das Gerät handeln.
    > [!Note]
    > Stellen Sie sicher, dass alle angegebenen URLs gültig sind oder zum Zeitpunkt der Veröffentlichung nach der Genehmigung aktiv sind. *)

1. Geben Sie bis zu drei Zielbranchen an, für die Ihr Gerät optimiert ist.
1. Geben Sie Informationen für bis zu fünf Vertriebspartner Ihres Geräts an. Dies kann auch die eigene Website des Herstellers sein.

    > [!Note]
    > Wenn keine URL für die Produktseite eines Vertriebspartners angegeben wird, wird für die Schaltfläche `Shop` im Katalog standardmäßig der für `Distributor page` angegebene Link verwendet, der möglicherweise nicht spezifisch für das Gerät ist. Im Idealfall sollte die Vertriebspartner-URL zu einer bestimmten Seite führen, auf der ein Kunde ein Gerät kaufen kann – dies ist jedoch nicht obligatorisch. Wenn der Vertriebspartner mit dem Hersteller identisch ist, kann diese URL mit der Marketingseite des Herstellers identisch sein. *)

1. Klicken Sie auf `Save`, um Ihre Informationen zu bestätigen.
1. Klicken Sie oben auf der Seite auf `Project summary`, um zur Zusammenfassungsseite zurückzukehren.

## <a name="declaring-additional-industry-certifications"></a>Angeben zusätzlicher Branchenzertifizierungen

Sie können auch zusätzliche Branchenzertifizierungen präsentieren, die Sie bereits für Ihr Gerät erhalten haben. Diese Zertifizierungen tragen dazu bei, den Verwendungszweck Ihres Geräts zu verdeutlichen, und können im Azure Certified Device-Katalog gesucht werden.

1. Klicken Sie im Abschnitt „Industriezertifizierungen angeben“ auf `Add`.
1. Klicken Sie auf `Add a certification`, um aus einer Liste der gängigen Industriezertifizierungsprogramme auszuwählen. Wenn Ihr Produkt eine Zertifizierung erhalten hat, die nicht auf der Liste steht ist, können Sie einen benutzerdefinierten Zeichenfolgewert angeben, indem Sie `Other (please specify)` auswählen.
1. Geben Sie optional eine Beschreibung oder Hinweise für den Prüfer an. Diese Hinweise werden nicht öffentlich im Katalog angezeigt.
1. Klicken Sie auf `Save`, um Ihre Informationen zu bestätigen.
1. Klicken Sie oben auf der Seite auf `Project summary`, um zur Zusammenfassungsseite zurückzukehren.

## <a name="next-steps"></a>Nächste Schritte

Sie haben den Prozess der Beschreibung Ihres Geräts jetzt abgeschlossen! Dies hilft dem Azure Certified Device-Bewertungsteam sowie Ihren Kunden, Ihr Produkt besser zu verstehen. Wenn Sie mit den von Ihnen bereitgestellten Informationen zufrieden sind, können Sie nun mit der Testphase des Zertifizierungsprozesses fortfahren.
> [!div class="nextstepaction"]
> [Tutorial: Testen Ihres Geräts](tutorial-03-testing-your-device.md)
