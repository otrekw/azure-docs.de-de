---
title: Einrichten der Abhängigkeitsanalyse ohne Agent in der Azure Migrate-Serverbewertung
description: Hier erfahren Sie, wie Sie die Abhängigkeitsanalyse ohne Agent in der Azure Migrate-Serverbewertung einrichten.
ms.topic: how-to
ms.date: 6/08/2020
ms.openlocfilehash: dc2ea0656198927cc8ae58533d296a2bedc37c13
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/02/2020
ms.locfileid: "84771375"
---
# <a name="analyze-machine-dependencies-agentless"></a>Analysieren von Computerabhängigkeiten (ohne Agent)

In diesem Artikel wird das Einrichten der Abhängigkeitsanalyse ohne Agent in der Azure Migrate-Serverbewertung beschrieben. Mit der [Abhängigkeitsanalyse](concepts-dependency-visualization.md) können Sie Abhängigkeiten zwischen Computern im Hinblick auf die Bewertung und Migration zu Azure besser identifizieren und verstehen.


> [!IMPORTANT]
> Die Abhängigkeitsvisualisierung ohne Agent ist derzeit als Vorschauversion für VMware-VMs verfügbar, die mithilfe des Azure Migrate-Serverbewertungstools ermittelt wurden.
> Die Funktionen sind möglicherweise eingeschränkt oder unvollständig.
> Diese Vorschau wird durch den Kundensupport abgedeckt und kann für Produktionsworkloads verwendet werden.
> Weitere Informationen finden Sie unter [Zusätzliche Nutzungsbestimmungen für Microsoft Azure-Vorschauen](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).

## <a name="current-limitations"></a>Aktuelle Einschränkungen

- In der Abhängigkeitsanalyseansicht können Sie zurzeit keine Server zu einer Gruppe hinzufügen oder daraus entfernen.
- Eine Abhängigkeitszuordnung für eine Gruppe von Servern ist derzeit nicht verfügbar.
- Die Abhängigkeitsdaten können nicht im Tabellenformat heruntergeladen werden.

## <a name="before-you-start"></a>Vorbereitung

