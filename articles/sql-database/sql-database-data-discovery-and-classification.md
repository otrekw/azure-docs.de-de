---
title: Datenermittlung und -klassifizierung
description: Datenermittlung und -klassifizierung für Azure SQL-Datenbank und Azure Synapse Analytics
services: sql-database
ms.service: sql-database
ms.subservice: security
ms.custom: ''
titleSuffix: Azure SQL Database and Azure Synapse
ms.devlang: ''
ms.topic: conceptual
author: DavidTrigano
ms.author: datrigan
ms.reviewer: vanto
ms.date: 04/21/2020
tags: azure-synapse
ms.openlocfilehash: f05b4d4fec99aaa2fb79da46e2167d883d1f15ec
ms.sourcegitcommit: d57d2be09e67d7afed4b7565f9e3effdcc4a55bf
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/22/2020
ms.locfileid: "81766989"
---
# <a name="data-discovery--classification-for-azure-sql-database-and-azure-synapse-analytics"></a>Datenermittlung und -klassifizierung für Azure SQL-Datenbank und Azure Synapse Analytics

Die Datenermittlung und -klassifizierung ist in Azure SQL-Datenbank integriert. Sie bietet erweiterte Funktionen zum Ermitteln, Klassifizieren und Bezeichnen vertraulicher Daten in Ihren Datenbanken und zum Erstellen von Berichten zu diesen.

Zu den vertraulichsten Daten gehören Geschäfts-, Finanz-, Gesundheits- und personenbezogene Informationen. Das Ermitteln und Klassifizieren dieser Daten kann eine entscheidende Rolle beim Ansatz Ihrer Organisation zum Schutz von Daten spielen. Sie kann für Folgendes als Infrastruktur gelten:

- Unterstützung bei der Einhaltung von Standards für den Datenschutz und von gesetzlichen Bestimmungen
- Verschiedene Sicherheitsszenarien, z.B. Überwachung und Warnungen bei abweichendem Zugriff auf sensible Daten
- Steuern des Zugriffs auf und Härten der Sicherheit von Datenbanken mit vertraulichen Daten

Die Datenermittlung und -klassifizierung ist Teil des Angebots [Advanced Data Security](sql-database-advanced-data-security.md). Dabei handelt es sich um ein vereinheitlichtes Paket für erweiterte SQL-Sicherheitsfunktionen. Sie finden die Datenermittlung und -klassifizierung über den zentralen Abschnitt **SQL – Advanced Data Security** im Azure-Portal. Dort können Sie diesen Dienst auch verwalten.

