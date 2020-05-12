---
title: Einführung in Azure Red Hat OpenShift
description: Lernen Sie die Features und Vorteile von Microsoft Azure Red Hat OpenShift zur Bereitstellung und Verwaltung containerbasierter Anwendungen kennen.
author: jimzim
ms.author: jzim
ms.service: container-service
ms.topic: overview
ms.date: 04/24/2020
ms.custom: mvc
ms.openlocfilehash: a3bdc3673474b778aa7c1003e48e215bac6d05bf
ms.sourcegitcommit: 1895459d1c8a592f03326fcb037007b86e2fd22f
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/01/2020
ms.locfileid: "82628519"
---
# <a name="azure-red-hat-openshift"></a>Azure Red Hat OpenShift

Der Microsoft *Azure Red Hat OpenShift*-Dienst ermöglicht Ihnen das Bereitstellen vollständig verwalteter [OpenShift](https://www.openshift.com/)-Cluster.

Azure Red Hat OpenShift stellt eine Erweiterung von [Kubernetes](https://kubernetes.io/) dar. Für die Ausführung von Containern in der Produktion mit Kubernetes sind zusätzliche Tools und Ressourcen erforderlich. Dabei muss häufig mit Imageregistrierungen, Speicherverwaltungslösungen, Netzwerklösungen sowie Protokollierungs- und Überwachungstools jongliert werden, die alle mit Versionen versehen und zusammen getestet werden müssen. Für das Erstellen containerbasierter Anwendungen ist sogar noch mehr Integrationsarbeit mit Middleware, Frameworks, Datenbanken und CI/CD-Tools zu leisten. Azure Red Hat OpenShift kombiniert all dies in einer einzelnen Plattform und verhilft so IT-Teams zu einem komfortablen Betrieb, während Anwendungsteams erhalten, was sie für die Ausführung benötigen.

Azure Red Hat OpenShift wird von Red Hat und Microsoft gemeinsam konzipiert, verarbeitet und supported, um eine integrierte Supporterfahrung zu bieten. Es müssen keine virtuellen Computer betrieben werden, und es sind keine Patches erforderlich. Master-, Infrastruktur- und Anwendungsknoten werden in Ihrem Auftrag von Red Hat und Microsoft mit Patches und Updates versehen und überwacht. Ihre Azure Red Hat OpenShift-Cluster werden in Ihrem Azure-Abonnement bereitgestellt und sind in Ihrer Azure-Rechnung enthalten.

Sie können für Registrierung, Netzwerk, Speicher und CI/CD eigene Lösungen wählen oder die integrierten Lösungen für automatische Quellcodeverwaltung, Container- und Anwendungsbuilds, Bereitstellungen, Skalierung, Health Management und mehr verwenden. Azure Red Hat OpenShift bietet über Azure Active Directory eine integrierte Anmeldeoberfläche.

Arbeiten Sie zum Einstieg das Tutorial [Erstellen eines Azure Red Hat OpenShift-Clusters](tutorial-create-cluster.md) durch.

## <a name="access-security-and-monitoring"></a>Zugriff, Sicherheit und Überwachung

Für verbesserte Sicherheit und Verwaltung ermöglicht Azure Red Hat OpenShift Ihnen die Integration in Azure Active Directory (Azure AD) und die Verwendung rollenbasierter (RBAC) Kubernetes-Zugriffssteuerungen. Sie können auch die Integrität Ihrer Cluster und Ressourcen überwachen.

## <a name="cluster-and-node"></a>Cluster- und Knoten

Azure Red Hat OpenShift-Knoten werden auf virtuellen Azure-Computern ausgeführt. Sie können eine Verbindung des Speichers mit Knoten und Pods herstellen und Clusterkomponenten aktualisieren.

## <a name="next-steps"></a>Nächste Schritte

Lernen Sie die Voraussetzungen für Azure Red Hat OpenShift kennen:

> [!div class="nextstepaction"]
> [Einrichten Ihrer Entwicklungsumgebung](tutorial-create-cluster.md)
