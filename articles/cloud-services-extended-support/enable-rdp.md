---
title: Anwenden des Remotedesktops in Cloud Services (erweiterter Support)
description: Aktivieren des Remotedesktops für Cloud Services (erweiterter Support)
ms.topic: how-to
ms.service: cloud-services-extended-support
author: gachandw
ms.author: gachandw
ms.reviewer: mimckitt
ms.date: 10/13/2020
ms.custom: ''
ms.openlocfilehash: 53f873013a6f16ce5a28ee5d915afa556057f643
ms.sourcegitcommit: 6272bc01d8bdb833d43c56375bab1841a9c380a5
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/23/2021
ms.locfileid: "98743974"
---
# <a name="apply-the-remote-desktop-extension-to-azure-cloud-services-extended-support"></a>Anwenden der Remotedesktoperweiterung auf Azure Cloud Services (erweiterter Support)

Das Azure-Portal verwendet die Remotedesktoperweiterung, damit Remotedesktop auch nach der Bereitstellung der Anwendung aktiviert werden kann. Mit den Remotedesktopeinstellungen für Ihren Clouddienst können Sie Remotedesktop aktivieren, das lokale Administratorkonto aktualisieren, die zur Authentifizierung verwendeten Zertifikate auswählen und das Ablaufdatum für diese Zertifikate festlegen. 

## <a name="apply-remote-desktop--extension"></a>Anwenden der Remotedesktoperweiterung
1. Navigieren Sie zu dem Clouddienst, für den Sie Remotedesktop aktivieren möchten, und wählen Sie im linken Navigationsbereich **Remotedesktop** aus.

    :::image type="content" source="media/remote-desktop-1.png" alt-text="Die Abbildung zeigt das Auswählen der Option „Remotedesktop“ im Azure-Portal.":::

2. Wählen Sie **Hinzufügen**.
3. Wählen Sie die Rollen aus, für die Remotedesktop aktiviert werden soll.
4. Füllen Sie die erforderlichen Felder für Benutzername, Kennwort, Ablauf und Zertifikat aus (nicht erforderlich).

    :::image type="content" source="media/remote-desktop-2.png" alt-text="Die Abbildung zeigt das Eingeben der Informationen, die zum Herstellen einer Verbindung mit dem Remotedesktop erforderlich sind.":::

5. Wenn Sie fertig sind, wählen Sie **Speichern** aus. Es dauert einige Minuten, bis die Rolleninstanzen Verbindungen empfangen können.

## <a name="connect-to-role-instances-with-remote-desktop-enabled"></a>Herstellen einer Verbindung mit Rolleninstanzen mit aktiviertem Remotedesktop
Nachdem der Remotedesktop für die Rollen aktiviert wurde, können Sie direkt im Azure-Portal eine Verbindung initiieren.

1. Klicken Sie auf **Rollen und Instanzen**, um die Instanzeneinstellungen zu öffnen.

    :::image type="content" source="media/remote-desktop-3.png" alt-text="Die Abbildung zeigt, wie die Option „Rollen und Instanzen“ auf dem Konfigurationsblatt ausgewählt wird.":::

2. Wählen Sie eine Rolleninstanz aus, in der der Remotedesktop konfiguriert ist.
3. Klicken Sie auf **Verbinden**, um eine Remotedesktop-Verbindungsdatei herunterzuladen.

    :::image type="content" source="media/remote-desktop-4.png" alt-text="Die Abbildung zeigt die Auswahl der Workerrolleninstanz im Azure-Portal.":::
    
4. Öffnen Sie die Datei, um eine Verbindung mit der Rolleninstanz herzustellen.


## <a name="next-steps"></a>Nächste Schritte 
- Überprüfen Sie die [Bereitstellungsvoraussetzungen](deploy-prerequisite.md) für Cloud Services (erweiterter Support).
- Sehen Sie sich die [häufig gestellten Fragen](faq.md) zu Cloud Services (erweiterter Support) an.
- Stellen Sie eine Cloud Service-Instanz (erweiterter Support) über das [Azure-Portal](deploy-portal.md), mit [PowerShell](deploy-powershell.md), einer [Vorlage](deploy-template.md) oder [Visual Studio](deploy-visual-studio.md) bereit.