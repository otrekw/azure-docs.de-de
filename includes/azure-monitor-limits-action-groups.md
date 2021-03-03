---
title: include file
description: include file
services: azure-monitor
author: rboucher
tags: azure-service-management
ms.topic: include
ms.date: 10/01/2020
ms.author: robb
ms.custom: include file
ms.openlocfilehash: 6f9405e1b402b029b628821824a1727dd825a031
ms.sourcegitcommit: c27a20b278f2ac758447418ea4c8c61e27927d6a
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/03/2021
ms.locfileid: "101734038"
---
| Resource | Standardlimit | Maximales Limit |
| --- | --- | --- |
| Azure-App-Push | 10 Azure-App-Aktionen pro Aktionsgruppe. | Wie Standard |
| Email | 1\.000 E-Mail-Aktionen in einer Aktionsgruppe.<br>Maximal 100 E-Mails in einer Stunde.<br>Weitere Informationen finden Sie in den [Informationen zu Ratenbegrenzungen](../articles/azure-monitor/alerts/alerts-rate-limiting.md). | Wie Standard |
| ITSM | 10 ITSM-Aktionen in einer Aktionsgruppe. | Wie Standard | 
| Logik-App | 10 Logik-App-Aktionen in einer Aktionsgruppe. | Wie Standard |
| Runbook | 10 Runbook-Aktionen in einer Aktionsgruppe. | Wie Standard |
| sms | 10 SMS-Aktionen in einer Aktionsgruppe.<br>Maximal eine SMS alle fünf Minuten.<br>Weitere Informationen finden Sie in den [Informationen zu Ratenbegrenzungen](../articles/azure-monitor/alerts/alerts-rate-limiting.md). | Wie Standard |
| Sprache | 10 Sprachaktionen in einer Aktionsgruppe.<br>Maximal ein Anruf alle fünf Minuten.<br>Weitere Informationen finden Sie in den [Informationen zu Ratenbegrenzungen](../articles/azure-monitor/alerts/alerts-rate-limiting.md). | Wie Standard |
| Webhook | 10 Webhookaktionen in einer Aktionsgruppe.  Maximale Anzahl von Webhookaufrufen ist 1500 pro Minute pro Abonnement. Andere Grenzwerte finden Sie unter [Aktionsspezifische Informationen](../articles/azure-monitor/alerts/action-groups.md#action-specific-information).  | Wie Standard |