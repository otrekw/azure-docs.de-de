---
title: Aktualisieren eines Azure Red Hat OpenShift-Clusters
description: Erfahren Sie, wie Sie einen privaten Azure Red Hat OpenShift-Cluster mit OpenShift 4 erstellen.
ms.service: azure-redhat-openshift
ms.topic: article
ms.date: 1/10/2021
author: sakthi-vetrivel
ms.author: suvetriv
keywords: aro, openshift, az aro, red hat, cli
ms.openlocfilehash: 742da12bd3a10cd1f541e9c43f654cfe7df04340
ms.sourcegitcommit: f28ebb95ae9aaaff3f87d8388a09b41e0b3445b5
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/29/2021
ms.locfileid: "101720884"
---
# <a name="upgrade-an-azure-red-hat-openshift-aro-cluster"></a>Aktualisieren eines Azure Red Hat OpenShift-Clusters (ARO)

Im Rahmen des ARO-Clusterlebenszyklus müssen regelmäßige Upgrades auf die aktuelle OpenShift-Version ausgeführt werden. Es ist wichtig, jeweils die aktuellen Sicherheitsversionen anzuwenden oder bei einem Upgrade die neuesten Features zu erhalten. In diesem Artikel wird gezeigt, wie Sie alle Komponenten in einem OpenShift-Cluster mithilfe der OpenShift-Webkonsole aktualisieren.

## <a name="before-you-begin"></a>Voraussetzungen

Der Artikel setzt voraus, dass Sie mindestens Version 2.0.65 der Azure-Befehlszeilenschnittstelle (Azure CLI) ausführen. Führen Sie `az --version` aus, um Ihre aktuelle Version zu ermitteln. Informationen zum Durchführen einer Installation oder eines Upgrades finden Sie bei Bedarf unter [Installieren der Azure CLI](/cli/azure/install-azure-cli).

In diesem Artikel wird davon ausgegangen, dass Sie Zugriff auf einen Azure Red Hat OpenShift-Cluster mit `admin`-Berechtigungen haben.

## <a name="check-for-available-aro-cluster-upgrades"></a>Suchen nach verfügbaren ARO-Clusterupgrades

Wählen Sie in der OpenShift-Webkonsole **Administration** > **Cluster Settings** (Verwaltung > Clustereinstellungen) aus, und öffnen Sie die Registerkarte **Details**.

Wenn als **Update Status** für Ihren Cluster **Updates available** (Updates verfügbar) angezeigt wird, können Sie den Cluster aktualisieren.

## <a name="upgrade-your-aro-cluster"></a>Aktualisieren des ARO-Clusters

Legen Sie in der Webkonsole aus dem vorherigen Schritt den **Channel** (Kanal) auf den für die Version fest, auf die Sie ein Update ausführen möchten, z. B. `stable-4.5`.

Auswählen einer Version für das Update und Auswählen von **Update** Der Updatestatus wird in `Update to <product-version> in progress` geändert. Sie können den Fortschritt der Clusteraktualisierung überprüfen, indem Sie die Fortschrittsleisten für die Operatoren und Knoten überwachen.

## <a name="next-steps"></a>Nächste Schritte
- [Weitere Informationen zum Aktualisieren eines ARO-Clusters mithilfe der OC-Befehlszeilenschnittstelle](https://docs.openshift.com/container-platform/4.6/updating/updating-cluster-between-minor.html)
- Informationen zu den verfügbaren Empfehlungen und Updates für OpenShift Container Platform finden Sie im Abschnitt [Errata](https://access.redhat.com/downloads/content/290/ver=4.6/rhel---8/4.6.0/x86_64/product-errata) des Kundenportals.
