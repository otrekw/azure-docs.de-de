---
title: 'Bereitstellen der RD-Gateway-Rolle in Windows Virtual Desktop: Azure'
description: 'Vorgehensweise: Bereitstellen der RD-Gateway-Rolle in Windows Virtual Desktop.'
author: Heidilohr
ms.topic: how-to
ms.date: 01/26/2021
ms.author: helohr
manager: lizross
ms.openlocfilehash: ca3c36f339d5cbca6d5b5990415fba180ae42a13
ms.sourcegitcommit: 95c2cbdd2582fa81d0bfe55edd32778ed31e0fe8
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/26/2021
ms.locfileid: "98798018"
---
# <a name="deploy-the-rd-gateway-role-in-windows-virtual-desktop"></a>Bereitstellen der RD-Gateway-Rolle in Windows Virtual Desktop

In diesem Artikel erfahren Sie, wie Sie die Remotedesktop-Gateway-Server in Ihrer Umgebung bereitstellen. Sie können die Serverrollen auf physischen oder virtuellen Computern installieren, je nachdem, ob Sie eine lokale, cloudbasierte oder Hybrid-Umgebung erstellen.

## <a name="install-the-rd-gateway-role"></a>Installieren der RD-Gateway-Rolle

1. Melden Sie sich mit Administratoranmeldeinformationen beim Zielserver an.

2. Wählen Sie unter **Server-Manager** die Option **Verwalten** und dann **Rollen und Features hinzufügen** aus. Das Installationsprogramm **Rollen und Features hinzufügen** wird geöffnet.

3. Wählen Sie unter **Bevor Sie beginnen** die Option **Weiter** aus.

4. Wählen Sie in **Installationstyp auswählen** die Option **Rollenbasierte oder featurebasierte Installation** und dann **Weiter** aus.

5. Wählen Sie für **Zielserver auswählen** die Option **Einen Server aus Serverpool auswählen** aus. Wählen Sie für **Serverpool** den Namen des lokalen Computers aus. Klicken Sie auf **Weiter**, wenn Sie fertig sind.

6. Wählen Sie in **Serverrollen auswählen** > **Rollen** die Option **Remotedesktopdienste** aus. Klicken Sie auf **Weiter**, wenn Sie fertig sind.

7. Wählen Sie in **Remotedesktopdienste** die Option **Weiter** aus.

8. Wählen Sie für **Rollendienste auswählen** nur **Remotedesktop-Gateway** aus. Wenn Sie zum Hinzufügen erforderlicher Features aufgefordert werden, wählen Sie **Features hinzufügen** aus. Klicken Sie auf **Weiter**, wenn Sie fertig sind.

9. Wählen Sie für **Netzwerkrichtlinien- und Zugriffsdienste** die Option **Weiter** aus.

10. Wählen Sie für **Rolle „Webserver“ (IIS)** die Option **Weiter** aus.

11. Wählen Sie für **Rollendienste auswählen** die Option **Weiter** aus.

12. Wählen Sie für **Installationsauswahl bestätigen** die Option **Installieren** aus. Schließen Sie das Installationsprogramm nicht, während der Installationsvorgang ausgeführt wird.

## <a name="configure-rd-gateway-role"></a>Konfigurieren der RD-Gateway-Rolle

Nachdem Sie die RD-Gateway-Rolle installiert haben, müssen Sie sie konfigurieren.

So konfigurieren Sie die RD-Gateway-Rolle:

1. Öffnen Sie den **Server-Manager**, und wählen Sie dann **Remotedesktopdienste** aus.

2. Wechseln Sie zu **Server**, klicken Sie mit der rechten Maustaste auf den Namen Ihres Servers, und wählen Sie dann **RD-Gateway-Manager** aus.

3. Klicken Sie im RD-Gateway-Manager mit der rechten Maustaste auf den Namen Ihres Gateways, und wählen Sie dann **Eigenschaften** aus.

4. Öffnen Sie die Registerkarte **SSL-Zertifikat**, und wählen Sie die Blase **Ein Zertifikat in das RD-Gateway importieren** und dann **Zertifikat durchsuchen und importieren...** aus.

5. Wählen Sie den Namen Ihrer PFX-Datei und dann **Öffnen** aus.

6. Geben Sie das Kennwort für die PFX-Datei ein, sobald Sie dazu aufgefordert werden.

7. Nachdem Sie das Zertifikat und seinen privaten Schlüssel importiert haben, sollten die Schlüsselattribute des Zertifikats angezeigt werden.

>[!NOTE]
>Da die RD-Gateway-Rolle öffentlich sein sollte, sollten Sie ein öffentlich ausgestelltes Zertifikat verwenden. Wenn Sie ein privat ausgestelltes Zertifikat verwenden, müssen Sie sicherstellen, dass alle Clients zuvor mit der Vertrauenskette des Zertifikats konfiguriert werden.

## <a name="next-steps"></a>Nächste Schritte

Wenn Sie Ihrer RD-Gateway-Rolle Hochverfügbarkeit hinzufügen möchten, finden Sie weitere Informationen unter [Hinzufügen von Hochverfügbarkeit zu RD-Web und RD-Gateway (Webfront)](/windows-server/remote/remote-desktop-services/rds-rdweb-gateway-ha).