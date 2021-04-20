---
title: Vorgehensweise beim Migrieren von - Portal
description: Vorgehensweise beim Migrieren von Cloud Services (erweiterter Support) über das Azure-Portal
ms.topic: how-to
ms.service: cloud-services-extended-support
ms.subservice: classic-to-arm-migration
author: tanmaygore
ms.author: tagore
ms.reviewer: mimckitt
ms.date: 2/08/2021
ms.custom: ''
ms.openlocfilehash: 79889b08baa80dc67b30ae445004e37d9f9fe295
ms.sourcegitcommit: b8995b7dafe6ee4b8c3c2b0c759b874dff74d96f
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/03/2021
ms.locfileid: "106286705"
---
# <a name="migrate-to-cloud-services-extended-support-using-the-azure-portal"></a>Migrieren von Cloud Services (erweiterter Support) über das Azure-Portal

In diesem Artikel erfahren Sie, wie Sie das Azure-Portal für die Migration von [Cloud Services (klassisch)](../cloud-services/cloud-services-choose-me.md) zu [Cloud Services (erweiterter Support)](overview.md)verwenden.

> [!IMPORTANT]
> Die Migration von Cloud Services (klassisch) zu Cloud Services (erweiterter Support) mithilfe des Migrationstools befindet sich derzeit in der öffentlichen Vorschau. Diese Vorschauversion wird ohne Vereinbarung zum Servicelevel bereitgestellt und ist nicht für Produktionsworkloads vorgesehen. Manche Features werden möglicherweise nicht unterstützt oder sind nur eingeschränkt verwendbar. Weitere Informationen finden Sie unter [Zusätzliche Nutzungsbestimmungen für Microsoft Azure-Vorschauen](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).

## <a name="before-you-begin"></a>Vorbereitung

**Stellen Sie sicher, dass Sie der Administrator für das Abonnement sind.**

Um diese Migration auszuführen, müssen Sie im [Azure-Portal](https://portal.azure.com) als Co-Administrator für das Abonnement hinzugefügt werden.

1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com) an.
2. Wählen Sie im Menü **Hub** die Option **Abonnement** aus. Wählen Sie **Alle Dienste** aus, falls die Option nicht angezeigt wird.
3. Suchen Sie den entsprechenden Abonnementeintrag, und prüfen Sie dann das Feld **MEINE ROLLE**. Bei einem Co-Administrator muss der Wert *Kontoadministrator* lauten.

Sollten Sie keinen Co-Administrator hinzufügen können, lassen Sie sich von einem Dienstadministrator oder[Co-Administrator](../role-based-access-control/classic-administrators.md) für das Abonnement hinzufügen.

**Registrieren Sie sich als Migrationsressourcenanbieter**

