---
title: Blobversionsverwaltung (Vorschau)
titleSuffix: Azure Storage
description: Die Blob Storage-Versionsverwaltung (Vorschau) verwaltet automatisch vorherige Versionen eines Objekts und identifiziert diese durch Zeitstempel. Sie können frühere Versionen eines Blobs wiederherstellen, um Daten wiederherzustellen, wenn diese irrtümlich geändert oder gelöscht wurden.
services: storage
author: tamram
ms.service: storage
ms.topic: conceptual
ms.date: 05/05/2020
ms.author: tamram
ms.subservice: blobs
ms.openlocfilehash: 7e4bc74a51e3d6b19957bdd12512e18fa594c811
ms.sourcegitcommit: a8ee9717531050115916dfe427f84bd531a92341
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/12/2020
ms.locfileid: "83123835"
---
# <a name="blob-versioning-preview"></a>Blobversionsverwaltung (Vorschau)

Sie können Blob Storage-Versionsverwaltung (Vorschau) aktivieren, um frühere Versionen eines Objekts automatisch zu verwalten.  Wenn Blobversionsverwaltung aktiviert ist, können Sie eine frühere Version eines Blobs wiederherstellen, um Daten wiederherzustellen, wenn diese irrtümlich geändert oder gelöscht wurden.

Blobversionsverwaltung ist für das Speicherkonto aktiviert und gilt für alle Blobs im Speicherkonto. Nachdem Sie Blobversionsverwaltung für ein Speicherkonto aktiviert haben, werden durch Azure Storage automatisch Versionen für jedes Blob im Speicherkonto verwaltet.

Microsoft empfiehlt die Verwendung von Blobversionsverwaltung, um frühere Versionen eines Blobs für erstklassigen Datenschutz zu verwalten. Verwenden Sie Blobversionsverwaltung nach Möglichkeit anstelle von Blobmomentaufnahmen, um vorherige Versionen zu verwalten. Blobmomentaufnahmen bieten eine ähnliche Funktionalität, da Sie frühere Versionen eines Blobs beibehalten, aber Momentaufnahmen müssen manuell von Ihrer Anwendung verwaltet werden.

> [!IMPORTANT]
> Blobversionsverwaltung kann Ihnen nicht bei der Wiederherstellung nach dem versehentlichen Löschen eines Speicherkontos oder Containers helfen. Um das versehentliche Löschen des Speicherkontos zu verhindern, konfigurieren Sie eine **CannotDelete**-Sperre für die Speicherkontoressource. Weitere Informationen zum Sperren von Azure-Ressourcen finden Sie unter [Sperren von Ressourcen, um unerwartete Änderungen zu verhindern](../../azure-resource-manager/management/lock-resources.md).

## <a name="how-blob-versioning-works"></a>Funktionsweise der Blobversionsverwaltung

Eine Version erfasst den Zustand eines Blobs zu einem bestimmten Zeitpunkt. Wenn Blobversionsverwaltung für ein Speicherkonto aktiviert ist, erstellt Azure Storage automatisch jedes Mal eine neue Version eines Blobs, wenn dieses Blob geändert oder gelöscht wird.

Wenn Sie ein Blob mit aktivierter Versionsverwaltung erstellen, entspricht das neue Blob der aktuellen Version des Blobs (oder des Basisblobs). Wenn Sie dieses Blob anschließend ändern, erstellt Azure Storage eine Version, die den Zustand des Blobs vor der Änderung erfasst. Das geänderte Blob wird zur neuen aktuellen Version. Wenn Sie das Blob ändern, wird jedes Mal eine neue Version erstellt.

Wenn Sie ein Blob mit aktivierter Versionsverwaltung löschen, erstellt Azure Storage eine Version, die den Zustand des Blobs vor dem Löschen erfasst. Die aktuelle Version des Blobs wird dann gelöscht, aber die Versionen des Blobs bleiben erhalten, sodass sie bei Bedarf wiederhergestellt werden können. 

Blobversionen sind unveränderlich. Inhalte oder Metadaten einer vorhandenen Blobversion können nicht geändert werden.

### <a name="version-id"></a>Versions-ID

