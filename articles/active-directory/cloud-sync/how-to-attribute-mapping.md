---
title: Attributzuordnung bei der Azure AD Connect-Cloudsynchronisierung
description: In diesem Artikel wird beschrieben, wie Sie mit der Cloudsynchronisierungsfunktion von Azure AD Connect Attribute zuordnen können.
services: active-directory
author: billmath
manager: mtillman
ms.service: active-directory
ms.workload: identity
ms.topic: how-to
ms.date: 04/30/2021
ms.subservice: hybrid
ms.author: billmath
ms.collection: M365-identity-device-management
ms.openlocfilehash: e861a1adc2dead2ee7c4397b9fb09aae202aaf94
ms.sourcegitcommit: c072eefdba1fc1f582005cdd549218863d1e149e
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/10/2021
ms.locfileid: "111963692"
---
# <a name="attribute-mapping-in-azure-ad-connect-cloud-sync"></a>Attributzuordnung bei der Azure AD Connect-Cloudsynchronisierung

Sie können die Cloudsynchronisierungsfunktion von Azure Active Directory (Azure AD) Connect verwenden, um Attribute zwischen lokalen Benutzer- oder Gruppenobjekten und den Objekten in Azure AD zuzuordnen. Diese Funktion wurde der Cloudsynchronisierungskonfiguration hinzugefügt.

Sie können die Standardattributzuordnungen an die Anforderungen Ihres Unternehmens anpassen (ändern, löschen oder erstellen). Eine Liste der Attribute, die synchronisiert werden, finden Sie unter [Mit Azure Active Directory synchronisierte Attribute](../hybrid/reference-connect-sync-attributes-synchronized.md?context=azure%2factive-directory%2fcloud-provisioning%2fcontext%2fcp-context/hybrid/reference-connect-sync-attributes-synchronized.md).

> [!NOTE]
> In diesem Artikel wird beschrieben, wie Sie Attribute im Azure-Portal zuordnen.  Weitere Informationen zur Verwendung von Microsoft Graph finden Sie unter [Transformationen](how-to-transformation.md).

## <a name="understand-types-of-attribute-mapping"></a>Grundlegendes zu den Attributzuordnungstypen
Mit Attributzuordnungen steuern Sie, wie Attribute in Azure AD aufgefüllt werden. Azure AD unterstützt vier Zuordnungstypen:

|Zuordnungstyp|BESCHREIBUNG|
|-----|-----|
|**Direkt**|Das Zielattribut wird mit dem Wert eines Attributs des verknüpften Objekts in Active Directory aufgefüllt.|
|**Konstante**|Das Zielattribut wird mit einer bestimmten Zeichenfolge aufgefüllt, die Sie angeben.|
|**Ausdruck**|Das Zielattribut wird abhängig vom Ergebnis eines skriptähnlichen Ausdrucks aufgefüllt. Weitere Informationen finden Sie unter [Ausdrucks-Generator](how-to-expression-builder.md) und [Schreiben von Ausdrücken für Attributzuordnungen in Azure Active Directory](reference-expressions.md).|
|**None**|Das Zielattribut bleibt unverändert. Allerdings wird das Zielattribut, wenn es leer sein sollte, mit dem von Ihnen angegebenen Standardwert aufgefüllt.|

Neben diesen grundlegenden Typen unterstützen benutzerdefinierte Attributzuordnungen das Konzept einer optionalen Zuordnung von *Standardwerten*. Mit der Standardwertzuordnung wird sichergestellt, dass ein Zielattribut mit einem Wert aufgefüllt wird, wenn weder Azure AD noch das Zielobjekt ein Wert aufweisen. Bei der üblichen Konfiguration bleibt dieses Feld leer.

## <a name="schema-updates-and-mappings"></a>Schemaupdates und Zuordnungen
Die Cloudsynchronisierung aktualisiert gelegentlich das Schema und die Liste der Standardattribute, die [synchronisiert](../hybrid/reference-connect-sync-attributes-synchronized.md?context=%2fazure%2factive-directory%2fcloud-provisioning%2fcontext%2fcp-context) werden.  Diese Standardattributzuordnungen sind für neue Installationen verfügbar, werden jedoch nicht automatisch vorhandenen Installationen hinzugefügt.  Um diese Zuordnungen hinzuzufügen, können Sie die folgenden Schritte ausführen.


  1. Klicken Sie auf „Attributzuordnung hinzufügen“.
  2. Wählen Sie die Zielattribut-Dropdownliste aus.
  3. Die neuen Attribute, die hier verfügbar sind, sollten angezeigt werden.

Im Folgenden sind die neuen Zuordnungen aufgelistet, die hinzugefügt wurden.

Hinzugefügtes Attribut | Zuordnungstyp | Mit Agent-Version hinzugefügt
| ----- | -----| -----|
|preferredDataLocation|Direkt|1.1.359.0|
|EmployeeNumber|Direkt|1.1.359.0|
|UserType|Direkt|1.1.359.0|

Weitere Informationen zum Zuordnen von UserType finden Sie unter [Zuordnen von UserType mit Cloudsynchronisierung.](how-to-map-usertype.md)

## <a name="understand-properties-of-attribute-mappings"></a>Grundlegendes zu Eigenschaften der Attributzuordnung

