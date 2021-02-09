---
title: Einrichten von Azure Attestation über das Azure-Portal
description: Es wird beschrieben, wie Sie einen Nachweisanbieter über das Azure-Portal einrichten und konfigurieren.
services: attestation
author: msmbaldwin
ms.service: attestation
ms.topic: overview
ms.date: 08/31/2020
ms.author: mbaldwin
ms.openlocfilehash: 86adac557c6de133e95e97bfedbd302cc6a2b27e
ms.sourcegitcommit: eb546f78c31dfa65937b3a1be134fb5f153447d6
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/02/2021
ms.locfileid: "99429156"
---
# <a name="quickstart-set-up-azure-attestation-with-azure-portal"></a>Schnellstart: Einrichten von Azure Attestation über das Azure-Portal

## <a name="prerequisites"></a>Voraussetzungen

Wenn Sie kein Azure-Abonnement besitzen, können Sie ein [kostenloses Konto](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) erstellen, bevor Sie beginnen.

Führen Sie die folgenden Schritte aus, um einen Nachweisanbieter über das Azure-Portal zu verwalten.

## <a name="1-attestation-provider"></a>1. Nachweisanbieter

### <a name="11-create-an-attestation-provider"></a>1.1 Erstellen eines Nachweisanbieters

#### <a name="111-to-configure-the-provider-with-unsigned-policies"></a>1.1.1 Konfigurieren des Anbieters mit unsignierten Richtlinien

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

#### <a name="112-to-configure-the-provider-with-signed-policies"></a>1.1.2 Konfigurieren des Anbieters mit signierten Richtlinien

1.  Wählen Sie im Menü des Azure-Portals oder auf der Startseite die Option **Ressource erstellen** aus.
2.  Geben Sie im Suchfeld **Attestation** ein.
3.  Wählen Sie in der Ergebnisliste den Eintrag **Microsoft Azure Attestation** aus.
4.  Wählen Sie auf der Microsoft Azure Attestation-Seite die Option **Erstellen** aus.
5.  Geben Sie auf der Seite „Nachweisanbieter erstellen“ die folgenden Informationen an:
    
    a. **Abonnement**: Wählen Sie ein Abonnement.
    
    b. **Ressourcengruppe**: Wählen Sie eine bereits vorhandene Ressourcengruppe aus, oder wählen Sie die Option **Neu erstellen** aus, und geben Sie einen Ressourcengruppennamen ein.
    
    c. **Name**: Hier muss ein eindeutiger Name angegeben werden.

    d. **Standort**: Wählen Sie einen Standort aus. 
    
    e. **Datei für Richtlinien-Signaturgeberzertifikate**: Laden Sie die Zertifikatdatei hoch, um den Nachweisanbieter mit Richtlinien-Signaturgeberzertifikaten zu konfigurieren. Hilfreiche Beispiele finden Sie [hier](./policy-signer-examples.md). 
6.  Klicken Sie auf **Überprüfen und erstellen**, nachdem Sie die erforderlichen Daten eingegeben haben.
7.  Beheben Sie die Probleme bei der Überprüfung, falls vorhanden, und klicken Sie auf **Erstellen**.

### <a name="12-view-attestation-provider"></a>1.2 Anzeigen des Nachweisanbieters

1.  Wählen Sie im Azure-Portal im Menü oder auf der Startseite die Option **Alle Ressourcen** aus.
2.  Geben Sie im Filterfeld den Namen des Nachweisanbieters ein, und wählen Sie ihn aus.

### <a name="13-delete-attestation-provider"></a>1.3 Löschen des Nachweisanbieters

1.  Wählen Sie im Azure-Portal im Menü oder auf der Startseite die Option **Alle Ressourcen** aus.
2.  Geben Sie im Filterfeld den Namen des Nachweisanbieters ein.
3.  Aktivieren Sie das Kontrollkästchen, und klicken Sie auf **Löschen**.
4.  Geben Sie „Ja“ ein, und klicken Sie auf **Löschen**. [ODER]
1.  Wählen Sie im Azure-Portal im Menü oder auf der Startseite die Option **Alle Ressourcen** aus.
2.  Geben Sie im Filterfeld den Namen des Nachweisanbieters ein.
3.  Wählen Sie den Nachweisanbieter aus, und navigieren Sie zur Übersichtsseite.
4.  Klicken Sie im obersten Menü auf **Löschen** und dann auf **Ja**.


