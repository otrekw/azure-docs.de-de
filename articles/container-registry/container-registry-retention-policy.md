---
title: Richtlinie zur Aufbewahrung von nicht markierten Manifesten
description: Hier erfahren Sie, wie Sie eine Aufbewahrungsrichtlinie in Ihrer Azure Container Registry Premium aktivieren, um nicht markierte Manifeste nach einem definierten Zeitraum automatisch zu löschen.
ms.topic: article
ms.date: 04/26/2021
ms.openlocfilehash: 80e16c4b8cccdda2e171159cca807368b2648e82
ms.sourcegitcommit: 17345cc21e7b14e3e31cbf920f191875bf3c5914
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/19/2021
ms.locfileid: "110062828"
---
# <a name="set-a-retention-policy-for-untagged-manifests"></a>Festlegen einer Aufbewahrungsrichtlinie für nicht markierte Manifeste

In Azure Container Registry haben Sie die Möglichkeit, eine *Aufbewahrungsrichtlinie* für gespeicherte Imagemanifeste ohne zugeordnete Markierungen (*nicht markierte Manifeste*) festzulegen. Ist eine Aufbewahrungsrichtlinie aktiviert, werden nicht markierte Manifeste in der Registrierung nach einer von Ihnen festgelegten Anzahl von Tagen automatisch gelöscht. Mit dieser Funktion wird eine Überfüllung der Registrierung mit nicht mehr benötigten Artefakten vermieden, und Speicherkosten werden gespart. 

Sie können Azure Cloud Shell oder eine lokale Installation der Azure CLI verwenden, um die Beispielbefehle in diesem Artikel auszuführen. Für die lokale Verwendung ist Version 2.0.74 oder höher erforderlich. Führen Sie `az --version` aus, um die Version zu ermitteln. Informationen zum Durchführen einer Installation oder eines Upgrades finden Sie bei Bedarf unter [Installieren der Azure CLI][azure-cli].

Eine Aufbewahrungsrichtlinie für nicht markierte Manifeste ist derzeit eine Previewfunktion von **Premium** Container Registrys. Informationen zu den Tarifen von Container Registry finden Sie unter [Azure Container Registry-Tarife](container-registry-skus.md).

> [!WARNING]
> Legen Sie eine Aufbewahrungsrichtlinie stets mit Bedacht fest, da gelöschte Imagedaten nicht wiederhergestellt werden können. Bei Systemen, die Images nach dem Manifest-Digest pullen (und nicht nach dem Imagenamen), sollten Sie für nicht markierte Manifeste keine Aufbewahrungsrichtlinie festlegen. Wenn Sie Images ohne Tags löschen, hindern Sie diese Systeme daran, die Images aus Ihrer Registrierung zu löschen. Erwägen Sie statt des Pullens nach Manifest die Einführung eines *eindeutigen Tagging*-Schemas. Dies ist eine [bewährte Methode](container-registry-image-tag-version.md).

## <a name="about-the-retention-policy"></a>Informationen zur Aufbewahrungsrichtlinie

Azure Container Registry führt eine Verweiszählung für Manifeste in der Registrierung aus. Wenn ein Manifest nicht markiert ist, wird die Aufbewahrungsrichtlinie überprüft. Wenn eine Aufbewahrungsrichtlinie aktiviert ist, wird ein Manifestlöschvorgang mit einem bestimmten Datum in die Warteschlange eingereiht, das von der Anzahl der Tage abhängt, die in der Richtlinie festgelegt sind.

Ein gesonderter Warteschlangenverwaltungsauftrag verarbeitet ständig Nachrichten und wird nach Bedarf skaliert. Angenommen, Sie entfernen zum Beispiel bei zwei Manifesten die Markierungen, jeweils eine Stunde auseinander, in einer Registrierung mit einer Aufbewahrungsrichtlinie von 30 Tagen. Es würden zwei Nachrichten in die Warteschlange eingereiht. Anschließend würden 30 Tage später, ungefähr 1 Stunde auseinander, die Nachrichten aus der Warteschlange abgerufen und verarbeitet, vorausgesetzt, die Richtlinie ist weiterhin in Kraft.

Wenn das `delete-enabled`-Attribut eines nicht markierten Manifests auf `false` festgelegt ist, wird das Manifest gesperrt und nicht von der Richtlinie gelöscht.

> [!IMPORTANT]
> Die Aufbewahrungsrichtlinie gilt nur für nicht markierte Manifeste mit Zeitstempeln, die *nach* Aktivierung der Richtlinie liegen. Nicht markierte Manifeste in der Registrierung mit früheren Zeitstempeln unterliegen nicht der Richtlinie. Weitere Optionen zum Löschen von Imagedaten finden Sie in den Beispielen unter [Löschen von Containerimages in Azure Container Registry](container-registry-delete.md).

## <a name="set-a-retention-policy---cli"></a>Festlegen einer Aufbewahrungsrichtlinie über die CLI

Im folgenden Beispiel wird gezeigt, wie Sie mithilfe der Azure CLI eine Aufbewahrungsrichtlinie für nicht markierte Manifeste in einer Registrierung festlegen.

### <a name="enable-a-retention-policy"></a>Aktivieren einer Aufbewahrungsrichtlinie

