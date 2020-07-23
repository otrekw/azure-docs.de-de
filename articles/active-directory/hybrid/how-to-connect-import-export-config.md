---
title: Importieren und Exportieren von Azure AD Connect-Konfigurationseinstellungen
description: Dieses Dokument enthält häufig gestellte Fragen zur Cloudbereitstellung.
services: active-directory
author: billmath
manager: daveba
ms.service: active-directory
ms.workload: identity
ms.topic: how-to
ms.date: 06/25/2020
ms.subservice: hybrid
ms.author: billmath
ms.collection: M365-identity-device-management
ms.openlocfilehash: a13236294f74bbe4bdaf8c1a30408afad09d9796
ms.sourcegitcommit: f844603f2f7900a64291c2253f79b6d65fcbbb0c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/10/2020
ms.locfileid: "86224973"
---
# <a name="importing-and-exporting-azure-ad-connect-configuration-settings-public-preview"></a>Importieren und Exportieren von Azure AD Connect-Konfigurationseinstellungen (öffentliche Vorschau) 

Azure AD Connect-Bereitstellungen variieren zwischen Installationen im Express-Modus mit einer einzigen Gesamtstruktur und komplexen Bereitstellungen, die anhand benutzerdefinierter Synchronisierungsregeln über mehrere Gesamtstrukturen synchronisiert werden. Aufgrund der großen Anzahl von Konfigurationsoptionen und -mechanismen ist es von entscheidender Bedeutung, dass Sie über die wirksamen Einstellungen informiert und schnell dazu in der Lage sind, einen Server mit einer identischen Konfiguration bereitzustellen. Mit diesem Feature wird die Möglichkeit eingeführt, die Konfiguration eines bestimmten Synchronisierungsservers zu katalogisieren und die Einstellungen in eine neue Bereitstellung zu importieren. Unterschiedliche Momentaufnahmen der Synchronisierungseinstellungen können verglichen werden, um die Unterschiede zwischen zwei Servern oder zwischen zwei Zuständen desselben Servers zu unterschiedlichen Zeitpunkten zu visualisieren. 

Bei jeder Konfigurationsänderung über den Azure AD Connect-Assistenten wird automatisch eine neue JSON-Einstellungsdatei mit Zeitstempel in  **%ProgramData%\AADConnect** exportiert. Der Name der Einstellungsdatei weist das Format **Applied-SynchronizationPolicy-*.JSON** auf, wobei der letzte Teil des Dateinamens ein Zeitstempel ist. 

>[!IMPORTANT]
> Nur von Azure AD Connect vorgenommene Änderungen werden automatisch exportiert. Alle Änderungen, die mit PowerShell, Synchronization Service Manager oder dem Synchronisierungsregel-Editor vorgenommen wurden, müssen nach Bedarf exportiert werden, um eine aktuelle Kopie zu erhalten. Der Export nach Bedarf kann auch verwendet werden, um eine Kopie der Einstellungen zur Notfallwiederherstellung an einem sicheren Ort aufzubewahren. 

## <a name="exporting-azure-ad-connect-settings"></a>Exportieren von Azure AD Connect-Einstellungen 

Um eine Zusammenfassung der Konfigurationseinstellungen anzuzeigen, öffnen Sie das Azure AD Connect-Tool, und wählen Sie die zusätzliche Aufgabe namens „Aktuelle Konfiguration anzeigen oder exportieren“ aus. Eine kurze Zusammenfassung der Einstellungen wird angezeigt, und Sie erhalten die Möglichkeit zum Exportieren der vollständigen Konfiguration Ihres Servers. 

Standardmäßig werden die Einstellungen in **%ProgramData%\AADConnect** exportiert. Sie können die Einstellungen jedoch auch an einem geschützten Ort speichern, damit sie im Notfall verfügbar sind. Einstellungen werden im JSON-Dateiformat exportiert und dürfen zur Gewährleistung logischer Konsistenz nicht manuell erstellt oder bearbeitet werden. Das Importieren manuell erstellter oder bearbeiteter Dateien wird nicht unterstützt und kann zu unerwarteten Ergebnissen führen. 