## <a name="2-attestation-policy-signers"></a>2. Nachweisrichtlinien-Signaturgeber

### <a name="21-view-policy-signer-certificates"></a>2.1 Anzeigen von Richtlinien-Signaturgeberzertifikaten

1.  Wählen Sie im Azure-Portal im Menü oder auf der Startseite die Option **Alle Ressourcen** aus.
2.  Geben Sie im Filterfeld den Namen des Nachweisanbieters ein.
3.  Wählen Sie den Nachweisanbieter aus, und navigieren Sie zur Übersichtsseite.
4.  Klicken Sie links im Ressourcenmenü oder im unteren Bereich auf **Richtlinien-Signaturgeberzertifikate**.
5.  Klicken Sie auf **Download policy signer certificates** (Richtlinien-Signaturgeberzertifikate herunterladen). (Für Nachweisanbieter, die ohne obligatorische Signierung von Richtlinien erstellt wurden, ist die Schaltfläche deaktiviert.)
6.  Die heruntergeladene Textdatei enthält alle Zertifikate im JWS-Format.
a.  Überprüfen Sie die Anzahl von Zertifikaten und die heruntergeladenen Zertifikate.

### <a name="22-add-policy-signer-certificate"></a>2.2 Hinzufügen eines Richtlinien-Signaturgeberzertifikats

1.  Wählen Sie im Azure-Portal im Menü oder auf der Startseite die Option **Alle Ressourcen** aus.
2.  Geben Sie im Filterfeld den Namen des Nachweisanbieters ein.
3.  Wählen Sie den Nachweisanbieter aus, und navigieren Sie zur Übersichtsseite.
4.  Klicken Sie links im Ressourcenmenü oder im unteren Bereich auf **Richtlinien-Signaturgeberzertifikate**.
5.  Klicken Sie im obersten Menü auf **Hinzufügen**. (Für Nachweisanbieter, die ohne obligatorische Signierung von Richtlinien erstellt wurden, ist die Schaltfläche deaktiviert.)
6.  Laden Sie die Datei für Richtlinien-Signaturgeberzertifikate hoch, und klicken Sie auf **Hinzufügen**. Hilfreiche Beispiele finden Sie [hier](./policy-signer-examples.md).

### <a name="23-delete-policy-signer-certificate"></a>2.3 Löschen des Richtlinien-Signaturgeberzertifikats

1.  Wählen Sie im Azure-Portal im Menü oder auf der Startseite die Option **Alle Ressourcen** aus.
2.  Geben Sie im Filterfeld den Namen des Nachweisanbieters ein.
3.  Wählen Sie den Nachweisanbieter aus, und navigieren Sie zur Übersichtsseite.
4.  Klicken Sie links im Ressourcenmenü oder im unteren Bereich auf **Richtlinien-Signaturgeberzertifikate**.
5.  Klicken Sie im obersten Menü auf **Löschen**. (Für Nachweisanbieter, die ohne obligatorische Signierung von Richtlinien erstellt wurden, ist die Schaltfläche deaktiviert.)
6.  Laden Sie die Datei für Richtlinien-Signaturgeberzertifikate hoch, und klicken Sie auf **Löschen**. Hilfreiche Beispiele finden Sie [hier](./policy-signer-examples.md). 

## <a name="3-attestation-policy"></a>3. Nachweisrichtlinie

### <a name="31-view-attestation-policy"></a>3.1 Anzeigen der Nachweisrichtlinie

1.  Wählen Sie im Azure-Portal im Menü oder auf der Startseite die Option **Alle Ressourcen** aus.
2.  Geben Sie im Filterfeld den Namen des Nachweisanbieters ein.
3.  Wählen Sie den Nachweisanbieter aus, und navigieren Sie zur Übersichtsseite.
4.  Klicken Sie links im Ressourcenmenü oder im unteren Bereich auf **Richtlinie**.
5.  Wählen Sie den gewünschten **Nachweistyp** aus, und zeigen Sie die **Aktuelle Richtlinie** an.

