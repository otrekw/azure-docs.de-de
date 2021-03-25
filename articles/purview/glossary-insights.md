---
title: Glossarbericht zu Ihren Daten mithilfe von Purview-Erkenntnissen
description: In dieser Schrittanleitung wird beschrieben, wie Sie die Glossarberichterstellung mit Purview-Erkenntnissen für Ihre Daten anzeigen und verwenden können.
author: SunetraVirdi
ms.author: suvirdi
ms.service: purview
ms.subservice: purview-data-catalog
ms.topic: how-to
ms.date: 11/20/2020
ms.openlocfilehash: eb1d59ae41b04be60dec90aaee4b2305b6d39ca6
ms.sourcegitcommit: 867cb1b7a1f3a1f0b427282c648d411d0ca4f81f
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/20/2021
ms.locfileid: "102095849"
---
# <a name="glossary-insights-on-your-data-in-azure-purview"></a>Glossarerkenntnisse über Ihre Daten in Azure Purview

In dieser Schrittanleitung wird beschrieben, wie Sie auf Purview-Glossar-Erkenntnisberichte für Ihre Daten zugreifen, sie anzeigen und filtern können.

In dieser Schrittanleitung lernen Sie Folgendes:

> [!div class="checklist"]
> - Wechseln zu Erkenntnissen aus Ihrem Purview-Konto
> - Erhalten eines Überblicks über Ihre Daten

## <a name="prerequisites"></a>Voraussetzungen

Stellen Sie vor dem Beginn mit Purview--Erkenntnissen sicher, dass Sie die folgenden Schritte ausgeführt haben:

- Einrichten Ihrer Azure-Ressourcen und Auffüllen des Kontos mit Daten

- Einrichten und Ausführen eines Scans für den Quelltyp

- Einrichten eines Glossars und Anfügen von Ressourcen an Glossarbegriffe

Weitere Informationen finden Sie unter [Verwalten von Datenquellen in Azure Purview (Vorschau)](manage-data-sources.md).

## <a name="use-purview-glossary-insights"></a>Verwenden von Purview-Glossarerkenntnissen

In Azure Purview können Sie Glossarbegriffe erstellen und diese Ressourcen anfügen. Später können Sie die Glossarverteilung in „Glossarerkenntnisse“ anzeigen. So erfahren Sie den Status Ihres Glossars gemäß der Begriffe, die Ressourcen angefügt sind. Außerdem werden Begriffe nach Status und Verteilung der Rollen nach Anzahl der Benutzer aufgeführt.

**So zeigen Sie „Glossarerkenntnisse“ an:**

1. Wechseln Sie zum **Azure Purview**-[Instanzbildschirm im Azure-Portal](https://aka.ms/purviewportal), und wählen Sie Ihr Purview-Konto aus.

1. Wählen Sie auf der Seite **Übersicht** im Abschnitt **Erste Schritte** die Kontokachel **Purview starten** aus.

   :::image type="content" source="./media/glossary-insights/portal-access.png" alt-text="„Purview starten“ im Azure-Portal":::

1. Wählen Sie auf der **Startseite** von Purview die Kachel **Erkenntnisse anzeigen** aus, um auf Ihren Bereich **Erkenntnisse** (:::image type="icon" source="media/glossary-insights/ico-insights.png" border="false":::) zuzugreifen.

   :::image type="content" source="./media/glossary-insights/view-insights.png" alt-text="Anzeigen Ihrer Erkenntnisse im Azure-Portal":::

1. Wählen Sie im Bereich **Erkenntnisse** :::image type="icon" source="media/glossary-insights/ico-insights.png" border="false"::: die Option **Glossar** aus, um den **Glossarerkenntnisse**-Bericht von Purview anzuzeigen.

**Glossarerkenntnisse** bieten Geschäftskunden wertvolle Informationen, um ein genau definiertes Glossar für ihre Organisation zu verwalten.

1. Der Bericht beginnt mit **Allgemeine KPIs**. Dort werden die **_Total terms_ *(Begriffe insgesamt) in Ihrem Purview-Konto,* _Genehmigte Begriffe ohne Ressourcen_*und*_Abgelaufene Begriffe mit Ressourcen_** angezeigt. Mit jedem dieser Werte können Sie die Integrität Ihres Glossars ermitteln.

   :::image type="content" source="./media/glossary-insights/glossary-kpi.png" alt-text="Anzeige der KPIs in den Glossarerkenntnissen "::: 


2. Im Abschnitt **Momentaufnahme von Begriffen** (darüber angezeigt) werden für Begriffe mit Ressourcen und für Begriffe ohne Ressourcen die Begriffsstatus **_Entwurf_ *,* _Genehmigt_ *,* _Warnung_*und*_Abgelaufen_** angezeigt.

3. Klicken Sie auf **Mehr anzeigen**, um die Begriffsnamen mit verschiedenen Status und weitere Details zu  **_Inhaltsverantwortlichen_*und*_Experten_** anzuzeigen. 

   :::image type="content" source="./media/glossary-insights/glossary-view-more.png" alt-text="Momentaufnahme von Begriffen mit und ohne Ressourcen":::  

4. Wenn Sie für **Approved terms with assets** (Genehmigte Begriffe mit Ressourcen) auf „Mehr anzeigen“ klicken, können Sie zur Detailseite für Begriffe im *Glossar* und von dort zur Liste der Ressourcen mit den zugehörigen Begriffen navigieren. 

   :::image type="content" source="./media/glossary-insights/navigate-to-glossary-detail.png" alt-text="Einblicke in das Glossar"::: 

4. Zeigen Sie auf der Seite „Glossarerkenntnisse“ die Verteilung **Unvollständiger Begriffe** nach dem Typ der fehlenden Informationen an. Im Diagramm wird in den Feldern **_Missing Definition_ *(Fehlende Definition),* _Missing Expert_ *(Fehlender Experte),* _Missing Steward_ *(Fehlender Inhaltsverantwortlicher) und*_Missing multiple_**  (Mehrere fehlend) die Anzahl der Begriffe angezeigt, für die die entsprechenden Informationen fehlen.

1. Klicken Sie in „Unvollständige Begriffe“ auf **Mehr anzeigen**, um die Begriffe anzuzeigen, für die Informationen fehlen. Sie können zur Detailseite für Glossarbegriffe navigieren, um die fehlenden Informationen für einen Begriff einzugeben und sicherzustellen, dass der Glossarbegriff vollständig ist.

## <a name="next-steps"></a>Nächste Schritte

Erfahren Sie mehr über das Erstellen eines [Glossarbegriffs](./how-to-create-import-export-glossary.md).