---
title: Registrieren einer Teradata-Quelle und Einrichten von Überprüfungen (Vorschauversion)
description: In diesem Artikel erfahren Sie, wie Sie eine Teradata-Quelle in Azure Purview registrieren und eine Überprüfung einrichten.
author: chandrakavya
ms.author: kchandra
ms.service: purview
ms.subservice: purview-data-catalog
ms.topic: overview
ms.date: 12/06/2020
ms.openlocfilehash: 170ece293f8d24f3a29774c64c14f9334fe0930c
ms.sourcegitcommit: 8b4b4e060c109a97d58e8f8df6f5d759f1ef12cf
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 12/07/2020
ms.locfileid: "96841651"
---
# <a name="register-and-scan-teradata-source-preview"></a>Registrieren und Überprüfen einer Teradata-Quelle (Vorschauversion)

In diesem Artikel erfahren Sie, wie Sie eine Teradata-Quelle in Purview registrieren und eine Überprüfung einrichten.

> [!IMPORTANT]
> Diese Datenquelle befindet sich derzeit in der Vorschauphase. Sie können ihn ausprobieren und uns Feedback geben.

## <a name="supported-capabilities"></a>Unterstützte Funktionen

Die Teradata-Quelle unterstützt die **vollständige Überprüfung**, um Metadaten aus einer Teradata-Datenbank zu extrahieren, und ruft die **Herkunft** zwischen Datasets ab.

## <a name="prerequisites"></a>Voraussetzungen

- Von dem Connector werden nur Datenspeicher in einem lokalen Netzwerk, in einem virtuellen Azure-Netzwerk oder in der virtuellen privaten Amazon-Cloud unterstützt. Für die Verbindungsherstellung muss daher eine [selbstgehostete Integration Runtime](manage-integration-runtimes.md) eingerichtet werden.

- Stellen Sie sicher, dass die Java Runtime Environment (JRE) auf dem virtuellen Computer installiert ist, auf dem auch die selbst gehostete Integration Runtime installiert ist.
 
