---
title: 'Azure Stack Edge Pro-GPU: Verwalten von Benutzern | Microsoft-Dokumentation'
description: Hier wird erläutert, wie Sie mit dem Azure-Portal Benutzer auf Ihrem Azure Stack Edge Pro-GPU-Gerät verwalten können.
services: databox
author: alkohli
ms.service: databox
ms.subservice: edge
ms.topic: how-to
ms.date: 02/21/2021
ms.author: alkohli
ms.openlocfilehash: e19c2b8a6785eb29426e150e7aa29097974765a1
ms.sourcegitcommit: 5bbc00673bd5b86b1ab2b7a31a4b4b066087e8ed
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/07/2021
ms.locfileid: "102436514"
---
# <a name="use-the-azure-portal-to-manage-users-on-your-azure-stack-edge-pro"></a>Verwalten von Benutzern auf Azure Stack Edge Pro-Geräten mit dem Azure-Portal

[!INCLUDE [applies-to-GPU-and-pro-r-and-mini-r-skus](../../includes/azure-stack-edge-applies-to-gpu-pro-r-mini-r-sku.md)]

In diesem Artikel wird beschrieben, wie Sie Benutzer auf Ihrem Azure Stack Edge Pro-Gerät verwalten. Sie können das Azure Stack Edge Pro-Gerät mithilfe des Azure-Portals oder über die lokale Webbenutzeroberfläche verwalten. Verwenden Sie das Azure-Portal, um Benutzer hinzuzufügen, zu ändern oder zu löschen.

In diesem Artikel werden folgende Vorgehensweisen behandelt:

> [!div class="checklist"]
> * Hinzufügen eines Benutzers
> * Ändern eines Benutzers
> * Löschen eines Benutzers

## <a name="about-users"></a>Informationen zu Benutzern

Benutzer können über Lesezugriff oder über Vollzugriff verfügen. Benutzer mit Lesezugriff können nur die Freigabedaten anzeigen. Benutzer mit Vollzugriff können Freigabedaten lesen, Schreibvorgänge für diese Freigaben ausführen sowie Freigabedaten ändern oder löschen.

 - **Benutzer mit Vollzugriff:** Lokaler Benutzer mit Vollzugriff.
 - **Benutzer mit Lesezugriff:** Lokaler Benutzer, der nur über Lesezugriff verfügt. Diese Benutzer werden Freigaben zugeordnet, die schreibgeschützte Vorgänge zulassen.

Die Benutzerberechtigungen werden definiert, wenn der Benutzer im Rahmen der Freigabeerstellung erstellt wird. Sie können mit dem Datei-Explorer geändert werden.


## <a name="add-a-user"></a>Hinzufügen eines Benutzers

Gehen Sie im Azure-Portal wie folgt vor, um einen Benutzer hinzuzufügen:

1. Wechseln Sie im Azure-Portal zu Ihrer Azure Stack Edge-Ressource und dann zu **Benutzern**. Wählen Sie auf der Befehlsleiste die Option **+ Benutzer hinzufügen** aus.

    ![Auswählen von „Benutzer hinzufügen“](media/azure-stack-edge-j-series-manage-users/add-user-1.png)

2. Geben Sie den Benutzernamen und das Kennwort für den Benutzer an, den Sie hinzufügen möchten. Bestätigen Sie das Kennwort, und wählen Sie **Hinzufügen** aus.

    ![Angeben des Benutzernamens und Kennworts](media/azure-stack-edge-j-series-manage-users/add-user-2.png)

    > [!IMPORTANT] 
    > Folgende Benutzer sind für das System reserviert und dürfen nicht verwendet werden: Administrator, EdgeUser, EdgeSupport, HcsSetupUser, WDAGUtilityAccount, CLIUSR, DefaultAccount, Guest.  

3. Eine Benachrichtigung wird angezeigt, wenn die Benutzererstellung beginnt und wenn sie abgeschlossen ist. Wählen Sie nach Abschluss der Benutzererstellung auf der Befehlsleiste die Option **Aktualisieren** aus, um die aktualisierte Benutzerliste anzuzeigen.


## <a name="modify-user"></a>Ändern eines Benutzers

Das Kennwort für einen Benutzer kann nach der Benutzererstellung geändert werden. Wählen Sie in der Benutzerliste einen Benutzer aus. Geben Sie ein neues Kennwort ein, und bestätigen Sie es. Speichern Sie die Änderungen.

![Ändern eines Benutzers](media/azure-stack-edge-j-series-manage-users/modify-user-1.png)


## <a name="delete-a-user"></a>Löschen eines Benutzers

Gehen Sie im Azure-Portal wie folgt vor, um einen Benutzer zu löschen:


1. Wechseln Sie im Azure-Portal zu Ihrer Azure Stack Edge-Ressource und dann zu **Benutzern**.

    ![Auswählen des zu löschenden Benutzers](media/azure-stack-edge-j-series-manage-users/delete-user-1.png)

2. Wählen Sie in der Benutzerliste einen Benutzer und anschließend **Löschen** aus. Bestätigen Sie den Löschvorgang, wenn Sie dazu aufgefordert werden.

    ![Auswählen des zu löschenden Benutzers 2](media/azure-stack-edge-j-series-manage-users/delete-user-2.png)

Die Benutzerliste wird nach dem Löschen des Benutzers entsprechend aktualisiert.

![Aktualisierte Benutzerliste](media/azure-stack-edge-j-series-manage-users/delete-user-4.png)

## <a name="next-steps"></a>Nächste Schritte

- Erfahren Sie, wie Sie [Bandbreite verwalten](azure-stack-edge-j-series-manage-bandwidth-schedules.md).
