---
title: Exportieren eines Microsoft Identity Manager-Connectors für die Verwendung mit dem Azure AD-ECMA-Connectorhost
description: Dieser Artikel beschreibt, wie ein Connector für die Verwendung mit dem Azure AD-ECMA-Connectorhost erstellt und aus MIM Sync exportiert werden kann.
services: active-directory
author: billmath
manager: mtillman
ms.service: active-directory
ms.workload: identity
ms.topic: how-to
ms.date: 06/01/2021
ms.subservice: hybrid
ms.author: billmath
ms.collection: M365-identity-device-management
ms.openlocfilehash: 229da477e358a0efd85ea555762443de9859b253
ms.sourcegitcommit: ff1aa951f5d81381811246ac2380bcddc7e0c2b0
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/07/2021
ms.locfileid: "111569998"
---
# <a name="export-a-microsoft-identity-manager-connector-for-use-with-azure-ad-ecma-connector-host"></a>Exportieren eines Microsoft Identity Manager-Connectors für die Verwendung mit dem Azure AD-ECMA-Connectorhost

>[!IMPORTANT]
> Die Vorschauversion der lokalen Bereitstellung ist derzeit nur auf Einladung verfügbar. Sie können [hier](https://aka.ms/onpremprovisioningpublicpreviewaccess) den Zugriff auf die Funktion beantragen. Wir öffnen die Vorschauversion im Rahmen der Vorbereitung auf die allgemeine Verfügbarkeit in den nächsten Monaten für weitere Kunden und Connectors.

Sie können eine spezifische Connector-Konfiguration aus einer FIM Sync- oder MIM Sync-Installation in den Azure AD-ECMA-Connectorhost importieren.  Beachten Sie, dass die MIM Sync-Installation nur für die Konfiguration und nicht für die laufende Synchronisierung mit Azure AD verwendet werden kann.

>[!IMPORTANT]
>Derzeit unterstützt nur der generische SQL-Connector (GSQL) die Verwendung mit dem Azure AD-ECMA-Connectorhost.


## <a name="creating-and-exporting-a-connector-configuration-in-mim-sync"></a>Erstellen und Exportieren einer Connector-Konfiguration in bzw. aus MIM Sync
Wenn Sie MIM Sync bereits mit Ihrem ECMA-Connector konfiguriert haben, fahren Sie mit Schritt 10 fort.

 1. Bereiten Sie einen Server mit Windows Server 2016 vor, der sich von dem Server unterscheidet, der zum Ausführen des Azure AD-ECMA-Connectorhosts verwendet wird.  Dieser Hostserver sollte entweder über eine SQL Server 2016-Datenbank oder über eine Netzwerkverbindung mit einer SQL Server 2016-Datenbank verfügen.  Der Server kann beispielsweise eingerichtet werden, indem eine Azure Virtual Machine mit dem Image **SQL Server 2016 SP1 Standard unter Windows Server 2016** bereitgestellt wird.  Beachten Sie, dass dieser Server keine Internetverbindung benötigt, sondern ausschließlich einen Remotedesktopzugriff zu Einrichtungszwecken.
 2. Erstellen Sie ein Konto, dass Sie während der MIM Sync-Installation verwenden können.  Dabei kann es sich z. B. um ein lokales Konto auf dem Windows Server handeln.  Starten Sie zum Erstellen eines lokalen Kontos zunächst die Systemsteuerung, öffnen Sie Benutzerkonten und fügen Sie ein **mimsync**-Benutzerkonto hinzu.
 3. Fügen Sie nun das im vorherigen Schritt erstellte Konto der lokalen Administratorgruppe hinzu.
 4. Erteilen Sie dem gerade erstellten Konto die Berechtigung, einen Dienst auszuführen.  Starten Sie Lokale Sicherheitsrichtlinien, klicken Sie auf Lokale Richtlinien, Zuweisen von Benutzerrechten und **Anmelden als Dienst**.  Fügen Sie das zuvor erwähnte Konto hinzu.
 5. Installieren Sie MIM Sync auf diesem Host. Wenn Sie nicht über MIM Sync-Binärdateien verfügen, können Sie eine Evaluierungssoftware installieren, indem Sie die ZIP-Datei über [https://www.microsoft.com/en-us/download/details.aspx?id=48244](https://www.microsoft.com/en-us/download/details.aspx?id=48244) herunterladen, das ISO-Image einbinden und den Ordner **Synchronisierungsdienst** auf den Windows Server-Host kopieren.  Führen Sie dann das in diesem Ordner enthaltene Setupprogramm aus.   Beachten Sie, dass die Evaluierungssoftware zeitlich begrenzt ist und ablaufen wird. Sie ist nicht für die Verwendung in der Produktion vorgesehen.
 6. Nachdem die Installation von MIM Sync abgeschlossen wurde, müssen Sie sich zunächst abmelden und dann erneut anmelden.
 7. Installieren Sie Ihren Connector auf demselben Server wie MIM Sync.  (Diese Testumgebungsanleitung veranschaulicht die Verwendung eines von Microsoft bereitgestellten Connectors, der über [https://www.microsoft.com/en-us/download/details.aspx?id=51495](https://www.microsoft.com/en-us/download/details.aspx?id=51495) heruntergeladen werden kann).
 8. Starten Sie die Synchronisierungsdienst-Benutzeroberfläche.  Klicken Sie auf **Verwaltungs-Agents**.  Klicken Sie auf **Erstellen** und geben Sie den Verwaltungs-Agent des entsprechenden Connectors an.  Achten Sie darauf, für den Connector einen ECMA-basierten Verwaltungs-Agent auszuwählen.
 9. Geben Sie dem Connector einen Namen und konfigurieren Sie die Parameter, die zum Importieren und Exportieren von Daten in den Connector erforderlich sind.  Stellen Sie sicher, dass der Connector einwertige Zeichenfolgenattribute eines Benutzer- oder Personenobjekttyps importieren und exportieren kann.
 10. Starten Sie auf dem MIM Sync-Servercomputer die Synchronisierungsdienst-Benutzeroberfläche, sofern diese noch nicht gestartet wurde.  Klicken Sie auf **Verwaltungs-Agents**.
 11. Wählen Sie den Connector aus und klicken Sie auf **Verwaltungs-Agent exportieren**.  Speichern Sie die XML-Datei und die DLL sowie die damit verbundene Software für Ihren Connector auf dem Windows-Server, der den ECMA-Connectorhost enthalten wird.

An diesem Punkt wird der MIM Sync-Server nicht mehr benötigt.

 1. Melden Sie sich bei Windows Server mit dem Konto an, über das der Azure AD-ECMA-Connectorhost ausgeführt wird.
 2. Wechseln Sie in das Verzeichnis c:\program files\Microsoft ECMA2host\Service\ECMA und stellen Sie sicher, dass in diesem Verzeichnis bereits eine oder mehrere DLLs vorhanden sind.  (Diese DLLs entsprechen den von Microsoft bereitgestellten Connectors.)
 3. Kopieren Sie die MA-DLL für Ihren Connector und alle weiteren erforderlichen DLLs in dasselbe ECMA-Unterverzeichnis des Dienstverzeichnisses.
 4. Wechseln Sie zum Verzeichnis C:\program files\Microsoft ECMA2Host\Wizard und führen Sie das Programm Microsoft.ECMA2Host.ConfigWizard.exe aus, um den ECMA-Connectorhost zu konfigurieren.
 5. Es wird Ihnen ein neues Fenster mit einer Connector-Liste angezeigt. Standardmäßig sind darin keine Connectors vorhanden.  Klicken Sie auf **; Neuer Connector**.
 6. Geben Sie die XML-Datei des Verwaltungs-Agents an, die Sie zuvor aus MIM exportiert haben.  Fahren Sie mit den Anweisungen für die Konfiguration und die Schemazuordnung aus dem vorstehenden Abschnitt Konfigurieren eines Connectors fort.



## <a name="next-steps"></a>Nächste Schritte


- [App-Bereitstellung](user-provisioning.md)
- [Azure AD-ECMA-Connectorhost: Voraussetzungen](on-premises-ecma-prerequisites.md)
- [Azure AD-ECMA-Connectorhost: Installation](on-premises-ecma-install.md)
- [Azure AD-ECMA-Connectorhost: Konfiguration](on-premises-ecma-configure.md)
- [Generischer SQL-Connector](on-premises-sql-connector-configure.md)