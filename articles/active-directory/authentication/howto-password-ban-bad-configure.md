---
title: Verbannen unsicherer Kennwörter in Azure AD – Azure Active Directory
description: Verbannen Sie unsichere Kennwörter aus Ihrer Umgebung – mithilfe von dynamisch gesperrten Kennwörtern in Azure AD
services: active-directory
ms.service: active-directory
ms.subservice: authentication
ms.topic: conceptual
ms.date: 07/10/2019
ms.author: iainfou
author: iainfoulds
manager: daveba
ms.reviewer: rogoya
ms.collection: M365-identity-device-management
ms.openlocfilehash: eb47b9df51803c76662b5fb4ca1fe23740e7af9a
ms.sourcegitcommit: 276c1c79b814ecc9d6c1997d92a93d07aed06b84
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/16/2020
ms.locfileid: "76155055"
---
# <a name="configuring-the-custom-banned-password-list"></a>Konfigurieren der Liste benutzerdefinierter gesperrter Kennwörter

Viele Organisationen stellen fest, dass ihre Benutzer für ihre Kennwörter allgemeine Wörter verwenden, wie z.B. ihre Schule, ein Sportteam oder eine berühmte Person, sodass die Kennwörter leicht zu erraten sind. Mit der benutzerdefinierten Liste gesperrter Kennwörter von Microsoft können Organisationen Zeichenfolgen festlegen, die zusätzlich zur globalen Liste gesperrter Kennwörter ausgewertet und blockiert werden, wenn Benutzer oder Administratoren Kennwörter ändern oder zurücksetzen.

## <a name="add-to-the-custom-list"></a>Hinzufügen zur benutzerdefinierten Liste

Zum Konfigurieren der benutzerdefinierten Liste gesperrter Kennwörter ist eine Azure Active Directory Premium P1- oder P2-Lizenz erforderlich. Weitere Informationen zur Azure Active Directory-Lizenzierung finden Sie in auf der Seite [Azure Active Directory – Preise](https://azure.microsoft.com/pricing/details/active-directory/).

1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com) an, und navigieren Sie zu **Azure Active Directory** > **Sicherheit** > **Authentifizierungsmethoden** > **Kennwortschutz**.
1. Legen Sie die Option **Benutzerdefinierte Liste erzwingen** auf **Ja** fest.
1. Fügen Sie der **benutzerdefinierten Liste gesperrter Kennwörter** Zeichenfolgen hinzu. Geben Sie eine Zeichenfolge pro Zeile ein.
   * Die benutzerdefinierte Liste gesperrter Kennwörter kann bis zu 1.000 Ausdrücke umfassen.
   * Bei der benutzerdefinierten Liste gesperrter Kennwörter wird die Groß- und Kleinschreibung nicht beachtet.
   * Die benutzerdefinierte Liste gesperrter Kennwörter berücksichtigt die Ersetzung häufiger Zeichen.
      * Beispiel: „o“ und „0“ oder „a“ und „\@“
   * Die Zeichenfolgen müssen mindestens vier und höchstens 16 Zeichen lang sein.
1. Wenn Sie alle Zeichenfolgen hinzugefügt haben, klicken Sie auf **Speichern**.

> [!NOTE]
> Es kann mehrere Stunden dauern, bis Updates der benutzerdefinierten Liste gesperrter Kennwörter angewendet werden.

> [!NOTE]
> Die benutzerdefinierte Liste gesperrter Kennwörter ist auf maximal 1.000 Ausdrücke beschränkt. Sie dient nicht zum Blockieren von äußerst umfangreichen Kennwortlisten. Um die Vorteile der benutzerdefinierten Liste gesperrter Kennwörter umfassend nutzen zu können, empfiehlt Microsoft, sich zunächst mit dem Konzept und der Verwendung der Liste (siehe [Benutzerdefinierte Liste gesperrter Kennwörter](concept-password-ban-bad.md#custom-banned-password-list)) und dem Kennwortauswertungsalgorithmus (siehe [Auswerten von Kennwörtern](concept-password-ban-bad.md#how-are-passwords-evaluated)) vertraut zu machen.

![Ändern der benutzerdefinierten Liste gesperrter Kennwörter unter „Authentifizierungsmethoden“ im Azure-Portal](./media/howto-password-ban-bad/authentication-methods-password-protection.png)

## <a name="how-it-works"></a>Funktionsweise

Jedes Mal, wenn ein Benutzer oder Administrator ein Azure AD-Kennwort zurücksetzt oder ändert, wird es mit den benutzerdefinierten Listen gesperrter Kennwörter abgeglichen, um zu bestätigen, dass es sich auf keiner Liste befindet. Diese Überprüfung wird bei allen mithilfe von Azure AD festgelegten oder geänderten Kennwörter durchgeführt.

## <a name="what-do-users-see"></a>Anzeige für Benutzer

Wenn ein Benutzer versucht, ein Kennwort auf einen Wert zurückzusetzen, der gesperrt würde, wird eine der folgenden Fehlermeldungen angezeigt:

* Ihr Kennwort ist aufgrund eines enthaltenen Worts, Ausdrucks oder Musters leider leicht zu erraten. Wiederholen Sie den Vorgang mit einem anderen Kennwort.
* Unfortunately, you can't use that password because it contains words or characters that have been blocked by your administrator. (Sie können dieses Kennwort nicht verwenden, da es Wörter oder Zeichen enthält, die vom Administrator gesperrt wurden). Wiederholen Sie den Vorgang mit einem anderen Kennwort.

## <a name="next-steps"></a>Nächste Schritte

[Konzeptionelle Übersicht über Listen gesperrter Kennwörter](concept-password-ban-bad.md)

[Konzeptionelle Übersicht über den Azure AD-Kennwortschutz](concept-password-ban-bad-on-premises.md)

[Aktivieren der lokalen Integration der Listen gesperrter Kennwörter](howto-password-ban-bad-on-premises.md)
