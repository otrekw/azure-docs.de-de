---
title: Ermitteln von Apps, Rollen und Funktionen auf lokalen Servern mit Azure Migrate
description: Erfahren Sie, wie Sie Apps, Rollen und Funktionen auf lokalen Servern mithilfe der Azure Migrate-Serverbewertung ermitteln.
ms.topic: article
ms.date: 03/12/2020
ms.openlocfilehash: ff9f5489b513cd1405e6b093d7537e4cbcead041
ms.sourcegitcommit: 3beb067d5dc3d8895971b1bc18304e004b8a19b3
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/04/2020
ms.locfileid: "82744615"
---
# <a name="discover-machine-apps-roles-and-features"></a>Ermitteln von Apps, Rollen und Features eines Computers

In diesem Abschnitt wird beschrieben, wie Anwendungen, Rollen und Features, die auf lokalen Servern installiert sind, mithilfe der Azure Migrate: Server Assessment“ (Azure Migrate-Serverbewertung) erstellen.

Durch das Ermitteln des Bestands von Apps und Rollen/Features, die auf lokalen Computern ausgeführt werden, können Sie einen angepassten Migrationspfad zu Azure für Ihre Workloads identifizieren und planen.

> [!NOTE]
> Die App-Ermittlung befindet sich derzeit nur für VMware-VMs in der Vorschau und ist ausschließlich auf die Ermittlung beschränkt. Eine app-basierte Bewertung ist noch nicht verfügbar. Computerbasierte Bewertungen für lokale VMware-VMs, Hyper-V-VMs und physische Server.

App-Ermittlung mithilfe von Azure Migrate: Die Serverbewertung erfolgt ohne Agenten. Auf den Computern und VMs wird nichts installiert. Die Serverbewertung verwendet die Azure Migrate-Appliance zusammen mit den Gastanmeldeinformationen des Computers, um die Ermittlung durchzuführen. Die Anwendung greift remote per VMware-APIs auf die VMware-Computer zu.


## <a name="before-you-start"></a>Vorbereitung

