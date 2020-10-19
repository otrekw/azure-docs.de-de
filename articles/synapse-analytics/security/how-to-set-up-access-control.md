---
title: Sichern Ihres Synapse-Arbeitsbereichs (Vorschau)
description: In diesem Artikel erfahren Sie, wie Sie Rollen und Zugriffssteuerung verwenden, um Aktivitäten und den Zugriff auf Daten im Synapse-Arbeitsbereich zu kontrollieren.
services: synapse-analytics
author: matt1883
ms.service: synapse-analytics
ms.topic: how-to
ms.subservice: security
ms.date: 04/15/2020
ms.author: mahi
ms.reviewer: jrasnick
ms.openlocfilehash: 35fb8adaa5f7c0fff1c6d967f0136736b8071ce4
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/09/2020
ms.locfileid: "91260154"
---
# <a name="secure-your-synapse-workspace-preview"></a>Sichern Ihres Synapse-Arbeitsbereichs (Vorschau)

In diesem Artikel erfahren Sie, wie Sie Rollen und Zugriffssteuerung verwenden, um Aktivitäten und den Zugriff auf Daten zu kontrollieren. Wenn Sie diese Anweisungen befolgen, vereinfacht dies die Zugriffssteuerung in Azure Synapse Analytics. Sie müssen Benutzer nur einer von drei Sicherheitsgruppen hinzufügen bzw. daraus entfernen.

## <a name="overview"></a>Übersicht

Um einen Synapse-Arbeitsbereich (Vorschau) zu sichern, befolgen Sie ein Muster zur Konfiguration der folgenden Elemente:

- Azure-Rollen (z. B. die integrierten Rollen wie „Besitzer“, „Mitwirkender“ usw.)
- Synapse-Rollen – diese Rollen sind für Synapse eindeutig und basieren nicht auf Azure-Rollen. Es gibt drei solche Rollen:
  - Synapse-Arbeitsbereichsadministrator
  - Synapse SQL-Administrator
  - Apache Spark für Azure Synapse Analytics-Administrator
- Zugriffssteuerung für Daten in Azure Data Lake Storage Gen 2 (ADLSGEN2).
- Zugriffssteuerung für Synapse-SQL- und Spark-Datenbanken

## <a name="steps-to-secure-a-synapse-workspace"></a>Schritte zur Sicherung eines Synapse-Arbeitsbereichs

In diesem Dokument werden Standardnamen verwendet, um die Anweisungen zu vereinfachen. Ersetzen Sie sie durch Namen Ihrer Wahl.

|Einstellung | Beispielwert | BESCHREIBUNG |
| :------ | :-------------- | :---------- |
| **Synapse-Arbeitsbereich** | WS1 |  Der Name, den der Synapse-Arbeitsbereich erhält. |
| **ADLSGEN2-Konto** | STG1 | Das ADLS-Konto, das mit Ihrem Arbeitsbereich verwendet werden soll. |
| **Container** | CNT1 | Der Container in STG1, der vom Arbeitsbereich standardmäßig verwendet wird. |
| **Active Directory-Mandant** | contoso | Der Name des Active Directory-Mandanten.|
||||

## <a name="step-1-set-up-security-groups"></a>SCHRITT 1: Einrichten von Sicherheitsgruppen

Erstellen Sie drei Sicherheitsgruppen für Ihren Arbeitsbereich, und füllen Sie sie auf:

- **WS1\_WSAdmins**: für Benutzer, die vollständige Kontrolle über den Arbeitsbereich benötigen.
- **WS1\_SparkAdmins**: für Benutzer, die vollständige Kontrolle über die Spark-Aspekte des Arbeitsbereichs benötigen.
- **WS1\_SQLAdmins**: für Benutzer, die vollständige Kontrolle über die SQL-Aspekte des Arbeitsbereichs benötigen.
- Hinzufügen von **WS1\_WSAdmins** zu **WS1\_SQLAdmins**
- Hinzufügen von **WS1\_WSAdmins** zu **WS1\_SparkAdmins**

## <a name="step-2-prepare-your-data-lake-storage-gen2-account"></a>SCHRITT 2: Vorbereiten Ihres Data Lake Storage Gen2-Kontos

Ermitteln Sie diese Informationen über Ihren Speicher:

- Das ADLSGEN2-Konto, das mit Ihrem Arbeitsbereich verwendet werden soll. In diesem Dokument heißt es STG1.  STG1 wird als „primäres“ Speicherkonto für Ihren Arbeitsbereich betrachtet.
- Der Container innerhalb von WS1, der von Ihrem Arbeitsbereich standardmäßig verwendet wird. In diesem Dokument heißt er CNT1.  Dieser Container wird für Folgendes verwendet:
  - Speichern der Unterstützungsdatendateien für Spark-Tabellen
  - Ausführungsprotokolle für Spark-Aufträge

