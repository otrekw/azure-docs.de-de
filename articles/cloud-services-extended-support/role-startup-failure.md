---
title: Fehler beim Starten der Rolleninstanz in Azure Cloud Services (erweiterter Support)
description: Problembehandlung für Fehler beim Starten der Rolleninstanz in Azure Cloud Services (erweiterter Support).
ms.topic: article
ms.service: cloud-services-extended-support
author: surbhijain
ms.author: surbhijain
ms.reviewer: gachadw
ms.date: 04/01/2021
ms.custom: ''
ms.openlocfilehash: 9c6fdee8dbb28e86e5084c9c0cfca97f5ac5ea05
ms.sourcegitcommit: fc9fd6e72297de6e87c9cf0d58edd632a8fb2552
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/30/2021
ms.locfileid: "108287156"
---
# <a name="troubleshoot-azure-cloud-services-extended-support-roles-that-fail-to-start"></a>Behandeln von Problemen mit Rollen in Azure Cloud Services (erweiterter Support), die nicht gestartet werden

In diesem Artikel finden Sie Informationen zu allgemeinen Problemen im Zusammenhang mit Rollen in Azure Cloud Services (erweiterter Support), die nicht gestartet werden, sowie entsprechende Lösungen.

## <a name="cloud-service-operation-fails-with-roleinstancestartuptimeouterror"></a>Nicht erfolgreicher Clouddienstvorgang mit Fehler „RoleInstanceStartupTimeoutError“

Einzelne oder mehrere Ihrer Rolleninstanzen in Azure Cloud Services (erweiterter Support) starten möglicherweise langsam, oder sie werden wiederverwendet, und für die Rolleninstanz tritt ein Fehler auf. Der Rollenanwendungsfehler `RoleInstanceStartupTimeoutError` wird angezeigt.

Die Rollenanwendung enthält zwei Komponenten, die ggf. zur Wiederverwendung einer Rolle führen: *Starttasks* und *Rollencode (Implementierung von „RoleEntryPoint“)* . 

Wenn die Rolle beendet wird, wird sie vom PaaS-Agent (Platform-as-a-Service) neu gestartet.

Sie können den aktuellen Zustand sowie Details einer Rolleninstanz mithilfe von PowerShell oder über das Azure-Portal ermitteln, um Fehler zu diagnostizieren:

* **PowerShell:** Verwenden Sie das Cmdlet [Get-AzCloudServiceRoleInstanceView](/powershell/module/az.cloudservice/get-azcloudserviceroleinstanceview), um Informationen zum Laufzeitzustand der Rolleninstanz abzurufen:

    ```powershell
    Get-AzCloudServiceRoleInstanceView -ResourceGroupName "ContosOrg" -CloudServiceName "ContosoCS" -RoleInstanceName "WebRole1_IN_0"
     
    Statuses           PlatformFaultDomain PlatformUpdateDomain
    --------           ------------------- --------------------
    {RoleStateStarting} 0                   0
    ```

* **Azure-Portal:** Navigieren Sie im Portal zur Clouddienstinstanz. Wählen Sie zum Anzeigen der Statusdetails die Option **Rollen und Instanzen** und anschließend die Rolleninstanz aus.

  :::image type="content" source="media/role-startup-failure-portal.png" alt-text="Screenshot: Rollenstartfehler im Azure-Portal":::

## <a name="missing-dlls-or-dependencies"></a>Fehlende DLLs oder Abhängigkeiten

Nicht reagierende Rollen oder Rollen, die zwischen Zuständen pendeln, können durch fehlende DLLs oder Assemblys verursacht werden.

Im Anschluss finden Sie einige Symptome bei fehlenden DLLs oder Assemblys:

* Ihre Rolleninstanz durchläuft die Zustände **Wird initialisiert**, **Ausgelastet** und **Wird beendet**.
* Die Rolleninstanz befindet sich im Zustand **Bereit**, wenn Sie jedoch zu Ihrer Webanwendung navigieren, wird die Seite nicht angezeigt.


Auf einer in einer Webrolle bereitgestellten Website mit fehlender DLL wird möglicherweise der folgende Serverlaufzeitfehler angezeigt:

  :::image type="content" source="media/role-startup-failure-runtime-error.png" alt-text="Screenshot: Laufzeitfehler nach einem Rollenstartfehler":::

### <a name="resolve-missing-dlls-and-assemblies"></a>Beheben von Fehlern aufgrund von fehlenden DLLs und Assemblys

So beheben Sie Fehler aufgrund von fehlenden DLLs und Assemblys:

1. Öffnen Sie die Projektmappe in Visual Studio.
2. Öffnen Sie im Projektmappen-Explorer den Ordner *Verweise*.
3. Wählen Sie die im Fehler angegebene Assembly aus.
4. Legen Sie unter **Eigenschaften** die Eigenschaft **Lokale Kopie** auf **True** fest.
5. Stellen Sie den Clouddienst erneut bereit.

Vergewissern Sie sich, dass die Fehler nicht mehr auftreten, und stellen Sie den Dienst anschließend erneut bereit. Lassen Sie beim Einrichten der Bereitstellung das Kontrollkästchen **IntelliTrace für .NET 4-Rollen aktivieren** deaktiviert.

## <a name="diagnose-role-instance-errors"></a>Diagnostizieren von Rolleninstanzfehlern

Verwenden Sie eine der folgenden Optionen, um Probleme mit Rolleninstanzen zu diagnostizieren:

### <a name="turn-off-custom-errors"></a>Deaktivieren benutzerdefinierter Fehler

Legen Sie in der Datei *web.config* für die Webrolle den benutzerdefinierten Fehlermodus auf `off` fest, und stellen Sie den Dienst anschließend erneut zur bereit, um vollständige Fehlerinformationen anzuzeigen:

