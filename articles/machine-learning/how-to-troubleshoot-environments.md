---
title: Problembehandlung für Umgebungsimages
titleSuffix: Azure Machine Learning
description: Es wird beschrieben, wie Sie die Problembehandlung durchführen, wenn Probleme mit dem Buildvorgang für Umgebungsimages und mit der Paketinstallation auftreten.
services: machine-learning
ms.service: machine-learning
ms.subservice: core
author: saachigopal
ms.author: sagopal
ms.date: 12/3/2020
ms.topic: troubleshooting
ms.custom: devx-track-python
ms.openlocfilehash: b452c24b4b2ed6021910f267b9941f3829acccd8
ms.sourcegitcommit: a89a517622a3886b3a44ed42839d41a301c786e0
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 12/22/2020
ms.locfileid: "97733218"
---
# <a name="troubleshoot-environment-image-builds"></a>Problembehandlung für Buildvorgänge für Umgebungsimages
Es wird beschrieben, wie Sie die Problembehandlung durchführen, wenn Probleme mit dem Buildvorgang für Docker-Umgebungsimages und mit der Paketinstallation auftreten.

## <a name="prerequisites"></a>Voraussetzungen

* Ein **Azure-Abonnement**. Probieren Sie die [kostenlose oder kostenpflichtige Version von Azure Machine Learning](https://aka.ms/AMLFree) aus.
* Das [Azure Machine Learning SDK](/python/api/overview/azure/ml/install?preserve-view=true&view=azure-ml-py).
* Die [Azure CLI](/cli/azure/install-azure-cli?preserve-view=true&view=azure-cli-latest)
* Die [CLI-Erweiterung für Azure Machine Learning](reference-azure-machine-learning-cli.md).
* Zum lokalen Debuggen benötigen Sie eine funktionierende Docker-Installation auf Ihrem lokalen System.

## <a name="docker-image-build-failures"></a>Fehler beim Erstellen eines Docker-Images
 
Bei den meisten Buildfehlern für Images ist die Grundursache im Buildprotokoll des Images zu finden.
Sie finden das Buildprotokoll des Images im Azure Machine Learning-Portal (20\_image\_build\_log.txt) oder in den Ausführungsprotokollen Ihrer ACR-Tasks.
 
In den meisten Fällen ist es einfacher, Fehler lokal zu reproduzieren. Überprüfen Sie, welche Art von Fehler vorliegt, und probieren Sie eines der folgenden `setuptools` aus:

- Conda-Abhängigkeit lokal installieren: `conda install suspicious-dependency==X.Y.Z`
- PIP-Abhängigkeit lokal installieren: `pip install suspicious-dependency==X.Y.Z`
- Gesamte Umgebung materialisieren: `conda create -f conda-specification.yml`

> [!IMPORTANT]
> Stellen Sie sicher, dass die Plattform und der Interpreter auf Ihrer lokalen Computeressource mit den entsprechenden Komponenten auf der Remoteressource übereinstimmen. 

### <a name="timeout"></a>Timeout 
 
Timeoutfehler können für unterschiedliche Netzwerkprobleme auftreten:
- Geringe Internetbandbreite
- Serverprobleme
- Umfangreiche Abhängigkeit, die nicht mit den jeweiligen Conda- oder PIP-Timeouteinstellungen heruntergeladen werden kann
 
Mit Meldungen der folgenden Art wird auf das Problem hingewiesen:
 
```
('Connection broken: OSError("(104, \'ECONNRESET\')")', OSError("(104, 'ECONNRESET')"))
```
```
ReadTimeoutError("HTTPSConnectionPool(host='****', port=443): Read timed out. (read timeout=15)",)
```

Mögliche Lösungen:
 
- Versuchen Sie, eine andere Quelle für die Abhängigkeit zu verwenden, falls eine verfügbar ist. Beispiele hierfür sind Spiegel, Blobspeicher oder andere Python-Feeds.
- Führen Sie ein Update für Conda bzw. PIP durch. Aktualisieren Sie die Timeouteinstellungen, falls eine benutzerdefinierte Docker-Datei verwendet wird.
- Für einige PIP-Versionen bestehen bekannte Probleme. Erwägen Sie, eine bestimmte Version von PIP den Umgebungsabhängigkeiten hinzuzufügen.

### <a name="package-not-found"></a>Paket nicht gefunden

Dies ist der häufigste Fehler, der beim Buildvorgang für Images auftritt.

- Conda-Paket wurde nicht gefunden
        ```
        ResolvePackageNotFound: 
          - not-existing-conda-package
        ```

- Das angegebene PIP-Paket oder die Version wurde nicht gefunden
        ```
        ERROR: Could not find a version that satisfies the requirement invalid-pip-package (from versions: none)
        ERROR: No matching distribution found for invalid-pip-package
        ```

- Ungültige geschachtelte PIP-Abhängigkeit
        ```
        ERROR: No matching distribution found for bad-backage==0.0 (from good-package==1.0)
        ```

Überprüfen Sie, ob das Paket auf den angegebenen Quellen vorhanden ist. Verwenden Sie die [PIP-Suche](https://pip.pypa.io/en/stable/reference/pip_search/), um die PIP-Abhängigkeiten zu überprüfen.

`pip search azureml-core`

Verwenden Sie für Conda-Abhängigkeiten die [Conda-Suche](https://docs.conda.io/projects/conda/en/latest/commands/search.html).

`conda search conda-forge::numpy`

Weitere Optionen:
- `pip search -h`
- `conda search -h`

#### <a name="installer-notes"></a>Hinweise zum Installationsprogramm

Stellen Sie sicher, dass die erforderliche Verteilung für die angegebene Plattform und die Version des Python-Interpreters vorhanden ist.

Navigieren Sie für PIP-Abhängigkeiten zu `https://pypi.org/project/[PROJECT NAME]/[VERSION]/#files`, um zu ermitteln, ob die erforderliche Version verfügbar ist. Zum Beispiel, https://pypi.org/project/azureml-core/1.11.0/#files

Überprüfen Sie für Conda-Abhängigkeiten das Paket im Kanalrepository.
Führen Sie die Überprüfung für Kanäle, die von Anaconda, Inc. verwaltet werden, [hier](https://repo.anaconda.com/pkgs/) durch.

### <a name="pip-package-update"></a>Aktualisierung des PIP-Pakets

Während der Installation oder Aktualisierung eines PIP-Pakets muss der Konfliktlöser ggf. ein Update für ein bereits installiertes Paket durchführen, um die neuen Anforderungen zu erfüllen.
Für die Deinstallation kann aus verschiedenen Gründen ein Fehler auftreten, dessen Ursache die PIP-Version oder die Installationsweise der Abhängigkeit ist.
Das häufigste Szenario ist, dass eine von Conda installierte Abhängigkeit von PIP nicht deinstalliert werden konnte.
Erwägen Sie in diesem Fall, die Abhängigkeit mit `conda remove mypackage` zu deinstallieren.

```
  Attempting uninstall: mypackage
    Found existing installation: mypackage X.Y.Z
ERROR: Cannot uninstall 'mypackage'. It is a distutils installed project and thus we cannot accurately determine which files belong to it which would lead to only a partial uninstall.
``` 
### <a name="installer-issues"></a>Probleme mit dem Installationsprogramm

Für bestimmte Installationsprogrammversionen treten Probleme in den Paketkonfliktlösern auf, die zu einem Buildfehler führen können.

Wenn ein benutzerdefiniertes Basisimage oder ein Dockerfile verwendet wird, empfehlen wir Ihnen, Conda-Version 4.5.4 oder höher zu verwenden.

Das PIP-Paket wird benötigt, um PIP-Abhängigkeiten zu installieren. Falls eine Version in der Umgebung nicht angegeben ist, wird die aktuelle Version verwendet.
Wir empfehlen Ihnen, eine bekannte Version von PIP zu verwenden, um das Auftreten von vorübergehenden Problemen oder Breaking Changes zu vermeiden, die durch die aktuelle Version des Tools verursacht werden.

Erwägen Sie, die PIP-Version in Ihrer Umgebung anzuheften, falls eine der folgenden Meldungen angezeigt wird:

`Warning: you have pip-installed dependencies in your environment file, but you do not list pip itself as one of your conda dependencies. Conda may not use the correct pip to install your packages, and they may end up in the wrong place. Please add an explicit pip dependency. I'm adding one for you, but still nagging you.`

`Pip subprocess error:
ERROR: THESE PACKAGES DO NOT MATCH THE HASHES FROM THE REQUIREMENTS FILE. If you have updated the package versions, update the hashes as well. Otherwise, examine the package contents carefully; someone may have tampered with them.`

Darüber hinaus kann die PIP-Installation auch in einer unendlichen Schleife hängen bleiben, falls für die Abhängigkeiten unlösbare Konflikte bestehen. Wenn Sie lokal arbeiten, sollten Sie für die PIP-Version das Downgrade auf eine frühere Version als 20.3 durchführen. In einer Conda-Umgebung, die aus einer YAML-Datei erstellt wird, tritt dieses Problem nur auf, wenn „conda-forge“ der Kanal mit der höchsten Priorität ist. Geben Sie zum Beheben des Problems in der Conda-Spezifikationsdatei explizit „pip < 20.3“ (!=20.3 oder =20.2.4-Anheftung an andere Version) als Conda-Abhängigkeit an.

## <a name="service-side-failures"></a>Dienstseitige Fehler

### <a name="unable-to-pull-image-from-mcraddress-could-not-be-resolved-for-container-registry"></a>Der Fehler, bei dem das Pullen des Images aus MCR bzw. der Adresse nicht möglich war, konnte für Container Registry nicht behoben werden.
Mögliche Probleme:
- Der Pfadname für die Containerregistrierung wird ggf. nicht richtig aufgelöst. Überprüfen Sie, ob für Imagenamen doppelte Schrägstriche verwendet werden und ob die Richtung der Schrägstriche auf den Linux- bzw. Windows-Hosts korrekt ist.
- Wenn für die ACR-Instanz hinter einem VNET ein privater Endpunkt in einer [nicht unterstützten Region](https://docs.microsoft.com/azure/private-link/private-link-overview#availability) verwendet wird, sollten Sie die hinter dem VNET befindliche ACR-Instanz mit dem Dienstendpunkt (öffentlicher Zugriff) über das Portal konfigurieren und den Vorgang dann wiederholen.
- Stellen Sie nach dem Anordnen der ACR-Instanz hinter einem VNET sicher, dass die [ARM-Vorlage](https://docs.microsoft.com/azure/machine-learning/how-to-enable-virtual-network#azure-container-registry) ausgeführt wird. Auf diese Weise kann der Arbeitsbereich mit der ACR-Instanz kommunizieren.

### <a name="401-error-from-workspace-acr"></a>Fehler 401 für ACR-Instanz des Arbeitsbereichs
Führen Sie die erneute Synchronisierung von Speicherschlüsseln durch, indem Sie [ws.sync_keys()](https://docs.microsoft.com/python/api/azureml-core/azureml.core.workspace.workspace?view=azure-ml-py#sync-keys--) verwenden.

### <a name="environment-keeps-throwing-waiting-for-other-conda-operations-to-finish-error"></a>Fehler der Art „Warten auf den Abschluss anderer Conda-Vorgänge…“ in der Umgebung
Wenn gerade ein Buildvorgang für ein Image durchgeführt wird, wird Conda vom SDK-Client gesperrt. Falls der Prozess abgestürzt ist oder vom Benutzer nicht richtig abgebrochen wurde, verbleibt Conda im gesperrten Zustand. Um dieses Problem zu beheben, müssen Sie die Sperrdatei manuell löschen. 

### <a name="custom-docker-image-not-in-registry"></a>Benutzerdefiniertes Docker-Image nicht in der Registrierung vorhanden
Überprüfen Sie, ob das [richtige Tag](https://docs.microsoft.com/azure/machine-learning/how-to-use-environments#create-an-environment) verwendet wird und Folgendes gilt: `user_managed_dependencies = True`. Mit `Environment.python.user_managed_dependencies = True` wird Conda deaktiviert, und es werden die installierten Pakete des Benutzers verwendet.

### <a name="common-vnet-issues"></a>Häufige VNET-Probleme

1. Überprüfen Sie, ob sich das Speicherkonto, der Computecluster und die Azure Container Registry-Instanz in demselben Subnetz des virtuellen Netzwerks befinden.
2. Wenn die ACR-Instanz hinter einem VNET angeordnet ist, kann sie nicht direkt zum Erstellen von Images genutzt werden. Der Computecluster muss verwendet werden, um den Buildvorgang für Images durchzuführen.
3. Speicher muss ggf. hinter einem VNET angeordnet werden, wenn Folgendes gilt:
    - Verwendung von Rückschlüssen oder Private Wheel
    - Auftreten von Dienstfehlern vom Typ „403: Nicht autorisiert“
    - Kein Abruf der Imagedetails aus ACR/MCR möglich

### <a name="image-build-fails-when-trying-to-access-network-protected-storage"></a>Fehler beim Buildvorgang für das Image, wenn auf den geschützten Netzwerkspeicher zugegriffen werden soll
- ACR-Aufgaben funktionieren hinter dem VNET nicht. Falls der Benutzer seine ACR-Instanz hinter dem VNET angeordnet hat, muss er den Computecluster zum Erstellen eines Images verwenden.
- Speicher sollte sich hinter einem VNET befinden, damit dafür das Pullen von Abhängigkeiten möglich ist. 

### <a name="cannot-run-experiments-when-storage-has-network-security-enabled"></a>Ausführung von Experimenten nicht möglich, wenn für Speicher die Netzwerksicherheit aktiviert ist
Wenn Sie Docker-Standardimages nutzen und vom Benutzer verwaltete Abhängigkeiten aktiviert haben, müssen Sie die [Diensttags](https://docs.microsoft.com/azure/machine-learning/how-to-enable-virtual-network) „MicrosoftContainerRegistry“ und „AzureFrontDoor.FirstParty“ verwenden, um MCR und die zugehörigen Abhängigkeiten auf die Positivliste zu setzen.

 Weitere Informationen finden Sie im Artikel zum [Aktivieren von virtuellen Netzwerken](https://docs.microsoft.com/azure/machine-learning/how-to-enable-virtual-network#azure-container-registry).

### <a name="creating-an-icm"></a>Erstellen eines IcM-Tickets

Fügen Sie beim Erstellen bzw. Zuweisen eines IcM-Tickets zu Metastore das CSS-Supportticket hinzu, damit wir das Problem besser verstehen können.

## <a name="next-steps"></a>Nächste Schritte

- [Trainieren von Scikit-learn-Modellen im großen Stil mit Azure Machine Learning](how-to-train-scikit-learn.md)
- [Trainieren eines Modells mithilfe eines benutzerdefinierten Docker-Images](how-to-train-with-custom-image.md)