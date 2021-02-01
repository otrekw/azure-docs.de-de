---
title: Attribut-Editor für die Azure AD Connect-Cloudsynchronisierung
description: In diesem Artikel wird die Verwendung des Attribut-Editors beschrieben.
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
ms.openlocfilehash: c6d2adbd0fe0715cb22ac158d1804f53384f8b94
ms.sourcegitcommit: b39cf769ce8e2eb7ea74cfdac6759a17a048b331
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/22/2021
ms.locfileid: "98682104"
---
# <a name="azure-ad-connect-cloud-sync-attribute-mapping"></a>Attributzuordnung für die Azure AD Connect-Cloudsynchronisierung

Die Azure AD Connect-Cloudsynchronisierung umfasst eine neue Funktion, die Ihnen das einfache Zuordnen von Attributen zwischen Ihren lokalen Benutzer-/Gruppenobjekten und den Objekten in Azure AD ermöglicht.  Diese Funktion wurde der Cloudsynchronisierungskonfiguration hinzugefügt.

Sie können die Standardattributzuordnungen den Anforderungen Ihres Unternehmens entsprechend anpassen. Dies bedeutet, dass Sie vorhandene Attributzuordnungen ändern oder löschen und neue Attributzuordnungen erstellen können.  Eine Liste der Attribute, die synchronisiert werden, finden Sie unter [Attribute, die synchronisiert werden](../hybrid/reference-connect-sync-attributes-synchronized.md?context=azure%2factive-directory%2fcloud-provisioning%2fcontext%2fcp-context/hybrid/reference-connect-sync-attributes-synchronized.md).

## <a name="understanding-attribute-mapping-types"></a>Grundlegendes zu Attributzuordnungstypen
Mit Attributzuordnungen steuern Sie, wie die Attribute in Azure AD mit Daten aufgefüllt werden.
Vier verschiedene Zuordnungstypen werden unterstützt:

- **Direkt**: Das Zielattribut wird mit dem Wert eines Attributs des verknüpften Objekts in AD aufgefüllt.
- **Konstante**: Das Zielattribut wird mit einer bestimmten Zeichenfolge aufgefüllt, die Sie angegeben haben.
- **Ausdruck** : Das Zielattribut wird abhängig vom Ergebnis eines skriptähnlichen Ausdrucks mit Daten aufgefüllt.
  Weitere Informationen finden Sie unter [Schreiben von Ausdrücken für Attributzuordnungen](reference-expressions.md).
- **Kein** : Das Zielattribut bleibt unverändert. Wenn das Zielattribut allerdings leer ist, wird es mit dem von Ihnen angegebenen Standardwert aufgefüllt.

Zusätzlich zu diesen vier Basistypen unterstützen benutzerdefinierte Attributzuordnungen das Konzept einer optionalen Zuordnung von **Standardwerten**. Die Standardwertzuordnung stellt sicher, dass ein Zielattribut mit einem Wert aufgefüllt wird, wenn weder in Azure AD noch für das Zielobjekt ein Wert vorhanden ist. Bei der üblichen Konfiguration bleibt dieses Feld leer.

## <a name="understanding-attribute-mapping-properties"></a>Grundlegendes zu Attributzuordnungseigenschaften

Im vorherigen Abschnitt haben Sie bereits die Attributzuordnungstyp-Eigenschaft kennengelernt.
Zusätzlich zu dieser Eigenschaft unterstützen Attributzuordnungen auch die folgenden Attribute:

- **Quellattribut:** Das Benutzerattribut aus dem Quellsystem (Beispiel: Active Directory).
- **Zielattribut**: Das Benutzerattribut im Zielsystem (Beispiel: Azure Active Directory).
- **Standardwert bei Null (optional)** : Der Wert, der an das Zielsystem übermittelt wird, wenn das Quellattribut den Wert NULL hat. Dieser Wert wird nur beim Erstellen eines Benutzers bereitgestellt. Beim Aktualisieren eines vorhandenen Benutzers wird „Standardwert bei Null“ nicht bereitgestellt.  
- **Diese Zuordnung anwenden**
  - **Immer**: Wenden Sie diese Zuordnung sowohl bei der Aktion zum Erstellen eines Benutzers als auch bei der zum Aktualisieren eines Benutzers an.
  - **Nur während der Erstellung**: Wenden Sie diese Zuordnung nur bei der Aktion zum Erstellen eines Benutzers an.

