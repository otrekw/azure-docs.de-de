---
title: Metadaten und Lineage aus Power BI
description: In diesem Artikel wird die Datenlineage-Extraktion aus Power BI beschrieben.
author: chanuengg
ms.author: csugunan
ms.service: purview
ms.subservice: purview-data-catalog
ms.topic: how-to
ms.date: 03/30/2021
ms.openlocfilehash: b7143f7ccb0cb4c91cbb86cadb3cfa78c5c9bec3
ms.sourcegitcommit: b4032c9266effb0bf7eb87379f011c36d7340c2d
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/22/2021
ms.locfileid: "107912557"
---
# <a name="how-to-get-lineage-from-power-bi-into-azure-purview"></a>Wie man Lineage von Power BI in Azure Purview bekommt

Dieser Artikel erläutert die Daten-Lineage-Aspekte der Power BI-Quelle in Azure Purview. Die Voraussetzung zum Anzeigen der Datenlineage in Purview für Power BI ist die [Überprüfung Ihrer Power BI.](../purview/register-scan-power-bi-tenant.md) 

## <a name="common-scenarios"></a>Häufige Szenarios

1. Nachdem die Power BI-Quelle überprüft wurde, können Datenkonsumenten eine Ursachenanalyse eines Berichts oder Dashboards von Purview durchführen. Bei Datenabweichungen in einem Bericht können Benutzer die Downstream-Datasets leicht identifizieren und bei Bedarf ihre Besitzer kontaktieren.

2. Datenersteller können die Downstream-Berichte oder Dashboards sehen, die ihr Dataset verwenden. Bevor sie Änderungen an ihren Datasets vornehmen, können die Datenbesitzer fundierte Entscheidungen treffen.

2. Benutzer können nach Name, Vermerkstatus, Sensitivitätskennzeichnung, Besitzer, Beschreibung und anderen geschäftlichen Aspekten suchen, um die relevanten Power BI-Artefakte zurückzugeben.

## <a name="power-bi-artifacts-in-azure-purview"></a>Power BI-Artefakte in Azure Purview

Nachdem die [Überprüfung Ihrer Power BI](../purview/register-scan-power-bi-tenant.md) abgeschlossen ist, werden die folgenden Power BI Artefakte in Purview inventarisiert

* Capacity
* Arbeitsbereiche
* Datenfluss
* Dataset 
* Bericht
* Dashboard

Die Artefakte des Arbeitsbereichs zeigen die Linie vom Datenfluss -> Datensatz -> Bericht -> Dashboards an

:::image type="content" source="./media/how-to-lineage-powerbi/powerbi-overview.png" alt-text="Screenshot, der anzeigt, wie die Registerkarte Übersicht für Power BI-Assets dargestellt wird." lightbox="./media/how-to-lineage-powerbi/powerbi-overview.png":::

>[!Note]
> * Spalten-Lineage und Transformationen innerhalb von PowerBI Datasets werden derzeit nicht unterstützt
> * Für die Datenquellen, aus denen der PowerBI-Datenfluss oder das PowerBI-Dataset erstellt wird, werden derzeit nur begrenzte Informationen angezeigt. Z.B.: Für die SQL-Server-Quelle eines PowerBI-Datasets wird nur der Servername erfasst. 

## <a name="lineage-of-power-bi-artifacts-in-azure-purview"></a>Lineage von Power BI-Artefakten in Azure Purview

Benutzer können nach dem Power BI nach Namen, Beschreibung oder anderen Details suchen, um relevante Ergebnisse anzuzeigen. Auf der Registerkarte „Asset-Übersicht & Eigenschaften" werden die grundlegenden Details wie Beschreibung, Klassifizierung und andere Informationen angezeigt. Auf der Lineage-Registerkarte werden Assetbeziehungen mit den Upstream- und Downstreamabhängigkeiten angezeigt.

:::image type="content" source="./media/how-to-lineage-powerbi/powerbi-lineage.png" alt-text="Screenshot, der anzeigt, wie die Lineage für Power BI dargestellt wird." lightbox="./media/how-to-lineage-powerbi/powerbi-lineage.png":::

## <a name="next-steps"></a>Nächste Schritte

- [Erfahren Sie mehr über Data Lineage in Azure Purview](catalog-lineage-user-guide.md)
- [Verknüpfen Sie Azure Data Factory, um die automatisierte Lineage zu pushen](how-to-link-azure-data-factory.md)
