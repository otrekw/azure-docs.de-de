---
title: Hinzufügen und Verwalten von Benutzern für den kommerziellen Marketplace – Azure Marketplace
description: Erfahren Sie, wie Sie Benutzer im Programm „Kommerzieller Marketplace“ für ein Konto im kommerziellen Microsoft-Marketplace in Partner Center verwalten.
ms.service: marketplace
ms.subservice: partnercenter-marketplace-publisher
ms.topic: how-to
author: varsha-sarah
ms.author: vavargh
ms.custom: contperf-fy21q2
ms.date: 04/07/2021
ms.openlocfilehash: 108beee178cbf65bb4ba4f29479b335ab8d01a1e
ms.sourcegitcommit: 4a54c268400b4158b78bb1d37235b79409cb5816
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/28/2021
ms.locfileid: "108141593"
---
# <a name="add-and-manage-users-for-the-commercial-marketplace"></a>Hinzufügen und Verwalten von Benutzern für den kommerziellen Marketplace

**Geeignete Rollen**

- Besitzer
- Manager

In Partner Center können Sie (unter **Kontoeinstellungen**) im Abschnitt **Benutzer** Azure AD verwenden, um die Benutzer, Gruppen und Azure AD-Anwendungen zu verwalten, die über Zugriff auf Ihr Partner Center-Konto verfügen. Ihr Konto muss über Berechtigungen auf Managerebene für das [Geschäftskonto (Azure AD-Mandant)](company-work-accounts.md) verfügen, in dem Sie Benutzer hinzufügen oder bearbeiten möchten. Zum Verwalten von Benutzern in einem anderen Geschäftskonto/Mandanten müssen Sie sich abmelden und dann als Benutzer mit **Manager**-Berechtigungen für das betreffende Geschäftskonto/den betreffenden Mandanten wieder anmelden.

Nachdem Sie sich bei Ihrem Geschäftskonto (Azure AD-Mandanten) angemeldet haben, können Sie Benutzer hinzufügen und verwalten.

## <a name="add-existing-users"></a>Hinzufügen vorhandener Benutzer

So fügen Sie Ihrem Partner Center-Konto Benutzer hinzu, die bereits im [Geschäftskonto (Azure AD-Mandant)](company-work-accounts.md) Ihres Unternehmens vorhanden sind

1. Wechseln Sie zu **Benutzer** (unter **Kontoeinstellungen**), und wählen Sie **Benutzer hinzufügen** aus.
1. Wählen Sie einen oder mehrere Benutzer aus der angezeigten Liste aus. Über das Suchfeld können Sie bestimmte Benutzer suchen. \* Wenn Sie mehrere Benutzer auswählen, die Ihrem Partner Center-Konto hinzugefügt werden sollen, müssen Sie diesen dieselbe Rolle oder denselben Satz benutzerdefinierter Berechtigungen zuweisen. Um mehrere Benutzer mit unterschiedlichen Rollen/Berechtigungen hinzuzufügen, wiederholen Sie diese Schritte für jede Rolle bzw. für jeden Satz benutzerdefinierter Berechtigungen.
1. Wenn Sie die Auswahl von Benutzern abgeschlossen haben, wählen Sie **Ausgewählte hinzufügen** aus.
1. Geben Sie im Abschnitt **Rollen** die Rolle(n) oder die benutzerdefinierten Berechtigungen für den oder die ausgewählten Benutzer an.
1. Wählen Sie **Speichern** aus.

## <a name="create-new-users"></a>Erstellen neuer Benutzer

Um neue Benutzerkonten zu erstellen, benötigen Sie ein Konto mit den Berechtigungen [Globaler Administrator](../active-directory/roles/permissions-reference.md).

1. Wechseln Sie zu **Benutzer** (unter **Kontoeinstellungen**), und wählen Sie **Benutzer hinzufügen** und dann **Neue Benutzer erstellen** aus.
1. Geben Sie einen Vornamen, einen Nachnamen und einen Benutzernamen für jeden neuen Benutzer ein.
1. Wenn Sie dem neuen Benutzer ein globales Administratorkonto im Verzeichnis Ihrer Organisation zuweisen möchten, aktivieren Sie das Kontrollkästchen **Diesen Benutzer als globalen Administrator in Azure AD festlegen, mit vollständiger Kontrolle über alle Verzeichnisressourcen**. Dadurch erhält der Benutzer vollständigen Zugriff auf alle Administratorfunktionen in Ihrem Unternehmens-Azure AD. Dieser Benutzer kann dann Benutzer im Geschäftskonto (Azure AD-Mandant) Ihres Unternehmens hinzufügen und verwalten, allerdings nicht in Partner Center, es sei denn, Sie gewähren dem Konto die entsprechende Rolle/Berechtigungen.
1. Wenn Sie das Kontrollkästchen aktiviert haben, um **diesen Benutzer als globalen Administrator festzulegen**, müssen Sie eine *E-Mail-Adresse zur Kennwortwiederherstellung* für den Benutzer angeben, damit dieser bei Bedarf sein Kennwort wiederherstellen kann.
1. Wählen Sie im Abschnitt **Gruppenmitgliedschaft** alle Gruppen aus, zu denen der neue Benutzer gehören soll.
1. Geben Sie im Abschnitt **Rollen** die Rolle(n) oder die benutzerdefinierten Berechtigungen für den Benutzer an.
1. Wählen Sie **Speichern** aus.

