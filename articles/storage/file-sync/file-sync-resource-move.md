---
title: 'Azure-Dateisynchronisierung: Ressourcenverschiebungen und Topologieänderungen'
description: Hier erfahren Sie, wie Sie Synchronisierungsressourcen über Ressourcengruppen, Abonnements und AAD-Mandanten (Azure Active Directory) hinweg verschieben.
author: fauhse
ms.service: storage
ms.topic: how-to
ms.date: 04/13/2021
ms.author: fauhse
ms.subservice: files
ms.openlocfilehash: fb59b8d48d8411ade620304fbd143d86ac5ae919
ms.sourcegitcommit: 4b0e424f5aa8a11daf0eec32456854542a2f5df0
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/20/2021
ms.locfileid: "107796114"
---
# <a name="move-azure-file-sync-resources-to-a-different-resource-group-subscription-or-aad-tenant"></a>Verschieben von Ressourcen für die Azure-Dateisynchronisierung in eine andere Ressourcengruppe, ein anderes Abonnement oder einen anderen AAD-Mandanten

In diesem Artikel wird beschrieben, wie Sie Änderungen an einer Ressourcengruppe, einem Abonnement oder einem AAD-Mandanten (Azure Active Directory) für Ihre Cloudressourcen für die Azure-Dateisynchronisierung und Azure-Speicherkonten vornehmen.

Bei der Planung von Änderungen an den Cloudressourcen für die Azure-Dateisynchronisierung müssen alle Speicherressourcen gleichzeitig betrachtet werden. Folgende Ressourcen existieren:

**Ressourcen für die Azure-Dateisynchronisierung (in hierarchischer Reihenfolge)**

* :::image type="icon" source="media/storage-sync-resource-move/storage-sync-resource-move-storage-sync-service.png" border="false"::: Speichersynchronisierungsdienst
  * :::image type="icon" source="media/storage-sync-resource-move/storage-sync-resource-move-registered-servers.png" border="false"::: Registrierter Server
  * :::image type="icon" source="media/storage-sync-resource-move/storage-sync-resource-move-sync-group.png" border="false"::: Synchronisierungsgruppe
    * :::image type="icon" source="media/storage-sync-resource-move/storage-sync-resource-move-cloud-endpoint.png" border="false"::: Cloudendpunkt
    * :::image type="icon" source="media/storage-sync-resource-move/storage-sync-resource-move-server-endpoint.png" border="false"::: Serverendpunkt

In der Azure-Dateisynchronisierung ist die Speichersynchronisierungsdienst-Ressource die einzige Ressource, die verschoben werden kann. Untergeordnete Ressourcen sind an diese übergeordnete Ressource gebunden und können nicht in einen anderen Speichersynchronisierungsdienst verschoben werden.

**Azure-Speicherressourcen (in hierarchischer Reihenfolge)**

* :::image type="icon" source="media/storage-sync-resource-move/storage-sync-resource-move-storage-account.png" border="false"::: Speicherkonto
    * :::image type="icon" source="media/storage-sync-resource-move/storage-sync-resource-move-file-share.png" border="false"::: Dateifreigabe

In Azure Storage ist das Speicherkonto, die einzige Ressource, die verschoben werden kann. Eine Azure-Dateifreigabe kann, ebenso wie eine untergeordnete Ressource, nicht in ein anderes Speicherkonto verschoben werden.

## <a name="supported-combinations"></a>Unterstützte Kombinationen

Bei der Planung einer Ressourcenverschiebung müssen das Speicherkonto und die Azure-Dateisynchronisierungsressource auf oberster Ebene – der so genannte *Speichersynchronisierungsdienst* – gemeinsam betrachtet werden.

Als Best Practice sollten sich der Speichersynchronisierungsdienst und die Speicherkonten, die zu synchronisierende Dateifreigaben enthalten, immer im selben Abonnement befinden. Die folgenden Kombinationen werden unterstützt:

