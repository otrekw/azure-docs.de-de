---
title: 'Azure: Zuweisungstyp für persönliche Desktops in Azure Virtual Desktop (klassisch)'
description: So konfigurieren Sie den Zuweisungstyp für einen Azure Virtual Desktop-Hostpool (klassisch) mit persönlichen Desktops.
author: Heidilohr
ms.topic: how-to
ms.date: 05/22/2020
ms.author: helohr
manager: femila
ms.openlocfilehash: 20123cb66bfd5fdd2b77c1a7c1afbae28a93f45e
ms.sourcegitcommit: 8bca2d622fdce67b07746a2fb5a40c0c644100c6
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/09/2021
ms.locfileid: "111750118"
---
# <a name="configure-the-personal-desktop-host-pool-assignment-type-for-azure-virtual-desktop-classic"></a>Konfigurieren des Zuweisungstyps für einen Azure Virtual Desktop-Hostpool (klassisch) mit persönlichen Desktops

>[!IMPORTANT]
>Dieser Inhalt gilt für Azure Virtual Desktop (klassisch). Der Dienst unterstützt keine Azure Virtual Desktop-Objekte in Azure Resource Manager. Wenn Sie Azure Virtual Desktop-Objekte in Azure Resource Manager verwalten möchten, helfen Ihnen die Informationen in [diesem Artikel](../configure-host-pool-personal-desktop-assignment-type.md) weiter.

Sie können den Zuweisungstyp Ihres Hostpools mit persönlichen Desktops konfigurieren, um Ihre Azure Virtual Desktop-Umgebung besser an Ihre Bedürfnisse anzupassen. In diesem Thema erfahren Sie, wie Sie die automatische oder direkte Zuweisung für Ihre Benutzer konfigurieren.

>[!NOTE]
> Die Anweisungen in diesem Artikel gelten nur für Hostpools mit persönlichen Desktops, nicht für gepoolte Hostpools, da Benutzer in gepoolten Hostpools keinen bestimmten Sitzungshosts zugewiesen sind.

## <a name="configure-automatic-assignment"></a>Konfigurieren der automatischen Zuweisung

Die automatische Zuweisung ist der Standardzuweisungstyp für neue Hostpools mit persönlichen Desktops, die in Ihrer Azure Virtual Desktop-Umgebung erstellt werden. Für das automatische Zuweisen von Benutzern ist kein bestimmter Sitzungshost erforderlich.

Um Benutzer automatisch zuzuweisen, weisen Sie sie zunächst dem Hostpool mit persönlichen Desktops zu, damit sie den Desktop in ihrem Feed sehen können. Wenn ein zugewiesener Benutzer den Desktop in seinem Feed startet, beansprucht er einen verfügbaren Sitzungshost, wenn er nicht bereits eine Verbindung mit dem Hostpool hergestellt hat, wodurch der Zuweisungsvorgang abgeschlossen wird.

Zur Vorbereitung müssen Sie ggf. zunächst das [Azure Virtual Desktop-PowerShell-Modul herunterladen und importieren](/powershell/windows-virtual-desktop/overview/).

> [!NOTE]
> Stellen Sie sicher, dass Version 1.0.1534.2001 oder höher des Azure Virtual Desktop-PowerShell-Moduls installiert ist, bevor Sie diese Anweisungen ausführen.

Führen Sie anschließend das folgende Cmdlet aus, um sich bei Ihrem Konto anzumelden:

```powershell
Add-RdsAccount -DeploymentUrl "https://rdbroker.wvd.microsoft.com"
```

Führen Sie das folgende PowerShell-Cmdlet aus, um einen Hostpool für das automatische Zuweisen von Benutzern zu VMs zu konfigurieren:

```powershell
Set-RdsHostPool <tenantname> <hostpoolname> -AssignmentType Automatic
```

Führen Sie das folgende PowerShell-Cmdlet aus, um einen Benutzer zum Hostpool mit persönlichen Desktops zuzuweisen:

```powershell
Add-RdsAppGroupUser <tenantname> <hostpoolname> "Desktop Application Group" -UserPrincipalName <userupn>
```

## <a name="configure-direct-assignment"></a>Konfigurieren der direkten Zuweisung

Im Gegensatz zur automatischen Zuweisung müssen Sie bei der direkten Zuweisung den Benutzer sowohl dem Hostpool mit persönlichen Desktops als auch einem bestimmten Sitzungshost zuweisen, bevor er sich mit seinem persönlichen Desktop verbinden kann. Wenn der Benutzer nur einem Hostpool ohne eine Sitzungshostzuweisung zugewiesen ist, kann er nicht auf Ressourcen zugreifen.

Um einen Hostpool so zu konfigurieren, dass eine direkte Zuweisung von Benutzern zu Sitzungshosts erforderlich ist, führen Sie das folgende PowerShell-Cmdlet aus:

```powershell
Set-RdsHostPool <tenantname> <hostpoolname> -AssignmentType Direct
```

Führen Sie das folgende PowerShell-Cmdlet aus, um einen Benutzer zum Hostpool mit persönlichen Desktops zuzuweisen:

```powershell
Add-RdsAppGroupUser <tenantname> <hostpoolname> "Desktop Application Group" -UserPrincipalName <userupn>
```

Um einen Benutzer einem bestimmten Sitzungshost zuzuweisen, führen Sie das folgende PowerShell-Cmdlet aus:

```powershell
Set-RdsSessionHost <tenantname> <hostpoolname> -Name <sessionhostname> -AssignedUser <userupn>
```

## <a name="remove-a-user-assignment"></a>Entfernen einer Benutzerzuweisung

Eine Benutzerzuweisung kann beispielsweise entfernt werden, wenn der Benutzer den persönlichen Desktop nicht mehr benötigt oder das Unternehmen verlassen hat oder wenn Sie den Desktop für eine andere Person wiederverwenden möchten.

Aktuell muss zum Entfernen der Benutzerzuweisung für einen persönlichen Desktop der Sitzungshost vollständig entfernt werden. Führen Sie zum Entfernen des Sitzungshosts das folgende Cmdlet aus:

```powershell
Remove-RdsSessionHost
```

Wenn Sie den Sitzungshost wieder dem Hostpool mit persönlichen Desktops hinzufügen möchten, deinstallieren Sie Azure Virtual Desktop auf dem entsprechenden Computer, und führen Sie dann die unter [Erstellen eines Hostpools mit PowerShell](create-host-pools-powershell-2019.md) beschriebenen Schritte aus, um den Sitzungshost erneut zu registrieren.

## <a name="next-steps"></a>Nächste Schritte

Nachdem Sie den persönlichen Desktopzuweisungstyp konfiguriert haben, können Sie sich an einem Azure Virtual Desktop-Client anmelden, um ihn als Teil einer Benutzersitzung zu testen. In den nächsten zwei Vorgehensweisen erfahren Sie, wie Sie mit dem ausgewählten Client eine Verbindung zu einer Sitzung herstellen:

- [Herstellen einer Verbindung mit dem Windows-Desktopclient](connect-windows-7-10-2019.md)
- [Herstellen einer Verbindung mit dem Webclient](connect-web-2019.md)