Jede Blobversion wird durch eine Versions-ID identifiziert. Der Wert der Versions-ID ist der Zeitstempel, an dem das Blob geschrieben oder aktualisiert wurde. Die Versions-ID wird zum Zeitpunkt der Erstellung der Version zugewiesen.

Sie können Lese- oder Löschvorgänge für eine bestimmte Version eines Blobs durchführen, indem Sie die zugehörige Versions-ID angeben. Wenn Sie die Versions-ID nicht angeben, wird der Vorgang für die aktuelle Version (das Basisblob) ausgeführt.

Wenn Sie einen Schreibvorgang zum Erstellen oder Ändern eines Blobs aufrufen, gibt Azure Storage den Header *x-ms-version-id* Header in der Antwort zurück. Dieser Header enthält die Versions-ID für die aktuelle Version des Blobs, das durch den Schreibvorgang erstellt wurde.

Die Versions-ID bleibt für die Lebensdauer der Version gleich.

### <a name="versioning-on-write-operations"></a>Versionsverwaltung für Schreibvorgänge

Wenn Blobversionsverwaltung aktiviert ist, erstellt jeder Schreibvorgang in einem Blob eine neue Version. Zu den Schreibvorgängen zählen [Put Blob](/rest/api/storageservices/put-blob), [Put Block List](/rest/api/storageservices/put-block-list), [Copy Blob](/rest/api/storageservices/copy-blob) und [Set Blob Metadata](/rest/api/storageservices/set-blob-metadata).

Wenn der Schreibvorgang ein neues Blob erstellt, entspricht das sich ergebende Blob der aktuellen Version des Blobs. Wenn der Schreibvorgang ein vorhandenes Blob ändert, werden die neuen Daten im aktualisierten Blob aufgezeichnet. Dies ist die aktuelle Version, und Azure Storage erstellt eine Version, die den vorherigen Zustand des Blobs speichert.

Der Einfachheit halber wird in den in diesem Artikel gezeigten Abbildungen die Versions-ID als einfacher Integerwert angezeigt. In Wirklichkeit ist die Versions-ID ein Zeitstempel. Die aktuelle Version ist blau dargestellt, und vorherige Versionen werden grau dargestellt.

Die folgende Abbildung zeigt, wie sich Schreibvorgänge auf Blobversionen auswirken. Wenn ein Blob erstellt wird, ist dieses Blob die aktuelle Version. Wenn das gleiche Blob geändert wird, wird eine neue Version erstellt, um den vorherigen Zustand des Blobs zu speichern, und das aktualisierte Blob wird zur aktuellen Version.

:::image type="content" source="media/versioning-overview/write-operations-blob-versions.png" alt-text="Abbildung, die zeigt, wie sich Schreibvorgänge auf Blobs mit Versionsangabe auswirken":::

> [!NOTE]
> Ein Blob, das vor Aktivierung von Versionsverwaltung für das Speicherkonto erstellt wurde, weist keine Versions-ID auf. Wenn dieses Blob geändert wird, wird das geänderte Blob zur aktuellen Version, und es wird eine Version erstellt, um den Zustand des Blobs vor der Aktualisierung zu speichern. Der Version wird eine Versions-ID zugewiesen, die ihre Erstellungszeit angibt.

### <a name="versioning-on-delete-operations"></a>Versionsverwaltung für Löschvorgänge

Wenn Sie ein Blob löschen, wird die aktuelle Version des Blobs zu einer früheren Version, und das Basisblob wird gelöscht. Alle vorhandenen früheren Versionen des Blobs bleiben erhalten, wenn das Blob gelöscht wird.

Durch Aufrufen des Vorgangs [Delete Blob](/rest/api/storageservices/delete-blob) Vorgang ohne Versions-ID wird das Basisblob gelöscht. Um eine bestimmte Version zu löschen, geben Sie die ID für diese Version für den Löschvorgang an.

Die folgende Abbildung zeigt die Auswirkung eines Löschvorgangs auf ein Blob mit Versionsangabe:

:::image type="content" source="media/versioning-overview/delete-versioned-base-blob.png" alt-text="Abbildung, die das Löschen eines Blobs mit Versionsangabe zeigt":::

