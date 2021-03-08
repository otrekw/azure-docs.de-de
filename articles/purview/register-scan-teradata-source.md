---
title: Registrieren einer Teradata-Quelle und Einrichten von Überprüfungen (Vorschauversion) in Azure Purview
description: In diesem Artikel erfahren Sie, wie Sie eine Teradata-Quelle in Azure Purview registrieren und eine Überprüfung einrichten.
author: chandrakavya
ms.author: kchandra
ms.service: purview
ms.subservice: purview-data-catalog
ms.topic: overview
ms.date: 2/25/2021
ms.openlocfilehash: 0885870497ef3488d78fe899c96ee2a82a5b84fc
ms.sourcegitcommit: c27a20b278f2ac758447418ea4c8c61e27927d6a
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/03/2021
ms.locfileid: "101696090"
---
# <a name="register-and-scan-teradata-source-preview"></a>Registrieren und Überprüfen einer Teradata-Quelle (Vorschauversion)

In diesem Artikel erfahren Sie, wie Sie eine Teradata-Quelle in Purview registrieren und eine Überprüfung einrichten.

## <a name="supported-capabilities"></a>Unterstützte Funktionen

Die Teradata-Quelle unterstützt die **vollständige Überprüfung**, um Metadaten aus einer Teradata-Datenbank zu extrahieren, und ruft die **Herkunft** zwischen Datasets ab.

## <a name="prerequisites"></a>Voraussetzungen

