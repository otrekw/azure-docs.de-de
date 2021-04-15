---
title: Azure Certified Device-Programm – Glossar
description: Eine Liste der allgemeinen Begriffe, die im Azure Certified Device-Programm verwendet werden.
author: nikuntjo
ms.author: nikuntjo
ms.service: certification
ms.topic: conceptual
ms.date: 03/03/2021
ms.custom: template-concept
ms.openlocfilehash: 7dfdd4a89535621e08db8a8924282febdafd1569
ms.sourcegitcommit: f5448fe5b24c67e24aea769e1ab438a465dfe037
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/30/2021
ms.locfileid: "105975742"
---
# <a name="azure-certified-device-program-glossary"></a>Azure Certified Device-Programm – Glossar

Dieser Leitfaden enthält Definitionen zu Begriffen, die im Azure Certified Device-Programm und -Portal häufig verwendet werden. In diesem Glossar finden Sie Erläuterungen zum Zertifizierungsprozess. Zu Ihrer Bequemlichkeit ist dieses Glossar nach den wichtigsten Zertifizierungskonzepten geordnet, zu denen Sie möglicherweise Fragen haben.

## <a name="device-class"></a>Geräteklasse

Beim Erstellen Ihres Zertifizierungsprojekts werden Sie aufgefordert, eine Geräteklasse anzugeben. „Geräteklasse“ bezieht sich auf den Formfaktor oder die Klassifizierung, der Ihr Gerät am besten repräsentiert.

- **Gateway**

    Ein Gerät, das Daten verarbeitet, die über ein IoT-Netzwerk gesendet werden.

- **Sensor**

    Ein Gerät, das Änderungen in einer Umgebung erkennt, darauf reagiert und eine Verbindung mit Gateways herstellt, um die Änderungen zu verarbeiten.

- **Andere**

    Wenn Sie „Andere“ auswählen, fügen sie eine Beschreibung Ihrer Geräteklasse in Ihren eigenen Worten hinzu. Im Laufe der Zeit werden wir dieser Liste möglicherweise weitere Werte hinzufügen, insbesondere aufgrund unserer Beobachtung des Feedbacks unserer Partner.

## <a name="device-type"></a>Gerätetyp

Sie werden darüber hinaus gebeten, im Rahmen des Zertifizierungsprozesses einen von zwei Gerätetypen auszuwählen.

- **Fertiggestelltes Produkt**

    Ein Gerät das für die Implementierung und die Bereitstellung in einer Produktionsumgebung bereit ist. In der Regel in einem abgeschlossenen Formfaktor mit Firmware und Betriebssystem. Hier kann es sich um Geräte für allgemeine Zwecke handeln, die weitere Anpassung erfordern, oder um spezialisierte Geräte, für deren Nutzung keine Änderungen erforderlich sind.
- **Implementierungsfertiges Entwicklungskit**

    Ein Entwicklungskit, das Hardware und Software enthält und sich ideal für einfache Prototypen eignet, in der Regel nicht in einem abgeschlossenen Formfaktor. Enthält normalerweise Beispielcode und Tutorials, um Quick Prototyping zu ermöglichen.

## <a name="component-type"></a>Komponententyp

Im Detailabschnitt zu einem Gerät beschreiben Sie Ihr Gerät, indem Sie Komponenten nach Komponententyp geordnet auflisten. Weitere Anleitungen zu Komponenten finden Sie [hier](./how-to-using-the-components-feature.md).

- **Sofort einsatzfähiges Produkt**

    Eine Komponentendarstellung des Gesamt- oder Hauptgeräts. Dies unterscheidet sich von **Fertiggestelltes Produkt**, bei dem es sich um eine Klassifizierung des Geräts als ohne weitere Entwicklung einsatzfähig für den Kunden handelt. Ein Fertiggestelltes Produkt enthält eine Komponente „Sofort einsatzfähiges Produkt“.
- **Entwicklungsboard**

    Ein integriertes oder abnehmbares Board mit Mikroprozessor für eine einfache Anpassung.
- **Periperiegerät**

    Entweder eine integrierte oder eine abnehmbare Ergänzung des Produkts (wie etwa ein Zubehör). Dabei handelt es sich in der Regel um Geräte, die eine Verbindung mit dem Hauptgerät herstellen, aber nicht zu den primären Gerätefunktionen beitragen. Stattdessen stellen sie ergänzende Funktionen bereit. Arbeitsspeicher, RAM, Massenspeicher, Festplatten und CPUs werden nicht als Peripheriegeräte angesehen (sie sollten stattdessen unter „Zusätzliche Spezifikationen“ in der Komponente „Sofort einsatzfähiges Produkt“ aufgeführt werden).
- **System On Module**  

    Ein Schaltkreis auf Boardebene, der eine Systemfunktion in einem einzelnen Modul integriert.

## <a name="component-attachment-method"></a>Komponentenanschlussmethode

Die Komponentenanschlussmethode ist ein weiteres Komponentendetail, das dem Kunden mitteilt, wie die Komponente in das Gesamtprodukt integriert ist.

- **Integriert**
 
    Bezieht sich auf den Fall, dass eine Gerätekomponente Teil des Hauptchassis des Produkts ist. Dies bezieht sich meist auf einen Typ von Peripheriekomponente, der nicht aus dem Gerät entfernt werden kann.  
    Beispiel: ein integrierter Temperatursensor in einem Gatewaychassis.

- **Discrete**

    Bezieht sich auf den Fall, dass eine Komponente **nicht** Teil des Hauptchassis des Produkts ist.  
    Beispiel: ein externer Temperatursensor, der an das Gerät angeschlossen werden muss.


## <a name="next-steps"></a>Nächste Schritte

Dieses Glossar führt Sie durch den Vorgang der Zertifizierung Ihres Projekts auf dem Portal. Sie sind jetzt bereit, mit Ihrem Projekt zu beginnen!
- [Tutorial: Erstellen Ihres Projekts](./tutorial-01-creating-your-project.md)
