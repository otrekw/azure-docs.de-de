---
title: Bewährte Methoden für das Taggen von Images
description: Bewährte Methoden für das Taggen und die Versionsverwaltung von Docker-Containerimages beim Durchführen von Push- und Pullvorgängen für Images für eine Azure-Containerregistrierung
author: stevelasker
ms.topic: article
ms.date: 07/10/2019
ms.author: stevelas
ms.openlocfilehash: 9dfd29f2265e599f3cc7c412b81c3b7b93af40db
ms.sourcegitcommit: 5fd1f72a96f4f343543072eadd7cdec52e86511e
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/01/2021
ms.locfileid: "106109712"
---
# <a name="recommendations-for-tagging-and-versioning-container-images"></a>Empfehlungen für das Taggen und die Versionsverwaltung von Containerimages

Beim Verschieben von Container-Abbildungen in eine Container-Registry und anschließendem Einsatz benötigen Sie eine Strategie für das Etikettieren und Versionieren von Abbildungen. In diesem Artikel werden zwei verschiedene Verfahren erläutert und beschrieben, welches Verfahren sich an verschiedenen Punkten des Lebenszyklus des Containers am besten eignet:

* **Stabile Tags**: Diese Tags können Sie wiederverwenden, um z.B. eine Haupt- und eine Nebenversion wie *mycontainerimage:1.0* zu bezeichnen.
* **Eindeutige Tags**: Sie verwenden für jedes Image, das Sie in eine Registrierung pushen, ein anderes Tag, z.B. *mycontainerimage:abc123*.

## <a name="stable-tags"></a>Stabile Tags

**Empfehlung:** Verwenden Sie robuste Tags, um **Basisimages** für Ihre Containerbuilds zu verwalten. Vermeiden Sie Bereitstellungen mit stabilen Tags, da diese Tags weiterhin Updates erhalten und dadurch Inkonsistenzen in Produktionsumgebungen verursachen können.

*Stabile Tags* bedeuten, dass ein Entwickler oder ein Buildsystem immer wieder ein Pull für ein bestimmtes Tag ausführen kann, das weiter Updates erhält. „Stabil“ bedeutet nicht, dass die Inhalte unveränderlich sind. Es bedeutet, dass das Image hinsichtlich der Absicht dieser Version stabil sein muss. Damit das Image weiter stabil bleibt, werden möglicherweise Sicherheitspatches oder Frameworkupdates für das Image ausgeführt.

### <a name="example"></a>Beispiel

Ein Frameworkteam liefert Version 1.0 aus. Das Team weiß, dass in Zukunft Updates und Nebenversionen ausgeliefert werden. Um stabile Tags für eine bestimmte Haupt- und Nebenversion zu unterstützen, richtet das Team zwei Sätze stabiler Tags ein.

* `:1`: ein stabiles Tag für die Hauptversion. `1` repräsentiert die „neueste“ Version: 1.*.
* `:1.0`: ein stabiles Tag für Version 1.0, mit dem ein Entwickler Code an Updates von 1.0 binden kann, für den kein Rollforward zu Version 1.1 ausgeführt wird, wenn diese veröffentlicht wird.

Das Team verwendet auch das `:latest`-Tag, das auf das neueste stabile Tag zeigt, unabhängig von der aktuellen Hauptversion.

Wenn Updates für das Basisimage oder irgendeine Art Wartungsupdate des Frameworks verfügbar sind, werden Images mit stabilen Tags auf den neuesten Hash aktualisiert, der das neueste stabile Release dieser Version repräsentiert.

In diesem Fall werden sowohl das Tag für die Hauptversion als auch das Tag für die Nebenversion weiterhin gewartet. Für ein Szenario mit Basisimages bedeutet dies, dass der Imagebesitzer gewartete Images bereitstellen kann.

### <a name="delete-untagged-manifests"></a>Löschen von Manifesten ohne Tags

Wenn ein Image mit einem stabilen Tag aktualisiert wird, wird die Markierung des zuvor markierten Image aufgehoben, was zu einem verwaisten Image führt. Das Manifest des vorherigen Images und dessen eindeutigen Ebenendaten verbleiben in der Registrierung. Zum Verwalten der Registrierungsgröße können Sie regelmäßig nicht markierte Manifeste löschen, die sich aus stabilen Imageaktualisierungen ergeben. Führen Sie beispielsweise eine [automatische Bereinigung](container-registry-auto-purge.md) von nicht markierten Manifesten durch, die älter als eine angegebene Dauer sind, oder legen Sie eine [Aufbewahrungsrichtlinie](container-registry-retention-policy.md) für nicht markierte Manifeste fest.

## <a name="unique-tags"></a>Eindeutige Tags