Standardmäßig ist in einer Containerregistrierung keine Aufbewahrungsrichtlinie festgelegt. Führen Sie zum Festlegen oder Aktualisieren einer Aufbewahrungsrichtlinie den Befehl [az acr config retention update][az-acr-config-retention-update] in der Azure CLI aus. Sie können einen Zeitraum zwischen 0 und 365 Tagen angeben, in dem die nicht markierten Manifeste aufbewahrt werden. Wenn Sie keine Angaben machen, legt der Befehl einen Standardwert von sieben Tagen fest. Nach Ablauf der Aufbewahrungsdauer werden alle nicht markierten Manifeste in der Registrierung automatisch gelöscht.

Im folgenden Beispiel wird eine Aufbewahrungsrichtlinie mit einer Aufbewahrungsdauer von 30 Tagen für nicht markierte Manifeste in der Registrierung *myregistry* festgelegt:

```azurecli
az acr config retention update --registry myregistry --status enabled --days 30 --type UntaggedManifests
```

Im folgenden Beispiel wird eine Richtlinie festgelegt, durch die jedes Manifest in der Registrierung gelöscht wird, das keine Markierung aufweist. Erstellen Sie diese Richtlinie, indem Sie eine Aufbewahrungsdauer von 0 Tagen festlegen. 

```azurecli
az acr config retention update \
  --registry myregistry --status enabled \
  --days 0 --type UntaggedManifests
```

### <a name="validate-a-retention-policy"></a>Überprüfen einer Aufbewahrungsrichtlinie

Wenn Sie die vorherige Richtlinie mit einer Aufbewahrungsdauer von 0 Tagen aktivieren, können Sie schnell überprüfen, ob nicht markierte Manifeste gelöscht werden:

1. Pushen Sie ein Testbild `hello-world:latest` in Ihre Registrierung, oder ersetzen Sie ein anderes Testbild Ihrer Wahl.
1. Entfernen Sie die Markierung des Bilds `hello-world:latest`, indem Sie beispielsweise den Befehl [az acr repository untag][az-acr-repository-untag] verwenden. Das nicht markierte Manifest verbleibt in der Registrierung.
    ```azurecli
    az acr repository untag \
      --name myregistry --image hello-world:latest
    ```
1. Innerhalb weniger Sekunden wird das nicht markierte Manifest gelöscht. Sie können den Löschvorgang überprüfen, indem Sie Manifeste im Repository auflisten, z. B. mit dem Befehl [az acr repository show-manifests][az-acr-repository-show-manifests]. Wenn das Testbild das einzige im Repository war, wird das Repository selbst gelöscht.

### <a name="manage-a-retention-policy"></a>Verwalten einer Aufbewahrungsrichtlinie

Führen Sie den Befehl [az acr config retention show][az-acr-config-retention-show] aus, um die in einer Registrierung festgelegte Aufbewahrungsrichtlinie anzuzeigen:

```azurecli
az acr config retention show --registry myregistry
```

Führen Sie den Befehl [az acr config retention update][az-acr-config-retention-update] aus, und legen Sie `--status disabled` fest, um die Aufbewahrungsrichtlinie in einer Registrierung zu deaktivieren:

```azurecli
az acr config retention update \
  --registry myregistry --status disabled \
  --type UntaggedManifests
```

## <a name="set-a-retention-policy---portal"></a>Festlegen einer Aufbewahrungsrichtlinie über das Portal

Sie können eine Aufbewahrungsrichtlinie in einer Registrierung auch über das [Azure-Portal](https://portal.azure.com) festlegen. 

### <a name="enable-a-retention-policy"></a>Aktivieren einer Aufbewahrungsrichtlinie

1. Navigieren Sie zu Ihrer Azure-Containerregistrierung. Wählen Sie unter **Richtlinien** die Option **Aufbewahrung (Vorschau)** aus.
1. Wählen Sie **Aktiviert** als **Status** aus.
1. Geben Sie einen Zeitraum zwischen 0 und 365 Tagen an, in dem die nicht markierten Manifeste aufbewahrt werden. Wählen Sie **Speichern** aus.

![Aktivieren einer Aufbewahrungsrichtlinie im Azure-Portal](media/container-registry-retention-policy/container-registry-retention-policy01.png)

### <a name="disable-a-retention-policy"></a>Deaktivieren einer Aufbewahrungsrichtlinie

1. Navigieren Sie zu Ihrer Azure-Containerregistrierung. Wählen Sie unter **Richtlinien** die Option **Aufbewahrung (Vorschau)** aus.
1. Wählen Sie **Deaktiviert** als **Status** aus. Wählen Sie **Speichern** aus.

## <a name="next-steps"></a>Nächste Schritte

* Hier erfahren Sie mehr zu den Optionen zum [Löschen von Images und Repositorys](container-registry-delete.md) in Azure Container Registry.

* Hier erfahren Sie, wie Sie ausgewählte Images und Manifeste aus einer Registrierung [automatisch bereinigen](container-registry-auto-purge.md).

* Hier erfahren Sie mehr zu den Optionen zum [Sperren von Images und Manifesten](container-registry-image-lock.md) in einer Registrierung.

<!-- LINKS - external -->
[terms-of-use]: https://azure.microsoft.com/support/legal/preview-supplemental-terms/


<!-- LINKS - internal -->
[azure-cli]: /cli/azure/install-azure-cli
[az-acr-config-retention-update]: /cli/azure/acr/config/retention#az_acr_config_retention_update
[az-acr-config-retention-show]: /cli/azure/acr/config/retention#az_acr_config_retention_show
[az-acr-repository-untag]: /cli/azure/acr/repository#az_acr_repository_untag
[az-acr-repository-show-manifests]: /cli/azure/acr/repository#az_acr_repository_show_manifests
