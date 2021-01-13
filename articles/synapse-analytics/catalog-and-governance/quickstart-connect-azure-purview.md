---
title: Herstellen einer Verbindung mit einem Azure Purview-Konto 
description: Herstellen einer Verbindung zwischen einem Azure Purview-Konto und einem Synapse-Arbeitsbereich
services: synapse-analytics
author: ArnoMicrosoft
ms.service: synapse-analytics
ms.topic: quickstart
ms.date: 12/16/2020
ms.author: acomet
ms.reviewer: jrasnick
ms.openlocfilehash: f85a76a7ce01c11255f54c6b956088f9328dfc9f
ms.sourcegitcommit: 67b44a02af0c8d615b35ec5e57a29d21419d7668
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/06/2021
ms.locfileid: "97918464"
---
# <a name="quickstartconnect-an-azure-purview-account-to-a-synapse-workspace"></a>Schnellstart: Herstellen einer Verbindung zwischen einem Azure Purview-Konto und einem Synapse-Arbeitsbereich 

> [!IMPORTANT]
> Die Integration zwischen Azure Synapse Analytics und Azure Purview befindet sich derzeit in der Vorschauphase. Wenn Sie Azure Purview in Synapse ausprobieren möchten, wenden Sie sich an einen Microsoft-Vertriebsmitarbeiter.

In dieser Schnellstartanleitung registrieren Sie ein Azure Purview-Konto in einem Synapse-Arbeitsbereich. Dank dieser Verbindung können Sie Azure Purview-Ressourcen ermitteln und mit ihnen über Synapse-Funktionen interagieren. 

Sie können die folgenden Aufgaben in Synapse ausführen: 
- Suchen von Purview-Objekten anhand von Schlüsselwörtern mithilfe des Suchfelds im oberen Bereich 
- Verstehen der Daten auf der Grundlage von Metadaten, Herkunft und Anmerkungen 
- Verbinden dieser Daten mit Ihrem Arbeitsbereich mit verknüpften Diensten oder Integrationsdatasets 
- Analysieren dieser Datasets mit Synapse Apache Spark, Synapse SQL und Datenflüssen 

## <a name="prerequisites"></a>Voraussetzungen 
- [Azure Purview-Konto](../../purview/create-catalog-portal.md) 
- [Synapse-Arbeitsbereich](../quickstart-create-workspace.md) 

## <a name="signin-toa-synapse-workspace"></a>Anmelden bei einem Synapse-Arbeitsbereich 

Navigieren Sie zu https://web.azuresynapse.net, und melden Sie sich bei Ihrem Arbeitsbereich an. 

## <a name="permissions-for-connecting-an-azure-purview-account"></a>Berechtigungen zum Herstellen einer Verbindung mit einem Azure Purview-Konto 

- Zum Herstellen einer Verbindung zwischen einem Azure Purview-Konto und einem Synapse-Arbeitsbereich benötigen Sie die Rolle **Mitwirkender** im Synapse-Arbeitsbereich (über das IAM im Azure-Portal) sowie Zugriff auf das entsprechende Azure Purview-Konto.

## <a name="connect-an-azure-purview-account"></a>Herstellen einer Verbindung mit einem Azure Purview-Konto  

- Navigieren Sie im Synapse-Arbeitsbereich zu **Verwalten** -> **Azure Purview**. Wählen Sie **Connect to a Purview account** (Mit einem Purview-Konto verbinden) aus. 
- Sie können **Aus Azure-Abonnement** oder **Manuell eingeben** auswählen. Unter **Aus Azure-Abonnement** können Sie das Konto auswählen, auf das Sie Zugriff haben. 
- Nachdem die Verbindung hergestellt wurde, sollte der Name des Purview-Kontos auf der Registerkarte **Azure Purview-Konto** angezeigt werden. 
- Sie können über die Suchleiste oben in der Mitte des Synapse-Arbeitsbereichs nach Daten suchen. 

## <a name="nextsteps"></a>Nächste Schritte 

[Registrieren und Überprüfen von Azure Synapse Analytics](../../purview/register-scan-azure-synapse-analytics.md)

[Entdecken, Verbinden und Untersuchen von Daten in Synapse mithilfe von Azure Purview](how-to-discover-connect-analyze-azure-purview.md)   