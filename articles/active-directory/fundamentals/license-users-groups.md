---
title: Zuweisen oder Entfernen von Lizenzen – Azure Active Directory | Microsoft-Dokumentation
description: Hier finden Sie Anweisungen zum Zuweisen oder Entfernen von Azure Active Directory-Lizenzen für Ihre Benutzer oder Gruppen.
services: active-directory
author: ajburnle
manager: daveba
ms.assetid: f8b932bc-8b4f-42b5-a2d3-f2c076234a78
ms.service: active-directory
ms.subservice: fundamentals
ms.workload: identity
ms.topic: how-to
ms.date: 12/14/2020
ms.author: ajburnle
ms.reviewer: jeffsta
ms.custom: it-pro, seodec18
ms.collection: M365-identity-device-management
ms.openlocfilehash: b6900647acf7182529f34c8cc065dbb039de38be
ms.sourcegitcommit: f28ebb95ae9aaaff3f87d8388a09b41e0b3445b5
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/29/2021
ms.locfileid: "97504405"
---
# <a name="assign-or-remove-licenses-in-the-azure-active-directory-portal"></a>Zuweisen oder Entfernen von Lizenzen im Azure Active Directory-Portal

Viele Dienste von Azure Active Directory (Azure AD) erfordern, dass Sie Ihre einzelnen Benutzer oder Gruppen (und die zugehörigen Mitglieder) für den jeweiligen Dienst lizenzieren. Nur Benutzer mit aktiven Lizenzen können auf die lizenzierten Azure AD-Dienste zugreifen und diese verwenden, auf die das zutrifft. Lizenzen werden pro Mandant angewendet und nicht an andere Mandanten übertragen. 

## <a name="available-license-plans"></a>Verfügbare Lizenzpläne

Für den Azure AD-Dienst sind mehrere Lizenzpläne verfügbar, einschließlich der folgenden:

- Azure AD Free

- Azure AD Premium P1

- Azure AD Premium P2

