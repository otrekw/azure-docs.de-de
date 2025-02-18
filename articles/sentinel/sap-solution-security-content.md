---
title: 'Azure Sentinel-Lösung für SAP: Referenz zu sicherheitsbezogenen Inhalten | Microsoft-Dokumentation'
description: Erfahren Sie mehr über die integrierten sicherheitsbezogenen Inhalte, die von der Azure Sentinel-Lösung für SAP bereitgestellt werden.
author: batamig
ms.author: bagold
ms.service: azure-sentinel
ms.topic: reference
ms.custom: mvc
ms.date: 05/12/2021
ms.subservice: azure-sentinel
ms.openlocfilehash: 25f06704a4c9accbfddbb2c85155ec3f8f15738a
ms.sourcegitcommit: 8651d19fca8c5f709cbb22bfcbe2fd4a1c8e429f
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/14/2021
ms.locfileid: "112072611"
---
# <a name="azure-sentinel-sap-solution-security-content-reference-public-preview"></a>Azure Sentinel-Lösung für SAP: Referenz zu sicherheitsbezogenen Inhalten (Public Preview)

In diesem Artikel werden die sicherheitsbezogenen Inhalte für die [Azure Sentinel-Lösung für SAP](sap-deploy-solution.md#deploy-sap-security-content) detailliert beschrieben.

Dazu gehören eine integrierte Arbeitsmappe und integrierte Analyseregeln. Sie können auch SAP-bezogene [Watchlists](watchlists.md) hinzufügen, um sie in Ihrer Suche, für Erkennungsregeln, bei der Bedrohungssuche sowie in Playbooks für die Reaktion auf Bedrohungen zu verwenden.

> [!IMPORTANT]
> Die Azure Sentinel-Lösung für SAP befindet sich derzeit in der PREVIEW-Phase. In den [zusätzlichen Nutzungsbestimmungen für Microsoft Azure-Vorschauen](https://azure.microsoft.com/support/legal/preview-supplemental-terms/) finden Sie weitere rechtliche Bedingungen, die für Azure-Features gelten, die sich in der Beta- oder Vorschauversion befinden oder anderweitig noch nicht zur allgemeinen Verfügbarkeit freigegeben sind.
>


## <a name="sap---system-applications-and-products-workbook"></a>Arbeitsmappe „SAP: Systemanwendungen und Produkte“

Verwenden Sie die Arbeitsmappe [SAP: Systemanwendungen und Produkte](sap-deploy-solution.md#deploy-sap-security-content), um die über den SAP-Datenconnector erfassten Daten zu visualisieren und zu überwachen.

Beispiel:

:::image type="content" source="media/sap/sap-workbook.png" alt-text="Arbeitsmappe „SAP: Systemanwendungen und Produkte“.":::

Weitere Informationen finden Sie unter [Tutorial: Visualisieren und Überwachen Ihrer Daten](tutorial-monitor-your-data.md).

## <a name="built-in-analytics-rules"></a>Integrierte Analyseregeln

In den folgenden Tabellen sind die integrierten [Analyseregeln](sap-deploy-solution.md#deploy-sap-security-content) in der Azure Sentinel-Lösung für SAP aufgeführt, die über den Marketplace für Azure Sentinel-Lösungen bereitgestellt wird.

### <a name="high-level-built-in-sap-solution-analytics-rules"></a>Integrierte Analyseregeln für die SAP-Lösung auf der Ebene „Hoch“

|Regelname  |Beschreibung  |Quellaktion  |Taktik  |
|---------|---------|---------|---------|
|**SAP – Hoch – Änderung an sensiblem privilegierten Benutzern**     |  Identifiziert Änderungen an sensiblen privilegierten Benutzern.    <br> <br>Verwalten Sie privilegierte Benutzer in der Watchlist [SAP – Privilegierte Benutzer](#users). |   Ändern Sie Benutzerdetails/Autorisierungen mithilfe von `SU01`. <br><br>**Datenquellen**: SAPcon – Überwachungsprotokoll     |  Rechteausweitung, Zugriff auf Anmeldeinformationen       |
|**SAP – Hoch – Änderung der Clientkonfiguration**     |     Identifiziert Änderungen an der Clientkonfiguration, z. B. Clientrolle oder Änderungsaufzeichnungsmodus.    |  Führen Sie Clientkonfigurationsänderungen mithilfe des Transaktionscodes `SCC4` aus. <br><br>**Datenquellen**: SAPcon – Überwachungsprotokoll  |    Umgehen von Verteidigungsmaßnahmen, Exfiltration, Persistenz     |
|**SAP – Hoch – Daten haben sich während der Debugaktivität geändert**     | Identifiziert Änderungen an Laufzeitdaten während einer Debugaktivität.         | 1. Aktivieren Sie das Debuggen (/h). <br>2. Wählen Sie ein zu änderndes Feld aus, und aktualisieren Sie dessen Wert.<br><br>**Datenquellen**: SAPcon – Überwachungsprotokoll        |  Ausführung/Lateral Movement       |
|**SAP – Hoch – Deaktivierung des Sicherheitsüberwachungsprotokolls**     | Identifiziert die Deaktivierung des Sicherheitsüberwachungsprotokolls.        |  Deaktivieren Sie das Sicherheitsüberwachungsprotokoll mit `SM19/RSAU_CONFIG`. <br><br>**Datenquellen**: SAPcon – Überwachungsprotokoll |   Exfiltration, Umgehen von Verteidigungsmaßnahmen, Persistenz      |
|**SAP – Hoch – Ausführung eines sensiblen ABAP-Programms**     |Identifiziert die direkte Ausführung eines sensiblen ABAP-Programms. <br><br>Verwalten Sie ABAP-Programme in der Watchlist [SAP – Sensible ABAP-Programme](#programs).        | Führen Sie ein Programm direkt mithilfe von `SE38`/`SA38`/`SE80` aus. <br> <br>**Datenquellen**: SAPcon – Überwachungsprotokoll      |     Exfiltration, Lateral Movement, Ausführung    |
|**SAP – Hoch – Ausführung eines sensiblen Transaktionscodes**     | Identifiziert die Ausführung eines sensiblen Transaktionscodes. <br><br>Verwalten Sie Transaktionscodes in der Watchlist [SAP – Sensible Transaktionscodes](#transactions).       |  Führen Sie einen sensiblen Transaktionscode aus. <br><br>**Datenquellen**: SAPcon – Überwachungsprotokoll      |    Ermittlung, Ausführung     |
|**SAP – Hoch – Funktionsmodul getestet**     |  Identifiziert das Testen eines Funktionsmoduls.       |   Testen Sie ein Funktionsmodul mit `SE37` / `SE80`.  <br><br>**Datenquellen**: SAPcon – Überwachungsprotokoll    |   Sammlung, Umgehen von Verteidigungsmaßnahmen, Lateral Movement      |
|**SAP – Hoch – HANA DB – Zuweisen von Administratorautorisierungen**     |   Identifiziert Administratorrechte oder Rollenzuweisungen.      |  Weisen Sie einen Benutzer mit Administratorrolle oder -berechtigungen zu.  <br><br>**Datenquellen**: Linux-Agent – Syslog     | Berechtigungsausweitung        |
|**SAP – Hoch – HANA DB – Änderungen an Überwachungsprotokollrichtlinien**     |  Identifiziert Änderungen an Überwachungsprotokollrichtlinien für HANA DB.       |     Erstellen oder aktualisieren Sie die vorhandene Überwachungsrichtlinie in Sicherheitsdefinitionen. <br> <br>**Datenquellen**: Linux-Agent – Syslog    |  Lateral Movement, Umgehen von Verteidigungsmaßnahmen, Persistenz       |
|**SAP – Hoch – HANA DB – Deaktivierung des Überwachungsprotokolls**     |    Identifiziert die Deaktivierung des HANA DB-Überwachungsprotokolls.     |    Deaktivieren Sie das Überwachungsprotokoll in der HANA DB-Sicherheitsdefinition. <br><br>**Datenquellen**: Linux-Agent – Syslog     |  Persistenz, Lateral Movement, Umgehen von Verteidigungsmaßnahmen       |
|  **SAP – Hoch – HANA DB – Benutzerverwaltungsaktionen**   | Identifiziert Benutzerverwaltungsaktionen.        |  Erstellen, Aktualisieren oder Löschen Sie einen Datenbankbenutzer. <br><br>**Datenquellen**: Linux-Agent – Syslog*       |Berechtigungsausweitung         |
|**SAP – Hoch – Anmeldung über unerwartetes Netzwerk**     |   Identifiziert eine Anmeldung über ein unerwartetes Netzwerk. <br><br>Verwalten Sie Netzwerke in der Watchlist [SAP – Netzwerke](#networks).    |    Melden Sie sich beim Back-End-System mit einer IP-Adresse an, die keinem der Netzwerke zugewiesen ist. <br><br>**Datenquellen**: SAPcon – Überwachungsprotokoll    |   Erstzugriff      |
|**SAP – Hoch – RFC-Ausführung eines sensiblen Funktionsmoduls**     | Sensible Funktionsmodelle, die bei relevanten Erkennungen verwendet werden sollen.    <br><br>Verwalten Sie Funktionsmodule in der Watchlist [SAP – Sensible Funktionsmodelle](#module).   |      Führen Sie ein Funktionsmodul mit RFC aus.  <br><br>**Datenquellen**: SAPcon – Überwachungsprotokoll |  Ausführung, Lateral Movement, Ermittlung       |
|**SAP – Hoch – Angemeldeter sensibler privilegierter Benutzer**     |    Identifiziert die Dialoganmeldung eines sensiblen privilegierten Benutzers. <br><br>Verwalten Sie privilegierte Benutzer in der Watchlist [SAP – Privilegierte Benutzer](#users).    |  Melden Sie sich mit `SAP*` oder einem anderen privilegierten Benutzer am Back-End-System an.  <br><br>**Datenquellen**: SAPcon – Überwachungsprotokoll     |   Erstzugriff, Zugriff auf Anmeldeinformationen      |
|  **SAP – Hoch – Sensibler privilegierter Benutzer nimmt eine Änderung bei anderem Benutzer vor**   |   Identifiziert Änderungen des sensiblen privilegierten Benutzers bei anderen Benutzern.       | Ändern Sie Benutzerdetails/Autorisierungen mithilfe von SU01.  <br><br>**Datenquellen**: SAPcon – Überwachungsprotokoll     |   Rechteausweitung, Zugriff auf Anmeldeinformationen       |
|**SAP – Hoch – Systemkonfigurationsänderung**     | Identifiziert Änderungen der Systemkonfiguration.        |   Passen Sie Systemänderungsoptionen oder Änderungen an Softwarekomponenten mithilfe des Transaktionscodes `SE06` an.<br><br>**Datenquellen**: SAPcon – Überwachungsprotokoll |Exfiltration, Umgehen von Verteidigungsmaßnahmen, Persistenz   |
|**SAP – Hoch – Benutzer erstellt und verwendet neuen Benutzer**     | Identifiziert einen Benutzer, der andere Benutzer erstellt und verwendet.        |  Erstellen Sie einen Benutzer mit SU01, und melden Sie sich dann mit dem neu erstellten Benutzer und derselben IP-Adresse an.<br><br>**Datenquellen**: SAPcon – Überwachungsprotokoll | Ermittlung, PreAttack, Erstzugriff  |
|**SAP – Hoch – Benutzer entsperrt und verwendet andere Benutzer**     |Identifiziert einen Benutzer, der entsperrt und von anderen Benutzern verwendet wird.       |  Entsperren Sie einen Benutzer mit SU01, und melden Sie sich dann mit dem entsperrten Benutzer und derselben IP-Adresse an.<br><br>**Datenquellen**: SAPcon – Überwachungsprotokoll, SAPcon – Dokumentänderungsprotokoll | Ermittlung, PreAttack, Erstzugriff, Lateral Movement  |
|**SAP – Hoch – Sensible Benutzer– Kennwortänderung und Anmeldung**     | Identifiziert Kennwortänderungen privilegierter Benutzer.      |  Ändern Sie das Kennwort eines privilegierten Benutzers, und melden Sie sich beim System an. <br>Verwalten Sie privilegierte Benutzer in der Watchlist [SAP – Privilegierte Benutzer](#users).<br><br>**Datenquellen**: SAPcon – Überwachungsprotokoll | Auswirkung, Command-and-Control, Rechteausweitung |
|**SAP – Hoch – Aktivierung oder Deaktivierung des ICF-Diensts**     | Identifiziert die Aktivierung oder Deaktivierung des ICF-Diensts.       |  Aktivieren Sie einen Dienst mithilfe von SICF.<br><br>**Datenquellen**: SAPcon – Tabellendatenprotokoll | Command-and-Control, Lateral Movement, Persistenz  |
|**SAP – Hoch – Neue ICF-Dienste**     | Identifiziert die Erstellung von ICF-Diensten.       |  Erstellen Sie einen Dienst mithilfe von SICF.<br><br>**Datenquellen**: SAPcon – Tabellendatenprotokoll | Command-and-Control, Lateral Movement, Persistenz  |
|**SAP – Hoch – Neue ICF-Diensthandler**     | Identifiziert die Erstellung von ICF-Handlern.       |  Weisen Sie einem Dienst mithilfe von SICF einen neuen Handler zu.<br><br>**Datenquellen**: SAPcon – Überwachungsprotokoll | Command-and-Control, Lateral Movement, Persistenz  |
|**SAP – Hoch – SPNego-Angriff**     | Identifiziert einen SPNego-Replay-Angriff.       |  **Datenquellen**: SAPcon – Überwachungsprotokoll | Auswirkung, Lateral Movement  |
|**SAP – Hoch – Ausführung eines sensiblen Funktionsmoduls**     | Identifiziert die Ausführung eines sensiblen ABAP-Funktionsmoduls.     | Führen Sie ein sensibles Funktionsmodul direkt mit SE37 aus. <br><br>**Hinweis**: Nur für Produktionssysteme relevant. <br><br>Verwalten Sie sensible Funktionen in der Watchlist [SAP – Sensible Funktionsmodule](#modules), und aktivieren Sie Tabellenprotokollierungsänderungen im Back-End für die Tabelle EUFUNC. (SE13) <br><br>**Datenquellen**: SAPcon – Tabellendatenprotokoll | Ermittlung, Command-and-Control 
|     |         |         |         |

### <a name="medium-level-built-in-sap-solution-analytics-rules"></a>Integrierte Analyseregeln für die SAP-Lösung auf der Ebene „Mittel“

|Regelname  |Beschreibung  |Quellaktion  |Taktik  |
|---------|---------|---------|---------|
|**SAP – Mittel – Zuweisung eines sensiblen Profils**     |  Identifiziert neue Zuweisungen eines sensiblen Profils zu einem Benutzer. <br><br>Verwalten Sie sensible Profile in der Watchlist [SAP – Sensible Profile](#profiles).      |    Weisen Sie mit `SU01` einem Benutzer ein Profil zu. <br><br>**Datenquellen**: SAPcon – Dokumentänderungsprotokoll    |  Berechtigungsausweitung       |
|**SAP – Mittel – Zuweisung einer sensiblen Rolle**     |    Identifiziert neue Zuweisungen für eine sensible Rolle für einen Benutzer.     <br><br>Verwalten Sie sensible Rollen in der Watchlist [SAP – Sensible Rollen](#roles).|  Weisen Sie mit `SU01` / `PFCG` einem Benutzer eine Rolle zu. <br><br>**Datenquellen**: SAPcon – Dokumentänderungsprotokoll, Überwachungsprotokoll     |   Berechtigungsausweitung      |
|**SAP – Mittel – Brute-Force-Angriffe**     |     Identifiziert Brute-Force-Angriffe auf das SAP-System anhand fehlgeschlagener Anmeldeversuche am Back-End-System.    |   Versuchen Sie, sich innerhalb des geplanten Zeitintervalls mit derselben IP-Adresse bei mehreren Systemen/Clients anzumelden. <br><br>**Datenquellen**: SAPcon – Überwachungsprotokoll      | Zugriff auf Anmeldeinformationen        |
|**SAP – Mittel – Kritische Autorisierungszuweisung – Neuer Autorisierungswert**     | Identifiziert die Zuweisung eines kritischen Autorisierungsobjektwerts zu einem neuen Benutzer.  <br><br>Verwalten Sie kritische Autorisierungsobjekte in der Watchlist [SAP – Kritische Autorisierungsobjekte](#objects).      |  Weisen Sie mithilfe von `PFCG` ein neues Autorisierungsobjekt zu, oder aktualisieren Sie ein vorhandenes in einer Rolle. <br><br>**Datenquellen**: SAPcon – Dokumentänderungsprotokoll      |     Berechtigungsausweitung    |
|**SAP – Mittel – Kritische Autorisierungszuweisung – Neue Benutzerzuweisung**     |  Identifiziert die Zuweisung eines kritischen Autorisierungsobjektwerts zu einem neuen Benutzer. <br><br>Verwalten Sie kritische Autorisierungsobjekte in der Watchlist [SAP – Kritische Autorisierungsobjekte](#objects).    |    Weisen Sie mithilfe von `SU01`/`PFCG` einen neuen Benutzer einer Rolle zu, die kritische Autorisierungswerte enthält. <br><br>**Datenquellen**: SAPcon – Dokumentänderungsprotokoll    |  Berechtigungsausweitung       |
|**SAP – Mittel – Debugaktivitäten**     |  Identifiziert alle auf das Debuggen bezogenen Aktivitäten.       |Aktivieren Sie das Debuggen (/h) im System, debuggen Sie einen aktiven Prozess, fügen Sie dem Quellcode Haltepunkte hinzu usw. <br><br>**Datenquellen**: SAPcon – Überwachungsprotokoll |   Ermittlung      |
|**SAP – Mittel – Mehrere Anmeldungen über IP-Adresse**     |  Identifiziert die Anmeldung mehrerer Benutzer über dieselbe IP-Adresse innerhalb eines geplanten Zeitintervalls.       |    Melden Sie mehrere Benutzer über die gleiche IP-Adresse an. <br><br>**Datenquellen**: SAPcon – Überwachungsprotokoll | Erstzugriff        |
|**SAP – Mittel – Mehrere Anmeldungen durch Benutzer**     | Identifiziert Anmeldungen desselben Benutzers an mehreren Terminals innerhalb des geplanten Zeitintervalls.  <br><br>Bei SAP-Versionen ab 7.5 nur über die Methode „SAL überwachen“ verfügbar.      |   Melden Sie sich mit demselben Benutzer und verschiedenen IP-Adressen an.   <br><br>**Datenquellen**: SAPcon – Überwachungsprotokoll   |  PreAttack, Zugriff auf Anmeldeinformationen, Erstzugriff, Sammlung       |
|**SAP – Mittel – Änderung der Konfiguration des Sicherheitsüberwachungsprotokolls**     |  Identifiziert Änderungen an der Konfiguration des Sicherheitsüberwachungsprotokolls.       |   Ändern Sie mit `SM19`/`RSAU_CONFIG` beliebige Konfigurationseinstellungen des Überwachungsprotokolls, z. B. Filter, Status, Aufzeichnungsmodus usw. <br><br>**Datenquellen**: SAPcon – Überwachungsprotokoll      |    Persistenz, Exfiltration, Umgehen von Verteidigungsmaßnahmen     |
|**SAP – Mittel – Transaktion wird entsperrt**     |Identifiziert das Entsperren einer Transaktion.         |     Entsperren Sie einen Transaktionscode mit `SM01`/`SM01_DEV`/`SM01_CUS`. <br><br>**Datenquellen**: SAPcon – Überwachungsprotokoll   |  Persistenz, Ausführung       |
|**SAP – Mittel – Download mehrerer Dateien**     |Identifiziert Downloads mehrerer Dateien für einen Benutzer innerhalb einer bestimmten Zeitspanne. | Laden Sie mithilfe von SAPGui für Excel, Listen usw. mehrere Dateien herunter. <br><br>**Datenquellen**: SAPcon – Überwachungsprotokoll   |  Sammlung, Exfiltration, Zugriff auf Anmeldeinformationen       |
|**SAP – Mittel – Mehrere Spoolausführungen**     |Identifiziert mehrere Spools für einen Benutzer innerhalb einer bestimmten Zeitspanne. | Erstellen und führen Sie mehrere Spoolaufträge eines beliebigen Typs durch einen Benutzer aus. (SP01) <br><br>**Datenquellen**: SAPcon – Spoolprotokoll, SAPcon – Überwachungsprotokoll   |  Sammlung, Exfiltration, Zugriff auf Anmeldeinformationen       |
|**SAP – Mittel – Mehrere Spoolausgabeausführungen**     |Identifiziert mehrere Spools für einen Benutzer innerhalb einer bestimmten Zeitspanne. | Erstellen und führen Sie mehrere Spoolaufträge eines beliebigen Typs durch einen Benutzer aus. (SP01) <br><br>**Datenquellen**: SAPcon – Spoolprotokoll, SAPcon – Überwachungsprotokoll   |  Sammlung, Exfiltration, Zugriff auf Anmeldeinformationen       |
|**SAP – Mittel – Spoolübernahme**     |Identifiziert einen Benutzer, der eine Spoolanforderung druckt, die von einer anderen Person erstellt wurde. | Erstellen Sie eine Spoolanforderung mit einem Benutzer, und geben Sie sie dann in als ein anderer Benutzer aus. <br><br>**Datenquellen**: SAPcon – Spoolprotokoll, SAPcon – Spoolausgabeprotokoll, SAPcon – Überwachungsprotokoll   |  Sammlung, Exfiltration, Command-and-Control      |
|**SAP – Mittel – Konfiguration unsicherer FTP-Server**     |Identifiziert unsichere FTP-Serverkonfigurationen, z. B. wenn eine FTP-Positivliste leer ist oder Platzhalter enthält. | Verwalten Sie in der Verwaltungsansicht `SAPFTP_SERVERS_V` keine Werte, die in der Tabelle `SAPFTP_SERVERS` Platzhalter enthalten. (SM30) <br><br>**Datenquellen**: SAPcon – Überwachungsprotokoll   |  Erstzugriff, Command-and-Control      |
|**SAP – Mittel – FTP für nicht autorisierte Server**     |Identifiziert eine FTP-Verbindung für einen nicht autorisierten Server. | Erstellen Sie eine neue FTP-Verbindung, z. B. mithilfe des Funktionsmoduls FTP_CONNECT. <br><br>**Datenquellen**: SAPcon – Überwachungsprotokoll   |  Ermittlung, Erstzugriff, Command-and-Control      |
|**SAP – Mittel – Sensible Tabellen – Direktzugriff durch RFC-Anmeldung**     |Identifiziert den Zugriff auf eine generische Tabelle per RFC-Anmeldung. <br><br> Verwalten Sie Tabellen in der Watchlist [SAP – Sensible Tabellen](#tables).<br><br> **Hinweis**: Nur für Produktionssysteme relevant.   | Öffnen Sie den Tabelleninhalt mit SE11/SE16/SE16N.<br><br>**Datenquellen**: SAPcon – Überwachungsprotokoll   |  Sammlung, Exfiltration, Zugriff auf Anmeldeinformationen     |
|**SAP – Mittel – Änderungen an sensiblen Rollen**     |Identifiziert Änderungen an sensiblen Rollen. <br><br> Verwalten Sie sensible Rollen in der Watchlist [SAP – Sensible Rollen](#roles).       |  Ändern Sie eine Rolle mithilfe von PFCG. <br><br>**Datenquellen**: SAPcon – Dokumentänderungsprotokoll, SAPcon – Überwachungsprotokoll   |  Auswirkung, Rechteausweitung, Persistenz     |
|**SAP – Mittel – Änderungen an sensiblen Profilen**     |Identifiziert Änderungen an sensiblen Profilen.  <br><br> Verwalten Sie sensible Profile in der Watchlist [SAP – Sensible Profile](#profiles).     |  Ändern Sie eine Rolle mithilfe von PFCG.  <br><br>**Datenquellen**: SAPcon – Dokumentänderungsprotokoll, SAPcon – Überwachungsprotokoll   | Berechtigungen, Eskalation, Persistenz, Command-and-Control |
|**SAP – Mittel – Ausführung eines veralteten/unsicheren Programms**     |Identifiziert die Ausführung eines veralteten oder unsicheren ABAP-Programms. <br><br> Verwalten Sie veraltete Programme in der Watchlist [SAP – Veraltete Programme](#programs).<br><br> **Hinweis**: Nur für Produktionssysteme relevant.  | Führen Sie ein Programm direkt mithilfe von SE38/SA38/SE80 oder eines Hintergrundauftrags aus.  <br><br>**Datenquellen**: SAPcon – Überwachungsprotokoll   | Ermittlung, Command-and-Control |
|**SAP – Mittel – Ausführung eines veralteten oder unsicheren Funktionsmoduls**     |Identifiziert die Ausführung eines veralteten oder unsicheren ABAP-Funktionsmoduls. <br><br>Verwalten Sie veraltete Funktionen in der Watchlist [SAP – Veraltete Funktionsmodule](#modules). Stellen Sie sicher, dass Sie im Back-End Änderungen an der Tabellenprotokollierung für die Tabelle `EUFUNC` aktivieren. (SE13)<br><br> **Hinweis**: Nur für Produktionssysteme relevant.  | Führen Sie ein veraltetes oder unsicheres Funktionsmodul direkt mit SE37 aus. <br><br>**Datenquellen**: SAPcon – Tabellendatenprotokoll   | Ermittlung, Command-and-Control |
|     |         |         |         |

### <a name="low-level-built-in-sap-solution-analytics-rules"></a>Integrierte Analyseregeln für die SAP-Lösung auf der Ebene „Niedrig“

|Regelname  |Beschreibung  |Quellaktion  |Taktik  |
|---------|---------|---------|---------|
|**SAP – Niedrig – Mehrere Kennwortänderungen durch Benutzer**     |   Identifiziert mehrere Kennwortänderungen durch Benutzer.      |   Benutzerkennwort ändern <br><br>**Datenquellen**: SAPcon – Überwachungsprotokoll      |    Zugriff auf Anmeldeinformationen     |
|**SAP – Niedrig – Sensible Tabellen– Direktzugriff per Dialoganmeldung**     |   Identifiziert den Zugriff auf generische Tabellen per Dialoganmeldung.      |  Öffnen Sie den Tabelleninhalt mit `SE11`/`SE16`/`SE16N`. <br><br>**Datenquellen**: SAPcon – Überwachungsprotokoll      |    Ermittlung     |
|**SAP – Niedrig – Dynamisches ABAP-Programm**     | Identifiziert die Ausführung dynamischer ABAP-Programmierung. Beispielsweise, wenn ABAP-Code dynamisch erstellt, geändert oder gelöscht wurde. <br><br> Verwalten Sie ausgeschlossene Transaktionscodes in der Watchlist [SAP – Transaktionen für Generierungen von ABAP-Code](#transactions). | Erstellen Sie einen ABAP-Bericht, der ABAP-Programmgenerierungsbefehle wie INSERT REPORT verwendet, und führen Sie den Bericht dann aus.  <br><br>**Datenquellen**: SAPcon – Überwachungsprotokoll      |    Ermittlung, Command-and-Control, Auswirkung     |
|**SAP – Niedrig – Dynamisches RFC-Ziel**     |   Identifiziert die Ausführung von RFC mithilfe dynamischer Ziele. | Führen Sie einen ABAP-Bericht aus, der dynamische Ziele (cl_dynamic_destination) verwendet. Beispiel: DEMO_RFC_DYNAMIC_DEST.   <br><br>**Datenquellen**: SAPcon – Überwachungsprotokoll      |    Sammlung, Exfiltration     |
|     |         |         |         |

### <a name="informational-level-built-in-sap-solution-analytics-rules"></a>Integrierte Analyseregeln für die SAP-Lösung auf der Ebene „Information“

|Regelname  |Beschreibung  |Quellaktion  |
|---------|---------|---------|---------|
|**SAP – Information – Lebenszyklus – SAP-Hinweise wurden im System implementiert**     |   Identifiziert die Implementierungen von SAP-Hinweisen im System. | Implementieren Sie einen SAP-Hinweis mithilfe von SNOTE/TCI. <br><br>**Datenquellen**: SAPcon – Änderungsanforderungen      |
| | | |

## <a name="available-watchlists"></a>Verfügbare Watchlists

In der folgenden Tabelle sind die für die Azure Sentinel-Lösung für SAP verfügbaren [Watchlists](sap-deploy-solution.md#deploy-sap-security-content) und deren Felder aufgeführt.

Diese Watchlists stellen die Konfiguration für die Azure Sentinel-Lösung „Kontinuierliche Bedrohungsüberwachung in SAP“ bereit und sind im GitHub-Repository für Azure Sentinel unter https://github.com/Azure/Azure-Sentinel/tree/master/Solutions/SAP/Analytics/Watchlists zu finden.


|Name der Watchlist  |Beschreibung und Felder  |
|---------|---------|
|<a name="objects"></a>**SAP – Kritische Autorisierungsobjekte**     |  Kritisches Autorisierungsobjekt, für das Zuweisungen gesteuert werden sollen.     <br><br>- **AuthorizationObject**: ein SAP-Autorisierungsobjekt wie `S_DEVELOP`, `S_TCODE` oder `Table TOBJ` <br>- **AuthorizationField**: ein SAP-Autorisierungsfeld wie `OBJTYP` oder `TCD`    <br>- **AuthorizationValue**: ein SAP-Autorisierungswert wie `DEBUG`       <br>- **ActivityField**: SAP-Aktivitätsfeld. In den meisten Fällen ist dieser Wert `ACTVT`. Für Autorisierungsobjekte ohne **Aktivität** oder mit nur einem Feld **Aktivität**, das mit `NOT_IN_USE` gefüllt ist.        <br>- **Aktivität**: SAP-Aktivität gemäß dem Autorisierungsobjekt, z. B.: `01`: Erstellen; `02`: Ändern; `03`: Anzeigen usw.      <br>- **Beschreibung**: eine aussagekräftige Beschreibung des kritischen Autorisierungsobjekts. |
|**SAP – Ausgeschlossene Netzwerke** | Für die interne Wartung ausgeschlossener Netzwerke, z. B. zum Ignorieren von Webdispatchern, Terminalservern usw. <br><br>-**Netzwerk**: eine Netzwerk-IP-Adresse oder ein IP-Adressbereich, z. B. `111.68.128.0/17`. <br>-**Beschreibung**: eine aussagekräftige Netzwerkbeschreibung.|
|**SAP – Ausgeschlossene Benutzer** |Systembenutzer, die am System angemeldet sind und ignoriert werden müssen. Beispielsweise Warnungen bei mehreren Anmeldungen durch denselben Benutzer. <br><br>- **Benutzer**: SAP-Benutzer <br>-**Beschreibung**: eine aussagekräftige Benutzerbeschreibung. |
|<a name="networks"></a>**SAP – Netzwerke**     |  Interne und Wartungsnetzwerke zur Identifizierung nicht autorisierter Anmeldungen.      <br><br>- **Netzwerk**: Netzwerk-IP-Adresse oder IP-Adressbereich, z. B. `111.68.128.0/17`     <br>- **Beschreibung**: eine aussagekräftige Netzwerkbeschreibung.|
|<a name="users"></a>**SAP – Privilegierte-Benutzer**     |   Privilegierte Benutzer, für die zusätzliche Einschränkungen gelten.  <br><br>- **Benutzer**: der ABAP-Benutzer, z. B. `DDIC` oder `SAP` <br>- **Beschreibung**: eine aussagekräftige Benutzerbeschreibung. |
|<a name= "programs"></a>**SAP – Sensible ABAP-Programme**     |      Sensible ABAP-Programme (Berichte), in denen die Ausführung gesteuert werden soll.   <br><br>- **ABAPProgram**: ABAP-Programm oder -Bericht, z. B. `RSPFLDOC`     <br>- **Beschreibung**: eine aussagekräftige Programmbeschreibung.|
|<a name="module"></a>**SAP – Sensibles Funktionsmodul**     |   Interne und Wartungsnetzwerke zur Identifizierung nicht autorisierter Anmeldungen.      <br><br>- **FunctionModule**: ein ABAP-Funktionsmodul, z. B. `RSAU_CLEAR_AUDIT_LOG`       <br>- **Beschreibung**: eine aussagekräftige Modulbeschreibung.     |
|<a name="profiles"></a>**SAP – Sensible Profile**     |  Sensible Profile, bei denen Zuweisungen gesteuert werden sollen.     <br><br>- **Profil**: SAP-Autorisierungsprofil, z. B. `SAP_ALL` oder `SAP_NEW`      <br>- **Beschreibung**: eine aussagekräftige Profilbeschreibung.|
|<a name="tables"></a>**SAP – Sensible Tabellen**     |  Sensible Tabellen, bei denen der Zugriff gesteuert werden soll.  <br><br>- **Tabelle**: ABAP-Wörterbuchtabelle, z. B. `USR02` oder `PA008` <br>- **Beschreibung**: eine aussagekräftige Tabellenbeschreibung. |
|<a name="roles"></a>**SAP – Sensible Rollen**     |  Sensible Rollen, bei denen die Zuweisung gesteuert werden soll.    <br><br>- **Rolle**: SAP-Autorisierungsrolle, z. B. `SAP_BC_BASIS_ADMIN`  <br>- **Beschreibung**: eine aussagekräftige Rollenbeschreibung. |
|<a name="transactions"></a>**SAP – Sensible Transaktionen**     |     Sensible Transaktionen, bei denen die Ausführung gesteuert werden soll.  <br><br>- **TransactionCode**: SAP-Transaktionscode, z. B. `RZ11` <br>- **Beschreibung**: eine aussagekräftige Codebeschreibung. |
|<a name="systems"></a>**SAP – Systeme**     |    Beschreibt die Landschaft von SAP-Systemen je nach Rolle und Nutzung.<br><br>- **SystemID**: die SAP-System-ID (SYSID) <br>- **SystemRole**: die SAP-Systemrolle, einer der folgenden Werte: `Sandbox`, `Development`, `Quality Assurance`, `Training`, `Production` <br>- **SystemUsage**: die SAP-Systemnutzung, einer der folgenden Werte: `ERP`, `BW`, `Solman`, `Gateway`, `Enterprise Portal`        |
|<a name="users"></a>**SAP – Ausgeschlossene Benutzer**     |  Systembenutzer, die angemeldet sind und ignoriert werden müssen, z. B. für die Warnung „Mehrere Anmeldungen durch Benutzer“. <br><br>- **Benutzer**: SAP-Benutzer  <br>- **Beschreibung**: eine aussagekräftige Benutzerbeschreibung  |
|<a name="networks"></a>**SAP – Ausgeschlossene Netzwerke**     |  Verwalten Sie interne, ausgeschlossene Netzwerke zum Ignorieren von Webdispatchern, Terminalservern usw.  <br><br>- **Netzwerk**: Netzwerk-IP-Adresse oder IP-Adressbereich, z. B. `111.68.128.0/17`  <br>- **Beschreibung**: eine aussagekräftige Netzwerkbeschreibung |
|<a name="modules"></a>**SAP – Veraltete Funktionsmodule**     | Veraltete Funktionsmodule, deren Ausführung gesteuert werden soll.    <br><br>- **FunctionModule**: ABAP-Funktionsmodul, z. B. TH_SAPREL  <br>- **Beschreibung**: eine aussagekräftige Funktionsmodulbeschreibung |
|<a name="programs"></a>**SAP – Veraltete Programme**     | Sensible ABAP-Programme (Berichte), in denen die Ausführung gesteuert werden soll.  <br><br>- **ABAPProgram**: ABAP-Programm, z. B. TH_ RSPFLDOC  <br>- **Beschreibung**: eine aussagekräftige ABAP-Programmbeschreibung |
|<a name="transactions"></a>**SAP – Transaktionen für ABAP-Generierungen**     |  Transaktionen für ABAP-Generierungen, deren Ausführung gesteuert werden soll. <br><br>- **TransactionCode**: Transaktionscode, z. B. SE11.  <br>- **Beschreibung**: eine aussagekräftige Transaktionscodebeschreibung  |
|<a name="servers"></a>**SAP – FTP-Server**     |  FTP-Server zur Identifizierung nicht autorisierter Verbindungen.    <br><br>- **Client**: z. B. 100.  <br>- **FTP_Server_Name**: FTP-Servername, z. B. http://contoso.com/ <br>-**FTP_Server_Port**: FTP-Serverport, z. B. 22. <br>- **Beschreibung**: eine aussagekräftige FTP-Serverbeschreibung |
| | |


## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen finden Sie unter

- [Tutorial: Bereitstellen der Azure Sentinel-Lösung für SAP](sap-deploy-solution.md)
- [Referenz zu Protokollen der Azure Sentinel-Lösung für SAP](sap-solution-log-reference.md)
- [Lokales Bereitstellen des Azure Sentinel-Datenconnectors für SAP](sap-solution-deploy-alternate.md)
- [Detaillierte SAP-Anforderungen für die Azure Sentinel-Lösung für SAP](sap-solution-detailed-requirements.md)