## <a name="importing-azure-ad-connect-settings"></a>Importieren von Azure AD Connect-Einstellungen 

Gehen Sie folgendermaßen vor, um zuvor exportierte Einstellungen zu importieren:
 
1. Installieren Sie **Azure AD Connect** auf einem neuen Server. 
2. Wählen Sie nach der **Homepage** die Option **Anpassen** aus. 
3. Klicken Sie auf **Synchronisierungseinstellungen importieren**. Suchen Sie nach der zuvor exportierten JSON-Einstellungsdatei.  
4. Klicken Sie auf **Installieren**.

![Installieren von Komponenten](media/how-to-connect-import-export-config/import1.png)

> [!NOTE]
> Überschreiben Sie die Einstellungen auf dieser Seite, beispielsweise zur Verwendung von SQL Server anstelle von LocalDB oder zur Verwendung eines vorhandenen Dienstkontos anstelle des Standard-VSA usw. Diese Einstellungen werden nicht aus der Konfigurationseinstellungsdatei importiert, sind jedoch zu Informations- und Vergleichszwecken enthalten.

### <a name="import-installation-experience"></a>Importinstallationsoberfläche 

Die Oberfläche zur Importinstallation ist mit einem Mindestmaß an Benutzereingaben absichtlich einfach gehalten, um die Reproduktion eines vorhandenen Servers zu erleichtern.  

Im Folgenden sind die einzigen Änderungen aufgeführt, die während der Installation vorgenommen werden können. Alle übrigen Änderungen können nach der Installation über den Azure AD Connect-Assistenten durchgeführt werden: 
- **Azure Active Directory-Anmeldeinformationen** : Standardmäßig wird der Kontoname des globalen Azure-Administrators vorgeschlagen, der den ursprünglichen Server konfiguriert hat. Dieser  **MUSS** geändert werden, wenn Sie Informationen mit einem neuen Verzeichnis synchronisieren möchten.
- **Benutzeranmeldung** : Die für den ursprünglichen Server konfigurierten Anmeldeoptionen sind standardmäßig ausgewählt. Sie werden automatisch zur Eingabe von Anmeldeinformationen oder anderen Informationen aufgefordert, die während der Konfiguration erforderlich sind. In seltenen Fällen muss ein Server mit anderen Optionen eingerichtet werden, um eine Änderung des Verhaltens auf dem aktiven Server zu vermeiden. Klicken Sie andernfalls einfach auf „Weiter“, um dieselben Einstellungen zu verwenden. 
- **Anmeldeinformationen für lokale Verzeichnisse** : Für jedes in den Synchronisierungseinstellungen enthaltene lokale Verzeichnis müssen Sie die Anmeldeinformationen angeben, um ein Synchronisierungskonto zu erstellen oder ein vorab erstelltes benutzerdefiniertes Synchronisierungskonto bereitzustellen. Diese Vorgehensweise ist mit der Neuinstallation identisch, allerdings können Sie keine Verzeichnisse hinzufügen oder entfernen. 
- **Konfigurationsoptionen** : Wie bei einer Neuinstallation können Sie die Anfangseinstellungen für die automatische Synchronisierung oder die Aktivierung des Stagingmodus konfigurieren. Der Hauptunterschied besteht darin, dass der Stagingmodus extra standardmäßig aktiviert ist, um vor dem aktiven Exportieren der Ergebnisse nach Azure einen Vergleich der Konfigurations- und Synchronisierungsergebnisse zu gestatten. 

![Connect-Verzeichnisse](media/how-to-connect-import-export-config/import2.png)

>[!NOTE]
>Die primäre Rolle kann nur einem Synchronisierungsserver zugeordnet werden, der Konfigurationsänderungen aktiv nach Azure exportiert. Alle übrigen Server müssen in den Stagingmodus versetzt werden. 

