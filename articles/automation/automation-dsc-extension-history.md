---
title: Arbeiten mit der Versionsgeschichte der Azure Desired State Configuration-Erweiterung
description: In diesem Artikel wird beschrieben, wie Sie mit der Versionsgeschichte für die Desired State Configuration-Erweiterung (DSC) in Azure arbeiten.
ms.date: 06/21/2018
keywords: DSC, PowerShell, Azure, Erweiterung
author: mgoedtel
ms.author: magoedte
services: automation
ms.service: automation
ms.subservice: dsc
ms.topic: conceptual
ms.openlocfilehash: 0bb82ce92a151bd8e955c136751d8e7f307bbf1b
ms.sourcegitcommit: ec682dcc0a67eabe4bfe242fce4a7019f0a8c405
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/09/2020
ms.locfileid: "86186417"
---
# <a name="work-with-azure-desired-state-configuration-extension-version-history"></a>Arbeiten mit der Versionsgeschichte der Azure Desired State Configuration-Erweiterung

Die VM-Erweiterung von Azure Desired State Configuration (DSC) wird bei Bedarf aktualisiert, um Verbesserungen und neue Funktionen von Azure, Windows Server und Windows Management Framework (WMF) zu unterstützen, die in Windows PowerShell enthalten sind.

Dieser Artikel informiert Sie über alle Versionen der Azure DSC-VM-Erweiterung sowie über die unterstützten Umgebungen und enthält Kommentare und Hinweise zu neuen Features und Änderungen.

## <a name="latest-version"></a>Aktuelle Version

### <a name="version-276"></a>Version 2.76

- **Veröffentlichungsdatum:**
  - 9\. Mai 2018 (Azure) | 21. Juni 2018 (Azure China 21Vianet, Azure Government)
- **Betriebssystemunterstützung :**
  - Windows Server 2016
  - Windows Server 2012 R2
  - Windows Server 2012
  - Windows Server 2008 R2 SP1
  - Windows Client 7/8.1/10
  - Nano Server
- **WMF-Support:**
  - WMF 5.1
  - WMF 5.0 RTM
  - WMF 4.0 Update
  - WMF 4.0
- **Umgebung:**
  - Azure
  - Azure China 21Vianet
  - Azure Government