* Speichersynchronisierungsdienst und Speicherkonten befinden sich in **verschiedenen Ressourcengruppen** (im selben Azure-Mandanten).
* Speichersynchronisierungsdienst und Speicherkonten befinden sich in **verschiedenen Abonnements** (im selben Azure-Mandanten).

> [!IMPORTANT]
> Durch verschiedene Kombinationen von Verschiebungen können Speichersynchronisierungsdienst und Speicherkonten in unterschiedlichen Abonnements landen, die von verschiedenen AAD-Mandanten gesteuert werden. Die Synchronisierung scheint sogar zu funktionieren, aber dies ist keine unterstützte Konfiguration. Die Synchronisierung kann zu einem späteren Zeitpunkt beendet werden, und es gibt keine Möglichkeit, sie wieder in einen funktionierenden Zustand zu versetzen.

Bei der Planung der Ressourcenverschiebung müssen Sie verschiedene Überlegungen zum [Verschieben innerhalb desselben AAD-Mandanten](#move-within-the-same-azure-active-directory-tenant) und zum [Verschieben in einen anderen AAD-Mandanten](#move-to-a-new-azure-active-directory-tenant) berücksichtigen. Wenn Sie AAD-Mandanten verschieben, verschieben Sie Synchronisierungs- und Speicherressourcen immer gemeinsam.

### <a name="move-within-the-same-azure-active-directory-tenant"></a>Verschieben innerhalb desselben Azure Active Directory-Mandanten

:::row:::
    :::column:::
        :::image type="content" source="media/storage-sync-resource-move/storage-sync-resource-move-small.png" alt-text="Abbildung: Azure-Portal für eine Ressource des Speichersynchronisierungsdiensts mit erweitertem Befehl „Verschieben. Das Bild zeigt die Optionen für die Verschiebung von Ressourcengruppe und Abonnement." lightbox="media/storage-sync-resource-move/storage-sync-resource-move.png":::
    :::column-end:::
    :::column:::
        Das Azure-Portal bietet eine bequeme Möglichkeit zum Verschieben einer Speichersynchronisierungsdienst-Ressource. Navigieren Sie zu dem Speichersynchronisierungsdienst, den Sie verschieben möchten, und wählen Sie in der Befehlsleiste den Befehl **Verschieben** aus. Dieselben Schritte gelten für das Verschieben eines Speicherkontos. Auf diese Weise können Sie auch alle Ressourcen in einer Ressourcengruppe verschieben. Das Verschieben einer vollständigen Ressourcengruppe wird empfohlen, wenn sich der Speichersynchronisierungsdienst und alle von diesem Dienst verwendeten Speicherkonten in dieser Ressourcengruppe befinden.
    :::column-end:::
:::row-end:::

> [!WARNING]
> Wenn Sie eine Speicherkontoressource verschieben, wird die Synchronisierung sofort beendet. Sie müssen die Synchronisierung manuell dazu autorisieren, auf die entsprechenden Speicherkonten im neuen Abonnement zuzugreifen. Im Abschnitt [Autorisierung des Speicherzugriffs durch die Azure-Dateisynchronisierung](#azure-file-sync-storage-access-authorization) finden Sie die notwendigen Schritte.

### <a name="move-to-a-new-azure-active-directory-tenant"></a>Verschieben in einen neuen Azure Active Directory-Mandanten

Einzelne Ressourcen, wie z. B. ein Speichersynchronisierungsdienst oder Speicherkonten, können nicht alleine in einen anderen AAD-Mandanten verschoben werden. Nur Azure-Abonnements können AAD-Mandanten verschieben. Betrachten Sie Ihre Abonnementstruktur im neuen AAD-Mandanten. Sie können ein dediziertes Abonnement für die Azure-Dateisynchronisierung verwenden. 

1. Erstellen Sie ein Azure-Abonnement (oder bestimmen Sie ein vorhandenes im alten Mandanten, das verschoben werden soll).
1. Führen Sie für Ihren Speichersynchronisierungsdienst und alle zugehörigen Speicherkonten [eine Abonnementverschiebung innerhalb desselben AAD-Mandanten](#move-within-the-same-azure-active-directory-tenant) aus.
1. Die Synchronisierung wird beendet. Schließen Sie die Mandantenverschiebung sofort ab, oder [stellen Sie den Zugriff der Synchronisierung auf die verschobenen Speicherkonten wieder her](#azure-file-sync-storage-access-authorization). Sie können diese dann später in den neuen AAD-Mandanten verschieben.

Sobald alle zugehörigen Ressourcen für die Azure-Dateisynchronisierung in einem eigenen Abonnement zusammengefasst wurden, können Sie das gesamte Abonnement in den AAD-Zielmandanten verschieben. Mit dem [Leitfaden zum Übertragen von Abonnements](../../role-based-access-control/transfer-subscription.md) können Sie eine solche Übertragung planen und ausführen.

> [!WARNING]
> Wenn Sie ein Abonnement von einem Mandanten auf einen anderen übertragen, wird die Synchronisierung sofort beendet. Sie müssen die Synchronisierung manuell dazu autorisieren, auf die entsprechenden Speicherkonten im neuen Abonnement zuzugreifen. Im Abschnitt [Autorisierung des Speicherzugriffs durch die Azure-Dateisynchronisierung](#azure-file-sync-storage-access-authorization) finden Sie die notwendigen Schritte.

:::row:::
    :::column:::
        :::image type="content" source="media/storage-sync-resource-move/storage-sync-resource-move-aad-tenant.png" alt-text="Diese Abbildung zeigt das Azure-Portal mit dem Übersichtsblatt für das Abonnement und hervorgehobenem Symbolleistenbefehl „Verzeichnis ändern“ im oberen Bereich der Seite." lightbox="media/storage-sync-resource-move/storage-sync-resource-move-aad-tenant-expanded.png":::
    :::column-end:::
    :::column:::
        Sie können mit der Migration beginnen, sobald Sie über einen Plan und die erforderlichen Berechtigungen verfügen:
        1. Navigieren Sie im Azure-Portal zum Blatt **Übersicht** für Ihr Abonnement.
        1. Wählen Sie **Verzeichnis ändern** aus.
        1. Führen Sie die Schritte des Assistenten aus, um den neuen AAD-Mandanten zuzuweisen.
    :::column-end:::
:::row-end:::

## <a name="azure-file-sync-storage-access-authorization"></a>Autorisierung des Speicherzugriffs durch die Azure-Dateisynchronisierung

Wenn Speicherkonten in ein neues Abonnement oder innerhalb eines Abonnements in einen neuen AAD-Mandanten verschoben werden, wird die Synchronisierung beendet. Die Autorisierung der Azure-Dateisynchronisierung zum Zugriff auf ein Speicherkonto erfolgt über die rollenbasierte Zugriffssteuerung (RBAC). Diese Rollenzuweisungen werden nicht zusammen mit den Ressourcen migriert.

### <a name="azure-file-sync-service-principal"></a>Dienstprinzipal der Azure-Dateisynchronisierung

:::row:::
    :::column:::
        :::image type="content" source="media/storage-sync-resource-move/storage-sync-resource-move-afs-rp-registered-small.png" alt-text="Abbildung: Azure-Portal mit Abonnementverwaltung und registrierten Ressourcenanbietern." lightbox="media/storage-sync-resource-move/storage-sync-resource-move-afs-rp-registered.png":::
    :::column-end:::
    :::column:::
        Der Dienstprinzipal der Azure-Dateisynchronisierung muss im AAD-Mandanten vorhanden sein, bevor Sie die Synchronisierung zum Zugriff auf ein Speicherkonto autorisieren können. </br></br> Wenn Sie heute ein neues Azure-Abonnement erstellen, wird der Ressourcenanbieter *Microsoft.StorageSync* für die Azure-Dateisynchronisierung automatisch bei Ihrem Abonnement registriert. Durch die Registrierung des Ressourcenanbieters wird ein *Dienstprinzipal* für die Synchronisierung im Azure Active Directory-Mandanten verfügbar gemacht, der das Abonnement regelt. Ein Dienstprinzipal ähnelt einem Benutzerkonto in Ihrem Azure Active Directory. Sie können den Dienstprinzipal der Azure-Dateisynchronisierung dazu verwenden, den Zugriff auf Ressourcen über die rollenbasierte Zugriffssteuerung (RBAC) zu autorisieren. Die einzige Ressource, auf die die Synchronisierung zugreifen muss, sind Ihre Speicherkonten mit den Dateifreigaben, die synchronisiert werden sollen. *Microsoft.StorageSync* muss der integrierten Rolle **Lese- und Datenzugriff** im Speicherkonto zugewiesen sein. </br></br> Diese Zuweisung erfolgt automatisch über den Benutzerkontext des angemeldeten Benutzers, wenn Sie eine Dateifreigabe zu einer Synchronisierungsgruppe hinzufügen, Sie also einen Cloudendpunkt erstellen. Wenn ein Speicherkonto in ein neues Abonnement oder einen neuen AAD-Mandanten verschoben wird, geht diese Rollenzuweisung verloren und [muss manuell wiederhergestellt werden](#establish-sync-access-to-a-storage-account).
    :::column-end:::
:::row-end:::

> [!IMPORTANT]
> Wenn das Azure-Zielabonnement nicht erst vor Kurzem erstellt wurde, überprüfen Sie, ob der Ressourcenanbieter *Microsoft.StorageSync* beim Abonnement registriert ist. Fügen Sie ihn ggf. auf demselben Portalblatt hinzu.

### <a name="establish-sync-access-to-a-storage-account"></a>Einrichten des Zugriffs der Synchronisierung auf ein Speicherkonto

Der [Dienstprinzipal der Azure-Dateisynchronisierung](#azure-file-sync-service-principal) muss verwendet werden, den Zugriff auf ein Speicherkonto per rollenbasierter Zugriffssteuerung zu autorisieren. *Microsoft.StorageSync* muss der integrierten Rolle **Lese- und Datenzugriff** im Speicherkonto zugewiesen sein. 

Diese Zuweisung erfolgt in der Regel automatisch über den Benutzerkontext des angemeldeten Benutzers, wenn Sie eine Dateifreigabe zu einer Synchronisierungsgruppe hinzufügen, Sie also einen Cloudendpunkt erstellen. Wenn jedoch ein Speicherkonto in ein neues Abonnement oder einen neuen AAD-Mandanten verschoben wird, geht diese Rollenzuweisung verloren und muss manuell wiederhergestellt werden.

:::row:::
    :::column:::
        :::image type="content" source="media/storage-sync-resource-move/storage-sync-resource-move-assign-rbac.png" alt-text="Diese Abbildung zeigt den Dienstprinzipal „Microsoft.StorageSync“, dem in einem Speicherkonto die Rolle „Lese- und Datenzugriff“ zugewiesen ist.":::
    :::column-end:::
    :::column:::
        Navigieren Sie im Azure-Portal zu dem Speicherkonto, für den Sie den Zugriff durch die Synchronisierung erneut autorisieren müssen. <ol><li>Wählen Sie im Inhaltsverzeichnis auf der linken Seite die Option **Zugriffssteuerung (IAM)** aus.</li><li>Wählen Sie die Registerkarte „Rollenzuweisungen“ aus, um die Liste der Benutzer und Anwendungen (Dienstprinzipale) anzuzeigen, die Zugriff auf Ihr Speicherkonto haben.</li><li>Wählen Sie **Hinzufügen** aus.</li><li>Wählen Sie im Feld **Rolle** die Option **Lese- und Datenzugriff** aus.</li><li>Geben Sie im Feld **Auswählen** die Zeichenfolge *Microsoft.StorageSync* ein, wählen Sie die Rolle aus, und klicken Sie auf **Speichern**.</li></ol>
    :::column-end:::
:::row-end:::

## <a name="move-to-a-different-azure-region"></a>Verschieben in eine andere Azure-Region

Die Azure-Dateisynchronisierungsressource *Speichersynchronisierungsdienst* und die Speicherkonten, die zu synchronisierende Dateifreigaben enthalten, sind in einer bestimmten Azure-Region bereitgestellt. Sie bestimmen diese Region, wenn Sie eine Ressource erstellen. Die Region der Ressourcen für Speichersynchronisierungsdiensts und Speicherkonto muss dieselbe sein. Diese Regionen können nach der Erstellung für keinen Ressourcentyp geändert werden.

Die Zuweisung einer anderen Region für eine Ressource ist nicht dasselbe wie ein [Regionsfailover](#region-fail-over), das je nach Redundanzeinstellung Ihres Speicherkontos unterstützt werden kann.

## <a name="region-fail-over"></a>Regionsfailover

[Azure Storage bietet Georedundanzoptionen](../common/storage-redundancy.md#geo-redundant-storage) für ein Speicherkonto. Diese Redundanzoptionen können bei Speicherkonten mit Azure-Dateisynchronisierung zu Problemen führen. Der Hauptgrund dafür ist, dass die Replikation zwischen geografisch getrennten Regionen nicht durch die Azure-Dateisynchronisierung ausgeführt wird, sondern durch eine Speicherreplikationstechnologie, die in das Speichersubsystem von Azure integriert ist. Diese hat keine Informationen zum Zustand einer Anwendung, und die Azure-Dateisynchronisierung ist eine Anwendung, in der zu jedem beliebigen Zeitpunkt Dateien aus und in Azure-Dateifreigaben synchronisiert werden. Wenn Sie sich für eine dieser geografisch verteilten Speicherredundanzoptionen entscheiden, verlieren Sie im Katastrophenfall nicht all Ihre Daten. Sie müssen allerdings mit einem gewissen Maß an [Datenverlust rechnen](../common/storage-disaster-recovery-guidance.md#anticipate-data-loss).

> [!CAUTION]
> Ein Failover ist niemals ein geeigneter Ersatz für das Bereitstellen von Ressourcen in der richtigen Azure-Region. Wenn Ihre Ressourcen sich in der „falschen“ Region befinden, sollten Sie die Synchronisierung beenden und für neue Azure-Dateifreigaben einrichten, die in der gewünschten Region bereitgestellt wurden.

Ein Regionsfailover kann von Microsoft nach einem schwerwiegenden Ereignis gestartet werden, durch das Rechenzentren in einer Azure-Region über einen längeren Zeitraum hinweg nicht verfügbar sein werden. Die Ausfallzeiten, die Ihr Unternehmen tolerieren kann, sind möglicherweise kürzer als der Zeitraum, den Microsoft verstreichen lässt, bevor ein Regionsfailover gestartet wird. Für solche Situationen [können Failover auch durch Kunden initiiert werden](../common/storage-initiate-account-failover.md).

> [!IMPORTANT]
> Bei einem Failover müssen Sie ein Supportticket für Ihre betroffenen Speichersynchronisierungsdienste eröffnen, damit die Synchronisierung wieder funktioniert.

## <a name="see-also"></a>Siehe auch

- [Übersicht über Migrationsleitfäden für Azure-Dateifreigaben und Synchronisierung](../files/storage-files-migration-overview.md?toc=%2fazure%2fstorage%2ffilesync%2ftoc.json)
- [Problembehandlung für die Azure-Dateisynchronisierung](file-sync-troubleshoot.md)
- [Planung für die Bereitstellung einer Azure-Dateisynchronisierung](file-sync-planning.md)