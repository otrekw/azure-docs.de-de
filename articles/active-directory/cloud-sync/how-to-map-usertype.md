---
title: Verwenden der Zuordnung von UserType mit Azure AD Connect-Cloudsynchronisierung
description: In diesem Artikel wird beschrieben, wie die Zuordnung des UserType-Attributs mit Cloudsynchronisierung verwendet wird.
services: active-directory
author: billmath
manager: mtillman
ms.service: active-directory
ms.workload: identity
ms.topic: how-to
ms.date: 05/04/2021
ms.subservice: hybrid
ms.author: billmath
ms.collection: M365-identity-device-management
ms.openlocfilehash: 14bbeb7339b3af41b24b25aed2332ac8367e99ae
ms.sourcegitcommit: 02d443532c4d2e9e449025908a05fb9c84eba039
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/06/2021
ms.locfileid: "108776637"
---
# <a name="map-usertype-with-cloud-sync"></a>Zuordnen von UserType mit Cloudsynchronisierung

Cloudsynchronisierung unterstützt die Synchronisierung des UserType-Attributs für User-Objekte. 

Standardmäßig ist das UserType-Attribut nicht für die Synchronisierung aktiviert, da kein entsprechendes UserType-Attribut im lokalen Active Directory vorhanden ist. Sie müssen diese Zuordnung für die Synchronisierung manuell hinzufügen. Beachten Sie das folgende von Azure AD erzwungene Verhalten:

- Azure AD akzeptiert nur zwei Werte für das UserType-Attribut: Member und Guest.
- Wenn das UserType-Attribut in der Cloudsynchronisierung nicht zugeordnet ist, ist das UserType-Attribut von Azure AD-Benutzern, die über die Verzeichnissynchronisierung erstellt wurden, auf „Member“ festgelegt.

Vor dem Hinzufügen einer Zuordnung für das UserType-Attribut müssen Sie zunächst entscheiden, wie das Attribut vom lokalen Active Directory abgeleitet wird. Folgende Vorgehensweisen sind die gängigsten:

 - Legen Sie ein nicht verwendetes lokales AD-Attribut (z.B. „extensionAttribute1“) als Quellattribut fest. Das festgelegte lokale AD-Attribut sollte vom Typ Zeichenfolge und einwertig sein und den Wert Member oder Guest enthalten.
 - Wenn Sie sich für diesen Ansatz entscheiden, müssen Sie vor dem Aktivieren der Synchronisierung des UserType-Attributs sicherstellen, dass das ausgewählte Attribut mit dem richtigen Wert für alle vorhandenen Benutzerobjekte im lokalen Active Directory, die mit Azure AD synchronisiert werden, aufgefüllt wird.

## <a name="to-add-the-usertype-mapping"></a>So fügen Sie die UserType-Zuordnung hinzu
Um die UserType-Zuordnung hinzuzufügen, verwenden Sie die folgenden Schritte.

 1. Wählen Sie im Azure-Portal die Option **Azure Active Directory** aus.
 2. Wählen Sie **Azure AD Connect** aus.
 3. Wählen Sie **Cloudsynchronisierung verwalten** aus.
 4. Wählen Sie unter **Konfiguration** Ihre Konfiguration aus.
 5. Wählen Sie unter **Attribute verwalten** die Option **Zum Bearbeiten von Zuordnungen klicken** aus.
  ![Bearbeiten der Attributzuordnungen](media/how-to-map-usertype/usertype-1.png) 

 6. Klicken Sie auf **Attributzuordnung hinzufügen**.
    ![Hinzufügen einer neuen Attributzuordnung](media/how-to-map-usertype/usertype-2.png) 
7. Wählen Sie den Zuordnungstyp aus. Sie können die Zuordnung auf eine von drei Arten vornehmen:
 - direkte Zuordnung (d. h. aus einem AD-Attribut)
 - Ausdruck (IIF(InStr([userPrincipalName], "@partners") > 0,"Guest","Member"))
 - Konstante (d. h. alle Benutzerobjekte als Gast zuordnen).
  ![Benutzertyp hinzufügen](media/how-to-map-usertype/usertype-3.png) 
8. Wählen Sie in der Zielattribut-Dropdownliste die Option UserType aus.
9. Klicken Sie unten auf der Seite auf die Schaltfläche **Zuweisen**. Dadurch wird eine Zuordnung für das Azure AD UserType-Attribut erstellt.

## <a name="next-steps"></a>Nächste Schritte 

- [Schreiben von Ausdrücken für Attributzuordnungen in Azure Active Directory](reference-expressions.md)
- [Konfiguration der Cloudsynchronisierung](how-to-configure.md)