Wenn Sie neue Daten in das Blob schreiben, wird eine neue Version des Blobs erstellt. Alle vorhandenen Versionen sind nicht betroffen, wie in der folgenden Abbildung gezeigt.

:::image type="content" source="media/versioning-overview/recreate-deleted-base-blob.png" alt-text="Abbildung, die die erneute Erstellung eines Blobs mit Versionsangabe nach dem Löschen zeigt":::

### <a name="blob-types"></a>Blobtypen

Wenn Blobversionsverwaltung für ein Speicherkonto aktiviert ist, lösen alle Schreib- und Löschvorgänge für Blockblobs die Erstellung einer neuen Version aus, mit Ausnahme des Vorgangs [Put Block](/rest/api/storageservices/put-block).

Bei Seitenblobs und Anfügeblobs löst nur eine Teilmenge von Schreib- und Löschvorgängen die Erstellung einer Version aus. Dazu zählen die Operationen:

- [Put Blob](/rest/api/storageservices/put-blob)
- [Put Block List](/rest/api/storageservices/put-block-list)
- [Delete Blob](/rest/api/storageservices/delete-blob)
- [Set Blob Metadata](/rest/api/storageservices/set-blob-metadata)
- [Copy Blob](/rest/api/storageservices/copy-blob)

Die folgenden Vorgänge führen nicht zum Erstellen einer neuen Version. Um Änderungen aus diesen Vorgängen aufzuzeichnen, erstellen Sie eine manuelle Momentaufnahme:

- [Put Page](/rest/api/storageservices/put-page) (Seitenblob)
- [Append Block](/rest/api/storageservices/append-block) (Anfügeblob)

Alle Versionen eines Blobs müssen denselben Blobtyp aufweisen. Wenn ein Blob über vorherige Versionen verfügt, können Sie ein Blob eines Typs nicht mit einem anderen Typ überschreiben, es sei denn, Sie löschen zuerst das Blob und alle zugehörigen Versionen.

### <a name="access-tiers"></a>Zugriffsebenen

Sie können jede beliebige Version eines Blockblobs (einschließlich der aktuellen Version) in eine andere Blobzugriffsebene verschieben, indem Sie den Vorgang [Set Blob Tier](/rest/api/storageservices/set-blob-tier) aufrufen. Sie können niedrigere Kapazitätspreise nutzen, indem Sie ältere Versionen eines Blobs in die kalte Ebene oder die Archivebene verschieben. Weitere Informationen finden Sie unter [Azure Blob Storage: Archivzugriffsebenen „Heiß“, „Kalt“ und „Archiv“](storage-blob-storage-tiers.md).

Verwenden Sie Bloblebenszyklusverwaltung, um den Prozess des Verschiebens von Blockblobs in die entsprechende Ebene zu automatisieren. Weitere Informationen zur Lebenszyklusverwaltung finden Sie unter [Verwalten des Lebenszyklus von Azure Blob Storage](storage-lifecycle-management-concepts.md).

## <a name="enable-or-disable-blob-versioning"></a>Aktivieren/Deaktivieren von Blobversionsverwaltung

Informationen zum Aktivieren oder Deaktivieren von Blobversionsverwaltung finden Sie unter [Aktivieren oder Deaktivieren von Blobversionsverwaltung](versioning-enable.md).

Durch Deaktivierung von Blobversionsverwaltung werden vorhandene Blobs, Versionen oder Momentaufnahmen nicht gelöscht. Wenn Sie Blobversionsverwaltung deaktivieren, kann auf alle vorhandenen Versionen in Ihrem Speicherkonto weiterhin zugegriffen werden. Anschließend werden keine weiteren neuen Versionen erstellt.

Wenn ein Blob erstellt oder geändert wurde, nachdem Versionsverwaltung für das Speicherkonto deaktiviert wurde, wird durch das Überschreiben des Blobs eine neue Version erstellt. Das aktualisierte Blob ist nicht mehr die aktuelle Version und weist keine Versions-ID auf. Alle nachfolgenden Aktualisierungen des Blobs überschreiben seine Daten, ohne den vorherigen Zustand zu speichern.

Sie können Versionen mithilfe der Versions-ID lesen oder löschen, nachdem Versionsverwaltung deaktiviert wurde. Nach Deaktivierung von Versionsverwaltung können Sie auch die Versionen eines Blobs auflisten.

