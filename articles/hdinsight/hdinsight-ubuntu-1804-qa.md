---
title: Azure HDInsight Ubuntu 18.04-Update
description: Erfahren Sie mehr über Betriebssystemänderungen für Azure HDInsight Ubuntu 18.04.
ms.service: hdinsight
ms.topic: conceptual
ms.author: yanacai
author: yanancai
ms.date: 05/25/2021
ms.openlocfilehash: 61d801d7091d2a619ff6a8b6436f386c125ca4be
ms.sourcegitcommit: c385af80989f6555ef3dadc17117a78764f83963
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/04/2021
ms.locfileid: "111415121"
---
# <a name="hdinsight-ubuntu-1804-os-update"></a>HDInsight Ubuntu 18.04-Betriebssystemupdate

Dieser Artikel enthält weitere Details zum HdInsight Ubuntu 18.04-Betriebssystemupdate und zu möglicherweise erforderlichen Änderungen.

## <a name="update-overview"></a>Übersicht über das Update

HDInsight hat im Mai 2021 mit dem Rollout des neuen HDInsight 4.0-Clusterimages unter Ubuntu 18.04 begonnen. Neu erstellte HDInsight 4.0-Cluster werden standardmäßig unter Ubuntu 18.04 ausgeführt, sobald sie verfügbar sind. Vorhandene Cluster unter Ubuntu 16.04 werden ohne vollständige Unterstützung ausgeführt.

HDInsight 3.6 wird weiterhin unter Ubuntu 16.04 ausgeführt. Die Standardunterstützung wird bis zum 30. Juni 2021 beendet und ab dem 1. Juli 2021 in Basic-Support geändert. Weitere Informationen zu Datumsangaben und Supportoptionen finden Sie unter [Azure HDInsight-Versionen](./hdinsight-component-versioning.md). Ubuntu 18.04 wird für HDInsight 3.6 nicht unterstützt. Wenn Sie Ubuntu 18.04 verwenden möchten, müssen Sie Ihre Cluster zu HDInsight 4.0 migrieren.

Sie müssen Ihre Cluster löschen und neu erstellen, wenn Sie vorhandene Cluster auf Ubuntu 18.04 verlagern möchten. Planen Sie die Erstellung oder Neuerstellung des Clusters. 

## <a name="script-actions-changes"></a>Änderungen an Skriptaktionen

HDInsight-Skriptaktionen werden zum Installieren weitere Komponenten und zum Ändern von Konfigurationseinstellungen verwendet. Eine Skriptaktion ist ein Bash-Skript, das auf den Knoten in einem HDInsight-Cluster ausgeführt wird.

Es gibt u. U. einige mögliche Änderungen, die Sie für Skriptaktionen vornehmen müssen.

**Ändern Sie jedes Vorkommen von `xenial` in `bionic`, wenn Sie Pakete erfassen (überall dort, wo dies erforderlich ist):**

Beispiel:
- Aktualisieren Sie `http://packages.treasuredata.com/3/ubuntu/xenial/ xenial contrib` auf `http://packages.treasuredata.com/3/ubuntu/bionic/ bionic contrib`.
- Aktualisieren Sie `http://azure.archive.ubuntu.com/ubuntu/ xenial main restricted` auf `http://azure.archive.ubuntu.com/ubuntu/ bionic main restricted`.

**Einige Paketversionen sind für „bionic“ nicht vorhanden:** 

Beispielsweise ist [Node.js, Version 4.x](https://deb.nodesource.com/node_4.x/dists/) nicht im bionic-Repository vorhanden. [Node.js, Version 12.x](https://deb.nodesource.com/node_12.x/dists/bionic/) ist vorhanden.

Skripts, die alte Versionen installieren, die für „bionic“ nicht vorhanden sind, müssen auf höhere Versionen aktualisiert werden.

**„/etc/rc.local“ ist in 18.04 standardmäßig nicht vorhanden:**

Einige Skripts verwenden `/etc/rc.local`, das in Ubuntu 18.04 standardmäßig aber nicht vorhanden ist, für den Start von Dienste. Es sollte in eine geeignete systemd-Einheit konvertiert werden. 

**Basisbetriebssystempakete wurden aktualisiert:**

Wenn Ihre Skripts auf einem älteren Versionspaket in Ubuntu 16.04 basieren, funktioniert das Skript möglicherweise nicht. Rufen Sie SSH auf dem Clusterknoten auf, und führen Sie `dpkg --list` auf dem Clusterknoten aus, um die Details aller installierten Pakete anzuzeigen.
 
**Im Allgemeinen sind Regeln in Ubuntu 18.04 strenger als in 16.04.**

## <a name="custom-applications"></a>Benutzerdefinierte Anwendungen
Einige [Drittanbieteranwendungen](./hdinsight-apps-install-applications.md) können im HDInsight-Cluster installiert werden. Diese Anwendungen funktionieren möglicherweise mit Ubuntu 18.04 nicht einwandfrei. Um das Risiko von Breaking Changes zu verringern, führt HDInsight seit dem 25. Februar 2021 kein Rollout des neuen Images für Abonnements aus, die benutzerdefinierte Anwendungen installiert haben. Öffnen Sie ein Supportticket, wenn Sie das neue Image mit Ihren Testabonnements ausprobieren möchten, um Ihr Abonnement zu aktivieren.

## <a name="edge-nodes"></a>Edgeknoten
Mit dem neuen Image wird das Betriebssystem für Cluster-Edgeknoten ebenfalls auf Ubuntu 18.04 aktualisiert. Die vorhandenen Clients müssen mit Ubuntu 18.04 getestet werden. Um das Risiko von Breaking Changes zu verringern, führt HDInsight seit dem 25. Februar 2021 kein Rollout des neuen Images für Abonnements aus, die Edgeknoten verwendet haben. Öffnen Sie ein Supportticket, wenn Sie das neue Image mit Ihren Testabonnements ausprobieren möchten, um Ihr Abonnement zu aktivieren.

## <a name="references"></a>Referenzen
 - [Versionshinweise zu Ubuntu 18.04 LTS](https://wiki.ubuntu.com/BionicBeaver/ReleaseNotes/)





