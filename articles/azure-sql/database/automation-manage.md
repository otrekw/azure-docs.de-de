---
title: Verwalten von Datenbanken mit Azure Automation
description: Erfahren Sie, wie Sie den Azure Automation-Dienst zur Verwaltung von Azure SQL-Datenbank im großen Stil verwenden.
services: sql-database
ms.service: sql-db-mi
ms.subservice: service
ms.custom: sqldbrb=1
ms.devlang: ''
ms.topic: conceptual
author: juliemsft
ms.author: jrasnick
ms.reviewer: sstein
ms.date: 03/12/2019
ms.openlocfilehash: faf5b1108e28b352a0b8622feed8bdd99264ff16
ms.sourcegitcommit: f28ebb95ae9aaaff3f87d8388a09b41e0b3445b5
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/29/2021
ms.locfileid: "91327577"
---
# <a name="manage-databases-in-azure-sql-database-by-using-azure-automation"></a>Verwalten von Datenbanken in Azure SQL-Datenbank mit Azure Automation

[!INCLUDE[appliesto-sqldb-sqlmi](../includes/appliesto-sqldb-sqlmi.md)]

Dieser Leitfaden bietet eine Einführung in den Azure Automation-Dienst und zeigt, wie er zur Vereinfachung der Verwaltung von Datenbanken in Azure SQL-Datenbank verwendet werden kann.

## <a name="about-azure-automation"></a>Grundlegendes zu Azure Automation

[Azure Automation](https://azure.microsoft.com/services/automation/) ist ein Azure-Dienst für die Vereinfachung der Cloudverwaltung durch eine Prozessautomatisierung. Mit Azure Automation können zeitintensive, manuelle, fehleranfällige und häufig ausgeführte Aufgaben automatisiert werden, um Zuverlässigkeit und Effizienz zu steigern und eine schnellere Amortisierung für Ihre Organisation zu erzielen. Weitere Informationen zu den ersten Schritten finden Sie unter [Einführung in Azure Automation](../../automation/automation-intro.md)

Azure Automation bietet eine Workflowausführungs-Engine mit hoher Zuverlässigkeit und Hochverfügbarkeit, die sich an Ihre Bedürfnisse anpasst, wenn Ihr Unternehmen wächst. In Azure Automation können Prozesse manuell, durch Drittanbietersysteme oder in geplanten Intervallen gestartet werden, sodass Aufgaben genau nach Bedarf ausgeführt werden.

Indem Sie die Aufgaben in Zusammenhang mit der Cloudverwaltung mit Azure Automation automatisieren, verringern Sie den Verwaltungsaufwand und ermöglichen es Ihren IT- und DevOps-Mitarbeitern, sich Aufgaben zu widmen, die einen Mehrwert für Ihr Unternehmen liefern.

## <a name="how-azure-automation-can-help-manage-your-databases"></a>So hilft Azure Automation bei der Verwaltung Ihrer Datenbanken

Mit Azure Automation können Sie Datenbanken in Azure SQL-Datenbank mit [PowerShell-Cmdlets](/powershell/module/servicemanagement/azure.service/#sql) verwalten, die in den [Azure PowerShell-Tools](/powershell/azure/) zur Verfügung stehen. In Azure Automation sind diese PowerShell-Cmdlets für Azure SQL-Datenbank sofort einsatzfähig, sodass Sie alle Aufgaben in Zusammenhang mit der Verwaltung Ihrer SQL-Datenbank-Instanz innerhalb des Diensts ausführen können. Sie können diese Cmdlets in Azure Automation auch an die Cmdlets für andere Azure-Dienste koppeln, um komplexe Aufgaben über Azure-Dienste und Drittanbietersysteme hinweg zu automatisieren.

Azure Automation bietet außerdem die Möglichkeit, über die Ausgabe von SQL-Befehlen in PowerShell direkt mit SQL-Servern zu kommunizieren.

Die Runbook- und Modulkataloge für [Azure Automation](../../automation/automation-runbook-gallery.md) bieten zahlreiche Runbooks von Microsoft und der Community, die Sie in Azure Automation importieren können. Um eins zu verwenden, können Sie ein Runbook aus dem Katalog herunterladen, oder Sie können Runbooks direkt aus dem Katalog oder aus Ihrem Automation-Konto im Azure-Portal importieren.

## <a name="next-steps"></a>Nächste Schritte

Nachdem Sie sich nun mit den Grundlagen von Azure Automation und dessen Verwendung zum Verwalten von Azure SQL-Datenbank vertraut gemacht haben, folgen Sie diesen Links, um weitere Informationen zu Azure Automation zu erhalten.

- [Übersicht über Azure Automation](../../automation/automation-intro.md)
- [Mein erstes Runbook](../../automation/learn/automation-tutorial-runbook-graphical.md)