Die folgende Abbildung zeigt, wie durch das Ändern eines Blobs nach dem Deaktivieren von Versionsverwaltung ein Blob ohne Versionsangabe erstellt wird. Alle vorhandenen Versionen, die dem Blob zugeordnet sind, bleiben erhalten.

:::image type="content" source="media/versioning-overview/modify-base-blob-versioning-disabled.png" alt-text="Abbildung, die zeigt, wie das Basisblob nach dem Deaktivieren von Versionsverwaltung geändert wird":::

## <a name="blob-versioning-and-soft-delete"></a>Blobversionsverwaltung und vorläufiges Löschen

Blobversionsverwaltung und vorläufiges Löschen von Blobs arbeiten zusammen, um Ihnen optimalen Datenschutz zu bieten. Wenn Sie vorläufiges Löschen aktivieren, geben Sie an, wie lange Azure Storage ein vorläufig gelöschtes Blob beibehalten soll. Alle vorläufig gelöschten Blobversionen verbleiben im System und können innerhalb der Beibehaltungsdauer für vorläufiges Löschen wiederhergestellt werden. Weitere Informationen zum vorläufigen Löschen von Blobs finden Sie unter [Vorläufiges Löschen für Azure Storage-Blobs](storage-blob-soft-delete.md).

### <a name="deleting-a-blob-or-version"></a>Löschen eines Blobs oder einer Version

Vorläufiges Löschen bietet zusätzlichen Schutz beim Löschen von Blobversionen. Wenn sowohl die Versionsverwaltung als auch das vorläufige Löschen für das Speicherkonto aktiviert sind, erstellt Azure Storage beim Löschen eines Blobs eine neue Version, um den Status des Blobs unmittelbar vor dem Löschen zu speichern und die aktuelle Version zu löschen. Die neue Version wird nicht vorläufig gelöscht und nicht entfernt, wenn die Beibehaltungsdauer für vorläufiges Löschen abläuft.

Wenn Sie eine frühere Version des Blobs löschen, wird die Version vorläufig gelöscht. Die vorläufig gelöschte Version wird während der Beibehaltungsdauer aufbewahrt, die in den Einstellungen für vorläufiges Löschen für das Speicherkonto angegeben wird, und wird dauerhaft gelöscht, wenn die Beibehaltungsdauer für vorläufiges Löschen abläuft.

Um eine frühere Version eines Blobs zu entfernen, löschen Sie diese explizit, indem Sie die Versions-ID angeben.

Die folgende Abbildung zeigt, was geschieht, wenn Sie ein Blob oder eine Blobversion löschen.

:::image type="content" source="media/versioning-overview/soft-delete-historical-version.png" alt-text="Abbildung, die das Löschen einer Version mit aktiviertem vorläufigem Löschen zeigt":::

Wenn Versionsverwaltung und vorläufiges Löschen für ein Speicherkonto aktiviert sind, wird keine vorläufig gelöschte Momentaufnahme erstellt, wenn ein Blob oder eine Blobversion geändert oder gelöscht wird.

### <a name="restoring-a-soft-deleted-version"></a>Wiederherstellen einer vorläufig gelöschten Version

Sie können eine vorläufig gelöschte Blobversion wiederherstellen, indem Sie den Vorgang [Undelete Blob](/rest/api/storageservices/undelete-blob) für die Version aufrufen, während die Beibehaltungsdauer für vorläufiges Löschen wirksam ist. Der Vorgang **Undelete Blob** stellt alle vorläufig gelöschten Versionen des Blobs wieder her.

Durch das Wiederherstellen vorläufig gelöschter Versionen mit dem Vorgang **Undelete Blob** wird keine Version in die aktuelle Version höher gestuft. Stellen Sie zum Wiederherstellen der aktuellen Version zunächst alle vorläufig gelöschten Versionen wieder her, und verwenden Sie dann den Vorgang [Copy Blob](/rest/api/storageservices/copy-blob), um eine vorherige Version zu kopieren, um das Blob wiederherzustellen.

In der folgenden Abbildung wird gezeigt, wie Sie vorläufig gelöschte Blobversionen mit dem Vorgang **Undelete Blob** wiederherstellen, und wie Sie die aktuelle Version des Blobs mit dem Vorgang **Copy Blob** wiederherstellen.

