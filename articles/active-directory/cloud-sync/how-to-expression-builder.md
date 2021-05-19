---
title: Verwenden des Ausdrucks-Generators mit Azure AD Connect Cloudsynchronisierung
description: Dieser Artikel beschreibt die Verwendung des Ausdrucks-Generators mit Cloudsynchronisierung.
services: active-directory
author: billmath
manager: mtillman
ms.service: active-directory
ms.workload: identity
ms.topic: how-to
ms.date: 04/19/2021
ms.subservice: hybrid
ms.author: billmath
ms.collection: M365-identity-device-management
ms.openlocfilehash: e169c2cbabf9e9e37bb93b1e3ee26080105d501e
ms.sourcegitcommit: 02d443532c4d2e9e449025908a05fb9c84eba039
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/06/2021
ms.locfileid: "108776646"
---
# <a name="expression-builder-with-cloud-sync"></a>Ausdrucks-Generator mit Cloudsynchronisierung
Der Ausdrucks-Generator ist ein neues Blatt in Azure unter Cloudsynchronisierung.  Damit können Sie komplexe Ausdrücke erstellen und diese Ausdrücke testen, bevor Sie diese auf Ihre Cloudsynchronisierungsumgebung anwenden.

## <a name="use-the-expression-builder"></a>Verwenden des Ausdrucks-Generators
Um auf den Ausdrucks-Generator zuzugreifen, folgen Sie diesen Schritten.

 1. Wählen Sie im Azure-Portal die Option **Azure Active Directory** aus.
 2. Wählen Sie **Azure AD Connect** aus.
 3. Wählen Sie **Cloudsynchronisierung verwalten** aus.
 4. Wählen Sie unter **Konfiguration** Ihre Konfiguration aus.
 5. Wählen Sie unter **Attribute verwalten** die Option **Zum Bearbeiten von Zuordnungen klicken**.
 6. Klicken Sie auf dem Blatt **Attributzuordnungen bearbeiten** auf **Attributzuordnung hinzufügen**.
 7. Wählen Sie unter **Zuordnungstyp** die Option **Ausdruck** aus.
 8. Wählen Sie **Ausdrucks-Generator ausprobieren (Vorschau)** aus.
 ![Ausdrucks-Generator verwenden](media/how-to-expression-builder/expression-1.png)

## <a name="build-an-expression"></a>Generieren eines Ausdrucks
In diesem Abschnitt können Sie über das Dropdownmenüs aus einer Liste der unterstützten Funktionen auswählen.  Anschließend werden je nach ausgewählter Funktion zusätzliche Felder zum Ausfüllen angezeigt.  Nach dem Auswählen von **Ausdruck anwenden** wird die Syntax im Feld **Ausdruckseingabe** angezeigt.

Wenn Sie beispielsweise in der Dropdownliste **Ersetzen** auswählen, werden zusätzliche Felder bereitgestellt.  Die Syntax für die Funktion wird im hellblauen Feld angezeigt.  Die angezeigten Felder entsprechen der Syntax der ausgewählten Funktion.  „Ersetzen“ funktioniert je nach angegebenen Parametern unterschiedlich.  Für unser Beispiel verwenden wir Folgendes:

- Wenn alterWert und Ersatzwert angegeben werden:
    - Ersetzt alle Vorkommen von "AlterWert" in der Quelle durch "Ersatzwert".

Weitere Informationen finden Sie unter [Ersetzen](reference-expressions.md#replace).

Zunächst müssen wir das Attribut auswählen, welches die Quelle für die Funktion „Ersetzen“ sein soll. In unserem Beispiel wählen wir das Attribut **mail** aus. 

Als Nächstes füllen wir den Wert for „oldValue“ aus.  Dieser „oldValue“ ist **@fabrikam.com** .  Schließlich geben wir in das Feld „replacementValue“ folgenden Wert ein: **@contoso.com** .

Unser Ausdruck ersetzt also im Grunde genommen das Mailattribut für Benutzerobjekte, die den Wert @fabrikam.com haben, durch den Wert @contoso.com.  Wenn Sie auf die Schaltfläche **Ausdruck hinzufügen** klicken, wird die Syntax in der **Ausdruckseingabe** angezeigt.


>[!NOTE]
>Achten Sie darauf, die Werte in die Felder zu platzieren, die „oldValue“ und „replacementValue“ entsprechen – basierend auf der Syntax, die auftrat, als Sie „Ersetzen“ ausgewählt haben.

Weitere Informationen zu unterstützten Ausdrücken finden Sie unter [Schreiben von Ausdrücken für Attributzuordnungen in Azure Active Directory](reference-expressions.md).

### <a name="information-on-expression-builder-input-boxes"></a>Informationen zu Eingabefeldern des Ausdrucks-Generators
Je nach ausgewählter Funktion akzeptieren die vom Ausdrucks-Generator bereitgestellten Felder mehrere Werte.  Beispielsweise akzeptiert die JOIN-Funktion Zeichenfolgen oder den Wert, der einem angegebenen Attribut zugeordnet ist.  Beispielsweise können wir den Wert im Attributwert von [givenName] verwenden und diesen mit dem Zeichenfolgenwert „@contoso.com“ verbinden, um eine E-Mail-Adresse zu erstellen.

  ![Eingabefeldwerte](media/how-to-expression-builder/expression-8.png)

Weitere Informationen zu akzeptablen Werten und zum Schreiben von Ausdrücken finden Sie unter [Schreiben von Ausdrücken für Attributzuordnungen in Azure Active Directory](reference-expressions.md).

## <a name="test-an-expression"></a>Testen eines Ausdrucks
In diesem Abschnitt können Sie Ihre Ausdrücke testen.  Wählen Sie im Dropdownmenü das Attribut **mail** aus.  Füllen Sie den Wert mit **@fabrikam.com** aus und klicken Sie nun auf **Ausdruck testen**.  

Der Wert von **@contoso.com** erscheint im Feld **Ausdrucksausgabe anzeigen**.

 ![Testen Ihres Ausdrucks](media/how-to-expression-builder/expression-4.png)

## <a name="deploy-the-expression"></a>Bereitstellen des Ausdrucks
Wenn Sie mit dem Ausdruck zufrieden sind, klicken Sie einfach auf die Schaltfläche **Ausdruck anwenden**.
![Hinzufügen Ihres Ausdrucks](media/how-to-expression-builder/expression-5.png)

Dadurch wird der Ausdruck zur Agentkonfiguration hinzugefügt.
![Agent-Konfiguration](media/how-to-expression-builder/expression-6.png)

## <a name="setting-a-null-value-on-an-expression"></a>Festlegen eines NULL-Werts für einen Ausdruck
So legen Sie einen Attributwert auf NULL fest.  Sie können einen Ausdruck mit dem Wert von `""` verwenden.  Dadurch wird der NULL-Wert an das Zielattribut fließen.

![NULL-Wert](media/how-to-expression-builder/expression-7.png)



## <a name="next-steps"></a>Nächste Schritte 

- [Schreiben von Ausdrücken für Attributzuordnungen in Azure Active Directory](reference-expressions.md)
- [Konfiguration der Cloudsynchronisierung](how-to-configure.md)
