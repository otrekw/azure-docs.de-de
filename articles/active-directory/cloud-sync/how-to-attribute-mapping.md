---
title: Attributzuordnung bei der Azure AD Connect-Cloudsynchronisierung
description: In diesem Artikel wird beschrieben, wie Sie mit der Cloudsynchronisierungsfunktion von Azure AD Connect Attribute zuordnen können.
services: active-directory
author: billmath
manager: daveba
ms.service: active-directory
ms.workload: identity
ms.topic: how-to
ms.date: 01/21/2021
ms.subservice: hybrid
ms.author: billmath
ms.collection: M365-identity-device-management
ms.openlocfilehash: cdb043374cf6252da3929c8f0cda6c0a4be558b7
ms.sourcegitcommit: 7edadd4bf8f354abca0b253b3af98836212edd93
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/10/2021
ms.locfileid: "102555209"
---
# <a name="attribute-mapping-in-azure-ad-connect-cloud-sync"></a>Attributzuordnung bei der Azure AD Connect-Cloudsynchronisierung

Sie können die Cloudsynchronisierungsfunktion von Azure Active Directory (Azure AD) Connect verwenden, um Attribute zwischen lokalen Benutzer- oder Gruppenobjekten und den Objekten in Azure AD zuzuordnen. Diese Funktion wurde der Cloudsynchronisierungskonfiguration hinzugefügt.

Sie können die Standardattributzuordnungen an die Anforderungen Ihres Unternehmens anpassen (ändern, löschen oder erstellen). Eine Liste der Attribute, die synchronisiert werden, finden Sie unter [Mit Azure Active Directory synchronisierte Attribute](../hybrid/reference-connect-sync-attributes-synchronized.md?context=azure%2factive-directory%2fcloud-provisioning%2fcontext%2fcp-context/hybrid/reference-connect-sync-attributes-synchronized.md).

## <a name="understand-types-of-attribute-mapping"></a>Grundlegendes zu den Attributzuordnungstypen
Mit Attributzuordnungen steuern Sie, wie Attribute in Azure AD aufgefüllt werden. Azure AD unterstützt vier Zuordnungstypen:

- **Direkt**: Das Zielattribut wird mit dem Wert eines Attributs des verknüpften Objekts in Active Directory aufgefüllt.
- **Konstante**: Das Zielattribut wird mit einer bestimmten Zeichenfolge aufgefüllt, die Sie angeben.
- **Ausdruck**: Das Zielattribut wird abhängig vom Ergebnis eines skriptähnlichen Ausdrucks aufgefüllt. Weitere Informationen finden Sie unter [Schreiben von Ausdrücken für Attributzuordnungen in Azure Active Directory](reference-expressions.md).
- **Keine**: Das Zielattribut bleibt unverändert. Allerdings wird das Zielattribut, wenn es leer sein sollte, mit dem von Ihnen angegebenen Standardwert aufgefüllt.

Neben diesen grundlegenden Typen unterstützen benutzerdefinierte Attributzuordnungen das Konzept einer optionalen Zuordnung von *Standardwerten*. Mit der Standardwertzuordnung wird sichergestellt, dass ein Zielattribut mit einem Wert aufgefüllt wird, wenn weder Azure AD noch das Zielobjekt ein Wert aufweisen. Bei der üblichen Konfiguration bleibt dieses Feld leer.

## <a name="understand-properties-of-attribute-mapping"></a>Grundlegendes zu Eigenschaften der Attributzuordnung

Neben der Typeigenschaft unterstützen Attributzuordnungen auch die folgenden Attribute:

- **Quellattribut**: Das Benutzerattribut aus dem Quellsystem (beispielsweise Active Directory).
- **Zielattribut**: Das Benutzerattribut im Zielsystem (beispielsweise Azure Active Directory).
- **Standardwert bei Null (optional)** : Der Wert, der an das Zielsystem übermittelt wird, wenn das Quellattribut den Wert NULL hat. Dieser Wert wird nur beim Erstellen eines Benutzers bereitgestellt. Er wird nicht beim Aktualisieren eines vorhandenen Benutzers bereitgestellt.  
- **Diese Zuordnung anwenden**:
  - **Immer**: Diese Zuordnung wird sowohl bei der Benutzererstellung als auch bei Aktualisierungsaktionen angewendet.
  - **Nur während der Erstellung**: Diese Zuordnung wird nur bei Benutzererstellungsaktionen angewendet.

