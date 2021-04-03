---
title: Verschieben eines dedizierten Azure Event Hubs-Clusters in eine andere Region | Microsoft-Dokumentation
description: In diesem Artikel erfahren Sie, wie Sie einen dedizierten Azure Event Hubs-Cluster aus der aktuellen Region in eine andere verschieben.
ms.topic: how-to
ms.date: 09/01/2020
ms.openlocfilehash: 94be44ee8f9442a3a65e899d7a58524b2570f194
ms.sourcegitcommit: f28ebb95ae9aaaff3f87d8388a09b41e0b3445b5
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/29/2021
ms.locfileid: "89380392"
---
# <a name="move-an-azure-event-hubs-dedicated-cluster-to-another-region"></a>Verschieben eines dedizierten Azure Event Hubs-Clusters in eine andere Region
In diesem Artikel erfahren Sie, wie Sie eine Azure Resource Manager-Vorlage für einen vorhandenen dedizierten Event Hubs-Cluster exportieren und anschließend mithilfe der Vorlage einen Cluster mit denselben Konfigurationseinstellungen in einer anderen Region erstellen können. 

Wenn die Azure-Ressourcengruppe, die den Event Hubs-Cluster enthält, andere Ressourcen wie Namespaces und Event Hubs aufweist, sollten Sie die Vorlage auf Ressourcengruppenebene exportieren, damit alle zugehörigen Ressourcen in einem Schritt in die neue Region verschoben werden können. Mit den in diesem Artikel beschriebenen Schritten können Sie einen **Event Hubs-Cluster** in die Vorlage exportieren. Die Schritte zum Exportieren einer **Ressourcengruppe** in die Vorlage sind ähnlich. 

## <a name="prerequisites"></a>Voraussetzungen
Stellen Sie sicher, dass der dedizierte Cluster in der Zielregion erstellt werden kann. Dies lässt sich am einfachsten herausfinden, indem Sie [einen dedizierten Event Hubs-Cluster](event-hubs-dedicated-cluster-create-portal.md) im Azure-Portal erstellen. Es wird Ihnen eine Liste der Regionen angezeigt, die zum jeweiligen Zeitpunkt für die Erstellung des Clusters unterstützt werden. 

## <a name="prepare"></a>Vorbereiten
Exportieren Sie zunächst eine Resource Manager-Vorlage. Diese Vorlage enthält Einstellungen, die Ihren dedizierten Event Hubs-Cluster beschreiben.

1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com) an.
2. Wählen Sie **Alle Ressourcen** und dann den dedizierten Event Hubs-Cluster aus.
3. Wählen Sie **Einstellungen** > **Vorlage exportieren** aus.
4. Wählen Sie **Herunterladen** auf der Seite **Vorlage exportieren** aus.

    :::image type="content" source="./media/move-cluster-across-regions/download-template.png" alt-text="Herunterladen einer Resource Manager-Vorlage" lightbox="./media/move-cluster-across-regions/download-template.png":::
5. Suchen Sie die aus dem Portal heruntergeladene ZIP-Datei, und entpacken Sie sie in einen Ordner Ihrer Wahl.

   Diese ZIP-Datei enthält die JSON-Dateien, in denen die Vorlage und Skripts zum Bereitstellen der Vorlage enthalten sind.


## <a name="move"></a>Move

Stellen Sie die Vorlage bereit, um einen dedizierten Event Hubs-Cluster in der Zielregion zu erstellen. 


1. Klicken Sie im Azure-Portal auf **Ressource erstellen**.
2. Geben Sie in **Marketplace durchsuchen** die Zeichenfolge **Vorlagenbereitstellung** ein, und wählen Sie **Vorlagenbereitstellung (mit benutzerdefinierten Vorlagen bereitstellen)** aus.
5. Wählen Sie **Eigene Vorlage im Editor erstellen**.
6. Wählen Sie **Datei laden** aus, und befolgen Sie dann die Anweisungen zum Laden der Datei **template.json**, die Sie im vorherigen Abschnitt heruntergeladen haben.
1. Aktualisieren Sie den Wert der `location`-Eigenschaft, sodass sie auf die neue Region verweist. Informationen zum Abrufen von Standortcodes finden Sie unter [Azure-Standorte](https://azure.microsoft.com/global-infrastructure/locations/). Der Code für eine Region ist der Name der Region ohne Leerzeichen, z. B. ist `West US` gleich `westus`.
1. Wählen Sie **Speichern** aus, um die Vorlage zu speichern. 
1. Gehen Sie auf der Seite **Benutzerdefinierte Bereitstellung** wie folgt vor: 
    1. Wählen Sie ein Azure-**Abonnement** aus. 
    2. Wählen Sie eine vorhandene **Ressourcengruppe** aus, oder erstellen Sie eine. 
    3. Wählen Sie den **Zielspeicherort** oder die Zielregion aus. Wenn Sie eine vorhandene Ressourcengruppe ausgewählt haben, ist diese Einstellung schreibgeschützt. 
    4. Führen Sie im Abschnitt **EINSTELLUNGEN** die folgenden Schritte aus:    
        1. Geben Sie den neuen **Clusternamen** ein. 

            :::image type="content" source="./media/move-cluster-across-regions/deploy-template.png" alt-text="Bereitstellen der Resource Manager-Vorlage":::
    5. Wählen Sie am unteren Rand der Seite die Option **Bewerten + erstellen** aus. 
    1. Überprüfen Sie die Einstellungen auf der Seite **Überprüfen und erstellen**, und wählen Sie dann **Erstellen** aus.  

## <a name="discard-or-clean-up"></a>Verwerfen oder Bereinigen
Wenn Sie nach der Bereitstellung von vorne beginnen möchten, können Sie den **dedizierten Event Hubs-Zielcluster** löschen und die in den Abschnitten [Vorbereiten](#prepare) und [Verschieben](#move) dieses Artikels beschriebenen Schritte wiederholen.

Um die Änderungen zu committen und das Verschieben eines Event Hubs-Clusters abzuschließen, löschen Sie den **Event Hubs-Cluster** in der ursprünglichen Region. 

So löschen Sie einen Event Hubs-Quell- oder -Zielcluster im Azure-Portal

1. Geben Sie im Suchfenster oben im Azure-Portal **Event Hubs-Cluster** ein, und wählen Sie in den Suchergebnissen **Event Hubs-Cluster** aus. Die Event Hubs-Cluster werden in einer Liste angezeigt.
2. Wählen Sie den zu löschenden Cluster aus, und klicken Sie dann auf der Symbolleiste auf **Löschen**. 
3. Bestätigen Sie auf der Seite **Cluster löschen** den Löschvorgang, indem Sie den **Clusternamen** eingeben und auf **Löschen** klicken. 

## <a name="next-steps"></a>Nächste Schritte
In diesem Tutorial haben Sie erfahren, wie Sie einen dedizierten Event Hubs-Cluster aus einer Region in eine andere verschieben. 

Anweisungen zum Verschieben eines Namespace aus einer Region in eine andere finden Sie im Artikel [Regionsübergreifendes Verschieben von Event Hubs Namespaces](move-across-regions.md). 

Weitere Informationen zum Verschieben von Ressourcen zwischen Regionen und zur Notfallwiederherstellung in Azure finden Sie unter:

- [Verschieben von Ressourcen in eine neue Ressourcengruppe oder ein neues Abonnement](../azure-resource-manager/management/move-resource-group-and-subscription.md)
- [Verschieben virtueller Azure-Computer in eine andere Region](../site-recovery/azure-to-azure-tutorial-migrate.md)
