---
title: Einrichten von Azure Attestation über das Azure-Portal
description: Es wird beschrieben, wie Sie einen Nachweisanbieter über das Azure-Portal einrichten und konfigurieren.
services: attestation
author: msmbaldwin
ms.service: attestation
ms.topic: overview
ms.date: 08/31/2020
ms.author: mbaldwin
ms.openlocfilehash: 52c0dd6825c8576d97e10d3f0568ca5fb81e789e
ms.sourcegitcommit: f5b8410738bee1381407786fcb9d3d3ab838d813
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/14/2021
ms.locfileid: "98210801"
---
# <a name="quickstart-set-up-azure-attestation-with-azure-portal"></a>Schnellstart: Einrichten von Azure Attestation über das Azure-Portal

Führen Sie die folgenden Schritte aus, um einen Nachweisanbieter über das Azure-Portal zu verwalten.

## <a name="attestation-provider"></a>Nachweisanbieter

### <a name="create-an-attestation-provider"></a>Erstellen eines Nachweisanbieters

#### <a name="to-configure-the-provider-with-unsigned-policies"></a>Konfigurieren des Anbieters mit unsignierten Richtlinien

1.  Wählen Sie im Menü des Azure-Portals oder auf der Startseite die Option **Ressource erstellen** aus.
2.  Geben Sie im Suchfeld **Attestation** ein.
3.  Wählen Sie in der Ergebnisliste den Eintrag **Microsoft Azure Attestation** aus.
4.  Wählen Sie auf der Microsoft Azure Attestation-Seite die Option **Erstellen** aus.
5.  Geben Sie auf der Seite „Nachweisanbieter erstellen“ die folgenden Daten an:
    
    **Abonnement**: Wählen Sie ein Abonnement.
    
    **Ressourcengruppe**: Wählen Sie eine bereits vorhandene Ressourcengruppe aus, oder wählen Sie die Option **Neu erstellen** aus, und geben Sie einen Ressourcengruppennamen ein.
    
    **Name**: Hier muss ein eindeutiger Name angegeben werden.

    **Standort**: Wählen Sie einen Standort aus. 
    
    **Datei für Richtlinien-Signaturgeberzertifikate**: Laden Sie keine Datei für Richtlinien-Signaturgeberzertifikate hoch, um den Anbieter mit unsignierten Richtlinien zu konfigurieren. 
6.  Klicken Sie auf **Überprüfen und erstellen**, nachdem Sie die erforderlichen Daten eingegeben haben.
7.  Beheben Sie die Probleme bei der Überprüfung, falls vorhanden, und klicken Sie auf **Erstellen**.

#### <a name="to-configure-the-provider-with-signed-policies"></a>Konfigurieren des Anbieters mit signierten Richtlinien

1.  Wählen Sie im Menü des Azure-Portals oder auf der Startseite die Option **Ressource erstellen** aus.
2.  Geben Sie im Suchfeld **Attestation** ein.
3.  Wählen Sie in der Ergebnisliste den Eintrag **Microsoft Azure Attestation** aus.
4.  Wählen Sie auf der Microsoft Azure Attestation-Seite die Option **Erstellen** aus.
5.  Geben Sie auf der Seite „Nachweisanbieter erstellen“ die folgenden Informationen an:
    
    a. **Abonnement**: Wählen Sie ein Abonnement.
    
    b. **Ressourcengruppe**: Wählen Sie eine bereits vorhandene Ressourcengruppe aus, oder wählen Sie die Option **Neu erstellen** aus, und geben Sie einen Ressourcengruppennamen ein.
    
    c. **Name**: Hier muss ein eindeutiger Name angegeben werden.

    d. **Standort**: Wählen Sie einen Standort aus. 
    
    e. **Datei für Richtlinien-Signaturgeberzertifikate**: Laden Sie die Zertifikatdatei hoch, um den Nachweisanbieter mit Richtlinien-Signaturgeberzertifikaten zu konfigurieren. Hilfreiche Beispiele finden Sie [hier](/azure/attestation/policy-signer-examples). 
6.  Klicken Sie auf **Überprüfen und erstellen**, nachdem Sie die erforderlichen Daten eingegeben haben.
7.  Beheben Sie die Probleme bei der Überprüfung, falls vorhanden, und klicken Sie auf **Erstellen**.

### <a name="view-attestation-provider"></a>Anzeigen des Nachweisanbieters

1.  Wählen Sie im Azure-Portal im Menü oder auf der Startseite die Option **Alle Ressourcen** aus.
2.  Geben Sie im Filterfeld den Namen des Nachweisanbieters ein, und wählen Sie ihn aus.

### <a name="delete-attestation-provider"></a>Löschen des Nachweisanbieters

