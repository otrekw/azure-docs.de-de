---
title: Einrichten der Abhängigkeitsvisualisierung ohne Agent in Azure Migrate
description: Einrichten von Gruppen mit der Abhängigkeitsvisualisierung ohne Agent in der Azure Migrate-Serverbewertung.
ms.topic: article
ms.date: 2/24/2020
ms.openlocfilehash: c9425ad1fa78f14a194d3fe13c259dadf4eb5eb6
ms.sourcegitcommit: 7f929a025ba0b26bf64a367eb6b1ada4042e72ed
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/25/2020
ms.locfileid: "77589129"
---
# <a name="set-up-agentless-dependency-visualization"></a>Einrichten der Abhängigkeitsvisualisierung ohne Agent 

In diesem Artikel wird das Einrichten der Abhängigkeitsvisualisierung in der Azure Migrate-Serverbewertung beschrieben. Mit der [Abhängigkeitsvisualisierung](concepts-dependency-visualization.md#what-is-dependency-visualization) können Sie Abhängigkeiten zwischen Computern, die Sie bewerten und zu Azure migrieren möchten, besser identifizieren und verstehen.

Mit der Abhängigkeitsvisualisierung ohne Agent können Sie Abhängigkeiten von Computern identifizieren, ohne einen Agent auf einem Computer installieren zu müssen. Sie erfasst TCP-Verbindungsdaten von den Computern, für die sie aktiviert wurde.

> [!IMPORTANT]
> Die Abhängigkeitsvisualisierung ohne Agent ist derzeit als Vorschauversion nur für virtuelle Azure VMware-Computer verfügbar, die mithilfe des Azure Migrate-Serverbewertungstools ermittelt wurden.
> Die Funktionen sind möglicherweise eingeschränkt oder unvollständig.
> Diese Vorschau wird durch den Kundensupport abgedeckt und kann für Produktionsworkloads verwendet werden.
> Weitere Informationen finden Sie unter [Zusätzliche Nutzungsbestimmungen für Microsoft Azure-Vorschauen](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).

## <a name="current-limitations"></a>Aktuelle Einschränkungen

- In der Abhängigkeitsanalyseansicht können Sie zurzeit keine Server hinzufügen oder aus einer Gruppe entfernen.
- Eine Abhängigkeitszuordnung für eine Gruppe von Servern ist derzeit nicht verfügbar.
- Die Abhängigkeitsdaten können derzeit nicht im Tabellenformat heruntergeladen werden.

## <a name="before-you-start"></a>Vorbereitung

- [Überprüfen](concepts-dependency-visualization.md#agentless-visualization) Sie die Anforderungen und die Kosten, die mit der Abhängigkeitsvisualisierung ohne Agent verbunden sind.
- Überprüfen Sie die [Supportanforderungen](migrate-support-matrix-vmware.md#agentless-dependency-visualization) für das Einrichten einer Abhängigkeitsvisualisierung ohne Agent.
- Stellen Sie sicher, dass Sie ein Azure Migrate-Projekt [erstellt](how-to-add-tool-first-time.md) haben.
- Wenn Sie bereits ein Projekt erstellt haben, vergewissern Sie sich, dass Sie das Serverbewertungstool von Azure Migrate [hinzugefügt](how-to-assess.md) haben.
- Stellen Sie sicher, dass Sie eine [Azure Migrate-Appliance](migrate-appliance.md) eingerichtet haben, mit der Ihre lokalen Computer ermittelt werden können. Erfahren Sie, wie Sie eine Appliance für virtuelle [VMware](how-to-set-up-appliance-vmware.md)-Computer einrichten. Die Appliance ermittelt lokale Computer und sendet Metadaten und Leistungsdaten an die Azure Migrate-Serverbewertung.


## <a name="create-a-user-account-for-discovery"></a>Erstellen eines Benutzerkontos für die Ermittlung

Richten Sie ein Benutzerkonto ein, damit die Serverbewertung zur Ermittlung auf den virtuellen Computer zugreifen kann. Sie können ein Benutzerkonto angeben.

- **Virtuelle Windows-Computer:** Das Benutzerkonto muss ein lokales Konto oder ein Domänenadministratorkonto sein.
- **Virtuelle Linux-Computer:** Für das Konto ist root-Berechtigung erforderlich. Alternativ benötigt das Benutzerkonto diese beiden Funktionen für /bin/netstat- und /bin/ls-Dateien: CAP_DAC_READ_SEARCH und CAP_SYS_PTRACE.

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
3. Wählen Sie auf der Seite **Server hinzufügen** die Appliance aus, die die relevanten Computer ermittelt.
4. Wählen Sie in der Liste der Computer die Computer aus.
5. Klicken Sie auf **Server hinzufügen**.

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
