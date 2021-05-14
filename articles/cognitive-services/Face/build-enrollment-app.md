---
title: Erstellen einer React-App zum Hinzufügen von Benutzern zu einem Gesichtserkennungs-Dienst
titleSuffix: Azure Cognitive Services
description: Erfahren Sie, wie Sie Ihre Entwicklungsumgebung einrichten und eine Gesichtserkennungs-App bereitstellen, um die Zustimmung von Kunden zu erhalten.
author: PatrickFarley
manager: nitinme
ms.service: cognitive-services
ms.subservice: face-api
ms.topic: conceptual
ms.date: 11/17/2020
ms.author: pafarley
ms.openlocfilehash: 39a74c7f3d5fb8f8b60a66947fcce9837ed6ee13
ms.sourcegitcommit: 3b5cb7fb84a427aee5b15fb96b89ec213a6536c2
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/14/2021
ms.locfileid: "107505104"
---
# <a name="build-a-react-app-to-add-users-to-a-face-service"></a>Erstellen einer React-App zum Hinzufügen von Benutzern zu einem Gesichtserkennungs-Dienst

In dieser Anleitung lernen Sie die ersten Schritte mit der Registrierungsbeispielanwendung für Gesichtserkennung kennen. Die App veranschaulicht bewährte Methoden, um eine aussagekräftige Zustimmung zum Hinzufügen von Benutzern bei einem Gesichtserkennungsdienst und zum Abrufen von hoch exakten Gesichtserkennungsdaten zu erhalten. Ein integriertes System könnte eine App wie diese verwenden, um eine berührungslose Zugriffssteuerung, eine Identitätsprüfung, eine Anwesenheitserfassung, einen Personalisierungskiosk oder eine auf ihren Gesichtserkennungsdaten basierende Identitätsprüfung bereitzustellen.

Wenn die Anwendung gestartet wird, zeigt sie den Benutzern einen detaillierten Zustimmungsbildschirm an. Wenn der Benutzer seine Zustimmung erteilt, fordert die App ihn zur Eingabe eines Benutzernamens und Kennworts auf und erfasst dann ein qualitativ hochwertiges Bild des Gesichts mithilfe der Kamera des Geräts.

Die Beispiel-App wird mit JavaScript und dem React Native-Framework geschrieben. Sie kann derzeit auf Android-Geräten bereitgestellt werden. Weitere Bereitstellungsoptionen werden in Zukunft verfügbar gemacht.

## <a name="prerequisites"></a>Voraussetzungen 