Wenn Sie einen neuen Benutzer in Partner Center erstellen, wird für diesen Benutzer auch ein Konto im Geschäftskonto (Azure AD-Mandant) erstellt, in dem Sie angemeldet sind. Wenn Sie Änderungen am Namen eines Benutzers in Partner Center vornehmen, werden diese Änderungen im Geschäftskonto (Azure AD-Mandant) Ihrer Organisation ebenfalls vorgenommen.

## <a name="invite-new-users-by-email"></a>Einladen neuer Benutzer per E-Mail

Um Benutzer per E-Mail einzuladen, die derzeit nicht zum Geschäftskonto (Azure AD-Mandant) Ihres Unternehmens gehören, benötigen Sie ein Konto mit Berechtigungen als [globaler Administrator](../active-directory/roles/permissions-reference.md).

1. Wechseln Sie zu **Benutzer** (unter **Kontoeinstellungen**), und wählen Sie dann **Benutzer hinzufügen** und anschließend **Benutzer per E-Mail einladen** aus.
1. Geben Sie bis zu 10 E-Mail-Adressen getrennt durch Kommas oder Semikolons ein.
1. Geben Sie im Abschnitt **Rollen** die Rolle(n) oder die benutzerdefinierten Berechtigungen für den Benutzer an.
1. Wählen Sie **Speichern** aus.

Die von Ihnen eingeladenen Benutzer erhalten eine E-Mail-Einladung zur Teilnahme an Ihrem Partner Center-Konto. In Ihrem Geschäftskonto (Azure AD-Mandant) wird ein neues Gastbenutzerkonto erstellt. Jeder Benutzer muss zunächst die Einladung akzeptieren, bevor er auf Ihr Konto zugreifen kann.

Wenn Sie eine Einladung erneut senden möchten, wechseln Sie auf die Seite *Benutzer*, suchen Sie die Einladung in der Benutzerliste, und wählen Sie die betreffende E-Mail-Adresse aus (oder den Text *Die Einladung steht aus*). Wählen Sie dann am unteren Rand der Seite **Einladung erneut senden** aus.

Wenn Ihre Organisation [Verzeichnisintegration](/previous-versions/azure/azure-services/jj573653(v=azure.100)) verwendet, um den lokalen Verzeichnisdienst mit Ihrem Azure AD zu synchronisieren, können Sie keine neuen Benutzer, Gruppen oder Azure AD-Anwendungen in Partner Center erstellen. Diese müssen Sie (oder ein anderer Administrator in Ihrem lokalen Verzeichnis) direkt im lokalen Verzeichnis erstellen, bevor Sie sie im Partner Center anzeigen und hinzufügen können.

## <a name="remove-a-user"></a>Benutzer entfernen

Um einen Benutzer aus Ihrem Geschäftskonto (Azure AD-Mandant) zu entfernen, wechseln Sie zu **Benutzer** (unter **Kontoeinstellungen**), wählen Sie über das Kontrollkästchen in der Spalte ganz rechts den Benutzer aus, den Sie entfernen möchten, und wählen Sie anschließend aus den verfügbaren Aktionen **Entfernen** aus. In einem Popupfenster können Sie bestätigen, dass Sie den bzw. die ausgewählten Benutzer entfernen möchten.

## <a name="change-a-user-password"></a>Ändern eines Benutzerkennworts

Ein Benutzer kann sein Kennwort bei Bedarf selbst ändern, sofern Sie beim Erstellen des Benutzerkontos eine *E-Mail-Adresse zur Kenntwortwiederherstellung* angegeben haben. Sie können das Kennwort eines Benutzers auch aktualisieren, indem Sie die folgenden Schritte ausführen. Um das Kennwort eines Benutzers im Geschäftskonto (Azure AD-Mandant) Ihres Unternehmens zu ändern, müssen Sie bei einem Konto mit den Berechtigungen als [globaler Administrator](../active-directory/roles/permissions-reference.md) angemeldet sein. Hierdurch werden das Kennwort des Benutzers in Ihrem Azure AD-Mandanten und das Kennwort geändert, das er zum Zugriff auf Partner Center verwendet.

1. Wählen Sie auf der Seite **Benutzer** (unter **Kontoeinstellungen**) den Namen des Benutzerkontos aus, das Sie bearbeiten möchten.
1. Wählen Sie unten auf der Seite die Schaltfläche **Kennwort zurücksetzen** aus.
1. Auf einer Bestätigungsseite werden die Anmeldeinformationen des Benutzers einschließlich eines temporären Kennworts angezeigt. Da Sie nach Verlassen dieser Seite nicht mehr auf das temporäre Kennwort zugreifen können, sollten Sie diese Informationen unbedingt drucken oder kopieren und dem Benutzer zur Verfügung stellen.