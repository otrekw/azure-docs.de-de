---
title: Sichern einer SharePoint-Farm in Azure mit DPM
description: Dieser Artikel enthält eine Übersicht über den DPM-/Azure Backup Server-Schutz einer SharePoint-Farm in Azure.
ms.topic: conceptual
ms.date: 03/09/2020
ms.openlocfilehash: 7661d64e487c8b8badca240852d17bcf736ba8cf
ms.sourcegitcommit: 772eb9c6684dd4864e0ba507945a83e48b8c16f0
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/19/2021
ms.locfileid: "91254430"
---
# <a name="back-up-a-sharepoint-farm-to-azure-with-dpm"></a>Sichern einer SharePoint-Farm in Azure mit DPM

Die Vorgehensweise zum Sichern einer SharePoint-Farm mithilfe von System Center Data Protection Manager (DPM) in Microsoft Azure ähnelt der Vorgehensweise zum Sichern anderer Datenquellen. Azure Backup ermöglicht die Verwendung eines flexiblen Sicherungszeitplans, mit dem Sie tägliche, wöchentliche, monatliche oder jährliche Sicherungspunkte erstellen und Aufbewahrungsrichtlinienoptionen für unterschiedliche Sicherungspunkte konfigurieren können. Mit DPM können Sie lokale Festplattenkopien speichern, um die Wiederherstellung zu beschleunigen. Außerdem können Sie Kopien zur kostengünstigen, langfristigen Aufbewahrung in Azure speichern.

Das Sichern von SharePoint auf Azure mit DPM ist ein Vorgang, der dem lokalen Sichern von SharePoint auf DPM sehr ähnlich ist. Besondere Überlegungen zu Azure finden Sie in diesem Artikel.

## <a name="sharepoint-supported-versions-and-related-protection-scenarios"></a>Von SharePoint unterstützte Versionen und entsprechende Sicherungsszenarien

Eine Liste der unterstützten SharePoint-Versionen und der DPM-Versionen, die zu deren Sicherung erforderlich sind, finden Sie unter [Was kann mit DPM gesichert werden?](/system-center/dpm/dpm-protection-matrix#applications-backup)

## <a name="before-you-start"></a>Vorbereitung

Vor dem Sichern einer SharePoint-Farm in Azure müssen Sie ein paar Punkte bestätigen.

### <a name="prerequisites"></a>Voraussetzungen

Vergewissern Sie sich zunächst, dass alle [Voraussetzungen für die Verwendung von Microsoft Azure Backup](backup-azure-dpm-introduction.md#prerequisites-and-limitations) für den Schutz von Workloads erfüllt sind. Die Voraussetzungen umfassen folgende Aufgaben: Erstellen eines Sicherungstresors, Herunterladen der Anmeldeinformationen, Installieren des Azure Backup-Agents und Registrieren des DPM-/Azure Backup Servers beim Tresor.

Weitere Voraussetzungen und Einschränkungen finden Sie im Artikel [Sichern von SharePoint mit DPM](/system-center/dpm/back-up-sharepoint#prerequisites-and-limitations).

## <a name="configure-backup"></a>Konfigurieren der Sicherung

Um die SharePoint-Farm zu sichern, konfigurieren Sie den Schutz für SharePoint mithilfe von „ConfigureSharePoint.exe“ und erstellen dann eine Schutzgruppe in DPM. Anweisungen finden Sie unter [Konfigurieren der Sicherung](/system-center/dpm/back-up-sharepoint#configure-backup) in der DPM-Dokumentation.

## <a name="monitoring"></a>Überwachung

Zum Überwachen des Sicherungsauftrags befolgen Sie die Anweisungen unter [Überwachung der DPM-Sicherung](/system-center/dpm/back-up-sharepoint#monitoring).

## <a name="restore-sharepoint-data"></a>Wiederherstellen der SharePoint-Daten

Informationen zum Wiederherstellen eines SharePoint-Elements von einem Datenträger mit DPM finden Sie unter [Wiederherstellen von SharePoint-Daten](/system-center/dpm/back-up-sharepoint#restore-sharepoint-data).

## <a name="restore-a-sharepoint-database-from-azure-by-using-dpm"></a>Azure-basiertes Wiederherstellen einer SharePoint-Datenbank mit DPM

1. Durchsuchen Sie zum Wiederherstellen einer SharePoint-Inhaltsdatenbank verschiedene Wiederherstellungspunkte (siehe oben), und wählen Sie den wiederherzustellenden Wiederherstellungspunkt aus.

    ![DPM SharePoint Protection8](./media/backup-azure-backup-sharepoint/dpm-sharepoint-protection9.png)
2. Doppelklicken Sie auf den SharePoint-Wiederherstellungspunkt, um die verfügbaren SharePoint-Kataloginformationen anzuzeigen.

   > [!NOTE]
   > Da die SharePoint-Farm mit langfristiger Aufbewahrung in Azure geschützt ist, sind auf dem DPM-Server keine Kataloginformationen (Metadaten) verfügbar. Wenn also eine Zeitpunktwiederherstellung einer SharePoint-Inhaltsdatenbank erforderlich ist, muss die SharePoint-Farm neu katalogisiert werden.
   >
   >
3. Wählen Sie **Neu katalogisieren** aus.

    ![DPM SharePoint Protection10](./media/backup-azure-backup-sharepoint/dpm-sharepoint-protection12.png)

    Das Statusfenster für die **Neukatalogisierung der Cloud** wird geöffnet.

    ![DPM SharePoint Protection11](./media/backup-azure-backup-sharepoint/dpm-sharepoint-protection13.png)

    Nach Abschluss der Katalogisierung ändert sich der Status in *Erfolgreich*. Klicken Sie auf **Schließen**.

    ![DPM SharePoint Protection12](./media/backup-azure-backup-sharepoint/dpm-sharepoint-protection14.png)
4. Wählen Sie das SharePoint-Objekt aus, das auf der DPM-Registerkarte **Wiederherstellung** angezeigt wird, um die Struktur der Inhaltsdatenbank abzurufen. Klicken Sie mit der rechten Maustaste auf das Element, und wählen Sie **Wiederherstellen** aus.

    ![DPM SharePoint Protection13](./media/backup-azure-backup-sharepoint/dpm-sharepoint-protection15.png)
5. Führen Sie nun die weiter oben in diesem Artikel beschriebenen Wiederherstellungsschritte für die Wiederherstellung einer SharePoint-Inhaltsdatenbank vom Datenträger aus.

## <a name="switching-the-front-end-web-server"></a>Wechseln des Front-End-Webservers

Wenn Sie über mehrere Front-End-Webserver verfügen und den Server, den DPM zum Schützen der Farm verwendet, wechseln möchten, befolgen Sie die Anweisungen unter [Wechseln des Front-End-Webservers](/system-center/dpm/back-up-sharepoint#switching-the-front-end-web-server).

## <a name="next-steps"></a>Nächste Schritte

* [Azure Backup Server und DPM – häufig gestellte Fragen](backup-azure-dpm-azure-server-faq.md)
* [Behandeln von Problemen in System Center Data Protection Manager](backup-azure-scdpm-troubleshooting.md)