1. Stellen Sie sicher, dass Sie ein Azure Migrate-Projekt [erstellt](how-to-add-tool-first-time.md) haben.
2. Stellen Sie sicher, dass Sie das Azure Migrate-Tool für die Serverbewertung einem Projekt [hinzugefügt](how-to-assess.md) haben.
4. Überprüfen Sie die [VMware-Anforderungen](migrate-support-matrix-vmware.md#vmware-requirements) zum Ermitteln und Bewerten von VMware-VMs mit der Azure Migrate-Appliance.
5. Überprüfen Sie die [Anforderungen](migrate-appliance.md) für die Bereitstellung der Azure Migrate-Appliance.
6. [Überprüfen Sie die Unterstützung und Anforderungen](migrate-support-matrix-vmware.md#application-discovery) für die Anwendungsermittlung.



## <a name="deploy-the-azure-migrate-appliance"></a>Bereitstellen der Azure Migrate-Appliance

1. [Überprüfen Sie die Anforderungen](migrate-appliance.md#appliance---vmware) für die Bereitstellung der Azure Migrate-Appliance.
2. Überprüfen Sie die Azure-URLs, die die Appliance für den Zugriff in [öffentlichen Clouds](migrate-appliance.md#public-cloud-urls) und [Government-Clouds](migrate-appliance.md#government-cloud-urls) benötigt.
3. [Überprüfen Sie die Daten](migrate-appliance.md#collected-data---vmware), die die Appliance während der Ermittlung und Bewertung sammelt.
4. [Beachten](migrate-support-matrix-vmware.md#port-access) Sie die Portzugriffsanforderungen für die Appliance.
5. [Stellen Sie die Azure Migrate-Appliance bereit](how-to-set-up-appliance-vmware.md), um die Ermittlung zu starten. Zum Bereitstellen der Appliance laden Sie eine OVA-Vorlage herunter und importieren Sie diese in VMware, um die Appliance als VMware-VM zu erstellen. Sie konfigurieren die Appliance und registrieren sie dann bei Azure Migrate.
6. Um eine kontinuierliche Ermittlung zu starten, geben Sie bei der Bereitstellung der Appliance folgende Informationen an:
    - Name des vCenter-Servers, zu dem eine Verbindung hergestellt werden soll.
    - Anmeldeinformationen, die Sie für die Appliance erstellt haben, um eine Verbindung zum vCenter-Server herzustellen.
    - Die Kontoanmeldeinformationen, die Sie für die Appliance für die Verbindung zu Windows-/Linux-VMs erstellt haben.

Nachdem die Appliance bereitgestellt wurde und Sie die Anmeldeinformationen angegeben haben, startet das Gerät die kontinuierliche Ermittlung von VM-Metadaten und Leistungsdaten sowie die Ermittlung von Apps, Features und Rollen.  Die Dauer der App-Ermittlung hängt von der Anzahl der vorhandenen VMs ab. Die App-Ermittlung für 500 VMs dauert ca. eine Stunde.

## <a name="prepare-a-user-account"></a>Vorbereiten eines Benutzerkontos

Erstellen Sie ein Konto, das für die Ermittlung verwendet werden soll, und fügen Sie es der Appliance hinzu.

### <a name="create-a-user-account-for-discovery"></a>Erstellen eines Benutzerkontos für die Ermittlung

Richten Sie ein Benutzerkonto ein, damit die Serverbewertung zur Ermittlung auf den virtuellen Computer zugreifen kann. [Informieren Sie sich](migrate-support-matrix-vmware.md#application-discovery) über die Kontoanforderungen.


### <a name="add-the-user-account-to-the-appliance"></a>Hinzufügen des Benutzerkontos zur Appliance

Fügen Sie das Benutzerkonto zur Appliance hinzu.

1. Öffnen Sie die Verwaltungs-App für die Appliance. 
2. Navigieren Sie zum Bereich **vCenter-Details angeben**.
3. Klicken Sie unter **Anwendungen und Abhängigkeiten auf VMs ermitteln** auf **Anmeldeinformationen hinzufügen**.
3. Wählen Sie das **Betriebssystem** aus, geben Sie einen benutzerfreundlichen Namen für das Konto an, und geben Sie dann **Benutzername**/**Kennwort** an.
6. Klicken Sie auf **Speichern**.
7. Klicken Sie auf **Speichern und Ermittlung starten**.

    ![VM-Benutzerkonto hinzufügen](./media/how-to-create-group-machine-dependencies-agentless/add-vm-credential.png)





## <a name="review-and-export-the-inventory"></a>Überprüfen und Exportieren des Bestands

Wenn Sie Anmeldeinformationen für die App-Ermittlung eingegeben haben, können Sie den App-Bestand nach Abschluss der Ermittlung im Azure-Portal überprüfen und exportieren.

1. Klicken Sie unter **Azure Migrate – Server** > **Azure Migrate: Serverbewertung** auf die angezeigte Anzahl, um die Seite **Ermittelte Server** zu öffnen.

    > [!NOTE]
    > In dieser Phase können Sie optional auch die Abhängigkeitszuordnung für ermittelte Computer einrichten, sodass Sie Abhängigkeiten zwischen den Computern visualisieren können, die Sie bewerten möchten. [Weitere Informationen](how-to-create-group-machine-dependencies.md)

2. Klicken Sie in **Ermittelte Anwendungen** auf die angezeigte Anzahl.
3. In **Anwendungsbestand** können Sie die ermittelten Apps, Rollen und Features überprüfen.
4. Klicken Sie zum Exportieren des Bestands in **Ermittelte Server** auf **App-Bestand exportieren**.

Der App-Bestand wird im Excel-Format exportiert und heruntergeladen. Im Arbeitsblatt für den **Anwendungsbestand** werden alle Apps angezeigt, die auf allen Computern ermittelt wurden.

## <a name="next-steps"></a>Nächste Schritte

- [Erstellen Sie eine Bewertung](how-to-create-assessment.md) für die Lift & Shift-Migration der ermittelten Server.
- Bewerten Sie SQL Server-Datenbanken mithilfe der [Azure Migrate: Datenbankbewertung](https://docs.microsoft.com/sql/dma/dma-assess-sql-data-estate-to-sqldb?view=sql-server-2017).