1.  Wählen Sie im Azure-Portal im Menü oder auf der Startseite die Option **Alle Ressourcen** aus.
2.  Geben Sie im Filterfeld den Namen des Nachweisanbieters ein.
3.  Aktivieren Sie das Kontrollkästchen, und klicken Sie auf **Löschen**.
4.  Geben Sie „Ja“ ein, und klicken Sie auf **Löschen**. [ODER]
1.  Wählen Sie im Azure-Portal im Menü oder auf der Startseite die Option **Alle Ressourcen** aus.
2.  Geben Sie im Filterfeld den Namen des Nachweisanbieters ein.
3.  Wählen Sie den Nachweisanbieter aus, und navigieren Sie zur Übersichtsseite.
4.  Klicken Sie im obersten Menü auf **Löschen** und dann auf **Ja**.


## <a name="attestation-policy-signers"></a>Nachweisrichtlinien-Signaturgeber

### <a name="view-policy-signer-certificates"></a>Anzeigen von Richtlinien-Signaturgeberzertifikaten

1.  Wählen Sie im Azure-Portal im Menü oder auf der Startseite die Option **Alle Ressourcen** aus.
2.  Geben Sie im Filterfeld den Namen des Nachweisanbieters ein.
3.  Wählen Sie den Nachweisanbieter aus, und navigieren Sie zur Übersichtsseite.
4.  Klicken Sie links im Ressourcenmenü oder im unteren Bereich auf **Richtlinien-Signaturgeberzertifikate**.
5.  Klicken Sie auf **Download policy signer certificates** (Richtlinien-Signaturgeberzertifikate herunterladen). (Für Nachweisanbieter, die ohne obligatorische Signierung von Richtlinien erstellt wurden, ist die Schaltfläche deaktiviert.)
6.  Die heruntergeladene Textdatei enthält alle Zertifikate im JWS-Format.
a.  Überprüfen Sie die Anzahl von Zertifikaten und die heruntergeladenen Zertifikate.

### <a name="add-policy-signer-certificate"></a>Hinzufügen eines Richtlinien-Signaturgeberzertifikats

1.  Wählen Sie im Azure-Portal im Menü oder auf der Startseite die Option **Alle Ressourcen** aus.
2.  Geben Sie im Filterfeld den Namen des Nachweisanbieters ein.
3.  Wählen Sie den Nachweisanbieter aus, und navigieren Sie zur Übersichtsseite.
4.  Klicken Sie links im Ressourcenmenü oder im unteren Bereich auf **Richtlinien-Signaturgeberzertifikate**.
5.  Klicken Sie im obersten Menü auf **Hinzufügen**. (Für Nachweisanbieter, die ohne obligatorische Signierung von Richtlinien erstellt wurden, ist die Schaltfläche deaktiviert.)
6.  Laden Sie die Datei für Richtlinien-Signaturgeberzertifikate hoch, und klicken Sie auf **Hinzufügen**. Hilfreiche Beispiele finden Sie [hier](/azure/attestation/policy-signer-examples).

### <a name="delete-policy-signer-certificate"></a>Löschen des Richtlinien-Signaturgeberzertifikats

1.  Wählen Sie im Azure-Portal im Menü oder auf der Startseite die Option **Alle Ressourcen** aus.
2.  Geben Sie im Filterfeld den Namen des Nachweisanbieters ein.
3.  Wählen Sie den Nachweisanbieter aus, und navigieren Sie zur Übersichtsseite.
4.  Klicken Sie links im Ressourcenmenü oder im unteren Bereich auf **Richtlinien-Signaturgeberzertifikate**.
5.  Klicken Sie im obersten Menü auf **Löschen**. (Für Nachweisanbieter, die ohne obligatorische Signierung von Richtlinien erstellt wurden, ist die Schaltfläche deaktiviert.)
6.  Laden Sie die Datei für Richtlinien-Signaturgeberzertifikate hoch, und klicken Sie auf **Löschen**. Hilfreiche Beispiele finden Sie [hier](/azure/attestation/policy-signer-examples). 

## <a name="attestation-policy"></a>Nachweisrichtlinie

### <a name="view-attestation-policy"></a>Anzeigen der Nachweisrichtlinie

1.  Wählen Sie im Azure-Portal im Menü oder auf der Startseite die Option **Alle Ressourcen** aus.
2.  Geben Sie im Filterfeld den Namen des Nachweisanbieters ein.
3.  Wählen Sie den Nachweisanbieter aus, und navigieren Sie zur Übersichtsseite.
4.  Klicken Sie links im Ressourcenmenü oder im unteren Bereich auf **Richtlinie**.
5.  Wählen Sie den gewünschten **Nachweistyp** aus, und zeigen Sie die **Aktuelle Richtlinie** an.

