---
title: Grundlegendes zu den erforderlichen Rollen zum Ausführen allgemeiner Aufgaben in Synapse
description: In diesem Artikel wird beschrieben, welche integrierten Synapse RBAC-Rollen für bestimmte Aufgaben erforderlich sind.
author: RonyMSFT
ms.service: synapse-analytics
ms.topic: conceptual
ms.subservice: security
ms.date: 12/1/2020
ms.author: ronytho
ms.reviewer: jrasnick
ms.openlocfilehash: 87156914769c2cb76b7509b74ebfeb13b15af95e
ms.sourcegitcommit: 7f59e3b79a12395d37d569c250285a15df7a1077
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/02/2021
ms.locfileid: "110784193"
---
# <a name="understand-the-roles-required-to-perform-common-tasks-in-synapse"></a>Grundlegendes zu den erforderlichen Rollen zum Ausführen allgemeiner Aufgaben in Synapse

Dieser Artikel soll Ihnen helfen, zu verstehen, welche Synapse RBAC- (rollenbasierte Zugriffssteuerung) oder Azure RBAC-Rollen Sie für Ihre Arbeit in Synapse Studio benötigen.  

## <a name="synapse-studio-access-control-and-workflow-summary"></a>Synapse Studio-Zugriffssteuerung und -Workflow – Zusammenfassung 

### <a name="accessing-synapse-studio-and-viewing-its-content"></a>Zugreifen auf Synapse Studio und Anzeigen der Inhalte

- Um Synapse Studio zu öffnen, Details des Arbeitsbereichs anzuzeigen und Azure-Ressourcen (SQL-Pools, Spark-Pools oder Integration Runtimes) aufzulisten, benötigen Sie eine beliebige Synapse RBAC-Rolle oder eine der Rollen „Azure-Besitzer“, „Mitwirkender“ oder „Leser“ für den Arbeitsbereich.

### <a name="resource-management"></a>Ressourcenverwaltung

- Sie können SQL-Pools, Apache Spark-Pools und Integration Runtimes erstellen, wenn Sie für den Arbeitsbereich eine der Rollen „Azure-Besitzer“ oder „Mitwirkender“ haben.
- Für das Anhalten oder Skalieren eines dedizierten SQL-Pools oder das Konfigurieren eines Spark-Pools oder einer Integration Runtime benötigen Sie eine der Rollen „Azure-Besitzer“ oder „Mitwirkender“ für den Arbeitsbereich oder diese Ressource.

### <a name="viewing-and-editing-code-artifacts"></a>Anzeigen und Bearbeiten von Codeartefakten

- Wenn Sie Zugriff auf Synapse Studio haben, können Sie neue Codeartefakte erstellen. Dies sind z. B. SQL-Skripts, Notebooks, Spark-Aufträge, verknüpfte Dienste, Pipelines, Datenflüsse, Trigger und Anmeldeinformationen.  (Diese Artefakte können mit zusätzlichen Berechtigungen auch veröffentlicht oder gespeichert werden.)  
- Wenn Sie „Benutzer von Synapse-Artefakten“, „Herausgeber von Synapse-Artefakten“, „Synapse-Mitwirkender“ oder „Synapse-Administrator“ sind, können Sie bereits veröffentlichte Codeartefakte auflisten, öffnen und bearbeiten.

### <a name="executing-your-code"></a>Ausführen von Code

- Sie können SQL-Skripts für SQL-Pools ausführen, wenn die erforderlichen SQL-Berechtigungen in den SQL-Pools definiert sind.  
- Sie können Notebooks und Spark-Aufträge ausführen, wenn Sie über die Berechtigungen der Rolle „Operator von Synapse-Computeressourcen“ für den Arbeitsbereich oder bestimmte Apache Spark-Pools verfügen.  
- Mit den Berechtigungen „Operator von Synapse-Computeressourcen“ für den Arbeitsbereich oder bestimmte Integration Runtimes sowie den entsprechenden Berechtigungen für Ihre Anmeldeinformationen können Sie Pipelines ausführen.

