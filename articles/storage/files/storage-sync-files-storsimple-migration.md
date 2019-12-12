---
title: Migrieren von StorSimple zur Azure-Dateisynchronisierung
author: fauhse
ms.service: storage
ms.topic: conceptual
ms.date: 11/26/2019
ms.author: fauhse
ms.subservice: files
description: StorSimple ist ein Produkt, das das Ende seiner Lebensdauer erreicht hat, und die Azure-Dateisynchronisierung ist die Lösung, zu der migriert werden sollte. Erfahren Sie mehr über das Migrationskonzept, und wenden Sie sich an AzureFiles@microsoft.com, um angepasste Migrationshilfe zu erhalten.
ms.openlocfilehash: edad4d1c6be2c39dbf8150b6ab8979ae3924fb53
ms.sourcegitcommit: 57eb9acf6507d746289efa317a1a5210bd32ca2c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 12/01/2019
ms.locfileid: "74666538"
---
# <a name="storsimple-migration-to-azure-file-sync"></a>StorSimple-Migration zur Azure-Dateisynchronisierung

StorSimple ist ein nicht mehr unterstütztes Microsoft-Produkt. Der erweiterte Support für dieses Produkt und seinen Clouddienst steht noch bis Ende 2022 bereit.
Es ist wichtig, jetzt mit der Planung einer Migration von StorSimple zu beginnen.

Der standardmäßige und strategisch langfristige Azure-Dienst, zu dem StorSimple-Geräte migriert werden können, ist die Azure-Dateisynchronisierung. Es handelt sich hierbei um einen allgemein verfügbaren Azure-Dienst mit einem größeren Funktionsumfang als StorSimple.

## <a name="full-cloud-side-migration-with-limited-downtime"></a>Vollständige cloudseitige Migration mit begrenzter Ausfallzeit
In diesem Artikel wird das Konzept zum Einleiten einer Migration beschrieben.
Es ist unbedingt zu beachten, dass Kunden, für die eine Migration von StorSimple zur Azure-Dateisynchronisierung erforderlich ist, dies nicht alleine durchführen müssen.

> [!IMPORTANT]
> Microsoft ist bestrebt, Kunden bei der Migration zu unterstützen. Senden Sie eine E-Mail an AzureFiles@microsoft.com, um einen angepassten Migrationsplan sowie Unterstützung während der Migration zu erhalten.

## <a name="migration-approach"></a>Migrationsansatz
Die Migration zur Azure-Dateisynchronisierung beginnt cloudseitig mit minimalen Auswirkungen auf lokale Systeme und begrenzter Ausfallzeit.
Das folgende Konzept betrifft Geräte der StorSimple 8000-Serie.
Wenn für Sie eine Migration von der StorSimple 7000-Serie erforderlich ist, umfasst der erste Schritt ein kostenloses Upgrade auf ein entsprechendes Leihgerät der 8000-Serie von Microsoft.
Wenden Sie sich an AzureFiles@microsoft.com, und wir helfen Ihnen dabei, eine geeignete Anzahl von Leihgeräten zu organisieren.

### <a name="general-approach"></a>Allgemeiner Ansatz
![Alt](media/storage-sync-files-storsimple-migration/storsimple-docs-overview-concept.png "Veranschaulichung der cloudseitigen Migration über ein temporäres virtuelles Gerät und Windows Server zu einem neuen lokalen Windows Server, der das lokale StorSimple-Gerät ersetzt")

1. Erstellen Sie einen Volumeklon Ihres lokalen StorSimple-Geräts, und stellen Sie ihn auf einem temporären virtuellen StorSimple-Gerät bereit.
2. Verbinden Sie das virtuelle Gerät über iSCSI mit einer temporären Azure-VM.
3. Installieren Sie die Azure-Dateisynchronisierung auf der temporären Windows Server-VM. Es muss außerdem ein spezifischer Registrierungsschlüssel für diese Migration festgelegt werden, bevor die Synchronisierung auf dem Server konfiguriert wird.
    * Stellen Sie je nach Anzahl der Freigaben auf Ihrem StorSimple-Volume ebenso viele Azure-Dateifreigaben bereit. (Es wird empfohlen, eine Azure-Dateifreigabe pro Speicherkonto bereitzustellen.)
    * Konfigurieren Sie die Synchronisierung zwischen der einzelnen Freigabe auf der Windows Server-Cloud-VM und einer Azure-Dateifreigabe. (1:1-Zuordnung)
    * Fügen Sie optional einen lokalen Server als lokalen Leistungscache mit aktiviertem Cloudtiering hinzu. Dieser Schritt ist erforderlich, wenn Sie Ihr lokales StorSimple-Gerät durch einen funktionsreicheren, lokalen Cache ersetzen möchten, der von Windows Server und Cloudtiering der Azure-Dateisynchronisierung unterstützt wird. Beim lokalen Windows Server kann es sich um einen physischen Computer oder Cluster oder einen virtuellen Computer handeln. Hierfür muss kein Speicher entsprechend der Datasetgröße bereitgestellt werden. Es ist lediglich ausreichender Speicher zum lokalen Zwischenspeichern der am häufigsten verwendeten Dateien erforderlich.