### <a name="configure-attestation-policy"></a>Konfigurieren der Nachweisrichtlinie

#### <a name="when-attestation-provider-is-created-without-policy-signing-requirement"></a>Erstellung des Nachweisanbieters ohne obligatorische Signierung von Richtlinien

##### <a name="upload-policy-in-jwt-format"></a>Laden Sie die Richtlinie im JWT-Format hoch.

1.  Wählen Sie im Azure-Portal im Menü oder auf der Startseite die Option **Alle Ressourcen** aus.
2.  Geben Sie im Filterfeld den Namen des Nachweisanbieters ein.
3.  Wählen Sie den Nachweisanbieter aus, und navigieren Sie zur Übersichtsseite.
4.  Klicken Sie links im Ressourcenmenü oder im unteren Bereich auf **Richtlinie**.
5.  Klicken Sie im obersten Menü auf **Konfigurieren**.
6.  Nachdem der Nachweisanbieter ohne obligatorische Signierung von Richtlinien erstellt wurde, kann der Benutzer eine Richtlinie im Format **JWT** oder **Text** hochladen.
7.  Wählen Sie unter **Richtlinienformat** die Option **JWT** aus.
8.  Laden Sie eine Richtliniendatei mit Richtlinieninhalten im Format **Unsigniertes/Signiertes JWT** hoch, und klicken Sie auf **Speichern**. Beispiele finden Sie [hier](/azure/attestation/policy-examples).
    
    Bei der Option zum Hochladen von Dateien wird die Richtlinienvorschau im Textformat angezeigt und kann nicht bearbeitet werden.

7.  Klicken Sie im obersten Menü auf **Aktualisieren**, um die konfigurierte Richtlinie anzuzeigen.

##### <a name="upload-policy-in-text-format"></a>Hochladen einer Richtlinie im Textformat

1.  Wählen Sie im Azure-Portal im Menü oder auf der Startseite die Option **Alle Ressourcen** aus.
2.  Geben Sie im Filterfeld den Namen des Nachweisanbieters ein.
3.  Wählen Sie den Nachweisanbieter aus, und navigieren Sie zur Übersichtsseite.
4.  Klicken Sie links im Ressourcenmenü oder im unteren Bereich auf **Richtlinie**.
5.  Klicken Sie im obersten Menü auf **Konfigurieren**.
6.  Nachdem der Nachweisanbieter ohne obligatorische Signierung von Richtlinien erstellt wurde, kann der Benutzer eine Richtlinie im Format **JWT** oder **Text** hochladen.
7.  Wählen Sie unter **Richtlinienformat** die Option **Text** aus.
8.  Laden Sie die Richtliniendatei mit den entsprechenden Inhalten im Format **Text** hoch, oder geben Sie die Richtlinieninhalte im Textbereich ein, und klicken Sie auf **Speichern**. Beispiele finden Sie [hier](/azure/attestation/policy-examples).

    Bei der Option zum Hochladen von Dateien wird die Richtlinienvorschau im Textformat angezeigt und kann nicht bearbeitet werden.

8.  Klicken Sie auf **Aktualisieren**, um die konfigurierte Richtlinie anzuzeigen.

#### <a name="when-attestation-provider-is-created-with-policy-signing-requirement"></a>Erstellung des Nachweisanbieters mit obligatorischer Signierung von Richtlinien

##### <a name="upload-policy-in-jwt-format"></a>Laden Sie die Richtlinie im JWT-Format hoch.

1.  Wählen Sie im Azure-Portal im Menü oder auf der Startseite die Option **Alle Ressourcen** aus.
2.  Geben Sie im Filterfeld den Namen des Nachweisanbieters ein.
3.  Wählen Sie den Nachweisanbieter aus, und navigieren Sie zur Übersichtsseite.
4.  Klicken Sie links im Ressourcenmenü oder im unteren Bereich auf **Richtlinie**.
5.  Klicken Sie im obersten Menü auf **Konfigurieren**.
6.  Nachdem der Nachweisanbieter mit obligatorischer Signierung von Richtlinien erstellt wurde, kann der Benutzer eine Richtlinie nur im Format **Signiertes JWT** hochladen.
7.  Laden Sie die Richtliniendatei im Format **Signiertes JWT** hoch, und klicken Sie auf **Speichern**. Beispiele finden Sie [hier](/azure/attestation/policy-examples).

    Bei der Option zum Hochladen von Dateien wird die Richtlinienvorschau im Textformat angezeigt und kann nicht bearbeitet werden.
    
8.  Klicken Sie auf **Aktualisieren**, um die konfigurierte Richtlinie anzuzeigen.

 










