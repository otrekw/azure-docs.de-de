---
title: Azure-Abonnementstatus
description: In diesem Artikel werden die verschiedenen Zustände und Statusoptionen eines Azure-Abonnements beschrieben.
keywords: Azure-Abonnementzustand/-status
author: anuragdalmia
ms.reviewer: banders
tags: billing
ms.service: cost-management-billing
ms.topic: conceptual
ms.date: 06/11/2020
ms.author: andalmia
ms.openlocfilehash: 8deda3d8f584c83b61ae50c86c3ee9f43b247ae2
ms.sourcegitcommit: c4ad4ba9c9aaed81dfab9ca2cc744930abd91298
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/12/2020
ms.locfileid: "84735403"
---
# <a name="azure-subscription-states"></a>Azure-Abonnementstatus

In diesem Artikel werden die verschiedenen Status beschrieben, die ein Azure-Abonnement aufweisen kann. Diese Zustände werden in den Abonnementbereichen des Azure-Portals als **Status** angezeigt.

| Abonnementstatus | BESCHREIBUNG |
|-------------| ----------------|
| **Aktiv** | Ihr Azure-Abonnement ist aktiv. Das Abonnement kann zum Bereitstellen neuer Ressourcen sowie zum Verwalten bereits vorhandener Ressourcen verwendet werden.<br><br>Alle Vorgänge (PUT, PATCH, DELETE, POST, GET) sind für die Ressourcenanbieter verfügbar, die [für Ihr Abonnement registriert](../../azure-resource-manager/management/resource-providers-and-types.md#azure-portal) sind. |
| **Gelöscht** | Ihr Azure-Abonnement wurde zusammen mit allen zugrunde liegenden Ressourcen/Daten gelöscht.<br><br>Es sind keine Vorgänge verfügbar. |
| **Disabled** | Ihr Azure-Abonnement ist deaktiviert, und Sie können damit keine Azure-Ressourcen mehr erstellen oder verwalten. In diesem Status ist die Zuordnung Ihrer virtuellen Computer aufgehoben, temporäre IP-Adressen sind freigegeben, der Speicher ist schreibgeschützt, und andere Dienste sind deaktiviert. Die Deaktivierung eines Abonnements kann folgende Ursachen haben: Ihr Guthaben ist abgelaufen. Sie haben Ihr Ausgabenlimit erreicht. Es ist eine überfällige Rechnung vorhanden. Ihr Kreditkartenlimit wurde überschritten. Oder: Das Abonnement wurde explizit deaktiviert oder gekündigt. Ein Abonnement kann abhängig vom Abonnementtyp bis zu 90 Tage deaktiviert bleiben. Danach wird es endgültig gelöscht. Weitere Informationen finden Sie unter [Reaktivieren eines deaktivierten Azure-Abonnements](subscription-disabled.md).<br><br>Vorgänge zum Erstellen oder Aktualisieren von Ressourcen (PUT, PATCH) sind deaktiviert. Vorgänge, die eine Aktion ausführen (POST), sind ebenfalls deaktiviert. Sie können Ressourcen abrufen oder löschen (GET, DELETE). Ihre Ressourcen sind weiterhin verfügbar. |
| **Abgelaufen** | Ihr Azure-Abonnement ist abgelaufen, da es gekündigt wurde. Abgelaufene Abonnements können reaktiviert werden. Weitere Informationen finden Sie unter [Reaktivieren eines deaktivierten Azure-Abonnements](subscription-disabled.md).<br><br>Vorgänge zum Erstellen oder Aktualisieren von Ressourcen (PUT, PATCH) sind deaktiviert. Vorgänge, die eine Aktion ausführen (POST), sind ebenfalls deaktiviert. Sie können Ressourcen abrufen oder löschen (GET, DELETE).|
| **Überfällig** | Für Ihr Azure-Abonnement liegt eine ausstehende Zahlung vor. Ihr Abonnement ist zwar noch aktiv, wenn die fälligen Gebühren jedoch nicht bezahlt werden, kann das Abonnement deaktiviert werden. Weitere Informationen finden Sie unter [Begleichen überfälliger Zahlungen für Ihr Azure-Abonnement](resolve-past-due-balance.md).<br><br>Alle Vorgänge sind verfügbar. |
