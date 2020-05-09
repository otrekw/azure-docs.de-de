---
title: 'Konzepte: Upgrades für private Clouds'
description: Erfahren Sie mehr über die wichtigsten Upgradeprozesse und Features in Azure VMware Solution (AVS).
ms.topic: conceptual
ms.date: 05/04/2020
ms.openlocfilehash: c5c79625ef661f220a0ba88d6d24ab59e7228195
ms.sourcegitcommit: d9cd51c3a7ac46f256db575c1dfe1303b6460d04
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/04/2020
ms.locfileid: "82739786"
---
# <a name="azure-vmware-solution-avs-upgrade-concepts"></a>Upgradekonzepte von Azure VMware Solution (AVS)

Einer der Hauptvorteile von privaten Clouds von Azure VMware Solution (AVS) besteht darin, dass die Plattform für Sie verwaltet wird. Die Plattformverwaltung umfasst automatisierte Upgrades eines von VMware validierten Softwarepakets. Die Upgrades werden regelmäßig durchgeführt, um sicherzustellen, dass Sie immer über die neuesten validierten Versionen der Software verfügen.

## <a name="avs-private-cloud-software-upgrades"></a>Upgrades der AVS-Software für private Clouds

Die private AVS-Cloudplattform umfasst bestimmte Versionen von VMware vSphere-, ESXi-, vSAN- und NSX-T-Software. Das Softwarepaket für die private Cloud wird für die Verwendung in neuen privaten Cloudinstallationen und für Upgrades vorhandener privater Clouds validiert.

Der Upgradeprozess für die Lebenszyklusverwaltung erfordert keine Ausfallzeiten für Ihre privaten Clouds. Beim Upgradeprozess wird sichergestellt, dass Sie automatisch die neueste Version der validierten AVS-Software für private Clouds verwenden. Upgrades werden in regelmäßigen Intervallen angewendet, sodass private Clouds nie mehr als eine Version hinter der neuesten Version des validierten Softwarepakets liegen. Sie werden über geplante Upgrades für Ihre private Cloud benachrichtigt. Sie können das Upgrade aufschieben, wenn Ihre private Cloud innerhalb einer Version des neuesten Release liegt.

Wichtige Patches und Updates werden angewendet, nachdem sie validiert wurden. Sie werden im Voraus über erforderliche wichtige Upgrades benachrichtigt. Diese Richtlinie stellt sicher, dass wichtige Patches und Updates sofort auf Ihre private Cloud angewendet werden.

VMware-Softwareversionen finden Sie im [Konzeptartikel zu privaten Clouds und Clustern](concepts-private-clouds-clusters.md) und in den [häufig gestellten Fragen (FAQ)](faq.md).

## <a name="next-steps"></a>Nächste Schritte

Der nächste Schritt besteht darin, eine [private Cloud zu erstellen](tutorial-create-private-cloud.md).

<!-- LINKS - external -->

<!-- LINKS - internal -->