- [Lesen](migrate-support-matrix-vmware.md#dependency-analysis-requirements-agentless) Sie die Informationen zu unterstützten Betriebssystemen und erforderlichen Berechtigungen.
- Stellen Sie Folgendes sicher:
    - Sie verfügen über ein Azure Migrate-Projekt. Wenn dies nicht der Fall ist, [erstellen](how-to-add-tool-first-time.md) Sie jetzt ein entsprechendes Projekt.
    - Vergewissern Sie sich, dass Sie das Azure Migrate-Serverbewertungstool zum Projekt [hinzugefügt](how-to-assess.md) haben.
    - Richten Sie eine [Azure Migrate-Appliance](migrate-appliance.md) ein, um lokale Computer zu ermitteln. [Richten Sie eine Appliance für VMware-VMs ein](how-to-set-up-appliance-vmware.md). Die Appliance ermittelt lokale Computer und sendet Metadaten und Leistungsdaten an die Azure Migrate-Serverbewertung.
- Überprüfen Sie, ob auf jeder VM, die Sie analysieren möchten, VMware Tools (höhere Version als 10.2) installiert ist.


## <a name="create-a-user-account-for-discovery"></a>Erstellen eines Benutzerkontos für die Ermittlung

Richten Sie ein Benutzerkonto ein, damit die Serverbewertung zur Ermittlung von Abhängigkeiten auf den virtuellen Computer zugreifen kann. [Informieren](migrate-support-matrix-vmware.md#dependency-analysis-requirements-agentless) Sie sich über die Kontoanforderungen für Windows- und Linux-VMs.


## <a name="add-the-user-account-to-the-appliance"></a>Hinzufügen des Benutzerkontos zur Appliance

Fügen Sie das Benutzerkonto zur Appliance hinzu.

1. Öffnen Sie die Verwaltungs-App für die Appliance. 
2. Navigieren Sie zum Bereich **vCenter-Details angeben**.
3. Klicken Sie unter **Anwendungen und Abhängigkeiten auf VMs ermitteln** auf **Anmeldeinformationen hinzufügen**.
3. Wählen Sie das **Betriebssystem** aus, geben Sie einen benutzerfreundlichen Namen für das Konto an, und geben Sie dann **Benutzername**/**Kennwort** an.
6. Klicken Sie auf **Speichern**.
7. Klicken Sie auf **Speichern und Ermittlung starten**.

    ![VM-Benutzerkonto hinzufügen](./media/how-to-create-group-machine-dependencies-agentless/add-vm-credential.png)

## <a name="start-dependency-discovery"></a>Starten der Abhängigkeitsermittlung

Wählen Sie die Computer aus, auf denen Sie die Abhängigkeitsermittlung aktivieren möchten.

1. Klicken Sie in **Azure Migrate: Serverbewertung** auf **Ermittelte Server**.
2. Klicken Sie auf das Symbol **Abhängigkeitsanalyse**.
3. Klicken Sie auf **Server hinzufügen**.
4. Wählen Sie auf der Seite **Server hinzufügen** die Appliance aus, die die relevanten Computer ermittelt.
5. Wählen Sie in der Liste der Computer die Computer aus.
6. Klicken Sie auf **Server hinzufügen**.

    ![Starten der Abhängigkeitsermittlung](./media/how-to-create-group-machine-dependencies-agentless/start-dependency-discovery.png)

Ungefähr sechs Stunden nach dem Start der Abhängigkeitsermittlung können Sie die Abhängigkeiten visualisieren.

## <a name="visualize-dependencies"></a>Visualisieren von Abhängigkeiten

1. Klicken Sie in **Azure Migrate: Serverbewertung** auf **Ermittelte Server**.
2. Suchen Sie nach dem Computer, den Sie anzeigen möchten.
3. Klicken Sie in der Spalte **Abhängigkeiten** auf **Abhängigkeiten anzeigen**.
4. Ändern Sie den Zeitraum, für den Sie die Zuordnung anzeigen möchten, über die Dropdown-Liste **Zeitdauer**.
5. Erweitern Sie die Gruppe **Client**, um die Computer aufzulisten, die eine Abhängigkeit zum ausgewählten Computer aufweisen.
6. Erweitern Sie die Gruppe **Port**, um die Computer aufzulisten, die eine Abhängigkeit vom ausgewählten Computer aufweisen.
7. Wenn Sie zur Zuordnungsansicht eines abhängigen Computers navigieren möchten, klicken Sie auf den Computernamen und dann auf **Serverzuordnung laden**.

    ![Erweitern der Portgruppe des Servers und Laden der Serverzuordnung](./media/how-to-create-group-machine-dependencies-agentless/load-server-map.png)

    ![Erweitern der Clientgruppe ](./media/how-to-create-group-machine-dependencies-agentless/expand-client-group.png)

8. Erweitern Sie den ausgewählten Computer, um Details auf Prozessebene für die einzelnen Abhängigkeiten anzuzeigen.

    ![Erweitern des Servers zum Anzeigen von Prozessen](./media/how-to-create-group-machine-dependencies-agentless/expand-server-processes.png)

> [!NOTE]
> Prozessinformationen für eine Abhängigkeit sind nicht immer verfügbar. Wenn keine Informationen verfügbar sind, wird die Abhängigkeit mit „Unbekannter Prozess“ als Prozess dargestellt.

## <a name="export-dependency-data"></a>Exportieren der Abhängigkeitsdaten

1. Klicken Sie in **Azure Migrate: Serverbewertung** auf **Ermittelte Server**.
2. Klicken Sie auf das Symbol **Abhängigkeitsanalyse**.
3. Klicken Sie auf **Anwendungsabhängigkeiten exportieren**.
4. Wählen Sie auf der Seite **Anwendungsabhängigkeiten exportieren** die Appliance aus, die die relevanten Computer ermittelt.
5. Wählen Sie die Start- und Endzeit aus. Beachten Sie, dass Sie nur Daten für die letzten 30 Tage herunterladen können.
6. Klicken Sie auf **Abhängigkeit exportieren**.

Die Abhängigkeitsdaten werden im CSV-Format exportiert und heruntergeladen. Die heruntergeladene Datei enthält die Abhängigkeitsdaten für alle Computer, auf denen die Abhängigkeitsanalyse aktiviert war. 

![Exportieren von Abhängigkeiten](./media/how-to-create-group-machine-dependencies-agentless/export.png)

### <a name="dependency-information"></a>Abhängigkeitsinformationen

Jede Zeile in der exportierten CSV-Datei entspricht einer Abhängigkeit, die im angegebenen Zeitfenster beobachtet wurde. 

In der folgenden Tabelle sind die Felder der exportierten CSV-Datei zusammengefasst. Beachten Sie, dass die Felder für den Servernamen, die Anwendung und den Prozess nur für Server aufgefüllt werden, auf denen die Abhängigkeitsanalyse ohne Agent aktiviert ist.

**Feldname** | **Details**
--- | --- 
Zeitfenster | Das Zeitfenster, in dem die Abhängigkeit beobachtet wurde. <br/> Abhängigkeitsdaten werden derzeit in Zeitfenstern von 6 Stunden erfasst.
Name des Quellservers | Der Name des Quellcomputers. 
Quellanwendung | Der Name der Anwendung auf dem Quellcomputer. 
Quellprozess | Der Name des Prozesses auf dem Quellcomputer. 
Name des Zielservers | Der Name des Zielcomputers.
Ziel-IP | Die IP-Adresse des Zielcomputers.
Zielanwendung | Der Name der Anwendung auf dem Zielcomputer.
Zielprozess | Der Name des Prozesses auf dem Zielcomputer. 
Zielport | Die Portnummer auf dem Zielcomputer.


## <a name="stop-dependency-discovery"></a>Beenden der Abhängigkeitsermittlung

Wählen Sie die Computer aus, auf denen Sie die Abhängigkeitsermittlung beenden möchten.

1. Klicken Sie in **Azure Migrate: Serverbewertung** auf **Ermittelte Server**.
2. Klicken Sie auf das Symbol **Abhängigkeitsanalyse**.
3. Klicken Sie auf **Server entfernen**.
3. Wählen Sie auf der Seite **Server entfernen** die **Appliance** aus, die die virtuellen Computer ermittelt, auf denen die Abhängigkeitsermittlung beendet werden soll.
4. Wählen Sie in der Liste der Computer die Computer aus.
5. Klicken Sie auf **Server entfernen**.


## <a name="next-steps"></a>Nächste Schritte

[Gruppieren von Computern](how-to-create-a-group.md) für die Bewertung
