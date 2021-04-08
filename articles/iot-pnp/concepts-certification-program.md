---
title: Azure Certified Device-Programm | Microsoft-Dokumentation
description: Grundlegendes zum Azure Certified Device-Programm.
author: koichih
ms.author: koichih
ms.date: 09/25/2020
ms.topic: conceptual
ms.custom: mvc
ms.service: iot-pnp
services: iot-pnp
ms.openlocfilehash: 1fcfc7a9e632e5db1fb809dba7a938c8641c9ddc
ms.sourcegitcommit: f28ebb95ae9aaaff3f87d8388a09b41e0b3445b5
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/29/2021
ms.locfileid: "92048099"
---
# <a name="what-is-the-azure-certified-device-program"></a>Was ist das Azure Certified Device-Programm?

Das Azure Certified Device-Programm stellt sicher, dass Kundenlösungen nahtlos mit Ihren Azure-Diensten zusammenarbeiten. Das Programm verwendet Tools, Dienste und einen Marketplace, um Branchenwissen und bewährte Methoden mit einer Community von Geräte- und Lösungsherstellern zu teilen.

Mit diesem Programm werden folgende Zwecke verfolgt:

- **Vertrauen bei den Kunden schaffen:** Kunden können getrost Geräte erwerben, die von Microsoft für die Verwendung mit Azure-Diensten zertifiziert sind.

- **Kunden helfen, die richtigen Geräte für ihre Lösung zu finden:** Gerätehersteller können die einzigartigen Funktionen ihrer Geräte im Rahmen des Zertifizierungsprozesses veröffentlichen. Kunden können leicht diejenigen Produkte finden, die ihren Anforderungen entsprechen.

- **Zertifizierte Geräte bewerben:** Gerätehersteller erhalten mehr Sichtbarkeit und Kundenkontakte und können die Marke „Azure Certified Device“ nutzen.

Dieser Artikel beschreibt, wie Sie:

- Das Onboarding Ihres Unternehmens in das Azure Certified Device-Programm durchführen.
- Bestimmen, welche Zertifizierung sich für Ihr Gerät eignet.
- Programmanforderungen und andere Ressourcen finden, die Ihnen den Einstieg in Tests erleichtern.
- Das Azure Certified Device-Portal verwenden, um Ihr Gerät zu überprüfen.

## <a name="onboarding"></a>Onboarding

