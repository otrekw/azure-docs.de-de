---
title: 'Programm „Azure Certified Device“: Tutorial: Testen Ihres Geräts'
description: Eine Schritt-für-Schritt-Anleitung zum Testen Ihres Geräts mit dem AICS-Dienst im Azure Certified Device-Portal
author: nkuntjoro
ms.author: nikuntjo
ms.service: certification
ms.topic: tutorial
ms.date: 03/02/2021
ms.custom: template-tutorial
ms.openlocfilehash: ef82d44ef44189c0430ba9789baf3279fbe49a9c
ms.sourcegitcommit: b4fbb7a6a0aa93656e8dd29979786069eca567dc
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/13/2021
ms.locfileid: "107310953"
---
# <a name="tutorial-test-and-submit-your-device"></a>Tutorial: Testen und Übermitteln Ihres Geräts

Die nächste Hauptphase des Zertifizierungsprozesses (die auch vor dem Hinzufügen der Gerätedetails abgeschlossen werden kann) umfasst das Testen Ihres Geräts. Sie nutzen im Portal den Azure IoT Certification Service (AICS), um die Leistung Ihres Geräts gemäß unseren Zertifizierungsanforderungen nachzuweisen. Nachdem Sie die Testphase erfolgreich abgeschlossen haben, übermitteln Sie Ihr Gerät für die abschließende Bewertung und Genehmigung durch das Azure-Zertifizierungsteam!

In diesem Tutorial lernen Sie, wie die folgenden Aufgaben ausgeführt werden:

> [!div class="checklist"]
> * Verbinden Ihres Geräts mit IoT Hub über Device Provisioning Service (DPS)
> * Testen Ihres Geräts gemäß den ausgewählten Zertifizierungsprogrammen
> * Übermitteln Ihres Geräts für die Überprüfung durch das Azure-Zertifizierungsteam

## <a name="prerequisites"></a>Voraussetzungen