> [!NOTE]
> In diesem Artikel wird beschrieben, wie Sie Attribute im Azure-Portal zuordnen.  Weitere Informationen zur Verwendung von Microsoft Graph finden Sie unter [Transformationen](how-to-transformation.md).

## <a name="add-an-attribute-mapping"></a>Hinzufügen einer Attributzuordnung

Führen Sie die folgenden Schritte aus, um die neue Funktion zu verwenden:

1.  Wählen Sie im Azure-Portal die Option **Azure Active Directory** aus.
2.  Wählen Sie **Azure AD Connect** aus.
3.  Wählen Sie **Cloudsynchronisierung verwalten** aus.

    ![Screenshot: Link zum Verwalten der Cloudsynchronisierung](media/how-to-install/install-6.png)

4. Wählen Sie unter **Konfiguration** Ihre Konfiguration aus.
5. Wählen Sie **Zum Bearbeiten der Zuordnungen klicken** aus.  Über diesen Link wird der Bildschirm **Attributzuordnungen** geöffnet.

    ![Screenshot: Link zum Hinzufügen von Attributen](media/how-to-attribute-mapping/mapping-6.png)

6.  Klicken Sie auf **Attribut hinzufügen**.

    ![Screenshot: Schaltfläche zum Hinzufügen eines Attributs sowie Listen von Attributen und Zuordnungstypen](media/how-to-attribute-mapping/mapping-1.png)

7. Wählen Sie den Zuordnungstyp aus. In diesem Beispiel wird **Ausdruck** verwendet.
8. Geben Sie den Ausdruck im Feld ein. In diesem Beispiel wird `Replace([mail], "@contoso.com", , ,"", ,)` verwendet.
9. Geben Sie das Zielattribut ein. In diesem Beispiel wird **ExtensionAttribute15** verwendet.
10. Wählen Sie aus, wann diese Zuordnung angewendet werden soll, und wählen Sie dann **Anwenden** aus.

    ![Screenshot: Ausgefüllte Felder beim Erstellen einer Attributzuordnung](media/how-to-attribute-mapping/mapping-2a.png)

11. Wenn Sie zum Bildschirm **Attributzuordnung** zurückkehren, sollte die neue Attributzuordnung angezeigt werden.  
12. Wählen Sie **Schema speichern** aus.

    ![Screenshot: Schaltfläche „Schema speichern“](media/how-to-attribute-mapping/mapping-3.png)

## <a name="test-your-attribute-mapping"></a>Testen der Attributzuordnung

Zum Testen der Attributzuordnung können Sie die [bedarfsorientierte Bereitstellung](how-to-on-demand-provision.md) verwenden: 

1. Wählen Sie im Azure-Portal die Option **Azure Active Directory** aus.
2. Wählen Sie **Azure AD Connect** aus.
3. Wählen Sie **Bereitstellung verwalten** aus.
4. Wählen Sie unter **Konfiguration** Ihre Konfiguration aus.
5. Wählen Sie unter **Validieren** die Schaltfläche **Benutzer bereitstellen** aus. 
6. Geben Sie im Bildschirm **Bedarfsgesteuerte Bereitstellung** den Distinguished Name eines Benutzers oder einer Gruppe ein, und wählen Sie die Schaltfläche **Bereitstellen** aus. 

   Auf dem Bildschirm wird gezeigt, dass die Bereitstellung ausgeführt wird.

   ![Screenshot: Anzeige des Bereitstellungsfortschritts](media/how-to-attribute-mapping/mapping-4.png)

8. Nachdem die Bereitstellung abgeschlossen ist, wird für den erfolgreichen Vorgang ein Bildschirm mit vier grünen Häkchen angezeigt. 

   Wählen Sie unter **Aktion ausführen** die Option **Details anzeigen** aus. Auf der rechten Seite sollte angezeigt werden, dass das neue Attribut synchronisiert und der Ausdruck angewendet wurde.

   ![Screenshot: Bildschirm mit Erfolgs- und Exportdetails](media/how-to-attribute-mapping/mapping-5.png)

## <a name="next-steps"></a>Nächste Schritte

- [Worum handelt es sich bei der Azure AD Connect-Cloudsynchronisierung?](what-is-cloud-sync.md)
- [Schreiben von Ausdrücken für Attributzuordnungen](reference-expressions.md)
- [Mit Azure Active Directory synchronisierte Attribute](../hybrid/reference-connect-sync-attributes-synchronized.md?context=azure%2factive-directory%2fcloud-provisioning%2fcontext%2fcp-context/hybrid/reference-connect-sync-attributes-synchronized.md)