:::image type="content" source="media/versioning-overview/undelete-version.png" alt-text="Abbildung, die zeigt, wie vorläufig gelöschte Versionen wiederhergestellt werden":::

Nachdem die Beibehaltungsdauer für vorläufiges löschen abgelaufen ist, werden alle vorläufig gelöschten Blobversionen dauerhaft gelöscht.

## <a name="blob-versioning-and-blob-snapshots"></a>Blobversionsverwaltung und Blobmomentaufnahmen

Eine Blobmomentaufnahme ist eine schreibgeschützte Kopie eines Blobs, die zu einem bestimmten Zeitpunkt erstellt wird. Blobmomentaufnahmen und Blobversionen sind ähnlich, aber eine Momentaufnahme wird manuell von Ihnen oder Ihrer Anwendung erstellt, während eine Blobversion bei einem Schreib- oder Löschvorgang automatisch erstellt wird, wenn die Blobversionsverwaltung für Ihr Speicherkonto aktiviert ist.

> [!IMPORTANT]
> Microsoft empfiehlt, dass Sie nach der Aktivierung von Blobversionsverwaltung auch Ihre Anwendung aktualisieren, damit keine Momentaufnahmen von Blockblobs mehr erstellt werden. Wenn Versionsverwaltung für Ihr Speicherkonto aktiviert ist, werden alle Blockblobaktualisierungen und -löschungen durch Versionen aufgezeichnet und beibehalten. Die Erstellung von Momentaufnahmen bietet keinen zusätzlichen Schutz für Ihre Blockblobdaten, wenn Blobversionsverwaltung aktiviert ist, und sie erhöht möglicherweise die Kosten und die Komplexität der Anwendung.

### <a name="snapshot-a-blob-when-versioning-is-enabled"></a>Momentaufnahme eines Blobs bei aktivierter Versionsverwaltung

Obwohl dies nicht empfohlen wird, können Sie eine Momentaufnahme eines Blobs erstellen, das auch mit einer Versionsangabe versehen ist. Wenn Sie die Anwendung nicht so aktualisieren können, dass keine Momentaufnahmen von Blobs erstellt werden, wenn Sie Versionsverwaltung aktivieren, kann Ihre Anwendung sowohl Momentaufnahmen als auch Versionen unterstützen.

Wenn Sie eine Momentaufnahme eines Blobs mit Versionsangabe erstellen, wird eine neue Version zur gleichen Zeit erstellt, zu der die Momentaufnahme erstellt wird. Eine neue aktuelle Version wird ebenfalls erstellt, wenn eine Momentaufnahme erstellt wird.

Die folgende Abbildung zeigt, was geschieht, wenn Sie eine Momentaufnahme eines Blobs mit Versionsangabe erstellen. In der Abbildung enthalten Blobversionen und Momentaufnahmen mit der Versions-ID 2 und 3 identische Daten.

:::image type="content" source="media/versioning-overview/snapshot-versioned-blob.png" alt-text="Abbildung, die Momentaufnahmen eines Blobs mit Versionsangabe zeigt":::

## <a name="authorize-operations-on-blob-versions"></a>Autorisieren von Vorgängen für Blobversionen

Sie können Zugriff auf Blobversionen mit einer der folgenden Methoden autorisieren:

- Über die rollenbasierte Zugriffssteuerung (Role-Based Access Control, RBAC), um Berechtigungen für einen Azure Active Directory-Sicherheitsprinzipal (Azure AD) zu erteilen. Microsoft empfiehlt die Verwendung von Azure AD für mehr Sicherheit und Benutzerfreundlichkeit. Weitere Informationen zur Verwendung von Azure AD mit Blobvorgängen finden Sie unter [Autorisieren des Zugriffs auf Blobs und Warteschlangen mit Azure Active Directory](../common/storage-auth-aad.md).
- Durch Delegieren des Zugriffs auf Blobversionen mithilfe einer Shared Access Signature (SAS). Geben Sie die Versions-ID für den signierten Ressourcentyp `bv` an, der eine Blobversion darstellt, um ein SAS-Token für Vorgänge mit einer bestimmten Version zu erstellen. Weitere Informationen zu SAS (Shared Access Signatures) finden Sie unter [Gewähren von eingeschränktem Zugriff auf Azure Storage-Ressourcen mithilfe von SAS (Shared Access Signature)](../common/storage-sas-overview.md).
- Durch Verwendung der Kontozugriffsschlüssel zur Autorisierung von Vorgängen für Blobversionen mit gemeinsam verwendetem Schlüssel. Weitere Informationen finden Sie unter [Authentifizieren mit gemeinsam verwendetem Schlüssel](/rest/api/storageservices/authorize-with-shared-key).