* Azure-Abonnement ([kostenloses Abonnement erstellen](https://azure.microsoft.com/free/cognitive-services/)).  
* Sobald Sie über Ihr Azure-Abonnement verfügen, sollten Sie im Azure-Portal [eine Gesichtserkennungsressource erstellen](https://portal.azure.com/#create/Microsoft.CognitiveServicesFace), um Ihren Schlüssel und Endpunkt zu erhalten. Wählen Sie nach Abschluss der Bereitstellung **Zu Ressource wechseln** aus.  
  * Sie benötigen den Schlüssel und Endpunkt der von Ihnen erstellten Ressource, um Ihre Anwendung mit der Gesichtserkennungs-API zu verbinden.  
  * Für lokale Entwicklungs- und Testzwecke können Sie den API-Schlüssel und Endpunkt in die Konfigurationsdatei einfügen. Speichern Sie den API-Schlüssel für die endgültige Bereitstellung an einem sicheren Ort und nie im Code.  

> [!IMPORTANT]
> Diese Abonnementschlüssel werden für den Zugriff auf Ihre Cognitive Service-API verwendet. Geben Sie Ihre Schlüssel nicht weiter. Speichern Sie diese beispielsweise sicher mit Azure Key Vault. Es wird außerdem empfohlen, diese Schlüssel regelmäßig neu zu generieren. Für einen API-Aufruf ist nur ein Schlüssel erforderlich. Beim erneuten Generieren des ersten Schlüssels können Sie den zweiten Schlüssel für kontinuierlichen Zugriff auf den Dienst verwenden.

## <a name="set-up-the-development-environment"></a>Einrichten der Entwicklungsumgebung

1. Klonen Sie das Git-Repository für die [Beispiel-App](https://github.com/azure-samples/cognitive-services-FaceAPIEnrollmentSample).
1. Um Ihre Entwicklungsumgebung einzurichten, befolgen Sie die <a href="https://reactnative.dev/docs/environment-setup"  title=""  target="_blank">React Native-Dokumentation</a>. Wählen Sie **React Native CLI Schnellstart** als Entwicklungsbetriebssystem und **Android** als Zielbetriebssystem aus. Vervollständigen Sie die Abschnitte **Installieren von Abhängigkeiten** und **Android-Entwicklungsumgebung**.
1. Öffnen Sie die Datei „env.json“ in Ihrem bevorzugten Text-Editor, z. B. [Visual Studio Code](https://code.visualstudio.com/), und fügen Sie Ihren Endpunkt und Schlüssel hinzu. Sie können Ihren Endpunkt und Schlüssel im Azure-Portal auf der Registerkarte **Übersicht** Ihrer Ressource finden. Dieser Schritt dient nur lokalen Testzwecken. Checken Sie nicht Ihren Gesichtserkennungs-API-Schlüssel in Ihrem Remoterepository ein.
1. Führen Sie die App entweder mit dem Android-Emulator für virtuelle Geräte aus Android Studio oder Ihrem eigenen Android-Gerät aus. Um Ihre App auf einem physischen Gerät zu testen, befolgen Sie die relevante <a href="https://reactnative.dev/docs/running-on-device"  title=""  target="_blank">React Native-Dokumentation</a>.  


## <a name="create-a-user-add-experience"></a>Erstellen einer Benutzeroberfläche zum Hinzufügen von Benutzern  

Nachdem Sie nun die Beispiel-App eingerichtet haben, können Sie sie an Ihre eigenen Anforderungen anpassen.

Beispielsweise können Sie auf Ihrer Zustimmungsseite situationsabhängige Informationen hinzufügen:

> [!div class="mx-imgBorder"]
> ![App-Zustimmungsseite](./media/enrollment-app/1-consent-1.jpg)

Der Dienst bietet Bildqualitätstests, um Ihnen bei der Entscheidung zu helfen, ob die Qualität des Bilds ausreichend ist, um den Kunden hinzuzufügen oder die Gesichtserkennung zu versuchen. Diese App veranschaulicht, wie Sie über die Kamera des Geräts auf Frames zugreifen, die Frames mit der höchsten Qualität auswählen und das erkannte Gesicht zum Gesichtserkennungs-API-Dienst hinzufügen. 

Viele Gesichtserkennungsprobleme werden durch Referenzbilder von niedriger Qualität verursacht. Einige Faktoren, die die Leistung des Modells beeinträchtigen können, sind:
* Gesichtsgröße (Gesichter, die weit von der Kamera entfernt sind)
* Gesichtsausrichtung (geneigte oder von der Kamera abgewendete Gesichter)
* Schlechte Beleuchtungsbedingungen (zu wenig Licht oder Gegenlicht), bei denen das Bild unterbelichtet wird oder zu viel Rauschen auftritt.
* Verdeckung (teilweise verborgene oder verdeckte Gesichter), einschließlich Accessoires wie Hüten oder Brillen mit dickem Gestell.
* Verschwommen/Weichzeichnung (z. B. durch schnelle Bewegung des Gesichts während der Aufnahme des Fotos). 

> [!div class="mx-imgBorder"]
> ![Seite mit Anweisungen für die App-Bilderfassung](./media/enrollment-app/4-instruction.jpg)

Beachten Sie, dass die App auch Funktionen zum Löschen der Information des Benutzers sowie die Option zum erneuten Hinzufügen bietet.

> [!div class="mx-imgBorder"]
> ![Profilverwaltungsseite](./media/enrollment-app/10-manage-2.jpg)

Um die Funktionalität der App so zu erweitern, dass Sie die gesamte Erfahrung abdeckt, lesen Sie die [Übersicht](enrollment-overview.md), wo Sie zusätzliche zu implementierende Funktionen und bewährte Methoden finden.

## <a name="deploy-the-app"></a>Bereitstellen der App

### <a name="android"></a>Android

Stellen Sie zunächst sicher, dass Ihre App für die Produktionsbereitstellung bereit ist: Entfernen Sie alle Schlüssel oder Geheimnisse aus dem App-Code, und stellen Sie sicher, dass Sie die [bewährten Sicherheitsmethoden](../cognitive-services-security.md?tabs=command-line%2ccsharp) befolgt haben.

Wenn Sie bereit sind, Ihre App für die Produktion freizugeben, generieren Sie eine releasebereite APK-Datei, bei der es sich um das Paketdateiformat für Android-Apps handelt. Diese APK-Datei muss mit einem privaten Schlüssel signiert werden. Mit diesem Releasebuild können Sie beginnen, die App direkt an Ihre Geräte zu verteilen. 

Befolgen Sie die Dokumentation zur <a href="https://developer.android.com/studio/publish/preparing#publishing-build"  title="Vorbereiten für das Release"  target="_blank">Vorbereiten für das Release</a>, um zu erfahren, wie Sie einen privaten Schlüssel generieren, Ihre Anwendung signieren und ein APK-Releasepaket generieren.  

Wenn Sie ein signiertes APK-Paket erstellt haben, finden Sie weitere Informationen zum Freigeben Ihrer App in der Dokumentation <a href="https://developer.android.com/studio/publish"  title="Veröffentlichen Ihrer App"  target="_blank">Veröffentlichen Ihrer App</a>.

## <a name="next-steps"></a>Nächste Schritte  

In dieser Anleitung haben Sie erfahren, wie Sie Ihre Entwicklungsumgebung einrichten und mit der Beispiel-App beginnen. Wenn Sie nicht mit React Native vertraut sind, können Sie die dazugehörige [Dokumentation zu den ersten Schritten](https://reactnative.dev/docs/getting-started) lesen, um weitere Hintergrundinformationen zu erhalten. Es kann auch hilfreich sein, sich selbst mit der [Gesichtserkennungs-API](Overview.md) vertraut zu machen. Lesen Sie die anderen Abschnitte zum Hinzufügen von Benutzern zur App, bevor Sie mit der Entwicklung beginnen.