## <a name="migrating-settings-from-an-existing-server"></a>Migrieren von Einstellungen eines vorhandenen Servers 

Wenn die Einstellungsverwaltung von einem vorhandenen Server nicht unterstützt wird, können Sie den Server entweder direkt aktualisieren oder die Einstellungen zur Verwendung auf einem neuen Stagingserver migrieren.  

Für die Migration muss ein PowerShell-Skript ausgeführt werden, das die vorhandenen Einstellungen zur Verwendung in einer neuen Installation extrahiert.  Diese Methode wird empfohlen, um die Einstellungen des vorhandenen Servers zu katalogisieren und dann auf einen neu installierten Stagingserver anzuwenden.  Bei einem Vergleich der Einstellungen des ursprünglichen Servers mit denen des neu erstellten Servers werden die Änderungen zwischen den Servern schnell sichtbar.  Befolgen Sie grundsätzlich den Zertifizierungsprozess Ihrer Organisation, um sicherzustellen, dass keine zusätzliche Konfiguration erforderlich ist.  

### <a name="migration-process"></a>Migrationsvorgang 
Gehen Sie zur Migration der Einstellungen folgendermaßen vor:

1. Öffnen Sie die Eingabeaufforderung (cmd) auf dem neuen Stagingserver als Administrator.
2. Extrahieren Sie **AzureADConnect.msi**, indem Sie Folgendes ausführen: `msiexec /a msifile/qb TARGETDIR=targetpath`. Hierbei entspricht **msifile** der Adresse der MSI-Datei und **targetpath** dem Verzeichnis, in dem Sie die Dateien extrahieren möchten.
   
   Beispiel: `msiexec /a "C:\Holding\AzureADConnect.msi" TARGETDIR="C:\extractedfiles"`
3. Kopieren Sie **MigrateSettings.ps1** aus dem Verzeichnis „Microsoft Azure AD Connect\Tools“ in einen Speicherort auf dem vorhandenen Server.  Beispiel: C:\setup.  Dabei ist „setup“ ein Verzeichnis, das auf dem vorhandenen Server erstellt wurde. 
![Connect-Verzeichnisse](media/how-to-connect-import-export-config/migrate1.png)

4. Führen Sie das Skript wie unten dargestellt aus, und speichern Sie das gesamte Serverkonfigurationsverzeichnis. Kopieren Sie dieses Verzeichnis auf den neuen Stagingserver. Beachten Sie, dass Sie den gesamten Ordner **Exported-ServerConfiguration-** * auf den neuen Server kopieren müssen. 
 ![Connect-Verzeichnisse](media/how-to-connect-import-export-config/migrate2.png)

 ![Connect-Verzeichnisse](media/how-to-connect-import-export-config/migrate3.png)

5. Starten Sie **Azure AD Connect**, indem Sie auf das Symbol auf dem Desktop doppelklicken. Akzeptieren Sie die Software-Lizenzbedingungen, und klicken Sie auf der nächsten Seite auf die Schaltfläche **Anpassen**. 
6. Aktivieren Sie das Kontrollkästchen **Synchronisierungseinstellungen importieren**, und klicken Sie auf die Schaltfläche **Durchsuchen**, um den kopierten Ordner „Exported-ServerConfiguration-*“ zu durchsuchen und die Datei „MigratedPolicy.json“ auszuwählen.
 ![Connect-Verzeichnisse](media/how-to-connect-import-export-config/migrate4.png)

7. Um die migrierten Einstellungen mit den angewendeten Einstellungen zu vergleichen, suchen Sie unter **%ProgramData%\AADConnect** nach der neuesten Version der Dateien **Migrated-SynchronizationPolicy-*.JSON** und **Applied-SynchronizationPolicy-*.JSON**. („*“ ist der Zeitstempel.) Vergleichen Sie die Parität mit Ihrem bevorzugten Dateivergleichstool. 