1. Öffnen Sie die Projektmappe in Visual Studio.
2. Öffnen Sie im Projektmappen-Explorer die Datei *web.config*.
3. Fügen Sie im Abschnitt `system.web` den folgenden Code hinzu:

   ```xml
   <customErrors mode="Off" />
   ```

4. Speichern Sie die Datei .
5. Packen Sie den Dienst neu, und stellen Sie ihn erneut bereit.

Nach erneuter Bereitstellung des Diensts enthält eine Fehlermeldung den Namen fehlender Assemblys oder DLLs.

### <a name="use-remote-desktop"></a>Verwenden von Remotedesktop

Verwenden Sie Remotedesktop, um auf die Rolle zuzugreifen und vollständige Fehlerinformationen anzuzeigen:

1. [Fügen Sie die Remotedesktoperweiterung für Azure Cloud Services (erweiterter Support) hinzu.](enable-rdp.md)
2. Wenn im Azure-Portal für die Clouddienstinstanz der Status **Bereit** angezeigt wird, melden Sie sich per Remotedesktop beim Clouddienst an. Weitere Informationen finden Sie unter [Herstellen einer Verbindung mit Rolleninstanzen mit aktiviertem Remotedesktop](enable-rdp.md#connect-to-role-instances-with-remote-desktop-enabled).
3. Melden Sie sich bei dem virtuellen Computer mit den Anmeldeinformationen an, die Sie bei der Remotedesktopeinrichtung verwendet haben.
4. Öffnen Sie ein Eingabeaufforderungsfenster.
5. Geben Sie an der Eingabeaufforderung **ipconfig** ein. Notieren Sie sich den Rückgabewert für die IPv4-Adresse.
6. Öffnen Sie Microsoft Internet Explorer.
7. Geben Sie nacheinander die IPv4-Adresse, einen Schrägstrich und den Namen der Standarddatei der Webanwendung in die Adressleiste ein. Beispiel: `http://<IPv4 address>/default.aspx`.

Wenn Sie nun zu der Website navigieren, werden Fehlermeldungen mit weiteren Informationen angezeigt. Hier sehen Sie ein Beispiel:

:::image type="content" source="media/role-startup-failure-error-message.png" alt-text="Screenshot: Beispiel einer Fehlermeldung":::
  
### <a name="use-the-compute-emulator"></a>Verwenden des Serveremulators

Sie können den Azure-Serveremulator verwenden, um Probleme mit fehlenden Abhängigkeiten und Fehler im Zusammenhang mit *web.config* zu diagnostizieren und zu beheben. Bei dieser Diagnosemethode erzielen Sie die besten Ergebnisse, wenn Sie einen physischen oder virtuellen Computer mit einer neuen Windows-Installation verwenden.

So diagnostizieren Sie Probleme mithilfe des Azure-Serveremulators:

1. Installieren Sie das [Azure SDK](https://azure.microsoft.com/downloads/).
2. Erstellen Sie auf dem Entwicklungscomputer das Clouddienstprojekt.
3. Navigieren Sie im Datei-Explorer im Clouddienstprojekt zum Ordner *bin\debug*.
4. Kopieren Sie den *CSX-Ordner* und die *CSCFG-Datei* auf den Computer, den Sie zum Debuggen von Problemen verwenden.
5. Öffnen Sie auf dem Computer mit der neuen Installation ein Azure SDK-Eingabeaufforderungsfenster.
6. Geben Sie an der Eingabeaufforderung **csrun.exe /devstore:start** ein.
7. Geben Sie dann **run csrun \<path to .csx folder\> \<path to .cscfg file\> /launchBrowser** ein.
8. Wenn die Rolle gestartet wird, werden in Internet Explorer detaillierte Fehlerinformationen angezeigt.

Sollten weitere Diagnoseschritte erforderlich sein, können Sie standardmäßige Windows-Problembehandlungstools verwenden.

### <a name="use-intellitrace"></a>Verwenden von IntelliTrace

Für Worker- und Webrollen mit .NET Framework 4 können Sie [IntelliTrace](/visualstudio/debugger/intellitrace) verwenden. IntelliTrace steht in Visual Studio Enterprise zur Verfügung.

So stellen Sie Ihren Clouddienst mit aktiviertem IntelliTrace bereit:

1. Vergewissern Sie sich, dass Azure SDK 1.3 oder höher installiert ist.
2. Stellen Sie die Projektmappe in Visual Studio bereit. Aktivieren Sie beim Einrichten der Bereitstellung das Kontrollkästchen **IntelliTrace für .NET 4-Rollen aktivieren**.
3. Öffnen Sie nach dem Start der Rolleninstanz den Server-Explorer.
4. Erweitern Sie den Knoten **Azure\Cloud Services**.
5. Erweitern Sie die Bereitstellung, um Rolleninstanzen aufzulisten. Klicken Sie mit der rechten Maustaste auf eine Rolleninstanz.
6. Wählen Sie **IntelliTrace-Protokolle anzeigen** aus.
7. Navigieren Sie in der **IntelliTrace-Zusammenfassung** zu **Ausnahmedaten**.
8. Erweitern Sie **Ausnahmedaten**, und suchen Sie nach einem Fehler vom Typ `System.IO.FileNotFoundException`:

   :::image type="content" source="media/role-startup-failure-exception-data.png" alt-text="Screenshot: Ausnahmedaten für einen Rollenstartfehler" lightbox="media/role-startup-failure-exception-data.png":::

## <a name="next-steps"></a>Nächste Schritte

- Erfahren Sie, wie Sie [Probleme mit Clouddienstrollen mithilfe von Computediagnosedaten in Azure PaaS beheben](/archive/blogs/kwill/windows-azure-paas-compute-diagnostics-data).