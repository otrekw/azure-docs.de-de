---
title: 'Bicep-Befehlszeilenschnittstelle: Befehle und Übersicht'
description: Hier werden die Befehle beschrieben, die Sie in der Bicep-Befehlszeilenschnittstelle verwenden können. Diese Befehle umfassen das Erstellen von Azure Resource Manager-Vorlagen über Bicep.
ms.topic: conceptual
ms.date: 06/01/2021
ms.openlocfilehash: dd1f292d4ce60353d2f8cecaaa83e38b26bdfaa3
ms.sourcegitcommit: bd65925eb409d0c516c48494c5b97960949aee05
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/06/2021
ms.locfileid: "111540812"
---
# <a name="bicep-cli-commands"></a>Befehle der Bicep-Befehlszeilenschnittstelle

In diesem Artikel werden die Befehle beschrieben, die Sie in der Bicep-Befehlszeilenschnittstelle verwenden können. Zum Ausführen der Befehle muss die [Bicep-Befehlszeilenschnittstelle installiert](./install.md) sein.

In diesem Artikel wird gezeigt, wie Sie die Befehle in der Azure CLI ausführen. Wenn Sie nicht die Azure CLI verwenden, lassen Sie bei der Ausführung `az` am Anfang des Befehls weg. `az bicep version` wird beispielsweise zu ``bicep version``.

## <a name="build"></a>build

Der Befehl `build` konvertiert eine Bicep-Datei in eine ARM-Vorlage (Azure Resource Manager). In der Regel müssen Sie diesen Befehl nicht ausführen, da er bei der Bereitstellung einer Bicep-Datei automatisch ausgeführt wird. Führen Sie ihn manuell aus, wenn Sie den JSON-Code der ARM-Vorlage sehen möchten, der auf der Grundlage Ihrer Bicep-Datei erstellt wird.

Im folgenden Beispiel wird eine Bicep-Datei mit dem Namen _main.bicep_ in eine ARM-Vorlage mit dem Namen _main.json_ konvertiert. Die neue Datei wird im gleichen Verzeichnis wie die Bicep-Datei erstellt.

```azurecli
az bicep build --file main.bicep
```

Im nächsten Beispiel wird _main.json_ in einem anderen Verzeichnis gespeichert.

```azurecli
 az bicep build --file main.bicep --outdir c:\jsontemplates
```

Im nächsten Beispiel werden der Name und der Speicherort der zu erstellenden Datei angegeben.

```azurecli
az bicep build --file main.bicep --outfile c:\jsontemplates\azuredeploy.json
```

Verwenden Sie zum Ausgeben der Datei in `stdout` Folgendes:

```azurecli
az bicep build --file main.bicep --stdout
```

## <a name="decompile"></a>decompile

Der Befehl `decompile` konvertiert den JSON-Code der ARM-Vorlage in eine Bicep-Datei.

```azurecli
az bicep decompile --file main.json
```

Weitere Informationen zur Verwendung dieses Befehls finden Sie unter [Dekompilieren von ARM-Vorlagen-JSON-Code in Bicep](decompile.md).

## <a name="install"></a>Installieren

Mit dem Befehl `install` wird die Bicep-Befehlszeilenschnittstelle Ihrer lokalen Umgebung hinzugefügt. Weitere Informationen finden Sie unter [Installieren von Bicep-Tools](install.md).

Verwenden Sie zur Installation der aktuellen Version Folgendes:

```azurecli
az bicep install
```

So installieren Sie eine bestimmte Version

```azurecli
az bicep install --version v0.3.255
```

## <a name="list-versions"></a>list-versions

Mit dem Befehl `list-versions` werden alle verfügbaren Versionen der Bicep-Befehlszeilenschnittstelle zurückgegeben. Verwenden Sie diesen Befehl, um zu sehen, ob Sie ein [Upgrade durchführen](#upgrade) oder eine neue Version [installieren](#install) sollten.

```azurecli
az bicep list-versions
```

Der Befehl gibt ein Array verfügbarer Versionen zurück.

```azurecli
[
  "v0.4.1",
  "v0.3.539",
  "v0.3.255",
  "v0.3.126",
  "v0.3.1",
  "v0.2.328",
  "v0.2.317",
  "v0.2.212",
  "v0.2.59",
  "v0.2.14",
  "v0.2.3",
  "v0.1.226-alpha",
  "v0.1.223-alpha",
  "v0.1.37-alpha",
  "v0.1.1-alpha"
]
```

## <a name="upgrade"></a>upgrade

Der Befehl `upgrade` aktualisiert Ihre installierte Version mit der neuesten Version.

```azurecli
az bicep upgrade
```

## <a name="version"></a>version

Der Befehl `version` gibt die installierte Version zurück.

```azurecli
az bicep version
```

Mit dem Befehl wird die Versionsnummer angezeigt.

```azurecli
Bicep CLI version 0.4.1 (e2387595c9)
```

Wenn Sie die Bicep-Befehlszeilenschnittstelle nicht installiert haben, wird ein Fehler mit dem Hinweis angezeigt, dass die Bicep-Befehlszeilenschnittstelle nicht gefunden wurde.

## <a name="next-steps"></a>Nächste Schritte

Informationen zum Bereitstellen einer Bicep-Datei finden Sie unter folgenden Links:

* [Azure-Befehlszeilenschnittstelle](deploy-cli.md)
* [Cloud Shell](deploy-cloud-shell.md)
* [PowerShell](deploy-powershell.md)
