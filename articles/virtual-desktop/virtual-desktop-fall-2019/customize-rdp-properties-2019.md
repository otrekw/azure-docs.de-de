---
title: Anpassen von RDP-Eigenschaften mit PowerShell Azure Virtual Desktop (klassisch) – Azure
description: So passen Sie RDP-Eigenschaften für Azure Virtual Desktop (klassisch) mit PowerShell-Cmdlets an.
author: Heidilohr
ms.topic: how-to
ms.date: 03/30/2020
ms.author: helohr
manager: femila
ms.openlocfilehash: 9a9d5461be3a3e8954593d8230ea3d58f0c7ef6c
ms.sourcegitcommit: 8bca2d622fdce67b07746a2fb5a40c0c644100c6
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/09/2021
ms.locfileid: "111752079"
---
# <a name="customize-remote-desktop-protocol-properties-for-a--azure-virtual-desktop-classic-host-pool"></a>Anpassen der Remotedesktopprotokoll-Eigenschaften für einen Azure Virtual Desktop (klassisch)-Hostpool

>[!IMPORTANT]
>Dieser Inhalt gilt für Azure Virtual Desktop (klassisch). Der Dienst unterstützt keine Azure Virtual Desktop-Objekte in Azure Resource Manager. Wenn Sie Azure Virtual Desktop-Objekte in Azure Resource Manager verwalten möchten, helfen Ihnen die Informationen in [diesem Artikel](../customize-rdp-properties.md) weiter.

Durch Anpassen von RDP-Eigenschaften (Remotedesktopprotokoll) eines Hostpools, z.B. Unterstützung mehrerer Monitore und Audioumleitung, können Sie für Benutzer eine optimale Bedienumgebung bereitstellen, die deren Anforderungen entspricht. RDP-Eigenschaften können Sie in Azure Virtual Desktop über den **-CustomRdpProperty**-Parameter im **Set-RdsHostPool**-Cmdlet anpassen.

Eine vollständige Liste der unterstützten Eigenschaften samt deren Standardwerten finden Sie unter [Unterstützte RDP-Dateieinstellungen](/windows-server/remote/remote-desktop-services/clients/rdp-files?context=%2fazure%2fvirtual-desktop%2fcontext%2fcontext).

Zunächst müssen Sie das [PowerShell-Modul für Azure Virtual Desktop](/powershell/windows-virtual-desktop/overview/) herunterladen und importieren, um es in Ihrer PowerShell-Sitzung verwenden zu können. Führen Sie anschließend das folgende Cmdlet aus, um sich bei Ihrem Konto anzumelden:

```powershell
Add-RdsAccount -DeploymentUrl "https://rdbroker.wvd.microsoft.com"
```

## <a name="default-rdp-properties"></a>RDP-Standardeigenschaften

Veröffentlichte RDP-Dateien enthalten standardmäßig die folgenden Eigenschaften:

|RDP-Eigenschaften | Desktops | RemoteApps |
|---|---| --- |
| Mehrfachmonitor-Modus | Aktiviert | – |
| Aktivierte Laufwerksumleitungen | Laufwerke, Zwischenablage, Drucker, COM-Anschlüsse, USB-Geräte und Smartcards| Laufwerke, Zwischenablage und Drucker |
| Remoteaudio-Modus | Lokale Wiedergabe | Lokale Wiedergabe |

Alle benutzerdefinierten Eigenschaften, die Sie für den Hostpool definieren, überschreiben diese Standardeinstellungen.

## <a name="add-or-edit-a-single-custom-rdp-property"></a>Hinzufügen oder Bearbeiten einer einzelnen benutzerdefinierten RDP-Eigenschaft

Wenn Sie eine einzelne benutzerdefinierte RDP-Eigenschaft hinzufügen oder bearbeiten möchten, führen Sie das folgende PowerShell-Cmdlet aus:

```powershell
Set-RdsHostPool -TenantName <tenantname> -Name <hostpoolname> -CustomRdpProperty "<property>"
```

> [!div class="mx-imgBorder"]
> ![Screenshot des PowerShell-Cmdlets Get-RDSRemoteApp mit hervorgehobenem Namen und Anzeigenamen, um eine benutzerdefinierte RDP-Eigenschaft zu bearbeiten.](../media/singlecustomrdpproperty.png)

## <a name="add-or-edit-multiple-custom-rdp-properties"></a>Hinzufügen oder Bearbeiten von mehreren benutzerdefinierten RDP-Eigenschaften

Wenn Sie mehrere benutzerdefinierte RDP-Eigenschaften hinzufügen oder bearbeiten möchten, führen Sie die folgenden PowerShell-Cmdlets aus, wobei Sie die benutzerdefinierten RDP-Eigenschaften als eine Zeichenfolge mit Semikolons als Trennzeichen angeben:

```powershell
$properties="<property1>;<property2>;<property3>"
Set-RdsHostPool -TenantName <tenantname> -Name <hostpoolname> -CustomRdpProperty $properties
```

> [!div class="mx-imgBorder"]
> ![Screenshot des PowerShell-Cmdlets Set-RDSRemoteApp mit hervorgehobenem Namen und Anzeigenamen, um eine benutzerdefinierte RDP-Eigenschaft zu bearbeiten.](../media/multiplecustomrdpproperty.png)

## <a name="reset-all-custom-rdp-properties"></a>Zurücksetzen aller benutzerdefinierten RDP-Eigenschaften

Sie können eine einzelne benutzerdefinierte RDP-Eigenschaft auf deren Standardwert zurücksetzen, indem Sie den Anweisungen unter [Hinzufügen oder Bearbeiten einer einzelnen benutzerdefinierten RDP-Eigenschaft](#add-or-edit-a-single-custom-rdp-property) folgen, oder Sie können alle benutzerdefinierten RDP-Eigenschaften für einen Hostpool zurücksetzen, indem Sie das folgende PowerShell-Cmdlet ausführen:

```powershell
Set-RdsHostPool -TenantName <tenantname> -Name <hostpoolname> -CustomRdpProperty ""
```

> [!div class="mx-imgBorder"]
> ![Screenshot des PowerShell-Cmdlets Get-RDSRemoteApp mit hervorgehobenem Namen und Anzeigenamen.](../media/resetcustomrdpproperty.png)

## <a name="next-steps"></a>Nächste Schritte

Nachdem Sie die RDP-Eigenschaften eines bestimmten Hostpools angepasst haben, können Sie sich bei einem Azure Virtual Desktop-Client anmelden, um die Einstellungen als Teil einer Benutzersitzung zu testen. In den nächsten zwei Vorgehensweisen erfahren Sie, wie Sie mit dem ausgewählten Client eine Verbindung zu einer Sitzung herstellen:

- [Herstellen einer Verbindung mit dem Windows-Desktopclient](connect-windows-7-10-2019.md)
- [Herstellen einer Verbindung mit dem Webclient](connect-web-2019.md)