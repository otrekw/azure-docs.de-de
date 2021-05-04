---
title: Verbinden von Azure Key Vault-Diagnoseprotokollen mit Azure Sentinel
description: Erfahren Sie, wie Sie Azure Policy verwenden, um Azure Key Vault-Diagnoseprotokolle mit Azure Sentinel zu verbinden.
author: yelevin
manager: rkarlin
ms.service: azure-sentinel
ms.subservice: azure-sentinel
ms.topic: how-to
ms.date: 04/22/2021
ms.author: yelevin
ms.openlocfilehash: fe86c963ab0ec009b527d2d6138a8af2af9ba68a
ms.sourcegitcommit: 5ce88326f2b02fda54dad05df94cf0b440da284b
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/22/2021
ms.locfileid: "107890832"
---
# <a name="connect-azure-key-vault-diagnostics-logs"></a>Verbinden von Azure Key Vault-Diagnoseprotokollen

Azure Key Vault ist ein Clouddienst zum sicheren Speichern und Zugreifen auf Geheimnisse. Als Geheimnis wird alles bezeichnet, für das Sie den Zugriff streng kontrollieren möchten, z. B. API-Schlüssel, Kennwörter, Zertifikate oder kryptografische Schlüssel.

Mit diesem Connector können Sie Azure Key Vault-Diagnoseprotokolle zu Azure Sentinel streamen, um die Aktivitäten in allen Instanzen kontinuierlich überwachen zu können.

Erfahren Sie mehr über die [Überwachung von Azure Key Vault](../azure-monitor/insights/key-vault-insights-overview.md) und [Azure Key Vault-Diagnosetelemetriedaten](../key-vault/general/logging.md).

## <a name="prerequisites"></a>Voraussetzungen

So erfassen Sie Azure Key Vault-Protokolle in Azure Sentinel

- Sie benötigen Lese- und Schreibberechtigungen für den Azure Sentinel-Arbeitsbereich.

- Wenn Sie Azure Policy verwenden möchten, um eine Richtlinie zum Protokollstreaming in Azure Key Vault-Ressourcen anzuwenden, müssen Sie die Rolle „Besitzer“ für den Zuweisungsbereich der Richtlinie besitzen.

## <a name="connect-to-azure-key-vault"></a>Herstellen einer Verbindung mit Azure Key Vault

Dieser Connector verwendet Azure Policy, um eine einzelne Konfiguration zum Azure Key Vault-Protokollstreaming auf eine Sammlung von Instanzen anzuwenden, die als Bereich definiert sind. Die in Azure Key Vault erfassten Protokolltypen werden links auf der Connectorseite unter **Datentypen** angezeigt.

1. Klicken Sie im Azure Sentinel-Navigationsmenü auf **Data connectors** (Datenconnectors).

1. Wählen Sie im Katalog für Datenconnectors die Option **Azure Key Vault** und dann im Vorschaubereich **Connectorseite öffnen** aus.

1. Erweitern Sie im Abschnitt **Konfiguration** der Connectorseite die Option **Streamdiagnoseprotokolle von Ihrem Azure Key Vault im großen Stil**.

1. Wählen Sie die Schaltfläche **Launch Azure Policy Assignment wizard** (Azure Policy-Zuweisungs-Assistent starten) aus.

    Der Richtlinienzuweisungs-Assistent wird geöffnet, und Sie können eine neue Richtlinie mit dem Namen **Bereitstellen: Diagnoseeinstellungen für Azure Key Vault in Log Analytics-Arbeitsbereich konfigurieren** erstellen.

    1. Klicken Sie auf der Registerkarte **Grundlagen** unter **Bereich** auf die Schaltfläche mit den drei Punkten, um Ihr Abonnement (und optional eine Ressourcengruppe) auszuwählen. Sie können auch eine Beschreibung hinzufügen.

    1. Lassen Sie auf der Registerkarte **Parameter** die Felder **Auswirkung** und **Einstellungsname** unverändert. Wählen Sie in der Dropdownliste **Log Analytics-Arbeitsbereich** Ihren Azure Sentinel-Arbeitsbereich aus. Die übrigen Dropdownfelder stellen die verfügbaren Diagnoseprotokolltypen dar. Behalten Sie für alle Protokolltypen, die erfasst werden sollen, die Markierung „True“ bei.

    1. Die Richtlinie wird auf in Zukunft hinzugefügte Ressourcen angewendet. Wählen Sie zum Anwenden der Richtlinie auf Ihre vorhandenen Ressourcen die Registerkarte **Korrektur** aus, und aktivieren Sie das Kontrollkästchen **Korrekturtask erstellen**.

    1. Klicken Sie auf der Registerkarte **Überprüfen + erstellen** auf **Erstellen**. Damit ist die Richtlinie dem ausgewählten Bereich zugewiesen.

> [!NOTE]
>
> Mit diesem speziellen Datenconnector werden die Konnektivitätsstatusindikatoren (ein Farbstreifen im Datenconnectors-Katalog sowie Verbindungssymbole neben den Datentypnamen) nur dann als *verbunden* (grün) angezeigt, wenn Daten irgendwann innerhalb der letzten 14 Tage erfasst wurden. Wenn 14 Tage ohne Datenerfassung vergangen sind, wird der Connector als „getrennt“ angezeigt. In dem Moment, in dem weitere Daten den Connector passieren, wird der Status wieder als *verbunden* angezeigt.

## <a name="next-steps"></a>Nächste Schritte

In diesem Dokument haben Sie erfahren, wie Sie mithilfe von Azure Policy Azure Key Vault mit Azure Sentinel verbinden. Weitere Informationen zu Azure Sentinel finden Sie in den folgenden Artikeln:

- Erfahren Sie, wie Sie [Einblick in Ihre Daten und potenzielle Bedrohungen erhalten](quickstart-get-visibility.md).
- Beginnen Sie mit der [Erkennung von Bedrohungen mithilfe von Azure Sentinel](tutorial-detect-threats-built-in.md).
