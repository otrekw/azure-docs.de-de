---
title: Übersicht über die Bereitstellung – Avere vFXT für Azure
description: In dieser Übersicht erfahren Sie, wie Sie einen Avere vFXT für Azure-Cluster bereitstellen. Verwandte Artikel enthalten spezifische Anweisungen zur Bereitstellung.
author: ekpgh
ms.service: avere-vfxt
ms.topic: conceptual
ms.date: 01/13/2020
ms.author: rohogue
ms.openlocfilehash: 4c63fdf2164dd4dce12912669eec29c79755cc2a
ms.sourcegitcommit: f28ebb95ae9aaaff3f87d8388a09b41e0b3445b5
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/29/2021
ms.locfileid: "88271225"
---
<!-- filename is linked to in the marketplace template, make sure it gets a redirect if we rename it -->

# <a name="avere-vfxt-for-azure---deployment-overview"></a>Avere vFXT für Azure – Übersicht über die Bereitstellung

Dieser Artikel bietet eine Übersicht über die Schritte, die erforderlich sind, um einen Avere vFXT für Azure-Cluster in Betrieb zu nehmen.

Vor und nach dem Erstellen des vFXT-Clusters aus Azure Marketplace sind mehrere Aufgaben erforderlich. Ein sicheres Gespür für den gesamten Prozess hilft Ihnen, den erforderlichen Aufwand zu minimieren.

## <a name="deployment-steps"></a>Bereitstellungsschritte

Nach der [Planung Ihres Systems](avere-vfxt-deploy-plan.md) können Sie mit der Erstellung des Avere vFXT-Clusters beginnen.

Eine Azure Resource Manager-Vorlage in Azure Marketplace erfasst die erforderlichen Informationen und stellt automatisch den gesamten Cluster bereit.

Nachdem der vFXT-Cluster aktiv ist und ausgeführt wird, müssen Sie noch einige Konfigurationsschritte ausführen, bevor Sie ihn verwenden können. Wenn Sie einen neuen Blobspeichercontainer erstellt haben, sollten Sie die Daten in den Container verschieben. Wenn Sie ein NAS-Speichersystem verwenden, müssen Sie es nach der Erstellung des Clusters hinzufügen. Sie möchten Clients mit dem Cluster verbinden.

Hier folgt eine Übersicht über alle Schritte.

1. Konfigurieren der Voraussetzungen

   Bevor Sie einen virtuellen Computer erstellen, müssen Sie ein neues Abonnement für das Avere vFXT-Projekt erstellen, den Abonnementbesitz konfigurieren, Kontingente prüfen und bei Bedarf eine Erhöhung anfordern sowie Bedingungen für die Nutzung der Avere vFXT-Software akzeptieren. Ausführliche Anweisungen finden Sie unter [Vorbereiten der Avere vFXT-Erstellung](avere-vfxt-prereqs.md).

1. Erstellen des Avere vFXT-Clusters

   Verwenden Sie Azure Marketplace, um den Avere vFXT-Cluster für Azure zu erstellen. Eine Vorlage erfasst die erforderlichen Informationen und führt Skripts aus, um das endgültige Produkt zu erstellen.

   Die Erstellung des Clusters umfasst die folgenden Schritte, die alle von der Marketplace-Vorlage ausgeführt werden:

   * Erstellen einer neuen Netzwerkinfrastruktur und von Ressourcengruppen, wenn erforderlich
   * Erstellen eines Clustercontrollers

     Der Clustercontroller ist eine einfache VM, die sich im gleichen virtuellen Netzwerk wie der Avere vFXT-Cluster befindet und über die für die Erstellung und Verwaltung des Clusters erforderliche benutzerdefinierte Software verfügt. Der Controller erstellt die vFXT-Knoten und bildet den Cluster, und er stellt auch eine Befehlszeilenschnittstelle zur Verfügung, um den Cluster während seiner Lebensdauer zu verwalten.

     Wenn Sie während der Bereitstellung ein neues virtuelles Netzwerk oder Subnetz erstellen, hat Ihr Controller eine öffentliche IP-Adresse. Dies bedeutet, dass der Controller als Jump Host für das Herstellen der Verbindung mit dem Cluster von außerhalb des virtuellen Netzwerks dienen kann.

   * Erstellen der Clusterknoten-VMs

   * Erstellen des Clusters aus den einzelnen Knoten

   * Optionales Erstellen eines neuen Blobcontainers und Konfigurieren des Containers als Back-End-Speicher für den Cluster

   Die Clustererstellung wird unter [Bereitstellen des vFXT-Clusters](avere-vfxt-deploy.md) ausführlich beschrieben.

1. Konfigurieren des Clusters

   Stellen Sie eine Verbindung zur Avere vFXT-Konfigurationsschnittstelle (Avere-Systemsteuerung) her, um die Einstellungen des Clusters anzupassen. Melden Sie sich für die Supportüberwachung an, und fügen Sie Ihr Speichersystem hinzu, wenn Sie Hardwarespeicher oder zusätzliche Blobcontainer verwenden.

   * [Zugreifen auf den vFXT-Cluster](avere-vfxt-cluster-gui.md)
   * [Aktivieren des Supports](avere-vfxt-enable-support.md)
   * [Konfigurieren des Speichers](avere-vfxt-add-storage.md) (falls erforderlich)

1. Einbinden von Clients

   Befolgen Sie die Anweisungen unter [Einbinden des Avere vFXT-Clusters](avere-vfxt-mount-clients.md), um mehr über den Lastausgleich und darüber zu erfahren, wie Clientcomputer den Cluster einbinden sollten.

1. Hinzufügen von Daten (falls erforderlich)

   Da es sich bei Avere vFXT um einen skalierbaren Cache mit mehreren Clients handelt, ist der beste Weg, Daten in einen neuen Back-End-Speichercontainer zu verschieben, die Strategie der Verwendung mehrerer Clients und mehrerer Threads.

   Wenn Sie Arbeitssatzdaten in einen neuen BLOB-Container oder ein anderes Back-End-Speichersystem verschieben müssen, befolgen Sie die Anweisungen unter [Verschieben von Daten in den vFXT-Cluster](avere-vfxt-data-ingest.md).

## <a name="next-steps"></a>Nächste Schritte

Fahren Sie mit dem [Vorbereiten der Avere vFXT-Erstellung](avere-vfxt-prereqs.md) fort, um die vorbereitenden Aufgaben abzuschließen.
