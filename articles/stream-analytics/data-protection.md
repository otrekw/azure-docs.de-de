---
title: Schutz von Daten in Azure Stream Analytics
description: In diesem Artikel wird beschrieben, wie Sie Ihre privaten Daten verschlüsseln, die von einem Azure Stream Analytics-Auftrag genutzt werden.
author: mamccrea
ms.author: mamccrea
ms.service: stream-analytics
ms.topic: how-to
ms.date: 03/05/2020
ms.openlocfilehash: 637ac97d1e054599ec297344ff0c5fff600c8487
ms.sourcegitcommit: e132633b9c3a53b3ead101ea2711570e60d67b83
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/07/2020
ms.locfileid: "86045347"
---
# <a name="data-protection-in-azure-stream-analytics"></a>Schutz von Daten in Azure Stream Analytics 

Azure Stream Analytics ist eine vollständig verwaltete Platform-as-a-Service-Lösung, mit der Sie Echtzeit-Analysepipelines erstellen können. Alle aufwändigen Aufgaben, z. B. die Clusterbereitstellung, die Skalierung von Knoten zur Erfüllung Ihrer Nutzungsanforderungen und die Verwaltung interner Prüfpunkte, werden im Hintergrund verwaltet.

## <a name="encrypt-your-data"></a>Verschlüsseln Ihrer Daten

Für Stream Analytics werden für die gesamte Infrastruktur automatisch die bestmöglichen Verschlüsselungsstandards genutzt, um Ihre Daten zu verschlüsseln und zu schützen. Sie können bei Stream Analytics darauf vertrauen, dass Ihre gesamten Daten sicher gespeichert werden, sodass Sie sich nicht um die Verwaltung der Infrastruktur kümmern müssen.

Falls Sie für die Verschlüsselung Ihrer Daten vom Kunden verwaltete Schlüssel verwenden möchten, können Sie Ihr eigenes Speicherkonto (Universell V1 oder V2) zum Speichern von privaten Datenressourcen nutzen, die von der Stream Analytics-Runtime benötigt werden. Ihr Speicherkonto kann bei Bedarf verschlüsselt werden. Von der Stream Analytics-Infrastruktur werden Ihre privaten Datenressourcen nicht dauerhaft gespeichert. 

Die entsprechende Einstellung muss beim Erstellen eines Stream Analytics-Auftrags konfiguriert werden und ist während des Lebenszyklus des Auftrags nicht änderbar. Eine Änderung oder Löschung von Speicher, der von Ihrer Stream Analytics-Instanz genutzt wird, ist nicht zu empfehlen. Wenn Sie Ihr Speicherkonto löschen, werden alle privaten Datenressourcen dauerhaft gelöscht, und dies führt dazu, dass für Ihren Auftrag ein Fehler auftritt. 

Die Aktualisierung oder Rotation von Schlüsseln für Ihr Speicherkonto ist mit dem Stream Analytics-Portal nicht möglich. Sie können die Schlüssel mit den REST-APIs aktualisieren.


## <a name="configure-storage-account-for-private-data"></a>Konfigurieren des Speicherkontos für private Daten 

Führen Sie die folgenden Schritte aus, um Ihr Speicherkonto für private Datenressourcen zu konfigurieren. Diese Konfiguration erfolgt über Ihren Stream Analytics-Auftrag und nicht über Ihr Speicherkonto.

1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com/) an.

1. Klicken Sie im Azure-Portal links oben auf **Ressource erstellen**. 

1. Wählen Sie in der Ergebnisliste **Analytics** > **Stream Analytics-Auftrag** aus. 

1. Fügen Sie auf der Seite des Stream Analytics-Auftrags die erforderlichen Details ein, z. B. Name, Region und Skalierung. 

1. Aktivieren Sie das Kontrollkästchen *Secure all private data assets needed by this job in my Storage account* (Alle privaten Datenressourcen für diesen Auftrag unter meinem Speicherkonto sichern).

1. Wählen Sie in Ihrem Abonnement ein Speicherkonto aus. Beachten Sie, dass diese Einstellung während des Lebenszyklus des Auftrags nicht geändert werden kann. 

   ![Einstellungen des Speicherkontos für private Daten](./media/data-protection/storage-account-create.png)

## <a name="private-data-assets-that-are-stored"></a>Gespeicherte private Datenressourcen

Alle privaten Daten, die von Stream Analytics dauerhaft aufbewahrt werden müssen, werden unter Ihrem Speicherkonto gespeichert. Beispiele für private Datenressourcen: 

* Von Ihnen erstellte Abfragen und zugehörige Konfigurationen  

* Benutzerdefinierte Funktionen 

* Von der Stream Analytics-Runtime benötigte Prüfpunkte

* Momentaufnahmen von Verweisdaten 

Verbindungsdetails Ihrer Ressourcen, die von Ihrem Stream Analytics-Auftrag genutzt werden, werden ebenfalls gespeichert. Verschlüsseln Sie Ihr Speicherkonto, um Ihre gesamten Daten zu schützen. 

Sie können sich weiter über die [Complianceangebote von Microsoft](https://gallery.technet.microsoft.com/Overview-of-Azure-c1be3942) informieren, um Ihre Konformitätsverpflichtungen für regulierte Branchen oder Umgebungen zu erfüllen. 

## <a name="known-issues"></a>Bekannte Probleme
Es gibt ein bekanntes Problem, bei dem ein Auftrag, bei dem ein vom Kunden verwalteter Schlüssel verwendet wird, zu Fehlern führt, wenn die verwaltete Identität zur Authentifizierung bei Ein- oder Ausgaben verwendet wird. Derzeit wird an der Lösung für dieses Problem gearbeitet. Diese wird in naher Zukunft verfügbar gemacht. 

## <a name="next-steps"></a>Nächste Schritte

* [Erstellen Sie ein Azure Storage-Konto.](../storage/common/storage-account-create.md)
* [Grundlegendes zu Eingaben für Azure Stream Analytics](stream-analytics-add-inputs.md)
* [Begriffe zu Prüfpunkten und zur Wiedergabe bei Azure Stream Analytics-Aufträgen](stream-analytics-concepts-checkpoint-replay.md)
* [Verwenden von Referenzdaten für Suchvorgänge in Stream Analytics](stream-analytics-use-reference-data.md)