Blobversionsverwaltung dient zum Schutz Ihrer Daten vor versehentlichem oder böswilligem Löschen. Um den Schutz zu verbessern, sind für das Löschen einer Blobversion spezielle Berechtigungen erforderlich. In den folgenden Abschnitten werden die Berechtigungen beschrieben, die zum Löschen einer Blobversion erforderlich sind.

### <a name="rbac-action-to-delete-a-blob-version"></a>RBAC-Aktion zum Löschen einer Blobversion

In der folgenden Tabelle wird gezeigt, welche RBAC-Aktionen das Löschen eines Blobs oder einer Blobversion unterstützen.

| BESCHREIBUNG | Vorgang des Blob-Diensts | Erforderliche RBAC-Datenaktion | Unterstützung für integrierte RBAC-Rollen |
|----------------------------------------------|------------------------|---------------------------------------------------------------------------------------|-------------------------------|
| Löschen der aktuellen Version des Blobs | Delete Blob | **Microsoft.Storage/storageAccounts/blobServices/containers/blobs/delete/actionDeleting** | Mitwirkender an Storage-Blobdaten |
| Löschen einer Version | Delete Blob | **Microsoft.Storage/storageAccounts/blobServices/containers/blobs/deleteBlobVersion/** | Besitzer von Speicherblobdaten |

### <a name="shared-access-signature-sas-parameters"></a>SAS-Parameter (Shared Access Signature)

Die signierte Ressource für eine Blobversion ist `bv`. Weitere Informationen finden Sie unter [Erstellen einer Dienst-SAS](/rest/api/storageservices/create-service-sas) oder [Erstellen einer SAS für die Benutzerdelegierung](/rest/api/storageservices/create-user-delegation-sas).

In der folgenden Tabelle werden die Berechtigungen aufgeführt, die für eine SAS erforderlich sind, um eine Blobversion zu löschen.

| **Berechtigung** | **URI-Symbol** | **Zulässige Vorgänge** |
|----------------|----------------|------------------------|
| Löschen         | x              | Löschen einer Blobversion. |

## <a name="about-the-preview"></a>Informationen zur Vorschau

Blobversionsverwaltung ist als Vorschau in den folgenden Regionen verfügbar:

- Frankreich, Mitte
- Kanada, Osten
- Kanada, Mitte

Diese Vorschau ist nur für die Verwendung außerhalb der Produktion bestimmt.

Version 2019-10-10 und höher der Azure Storage-REST-API unterstützt die Blobversionsverwaltung.

### <a name="storage-account-support"></a>Speicherkontounterstützung

Blobversionsverwaltung ist für die folgenden Typen von Speicherkonten verfügbar:

- Speicherkonten vom Typ „Universell V2“
- Blockblob-Speicherkonten
- Blob-Speicherkonten

Wenn Ihr Speicherkonto vom Typ „Universell V1“ ist, verwenden Sie das Azure-Portal, um ein Upgrade auf ein Konto vom Typ „Universell V2“ durchzuführen. Weitere Informationen zu Speicherkonten finden Sie unter [Azure-Speicherkonto – Übersicht](../common/storage-account-overview.md).

Speicherkonten mit einem hierarchischen Namespace, die für die Verwendung mit Azure Data Lake Storage Gen2 aktiviert sind, werden derzeit nicht unterstützt.

### <a name="register-for-the-preview"></a>Registrieren für die Vorschau

Um sich für die Vorschauversion der Blobversionsverwaltung zu registrieren, verwenden Sie PowerShell oder die Azure CLI, um eine Anforderung zum Registrieren des Features bei Ihrem Abonnement zu übermitteln. Nachdem die Anforderung genehmigt wurde, können Sie Blobversionsverwaltung mit allen neuen oder vorhandenen allgemeinen Speicherkonten vom Typ „Universell V2“, mit Blob Storage- oder Blockblob-Speicherkonten vom Typ „Premium“ aktivieren.

# <a name="powershell"></a>[PowerShell](#tab/powershell)

Für die Registrierung mit PowerShell rufen Sie den Befehl [Get-AzProviderFeature](/powershell/module/az.resources/get-azproviderfeature) auf.

```powershell
# Register for blob versioning (preview)
Register-AzProviderFeature -ProviderNamespace Microsoft.Storage `
    -FeatureName Versioning

# Refresh the Azure Storage provider namespace
Register-AzResourceProvider -ProviderNamespace Microsoft.Storage
```

# <a name="azure-cli"></a>[Azure-Befehlszeilenschnittstelle](#tab/azure-cli)

Zum Registrieren bei Azure CLI rufen Sie den Befehl „[az feature register](/cli/azure/feature#az-feature-register)“ auf.

```azurecli
az feature register --namespace Microsoft.Storage \
    --name Versioning
```

---

### <a name="check-the-status-of-your-registration"></a>Überprüfen des Registrierungsstatus

Verwenden Sie PowerShell oder die Azure CLI, um den Status Ihrer Registrierung zu überprüfen.

# <a name="powershell"></a>[PowerShell](#tab/powershell)

Um den Status Ihrer Registrierung mit PowerShell zu überprüfen, rufen Sie den Befehl [Get-AzProviderFeature](/powershell/module/az.resources/get-azproviderfeature) auf.

```powershell
Get-AzProviderFeature -ProviderNamespace Microsoft.Storage `
    -FeatureName Versioning
```

# <a name="azure-cli"></a>[Azure-Befehlszeilenschnittstelle](#tab/azure-cli)

Um den Status Ihrer Registrierung über die Azure-Befehlszeilenschnittstelle zu überprüfen, rufen Sie den Befehl [az feature](/cli/azure/feature#az-feature-show) auf.

```azurecli
az feature show --namespace Microsoft.Storage \
    --name Versioning
```

---

## <a name="pricing-and-billing"></a>Preise und Abrechnung

Wenn Sie Blobversionsverwaltung aktivieren, kann dies zu zusätzlichen Kosten für Datenspeicher für Ihr Konto führen. Wenn Sie beim Entwurf Ihrer Anwendung berücksichtigen, wie diese Gebühren auflaufen, können Sie die Kosten minimieren.

Blobversionen werden wie Blobmomentaufnahmen mit den gleichen Gebühren abgerechnet wie aktive Daten. Wenn eine Version Blöcke oder Seiten mit dem Basisblob gemeinsam verwendet, bezahlen Sie nur für zusätzliche Blöcke oder Seiten, die nicht von der Version und dem Basisblob gemeinsam genutzt werden.

> [!NOTE]
> Das Aktivieren von Versionsverwaltung für häufig überschriebene Daten kann zu höheren Gebühren für die Speicherkapazität und einer höheren Latenz bei Auflistungsvorgängen führen. Um diese Probleme zu vermeiden, speichern Sie häufig überschrieben Daten in einem separaten Speicherkonto mit deaktivierter Versionsverwaltung.

### <a name="important-billing-considerations"></a>Wichtige Überlegungen zur Abrechnung

Beachten Sie die folgenden Punkte, wenn Sie Blobversionsverwaltung aktivieren:

- Für Ihr Speicherkonto fallen Gebühren für eindeutige Blöcke oder Seiten an, die im Blob oder in einer vorherigen Version des Blobs enthalten sind. Ihrem Konto werden erst dann zusätzliche Gebühren für Versionen angerechnet, die einem Blob zugeordnet sind, wenn Sie das Blob aktualisieren, auf dem sie basieren. Sobald Sie das Blob aktualisieren, entstehen Abweichungen früheren Versionen. In diesem Fall werden für alle eindeutigen Blöcke oder Seiten in jedem Blob bzw. jeder Version Gebühren berechnet.
- Wenn Sie einen Block innerhalb eines Block-BLOBs ersetzen, wird dieser Block anschließend als eindeutiger Block berechnet. Dies gilt auch, wenn der Block dieselbe Block-ID und dieselben Daten enthält wie in der Version. Nachdem ein erneuter Commit für den Block ausgeführt wurde, weicht er von seinem Pendant in den Versionen ab, und Ihnen werden die Daten des Blocks berechnet. Gleiches gilt für eine Seite in einem Seitenblob, die mit identischen Daten aktualisiert wird.
- Blob Storage kann nicht feststellen, ob zwei Blöcke identische Daten enthalten. Jeder hochgeladene Block, für den ein Commit ausgeführt wird, wird als eindeutig behandelt, selbst wenn die enthaltenen Daten und die Block-ID identisch sind. Da Gebühren jeweils für eindeutige Blöcke anfallen, ist zu berücksichtigen, dass beim Aktualisieren eines Blobs mit aktivierter Versionsverwaltung zusätzliche eindeutige Blöcke generiert werden, für die zusätzliche Gebühren entstehen.
- Wenn Blobversionsverwaltung aktiviert ist, entwerfen Sie Aktualisierungsvorgänge für Blockblobs, damit Sie die geringstmögliche Anzahl von Blöcken aktualisieren. Die Schreibvorgänge, die eine differenzierte Steuerung von Blöcken ermöglichen, sind [Put Block](/rest/api/storageservices/put-block) und [Put Block List](/rest/api/storageservices/put-block-list). Der Vorgang [Put Blob](/rest/api/storageservices/put-blob) hingegen ersetzt den gesamten Inhalt eines Blobs und kann zu zusätzlichen Kosten führen.

### <a name="versioning-billing-scenarios"></a>Abrechnungsszenarien für Versionsverwaltung

Die folgenden Szenarien veranschaulichen, wie Gebühren für ein Blockblob und zugehörige Versionen berechnet werden.

#### <a name="scenario-1"></a>Szenario 1

In Szenario 1 ist eine frühere Version des Blobs vorhanden. Das Blob wurde nicht aktualisiert, seit die Version erstellt wurde, sodass Gebühren nur für die eindeutigen Blöcke 1, 2 und 3 anfallen.

![Azure Storage-Ressourcen](./media/versioning-overview/versions-billing-scenario-1.png)

#### <a name="scenario-2"></a>Szenario 2

In Szenario 2 wurde ein Block (Block 3 in der Abbildung) im Blob aktualisiert. Obwohl der aktualisierte Block die gleichen Daten und dieselbe ID enthält, ist er nicht identisch mit Block 3 in der vorherigen Version. Daher wird das Konto mit Gebühren für vier Blöcke belastet:

![Azure Storage-Ressourcen](./media/versioning-overview/versions-billing-scenario-2.png)

#### <a name="scenario-3"></a>Szenario 3

In Szenario 3 wurde das Blob aktualisiert, die Version jedoch nicht. Block 3 wurde im Basisblob durch Block 4 ersetzt, die vorherige Version enthält aber immer noch Block 3. Daher wird das Konto mit Gebühren für vier Blöcke belastet:

![Azure Storage-Ressourcen](./media/versioning-overview/versions-billing-scenario-3.png)

#### <a name="scenario-4"></a>Szenario 4

In Szenario 4 wurde das Basis-Blob vollständig aktualisiert und enthält keinen der ursprünglichen Blöcke. Folglich werden dem Konto alle acht eindeutigen Blöcke in Rechnung gestellt: vier im Basisblob und vier in der vorherigen Version. Dieses Szenario kann eintreten, wenn Sie in ein Blob mit dem Vorgang „Put Blob“ schreiben, da dabei der gesamte Inhalt des Basisblobs ersetzt wird.

![Azure Storage-Ressourcen](./media/versioning-overview/versions-billing-scenario-4.png)

## <a name="see-also"></a>Weitere Informationen

- [Aktivieren von Blobversionsverwaltung](versioning-enable.md)
- [Erstellen einer Momentaufnahme eines Blobs](/rest/api/storageservices/creating-a-snapshot-of-a-blob)
- [Vorläufiges Löschen für Azure Storage-Blobs](storage-blob-soft-delete.md)