- **Hinweise:** In dieser Version wird DSC verwendet, das in Windows Server 2016 enthalten ist. Für andere Windows-Betriebssysteme wird [Windows Management Framework 5.1](https://devblogs.microsoft.com/powershell/wmf-5-1-releasing-january-2017/) installiert. (Bei Installation von WMF ist ein Neustart erforderlich.) Für Nano Server wird die DSC-Rolle auf der VM installiert.
- **Neue Features:**
  - Verbesserungen bei Erweiterungsmetadaten für den Unterstatus und andere kleinere Fehlerkorrekturen

## <a name="supported-versions"></a>Unterstützte Versionen

> [!WARNING]
> Die Versionen 2.4 bis 2.13 verwenden WMF 5.0 Public Preview, dessen Signaturzertifikate im August abgelaufen sind.
> 2016. Weitere Informationen zu diesem Thema finden Sie in diesem [Blogbeitrag](https://devblogs.microsoft.com/powershell/azure-dsc-extension-versions-2-4-up-to-2-13-will-retire-in-august/).

### <a name="version-275"></a>Version 2.75

- **Veröffentlichungsdatum:** 5. März 2018
- **Betriebssystemunterstützung :** Windows Server 2016, Windows Server 2012 R2, Windows Server 2012, Windows Server 2008 R2 SP1, Windows Client 7/8.1/10, Nano Server
- **WMF-Support:** WMF 5.1, WMF 5.0 RTM, WMF 4.0 Update, WMF 4.0
- **Umgebung:** Azure
- **Hinweise:** In dieser Version wird DSC verwendet, das in Windows Server 2016 enthalten ist. Für andere Windows-Betriebssysteme wird [Windows Management Framework 5.1](https://devblogs.microsoft.com/powershell/wmf-5-1-releasing-january-2017/) installiert. (Bei Installation von WMF ist ein Neustart erforderlich.) Für Nano Server wird die DSC-Rolle auf der VM installiert.
- **Neue Features:**
  - Nach der neuesten Aktualisierung von GitHub auf TLS 1.2 können Sie mithilfe der im Azure Marketplace verfügbaren DIY-Ressourcen-Manager-Vorlagen keine VM in Azure Automation DSC integrieren, und Sie können die DSC-Erweiterung nicht zum Hosten einer Konfiguration auf GitHub nutzen. Bei der Bereitstellung der Erweiterung erhalten Sie eine Fehlermeldung, die der im Folgenden beschriebenen Meldung ähnelt:

    ```json
    {
        "code": "DeploymentFailed",
        "message": "At least one resource deployment operation failed. Please list deployment operations for details. Please see https://aka.ms/arm-debug for usage details.",
        "details": [{
            "code": "Conflict",
            "message": "{
                \"status\": \"Failed\",
                \"error\": {
                    \"code\": \"ResourceDeploymentFailure\",
                    \"message\": \"The resource operation completed with terminal provisioning state 'Failed'.\",
                    \"details\": [ {
                        \"code\": \"VMExtensionProvisioningError\",
                        \"message\": \"VM has reported a failure when processing extension 'Microsoft.Powershell.DSC'.
                        Error message: \\\"The DSC Extension failed to execute: Error downloading
                        https://github.com/Azure/azure-quickstart-templates/raw/master/dsc-extension-azure-automation-pullserver/UpdateLCMforAAPull.zip
                        after 29 attempts: The request was aborted: Could not create SSL/TLS secure channel..\\nMore information about the failure can
                        be found in the logs located under 'C:\\\\WindowsAzure\\\\Logs\\\\Plugins\\\\Microsoft.Powershell.DSC\\\\2.74.0.0' on the VM.\\\".\"
                    } ]
                }
            }"
        }]
    }
    ```

  - In der neuen Erweiterungsversion wird TLS 1.2 jetzt erzwungen. Bei der Bereitstellung der Erweiterung wird automatisch auf Version 2.75 aktualisiert, wenn in Ihrer Resource Manager-Vorlage AutoUpgradeMinorVersion = true ist. Für manuelle Updates geben Sie `TypeHandlerVersion = 2.75` in Ihrer Resource Manager-Vorlage an.

### <a name="version-270---272"></a>Version 2.70–2.72

- **Veröffentlichungsdatum:** 13. November 2017
- **Betriebssystemunterstützung :** Windows Server 2016, Windows Server 2012 R2, Windows Server 2012, Windows Server 2008 R2 SP1, Windows Client 7/8.1/10, Nano Server
- **WMF-Support:** WMF 5.1, WMF 5.0 RTM, WMF 4.0 Update, WMF 4.0
- **Umgebung:** Azure
- **Hinweise:** In dieser Version wird DSC verwendet, das in Windows Server 2016 enthalten ist. Für andere Windows-Betriebssysteme wird [Windows Management Framework 5.1](https://devblogs.microsoft.com/powershell/wmf-5-1-releasing-january-2017/) installiert. (Bei Installation von WMF ist ein Neustart erforderlich.) Für Nano Server wird die DSC-Rolle auf der VM installiert.
- **Neue Features:**
  - Fehlerkorrekturen und Verbesserungen, die die Verwendung von DSC Azure Automation durch die Benutzeroberfläche im Portal und die Resource Manager-Vorlage verbessern. Weitere Informationen finden Sie im [Standardkonfigurationsskript](../virtual-machines/extensions/dsc-overview.md) in der DSC-Erweiterungsdokumentation.

### <a name="version-226"></a>Version 2.26

- **Veröffentlichungsdatum:** 9. Juni 2017
- **Betriebssystemunterstützung :** Windows Server 2016, Windows Server 2012 R2, Windows Server 2012, Windows Server 2008 R2 SP1, Windows Client 7/8.1/10, Nano Server
- **WMF-Support:** WMF 5.1, WMF 5.0 RTM, WMF 4.0 Update, WMF 4.0
- **Umgebung:** Azure
- **Hinweise:** In dieser Version wird DSC verwendet, das in Windows Server 2016 enthalten ist. Für andere Windows-Betriebssysteme wird [Windows Management Framework 5.1](https://devblogs.microsoft.com/powershell/wmf-5-1-releasing-january-2017/) installiert. (Bei Installation von WMF ist ein Neustart erforderlich.) Für Nano Server wird die DSC-Rolle auf der VM installiert.
- **Neue Features:**
  - Telemetrie-Verbesserungen

### <a name="version-225"></a>Version 2.25

- **Veröffentlichungsdatum:** 2. Juni 2017
- **Betriebssystemunterstützung :** Windows Server 2016, Windows Server 2012 R2, Windows Server 2012, Windows Server 2008 R2 SP1, Windows Client 7/8.1/10, Nano Server
- **WMF-Support:** WMF 5.1, WMF 5.0 RTM, WMF 4.0 Update, WMF 4.0
- **Umgebung:** Azure
- **Hinweise:** In dieser Version wird DSC verwendet, das in Windows Server 2016 enthalten ist. Für andere Windows-Betriebssysteme wird [Windows Management Framework 5.1](https://devblogs.microsoft.com/powershell/wmf-5-1-releasing-january-2017/) installiert. (Bei Installation von WMF ist ein Neustart erforderlich.) Für Nano Server wird die DSC-Rolle auf der VM installiert.
- **Neue Features:**
  - Es wurden verschiedene Fehlerkorrekturen und andere geringfügige Verbesserungen hinzugefügt.

### <a name="version-224"></a>Version 2.24

- **Veröffentlichungsdatum:** 13. April 2017
- **Betriebssystemunterstützung :** Windows Server 2016, Windows Server 2012 R2, Windows Server 2012, Windows Server 2008 R2 SP1, Nano Server
- **WMF-Support:** WMF 5.1, WMF 5.0 RTM, WMF 4.0 Update, WMF 4.0
- **Umgebung:** Azure
- **Hinweise:** In dieser Version wird DSC verwendet, das in Windows Server 2016 enthalten ist. Für andere Windows-Betriebssysteme wird [Windows Management Framework 5.1](https://devblogs.microsoft.com/powershell/wmf-5-1-releasing-january-2017/) installiert. (Bei Installation von WMF ist ein Neustart erforderlich.) Für Nano Server wird die DSC-Rolle auf der VM installiert.
- **Neue Features:**
  - Stellt VM UUID und DSC Agent-ID als Erweiterungsmetadaten zur Verfügung. Es wurden weitere geringfügige Verbesserungen hinzugefügt.

### <a name="version-223"></a>Version 2.23

- **Veröffentlichungsdatum:** 15. März 2017
- **Betriebssystemunterstützung :** Windows Server 2016, Windows Server 2012 R2, Windows Server 2012, Windows Server 2008 R2 SP1, Nano Server
- **WMF-Support:** WMF 5.1, WMF 5.0 RTM, WMF 4.0 Update, WMF 4.0
- **Umgebung:** Azure
- **Hinweise:** In dieser Version wird DSC verwendet, das in Windows Server 2016 enthalten ist. Für andere Windows-Betriebssysteme wird [Windows Management Framework 5.1](https://devblogs.microsoft.com/powershell/wmf-5-1-releasing-january-2017/) installiert. (Bei Installation von WMF ist ein Neustart erforderlich.) Für Nano Server wird die DSC-Rolle auf der VM installiert.
- **Neue Features:**
  - Einige Fehlerkorrekturen und andere Verbesserungen wurden hinzugefügt.

### <a name="version-222"></a>Version 2.22

- **Veröffentlichungsdatum:** 8. Februar 2017
- **Betriebssystemunterstützung :** Windows Server 2016, Windows Server 2012 R2, Windows Server 2012, Windows Server 2008 R2 SP1, Nano Server
- **WMF-Support:** WMF 5.1, WMF 5.0 RTM, WMF 4.0 Update, WMF 4.0
- **Umgebung:** Azure
- **Hinweise:** In dieser Version wird DSC verwendet, das in Windows Server 2016 enthalten ist. Für andere Windows-Betriebssysteme wird [Windows Management Framework 5.1](https://devblogs.microsoft.com/powershell/wmf-5-1-releasing-january-2017/) installiert. (Bei Installation von WMF ist ein Neustart erforderlich.) Für Nano Server wird die DSC-Rolle auf der VM installiert.
- **Neue Features:**
  - Die DSC-Erweiterung unterstützt jetzt WMF 5.1.
  - Es wurden weitere geringfügige Verbesserungen hinzugefügt.

### <a name="version-221"></a>Version 2.21

- **Veröffentlichungsdatum:** 2. Dezember 2016
- **Betriebssystemunterstützung :** Windows Server 2016, Windows Server 2012 R2, Windows Server 2012, Windows Server 2008 R2 SP1, Nano Server
- **WMF-Support:** WMF 5.1 Preview, WMF 5.0 RTM, WMF 4.0 Update, WMF 4.0
- **Umgebung:** Azure
- **Hinweise:** In dieser Version wird DSC verwendet, das in Windows Server 2016 enthalten ist. Für andere Windows-Betriebssysteme wird [Windows Management Framework 5.0 RTM](https://devblogs.microsoft.com/powershell/windows-management-framework-wmf-5-0-rtm-is-now-available-via-the-microsoft-update-catalog/) installiert. (Bei Installation von WMF ist ein Neustart erforderlich.) Für Nano Server wird die DSC-Rolle auf der VM installiert.
- **Neue Features:**
  - Die DSC-Erweiterung ist jetzt auf Nano Server verfügbar. Diese Version enthält vorwiegend Codeänderungen zur fehlerfreien Ausführung der Erweiterung auf Nano Server.
  - Es wurden weitere geringfügige Verbesserungen hinzugefügt.

### <a name="version-220"></a>Version 2.20

- **Veröffentlichungsdatum:** 2. August 2016
- **Betriebssystemunterstützung :** Windows Server 2016 Technical Preview, Windows Server 2012 R2, Windows Server 2012, Windows Server 2008 R2 SP1
- **WMF-Support:** WMF 5.1 Preview, WMF 5.0 RTM, WMF 4.0 Update, WMF 4.0
- **Umgebung:** Azure
- **Hinweise:** In dieser Version wird DSC verwendet, das in Windows Server 2016 Technical Preview enthalten ist. Für andere Windows-Betriebssysteme wird [Windows Management Framework 5.0 RTM](https://devblogs.microsoft.com/powershell/windows-management-framework-wmf-5-0-rtm-is-now-available-via-the-microsoft-update-catalog/) installiert. (Bei Installation von WMF ist ein Neustart erforderlich.)
- **Neue Features:**
  - Unterstützung für WMF 5.1 Preview Bei der ersten Veröffentlichung war diese Version ein optionales Upgrade, und zur Installation von WMF 5.1 Preview musste in der Resource Manager-Vorlage Wmfversion = „5.1PP“ angegeben werden.
    Wmfversion = „neueste Version“ installiert noch immer [WMF 5.0 RTM](https://devblogs.microsoft.com/powershell/windows-management-framework-wmf-5-0-rtm-is-now-available-via-the-microsoft-update-catalog/).
    Weitere Informationen zur WMF 5.1-Preview finden Sie in [diesem Blog](https://devblogs.microsoft.com/powershell/announcing-windows-management-framework-wmf-5-1-preview/).
  - Es wurden weitere geringfügige Korrekturen und Verbesserungen hinzugefügt.

### <a name="version--219"></a>Version 2.19

- **Veröffentlichungsdatum:** 3. Juni 2016
- **Betriebssystemunterstützung :** Windows Server 2016 Technical Preview, Windows Server 2012 R2, Windows Server 2012, Windows Server 2008 R2 SP1
- **WMF-Support:** WMF 5.0 RTM, WMF 4.0 Update, WMF 4.0
- **Umgebung:** Azure, Azure China 21Vianet, Azure Government
- **Hinweise:** In dieser Version wird DSC verwendet, das in Windows Server 2016 Technical Preview enthalten ist. Für andere Windows-Betriebssysteme wird [Windows Management Framework 5.0 RTM](https://devblogs.microsoft.com/powershell/windows-management-framework-wmf-5-0-rtm-is-now-available-via-the-microsoft-update-catalog/) installiert. (Bei Installation von WMF ist ein Neustart erforderlich.)
- **Neue Features:**
  - Die DSC-Erweiterung ist jetzt in Azure China 21Vianet eingegliedert. Diese Version enthält vorwiegend Korrekturen zur fehlerfreien Ausführung der Erweiterung in Azure China 21Vianet.

### <a name="version-218"></a>Version 2.18

- **Veröffentlichungsdatum:** 3. Juni 2016
- **Betriebssystemunterstützung :** Windows Server 2016 Technical Preview, Windows Server 2012 R2, Windows Server 2012, Windows Server 2008 R2 SP1
- **WMF-Support:** WMF 5.0 RTM, WMF 4.0 Update, WMF 4.0
- **Umgebung:** Azure
- **Hinweise:** In dieser Version wird DSC verwendet, das in Windows Server 2016 Technical Preview enthalten ist. Für andere Windows-Betriebssysteme wird [Windows Management Framework 5.0 RTM](https://devblogs.microsoft.com/powershell/windows-management-framework-wmf-5-0-rtm-is-now-available-via-the-microsoft-update-catalog/) installiert. (Bei Installation von WMF ist ein Neustart erforderlich.)
- **Neue Features:**
  - Stellen Sie die Telemetrie auf „nicht blockieren“, wenn während des Downloads des Telemetriehotfixes (bekannt als Azure DNS-Problem) oder während der Installation ein Fehler auftritt.
  - Korrektur des zeitweiligen Problems, bei dem die Erweiterung nach einem Neustart aufhört, die Konfiguration zu verarbeiten.
    Dies ist der Grund dafür, weshalb die DSC-Erweiterung den Status „Im Übergang“ beibehielt.
  - Es wurden weitere geringfügige Korrekturen und Verbesserungen hinzugefügt.

### <a name="version-217"></a>Version 2.17

- **Veröffentlichungsdatum:** 26. April 2016
- **Betriebssystemunterstützung :** Windows Server 2016 Technical Preview, Windows Server 2012 R2, Windows Server 2012, Windows Server 2008 R2 SP1
- **WMF-Support:** WMF 5.0 RTM, WMF 4.0 Update, WMF 4.0
- **Umgebung:** Azure
- **Hinweise:** In dieser Version wird DSC verwendet, das in Windows Server 2016 Technical Preview enthalten ist. Für andere Windows-Betriebssysteme wird [Windows Management Framework 5.0 RTM](https://devblogs.microsoft.com/powershell/windows-management-framework-wmf-5-0-rtm-is-now-available-via-the-microsoft-update-catalog/) installiert. (Bei Installation von WMF ist ein Neustart erforderlich.)
- **Neue Features:**
  - Unterstützung für WMF 4.0 Update Weitere Informationen zu WMF 4.0 Update finden Sie auf [diesem Blog](https://devblogs.microsoft.com/powershell/windows-management-framework-wmf-4-0-update-now-available-for-windows-server-2012-windows-server-2008-r2-sp1-and-windows-7-sp1/).
  - Wenden Sie bei Fehlern, die während der Installation der DSC-Erweiterung oder beim Anwenden einer DSC-Konfiguration nach der Erweiterungsinstallation auftreten, die Wiederholungslogik an. Wenn eine vorherige Installation oder das erneute Inkraftsetzen einer DSC-Konfiguration zuvor fehlschlug, versucht die Erweiterung als Teil dieser Veränderung, die Installation maximal drei Mal zu wiederholen, bis der Abschlussstatus (Erfolg/Fehler) erreicht wird oder eine neue Anforderung empfangen wird. Wenn bei der Erweiterung aufgrund von ungültigen Benutzereinstellungen oder Benutzereingaben ein Fehler auftritt, führt das Programm keine Wiederholung durch. In diesem Fall muss die Erweiterung erneut mit einer neuen Anforderung und korrekten Benutzereinstellungen aufgerufen werden. Hinweis: Die DSC-Erweiterung ist für die Wiederholungsversuche vom Azure VM-Agent abhängig. Der Azure VM-Agent ruft so lange die Erweiterung mit der letzten fehlerhaften Anforderung auf, bis ein Erfolgs- bzw. Fehlerstatus angezeigt wird.

### <a name="version-216"></a>Version 2.16

- **Veröffentlichungsdatum:** 21. April 2016
- **Betriebssystemunterstützung :** Windows Server 2016 Technical Preview, Windows Server 2012 R2, Windows Server 2012, Windows Server 2008 R2 SP1
- **WMF-Support:** WMF 5.0 RTM, WMF 4.0
- **Umgebung:** Azure
- **Hinweise:** In dieser Version wird DSC verwendet, das in Windows Server 2016 Technical Preview enthalten ist. Für andere Windows-Betriebssysteme wird [Windows Management Framework 5.0 RTM](https://devblogs.microsoft.com/powershell/windows-management-framework-wmf-5-0-rtm-is-now-available-via-the-microsoft-update-catalog/) installiert. (Bei Installation von WMF ist ein Neustart erforderlich.)
- **Neue Features:**
  - Verbesserung bei der Fehlerbehandlung und andere geringfügige Fehlerkorrekturen.
  - Neue Eigenschaft in den DSC-Erweiterungseinstellungen In AdvancedOptions wird „ForcePullAndApply“ hinzugefügt, damit die DSC-Erweiterung DSC-Konfigurationen in Kraft setzen kann, wenn „Pullmodus“ der Aktualisierungsmodus ist (im Gegensatz zum standardmäßigen „Pushmodus“). Weitere Informationen zu den Einstellungen der DSC-Erweiterung finden Sie in [diesem Blog](https://devblogs.microsoft.com/powershell/arm-dsc-extension-settings/).

### <a name="version-215"></a>Version 2.15

- **Veröffentlichungsdatum:** 14. März 2016
- **Betriebssystemunterstützung :** Windows Server 2016 Technical Preview, Windows Server 2012 R2, Windows Server 2012, Windows Server 2008 R2 SP1
- **WMF-Support:** WMF 5.0 RTM, WMF 4.0
- **Umgebung:** Azure
- **Hinweise:** In dieser Version wird DSC verwendet, das in Windows Server 2016 Technical Preview enthalten ist. Für andere Windows-Betriebssysteme wird [Windows Management Framework 5.0 RTM](https://devblogs.microsoft.com/powershell/windows-management-framework-wmf-5-0-rtm-is-now-available-via-the-microsoft-update-catalog/) installiert. (Bei Installation von WMF ist ein Neustart erforderlich.)
- **Neue Features:**
  - In der Erweiterungsversion 2.14 wurden Veränderungen bezüglich der Installation von WMF RTM vorgenommen. Beim Upgrade der Erweiterungsversion von 2.13.2.0 auf 2.14.0.0 bemerken Sie möglicherweise, dass einige DSC-Cmdlets fehlschlagen oder bei der Konfiguration die Fehlermeldung „No Instance found with given property values“ (Es wurde keine Instanz mit den Eigenschaftswerten gefunden) angezeigt wird. Weitere Informationen finden Sie in den [DSC release notes (Anmerkungen zu DSC)](/powershell/scripting/wmf/known-issues/known-issues-dsc). Die Problemumgehungen für diese Probleme wurden in Version 2.15 hinzugefügt.
  - Wenn Sie die Version 2.14 schon installiert haben und eines der beiden oben genannten Probleme auftritt, müssen Sie diese Schritte manuell ausführen. In einer PowerShell-Sitzung mit erhöhten Rechten:
    - `Remove-Item -Path $env:SystemRoot\system32\Configuration\DSCEngineCache.mof`
    - `mofcomp $env:windir\system32\wbem\DscCoreConfProv.mof`

### <a name="version-214"></a>Version 2.14

- **Veröffentlichungsdatum:** 25. Februar 2016
- **Betriebssystemunterstützung :** Windows Server 2016 Technical Preview, Windows Server 2012 R2, Windows Server 2012, Windows Server 2008 R2 SP1
- **WMF-Support:** WMF 5.0 RTM, WMF 4.0
- **Umgebung:** Azure
- **Hinweise:** In dieser Version wird DSC verwendet, das in Windows Server 2016 Technical Preview enthalten ist. Für andere Windows-Betriebssysteme wird [Windows Management Framework 5.0 RTM](https://devblogs.microsoft.com/powershell/windows-management-framework-wmf-5-0-rtm-is-now-available-via-the-microsoft-update-catalog/) installiert. (Bei Installation von WMF ist ein Neustart erforderlich.)
- **Neue Features:**
  - Verwendet WMF RTM
  - Aktiviert die Datensammlung, um die Qualität der DSC-Erweiterung zu verbessern. Weitere Informationen finden Sie in diesem [Blog](https://devblogs.microsoft.com/powershell/azure-dsc-extension-data-collection-2/).
  - Stellt ein aktualisiertes Einstellungsformat für die Erweiterung in einer Resource Manager-Vorlage bereit. Weitere Informationen finden Sie in diesem [Blog](https://devblogs.microsoft.com/powershell/arm-dsc-extension-settings/).
  - Fehlerkorrekturen und andere Verbesserungen

## <a name="next-steps"></a>Nächste Schritte

- Weitere Informationen zu PowerShell DSC finden Sie im [PowerShell-Dokumentationscenter](/powershell/scripting/dsc/overview/overview).
- Überprüfen Sie die [Resource Manager-Vorlage für die DSC-Erweiterung](../virtual-machines/extensions/dsc-template.md).
- Weitere Funktionen und Ressourcen, die Sie mit PowerShell DSC verwalten können, finden Sie im [PowerShell-Katalog](https://www.powershellgallery.com/packages?q=DscResource&x=0&y=0).
- Informationen zur Weitergabe von sensiblen Parametern in Konfigurationen finden Sie unter [Übergeben von Anmeldeinformationen an den Azure DSC-Erweiterungshandler](../virtual-machines/extensions/dsc-credentials.md).