## <a name="minimizing-downtime"></a>Verringern der Ausfallzeit
Nach Schritt 3 oben wird das lokale StorSimple-Gerät von Benutzern und Anwendungen weiterhin aktiv verwendet. Darum ist der Satz von Dateien, der aus dem anfänglichen Volumeklon synchronisiert wurde, bei Abschluss der Synchronisierung etwas veraltet.
Die Methode zum Minimieren der Ausfallzeit besteht darin, die Synchronisierung vom Volumeklon zu wiederholen, damit die Synchronisierung bei jeder Wiederholung schneller abgeschlossen wird, was dadurch möglich ist, dass die Änderungen zwischen Volumeklonen immer geringer werden.
Sie können diesen Vorgang wiederholen, bis die Synchronisierung von einem Volumeklon in einem Zeitraum abgeschlossen ist, den Sie als Ausfallzeit für akzeptabel halten.
Sobald das der Fall ist, verhindern Sie Änderungen an Ihrem StorSimple-Gerät durch Blockieren von Benutzern und Anwendungen. Die Ausfallzeit beginnt.
Nehmen Sie einen anderen Volumeklon, und führen Sie eine Synchronisierung mit den verbundenen Servern durch.
Richten Sie den Zugriff Ihrer Benutzer und Anwendungen auf den neuen, mit Azure-Dateisynchronisierung gesicherten Windows Server ein.
Sie können einen DFS-Namespace bereitstellen oder anpassen, um den Übergang vom alten StorSimple-Gerät zum neuen Windows Server für Anwendungen und Benutzer transparent zu machen.
Die Migration ist abgeschlossen.

## <a name="migration-goal"></a>Migrationsziel
Nach Abschluss der Migration kann die Bereitstellung des temporären virtuellen StorSimple-Geräts und der Azure-VM aufgehoben werden.

Darüber hinaus kann die Bereitstellung des lokalen StorSimple-Geräts aufgehoben werden, da die Benutzer und Anwendungen stattdessen bereits auf Windows Server zugreifen.
Was bleibt, ist in der folgenden Abbildung dargestellt. Eine Standardbereitstellung der Azure-Dateisynchronisierung umfasst eine Reihe von Azure-Dateifreigaben und Windows Server-Computern, die über die Azure-Dateisynchronisierung damit verbunden sind. Beachten Sie, dass ein einzelner Server verschiedene lokale Ordner mit verschiedenen Dateifreigaben gleichzeitig verbinden kann.
Außerdem kann eine einzige Azure-Dateifreigabe mit vielen verschiedenen Servern synchronisiert werden, falls Sie in Zweigstellen zwischengespeicherte Daten benötigen. Überprüfen Sie auch, ob Sie Ihre Richtlinien für das Cloudtiering für eine effizientere Nutzung Ihres lokalen Speicherplatzes optimieren können.

![Alt](media/storage-sync-files-storsimple-migration/storsimple-docs-goal.PNG "Abbildung, die das Ziel nach Abschluss der Migration zeigt. Sie stellt eine Reihe von Dateifreigaben dar, die mit einem lokalen Windows Server synchronisiert werden, wobei Benutzer und Anwendungen auf Dateien in der Cloud oder auf Windows Server zugreifen.")

## <a name="next-steps"></a>Nächste Schritte
Machen Sie sich mit Azure Files und der Azure-Dateisynchronisierung vertraut. Für eine erfolgreiche Migration ist es wichtig, die Terminologie und das Bereitstellungsmuster der Azure-Dateisynchronisierung zu verstehen. Zu jedem Schritt in diesem Übersichtsartikel stehen ausführlichere Informationen bereit. Wenden Sie sich an Microsoft, um während der Planung und Ausführung der Migration angepasste Hilfe zu erhalten.

> [!IMPORTANT]
> Microsoft ist bestrebt, Kunden bei der Migration zu unterstützen. Senden Sie eine E-Mail an AzureFiles@microsoft.com, um einen angepassten Migrationsplan sowie Unterstützung während der Migration zu erhalten.

## <a name="additional-resources"></a>Zusätzliche Ressourcen
Für die Azure-Dateisynchronisierung als Zieldienst stehen zwei grundlegende Dokumente bereit, die Sie lesen sollten, wenn Sie noch nicht mit der Azure-Dateisynchronisierung vertraut sind.
* [Azure-Dateisynchronisierung – Übersicht](storage-sync-files-planning.md)
* [Azure-Dateisynchronisierung – Bereitstellungsleitfaden](storage-sync-files-deployment-guide.md)

Azure Files ist ein Speicherdienst in Azure, der Dateifreigaben als Dienst anbietet. Es ist nicht erforderlich, eine VM oder zugeordneten VM-Speicher zu bezahlen oder zu verwalten.
* [Azure Files – Übersicht](storage-files-introduction.md)
* [Azure Files – Bereitstellen einer Azure-Dateifreigabe](storage-how-to-create-file-share.md)