### <a name="monitoring-and-managing-execution"></a>Überwachen und Verwalten der Ausführung
- Wenn Sie „Synapse-Benutzer“ sind, können Sie den Status ausgeführter Notebooks und Aufträge in Apache Spark-Pools überwachen.
- Sie können Protokolle überprüfen und laufende Aufträge und Pipelines abbrechen, wenn Sie „Operator von Synapse-Computeressourcen“ in diesem Arbeitsbereich oder für einen bestimmten Spark-Pool oder eine bestimmte Pipeline sind.  

### <a name="publishing-and-saving-your-code"></a>Veröffentlichen und Speichern von Code

- Für das Veröffentlichen neuer oder aktualisierter Codeartefakte für den Dienst benötigen Sie eine der Rollen „Herausgeber von Synapse-Artefakten“, „Synapse-Mitwirkender“ oder „Synapse-Administrator“. 
- Sie können Codeartefakte in einen Arbeitsbranch eines Git-Repository committen, wenn Git im Arbeitsbereich aktiviert ist und Sie über Git-Berechtigungen verfügen. Wenn Git aktiviert ist, darf die Veröffentlichung nur im Kollaborationsbranch erfolgen.
- Wenn Sie Synapse Studio schließen, ohne Änderungen an Codeartefakten zu veröffentlichen oder zu committen, gehen diese Änderungen verloren.


## <a name="tasks-and-required-roles"></a>Aufgaben und erforderliche Rollen

In der folgenden Tabelle sind allgemeine Aufgaben und die jeweils erforderlichen Synapse RBAC- oder Azure RBAC-Rollen aufgeführt.  

>[!Note]
>- Die Rolle „Synapse-Administrator“ ist nur bei den Aufgaben aufgeführt, bei denen nur diese Rolle die erforderliche Berechtigung bereitstellt.  Ein Synapse-Administrator kann alle Aufgaben ausführen, die von anderen Synapse RBAC-Rollen gewährt werden.</br>
>- Die mindestens erforderliche Synapse RBAC-Rolle wird angezeigt.
>- Alle Synapse RBAC-Rollen gewähren in jedem Bereich die Berechtigungen der Rolle „Synapse-Benutzer“ im Arbeitsbereich.
>- Alle in der Tabelle gezeigten Synapse RBAC-Berechtigungen/-Aktionen weisen ein entsprechendes Präfix für Microsoft/Synapse/Arbeitsbereiche usw. auf. </br>