Unter [Welche Lizenz benötige ich?](https://azure.microsoft.com/pricing/details/active-directory/) finden Sie spezifische Informationen zu jedem Lizenzplan sowie die zugehörigen Lizenzdetails. Informationen zum Registrieren für Azure AD Premium-Lizenzpläne finden Sie [hier](./active-directory-get-started-premium.md).

Nicht alle Microsoft-Dienste sind an allen Standorten verfügbar. Bevor einer Gruppe eine Lizenz zugewiesen werden kann, müssen Sie den **Nutzungsspeicherort** für alle Mitglieder angeben. Sie können diesen Wert im Bereich **Azure Active Directory &gt; Benutzer &gt; Profil &gt; Einstellungen** in Azure AD festlegen. Jeder Benutzer, dessen Verwendungsstandort nicht angegeben ist, erbt den Standort der Azure AD-Organisation.

## <a name="view-license-plans-and-plan-details"></a>Anzeigen von Lizenzplänen und Plandetails

Sie können Ihre verfügbaren Dienstpläne einschließlich der einzelnen Lizenzen anzeigen, die bevorstehenden Ablaufdaten überprüfen und die Anzahl der verfügbaren Zuweisungen anzeigen.

### <a name="to-find-your-service-plan-and-plan-details"></a>So finden Sie Ihren Serviceplan und die Plandetails

1. Melden Sie sich mit dem Konto eines Lizenzadministrators Ihrer Azure AD-Organisation beim [Azure-Portal](https://portal.azure.com/) an.

1. Wählen Sie **Azure Active Directory** und dann **Lizenzen** aus.

    :::image type="content" source="media/license-users-groups/license-details-blade.png" alt-text="Seite „Lizenzen“ mit der Anzahl der erworbenen Dienste und zugewiesenen Lizenzen":::

1. Wählen Sie **Alle Produkte** aus, um die Seite „Alle Produkte“ und die Angaben **Gesamt**, **Zugewiesen**, **Verfügbar** und **Läuft bald ab** für Ihre Lizenzpläne anzuzeigen.

    :::image type="content" source="media/license-users-groups/license-products-blade-with-products.png" alt-text="Dienstseite mit Dienstlizenzplänen und den zugehörigen Lizenzinformationen":::

    > [!NOTE]
    > Die Zahlen sind wie folgt definiert: 
    > - Gesamt: Gesamtanzahl erworbener Lizenzen
    > - Zugewiesen: Anzahl von Lizenzen, die Benutzern zugewiesen sind
    > - Verfügbar: Anzahl von Lizenzen, die für die Zuweisung verfügbar sind (einschließlich demnächst ablaufender Lizenzen)
    > - Läuft bald ab: Anzahl bald ablaufender Lizenzen

1. Wählen Sie den Namen eines Plans aus, um die dafür lizenzierten Benutzer und Gruppen anzuzeigen.

## <a name="assign-licenses-to-users-or-groups"></a>Zuweisen von Lizenzen zu Benutzern oder Gruppen

Stellen Sie sicher, dass jeder, der einen lizenzierten Azure AD-Dienst nutzen muss, über die entsprechende Lizenz verfügt. Sie können den Benutzern oder einer gesamten Gruppe die Lizenzierungsrechte hinzufügen.

### <a name="to-assign-a-license-to-a-user"></a>So weisen Sie einem Benutzer eine Lizenz zu

1. Wählen Sie auf der Seite **Produkte** den Namen des Lizenzplans aus, den Sie dem Benutzer zuweisen möchten.

    ![Dienstseite mit hervorgehobenem Dienstlizenzplan](media/license-users-groups/license-products-blade-with-product-highlight.png)

1. Wählen Sie auf der Übersichtsseite des Lizenzplans die Option **Zuweisen** aus.

    ![Dienstseite mit hervorgehobener Option „Zuweisen“](media/license-users-groups/license-products-blade-with-assign-option-highlight.png)

1. Wählen Sie auf der Seite **Zuweisen****Benutzer und Gruppen**, und suchen und wählen Sie dann den Benutzer, dem Sie die Lizenz zuweisen.

    ![Seite „Lizenz hinzufügen“ mit den hervorgehobenen Optionen „Suchen“ und „Auswählen“](media/license-users-groups/assign-license-blade-with-highlight.png)

1. Wählen Sie **Zuordnungsoptionen**, stellen Sie sicher, dass die entsprechenden Lizenzoptionen aktiviert sind, und wählen Sie dann **OK**.

    ![Seite „Lizenzoptionen“ mit allen im Lizenzplan verfügbaren Optionen](media/license-users-groups/license-option-blade-assignments.png)

    Die Seite **Lizenz zuweisen** wird aktualisiert, um anzuzeigen, dass ein Benutzer ausgewählt ist und dass die Zuweisungen konfiguriert sind.

    > [!NOTE]
    > Nicht alle Microsoft-Dienste sind an allen Standorten verfügbar. Bevor einem Benutzer eine Lizenz zugewiesen werden kann, müssen Sie den **Nutzungsspeicherort** angeben. Sie können diesen Wert im Bereich **Azure Active Directory &gt; Benutzer &gt; Profil &gt; Einstellungen** in Azure AD festlegen. Jeder Benutzer, dessen Verwendungsstandort nicht angegeben ist, erbt den Standort der Azure AD-Organisation.

1. Wählen Sie **Zuweisen** aus.

    Der Benutzer wird der Liste der lizenzierten Benutzer hinzugefügt und hat Zugriff auf die enthaltenen Azure AD-Dienste.
    > [!NOTE]
    > Über die Seite **Lizenzen** des Benutzers können dem Benutzer Lizenzen auch direkt zugewiesen werden. Wenn einem Benutzer eine Lizenz über eine Gruppenmitgliedschaft zugewiesen ist und Sie dem Benutzer die gleiche Lizenz direkt zuweisen möchten, können Sie dies nur über die in Schritt 1 erwähnte Seite **Produkte** tun.

### <a name="to-assign-a-license-to-a-group"></a>So weisen Sie einer Gruppe eine Lizenz zu

1. Wählen Sie auf der Seite **Produkte** den Namen des Lizenzplans aus, den Sie dem Benutzer zuweisen möchten.

    ![Blatt „Produkte“ mit hervorgehobenem Produktlizenzplan](media/license-users-groups/license-products-blade-with-product-highlight.png)

1. Wählen Sie auf der Seite **Azure Active Directory Premium Plan 2** die Option **Zuweisen**.

    ![Seite „Produkte“ mit hervorgehobener Option „Zuweisen“](media/license-users-groups/license-products-blade-with-assign-option-highlight.png)

1. Wählen Sie auf der Seite **Zuweisen****Benutzer und Gruppen**, und suchen und wählen Sie dann die Gruppe, der Sie die Lizenz zuweisen.

    ![Seite „Lizenz zuweisen“ mit den hervorgehobenen Optionen „Suchen“ und „Auswählen“ – 2](media/license-users-groups/assign-group-license-blade-with-highlight.png)

1. Wählen Sie **Zuordnungsoptionen**, stellen Sie sicher, dass die entsprechenden Lizenzoptionen aktiviert sind, und wählen Sie dann **OK**.

    ![Seite „Lizenzoptionen“ mit allen im Lizenzplan verfügbaren Optionen – 2](media/license-users-groups/license-option-blade-group-assignments.png)

    Die Seite **Lizenz zuweisen** wird aktualisiert, um anzuzeigen, dass ein Benutzer ausgewählt ist und dass die Zuweisungen konfiguriert sind.

1. Wählen Sie **Zuweisen** aus.

    Die Gruppe wird der Liste der lizenzierten Gruppen hinzugefügt und alle Mitglieder haben Zugriff auf die enthaltenen Azure AD-Dienste.

## <a name="remove-a-license"></a>Entfernen einer Lizenz

Sie können eine Lizenz auf der Azure AD-Benutzerseite eines Benutzers und auf der Gruppenübersichtsseite für eine Gruppenzuweisung entfernen. Oder Sie beginnen auf der Azure AD-Seite **Lizenzen**, um die Benutzer und Gruppen für eine Lizenz anzuzeigen.

### <a name="to-remove-a-license-from-a-user"></a>So entfernen Sie eine Lizenz von einem Benutzer

1. Wählen Sie auf der Seite **Lizenzierte Benutzer** für den Dienstplan den Benutzer aus, der nicht mehr über die Lizenz verfügen soll. Beispielsweise _Alain Charon_.

1. Wählen Sie **Lizenz entfernen** aus.

    ![Seite „Lizenzierte Benutzer“ mit hervorgehobener Option „Lizenz entfernen“](media/license-users-groups/license-products-user-blade-with-remove-option-highlight.png)

> [!IMPORTANT]
> Lizenzen, die ein Benutzer von einer Gruppe erbt, können nicht direkt entfernt werden. Stattdessen müssen Sie den Benutzer aus der Gruppe entfernen, aus der er die Lizenz erbt.

### <a name="to-remove-a-license-from-a-group"></a>Entfernen einer Lizenz aus einer Gruppe

1. Wählen Sie auf der Seite **Lizenzierte Gruppen** für den Lizenzplan die Gruppe aus, die nicht mehr über die Lizenz verfügen soll.

1. Wählen Sie **Lizenz entfernen** aus.

    ![Seite „Lizenzierte Gruppen“ mit hervorgehobener Option „Lizenz entfernen“ – 2](media/license-users-groups/license-products-group-blade-with-remove-option-highlight.png)
    
    > [!NOTE]
    > Wenn ein lokales Benutzerkonto, das mit Azure AD synchronisiert wird, den Gültigkeitsbereich für die Synchronisierung überschreitet, oder wenn die Synchronisierung entfernt wird, wird der Benutzer in Azure AD vorläufig gelöscht. Wenn dies auftritt, werden Lizenzen, die dem Benutzer direkt oder über die gruppenbasierte Lizenzierung zugewiesen werden, als **angehalten** und nicht als **gelöscht** gekennzeichnet.

## <a name="next-steps"></a>Nächste Schritte

Nachdem Sie Ihre Lizenzen zugewiesen haben, können Sie die folgenden Prozesse ausführen:

- [Bestimmen und Beheben von Lizenzzuweisungsproblemen](../enterprise-users/licensing-groups-resolve-problems.md)

- [Hinzufügen von lizenzierten Benutzern zur Lizenzierung zu einer Gruppe](../enterprise-users/licensing-groups-migrate-users.md)

- [Szenarien, Einschränkungen und bekannte Probleme mit der Verwendung von Gruppen zum Verwalten der Lizenzierung in Azure Active Directory](../enterprise-users/licensing-group-advanced.md)

- [Hinzufügen oder Ändern von Profilinformationen](active-directory-users-profile-azure-portal.md)