### <a name="32-configure-attestation-policy"></a>3.2 Konfigurieren der Nachweisrichtlinie

#### <a name="321-when-attestation-provider-is-created-without-policy-signing-requirement"></a>3.2.1 Erstellung des Nachweisanbieters ohne obligatorische Signierung von Richtlinien

##### <a name="upload-policy-in-jwt-format"></a>Laden Sie die Richtlinie im JWT-Format hoch.

1.  Wählen Sie im Azure-Portal im Menü oder auf der Startseite die Option **Alle Ressourcen** aus.
2.  Geben Sie im Filterfeld den Namen des Nachweisanbieters ein.
3.  Wählen Sie den Nachweisanbieter aus, und navigieren Sie zur Übersichtsseite.
4.  Klicken Sie links im Ressourcenmenü oder im unteren Bereich auf **Richtlinie**.
5.  Klicken Sie im obersten Menü auf **Konfigurieren**.
6.  Nachdem der Nachweisanbieter ohne obligatorische Signierung von Richtlinien erstellt wurde, kann der Benutzer eine Richtlinie im Format **JWT** oder **Text** hochladen.
7.  Wählen Sie unter **Richtlinienformat** die Option **JWT** aus.
8.  Laden Sie eine Richtliniendatei mit Richtlinieninhalten im Format **Unsigniertes/Signiertes JWT** hoch, und klicken Sie auf **Speichern**. Beispiele finden Sie [hier](./policy-examples.md).
    
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
8.  Laden Sie die Richtliniendatei mit den entsprechenden Inhalten im Format **Text** hoch, oder geben Sie die Richtlinieninhalte im Textbereich ein, und klicken Sie auf **Speichern**. Beispiele finden Sie [hier](./policy-examples.md).

    Bei der Option zum Hochladen von Dateien wird die Richtlinienvorschau im Textformat angezeigt und kann nicht bearbeitet werden.

8.  Klicken Sie auf **Aktualisieren**, um die konfigurierte Richtlinie anzuzeigen.

#### <a name="322-when-attestation-provider-is-created-with-policy-signing-requirement"></a>3.2.2 Erstellung des Nachweisanbieters mit obligatorischer Signierung von Richtlinien

##### <a name="upload-policy-in-jwt-format"></a>Laden Sie die Richtlinie im JWT-Format hoch.

1.  Wählen Sie im Azure-Portal im Menü oder auf der Startseite die Option **Alle Ressourcen** aus.
2.  Geben Sie im Filterfeld den Namen des Nachweisanbieters ein.
3.  Wählen Sie den Nachweisanbieter aus, und navigieren Sie zur Übersichtsseite.
4.  Klicken Sie links im Ressourcenmenü oder im unteren Bereich auf **Richtlinie**.
5.  Klicken Sie im obersten Menü auf **Konfigurieren**.
6.  Nachdem der Nachweisanbieter mit obligatorischer Signierung von Richtlinien erstellt wurde, kann der Benutzer eine Richtlinie nur im Format **Signiertes JWT** hochladen.
7.  Laden Sie die Richtliniendatei im Format **Signiertes JWT** hoch, und klicken Sie auf **Speichern**. Beispiele finden Sie [hier](./policy-examples.md).

    Bei der Option zum Hochladen von Dateien wird die Richtlinienvorschau im Textformat angezeigt und kann nicht bearbeitet werden.
    
8.  Klicken Sie auf **Aktualisieren**, um die konfigurierte Richtlinie anzuzeigen.

## <a name="next-steps"></a>Nächste Schritte

- [Erstellen und Signieren einer Nachweisrichtlinie](author-sign-policy.md)
- [Erbringen eines Nachweises für eine SGX-Enclave mithilfe von Codebeispielen](/samples/browse/?expanded=azure&terms=attestation)

