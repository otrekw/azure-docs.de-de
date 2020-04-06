---
title: include file
description: include file
services: lighthouse
author: JnHs
ms.service: lighthouse
ms.topic: include
ms.date: 03/30/2020
ms.author: jenhayes
ms.custom: include file
ms.openlocfilehash: aacf1c287dca01b5993d35b311eed22e88ef7e87
ms.sourcegitcommit: 7581df526837b1484de136cf6ae1560c21bf7e73
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/31/2020
ms.locfileid: "80421134"
---
Diese Beispiele zeigen, wie Sie Azure Monitor zum Erstellen von Warnungen für Abonnements verwenden, für die ein Onboarding für die delegierte Azure-Ressourcenverwaltung durchgeführt wurde.

| **Vorlage** | **Beschreibung** |
|---------|---------|
| [monitor-delegation-changes](https://github.com/Azure/Azure-Lighthouse-samples/tree/master/Azure-Delegated-Resource-Management/tools/monitor-delegation-changes) | Fragt die Aktivitäten des letzten Tages ab und [erstellt einen Bericht über alle hinzugefügten oder entfernten Delegierungen](../articles/lighthouse/how-to/monitor-delegation-changes.md) (bzw. nicht erfolgreiche Versuche).|
| [alert-using-actiongroup](https://github.com/Azure/Azure-Lighthouse-samples/tree/master/Azure-Delegated-Resource-Management/templates/alert-using-actiongroup) | Mit dieser Vorlage werden eine Azure-Warnung erstellt und eine Verbindung mit einer vorhandenen Aktionsgruppe hergestellt.|
| [multiple-loganalytics-alerts](https://github.com/Azure/Azure-Lighthouse-samples/tree/master/Azure-Delegated-Resource-Management/templates/multiple-loganalytics-alerts) | Erstellt mehrere Protokollwarnungen basierend auf Kusto-Abfragen.|
| [delegation-alert-for-customer](https://github.com/Azure/Azure-Lighthouse-samples/tree/master/Azure-Delegated-Resource-Management/templates/delegation-alert-for-customer) | Stellt eine Warnung in einem Mandanten bereit, wenn ein Benutzer ein Abonnement an einen verwalteten Mandanten delegiert.|