> [!NOTE]
> In diesem Dokument wird beschrieben, wie Sie Attribute mit dem Azure-Portal zuordnen.  Weitere Informationen zur Verwendung von Graph finden Sie unter [Transformationen](how-to-transformation.md).

## <a name="using-attribute-mapping"></a>Verwenden der Attributzuordnung

Führen Sie die folgenden Schritte aus, um die neue Funktion zu verwenden.

1.  Wählen Sie im Azure-Portal die Option **Azure Active Directory** aus.
2.  Wählen Sie **Azure AD Connect** aus.
3.  Wählen Sie **Cloudsynchronisierung verwalten** aus.

    ![Verwalten der Bereitstellung](media/how-to-install/install-6.png)

4. Wählen Sie unter **Konfiguration** Ihre Konfiguration aus.
5. Wählen Sie **Zum Bearbeiten der Zuordnungen klicken** aus.  Der Bildschirm für die Attributzuordnung wird geöffnet.

    ![Hinzufügen von Attributen](media/how-to-attribute-mapping/mapping-6.png)

6.  Klicken Sie auf **Attribut hinzufügen**.

    ![Zuordnungstyp](media/how-to-attribute-mapping/mapping-1.png)

7. Wählen Sie den **Zuordnungstyp** aus.  In diesem Beispiel wird „Ausdruck“ verwendet.
8.  Geben Sie den Ausdruck im Feld ein.  In diesem Beispiel verwenden wir `Replace([mail], "@contoso.com", , ,"", ,).`.
9.  Geben Sie das Zielattribut ein.  In diesem Beispiel verwenden wir „ExtensionAttribute15“.
10. Wählen Sie aus, wann dieses angewendet werden soll, und klicken Sie dann auf **Anwenden**.

    ![Zuordnungen bearbeiten](media/how-to-attribute-mapping/mapping-2a.png)

11. Sie kehren zum Bildschirm für die Attributzuordnung zurück. Die neue Attributzuordnung sollte jetzt angezeigt werden.  
12. Klicken Sie auf **Schema speichern**.

    ![Schema speichern](media/how-to-attribute-mapping/mapping-3.png)

## <a name="test-your-attribute-mapping"></a>Testen der Attributzuordnung

Zum Testen der Attributzuordnung können Sie [Bedarfsorientierte Bereitstellung](how-to-on-demand-provision.md) verwenden.  Wählen Sie in der Liste 

1. Wählen Sie im Azure-Portal die Option **Azure Active Directory** aus.
2. Wählen Sie **Azure AD Connect** aus.
3. Wählen Sie **Bereitstellung verwalten** aus.
4. Wählen Sie unter **Konfiguration** Ihre Konfiguration aus.
5. Klicken Sie unter **Validieren** auf die Schaltfläche **Benutzer bereitstellen**. 
6. Führen Sie die folgenden Schritte auf dem Bildschirm für die bedarfsorientierte Bereitstellung aus.  Geben Sie den **Distinguished Name** eines Benutzers oder einer Gruppe ein, und klicken Sie auf die Schaltfläche **Bereitstellen**.  
7. Sobald der Vorgang abgeschlossen ist, sollten ein Erfolgsbildschirm und vier grüne Kontrollkästchen angezeigt werden, die darauf hinweisen, dass die Bereitstellung erfolgreich war.  

    ![Erfolgreiche Bereitstellung](media/how-to-attribute-mapping/mapping-4.png)

8. Klicken Sie unter **Aktion ausführen** auf **Details anzeigen**.  Auf der rechten Seite sollte angezeigt werden, dass das neue Attribut synchronisiert und der Ausdruck angewendet wurde.

  ![Aktion ausführen](media/how-to-attribute-mapping/mapping-5.png)

## <a name="next-steps"></a>Nächste Schritte

- [Worum handelt es sich bei der Azure AD Connect-Cloudsynchronisierung?](what-is-cloud-sync.md)
- [Schreiben von Ausdrücken für Attributzuordnungen](reference-expressions.md)
- [Attribute, die synchronisiert werden](../hybrid/reference-connect-sync-attributes-synchronized.md?context=azure%2factive-directory%2fcloud-provisioning%2fcontext%2fcp-context/hybrid/reference-connect-sync-attributes-synchronized.md)