Aufgabe |Erforderliche Rolle|Synapse RBAC-Berechtigung/-Aktion
--|--|--
|Öffnen von Synapse Studio in einem Arbeitsbereich|Synapse-Benutzer oder|Lesen
| |Azure-Besitzer, Mitwirkender oder Leser im Arbeitsbereich|Keine
|Auflisten von SQL-Pools, Apache Spark-Pools, Integration Runtimes und Zugreifen auf die zugehörigen Konfigurationsdetails|Synapse-Benutzer oder|Lesen|
||Azure-Besitzer, Mitwirkender oder Leser im Arbeitsbereich|Keine
|Auflisten von verknüpften Diensten, Anmeldeinformationen, verwalteten privaten Endpunkten|Synapse-Benutzer|Lesen
SQL-POOLS|
Erstellen eines dedizierten SQL-Pools oder eines serverlosen SQL-Pools|Azure-Besitzer oder Mitwirkender im Arbeitsbereich|Keine
Verwalten (Anhalten, Skalieren oder Löschen) eines dedizierten SQL-Pools|Azure-Besitzer oder Mitwirkender im SQL-Pool oder Arbeitsbereich|Keine
Erstellen eines SQL-Skripts</br>|Synapse-Benutzer oder </br>Azure-Besitzer oder Mitwirkender im Arbeitsbereich. </br>*Zum Ausführen eines SQL-Skripts und zum Veröffentlichen oder Committen von Änderungen sind zusätzliche SQL-Berechtigungen erforderlich.*|
Auflisten und Öffnen veröffentlichter SQL-Skripts| Benutzer von Synapse-Artefakten, Herausgeber von Synapse-Artefakten, Synapse-Mitwirkender|artifacts/read
Ausführen eines SQL-Skripts in einem serverlosen SQL-Pool|SQL-Berechtigungen für den Pool (werden Synapse-Administratoren automatisch gewährt)|Keine
Ausführen eines SQL-Skripts in einem dedizierten SQL-Pool|SQL-Berechtigungen für den Pool|none
Veröffentlichen neuer, aktualisierter oder gelöschter SQL-Skripts|Herausgeber von Synapse-Artefakten, Synapse-Mitwirkender|sqlScripts/write, delete
Committen von Änderungen an einem SQL-Skript im Git-Repository|Erfordert Git-Berechtigungen für das Repository|
Zuweisen eines Active Directory-Administrators für den Arbeitsbereich (über die Arbeitsbereichseigenschaften im Azure-Portal)|Azure-Besitzer oder Mitwirkender im Arbeitsbereich |
APACHE SPARK-POOLS|
Erstellen eines Apache Spark-Pools|Azure-Besitzer oder Mitwirkender im Arbeitsbereich|
Überwachen von Apache Spark-Anwendungen| Synapse-Benutzer|Lesen
Anzeigen der Protokolle für Notebook- und Auftragsausführungen |Operator von Synapse-Computeressourcen|
Abbrechen der Ausführung von Notebooks oder Spark-Aufträgen in einem Apache Spark-Pool|Operator von Synapse-Computeressourcen für den Apache Spark-Pool|bigDataPools/useCompute
Erstellen einer Notebook- oder Auftragsdefinition|Synapse-Benutzer oder </br>Azure-Besitzer, Mitwirkender oder Leser im Arbeitsbereich</br> *Zum Ausführen, Veröffentlichen oder Committen von Änderungen sind zusätzliche Berechtigungen erforderlich.*|Lesen</br></br></br></br></br> 
Auflisten und Öffnen einer veröffentlichten Notebook- oder Auftragsdefinition, einschließlich der Überprüfung gespeicherter Ausgaben|Benutzer von Synapse-Artefakten, Herausgeber von Synapse-Artefakten, Synapse-Mitwirkender für den Arbeitsbereich|artifacts/read
Ausführen eines Notebooks und Überprüfen seiner Ausgabe oder Übermitteln eines Spark-Auftrags|Synapse-Apache Spark-Administrator, Operator von Synapse-Computeressourcen für den ausgewählten Apache Spark-Pool|bigDataPools/useCompute 
Veröffentlichen oder Löschen einer Notebook- oder Auftragsdefinition (einschließlich Ausgabe) für den Dienst|Herausgeber von Synapse-Artefakten für den Arbeitsbereich, Synapse-Apache Spark-Administrator|notebooks/write, delete
Committen von Änderungen an einer Notebook- oder Auftragsdefinition im Git-Repository|Git-Berechtigungen|none
PIPELINES, INTEGRATION RUNTIMES, DATENFLÜSSE, DATASETS UND TRIGGER|
Erstellen, Aktualisieren oder Löschen einer Integration Runtime|Azure-Besitzer oder Mitwirkender im Arbeitsbereich|
Überwachen des Integration Runtime-Status|Synapse-Benutzer|read, pipelines/viewOutputs
Überprüfen von Pipelineausführungen|Herausgeber von Synapse-Artefakten, Synapse-Mitwirkender|read, pipelines/viewOutputs 
Erstellen einer Pipeline |Synapse-Benutzer</br>*Zum Debuggen, Hinzufügen von Triggern und Veröffentlichen oder Committen von Änderungen sind zusätzliche Synapse-Berechtigungen erforderlich.*|Lesen
Erstellen von Datenflüssen oder Datasets |Synapse-Benutzer</br>*Zum Veröffentlichen oder Committen von Änderungen sind zusätzliche Synapse-Berechtigungen erforderlich.*|Lesen
Auflisten und Öffnen veröffentlichter Pipelines |Benutzer von Synapse-Artefakten | artifacts/read
Anzeigen einer Datenvorschau für Datasets|Synapse-Benutzer und Synapse-Anmeldeinformationsbenutzer für WorkspaceSystemIdentity| 
Debuggen von Pipelines mit der Standard-Integration Runtime|Synapse-Benutzer und Synapse-Anmeldeinformationsbenutzer für WorkspaceSystemIdentity-Anmeldeinformationen|read, </br>credentials/useSecret
Erstellen von Triggern, einschließlich sofortiger Auslösung (erfordert die Berechtigung zum Ausführen der Pipeline)|Synapse-Benutzer und Synapse-Anmeldeinformationsbenutzer für WorkspaceSystemIdentity|read, credentials/useSecret/action
Ausführen von Pipelines|Synapse-Benutzer und Synapse-Anmeldeinformationsbenutzer für WorkspaceSystemIdentity|read, credentials/useSecret/action
Kopieren von Daten mithilfe des Tools zum Kopieren von Daten|Synapse-Benutzer und Synapse-Anmeldeinformationsbenutzer für die Systemidentität im Arbeitsbereich|read, credentials/useSecret/action
Erfassen von Daten (mit einem Zeitplan)|Synapse-Ersteller und Synapse-Anmeldeinformationsbenutzer für die Systemidentität im Arbeitsbereich|read, credentials/useSecret/action
Veröffentlichen neuer, aktualisierter oder gelöschter Pipelines, Datenflüsse oder Trigger für den Dienst|Herausgeber von Synapse-Artefakten für den Arbeitsbereich|pipelines/write, delete</br>dataflows/write, delete</br>triggers/write, delete
Committen von Änderungen an Pipelines, Datenflüssen, Datasets oder Triggern im Git-Repository |Git-Berechtigungen|none 
VERKNÜPFTE DIENSTE|
Erstellen verknüpfter Dienste (einschließlich Zuweisung von Anmeldeinformationen)|Synapse-Benutzer</br>*Zum Verwenden eines verknüpften Diensts mit Anmeldeinformationen oder zum Veröffentlichen oder Committen von Änderungen sind zusätzliche Berechtigungen erforderlich.*|Lesen
Auflisten und Öffnen veröffentlichter verknüpfter Dienste|Benutzer von Synapse-Artefakten|linkedServices/write, delete  
Testen der Verbindung für einen verknüpften Dienst, der durch Anmeldeinformationen geschützt ist|Synapse-Benutzer und Synapse-Anmeldeinformationsbenutzer|credentials/useSecret/action|
Veröffentlichen verknüpfter Dienste|Herausgeber von Synapse-Artefakten, Synapse-Manager für verknüpfte Daten|linkedServices/write, delete
Committen von Definitionen verknüpfter Dienste im Git-Repository|Git-Berechtigungen|none
ZUGRIFFSVERWALTUNG|
Überprüfen der Synapse RBAC-Rollenzuweisungen in einem beliebigen Bereich|Synapse-Benutzer|Lesen
Zuweisen und Entfernen von Synapse RBAC-Rollenzuweisungen für Benutzer, Gruppen und Dienstprinzipale| Synapse-Administrator für den Arbeitsbereich oder einen bestimmten Bereich für Arbeitsbereichselemente|roleAssignments/write, delete 

>[!Note]
>Gastbenutzer aus einem anderen Mandanten können ebenfalls Rollenzuweisungen überprüfen, hinzufügen oder ändern, nachdem sie als Synapse-Administrator zugewiesen wurden. 

## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen finden Sie unter [Überprüfen von Synapse RBAC-Rollenzuweisungen](./how-to-review-synapse-rbac-role-assignments.md).

Weitere Informationen finden Sie unter [Verwalten von Synapse RBAC-Rollenzuweisungen](./how-to-manage-synapse-rbac-role-assignments.md). 
