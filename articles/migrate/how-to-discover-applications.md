---
title: Ermitteln von Apps, Rollen und Funktionen auf lokalen Servern mit Azure Migrate
description: Erfahren Sie, wie Sie Apps, Rollen und Funktionen auf lokalen Servern mithilfe der Azure Migrate-Serverbewertung ermitteln.
ms.topic: article
ms.date: 11/20/2019
ms.openlocfilehash: adc22925d1152639babe2377a1eae440e0ce418e
ms.sourcegitcommit: dbcc4569fde1bebb9df0a3ab6d4d3ff7f806d486
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/15/2020
ms.locfileid: "76029073"
---
# <a name="discover-machine-apps-roles-and-features"></a>Ermitteln von Apps, Rollen und Features eines Computers

In diesem Abschnitt wird beschrieben, wie Anwendungen, Rollen und Features, die auf lokalen Servern installiert sind, mithilfe der Azure Migrate: Server Assessment“ (Azure Migrate-Serverbewertung) erstellen.

Durch das Ermitteln des Bestands von Apps und Rollen/Features, die auf lokalen Computern ausgeführt werden, können Sie einen angepassten Migrationspfad zu Azure für Ihre Workloads identifizieren und planen.

> [!NOTE]
> Die App-Ermittlung wird derzeit nur für VMware-VMs unterstützt und ist ausschließlich auf die Ermittlung beschränkt. Eine app-basierte Bewertung ist noch nicht verfügbar.  Die derzeitige Serverbewertung bewertet zurzeit lokale VMware-VMs, Hyper-V-VMs und physische Server auf Computerebene für Lift & Shift-Migrationen.

App-Ermittlung mithilfe von Azure Migrate: Die Serverbewertung erfolgt ohne Agenten. Auf den Computern und VMs muss nichts installiert werden. Die Serverbewertung verwendet die Azure Migrate-Appliance zusammen mit den Gastanmeldeinformationen des Computers, um die Ermittlung durchzuführen. Die Anwendung greift remote per VMware-APIs auf die VMware-Computer zu.


## <a name="before-you-start"></a>Vorbereitung

1. Informieren Sie sich über die [eingeschränkte Unterstützung](migrate-support-matrix-vmware.md#application-discovery) für die Ermittlung auf App-Ebene.
2. Stellen Sie sicher, dass Sie ein Azure Migrate-Projekt [erstellt](how-to-add-tool-first-time.md) haben.
3. Wenn Sie bereits ein Projekt erstellt haben, vergewissern Sie sich, dass Sie das Tool Azure Migrate-Serverbewertung[hinzugefügt](how-to-assess.md): Migrate-Serverbewertung bewerten.
4. Überprüfen Sie die [VMware-Anforderungen](migrate-support-matrix-vmware.md#vmware-requirements) zum Ermitteln und Bewerten von VMware-VMs mit der Azure Migrate-Appliance.
4. Überprüfen Sie die [Anforderungen](migrate-appliance.md) für die Bereitstellung der Azure Migrate-Appliance.

## <a name="prepare-for-app-discovery"></a>Vorbereiten der App-Ermittlung

1. [Bereiten Sie die Bereitstellung der Appliance vor](tutorial-prepare-vmware.md). Die Vorbereitung umfasst die Überprüfung von Applianceeinstellungen und die Einrichtung eines Kontos, das die Appliance für den Zugriff auf den vCenter-Server verwendet.
2. Sie müssen über ein Benutzerkonto (jeweils eines für Windows- und Linux-Server) mit Administratorberechtigungen für die Computer verfügen, auf denen Sie Apps, Rollen und Features ermitteln möchten.
3. [Stellen Sie die Azure Migrate-Appliance bereit](how-to-set-up-appliance-vmware.md), um die Ermittlung zu starten. Zum Bereitstellen der Appliance laden Sie eine OVA-Vorlage herunter und importieren Sie diese in VMware, um die Appliance als VMware-VM zu erstellen. Sie konfigurieren die Appliance und registrieren sie dann bei Azure Migrate.
2. Um eine kontinuierliche Ermittlung zu starten, geben Sie bei der Bereitstellung der Appliance folgende Informationen an:
    - Name des vCenter-Servers, zu dem eine Verbindung hergestellt werden soll.
    - Anmeldeinformationen, die Sie für die Appliance erstellt haben, um eine Verbindung zum vCenter-Server herzustellen.
    - Die Kontoanmeldeinformationen, die Sie für die Appliance für die Verbindung zu Windows-/Linux-VMs erstellt haben.

Nachdem die Appliance bereitgestellt wurde und Sie die Anmeldeinformationen angegeben haben, startet das Gerät die kontinuierliche Ermittlung von VM-Metadaten und Leistungsdaten sowie die Ermittlung von Apps, Features und Rollen.  Die Dauer der App-Ermittlung hängt von der Anzahl der vorhandenen VMs ab. Die App-Ermittlung für 500 VMs dauert ca. eine Stunde.

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
