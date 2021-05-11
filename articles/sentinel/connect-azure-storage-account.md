---
title: Verbinden von Azure Storage-Konto-Diagnoseprotokollen mit Azure Sentinel
description: Erfahren Sie, wie Sie Azure Policy verwenden, um Azure Storage-Konto-Diagnoseprotokolle mit Azure Sentinel zu verbinden.
author: yelevin
manager: rkarlin
ms.service: azure-sentinel
ms.subservice: azure-sentinel
ms.topic: how-to
ms.date: 04/21/2021
ms.author: yelevin
ms.openlocfilehash: e9f27b5cc3c4cb526dbcf6e6511fcd53e3c2fe67
ms.sourcegitcommit: 5ce88326f2b02fda54dad05df94cf0b440da284b
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/22/2021
ms.locfileid: "107891718"
---
# <a name="connect-azure-storage-account-diagnostics-logs"></a>Verbinden von Azure Storage-Konto-Diagnoseprotokollen

> [!IMPORTANT]
> Der Azure Storage-Konto-Connector befindet sich derzeit in der **VORSCHAU.** Die [zusätzlichen Nutzungsbestimmungen für Microsoft Azure-Vorschauen](https://azure.microsoft.com/support/legal/preview-supplemental-terms/) enthalten zusätzliche rechtliche Bedingungen, die für Azure-Features gelten, die sich in der Beta- oder Vorschauversion befinden bzw. anderweitig noch nicht zur allgemeinen Verfügbarkeit freigegeben sind.

Das Azure Storage-Konto ist eine Cloud-Lösung für moderne Datenspeicherszenarien. Es enthält alle Ihre Datenobjekte: Blobs, Dateien, Warteschlangen, Tabellen und Datenträger.

Mit diesem Connector können Sie die Diagnoseprotokolle Ihrer Azure-Storage-Konten in Azure Sentinel streamen, wodurch Sie den Prozess kontinuierlich überwachen und Sicherheitsbedrohungen in all Ihren Azure-Storage-Ressourcen innerhalb Ihrer Organisation erkennen können.

Weitere Informationen zur [Überwachung des Azure Storage-Kontos](../storage/common/storage-analytics-logging.md).

## <a name="prerequisites"></a>Voraussetzungen

So erfassen Sie Azure Storage-Konto-Diagnoseprotokolle in Azure Sentinel:

- Sie benötigen Lese- und Schreibberechtigungen für den Azure Sentinel-Arbeitsbereich.

- Wenn Sie Azure Policy verwenden möchten, um eine Richtlinie zum Protokollstreaming in Azure Storage-Ressourcen anzuwenden, müssen Sie die Rolle „Besitzer“ für den Zuweisungsbereich der Richtlinie besitzen.

## <a name="connect-to-azure-storage-account"></a>Verbinden mit dem Azure-Speicherkonto

Dieser Connector verwendet Azure Policy, um eine einzige Protokoll-Streaming-Konfiguration auf eine Sammlung von Azure-Storage-Konto-Ressourcen anzuwenden, die als Bereich definiert sind. Die verfügbaren Azure Storage-Protokoll- und Metrik-Typen sehen Sie links auf der Connector-Seite unter **Datentypen**.

1. Klicken Sie im Azure Sentinel-Navigationsmenü auf **Data connectors** (Datenconnectors).

1. Wählen Sie im Katalog für Datenconnectors die Option **Azure Storage-Konto** und dann im Vorschaubereich **Connectorseite öffnen** aus.

1. Erweitern Sie im Abschnitt **Konfiguration** der Connectorseite die Option **Streamdiagnoseprotokolle von Ihrem Azure Storage-Konto großflächig**.

1. Wählen Sie die Schaltfläche **Azure Policy-Zuweisungs-Assistent starten** aus.

    Der Richtlinien-Zuweisungs-Assistent wird geöffnet, und Sie können eine neue Richtlinie mit dem Namen **Diagnoseeinstellungen für Speicherkonten für Log-Analytics-Arbeitsbereich konfigurieren** erstellen.

    1. Klicken Sie auf der Registerkarte **Grundlagen** neben **Bereich** auf die Schaltfläche mit den drei Punkten, um Ihr Abonnement (und optional eine Ressourcengruppe) auszuwählen. Sie können auch eine Beschreibung hinzufügen.

    1. In der Registerkarte **Parameter**:
        - Wählen Sie in der Dropdownliste **Log Analytics-Arbeitsbereich** Ihren Azure Sentinel-Arbeitsbereich aus.
        - Wählen Sie in der Dropdownliste **Bereitzustellende Speicherdienste** die Speicherressourcentypen (Datei, Tabelle, Warteschlange usw.) aus, für die Sie Diagnoseeinstellungen bereitstellen möchten.
        - Lassen Sie die Felder **Einstellungsname** und **Effekt** unverändert.
        - Die verbleibenden Dropdown-Felder stellen die verfügbaren Diagnoseprotokoll- und Metrik-Typen dar. Behalten Sie für alle Protokolltypen, die erfasst werden sollen, die Einstellung auf „True“ bei.

    1. Mit den obigen Schritten wird die Richtlinie auf alle zukünftigen Speicherressourcen angewendet. Aktivieren Sie zum Anwenden der Richtlinie auf Ihre vorhandenen Ressourcen auf der Registerkarte **Korrektur** das Kontrollkästchen **Korrekturtask erstellen**.

    1. Klicken Sie auf der Registerkarte **Überprüfen + erstellen** auf **Erstellen**. Damit ist die Richtlinie dem ausgewählten Bereich zugewiesen.

> [!NOTE]
>
> Mit diesem speziellen Datenconnector werden die Konnektivitätsstatusindikatoren (ein Farbstreifen im Datenconnectors-Katalog sowie Verbindungssymbole neben den Datentypnamen) nur dann als *verbunden* (grün) angezeigt, wenn Daten irgendwann innerhalb der letzten 14 Tage erfasst wurden. Wenn 14 Tage ohne Datenerfassung vergangen sind, wird der Connector als „getrennt“ angezeigt. In dem Moment, in dem weitere Daten den Connector passieren, wird der Status wieder als *verbunden* angezeigt.

## <a name="next-steps"></a>Nächste Schritte

In diesem Dokument haben Sie erfahren, wie Sie Azure Storage-Konto mit Azure Sentinel verbinden. Weitere Informationen zu Azure Sentinel finden Sie in den folgenden Artikeln:

- Erfahren Sie, wie Sie [Einblick in Ihre Daten und potenzielle Bedrohungen erhalten](quickstart-get-visibility.md).
- Beginnen Sie mit der [Erkennung von Bedrohungen mithilfe von Azure Sentinel](tutorial-detect-threats-built-in.md).