Führen Sie die folgenden Schritte aus, um Ihre Geräte auf dem [Azure Certified Device-Portal](https://aka.ms/acdp) zertifizieren zu lassen:

1. Stellen Sie sicher, dass Ihr Unternehmen über ein Azure Active Directory-Konto mit einem Geschäfts- oder Schulmandanten verfügt.
2. Treten Sie mithilfe Ihres Kontos dem [Microsoft Partner Network (MPN)](https://partner.microsoft.com/) bei.
3. Melden Sie sich beim [Azure Certified Device-Portal](https://aka.ms/acdp) an, nachdem Sie dem MPN beigetreten sind.
4. Prüfen und unterzeichnen Sie die [Programmvereinbarung](https://aka.ms/acdagreement) auf der Seite „Unternehmensprofil“.

### <a name="company-profile"></a>Unternehmensprofil

Um das Profil Ihres Unternehmens auf dem Azure Certified Device-Portal zu verwalten, wählen Sie **Unternehmensprofil** aus. Das Unternehmensprofil umfasst die Firmen-URL, die E-Mail-Adresse und das Logo. Sie müssen die Programmvereinbarung auf dieser Seite akzeptieren, bevor Sie mit Zertifizierungsvorgängen fortfahren können.

Die Seite „Gerätebeschreibung“ im Azure Certified Device-Katalog verwendet die Informationen des Unternehmensprofils.

## <a name="choose-the-certification"></a>Auswählen der Zertifizierung

Es gibt drei verschiedene Zertifizierungen, die sich jeweils auf einen unterschiedlichen Kundennutzen konzentrieren. Je nach Art des Geräts, das Sie herstellen, und Ihrer Zielgruppe können Sie die Zertifizierung bzw. die Zertifizierungen wählen, die am besten geeignet ist/sind:

### <a name="azure-certified-device"></a>Azure Certified Device

Die _Azure Certified Device-Zertifizierung_ prüft, ob ein Gerät eine Verbindung mit Azure IoT Hub herstellen kann und eine sichere Bereitstellung über den Device Provisioning Service (DPS) ermöglicht. Diese Zertifizierung spiegelt die Funktionalität und Interoperabilität eines Geräts wider, die eine erforderliche Voraussetzung für weitergehende Zertifizierungen darstellt.

- Weitere Informationen finden Sie in den [Zertifizierungsanforderungen](https://aka.ms/acdrequirements).
- Weitere Informationen über die Verwendung von DPS zum Verbinden Ihres Geräts mit Azure IoT Hub finden Sie in der [Übersicht über die Bereitstellung von Geräten](../iot-dps/about-iot-dps.md).

### <a name="iot-plug-and-play"></a>IoT Plug & Play

Die _IoT Plug & Play-Zertifizierung_, eine über die Azure Certified Device-Zertifizierung hinausgehende Zertifizierung, vereinfacht das Entwickeln von Geräten ohne benutzerdefinierten Gerätecode. Mit IoT Plug & Play können Hardwarepartner Geräte herstellen, die sich auf einfache Weise in Cloudlösungen integrieren lassen, die auf Azure IoT Central und Lösungen von Drittanbietern basieren.

- Weitere Informationen finden Sie in den [Zertifizierungsanforderungen](https://aka.ms/acdiotpnprequirements).
- Weitere Informationen über das Vorbereiten eines Geräts für IoT Plug & Play-Tests finden Sie unter [Zertifizieren von IoT Plug & Play-Geräten](howto-certify-device.md).

### <a name="edge-managed"></a>Edge Managed

Die _Edge Managed-Zertifizierung_, eine über die Azure Certified Device-Zertifizierung hinausgehende Zertifizierung, konzentriert sich auf Geräteverwaltungsstandards für Azure IoT-Geräte, auf denen Windows, Linux oder RTOS ausgeführt wird. Derzeit konzentriert sich diese Programmzertifizierung auf die Edge-Runtime-Kompatibilität für die Modulbereitstellung und -verwaltung.

> [!TIP]
> Dieses Programm trug zuvor die Bezeichnung _IoT Edge-Zertifizierungsprogramm_.

- Weitere Informationen finden Sie in den [Zertifizierungsanforderungen](https://aka.ms/acdedgemanagedrequirements).
- Weitere Informationen zu IoT Edge finden Sie in der [Übersicht über IoT Edge](../iot-edge/about-iot-edge.md).
- Weitere Informationen zu unterstützten Betriebssystemen und Containern finden Sie unter [Von Azure IoT Edge unterstützte Systeme](../iot-edge/support.md).

## <a name="use-the-azure-certified-device-portal"></a>Verwenden des Azure Certified Device-Portals

In diesem Abschnitt wird die Verwendung des [Azure Certified Device-Portals](https://certify.azure.com) zusammenfassend beschrieben. Weitere Informationen finden Sie im Leitfaden [Erste Schritte mit dem Portal](https://aka.ms/acdhelp).

Führen Sie die folgenden vier Schritte aus, um ein Gerät im Azure Certified Device-Programm zertifizieren zu lassen:

1. Stellen Sie die Gerätedetails bereit
2. Testen Sie das Gerät
3. Übermitteln Sie die Überprüfung und schließen Sie sie ab
4. Nehmen Sie die Veröffentlichung im Azure Certified Device-Katalog vor (optional)

### <a name="provide-device-details"></a>Bereitstellen von Gerätedetails

Verwenden Sie für jedes Gerät, das Sie zertifizieren möchten, die Formulare im Zertifizierungsportal, um Details zur Gerätehardware, Einrichtungsanweisungen und Marketingmaterial zu erfassen:

- **Geräteinformationen:** Erfasst Informationen zum Gerät wie Name, Beschreibung, Hardwaredetails und Betriebssystem.
- **Leitfaden „Erste Schritte“** : Ein PDF-Dokument, das ein Kunde verwenden kann, um Ihr Produkt schnell in Gebrauch zu nehmen. [Beispielvorlagen](https://aka.ms/GSTemplate) sind verfügbar.
- **Marketingdetails:** Stellen Sie an den Kunden gerichtete Marketinginformationen zu Ihrem Gerät bereit, z. B. ein Bild und Informationen zum Vertriebspartner.
- **Zusätzliche Industriezertifizierungen:** In diesem optionalen Abschnitt können Sie Informationen zu weiteren Zertifizierungen angeben, über die das Gerät verfügt.

### <a name="test-the-device"></a>Testen des Geräts

Diese Phase interagiert mit Ihrem Gerät und führt eine Reihe von Tests durch, nachdem das Gerät mithilfe von DPS eine Verbindung mit IoT Hub hergestellt hat. Nach Abschluss der Tests können Sie sich eine Reihe von Protokolldateien mit den Testergebnissen Ihres Geräts anzeigen lassen.

Das Zertifizierungsportal enthält Anweisungen zum Herstellen einer Verbindung mit der IoT Hub-Instanz, die zum Testen verwendet wird. Sie können die DPS-Verbindung mithilfe einer der [unterstützten Nachweismethoden](../iot-dps/concepts-service.md#attestation-mechanism) herstellen.

Für eine weitere manuelle Prüfung des Geräts wendet sich das Azure Certified Device-Team unter Umständen an den Gerätehersteller.

### <a name="submit-and-publish"></a>Übermitteln und Veröffentlichen

Die letzte erforderliche Phase besteht darin, das Projekt zur Überprüfung zu übermitteln. Bei diesem Schritt wird ein Mitglied des Azure Certified Device-Teams benachrichtigt, damit es Ihr Projekt auf Vollständigkeit überprüft, einschließlich der Geräte- und Marketingdetails sowie des Leitfadens „Erste Schritte“. Vor dem Erteilen der Genehmigung kontaktiert Sie möglicherweise ein Teammitglied unter der zuvor angegebenen E-Mail-Adresse des Unternehmens mit Fragen oder Änderungsanforderungen.

Wenn Ihr Gerät im Rahmen der Zertifizierung eine weitere manuelle Überprüfung erfordert, werden Sie zu diesem Zeitpunkt hierüber benachrichtigt.

Nachdem ein Gerät zertifiziert wurde, können Sie auf Wunsch Ihre Produktdetails im Azure Certified Device-Katalog veröffentlichen, indem Sie die Funktion **In Katalog veröffentlichen** auf der Produktübersichtsseite verwenden.

## <a name="if-you-have-questions"></a>Bei Fragen

Wenden Sie sich unter [iotcert@microsoft.com](mailto:iotcert@microsoft.com?subject=Azure%20Certified%20Device%20question) an das Team, wenn Sie Fragen zu den Zertifizierungsprogrammen, dem Zertifizierungsportal oder dem Azure Certified Device-Katalog haben.

## <a name="next-steps"></a>Nächste Schritte

Nachdem Sie nun eine Übersicht über das Azure Certified Device-Programm erhalten haben, wird empfohlen, sich im nächsten Schritt mit dem [Zertifizieren von IoT Plug & Play-Geräten](howto-certify-device.md) zu befassen.