Neben der Typeigenschaft unterstützen Attributzuordnungen bestimmte Attribute.  Diese Attribute hängen vom Typ der ausgewählten Zuordnung ab.  In den folgenden Abschnitten werden die unterstützten Attributzuordnungen für jeden der einzelnen Typen beschrieben.

### <a name="direct-mapping-attributes"></a>Direktzuordnungsattribute
Im Folgenden werden die von einer direkten Zuordnung unterstützten Attribute beschrieben.

- **Quellattribut**: Das Benutzerattribut aus dem Quellsystem (beispielsweise Active Directory).
- **Zielattribut**: Das Benutzerattribut im Zielsystem (beispielsweise Azure Active Directory).
- **Standardwert bei Null (optional)** : Der Wert, der an das Zielsystem übermittelt wird, wenn das Quellattribut den Wert NULL hat. Dieser Wert wird nur beim Erstellen eines Benutzers bereitgestellt. Er wird nicht beim Aktualisieren eines vorhandenen Benutzers bereitgestellt.  
- **Diese Zuordnung anwenden**:
  - **Immer**: Diese Zuordnung wird sowohl bei der Benutzererstellung als auch bei Aktualisierungsaktionen angewendet.
  - **Nur während der Erstellung**: Diese Zuordnung wird nur bei Benutzererstellungsaktionen angewendet.

 ![Screenshot für Direktzuordnung](media/how-to-attribute-mapping/mapping-7.png)

### <a name="constant-mapping-attributes"></a>Konstantenzuordnungsattribute
Im Folgenden werden die von einer Konstantenzuordnung unterstützten Attribute beschrieben.

- **Konstanter Wert**: Der Wert, den Sie auf das Zielattribut anwenden möchten.
- **Zielattribut**: Das Benutzerattribut im Zielsystem (beispielsweise Azure Active Directory).
- **Diese Zuordnung anwenden**:
  - **Immer**: Diese Zuordnung wird sowohl bei der Benutzererstellung als auch bei Aktualisierungsaktionen angewendet.
  - **Nur während der Erstellung**: Diese Zuordnung wird nur bei Benutzererstellungsaktionen angewendet.

 ![Screenshot für Konstantenzuordnung](media/how-to-attribute-mapping/mapping-9.png)

### <a name="expression-mapping-attributes"></a>Ausdruckszuordnungsattribute
Im Folgenden werden die von einer Ausdruckszuordnung unterstützten Attribute beschrieben:

- **Ausdruck**: Dies ist der Ausdruck, der auf das Zielattribut angewendet werden soll.  Weitere Informationen finden Sie unter [Ausdrucks-Generator](how-to-expression-builder.md) und [Schreiben von Ausdrücken für Attributzuordnungen in Azure Active Directory](reference-expressions.md).
-  **Standardwert bei Null (optional)** : Der Wert, der an das Zielsystem übermittelt wird, wenn das Quellattribut den Wert NULL hat. Dieser Wert wird nur beim Erstellen eines Benutzers bereitgestellt. Er wird nicht beim Aktualisieren eines vorhandenen Benutzers bereitgestellt. 
- **Zielattribut**: Das Benutzerattribut im Zielsystem (beispielsweise Azure Active Directory).
 
- **Diese Zuordnung anwenden**:
  - **Immer**: Diese Zuordnung wird sowohl bei der Benutzererstellung als auch bei Aktualisierungsaktionen angewendet.
  - **Nur während der Erstellung**: Diese Zuordnung wird nur bei Benutzererstellungsaktionen angewendet.

 ![Screenshot für Ausdruckszuordnung](media/how-to-attribute-mapping/mapping-10.png)

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

7. Wählen Sie den Zuordnungstyp aus. Folgende Möglichkeiten stehen zur Auswahl:
     - **Direkt**: Das Zielattribut wird mit dem Wert eines Attributs des verknüpften Objekts in Active Directory aufgefüllt.
     - **Konstante**: Das Zielattribut wird mit einer bestimmten Zeichenfolge aufgefüllt, die Sie angeben.
     - **Ausdruck**: Das Zielattribut wird abhängig vom Ergebnis eines skriptähnlichen Ausdrucks aufgefüllt. 
     - **Keine**: Das Zielattribut bleibt unverändert. 
 
     Weitere Informationen finden Sie weiter oben unter [Grundlegendes zu den Attributzuordnungstypen](#understand-types-of-attribute-mapping).
8. Je nachdem, was Sie im vorherigen Schritt ausgewählt haben, stehen verschiedene Optionen zum Ausfüllen zur Verfügung.  Informationen zu diesen Attributen finden Sie weiter oben im Abschnitt [Grundlegendes zu Eigenschaften der Attributzuordnung](#understand-properties-of-attribute-mappings).
9. Wählen Sie aus, wann diese Zuordnung angewendet werden soll, und wählen Sie dann **Anwenden** aus.
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
- [Verwenden des Ausdrucks-Generators mit Cloudsynchronisierung](how-to-expression-builder.md)
- [Mit Azure Active Directory synchronisierte Attribute](../hybrid/reference-connect-sync-attributes-synchronized.md?context=azure%2factive-directory%2fcloud-provisioning%2fcontext%2fcp-context/hybrid/reference-connect-sync-attributes-synchronized.md)