## <a name="post-installation-verification"></a>Überprüfung nach der Installation 

Der Vergleich der ursprünglich importierten Einstellungsdatei mit der exportierten Einstellungsdatei des neu bereitgestellten Servers ist ein wichtiger Schritt, um die Unterschiede zwischen der beabsichtigten und der resultierenden Bereitstellung zu verstehen. Mit Ihrer bevorzugten Anwendung zum Textvergleich erhalten Sie sofort eine Darstellung, in der alle gewünschten oder versehentlichen Änderungen hervorgehoben sind. Auch wenn viele ehemals manuelle Konfigurationsschritte jetzt nicht mehr nötig sind, müssen Sie weiterhin den Zertifizierungsprozess Ihrer Organisation befolgen und so sicherstellen, dass keine zusätzliche Konfiguration erforderlich ist. Dies ist möglicherweise der Fall, wenn Sie erweiterte Einstellungen nutzen, die derzeit nicht im öffentlichen Vorschaurelease der Einstellungsverwaltung erfasst werden. 

Folgende Einschränkungen sind bekannt: 
- **Synchronisierungsregeln** : Die Rangfolge für eine benutzerdefinierte Regel muss im reservierten Bereich zwischen 0 und 99 liegen, um Konflikte mit den Standardregeln von Microsoft zu vermeiden. Wenn Sie eine benutzerdefinierte Regel außerhalb des reservierten Bereichs einfügen, wird Ihre benutzerdefinierte Regel beim Hinzufügen von Standardregeln zur Konfiguration möglicherweise verschoben. Ein ähnliches Problem tritt auf, wenn Ihre Konfiguration geänderte Standardregeln enthält. Von Änderungen an einer Standardregel wird dringend abgeraten, da Regeln höchstwahrscheinlich falsch angeordnet werden. Geräterückschreiben: Diese Einstellungen werden katalogisiert, aber derzeit nicht während der Konfiguration angewendet. Wenn das Geräterückschreiben für den ursprünglichen Server aktiviert war, müssen Sie das Feature auf dem neu bereitgestellten Server manuell konfigurieren. 
- **Synchronisierte Objekttypen** : Auch wenn die Liste synchronisierter Objekttypen (Benutzer, Kontakte, Gruppen usw.) mithilfe von Synchronization Service Manager eingeschränkt werden kann, wird dieses Feature über die Synchronisierungseinstellungen zurzeit nicht unterstützt. Nach Abschluss der Installation müssen Sie die erweiterte Konfiguration manuell erneut anwenden. 
- **Benutzerdefinierte Ausführungsprofile** : Obwohl der Standardsatz von Ausführungsprofilen mithilfe von Synchronization Service Manager geändert werden kann, wird dieses Feature über die Synchronisierungseinstellungen zurzeit nicht unterstützt. Nach Abschluss der Installation müssen Sie die erweiterte Konfiguration manuell erneut anwenden. 
- **Konfigurieren der Bereitstellungshierarchie** : Dieses erweiterte Feature von Synchronization Service Manager wird über die Synchronisierungseinstellungen nicht unterstützt und muss nach Abschluss der ersten Bereitstellung manuell neu konfiguriert werden. 
- **AD FS- und PingFederate-Authentifizierung** : Die diesen Authentifizierungsfeatures zugeordneten Anmeldemethoden werden automatisch vorab ausgewählt. Sie müssen jedoch alle anderen erforderlichen Konfigurationsparameter interaktiv angeben. 

 ## <a name="next-steps"></a>Nächste Schritte

- [Voraussetzungen für Azure AD Connect](how-to-connect-install-prerequisites.md) 
- [Express-Einstellungen](how-to-connect-install-express.md)
- [Benutzerdefinierte Einstellungen](how-to-connect-install-custom.md)
- [Installieren des Azure AD Connect Health-Agents](how-to-connect-health-agent-install.md) 