> [!NOTE]
> Dieser Artikel bezieht sich auf Azure SQL-Datenbank und Azure Synapse Analytics. Der Einfachheit halber wird *SQL-Datenbank* verwendet, wenn sowohl auf SQL-Datenbank als auch auf Azure Synapse verwiesen wird. Informationen zu SQL Server (lokal) finden Sie unter [SQL-Datenermittlung und -klassifizierung](https://go.microsoft.com/fwlink/?linkid=866999).

## <a name="what-is-data-discovery--classification"></a><a id="what-is-dc"></a>Was ist die Datenermittlung und -klassifizierung?

Mit der Datenermittlung und -klassifizierung wird eine Reihe erweiterter Dienste und neuer SQL-Datenbankfunktionen eingeführt. Sie bildet ein neues Paradigma für den Schutz von Informationen für SQL-Datenbank, das den Schutz der Daten und nicht der Datenbank selbst zum Ziel hat. Dieses Paradigma umfasst Folgendes:

- **Ermittlung und Empfehlungen:** Das Klassifizierungsmodul scannt Ihre Datenbank und identifiziert Spalten, die unter Umständen vertrauliche Daten enthalten. Es bietet dann eine einfache Möglichkeit, die Ergebnisse zu überprüfen und die empfohlenen Klassifizierungen über das Azure-Portal anzuwenden.

- **Bezeichnung:** Sie können Klassifizierungsbezeichnungen nach der Vertraulichkeit der Daten dauerhaft auf Spalten anwenden, indem Sie neue Metadatenattribute nutzen, die der SQL-Datenbank-Engine hinzugefügt wurden. Diese Metadaten können dann für erweiterte Überwachungs- und Schutzszenarien auf Grundlage der Vertraulichkeit genutzt werden.

- **Vertraulichkeit von Abfrageergebnissen:** Die Vertraulichkeit von Abfrageergebnissen wird zu Überwachungszwecken in Echtzeit berechnet.

- **Transparenz:** Der Klassifizierungsstatus der Datenbank kann in einem detaillierten Dashboard im Azure-Portal angezeigt werden. Darüber hinaus können Sie einen Bericht (im Excel-Format) herunterladen, der für Compliance- und Überwachungszwecke und andere Anforderungen verwendet werden kann.

## <a name="discover-classify-and-label-sensitive-columns"></a><a id="discover-classify-columns"></a>Ermitteln, Klassifizieren und Bezeichnen von vertraulichen Spalten

In diesem Abschnitt werden die Schritte für folgende Maßnahmen beschrieben:

- Ermitteln, Klassifizieren und Bezeichnen von Spalten mit vertraulichen Daten in Ihrer Datenbank
- Anzeigen des aktuellen Klassifizierungsstatus Ihrer Datenbank und Exportieren von Berichten

Die Klassifizierung umfasst zwei Metadatenattribute:

- **Bezeichnungen:** Die wichtigsten Klassifizierungsattribute zum Definieren der Vertraulichkeitsstufe der in der Spalte gespeicherten Daten.  
- **Informationstypen:** Attribute mit detaillierteren Informationen zum Typ der in der Spalte gespeicherten Daten.

### <a name="define-and-customize-your-classification-taxonomy"></a>Definieren und Anpassen der Klassifizierungstaxonomie

Datenermittlung und -klassifizierung verfügt über einen integrierten Satz von Vertraulichkeitsbezeichnungen und einen integrierten Satz von Informationstypen und Ermittlungslogik. Sie können diese Taxonomie nun anpassen und eine spezielle Gruppe und Rangfolge von Klassifizierungskonstrukten für Ihre Umgebung definieren.

Die Definition und Anpassung Ihrer Klassifizierungstaxonomie erfolgt an zentraler Stelle für die ganze Azure-Organisation. Diese Stelle befindet sich im [Azure Security Center](https://docs.microsoft.com/azure/security-center/security-center-intro) und ist Bestandteil Ihrer Sicherheitsrichtlinie. Nur Benutzer mit Administratorrechten für die Stammverwaltungsgruppe der Organisation können diese Aufgabe ausführen.

Im Rahmen der Verwaltung von SQL Information Protection-Richtlinien können Sie benutzerdefinierte Bezeichnungen definieren, deren Rangfolge festlegen und sie einer ausgewählten Gruppe von Informationstypen zuordnen. Sie können auch eigene benutzerdefinierte Informationstypen hinzufügen und diese mit Zeichenfolgenmustern konfigurieren. Die Muster werden der Ermittlungslogik hinzugefügt, um diese Datentypen in Ihren Datenbanken zu identifizieren.

Weitere Informationen zum Anpassen und Verwalten Ihrer Richtlinie finden Sie in der [Anleitung zu SQL Information Protection-Richtlinien](https://go.microsoft.com/fwlink/?linkid=2009845&clcid=0x409).

Nachdem die organisationsweite Richtlinie definiert wurde, können Sie die Klassifizierung einzelner Datenbanken mithilfe Ihrer benutzerdefinierten Richtlinie fortsetzen.

### <a name="classify-your-sql-database"></a>Klassifizieren Ihrer SQL-Datenbank

1. Öffnen Sie das [Azure-Portal](https://portal.azure.com).

2. Navigieren Sie im Bereich von Azure SQL-Datenbank unter der Überschrift **Sicherheit** zu **Advanced Data Security**. Wählen Sie **Advanced Data Security** und dann die Karte **Datenermittlung und -klassifizierung** aus.

   ![Bereich „Advanced Data Security“ im Azure-Portal](./media/sql-data-discovery-and-classification/data_classification.png)

3. Die Registerkarte **Übersicht** auf der Seite **Datenermittlung und -klassifizierung** enthält eine Zusammenfassung des aktuellen Klassifizierungsstatus der Datenbank. Die Zusammenfassung enthält eine ausführliche Liste aller klassifizierten Spalten, die Sie auch filtern können, um nur bestimmte Schemateile, Informationstypen und Bezeichnungen anzuzeigen. Wenn Sie noch keine Spalten klassifiziert haben, [fahren Sie mit Schritt 5 fort](#step-5).

   ![Zusammenfassung des aktuellen Klassifizierungsstatus](./media/sql-data-discovery-and-classification/2_data_classification_overview_dashboard.png)

4. Um einen Bericht im Excel-Format herunterzuladen, wählen Sie oben im Fenster im Menü die Option **Exportieren** aus.

5. <a id="step-5"></a>Um mit der Klassifizierung Ihrer Daten zu beginnen, wählen Sie die Registerkarte **Klassifizierung** auf der Seite **Datenermittlung und -klassifizierung** aus.

    Die Klassifizierungs-Engine scannt Ihre Datenbank nach Spalten, die potenziell vertrauliche Daten enthalten, und erstellt eine Liste der empfohlenen Spaltenklassifizierungen.

6. Anzeigen und Anwenden von Klassifizierungsempfehlungen:

   - Um die Liste der empfohlenen Spaltenklassifizierungen anzuzeigen, wählen Sie unten im Bereich den Empfehlungsbereich aus.

   - Zum Akzeptieren einer Empfehlung für eine bestimmte Spalte aktivieren Sie das Kontrollkästchen in der linken Spalte der entsprechenden Zeile. Sie können auch alle Empfehlungen als akzeptiert markieren, indem Sie das Kontrollkästchen ganz links im Tabellenkopf der Empfehlungen aktivieren.

       ![Überprüfen und Auswählen in der Liste der Klassifizierungsempfehlungen](./media/sql-data-discovery-and-classification/6_data_classification_recommendations_list.png)

   - Wählen Sie zum Anwenden der ausgewählten Empfehlungen die Option **Ausgewählte Empfehlungen annehmen** aus.

7. Alternativ oder zusätzlich zur empfehlungsbasierten Klassifizierung können Sie Spalten auch manuell klassifizieren:

   1. Wählen Sie im oberen Menü des Bereichs **Klassifizierung hinzufügen** aus.

   1. Wählen Sie im daraufhin geöffneten Kontextfenster das Schema, die Tabelle und dann die Spalte, die Sie klassifizieren möchten, sowie den Informationstyp und die Vertraulichkeitsbezeichnung aus.

   1. Wählen Sie dann am unteren Rand des Kontextfensters die Option **Klassifizierung hinzufügen** aus.

      ![Auswählen einer zu klassifizierenden Spalte](./media/sql-data-discovery-and-classification/9_data_classification_manual_classification.png)

8. Wählen Sie im oberen Menü des Fensters **Speichern** aus, um Ihre Klassifizierung abzuschließen und die Datenbankspalten dauerhaft mit den neuen Klassifizierungsmetadaten (Tags) zu bezeichnen.

## <a name="audit-access-to-sensitive-data"></a><a id="audit-sensitive-data"></a>Überwachen des Zugriffs auf vertrauliche Daten

Ein wichtiger Aspekt des Paradigmas für den Schutz von Informationen ist die Möglichkeit, den Zugriff auf vertrauliche Daten zu überwachen. Die [Überwachung von Azure SQL-Datenbank](sql-database-auditing.md) wurde erweitert, um ein neues Feld mit dem Namen `data_sensitivity_information` in das Überwachungsprotokoll zu integrieren. Dieses Feld protokolliert die Vertraulichkeitsklassifizierungen (Bezeichnungen) der Daten, die von einer Abfrage zurückgegeben wurden. Hier sehen Sie ein Beispiel:

![Überwachungsprotokoll](./media/sql-data-discovery-and-classification/11_data_classification_audit_log.png)

## <a name="permissions"></a><a id="permissions"></a>Berechtigungen

Diese integrierten Rollen können die Datenklassifizierung einer Azure SQL-Datenbank-Instanz lesen:

- Besitzer
- Leser
- Mitwirkender
- SQL-Sicherheits-Manager
- Benutzerzugriffsadministrator

Diese integrierten Rollen können die Datenklassifizierung einer Azure SQL-Datenbank-Instanz ändern:

- Besitzer
- Mitwirkender
- SQL-Sicherheits-Manager

Erfahren Sie mehr über rollenbasierte Berechtigungen unter [RBAC für Azure-Ressourcen](https://docs.microsoft.com/azure/role-based-access-control/overview).

## <a name="manage-classifications"></a><a id="manage-classification"></a>Verwalten von Klassifizierungen

Sie können zum Verwalten von Klassifizierungen T-SQL, eine REST-API oder PowerShell verwenden.

### <a name="use-t-sql"></a>Verwenden von T-SQL

Mit T-SQL können Sie Spaltenklassifizierungen hinzufügen oder entfernen sowie alle Klassifizierungen für die gesamte Datenbank abrufen.

> [!NOTE]
> Bei der Verwendung von T-SQL zur Verwaltung von Bezeichnungen erfolgt keine Überprüfung, ob die einer Spalte hinzugefügten Bezeichnungen in der Datenschutzrichtlinie der Organisation (die in den Empfehlungen im Portal angezeigt werden) enthalten sind. Aus diesem Grund müssen Sie dies überprüfen.

Weitere Informationen zur Verwendung von T-SQL für Klassifizierungen finden Sie in den folgenden Ressourcen:

- Zum Hinzufügen oder Aktualisieren von Klassifizierungen einer oder mehrere Spalten: [VERTRAULICHKEITSKLASSIFIZIERUNG HINZUFÜGEN](https://docs.microsoft.com/sql/t-sql/statements/add-sensitivity-classification-transact-sql)
- Zum Entfernen der Klassifizierung aus einer oder mehreren Spalten: [VERTRAULICHKEITSKLASSIFIZIERUNG LÖSCHEN](https://docs.microsoft.com/sql/t-sql/statements/drop-sensitivity-classification-transact-sql)
- Zum Anzeigen aller Klassifizierungen in der Datenbank: [sys.sensitivity_classifications](https://docs.microsoft.com/sql/relational-databases/system-catalog-views/sys-sensitivity-classifications-transact-sql)

### <a name="use-the-rest-api"></a>Verwenden der REST-API

Sie können die REST-API verwenden, um Klassifizierungen und Empfehlungen programmgesteuert zu verwalten. Die veröffentlichte REST-API unterstützt die folgenden Vorgänge:

- [Erstellen oder aktualisieren:](https://docs.microsoft.com/rest/api/sql/sensitivitylabels/createorupdate) Erstellt oder aktualisiert die Vertraulichkeitsbezeichnung der angegebenen Spalte.
- [Löschen:](https://docs.microsoft.com/rest/api/sql/sensitivitylabels/delete) Löscht die Vertraulichkeitsbezeichnung der angegebenen Spalte.
- [Deaktivieren von Empfehlungen:](https://docs.microsoft.com/rest/api/sql/sensitivitylabels/disablerecommendation) Deaktiviert Vertraulichkeitsempfehlungen für die angegebene Spalte.
- [Aktivieren von Empfehlungen:](https://docs.microsoft.com/rest/api/sql/sensitivitylabels/enablerecommendation) Aktiviert Vertraulichkeitsempfehlungen für die angegebene Spalte. (Standardmäßig sind Empfehlungen für alle Spalten aktiviert.)
- [Get](https://docs.microsoft.com/rest/api/sql/sensitivitylabels/get) (Abrufen): Ruft die Vertraulichkeitsbezeichnung der angegebenen Spalte ab.
- [Aktuelle nach Datenbank auflisten:](https://docs.microsoft.com/rest/api/sql/sensitivitylabels/listcurrentbydatabase) Ruft die aktuellen Vertraulichkeitsbezeichnungen der angegebenen Datenbank ab.
- [Empfohlene nach Datenbank auflisten:](https://docs.microsoft.com/rest/api/sql/sensitivitylabels/listrecommendedbydatabase) Ruft die empfohlenen Vertraulichkeitsbezeichnungen für die angegebene Datenbank ab.

### <a name="use-powershell-cmdlets"></a>Verwenden von PowerShell-Cmdlets
Sie können PowerShell verwenden, um Klassifizierungen und Empfehlungen für Azure SQL-Datenbank und verwaltete Instanzen zu verwalten.

#### <a name="powershell-cmdlets-for-sql-database"></a>PowerShell-Cmdlets für SQL-Datenbank

- [Get-AzSqlDatabaseSensitivityClassification](https://docs.microsoft.com/powershell/module/az.sql/get-azsqldatabasesensitivityclassification)
- [Set-AzSqlDatabaseSensitivityClassification](https://docs.microsoft.com/powershell/module/az.sql/set-azsqldatabasesensitivityclassification)
- [Remove-AzSqlDatabaseSensitivityClassification](https://docs.microsoft.com/powershell/module/az.sql/remove-azsqldatabasesensitivityclassification)
- [Get-AzSqlDatabaseSensitivityRecommendation](https://docs.microsoft.com/powershell/module/az.sql/get-azsqldatabasesensitivityrecommendation)
- [Enable-AzSqlDatabaSesensitivityRecommendation](https://docs.microsoft.com/powershell/module/az.sql/enable-azsqldatabasesensitivityrecommendation)
- [Disable-AzSqlDatabaseSensitivityRecommendation](https://docs.microsoft.com/powershell/module/az.sql/disable-azsqldatabasesensitivityrecommendation)

#### <a name="powershell-cmdlets-for-managed-instances"></a>PowerShell-Cmdlets für verwaltete Instanzen

- [Get-AzSqlInstanceDatabaseSensitivityClassification](https://docs.microsoft.com/powershell/module/az.sql/get-azsqlinstancedatabasesensitivityclassification)
- [Set-AzSqlInstanceDatabaseSensitivityClassification](https://docs.microsoft.com/powershell/module/az.sql/set-azsqlinstancedatabasesensitivityclassification)
- [Remove-AzSqlInstanceDatabaseSensitivityClassification](https://docs.microsoft.com/powershell/module/az.sql/remove-azsqlinstancedatabasesensitivityclassification)
- [Get-AzSqlInstanceDatabaseSensitivityRecommendation](https://docs.microsoft.com/powershell/module/az.sql/get-azsqlinstancedatabasesensitivityrecommendation)
- [Enable-AzSqlInstanceDatabaseSensitivityRecommendation](https://docs.microsoft.com/powershell/module/az.sql/enable-azsqlinstancedatabasesensitivityrecommendation)
- [Disable-AzSqlInstanceDatabaseSensitivityRecommendation](https://docs.microsoft.com/powershell/module/az.sql/disable-azsqlinstancedatabasesensitivityrecommendation)


## <a name="next-steps"></a><a id="next-steps"></a>Nächste Schritte

- Erfahren Sie mehr über [Advanced Data Security](sql-database-advanced-data-security.md).
- Sie sollten in Betracht ziehen, die [Azure SQL-Datenbank-Überwachung](sql-database-auditing.md) für die Überwachung und Überprüfung des Zugriffs auf Ihre klassifizierten sensiblen Daten zu konfigurieren.
- Eine Präsentation, die die Datenermittlung und -klassifizierung umfasst, finden Sie unter [Discovering, classifying, labeling & protecting SQL data | Data Exposed](https://www.youtube.com/watch?v=itVi9bkJUNc) (Ermitteln, Klassifizieren, Bezeichnen und Schützen von SQL-Daten | Data Exposed).