- Weisen Sie über das Azure-Portal den Sicherheitsgruppen die folgenden Rollen für CNT1 zu.

  - Weisen Sie **WS1\_WSAdmins** zur Rolle **Mitwirkender an Storage-Blobdaten** zu.
  - Wählen Sie **WS1\_SparkAdmins** zur Rolle **Mitwirkender an Storage-Blobdaten** zu.
  - Wählen Sie **WS1\_SQLAdmins** zur Rolle **Mitwirkender an Storage-Blobdaten** zu.

## <a name="step-3-create-and-configure-your-synapse-workspace"></a>SCHRITT 3: Erstellen und Konfigurieren Ihres Synapse-Arbeitsbereichs

Erstellen Sie im Azure-Portal einen Synapse-Arbeitsbereich:

- Nennen Sie den Arbeitsbereich WS1.
- Wählen Sie STG1 als Speicherkonto aus.
- Wählen Sie CNT1 als den Container aus, der als „Dateisystem“ verwendet wird.
- Öffnen Sie WS1 in Synapse Studio.
- Wählen Sie **Verwalten** > **Zugriffssteuerung** aus, und weisen Sie die Sicherheitsgruppen den folgenden Synapse-Rollen zu.
  - Weisen Sie **WS1\_WSAdmins** zu „Synapse-Arbeitsbereichsadministratoren“ zu.
  - Weisen Sie **WS1\_SparkAdmins** zu „Synapse Spark-Administratoren“ zu.
  - Weisen Sie **WS1\_SQLAdmins** zu „Synapse SQL-Administratoren“ zu.

## <a name="step-4-configure-data-lake-storage-gen2-for-use-by-synapse-workspace"></a>SCHRITT 4: Konfigurieren von Data Lake Storage Gen2 für die Verwendung durch den Synapse-Arbeitsbereich

Der Synapse-Arbeitsbereich benötigt Zugriff auf STG1 und CNT1, damit er Pipelines ausführen und Systemaufgaben ausführen kann.

- Öffnen Sie das Azure-Portal.
- Suchen Sie STG1.
- Navigieren Sie zu CNT1.
- Stellen Sie sicher, dass die MSI (Managed Service Identity, Verwaltete Dienstidentität) für WS1 der Rolle **Mitwirkender an Storage-Blobdaten** für CNT1 zugewiesen ist.
  - Wenn die Zuweisung nicht angezeigt wird, nehmen Sie die Zuweisung vor.
  - Der MSI trägt denselben Namen wie der Arbeitsbereich. In diesem Fall wäre dies &quot;WS1&quot;.

## <a name="step-5-configure-admin-access-for-sql-pools"></a>SCHRITT 5: Konfigurieren des Administratorzugriffs für SQL-Pools

- Öffnen Sie das Azure-Portal.
- Navigieren Sie zu WS1.
- Wählen Sie unter **Einstellungen** die Option **SQL Active Directory-Administrator** aus.
- Wählen Sie **Administrator festlegen** und dann „WS1\_SQLAdmins“ aus.

## <a name="step-6-maintain-access-control"></a>SCHRITT 6: Verwalten der Zugriffssteuerung

Die Konfiguration ist fertig gestellt.

Um den Zugriff für Benutzer zu verwalten, können Sie nun Benutzer zu den drei Sicherheitsgruppen hinzufügen und daraus entfernen.

Obwohl Sie Benutzer manuell den Synapse-Rollen zuweisen können, werden die Einstellungen, wenn Sie dies tun, nicht konsistent konfiguriert. Fügen Sie stattdessen Benutzer nur den Sicherheitsgruppen hinzu, oder entfernen Sie sie daraus.

## <a name="step-7-verify-access-for-users-in-the-roles"></a>SCHRITT 7: Überprüfen des Zugriffs für Benutzer in den Rollen

Benutzer in den einzelnen Rollen müssen die folgenden Schritte durchführen:

| Number | Schritt | Arbeitsbereichsadministratoren | Spark-Administratoren | SQL-Administratoren |
| --- | --- | --- | --- | --- |
| 1 | Hochladen einer Parquet-Datei in CNT1 | YES | YES | YES |
| 2 | Lesen der Parquet-Datei mithilfe von SQL On-Demand | YES | Nein | YES |
| 3 | Erstellen eines Spark-Pools | JA [1] | JA [1] | Nein  |
| 4 | Lesen der Parquet-Datei mit einem Notebook | YES | YES | Nein |
| 5 | Erstellen einer Pipeline aus dem Notebook und sofortiges Auslösen der Ausführung der Pipeline | YES | Nein | Nein |
| 6 | Erstellen eines SQL-Pools und Ausführen eines SQL-Skripts wie &quot;SELECT 1&quot; | JA [1] | Nein | JA[1] |

> [!NOTE]
> [1] Um SQL- oder Spark-Pools zu erstellen, muss der Benutzer mindestens über die Rolle „Mitwirkender“ für den Synapse-Arbeitsbereich verfügen.
>
 
