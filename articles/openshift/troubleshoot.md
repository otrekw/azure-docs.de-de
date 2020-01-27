---
title: Problembehandlung für Azure Red Hat OpenShift
description: Behandlung und Lösung häufiger Probleme mit Azure Red Hat OpenShift
author: jimzim
ms.author: jzim
ms.service: container-service
ms.topic: troubleshooting
ms.date: 05/08/2019
ms.openlocfilehash: ee032cdf4a3f72b2cd2e7da0658effe75b6fb1fa
ms.sourcegitcommit: 5397b08426da7f05d8aa2e5f465b71b97a75550b
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/19/2020
ms.locfileid: "76274928"
---
# <a name="troubleshooting-for-azure-red-hat-openshift"></a>Problembehandlung für Azure Red Hat OpenShift

Dieser Artikel enthält Informationen zu einigen häufigen Problemen, die beim Erstellen oder Verwalten von Microsoft Azure Red Hat OpenShift-Clustern auftreten können.

## <a name="retrying-the-creation-of-a-failed-cluster"></a>Durchführen eines erneuten Erstellungsversuchs für einen Cluster nach einem Fehler

Wenn die Erstellung eines Azure Red Hat OpenShift-Clusters mit dem CLI-Befehl `az` nicht erfolgreich ist, tritt auch bei einem erneuten Erstellungsversuch ein Fehler auf.
Verwenden Sie `az openshift delete`, um den fehlerhaften Cluster zu löschen, und erstellen Sie anschließend einen neuen Cluster.

## <a name="hidden-azure-red-hat-openshift-cluster-resource-group"></a>Ausgeblendete Azure Red Hat OpenShift-Clusterressourcengruppe

Derzeit ist die Ressource `Microsoft.ContainerService/openShiftManagedClusters`, die von der Azure CLI automatisch erstellt wird (Befehl `az openshift create`) im Azure-Portal ausgeblendet. Aktivieren Sie in der Ansicht **Ressourcengruppe** die Option **Ausgeblendete Typen anzeigen**, um die Ressourcengruppe anzuzeigen.

![Screenshot: Kontrollkästchen „Ausgeblendete Typen anzeigen“ im Portal](./media/aro-portal-hidden-type.png)

## <a name="creating-a-cluster-results-in-error-that-no-registered-resource-provider-found"></a>Erstellung eines Clusters führt zu einem Fehler mit dem Hinweis, dass kein registrierter Ressourcenanbieter gefunden wurde

Wenn die Erstellung eines Clusters zu einem Fehler der Art `No registered resource provider found for location '<location>' and API version '2019-04-30' for type 'openShiftManagedClusters'. The supported api-versions are '2018-09-30-preview` führt, haben Sie die Vorschauversion genutzt. Sie müssen nun [reservierte Azure-VM-Instanzen erwerben](https://aka.ms/openshift/buy), um das allgemein verfügbare Produkt zu verwenden. Eine Reservierung senkt Ihre Ausgaben, da Sie im Voraus für vollständig verwaltete Azure-Datendienste zahlen. Unter [*Was sind Azure-Reservierungen*](https://docs.microsoft.com/azure/billing/billing-save-compute-costs-reservations) erhalten Sie weitere Informationen zu Reservierungen und erfahren, wie Sie damit Geld sparen können.

## <a name="next-steps"></a>Nächste Schritte

- Im [Hilfecenter zu Red Hat OpenShift](https://help.openshift.com/) finden Sie weitere Informationen zur Problembehandlung für OpenShift.

- Hier erhalten Sie Antworten auf [häufig gestellte Fragen zu Azure Red Hat OpenShift](openshift-faq.md).
