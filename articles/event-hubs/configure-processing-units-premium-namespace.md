---
title: Konfigurieren von Verarbeitungseinheiten für einen Premium-Azure Event Hubs-Namespace
description: Enthält Anweisungen zum Konfigurieren von Verarbeitungseinheiten für einen Event Hubs-Namespace der Dienstebene „Premium“.
ms.topic: article
ms.date: 05/26/2021
ms.openlocfilehash: 443832c9fcd4ee8ce8e314a80251f2575aed003d
ms.sourcegitcommit: 6323442dbe8effb3cbfc76ffdd6db417eab0cef7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/28/2021
ms.locfileid: "110631602"
---
# <a name="configure-processing-units-for-a-premium-tier-azure-event-hubs-namespace"></a>Konfigurieren von Verarbeitungseinheiten für einen Azure Event Hubs-Namespace der Dienstebene „Premium“
In diesem Artikel finden Sie Anweisungen zum Konfigurieren von Verarbeitungseinheiten für einen Azure Event Hubs-Namespace der Dienstebene „Premium“. Weitere Informationen zur Dienstebene **Premium** finden Sie unter [Event Hubs Premium](event-hubs-premium-overview.md).

## <a name="configure-processing-units-when-creating-a-namespace"></a>Konfigurieren von Verarbeitungseinheiten beim Erstellen eines Namespace
Wenn Sie im Azure-Portal einen Namespace auf der Dienstebene **Premium** erstellen, wird das Feature für die automatische Vergrößerung automatisch aktiviert. Sie können die Anzahl der Verarbeitungseinheiten (Processing Units, PUs) für Ihren Namespace (1, 2, 4, 8 oder 16) konfigurieren, die Sie nach dem Erstellen des Namespace aktualisieren können. 

:::image type="content" source="./media/configure-processing-units-premium-namespace/event-hubs-auto-inflate-prem.png" alt-text="Screenshot des Aktivierens beim Erstellen eines Premium-Namespace":::

## <a name="configure-processing-units-for-an-existing-namespace"></a>Konfigurieren von Verarbeitungseinheiten für einen vorhandenen Namespace
Führen Sie die folgenden Schritte aus, um die Anzahl der Verarbeitungseinheiten für einen vorhandenen Premium-Namespace zu aktualisieren: 

1. Wählen Sie auf der Seite **Event Hubs-Namespace** für Ihren Namespace im linken Menü unter **Einstellungen** die Option **Skalieren** aus. 
1. Aktualisieren Sie den Wert für **Verarbeitungseinheiten**, und wählen Sie **Speichern** aus. 

    :::image type="content" source="./media/configure-processing-units-premium-namespace/scale-settings-prem.png" alt-text="Screenshot des Aktivierens der automatischen Vergrößerung für einen vorhandenen Premium-Namespace":::

## <a name="next-steps"></a>Nächste Schritte
Weitere Informationen zu Verarbeitungseinheiten und zur Event Hubs-Dienstebene „Premium“ finden Sie in den folgenden Artikeln.

- [Event Hubs Premium](event-hubs-premium-overview.md)
- [Event Hubs – Skalierbarkeit](event-hubs-scalability.md)
