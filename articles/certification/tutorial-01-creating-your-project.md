---
title: 'Programm „Azure Certified Device“: Tutorial: Erstellen Ihres Projekts'
description: Anleitung für die Erstellung eines Projekts im Azure Certified Device-Portal
author: nkuntjoro
ms.author: nikuntjo
ms.service: certification
ms.topic: tutorial
ms.date: 03/01/2021
ms.custom: template-tutorial
ms.openlocfilehash: 44c053eac2215354fbd43e4c0068f02419010a85
ms.sourcegitcommit: 4a54c268400b4158b78bb1d37235b79409cb5816
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/28/2021
ms.locfileid: "108131853"
---
# <a name="tutorial-create-your-project"></a>Tutorial: Erstellen Ihres Projekts

Wir gratulieren Ihnen zu der Entscheidung, Ihr Gerät im Rahmen des Programms „Azure Certified Device“ zertifizieren zu lassen! Sie haben das richtige Zertifizierungsprogramm für Ihr Gerät ausgewählt und können mit dem Prozess im Portal beginnen.

In diesem Tutorial lernen Sie Folgendes:

> [!div class="checklist"]
> * Anmelden beim [Azure Certified Device-Portal](https://certify.azure.com/)
> * Erstellen eines neuen Zertifizierungsprojekts für Ihr Gerät
> * Angeben von grundlegenden Gerätedetails Ihres Projekts

## <a name="prerequisites"></a>Voraussetzungen

- Sie benötigen ein gültiges [Azure Active Directory-Konto](../active-directory/fundamentals/active-directory-whatis.md) (Geschäfts-, Schul- oder Unikonto).
- Sie benötigen ein verifiziertes MPN-Konto (Microsoft Partner Network). Falls Sie kein MPN-Konto haben, müssen Sie zunächst dem [Partnernetzwerk beitreten](https://partner.microsoft.com/).

## <a name="signing-into-the-azure-certified-device-portal"></a>Anmelden beim Azure Certified Device-Portal

Als Erstes müssen Sie sich beim Portal anmelden. Darin geben Sie Ihre Geräteinformationen an, führen den Zertifizierungstest durch und verwalten die Veröffentlichung Ihrer Geräte im Azure Certified Device-Katalog.

1. Navigieren Sie zum [Azure Certified Device-Portal](https://certify.azure.com).
1. Wählen Sie auf der linken Seite die Option `Company profile` (Unternehmensprofil) aus, und aktualisieren Sie Ihre Herstellerinformationen.
   ![Abschnitt „Unternehmensprofil“](./media/images/company-profile.png)
1. Akzeptieren Sie die Programmvereinbarung, damit Sie mit der Arbeit am Projekt beginnen können.

## <a name="creating-your-project-on-the-portal"></a>Erstellen Ihres Projekts im Portal

Nachdem Sie im Portal alles eingerichtet haben, können Sie mit dem Zertifizierungsprozess beginnen. Zunächst müssen Sie ein Projekt für Ihr Gerät erstellen.

1. Wählen Sie auf der Startseite die Option `Create new project` (Neues Projekt erstellen) aus. Ein Fenster wird geöffnet, in dem Sie grundlegende Geräteinformationen hinzufügen können (siehe nächster Abschnitt).

 ![Abbildung: Schaltfläche „Neues Projekt erstellen“](./media/images/create-new-project.png)

## <a name="identifying-basic-device-information"></a>Angeben grundlegender Geräteinformationen

Als Nächstes geben Sie grundlegende Informationen zum Gerät an. Sie können diese Informationen später noch bearbeiten.

1. Füllen Sie im Abschnitt `Basics` (Grundlagen) die erforderlichen Felder aus. Die folgende Tabelle enthält eine Beschreibung der **erforderlichen** Felder:

    | Felder                  | BESCHREIBUNG                                                                                                                         |
    |------------------------|-------------------------------------------------------------------------------------------------------------------------------------|
    | Projektname           | Interner Name, der im Azure Certified Device-Katalog nicht angezeigt wird.                                                        |
    | Gerätename            | Öffentlicher Name Ihres Geräts                                                                                                |
    | Gerätetyp            | Spezifikation für fertiges Produkt oder implementierungsfertiges Entwicklungskit.     Weitere Informationen zur Terminologie finden Sie im [Glossar zur Zertifizierung](./resources-glossary.md).                                                                     |
    | Geräteklasse           | „Gateway“, „Sensor“ oder „Sonstiges“.  Weitere Informationen zur Terminologie finden Sie im [Glossar zur Zertifizierung](./resources-glossary.md).                                                                    |
    | URL des Gerätequellcodes | Erforderlich, wenn Sie die Zertifizierung für ein implementierungsfertiges Entwicklungskit durchführen (ansonsten optional). Die URL muss auf einen GitHub-Speicherort für Ihren Gerätecode verweisen. |
1. Wählen Sie die Schaltfläche `Next` (Weiter) aus, um mit der Registerkarte `Certifications` (Zertifizierungen) fortzufahren.

    ![Abbildung: Formular „Neues Projekt erstellen“, Registerkarte „Zertifizierungen“](./media/images/create-new-project-certificationswindow.png)

1. Geben Sie an, welche Zertifizierung(en) Sie für Ihr Gerät durchführen möchten.
1. Wählen Sie die Option `Create` (Erstellen) aus. Das neue Projekt wird gespeichert und im Portal auf der Startseite angezeigt.

    ![Abbildung: Projekttabelle](./media/images/project-table.png)

1. Wählen Sie den Projektnamen in der Tabelle aus. Die Seite mit der Projektzusammenfassung wird gestartet, auf der Sie Ihrem Gerät weitere Details hinzufügen und diese anzeigen können.

    ![Abbildung: Seite mit Projektdetails](./media/images/device-details-section.png)

## <a name="next-steps"></a>Nächste Schritte

Sie wissen nun, wie Sie Gerätedetails hinzufügen und Ihr Gerät mit unserem Zertifizierungsdienst testen können. Fahren Sie mit dem nächsten Artikel fort, um sich über das Bearbeiten Ihrer Gerätedetails zu informieren.
> [!div class="nextstepaction"]
> [Tutorial: Hinzufügen von Gerätedetails](tutorial-02-adding-device-details.md)