- Stellen Sie sicher, dass „Visual C++ Redistributable 2012 Update 4“ auf dem Computer mit der selbstgehosteten Integration Runtime installiert ist. Sollte diese Komponente noch nicht installiert sein, können Sie sie [hier](https://www.microsoft.com/download/details.aspx?id=30679) herunterladen.

- Auf Ihrem lokalen virtuellen Computer muss manuell ein Treiber namens „Teradata JDBC Driver“ installiert werden. Die ausführbare JAR-Datei kann von der [Teradata-Website](https://downloads.teradata.com/) heruntergeladen werden.

    > [!Note] 
    > Der Treiber muss für alle Konten des virtuellen Computers zugänglich sein. Installieren Sie ihn nicht in einem Benutzerkonto.

- Die Teradata-Datenbankversionen **12.x bis 16.x** werden unterstützt. Stellen Sie sicher, dass Sie für die zu überprüfende Teradata-Quelle über Lesezugriff verfügen.

### <a name="feature-flag"></a>Featureflag

Die Registrierung und Überprüfung einer Teradata-Quelle ist hinter einem Featureflag verfügbar. Fügen Sie Folgendes an die URL an: *?feature.ext.datasource=%7b"teradata":"true"%7d* 

Beispiel: vollständige URL [https://web.purview.azure.com/?feature.ext.datasource=%7b"metadata":"true"%7d](https://web.purview.azure.com/?feature.ext.datasource=%7b"teradata":"true"%7d)

## <a name="setting-up-authentication-for-a-scan"></a>Einrichten der Authentifizierung für eine Überprüfung
Als Authentifizierung für eine Teradata-Quelle kann ausschließlich die **Standard-Datenbankauthentifizierung** eingerichtet werden.

## <a name="register-a-teradata-source"></a>Registrieren einer Teradata-Quelle

Gehen Sie wie folgt vor, um in Ihrem Datenkatalog eine neue Teradata-Quelle zu registrieren:

1. Navigieren Sie zu Ihrem Purview-Konto.
2. Wählen Sie im linken Navigationsbereich die Option **Quellen** aus.
3. Wählen Sie **Registrieren** aus.
4. Wählen Sie unter **Register sources** (Quellen registrieren) die Option **Teradata** aus.
5. Wählen Sie **Weiter**.

Gehen Sie auf dem Bildschirm **Register sources (Teradata)** (Quellen registrieren (Teradata)) wie folgt vor:

1. Geben Sie unter **Name** einen Namen ein, unter dem die Datenquelle im Katalog aufgeführt werden soll.
2. Geben Sie unter **Host** den Hostnamen für die Verbindungsherstellung mit einer Teradata-Quelle ein. Hierbei kann es sich auch um eine IP-Adresse oder um eine vollqualifizierte Verbindungszeichenfolge für den Server handeln.
3. Optional: Wählen Sie eine Sammlung aus, oder erstellen Sie eine neue Sammlung.
4. Wählen Sie **Fertig stellen** aus, um die Datenquelle zu registrieren.

    :::image type="content" source="media/register-scan-teradata-source/register-sources.png" alt-text="Optionen für die Quellenregistrierung" border="true":::

## <a name="creating-and-running-a-scan"></a>Erstellen und Ausführen einer Überprüfung

Gehen Sie zum Erstellen und Ausführen einer neuen Überprüfung wie folgt vor:
1. Klicken Sie im Verwaltungscenter auf *Integration Runtimes*. Vergewissern Sie sich, dass eine selbstgehosteten Integration Runtime eingerichtet ist. Falls nicht, führen Sie die [hier](manage-integration-runtimes.md) beschriebenen Schritte aus, um eine selbstgehostete Integration Runtime für Überprüfungen auf einem lokalen virtuellen Computer oder auf einem virtuellen Azure-Computer mit Zugriff auf Ihr lokales Netzwerk zu erstellen.

2. Navigieren Sie anschließend zu **Quellen**.

3. Wählen Sie die registrierte Teradata-Quelle aus.

4. Wählen Sie „+ New scan“ (+ Neue Überprüfung) aus.
 
5. Geben Sie folgende Details an:

    - Name: Der Name der Überprüfung.

    - Verbindung über Integration Runtime herstellen: Wählen Sie die konfigurierte selbstgehostete Integration Runtime aus.

    - Authentifizierungsmethode: Aktuell wird nur die Datenbankauthentifizierung unterstützt. Diese Option ist standardmäßig ausgewählt.

    - Benutzername: Ein Benutzername für die Verbindungsherstellung mit dem Datenbankserver. Dieser Benutzername benötigt Lesezugriff auf den Server.

    - Password (Kennwort): Das Benutzerkennwort für die Verbindungsherstellung mit dem Datenbankserver.

    - Schema: Eine durch Semikolons getrennte Liste mit einer Teilmenge von Schemas, die importiert werden sollen. Beispiel: Schema1; Schema2. Ist diese Liste leer, werden alle Benutzerschemas importiert. Alle Systemschemas (beispielsweise „SysAdmin“) und Objekte werden standardmäßig ignoriert.

        Zu den zulässigen Schemanamensmustern mit SQL-LIKE-Ausdruckssyntax zählt auch die Verwendung von „%“ wie im folgenden Beispiel: A%; %B; %C%; D
        - beginnt mit „A“ oder    
        - endet mit „B“ oder    
        - enthält „C“ oder    
        - gleich „D“

        Die Verwendung von „NOT“ oder Sonderzeichen ist nicht zulässig.

    - „Driver location“ (Speicherort des Treibers): Vollständiger Pfad des Teradata-Treiberspeicherorts auf dem virtuellen Computer des Kunden. Der Name des Teradata-JDBC-Treibers muss „com.teradata.jdbc.TeraDriver“ lauten.

    - „Maximum memory available“ (Maximal verfügbarer Arbeitsspeicher): Maximaler Arbeitsspeicher (in GB), der auf dem virtuellen Computer des Kunden für Überprüfungsprozesse verfügbar ist. Abhängig von der Größe der zu überprüfenden Teradata-Quelle.

    > [!Note] 
    > Faustregel: 2 GB Arbeitsspeicher pro 1.000 Tabellen.

    :::image type="content" source="media/register-scan-teradata-source/setup-scan.png" alt-text="Einrichten der Überprüfung" border="true":::

6. Klicken Sie auf *Weiter*.

7. Wählen Sie den Auslöser für die Überprüfung. Sie können einen Zeitplan einrichten oder die Überprüfung einmalig ausführen.

    :::image type="content" source="media/register-scan-teradata-source/scan-trigger.png" alt-text="Auslöser für die Überprüfung" border="true":::

8. Sehen Sie sich Ihre Überprüfung noch einmal an, und klicken Sie dann auf *Speichern und ausführen*.

## <a name="viewing-your-scans-and-scan-runs"></a>Anzeigen Ihrer Überprüfungen und Überprüfungsausführungen

1. Navigieren Sie zum Verwaltungscenter. Wählen Sie im Abschnitt **Sources and scanning** (Quellen und Überprüfung) die Option **Datenquellen** aus.

2. Wählen Sie die gewünschte Datenquelle aus. Daraufhin wird eine Liste mit vorhandenen Überprüfungen für diese Datenquelle angezeigt.

3. Wählen Sie die Überprüfung aus, für deren Ergebnisse Sie sich interessieren.

4. Auf dieser Seite werden alle vorherigen Überprüfungsausführungen sowie die Metriken und der Status der jeweiligen Überprüfungsausführung angezeigt. Außerdem sehen Sie hier, ob es sich um eine geplante oder um eine manuelle Überprüfung gehandelt hat, auf wie viele Ressourcen Klassifizierungen angewendet wurden, wie viele Ressourcen insgesamt ermittelt wurden, wann die Überprüfung gestartet und beendet wurde und wie lange die Überprüfung insgesamt gedauert hat.

## <a name="manage-your-scans"></a>Verwalten Ihrer Überprüfungen

Gehen Sie zum Verwalten oder Löschen einer Überprüfung wie folgt vor:

1. Navigieren Sie zum Verwaltungscenter. Wählen Sie im Abschnitt **Sources and scanning** (Quellen und Überprüfung) die Option **Datenquellen** und anschließend die gewünschte Datenquelle aus.

2. Wählen Sie die Überprüfung aus, die Sie verwalten möchten. Wenn Sie die Überprüfung bearbeiten möchten, wählen Sie **Bearbeiten** aus.

3. Wenn Sie Ihre Überprüfung löschen möchten, wählen Sie **Löschen** aus.

## <a name="next-steps"></a>Nächste Schritte

- [Browsen im Azure Purview-Datenkatalog](how-to-browse-catalog.md)
- [Suchen im Azure Purview-Datenkatalog](how-to-search-catalog.md)