- Sie müssen angemeldet sein und über ein Projekt für Ihr Gerät im [Azure Certified Device-Portal](https://certify.azure.com) verfügen. Weitere Informationen finden Sie im [Tutorial](tutorial-01-creating-your-project.md).
- (Optional) Wir empfehlen Ihnen, Ihr Gerät vorzubereiten und dessen Leistung gemäß den Zertifizierungsanforderungen manuell zu überprüfen. Der Grund dafür ist, dass Sie ein neues Projekt erstellen müssen, wenn Sie einen erneuten Test mit einem anderen Gerätecode oder Zertifizierungsprogramm durchführen möchten.

## <a name="connecting-your-device-using-dps"></a>Verbinden Ihres Gerät mit DPS

Alle zertifizierten Geräte müssen die Fähigkeit nachweisen, über DPS eine Verbindung mit IoT Hub herzustellen. Die folgenden Schritte führen Sie durch die erfolgreiche Verbindung Ihres Geräts zu Testzwecken im Portal.

1. Wählen Sie auf der Seite „Projektzusammenfassung“ den Link `Connect & test`, um die Testphase zu starten:  

    ![Link „Verbinden und testen“](./media/images/connect-and-test-link.png)

1. Je nach ausgewählter Zertifizierungsstelle werden die erforderlichen Tests auf der Seite „Verbinden und testen“ angezeigt. Überprüfen Sie diese, um sicherzustellen, dass Sie sich für das richtige Zertifizierungsprogramm bewerben.  

    ![Seite „Verbinden und testen“](./media/images/connect-and-test.png)

1. Verbinden Sie Ihr Gerät über Device Provisioning Service (DPS) mit IoT Hub. DPS unterstützt Konnektivitätsoptionen für symmetrische Schlüssel, X.509-Zertifizierung und Trusted Platform Module (TPM). Dies ist für alle Zertifizierungen erforderlich.

    - *Weitere Informationen zum Herstellen einer Verbindung zwischen Ihrem Gerät und Azure IoT Hub mit DPS finden Sie in der [Übersicht zur Bereitstellung von Geräten](../iot-dps/about-iot-dps.md "Device Provisioning Service – Übersicht").*
    
1. Wenn Sie symmetrische Schlüssel verwenden, werden Sie aufgefordert, DPS mit dem angegebenen DPS-ID-Bereich, der Geräte-ID, dem Authentifizierungsschlüssel und dem DPS-Endpunkt zu konfigurieren. Andernfalls werden Sie aufgefordert, entweder ein X.509-Zertifikat oder einen Endorsement Key anzugeben.

1. Nachdem Sie Ihr Gerät mit DPS konfiguriert haben, bestätigen Sie die Verbindung, indem Sie unten auf der Seite auf die Schaltfläche `Connect` klicken. Nach erfolgreicher Verbindung können Sie mit der Testphase fortfahren, indem Sie auf die `Next` Schaltfläche klicken.  

    ![„Verbinden und testen“ verbunden](./media/images/connected.png)

## <a name="testing-your-device"></a>Testen Ihres Geräts

Nachdem Sie Ihr Gerät erfolgreich mit dem AICS verbunden haben, können Sie nun die spezifischen Zertifizierungstests für das Zertifizierungsprogramm durchführen, für das Sie sich bewerben.

1. **Für die Azure Certified Device-Zertifizierung**: Auf der Registerkarte „Gerätefunktion auswählen“ prüfen und wählen Sie aus, welche Tests Sie für Ihr Gerät durchführen möchten.
1. **Für die IoT Plug & Play-Zertifizierung**: Prüfen Sie sorgfältig die Parameter, die während des Tests überprüft werden, die Sie in Ihrem Gerätemodell angegeben haben.
1. **Für die Edge Managed-Zertifizierung**: Neben dem Nachweis der Konnektivität sind keine weiteren Schritte erforderlich.
1. Nachdem Sie die erforderlichen Vorbereitungen für das angegebene Zertifizierungsprogramm abgeschlossen haben, wählen Sie `Next` aus, um mit der Testphase fortzufahren.
1. Wählen Sie auf der Seite `Run tests` aus, um die Ausführung von AICs mit Ihrem Gerät zu starten.
1. Sobald Sie eine Benachrichtigung erhalten haben, dass die Tests bestanden wurden, wählen Sie `Finish` aus, um zur Zusammenfassungsseite zurückzukehren.

![Bestandene Tests](./media/images/test-pass.png)

7. Wenn Sie weitere Fragen haben oder Hilfe bei der Problembehandlung bei AICs benötigen, rufen Sie unser Handbuch zur Problembehandlung auf.

> [!NOTE]
> Sie können den Onlinezertifizierungsprozess für IoT Plug & Play und Edge Managed zwar abschließen, ohne Ihr Gerät zur manuellen Prüfung einreichen zu müssen, möglicherweise werden Sie jedoch von einem Mitglied des Azure Certified Device-Teams wegen weiterer Gerätevalidierungen kontaktiert, die über das hinausgehen, was durch unseren Automatisierungsdienst getestet wurde.

## <a name="submitting-your-device-for-review"></a>Übermitteln Ihres Geräts zur Bewertung

Wenn Sie alle Pflichtfelder im Abschnitt „Gerätedetails“ ausgefüllt und die automatisierten Tests im Prozess „Verbinden und testen“ erfolgreich bestanden haben, können Sie nun das Azure Certified Device-Team darüber informieren, dass Sie für die Zertifizierungsprüfung bereit sind.

1. Wählen Sie auf der Seite „Projektzusammenfassung“ `Submit for review` aus:  

    ![Link „Überprüfen und zertifizieren“](./media/images/review-and-certify.png)

1. Bestätigen Sie Ihre Übermittlung im Popupfenster. Nachdem ein Gerät übermittelt wurde, sind alle Gerätedetails schreibgeschützt, bis die Bearbeitung angefordert wird. (Weitere Informationen finden Sie unter [Bearbeiten von Geräteinformationen nach der Veröffentlichung](./how-to-edit-published-device.md).)  

    ![Dialogfeld „Zertifizierungsüberprüfung starten“](./media/images/start-certification-review.png)

1. Nachdem das Projekt übermittelt wurde, gibt die Projektzusammenfassungsseite an, dass das Projekt `Under Certification Review` durch das Azure-Zertifizierungsteam ist:  

    ![Wird überprüft](./media/images/review-and-certify-under-review.png)

1. Erwarten Sie innerhalb von 5–7 Werktagen eine E-Mail-Antwort vom Azure-Zertifizierungsteam an die in Ihrem Unternehmensprofil angegebene Adresse bezüglich des Status Ihrer Geräteübermittlung.

    - Genehmigte Übermittlung  
        Sobald Ihr Projekt überprüft und genehmigt wurde, erhalten Sie eine E-Mail. Die E-Mail enthält eine Reihe von Dateien, darunter den Azure Certified Device-Badge, Richtlinien zur Verwendung des Badges und weitere Informationen, wie Sie die Nachricht, dass Ihr Gerät zertifiziert ist, verbreiten können. Glückwunsch!

    - Ausstehende Übermittlung  
        Falls Ihr Projekt nicht genehmigt wird, können Sie Änderungen an den Projektdetails vornehmen und das Gerät anschließend erneut zur Zertifizierung einreichen. Sie erhalten eine E-Mail mit Informationen dazu, warum das Projekt nicht genehmigt wurde und welche Schritte zur erneuten Einreichung für die Zertifizierung erforderlich sind.

## <a name="next-steps"></a>Nächste Schritte

Glückwunsch! Ihr Gerät hat nun erfolgreich alle Tests bestanden und wurde über das Azure Certified Device-Programm genehmigt. Sie können Ihr Gerät jetzt in unserem Azure Certified Device-Katalog veröffentlichen, wo Kunden Ihre Produkte mit Vertrauen in deren Leistung in Verbindung mit Azure kaufen können.
> [!div class="nextstepaction"]
> [Tutorial: Veröffentlichen Ihres Geräts](tutorial-04-publishing-your-device.md)

