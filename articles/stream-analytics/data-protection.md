---
title: Schutz von Daten in Azure Stream Analytics
description: In diesem Artikel wird beschrieben, wie Sie Ihre privaten Daten verschlüsseln, die von einem Azure Stream Analytics-Auftrag genutzt werden.
author: mamccrea
ms.author: mamccrea
ms.service: stream-analytics
ms.topic: how-to
ms.date: 09/23/2020
ms.openlocfilehash: b54076413d3a6cabf2e3ef0b06e8e17875efbf97
ms.sourcegitcommit: 8c7f47cc301ca07e7901d95b5fb81f08e6577550
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/27/2020
ms.locfileid: "92746401"
---
# <a name="data-protection-in-azure-stream-analytics"></a>Schutz von Daten in Azure Stream Analytics 

Azure Stream Analytics ist eine vollständig verwaltete Platform-as-a-Service-Lösung, mit der Sie Echtzeit-Analysepipelines erstellen können. Alle aufwändigen Aufgaben, z. B. die Clusterbereitstellung, die Skalierung von Knoten zur Erfüllung Ihrer Nutzungsanforderungen und die Verwaltung interner Prüfpunkte, werden im Hintergrund verwaltet.

## <a name="private-data-assets-that-are-stored"></a>Gespeicherte private Datenressourcen

Azure Stream Analytics speichert die folgenden Metadaten und Daten zur Ausführung: 

* Abfragedefinition und deren zugehörige Konfiguration  

* Benutzerdefinierte Funktionen oder Aggregate  

* Von der Stream Analytics-Runtime benötigte Prüfpunkte

* Momentaufnahmen von Verweisdaten 

* Verbindungsdetails der Ressourcen, die von Ihrem Stream Analytics-Auftrag verwendet werden

Sie können sich weiter über die [Complianceangebote von Microsoft](https://gallery.technet.microsoft.com/Overview-of-Azure-c1be3942) informieren, um Ihre Konformitätsverpflichtungen für regulierte Branchen oder Umgebungen zu erfüllen. 

## <a name="in-region-data-residency"></a>Regionsbezogene Data Residency
Azure Stream Analytics speichert Kundendaten und andere Metadaten, die oben beschrieben sind. Kundendaten werden von Azure Stream Analytics standardmäßig in einer einzigen Region gespeichert, sodass dieser Dienst automatisch die Anforderungen hinsichtlich regionsbezogener Data Residency erfüllt, einschließlich der im [Trust Center](https://azuredatacentermap.azurewebsites.net/) angegebenen Anforderungen.
Außerdem können Sie auswählen, dass alle Datenassets (Kundendaten und andere Metadaten), die sich auf Ihren Stream Analytics-Auftrag beziehen, in einer einzigen Region gespeichert werden, indem Sie sie in einem Speicherkonto Ihrer Wahl verschlüsseln.

## <a name="encrypt-your-data"></a>Verschlüsseln Ihrer Daten

Für Stream Analytics werden für die gesamte Infrastruktur automatisch die bestmöglichen Verschlüsselungsstandards genutzt, um Ihre Daten zu verschlüsseln und zu schützen. Sie können bei Stream Analytics darauf vertrauen, dass Ihre gesamten Daten sicher gespeichert werden, sodass Sie sich nicht um die Verwaltung der Infrastruktur kümmern müssen.

Falls Sie für die Verschlüsselung Ihrer Daten vom Kunden verwaltete Schlüssel verwenden möchten, können Sie Ihr eigenes Speicherkonto (Universell V1 oder V2) zum Speichern von privaten Datenressourcen nutzen, die von der Stream Analytics-Runtime benötigt werden. Ihr Speicherkonto kann bei Bedarf verschlüsselt werden. Von der Stream Analytics-Infrastruktur werden Ihre privaten Datenressourcen nicht dauerhaft gespeichert. 

Die entsprechende Einstellung muss beim Erstellen eines Stream Analytics-Auftrags konfiguriert werden und ist während des Lebenszyklus des Auftrags nicht änderbar. Eine Änderung oder Löschung von Speicher, der von Ihrer Stream Analytics-Instanz genutzt wird, ist nicht zu empfehlen. Wenn Sie Ihr Speicherkonto löschen, werden alle privaten Datenressourcen dauerhaft gelöscht, und dies führt dazu, dass für Ihren Auftrag ein Fehler auftritt. 

Die Aktualisierung oder Rotation von Schlüsseln für Ihr Speicherkonto ist mit dem Stream Analytics-Portal nicht möglich. Sie können die Schlüssel mit den REST-APIs aktualisieren.


### <a name="configure-storage-account-for-private-data"></a>Konfigurieren des Speicherkontos für private Daten 


Verschlüsseln Sie Ihr Speicherkonto, um Ihre Daten zu schützen und den Speicherort Ihrer privaten Daten explizit auszuwählen. 

Sie können sich weiter über die [Complianceangebote von Microsoft](https://gallery.technet.microsoft.com/Overview-of-Azure-c1be3942) informieren, um Ihre Konformitätsverpflichtungen für regulierte Branchen oder Umgebungen zu erfüllen. 



Führen Sie die folgenden Schritte aus, um Ihr Speicherkonto für private Datenressourcen zu konfigurieren. Diese Konfiguration erfolgt über Ihren Stream Analytics-Auftrag und nicht über Ihr Speicherkonto.

1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com/) an.

1. Klicken Sie im Azure-Portal links oben auf **Ressource erstellen** . 

1. Wählen Sie in der Ergebnisliste **Analytics** > **Stream Analytics-Auftrag** aus. 

1. Fügen Sie auf der Seite des Stream Analytics-Auftrags die erforderlichen Details ein, z. B. Name, Region und Skalierung. 

1. Aktivieren Sie das Kontrollkästchen *Secure all private data assets needed by this job in my Storage account* (Alle privaten Datenressourcen für diesen Auftrag unter meinem Speicherkonto sichern).

1. Wählen Sie in Ihrem Abonnement ein Speicherkonto aus. Beachten Sie, dass diese Einstellung während des Lebenszyklus des Auftrags nicht geändert werden kann. 

   ![Einstellungen des Speicherkontos für private Daten](./media/data-protection/storage-account-create.png)


## <a name="next-steps"></a>Nächste Schritte

* [Erstellen Sie ein Azure Storage-Konto.](../storage/common/storage-account-create.md)
* [Grundlegendes zu Eingaben für Azure Stream Analytics](stream-analytics-add-inputs.md)
* [Begriffe zu Prüfpunkten und zur Wiedergabe bei Azure Stream Analytics-Aufträgen](stream-analytics-concepts-checkpoint-replay.md)
* [Verwenden von Referenzdaten für Suchvorgänge in Stream Analytics](stream-analytics-use-reference-data.md)
