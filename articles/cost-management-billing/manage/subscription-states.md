---
title: Azure-Abonnementstatus
description: In diesem Artikel werden die verschiedenen Zustände und Statusoptionen eines Azure-Abonnements beschrieben.
keywords: Azure-Abonnementzustand/-status
author: anuragdalmia
ms.reviewer: banders
tags: billing
ms.service: cost-management-billing
ms.topic: conceptual
ms.date: 03/03/2020
ms.author: andalmia
ms.openlocfilehash: 29af3d064b481548cdb9b9518e9735eb34aaf034
ms.sourcegitcommit: d45fd299815ee29ce65fd68fd5e0ecf774546a47
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/04/2020
ms.locfileid: "78270238"
---
# <a name="azure-subscription-states"></a>Azure-Abonnementstatus

In diesem Artikel werden die verschiedenen Zustände beschrieben, die ein Azure-Abonnement aufweisen kann. Diese Zustände werden in den Abonnementbereichen des Azure-Portals als **Status** angezeigt.

| Abonnementstatus | Beschreibung |
|-------------| ----------------|
| **Aktiv** | Ihr Azure-Abonnement ist aktiv. Das Abonnement kann zum Bereitstellen neuer Ressourcen sowie zum Verwalten bereits vorhandener Ressourcen verwendet werden.|
| **Gelöscht** | Ihr Azure-Abonnement wurde zusammen mit allen zugrunde liegenden Ressourcen/Daten gelöscht. |
| **Disabled** | Ihr Azure-Abonnement ist deaktiviert, und Sie können damit keine Azure-Ressourcen mehr erstellen oder verwalten. In diesem Status ist die Zuordnung Ihrer virtuellen Computer aufgehoben, temporäre IP-Adressen sind freigegeben, der Speicher ist schreibgeschützt, und andere Dienste sind deaktiviert. Die Deaktivierung eines Abonnements kann folgende Ursachen haben: Ihr Guthaben ist abgelaufen. Sie haben Ihr Ausgabenlimit erreicht. Es ist eine überfällige Rechnung vorhanden. Ihr Kreditkartenlimit wurde überschritten. Oder: Das Abonnement wurde explizit deaktiviert oder gekündigt. Ein Abonnement kann abhängig vom Abonnementtyp bis zu 90 Tage deaktiviert bleiben. Danach wird es endgültig gelöscht. Weitere Informationen finden Sie unter [Reaktivieren eines deaktivierten Azure-Abonnements](subscription-disabled.md). |
| **Abgelaufen** | Ihr Azure-Abonnement ist abgelaufen, da es gekündigt wurde. Abgelaufene Abonnements können reaktiviert werden. Weitere Informationen finden Sie unter [Reaktivieren eines deaktivierten Azure-Abonnements](subscription-disabled.md).|
| **Überfällig** | Für Ihr Azure-Abonnement liegt eine ausstehende Zahlung vor. Ihr Abonnement ist zwar noch aktiv, wenn die fälligen Gebühren jedoch nicht bezahlt werden, kann das Abonnement deaktiviert werden. Weitere Informationen finden Sie unter [Begleichen überfälliger Zahlungen für Ihr Azure-Abonnement](resolve-past-due-balance.md). |
