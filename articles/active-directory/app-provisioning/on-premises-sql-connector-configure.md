---
title: Konfigurieren eines generischen SQL-Connectors für den Azure AD-ECMA-Connectorhost
description: In diesem Dokument wird beschrieben, wie Sie den generischen SQL-Connector für den Azure AD-ECMA-Connectorhost konfigurieren.
services: active-directory
author: billmath
manager: mtillman
ms.service: active-directory
ms.subservice: app-provisioning
ms.topic: how-to
ms.workload: identity
ms.date: 06/06/2021
ms.author: billmath
ms.reviewer: arvinh
ms.openlocfilehash: 0748a1b3b326ce8c731d599471c3f96346a815c2
ms.sourcegitcommit: ff1aa951f5d81381811246ac2380bcddc7e0c2b0
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/07/2021
ms.locfileid: "111570158"
---
# <a name="azure-ad-ecma-connector-host-generic-sql-connector-configuration"></a>Konfigurieren eines generischen SQL-Connectors für den Azure AD-ECMA-Connectorhost

>[!IMPORTANT]
> Die Vorschauversion der lokalen Bereitstellung ist derzeit nur auf Einladung verfügbar. Sie können [hier](https://aka.ms/onpremprovisioningpublicpreviewaccess) den Zugriff auf die Funktion beantragen. Wir öffnen die Vorschauversion im Rahmen der Vorbereitung auf allgemeine Verfügbarkeit in den nächsten Monaten für weitere Kunden und Connectors.


In diesem Dokument wird beschrieben, wie Sie einen neuen SQL-Connector für den Azure AD-ECMA-Connectorhost erstellen und konfigurieren.  Sie müssen dies tun, nachdem Sie den Azure AD-ECMA-Connectorhost erfolgreich installiert haben.  

>[!NOTE] 
> In diesem Dokument wird nur die Konfiguration des generischen SQL-Connectors behandelt.  Ein Schritt-für-Schritt-Beispiel der Einrichtung des generischen SQL-Connectors finden Sie im [Tutorial: Generischer SQL-Connector für ECMA-Connectorhost](tutorial-ecma-sql-connector.md)

Installation und Konfiguration des Azure AD-ECMA-Connectorhosts erfolgen in einem Prozess. Dieser Prozess hat den folgenden Ablauf.

 ![Installationsablauf](./media/on-premises-sql-connector-configure/flow-1.png)  

Weitere Informationen zu Installation und Konfiguration finden Sie unter:
   - [Voraussetzungen für den Azure AD-ECMA-Connectorhost](on-premises-ecma-prerequisites.md)
   - [Installation des Azure AD-ECMA-Connectorhosts](on-premises-ecma-install.md)
   - [Konfigurieren des Azure AD-ECMA-Connectorhosts und Bereitstellungs-Agents](on-premises-ecma-configure.md)

Je nachdem, welche Optionen Sie auswählen, sind einige der Assistentenbildschirme möglicherweise nicht verfügbar, und die Informationen können sich geringfügig unterscheiden.  Für die Zwecke dieser Konfiguration wird der Objekttyp „Benutzer“ verwendet. Nutzen Sie für die Konfiguration die folgenden Informationen.


## <a name="create-a-generic-sql-connector"></a>Erstellen eines generischen SQL-Connectors

Führen Sie zum Erstellen eines generischen SQL Connectors die folgenden Schritte aus:

 1.  Klicken Sie auf dem Desktop auf die Verknüpfung „ECMA-Connectorhost“.
 2.  Wählen Sie **Neuer Connector** aus.
     ![Wählen von „Neuer Connector“](.\media\on-premises-sql-connector-configure\sql-1.png)

 3. Füllen Sie die Felder auf der Seite **Eigenschaften** aus, und klicken Sie auf „Weiter“.  In der Tabelle unter der Abbildung finden Sie eine Anleitung zu den einzelnen Feldern.
     ![Eingeben von Eigenschaften](.\media\on-premises-sql-connector-configure\sql-2.png)

     |Eigenschaft|BESCHREIBUNG|
     |-----|-----|
     |Name|Der Name dieses Connectors|
     |Zeitgeber für automatische Synchronisierung (Minuten)|Der zulässige Mindestwert ist 120 Minuten.|
     |Geheimes Token|123456 [Dies muss eine Zeichenfolge mit 10-20 ASCII-Buchstaben und/oder Ziffern sein.]|
     |BESCHREIBUNG|Die Beschreibung des Connectors|
     |Erweiterungs-DLL|Wählen Sie für einen generischen SQL-Connector „Microsoft.IAM.Connector.GenericSql.dll“ aus.|
 4. Füllen Sie die Felder auf der Seite **Konnektivität** aus, und klicken Sie auf „Weiter“.  In der Tabelle unter der Abbildung finden Sie eine Anleitung zu den einzelnen Feldern.
     ![Eingeben von Informationen zur Konnektivität](.\media\on-premises-sql-connector-configure\sql-3.png)

     |Eigenschaft|BESCHREIBUNG|
     |-----|-----|
     |DSN-Datei|Die Datei mit dem Datenquellennamen, die zum Herstellen einer Verbindung mit der SQL Server-Instanz verwendet wird|
     |Benutzername|Der Benutzername einer Einzelperson mit Rechten für die SQL Server-Instanz.  Dieser muss für eigenständige Server das Format „Hostname\Konto_des_SQL-Administrators“ und für Domänenmitgliedsserver das Format „Domäne\Konto_des_SQL-Administrators“ haben.|
     |Kennwort|Das Kennwort des oben angegebenen Benutzernamens.|
     |DN ist Anker|Sofern nicht bekannt ist, dass Ihre Umgebung diese Einstellungen erfordert, lassen Sie „DN ist Anker“ und „Exporttyp:Objekt ersetzen“ deaktiviert.|
     |TypeObjectReplace exportieren||
 5. Füllen Sie die Felder auf der Seite **Schema 1** aus, und klicken Sie auf „Weiter“.  In der Tabelle unter der Abbildung finden Sie eine Anleitung zu den einzelnen Feldern.
     ![Eingeben von Schema 1](.\media\on-premises-sql-connector-configure\sql-4.png)

     |Eigenschaft|BESCHREIBUNG|
     |-----|-----|
     |Erkennungsmethode für Objekttyp|Die Methode, mit der der Objekttyp erkannt wird, den der Connector bereitstellen soll.|
     |Liste mit festem Wert/Tabelle/Sicht/SP (Stored Procedure, gespeicherte Prozedur)|Muss „User“ (Benutzer) enthalten.|
     |Spaltenname für Tabelle/Sicht/SP||
     |Parameter gespeicherter Prozeduren||
     |Angeben einer SQL-Abfrage zum Erkennen von Objekttypen||
 6. Füllen Sie die Felder auf der Seite **Schema 2** aus, und klicken Sie auf „Weiter“.  In der Tabelle unter der Abbildung finden Sie eine Anleitung zu den einzelnen Feldern.  Dieser Schemabildschirm kann sich geringfügig unterscheiden oder zusätzliche Informationen enthalten, und zwar abhängig von den Objekttypen, die im vorherigen Schritt ausgewählt wurden.
     ![Eingeben von Schema 2](.\media\on-premises-sql-connector-configure\sql-5.png)

     |Eigenschaft|BESCHREIBUNG|
     |-----|-----|
     |Benutzer:Attributerkennung|Muss auf „Tabelle“ festgelegt werden.|
     |Benutzer:Tabelle/Sicht/SP|Muss „Employees“ (Mitarbeiter) enthalten.|
     |Benutzer:Name der Tabelle mit mehreren Werten/Sichten||
     |Benutzer:Parameter der gespeicherten Prozedur||
     |Benutzer:Angeben einer SQL-Abfrage zum Erkennen von Objekttypen||
 7. Füllen Sie die Felder auf der Seite **Schema 3** aus, und klicken Sie auf „Weiter“.  In der Tabelle unter der Abbildung finden Sie eine Anleitung zu den einzelnen Feldern.  Die gezeigten Attribute hängen von den Informationen ab, die Sie im vorherigen Schritt angegeben haben.
     ![Eingeben von Schema 3](.\media\on-premises-sql-connector-configure\sql-6.png)

     |Eigenschaft|BESCHREIBUNG|
     |-----|-----|
     |DN-Attribut für Benutzer auswählen||
 8. Überprüfen Sie auf der Seite **Schema 4** die DataType-Attribute und Datenflussrichtung des Connectors.  Sie können sie bei Bedarf anpassen und auf „Weiter“ klicken.
     ![Eingeben von Schema 4](.\media\on-premises-sql-connector-configure\sql-7.png)  
 9. Füllen Sie die Felder auf der Seite **Global** aus, und klicken Sie auf „Weiter“.  In der Tabelle unter der Abbildung finden Sie eine Anleitung zu den einzelnen Feldern.
     ![Eingeben globaler Informationen](.\media\on-premises-sql-connector-configure\sql-8.png)

     |Eigenschaft|BESCHREIBUNG|
     |-----|-----|
     |Wasserzeichenabfrage||
     |Zeitzone der Datenquelle|Wählen Sie die Zeitzone aus, in der sich die Datenquelle befindet.|
     |Datum-/Uhrzeitformat der Datenquelle|Geben Sie das Format für die Datenquelle an.|
     |Benannte Parameter zum Ausführen einer gespeicherten Prozedur verwenden||
     |Vorgangsmethoden||
     |Name der Erweiterung||
     |Kennwort festlegen: SP-Name||
     |Kennwort festlegen: SP-Parameter||
 10. Stellen Sie auf der Seite **Partition auswählen** sicher, dass die richtigen Partitionen ausgewählt sind, und klicken Sie auf „Weiter“.
     ![Eingeben von Partitionsinformationen](.\media\on-premises-sql-connector-configure\sql-9.png)  

 11. Wählen Sie auf der Seite **Ausführungsprofile** die gewünschten Ausführungsprofile aus, und klicken Sie auf „Weiter“.
     ![Eingeben von Ausführungsprofilen](.\media\on-premises-sql-connector-configure\sql-10.png)

     |Eigenschaft|BESCHREIBUNG|
     |-----|-----|
     |Exportieren|Ausführungsprofil zum Exportieren von Daten in SQL.  Dieses Ausführungsprofil ist erforderlich.|
     |Vollständiger Import|Ausführungsprofil zum Importieren aller Daten aus den zuvor angegebenen SQL-Quellen.|
     |Deltaimport|Ausführungsprofil, das nur Änderungen aus SQL seit dem letzten vollständigen oder Deltaimport importiert.|
 
 12. Füllen Sie die Felder auf der Seite **Ausführungsprofile** aus, und klicken Sie auf „Weiter“.  In der Tabelle unter der Abbildung finden Sie eine Anleitung zu den einzelnen Feldern. 
     ![Eingeben von Exportinformationen](.\media\on-premises-sql-connector-configure\sql-11.png)

     |Eigenschaft|BESCHREIBUNG|
     |-----|-----|
     |Vorgangsmethode||
     |Tabelle/Sicht/SP||
     |Name des Startindexparameters||
     |Name des Endindexparameters||
     |Parameter gespeicherter Prozeduren||
 
 13. Füllen Sie die Felder auf der Seite **Objekttypen** aus, und klicken Sie auf „Weiter“.  In der Tabelle unter der Abbildung finden Sie eine Anleitung zu den einzelnen Feldern. 
     ![Eingeben von Objekttypen](.\media\on-premises-sql-connector-configure\sql-12.png)

     |Eigenschaft|BESCHREIBUNG|
     |-----|-----|
     |Zielobjekt|Das Objekt, das Sie konfigurieren.|
     |Anchor|Das Attribut, das als Objektanker verwendet wird.  Dieses Attribut muss im Zielsystem eindeutig sein. Der Azure AD-Bereitstellungsdienst fragt nach dem ersten Zyklus mithilfe dieses Attributs den ECMA-Host ab. Dieser Ankerwert muss mit dem Ankerwert in Schema 3 identisch sein.|
     |Abfrageattribut|Wird vom ECMA-Host zum Abfragen des In-Memory-Caches verwendet. Dieses Attribut muss eindeutig sein.|
     |DN|Das Attribut, das für den Distinguished Name von Zielobjekten verwendet wird.  In den meisten Fällen sollte die Option zum automatischen Generieren ausgewählt werden. Falls deaktiviert, stellen Sie sicher, dass das DN-Attribut einem Attribut in Azure AD zugeordnet ist, das den DN in diesem Format speichert: CN = anchorValue, Objekt = objectType|
 
 14. Der ECMA-Host erkennt die vom Zielsystem unterstützten Attribute. Sie können wählen, welches dieser Attribute sie Azure AD verfügbar machen. Diese Attribute können dann im Azure-Portal zur Bereitstellung konfiguriert werden.  Wählen Sie auf der Seite **Attribute auswählen** in der Dropdownliste die Attribute aus, die Sie hinzufügen möchten. 
     ![Eingeben von Attributen](.\media\on-premises-sql-connector-configure\sql-13.png)

15. Überprüfen Sie auf der Seite **Bereitstellung aufheben** die Bereitstellungsinformationen, und nehmen Sie ggf. Anpassungen vor. Die auf der vorherigen Seite ausgewählten Attribute stehen auf der Seite „Bereitstellung aufheben“ nicht zur Auswahl. Klicken Sie auf Fertig stellen.
     ![Eingeben von Informationen zum Aufheben der Bereitstellung](.\media\on-premises-sql-connector-configure\sql-14.png)



## <a name="next-steps"></a>Nächste Schritte

- [App-Bereitstellung](user-provisioning.md)
- [Azure AD-ECMA-Connectorhost: Installation](on-premises-ecma-install.md)
- [Azure AD-ECMA-Connectorhost: Konfiguration](on-premises-ecma-configure.md)
- [Tutorial: Generischer SQL-Connector mit ECMA-Connectorhost](tutorial-ecma-sql-connector.md)