1. Registrieren Sie sich mit dem [Azure-Portal](https://docs.microsoft.com/azure/azure-resource-manager/management/resource-providers-and-types#register-resource-provider-1) beim Migrationsressourcenanbieter `Microsoft.ClassicInfrastructureMigrate` und der Previewfunktion `Cloud Services` unter dem Microsoft.Compute-Namespace.  
1. Warten Sie fünf Minuten, bis die Registrierung abgeschlossen ist, und überprüfen Sie dann den Genehmigungsstatus. 

## <a name="migrate-your-cloud-service-resources"></a>Migrieren Ihrer Clouddienst-Ressourcen

1. Wechseln Sie zum [Blatt "Cloud Services (klassisch)](https://ms.portal.azure.com/#blade/HubsExtension/BrowseResourceBlade/resourceType/microsoft.classicCompute%2FdomainNames)" des Portals. 
2. Wählen Sie dann den Cloud Service aus, den Sie migrieren möchten.
3. Wählen Sie das Blatt **zu ARM migrieren** aus.

    > [!NOTE]
    > Wenn ein Cloud Service (klassisch) in einem virtuellen Netzwerk (klassisch) migriert wird, wird eine Bannermeldung angezeigt, in der Sie aufgefordert werden, das Blatt für das virtuelle Netzwerk (klassisch) zu verschieben.
    > Sie gelangen zum Blatt „virtuelles Netzwerk" (klassisch), um sowohl die Migration der Bereitstellungen des virtuellen Netzwerks (klassisch) und der Cloud Services (klassisch) darin abzuschließen.
    > :::image type="content" source="media/in-place-migration-portal-2.png" alt-text="Das Bild zeigt das Verschieben eines klassischen virtuellen Netzwerks im Azure-Portal.":::
 

4. Überprüfen Sie die Migration. 

    Wenn die Überprüfung erfolgreich ist, werden alle Bereitstellungen unterstützt und können vorbereitet werden.  

    :::image type="content" source="media/in-place-migration-portal-1.png" alt-text="Das Bild zeigt das Blade &quot;Auf ARM migrieren&quot; im Azure-Portal.":::

    Wenn bei der Überprüfung ein Fehler auftritt, wird eine Liste der nicht unterstützten Szenarien angezeigt, die vor dem Fortsetzen der Migration korrigiert werden müssen. 

    :::image type="content" source="media/in-place-migration-portal-3.png" alt-text="Abbildung: Bei Überprüfung gemeldeter Fehler im Azure-Portal.":::

5. Bereiten Sie die Migration vor.

    Wenn die Vorbereitung erfolgreich ist, ist die Migration bereit für den Commit.
    
    :::image type="content" source="media/in-place-migration-portal-4.png" alt-text="Abbildung: Erfolgreiche Überprüfung im Azure-Portal.":::

    Wenn die Vorbereitung fehlschlägt, überprüfen Sie den Fehler, beheben Sie alle Probleme, und wiederholen Sie die Vorbereitung. 

    :::image type="content" source="media/in-place-migration-portal-5.png" alt-text="Im Bild wird ein Fehler bei der Überprüfung angezeigt.":::

      Nach der Vorbereitung stehen alle Cloud Services in einem virtuellen Netzwerk für Lesevorgänge mit den Azure-Portal Blatt Cloud Services (klassisch) und Cloud Services (erweiterter Support) zur Verfügung. Die Bereitstellung des Cloud Service (erweiterter Support) kann jetzt getestet werden, um eine ordnungsgemäße Funktionsweise sicherzustellen, bevor die Migration abgeschlossen wird. 
 
    :::image type="content" source="media/in-place-migration-portal-6.png" alt-text="Abbildung: Test der APIs auf dem Blatt „Portal“.":::

6.  **(Optional)** Abbrechen der Migration. 
    
    Wenn Sie die Migration einstellen möchten, verwenden Sie die Schaltfläche **Abbrechen**, um ein Rollback der vorherigen Schritte auszuführen. Die Cloud Services (klassisch) werden dann für alle CRUD-Vorgänge entsperrt.

    :::image type="content" source="media/in-place-migration-portal-7.png" alt-text="Abbildung: Erfolgreiche Überprüfung.":::

    Wenn Abbruch fehlschlägt, wählen Sie **Abbruch Abbrechen** aus. Beim erneuten Versuch sollte das Problem behoben sein. Kontaktieren Sie andernfalls den Support. 
 
    :::image type="content" source="media/in-place-migration-portal-8.png" alt-text="Abbildung: Fehler bei der Überprüfung.":::

7.  Führen Sie das Commit für die Migration aus.

    >[!IMPORTANT]
    > Wenn Sie ein Commit für die Migration durchführen, gibt es keine Möglichkeit, ein Rollback auszuführen. 
    
    Geben Sie "Ja" ein, um die Migration auszuführen und den Vorgang zu bestätigen. Die Migration ist nun abgeschlossen. Die migrierten Cloud Services (erweiterter Support) werden für alle Vorgänge entsperrt. 

## <a name="next-steps"></a>Nächste Schritte
Lesen Sie den Abschnitt [Änderungen nach der Migration](in-place-migration-overview.md#post-migration-changes), um Änderungen für die Bereitstellungsdateien, die Automatisierung und andere Attribute der neuen Cloud Services-Bereitstellung (erweiterter Support) anzuzeigen. 