1.  Richten Sie die neueste [selbstgehostete Integration Runtime](https://www.microsoft.com/download/details.aspx?id=39717) ein.
    Weitere Informationen finden Sie unter [Erstellen und Konfigurieren einer selbstgehosteten Integration Runtime](https://docs.microsoft.com/azure/data-factory/create-self-hosted-integration-runtime).

2.  Stellen Sie sicher, dass [JDK 11](https://www.oracle.com/java/technologies/javase-jdk11-downloads.html) auf dem virtuellen Computer installiert ist, auf dem auch die selbstgehostete Integration Runtime installiert ist.

3.  Stellen Sie sicher, dass \"Visual C++ Redistributable 2012 Update 4\" auf dem Computer mit der selbstgehosteten Integration Runtime installiert ist. Sollte diese Komponente noch nicht installiert sein, können Sie sie [hier](https://www.microsoft.com/download/details.aspx?id=30679) herunterladen.

4.  Sie müssen den JDBC-Treiber von Teradata manuell auf den virtuellen Computer herunterladen, auf dem die selbstgehostete Integration Runtime ausgeführt wird.
    Die ausführbare JAR-Datei kann von der Teradata-[Website](https://downloads.teradata.com/) heruntergeladen werden.

    > [!Note]
    > Der Treiber muss für alle Konten des virtuellen Computers zugänglich sein. Installieren Sie ihn nicht in einem Benutzerkonto.

5.  Die Teradata-Datenbankversionen 12.x bis 16.x werden unterstützt. Stellen Sie sicher, dass Sie für die zu überprüfende Teradata-Quelle über Lesezugriff verfügen.

## <a name="setting-up-authentication-for-a-scan"></a>Einrichten der Authentifizierung für eine Überprüfung

Die einzige unterstützte Authentifizierung für eine Teradata-Quelle ist die **Standardauthentifizierung**.

## <a name="register-a-teradata-source"></a>Registrieren einer Teradata-Quelle

Gehen Sie wie folgt vor, um in Ihrem Datenkatalog eine neue Teradata-Quelle zu registrieren:

1.  Navigieren Sie zu Ihrem Purview-Konto.
2.  Wählen Sie im linken Navigationsbereich die Option **Quellen** aus.
3.  Wählen Sie **Registrieren** aus.
4.  Wählen Sie unter „Quellen registrieren“ die Option **Teradata** aus. Wählen Sie **Weiter** aus.

    :::image type="content" source="media/register-scan-teradata-source/register-sources.png" alt-text="Teradata-Registrierung" border="true":::

Gehen Sie auf dem Bildschirm **Register sources (Teradata)** (Quellen registrieren (Teradata)) wie folgt vor:

1.  Geben Sie unter **Name** einen Namen ein, unter dem die Datenquelle im Katalog aufgeführt werden soll.

2.  Geben Sie unter **Host** den Hostnamen für die Verbindungsherstellung mit einer Teradata-Quelle ein. Hierbei kann es sich auch um eine IP-Adresse oder um eine vollqualifizierte Verbindungszeichenfolge für den Server handeln.

3.  Optional: Wählen Sie eine Sammlung aus, oder erstellen Sie eine neue Sammlung.

4.  Wählen Sie „Fertig stellen“ aus, um die Datenquelle zu registrieren.

    :::image type="content" source="media/register-scan-teradata-source/register-sources-2.png" alt-text="Registrieren von Teradata" border="true":::

## <a name="creating-and-running-a-scan"></a>Erstellen und Ausführen einer Überprüfung

Gehen Sie zum Erstellen und Ausführen einer neuen Überprüfung wie folgt vor:

1.  Klicken Sie im Verwaltungscenter auf **Integration Runtimes**. Vergewissern Sie sich, dass eine selbstgehosteten Integration Runtime eingerichtet ist. Falls sie nicht eingerichtet ist, richten Sie mit den [hier](https://docs.microsoft.com/azure/purview/manage-integration-runtimes) beschriebenen Schritten eine selbstgehostete Integration Runtime ein.

2.  Navigieren Sie zu **Quellen**.

3.  Wählen Sie die registrierte Teradata-Quelle aus.

4.  Wählen Sie **+ New scan** (+ Neue Überprüfung) aus.

5.  Geben Sie folgende Details an:

    a.  **Name**: Der Name der Überprüfung.

    b.  **Verbindung über Integration Runtime herstellen**: Wählen Sie die konfigurierte selbstgehostete Integration Runtime aus.

    c.  **Anmeldeinformationen**: Wählen Sie die Anmeldeinformationen zur Herstellung der Verbindung mit Ihrer Datenquelle aus. Stellen Sie sicher, dass Sie:

    -   beim Erstellen von Anmeldeinformationen die Standardauthentifizierung auswählen.
    -   im Eingabefeld „Benutzername“ einen Benutzernamen zur Herstellung der Verbindung mit dem Datenbankserver eingeben.
    -   das Kennwort für den Datenbankserver im geheimen Schlüssel speichern.

        Weitere Informationen zu Anmeldeinformationen finden Sie [hier](https://docs.microsoft.com/azure/purview/manage-credentials).

6.  **Schema**: Eine durch Semikolons getrennte Liste mit einer Teilmenge von Schemas, die importiert werden sollen. Beispiel: Schema1; Schema2. Ist diese Liste leer, werden alle Benutzerschemas importiert. Alle Systemschemas (beispielsweise „SysAdmin“) und Objekte werden standardmäßig ignoriert. Wenn die Liste leer ist, werden alle verfügbaren Schemas importiert.

        Acceptable schema name patterns using SQL LIKE expressions syntax include using %, e.g. A%; %B; %C%; D
        - start with A or    
        - end with B or    
        - contain C or    
        - equal D

        Usage of NOT and special characters are not acceptable

7.  **Treiberspeicherort**: Geben Sie den Pfad zum Speicherort des JDBC-Treibers auf dem virtuellen Computer an, auf dem die selbstgehostete Integration Runtime ausgeführt wird. Dabei sollte es sich um den Pfad zu einem gültigen Speicherort eines JAR-Ordners handeln.

8.  **Maximal verfügbarer Arbeitsspeicher:** Maximaler Arbeitsspeicher (in GB), der auf dem virtuellen Computer des Kunden für Überprüfungsprozesse verfügbar ist. Abhängig von der Größe der zu überprüfenden Teradata-Quelle.

    > [!Note] 
    > Faustregel: 2 GB Arbeitsspeicher pro 1.000 Tabellen.

    :::image type="content" source="media/register-scan-teradata-source/setup-scan.png" alt-text="Einrichten der Überprüfung" border="true":::

6.  Klicken Sie auf **Continue** (Weiter).

7.  Wählen Sie den **Auslöser für die Überprüfung**. Sie können einen Zeitplan einrichten oder die Überprüfung einmalig ausführen.

8.  Sehen Sie sich Ihre Überprüfung noch einmal an, und klicken Sie dann auf **Speichern und ausführen**.

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