**Empfehlung:** Verwenden Sie eindeutige Tags für **Bereitstellungen**, insbesondere in einer Umgebung, die sich über mehrere Knoten erstrecken kann. Ihr Ziel ist wahrscheinlich eine gut geplante Bereitstellung konsistenter Komponentenversionen. Wenn Ihr Container neu gestartet wird oder ein Orchestrator horizontal auf weitere Instanzen skaliert, rufen Ihre Hosts nicht versehentlich eine neuere Version ab, die inkonsistent mit den anderen Knoten wäre.

Eindeutige Tags bedeuten ganz einfach, dass jedes Image, das in eine Registrierung gepusht wird, über ein eindeutiges Tag verfügt. Tags werden nicht wiederverwendet. Es gibt verschiedene Muster, die Sie beim Generieren von eindeutigen Tags verwenden können, z.B. die folgenden:

* **Datums-/Uhrzeitstempel**: Dieser Ansatz wird recht häufig verwendet, weil Sie eindeutig erkennen können, wann das Image erstellt wurde. Aber wie erreichen Sie eine Korrelation mit Ihrem Buildsystem? Müssen Sie den Build suchen, der zur gleichen Zeit abgeschlossen wurde? In welcher Zeitzone befinden Sie sich? Sind all Ihre Buildsysteme auf UTC kalibriert?
* **Git-Commit**: Dieser Ansatz funktioniert, solange Sie keine Updates für Basisimages unterstützen. Wenn ein Basisimage aktualisiert wird, verwendet Ihr Buildsystem dennoch den gleichen Git-Commit wie beim vorherigen Build. Das Basisimage weist jedoch neue Inhalte auf. Allgemein gesagt, stellt ein Git-Commit ein *semistabiles* Tag bereit.
* **Manifesthash**: Jedes Containerimage, das in eine Containerregistrierung gepusht wird, ist mit einem Manifest verknüpft, das durch einen eindeutigen SHA-256-Hash identifiziert wird. Ein solcher Hash ist zwar eindeutig, aber lang, schwer zu lesen und nicht mit Ihrer Buildumgebung korreliert.
* **Build-ID**: Diese Option ist möglicherweise die beste, weil sie wahrscheinlich inkrementell ist und Ihnen die Korrelation mit einem bestimmten Build ermöglicht, um alle Artefakte und Protokolle zu finden. Ebenso wie ein Manifesthash ist eine solche ID allerdings möglicherweise für Menschen schwer zu lesen.

  Wenn Ihre Organisation über mehrere Buildsysteme verfügt, besteht eine Variante dieser Option darin, dem Tag den Namen des Buildsystems voranzustellen: `<build-system>-<build-id>`. So lassen sich beispielsweise Builds aus dem Jenkins-Buildsystem des API-Teams vom Azure Pipelines-Buildsystem des Webteams unterscheiden.

### <a name="lock-deployed-image-tags"></a>Sperren von bereitgestellten Imagetags

Als bewährte Methode wird empfohlen, dass Sie alle bereitgestellten Imagetags [sperren](container-registry-image-lock.md), indem Sie das `write-enabled`-Attribut auf `false` festlegen. Diese Vorgehensweise verhindert, dass Sie versehentlich ein Image aus der Registrierung entfernen und Ihre Bereitstellungen möglicherweise unterbrechen. Sie können den Sperrschritt in Ihre Releasepipeline einschließen.

Wenn Sie ein bereitgestelltes Image sperren, können Sie weiterhin andere, nicht bereitgestellte Images aus Ihrer Registrierung entfernen, indem Sie Azure Container Registry-Features verwenden, um die Registrierung beizubehalten. Führen Sie beispielsweise eine [automatische Bereinigung](container-registry-auto-purge.md) von nicht markierten Manifesten oder nicht gesperrten Images durch, die älter als eine angegebene Dauer sind, oder legen Sie eine [Aufbewahrungsrichtlinie](container-registry-retention-policy.md) für nicht markierte Manifeste fest.

## <a name="next-steps"></a>Nächste Schritte

Eine ausführlichere Erläuterung der Konzepte in diesem Artikel finden Sie im Blogbeitrag [Docker Tagging: Best practices for tagging and versioning docker images](https://stevelasker.blog/2018/03/01/docker-tagging-best-practices-for-tagging-and-versioning-docker-images/) (Docker-Markierung: Bewährte Methoden für das Taggen und die Versionsverwaltung von Docker-Images).

Informationen dazu, wie Sie die Leistung und den kosteneffektiven Einsatz Ihrer Azure Container Registry maximieren, finden Sie unter [Bewährte Methoden für Azure Container Registry](container-registry-best-practices.md).

<!-- IMAGES -->


<!-- LINKS - Internal -->

