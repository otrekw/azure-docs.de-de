---
title: Microsoft Defender Advanced Threat Protection – Azure Security Center
description: Dieses Dokument stellt die Integration von Azure Security Center und Microsoft Defender Advanced Threat Protection vor.
services: security-center
documentationcenter: na
author: memildin
manager: rkarlin
ms.service: security-center
ms.devlang: na
ms.topic: how-to
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 10/07/2020
ms.author: memildin
ms.openlocfilehash: 92feb159fe4c893a55d37fa90c34acf4c4c93631
ms.sourcegitcommit: d2222681e14700bdd65baef97de223fa91c22c55
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/07/2020
ms.locfileid: "91826166"
---
# <a name="microsoft-defender-advanced-threat-protection-with-azure-security-center"></a>Microsoft Defender Advanced Threat Protection mit Azure Security Center

Azure Security Center bietet Integration von [Microsoft Defender Advanced Threat Protection (ATP)](https://www.microsoft.com/microsoft-365/windows/microsoft-defender-atp), um umfassende Funktionen für Endpoint Detection and Response (EDR) bereitzustellen.


## <a name="availability"></a>Verfügbarkeit

|Aspekt|Details|
|----|:----|
|Status des Release:|Allgemein verfügbar (Generally Available, GA)|
|Preise:|Erfordert [Azure Defender](security-center-pricing.md)|
|Unterstützte Computer:|![Ja](./media/icons/yes-icon.png) Azure-Computer unter Windows<br>![Ja](./media/icons/yes-icon.png) Azure Arc-Computer unter Windows|
|Erforderliche Rollen und Berechtigungen:|So aktivieren/deaktivieren Sie die Integration: **Sicherheitsadministrator** oder **Besitzer**<br>So zeigen Sie MDATP-Warnungen in Security Center an: **Sicherheitsleseberechtigter**, **Leser**, **Ressourcengruppenmitwirkender**, **Ressourcengruppenbesitzer**, **Sicherheitsadministrator**, **Abonnementbesitzer** oder **Abonnementmitwirkender**|
|Clouds:|![Ja](./media/icons/yes-icon.png) Kommerzielle Clouds<br>![Nein](./media/icons/no-icon.png) GCC-Kunden, die Workloads in öffentlichen Azure-Clouds ausführen<br>![Ja](./media/icons/yes-icon.png) US Gov<br>![Nein](./media/icons/no-icon.png) China Gov/andere Gov-Clouds|
|||


## <a name="microsoft-defender-atp-features-in-security-center"></a>Microsoft Defender ATP-Features in Security Center

Microsoft Defender ATP bietet:

- **Erweiterte Erkennungssensoren für Sicherheitsverletzungen**: Microsoft Defender ATP-Sensoren für Windows-Server erfassen eine breite Palette von verhaltensbezogenen Signalen.

- **Auf Analysen basierende, cloudgesteuerte Erkennung von Sicherheitsverletzungen:** : Microsoft Defender ATP nimmt eine schnelle Anpassung an sich ändernde Bedrohungen vor. Das Produkt verwendet fortschrittliche Analysen und Big Data. Microsoft Defender ATP wird durch die Leistungsfähigkeit von Intelligent Security Graph mit Signalen aus Windows, Azure und Office verstärkt, um unbekannte Bedrohungen zu erkennen. Das Produkt stellt aussagekräftige Warnmeldungen zur Verfügung und ermöglicht eine schnelle Reaktion.

- **Informationen zu Bedrohungen**: Microsoft Defender ATP generiert Warnungen, wenn es Angriffstools, -techniken und -verfahren erkennt. Das Produkt verwendet Daten, die von Microsoft-Bedrohungsspezialisten und Sicherheitsteams generiert werden, ergänzt durch Informationen von Partnern.


Durch die Integration von Defender ATP in das Azure Security Center können Sie außerdem von den folgenden zusätzlichen Funktionen profitieren:

- **Automatisiertes Onboarding**: Die Integration aktiviert automatisch den Microsoft Defender ATP-Sensor für Windows-Server, die von Azure Security Center überwacht werden (sofern sie nicht Windows Server 2019 ausführen).

- **Zentralisierte Benutzeroberfläche**: Die Azure Security Center-Konsole zeigt Microsoft Defender ATP-Warnungen an. Verwenden Sie Microsoft Defender ATP, um weitere Informationen zu erhalten. Microsoft Defender ATP zeigt zusätzliche Informationen wie die Warnprozessstruktur und den Ereignisgraph an. Sie können auch eine detaillierte Computerzeitachse verwenden, die jedes Verhalten über einen historischen Zeitraum von bis zu sechs Monaten anzeigt.

    ![Microsoft Defender ATP-Seite mit Detailinformationen zu einer Warnung](media/security-center-wdatp/image3.png)

## <a name="platform-support"></a>Plattformunterstützung

Microsoft Defender ATP in Security Center unterstützt die Erkennung unter Windows Server 2016-, 2012 R2- und 2008 R2 SP1. Für Azure-VMs müssen Sie Azure Defender für Ihr Abonnement aktivieren. Für Nicht-Azure-VMs muss Azure Defender nur auf Arbeitsbereichsebene aktiviert werden.

Die Serverendpunktüberwachung, die diese Integration verwendet, wurde für Office 365-GCC-Kunden deaktiviert.

## <a name="data-storage-location"></a>Datenspeicherort

Wenn Sie Azure Security Center zum Überwachen von Servern verwenden, wird automatisch ein Microsoft Defender ATP-Mandant erstellt. Die von Microsoft Defender ATP gesammelten Daten werden im Rahmen der Bereitstellung des Mandanten am Standort des Mandanten gespeichert. Kundendaten in pseudonymierter Form können auch in den zentralen Speicher- und Verarbeitungssystemen in den USA gespeichert werden. 

Nach der Konfiguration können Sie den Speicherort der Daten nicht mehr ändern. Wenn Sie Ihre Daten an einen anderen Standort verschieben möchten, wenden Sie sich an den Microsoft-Support, um den Mandanten zurückzusetzen.


## <a name="onboard-servers-to-security-center"></a>Integrieren von Servern in Azure Security Center 

Klicken Sie unter der Integration von Servern in Microsoft Defender ATP auf **Go to Azure Security Center to onboard servers** (Zum Azure Security Center wechseln, um Server zu integrieren), um Server in Security Center zu integrieren.

1. Wählen Sie im Bereich **Onboarding** einen Arbeitsbereich aus (oder erstellen Sie einen Arbeitsbereich), in dem die Daten gespeichert werden sollen.

2. Wenn nicht alle Arbeitsbereiche angezeigt werden, kann dies auf fehlende Berechtigungen zurückzuführen sein. Stellen Sie daher sicher, dass Ihr Arbeitsbereich durch Azure Defender geschützt wird.
    
3. Wählen Sie **Server hinzufügen** aus, um Anweisungen zum Installieren des Log Analytics-Agents anzuzeigen. 

4. Nach der Integration können Sie die Computer unter [Ressourcenbestand](asset-inventory.md) überwachen.

   ![Integrieren von Computern](media/security-center-wdatp/onboard-computers.png)

## <a name="enable-microsoft-defender-atp-integration"></a>Aktivieren der Microsoft Defender ATP-Integration

Wählen Sie **Security Center** > **Preise und Einstellungen** und dann Ihr Abonnement aus, um anzuzeigen, ob die ATP-Integration von Microsoft Defender aktiviert ist.

Hier können Sie die Integrationen anzeigen, die zurzeit aktiviert sind.

  ![Einstellungsseite der Azure Security Center-Bedrohungserkennung mit aktivierter Microsoft Defender ATP-Integration](media/security-center-wdatp/enable-integrations.png)

- Wenn Sie Azure Defender bereits aktiviert haben, ist keine weitere Aktion erforderlich. Azure Security Center integriert die Server automatisch in Microsoft Defender ATP. Das Onboarding kann bis zu 24 Stunden dauern.

- Wenn Sie die Server noch nie in Azure Security Center integriert haben, integrieren Sie sie in Azure Security Center, und aktivieren Sie Azure Defender wie gewohnt.

- Gehen Sie folgendermaßen vor, wenn Sie die Server über Microsoft Defender ATP integriert haben:
  - Verwenden Sie die Anleitungen in der Dokumentation zum [Aufheben der Integration von Servercomputern](https://go.microsoft.com/fwlink/p/?linkid=852906).
  - Integrieren Sie diese Server in Azure Security Center.

## <a name="access-to-the-microsoft-defender-atp-portal"></a>Zugreifen auf das Microsoft Defender ATP-Portal

1. Befolgen Sie die Anweisungen unter [Zuweisen von Benutzerzugriff auf das Portal](https://docs.microsoft.com/windows/security/threat-protection/microsoft-defender-atp/assign-portal-access).

1. Überprüfen Sie, ob Sie einen Proxy oder eine Firewall einsetzen, der bzw. die anonymen Datenverkehr blockiert. Der Defender ATP-Sensor stellt eine Verbindung vom Systemkontext her, daher muss anonymer Datenverkehr zugelassen werden. Um den ungehinderten Zugriff auf das Microsoft Defender ATP-Portal sicherzustellen, befolgen Sie die Anweisungen unter [Aktivieren des Zugriffs auf Windows Defender ATP-Dienst-URLs im Proxyserver](https://docs.microsoft.com/windows/security/threat-protection/microsoft-defender-atp/configure-proxy-internet#enable-access-to-microsoft-defender-atp-service-urls-in-the-proxy-server).

## <a name="test-the-feature"></a>Testen des Features

So generieren Sie eine unbedenkliche Microsoft Defender ATP-Testwarnung:

1. Erstellen Sie einen Ordner „C:\test-MDATP-test“.

1. Verwenden Sie Remotedesktop, um auf eine Windows Server 2012 R2-VM oder eine Windows Server 2016-VM zuzugreifen. Öffnen Sie ein Befehlszeilenfenster.

1. Kopieren Sie an der Eingabeaufforderung den folgenden Befehl, und führen Sie ihn aus. Das Eingabeaufforderungsfenster wird automatisch geschlossen.

    ```
    powershell.exe -NoExit -ExecutionPolicy Bypass -WindowStyle Hidden (New-Object System.Net.WebClient).DownloadFile('http://127.0.0.1/1.exe', 'C:\\test-MDATP-test\\invoice.exe'); Start-Process 'C:\\test-MDATP-test\\invoice.exe'
    ```

   ![Ein Eingabeaufforderungsfenster mit dem oben gezeigten Befehl](media/security-center-wdatp/image4.jpeg)

1. Wenn der Befehl erfolgreich ist, wird Ihnen eine neue Warnung im Azure Security Center-Dashboard und im Microsoft Defender ATP-Portal angezeigt. Die Anzeige dieser Warnung kann einige Minuten dauern.

1. Um die Warnung in Security Center zu überprüfen, navigieren Sie zu **Sicherheitswarnungen** > **Verdächtige PowerShell-Befehlszeile**.

1. Wählen Sie im Untersuchungsfenster den Link aus, um zum Microsoft Defender ATP-Portal zu navigieren.

## <a name="next-steps"></a>Nächste Schritte

- [Von Azure Security Center unterstützte Features und Plattformen](security-center-os-coverage.md)
- [Festlegen von Sicherheitsrichtlinien in Azure Security Center](tutorial-security-policy.md): Erfahren Sie, wie Sie Sicherheitsrichtlinien für Ihre Azure-Abonnements und -Ressourcengruppen konfigurieren.
- [Verwalten von Sicherheitsempfehlungen in Azure Security Center](security-center-recommendations.md): Erfahren Sie, wie Empfehlungen Ihnen beim Schutz Ihrer Azure-Ressourcen helfen.
- [Überwachen der Sicherheitsintegrität in Azure Security Center](security-center-monitoring.md): Hier erfahren Sie, wie Sie die Integrität Ihrer Azure-Ressourcen überwachen.