>[!TIP]
> - Einige Schritte sind in Abhängigkeit von der Rolle absichtlich nicht zulässig.
> - Beachten Sie, dass einige Aufgaben möglicherweise fehlschlagen, wenn die Sicherheit nicht vollständig konfiguriert wurde. Diese Aufgaben sind in der Tabelle vermerkt.

## <a name="step-8-network-security"></a>SCHRITT 8: Netzwerksicherheit

So konfigurieren Sie die Arbeitsbereichsfirewall, das virtuelle Netzwerk und den [privaten Link](../../azure-sql/database/private-endpoint-overview.md).

## <a name="step-9-completion"></a>SCHRITT 9: Completion

Ihr Arbeitsbereich ist jetzt vollständig konfiguriert und geschützt.

## <a name="how-roles-interact-with-synapse-studio"></a>Interaktionsweise von Rollen mit Synapse Studio

Synapse Studio verhält sich in Abhängigkeit von der jeweiligen Benutzerrolle anders. Einige Elemente sind möglicherweise ausgeblendet oder deaktiviert, wenn ein Benutzer nicht den Rollen zugewiesen ist, die den erforderlichen Zugriff gewähren. In der folgenden Tabelle werden die Auswirkungen auf Synapse Studio zusammengefasst.

| Aufgabe | Arbeitsbereichsadministratoren | Spark-Administratoren | SQL-Administratoren |
| --- | --- | --- | --- |
| Öffnen von Synapse Studio | YES | YES | YES |
| Anzeigen des Start-Hubs | YES | YES | YES |
| Anzeigen des Datenhubs | YES | YES | YES |
| Datenhub/Anzeigen verknüpfter ADLS Gen2-Konten und -Container | JA [1] | JA[1] | JA[1] |
| Datenhub/Anzeigen von Datenbanken | YES | YES | YES |
| Datenhub/Anzeigen von Objekten in Datenbanken | YES | YES | YES |
| Datenhub/Zugreifen auf Daten in SQL-Pooldatenbanken | YES   | Nein   | YES   |
| Datenhub/Zugreifen auf Daten in SQL-on-Demand-Datenbanken | JA [2]  | Nein  | JA [2]  |
| Datenhub/Zugreifen auf Daten in Spark-Datenbanken | JA [2] | JA [2] | JA [2] |
| Verwenden des Entwicklungshubs | YES | YES | YES |
| Entwicklungshub/Verfassen von SQL-Skripts | YES | Nein | YES |
| Entwicklungshub/Verfassen von Spark-Auftragsdefinitionen | YES | YES | Nein |
| Entwicklungshub/Verfassen von Notebooks | YES | YES | Nein |
| Entwicklungshub/Verfassen von Datenflüssen | YES | Nein | Nein |
| Verwenden des Orchestratorhubs | YES | YES | YES |
| Orchestratorhub/Verwenden von Pipelines | YES | Nein | Nein |
| Verwenden des Verwaltungshubs | YES | YES | YES |
| Verwaltungshub/SQL-Pools | YES | Nein | YES |
| Verwaltungshub/Spark-Pools | YES | YES | Nein |
| Verwaltungshub/Trigger | YES | Nein | Nein |
| Verwaltungshub/Verknüpfte Dienste | YES | YES | YES |
| Verwaltungshub/Zugriffssteuerung (Zuweisen von Benutzern zu Synapse-Arbeitsbereichsrollen) | YES | Nein | Nein |
| Verwaltungshub/Integration Runtimes | YES | YES | YES |
| Verwenden des Überwachungshubs | YES | YES | YES |
| Überwachungshub/Orchestrierung/Pipelineausführungen  | YES | Nein | Nein |
| Überwachungshub/Orchestrierung/Triggerausführungen  | YES | Nein | Nein |
| Überwachungshub/Orchestrierung/Integration Runtimes  | YES | YES | YES |
| Überwachungshub/Aktivitäten/Spark-Anwendungen | YES | YES | Nein  |
| Überwachungshub/Aktivitäten/SQL-Anforderungen | YES | Nein | YES |
| Überwachungshub/Aktivitäten/Spark-Pools | YES | YES | Nein  |
| Überwachungshub/Trigger | YES | Nein | Nein |
| Verwaltungshub/Verknüpfte Dienste | YES | YES | YES |
| Verwaltungshub/Zugriffssteuerung (Zuweisen von Benutzern zu Synapse-Arbeitsbereichsrollen) | YES | Nein | Nein |
| Verwaltungshub/Integration Runtimes | YES | YES | YES |


> [!NOTE]
> [1] Der Zugriff auf Daten in Containern hängt von der Zugriffssteuerung in ADLS Gen2 ab. </br>
> [2] SQL OD-Tabellen und Spark-Tabellen speichern ihre Daten in ADLS Gen2, und für den Zugriff sind die entsprechenden Berechtigungen für ADLS Gen2 erforderlich.

## <a name="next-steps"></a>Nächste Schritte

Erstellen eines [Synapse-Arbeitsbereichs](../quickstart-create-workspace.md)
