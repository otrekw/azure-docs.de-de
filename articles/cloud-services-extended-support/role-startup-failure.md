---
title: Fehler beim Starten der Rolleninstanz für Azure Cloud Services (erweiterter Support)
description: Problembehandlung des Fehlers beim Starten der Rolleninstanz für Azure Cloud Services (erweiterter Support)
ms.topic: article
ms.service: cloud-services-extended-support
author: surbhijain
ms.author: surbhijain
ms.reviewer: gachadw
ms.date: 04/01/2021
ms.custom: ''
ms.openlocfilehash: ced7675350c0e0deadf77837cf0f13ba54fffab9
ms.sourcegitcommit: 77d7639e83c6d8eb6c2ce805b6130ff9c73e5d29
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/05/2021
ms.locfileid: "106382354"
---
# <a name="troubleshoot-azure-cloud-service-extended-support-roles-that-fail-to-start"></a>Problembehandlung bei Azure-Clouddienstrollen (erweiterter Support), die nicht gestartet werden
In diesem Artikel finden Sie Informationen zu allgemeinen Problemen in Zusammenhang mit nicht gestarteten Clouddienstrollen (erweiterter Support) sowie zur Lösung dieser Probleme.

## <a name="cloud-service-operation-failed-with-roleinstancestartuptimeouterror"></a>Fehler beim Clouddienstvorgang mit RoleInstanceStartupTimeoutError
Mindestens eine Rolleninstanz Ihres Clouddiensts (erweiterter Support) kann nicht in der festgelegten Zeit gestartet werden. Diese Rolleninstanzen könnten eine gewisse Zeit zum Starten in Anspruch nehmen oder wiederverwendet werden, und bei der Rolleninstanz könnte ein RoleInstanceStartupTimeoutError-Fehler auftreten. Dies ist ein Rollenanwendungsfehler. Die Rollenanwendung enthält zwei Hauptbestandteile: „Starttasks“ und „Rollencode (Implementierung von RoleEntryPoint)“, die beide dazu führen könnten, dass die Rolle wiederverwendet wird. Wenn die Rolle abstürzt, wird sie vom PaaS-Agent immer neu gestartet. 

So können Sie beim Auftreten von Fehlern den aktuellen Status und die Details der Rolleninstanzen abrufen:

* PowerShell: Rufen Sie mit dem Cmdlet [Get-AzCloudServiceRoleInstanceView](https://docs.microsoft.com/powershell/module/az.cloudservice/get-azcloudserviceroleinstanceview) Informationen über den Laufzeitstatus einer Rolleninstanz in einem Clouddienst ab.
```powershell
Get-AzCloudServiceRoleInstanceView -ResourceGroupName "ContosOrg" -CloudServiceName "ContosoCS" -RoleInstanceName "WebRole1_IN_0"
 
Statuses           PlatformFaultDomain PlatformUpdateDomain
--------           ------------------- --------------------
{RoleStateStarting} 0                   0
```

* Azure-Portal: Wechseln Sie zu Ihrem Clouddienst, und wählen Sie die Registerkarte „Rollen und Instanzen“ aus. Klicken Sie auf die Rolleninstanz, um die Statusdetails anzuzeigen. 
 
   :::image type="content" source="media/role-startup-failure-1.png" alt-text="Die Abbildung zeigt einen Rollenstartfehler im Portal.":::
   
Im Folgenden finden Sie einige häufige Probleme und Lösungen im Zusammenhang mit Azure Cloud Services-Rollen (erweiterter Support), die nicht gestartet werden können oder zwischen den Zuständen Initialisieren, Ausgelastet und Anhalten zirkulieren.

## <a name="missing-dlls-or-dependencies"></a>Fehlende DLLs oder Abhängigkeiten
Nicht reagierende Rollen oder Rollen, die sich in einer Schleife zwischen den Status Wird initialisiert, Ausgelastet und Wird beendet befinden, können durch fehlende DLLs oder Assemblys verursacht werden.
Folgende Symptome können auf fehlende DLLs oder Assemblys hinweisen:

* Ihre Rolleninstanz durchläuft die Status **Wird initialisiert**, **Ausgelastet** und **Wird beendet**.
* Die Rolleninstanz befindet sich im Status **Bereit** , aber wenn Sie zu Ihrer Webanwendung navigieren, wird die Seite nicht angezeigt.

Zur Untersuchung dieser Probleme gibt es mehrere empfohlene Methoden.

## <a name="diagnose-missing-dll-issues-in-a-web-role"></a>Diagnostizieren von Problemen mit fehlenden DLLs in einer Webrolle
Wenn Sie zu einer Website navigieren, die in einer Webrolle bereitgestellt wird, und im Browser ein Serverfehler wie der folgende Fehler angezeigt wird, kann dies darauf hinweisen, dass eine DLL fehlt.

:::image type="content" source="media/role-startup-failure-2.png" alt-text="Die Abbildung zeigt einen Laufzeitfehler bei einem Rollenstartfehler.":::

## <a name="diagnose-issues-by-turning-off-custom-errors"></a>Diagnostizieren von Problemen durch Deaktivieren von benutzerdefinierten Fehlern
Sie können umfassendere Fehlerinformationen anzeigen, indem Sie in der web.config-Datei für die Webrolle den Modus für benutzerdefinierte Fehler deaktivieren und den Dienst erneut bereitstellen.
So zeigen Sie umfassendere Fehlerinformationen an, ohne Remotedesktop zu verwenden:
1.  Öffnen Sie die Projektmappe in Microsoft Visual Studio.
2.  Suche Sie im Projektmappen-Explorer die Datei web.config, und öffnen Sie sie.
3.  Suchen Sie in der web.config-Datei den Abschnitt „system.web“, und fügen Sie folgende Zeile hinzu:
 ```xml
<customErrors mode="Off" />
```
4.  Speichern Sie die Datei .
5.  Packen Sie den Dienst neu, und stellen Sie ihn erneut bereit.
Nachdem der Dienst neu bereitgestellt wurde, sehen Sie eine Fehlermeldung mit dem Namen der fehlenden Assembly oder DLL.

## <a name="diagnose-issues-by-viewing-the-error-remotely"></a>Diagnostizieren von Problemen durch Remoteanzeige des Fehlers
Sie können Remotedesktop verwenden, um remote auf die Rolle zuzugreifen und vollständige Fehlerinformationen anzuzeigen. Gehen Sie wie folgt vor, um Fehler mithilfe von Remotedesktop anzuzeigen:
1.  Aktivieren Sie die Remotedesktoperweiterung für den Clouddienst (erweiterter Support). Weitere Informationen finden Sie unter [Anwenden der Remotedesktoperweiterung auf Cloud Services (erweiterter Support) über das Azure-Portal](enable-rdp.md).
2.  Wenn die Instanz im Microsoft Azure-Portal den Status „Bereit“ zeigt, stellen Sie eine Remoteverbindung mit der Instanz her. Weitere Informationen zur Verwendung des Remotedesktops mit Cloud Services (erweiterter Support) finden Sie unter [Herstellen einer Verbindung mit Rolleninstanzen mit aktiviertem Remotedesktop](https://docs.microsoft.com/azure/cloud-services-extended-support/enable-rdp#connect-to-role-instances-with-remote-desktop-enabled)
3.  Melden Sie sich mit den Anmeldeinformationen, die während der Remotedesktopkonfiguration angegeben wurden, an der virtuellen Maschine an.
4.  Öffnen Sie ein Befehlsfenster.
5.  Geben Sie "Ipconfig" ein.
6.  Notieren Sie sich den Wert der IPv4-Adresse.
7.  Öffnen Sie Internet Explorer.
8.  Geben Sie die Adresse und den Namen der Webanwendung ein. Beispielsweise „http://<IPV4 Address>/default.aspx“.
Wenn Sie zur Website navigieren, werden nun ausführlichere Fehlermeldungen zurückgegeben:
* Serverfehler in Anwendung '/'.
* Beschreibung: Unbehandelte Ausnahme beim Ausführen der aktuellen Webanforderung. Überprüfen Sie die Stapelüberwachung, um weitere Informationen über diesen Fehler anzuzeigen und festzustellen, wo der Fehler im Code verursacht wurde.
* Ausnahmedetails: System.IO.FIleNotFoundException: Die Datei oder Assembly  "Microsoft.WindowsAzure.StorageClient, Version=1.1.0.0, Culture=neutral, PublicKeyToken=31bf856ad364e35" oder eine Abhängigkeit davon wurde nicht gefunden. Die angegebene Datei wurde nicht gefunden.
Beispiel:

  :::image type="content" source="media/role-startup-failure-3.png" alt-text="Die Abbildung zeigt eine Ausnahme bei einem Rollenstartfehler.":::
  
## <a name="diagnose-issues-by-using-the-compute-emulator"></a>Diagnostizieren von Problemen mit dem Serveremulator
Sie können den Azure-Serveremulator verwenden, um Probleme mit fehlenden Abhängigkeiten sowie web.config-Fehler zu diagnostizieren und zu beheben.
Bei dieser Diagnosemethode erzielen Sie die besten Ergebnisse, wenn Sie einen physischen oder virtuellen Computer mit einer neuen Windows-Installation verwenden. 
1.  Installieren des [Azure SDK](https://azure.microsoft.com/downloads/) 
2.  Erstellen Sie das Clouddienstprojekt auf dem Entwicklungscomputer.
3.  Navigieren Sie im Windows-Explorer zum Ordner „bin\debug“ des Clouddienstprojekts.
4.  Kopieren Sie den CSX-Ordner und die CSCFG-Datei auf den Computer, den Sie zum Debuggen von Problemen verwenden.
5.  Öffnen Sie auf dem Computer mit Neuinstallation von Windows eine Azure SDK-Eingabeaufforderung, und geben Sie „csrun.exe /devstore:start“ ein.
6.  Geben Sie an der Eingabeaufforderung „run csrun <Pfad zum CSX-Ordner> <Pfad zur CSCFG-Datei> /launchBrowser“ ein.
7.  Wenn die Rolle gestartet wird, werden detaillierte Fehlerinformationen in Internet Explorer angezeigt. Für eine eingehendere Problemdiagnose können Sie auch standardmäßige Windows-Tools zur Problembehandlung verwenden.

## <a name="diagnose-issues-by-using-intellitrace"></a>Diagnostizieren von Problemen mit IntelliTrace
Für Worker- und Webrollen, die .NET Framework 4 verwenden, können Sie [IntelliTrace](https://docs.microsoft.com/visualstudio/debugger/intellitrace) nutzen. Diese Funktion steht in Microsoft Visual Studio Enterprise zur Verfügung.
Gehen Sie folgendermaßen vor, um den Dienst mit aktiviertem IntelliTrace bereitzustellen:
1.  Vergewissern Sie sich, dass Azure SDK 1.3 oder höher installiert ist.
2.  Stellen Sie die Projektmappe mit Visual Studio bereit. Aktivieren Sie bei der Bereitstellung das Kontrollkästchen IntelliTrace für .NET 4-Rollen aktivieren .
3.  Öffnen Sie den Server-Explorer, nachdem die Instanz gestartet wurde.
4.  Erweitern Sie den Knoten „Azure\Cloud Services“, und suchen Sie die Bereitstellung.
5.  Erweitern Sie die Bereitstellung, bis die Rolleninstanzen angezeigt werden. Klicken Sie mit der rechten Maustaste auf eine der Instanzen.
6.  Wählen Sie IntelliTrace-Protokolle anzeigen aus. Die IntelliTrace-Zusammenfassung wird geöffnet.
7.  Suchen Sie in der Zusammenfassung den Abschnitt mit den Ausnahmen. Wenn Ausnahmen vorhanden sind, ist der Abschnitt mit Ausnahmedatengekennzeichnet.
8.  Erweitern Sie die Ausnahmedaten, und suchen Sie System.IO.FileNotFoundException-Fehler ähnlich den folgenden:

    :::image type="content" source="media/role-startup-failure-4.png" alt-text="Die Abbildung zeigt Ausnahmedaten bei einem Rollenstartfehler." lightbox="media/role-startup-failure-4.png":::

## <a name="address-missing-dlls-and-assemblies"></a>Behandeln von Problemen mit fehlenden DLLs und Assemblys
Um Fehler mit fehlenden DLLs und Assemblys zu beheben, gehen Sie folgendermaßen vor:
1.  Öffnen Sie die Projektmappe in Visual Studio.
2.  Öffnen Sie im Projektmappen-Explorer den Ordner Verweise.
3.  Klicken Sie auf die im Fehler angegebene Assembly.
4.  Suchen Sie im Bereich Eigenschaften die Eigenschaft Lokale Kopie, und legen Sie den Wert auf True fest.
5.  Stellen Sie den Clouddienst erneut bereit.
Nachdem Sie bestätigt haben, dass alle Fehler korrigiert wurden, können Sie den Dienst bereitstellen, ohne das Kontrollkästchen IntelliTrace für .NET 4-Rollen aktivieren zu aktivieren.

## <a name="next-steps"></a>Nächste Schritte 
- Erfahren Sie in der [Blogreihe von Kevin Williamson](https://docs.microsoft.com/archive/blogs/kwill/windows-azure-paas-compute-diagnostics-data), wie Sie Probleme bei Clouddienstrollen mit den Compute-Diagnosedaten von Azure-PaaS beheben.
