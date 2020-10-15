---
title: Azure CLI-Voraussetzungen für Azure HPC Cache
description: Einrichtungsschritte, bevor Sie die Azure CLI zum Erstellen oder Ändern einer Azure HPC Cache-Instanz verwenden können
author: ekpgh
ms.service: hpc-cache
ms.topic: how-to
ms.date: 07/08/2020
ms.author: v-erkel
ms.openlocfilehash: 7d0844e699c012d987c23a75e2b0874005cf535a
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/09/2020
ms.locfileid: "87095014"
---
# <a name="set-up-azure-cli-for-azure-hpc-cache"></a>Einrichten der Azure-CLI für Azure HPC Cache

Führen Sie die unten beschriebenen Schritte aus, um die Umgebung vorzubereiten, bevor Sie die Azure CLI zum Erstellen oder Verwalten einer Azure HPC Cache-Instanz verwenden.

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

## <a name="install-azure-cli"></a>Installieren der Azure-Befehlszeilenschnittstelle

Für Azure HPC Cache ist mindestens Version 2.7 der Azure-Befehlszeilenschnittstelle (Azure CLI) erforderlich. Führen Sie `az --version` aus, um die installierte Version und die abhängigen Bibliotheken zu ermitteln. Informationen zum Ausführen einer Installation oder eines Upgrades finden Sie unter [Installieren der Azure CLI](/cli/azure/install-azure-cli).

```azurecli-interactive
az --version
```

### <a name="install-the-azure-hpc-cache-extension"></a>Installieren der Azure HPC Cache-Erweiterung

Azure HPC Cache-Funktionen sind nicht Teil der Hauptcodebasis, sodass Sie auch den Erweiterungsverweis installieren müssen. Befolgen Sie die nachstehenden Anweisungen.

1. Anmelden

   Melden Sie sich mit dem Befehl [az login](/cli/azure/reference-index#az-login) an, falls Sie eine lokal installierte Version der Befehlszeilenschnittstelle verwenden.

    ```azurecli
    az login
    ```

    Führen Sie die in Ihrem Terminal angezeigten Schritte aus, um den Authentifizierungsprozess abzuschließen.

   > [!TIP]
   > Wenn Sie über mehrere Abonnements verfügen, müssen Sie eines auswählen. Nehmen Sie die Auswahl vor, wenn Sie eine Cloud Shell-Sitzung im Azure-Portal starten, oder befolgen Sie die Anweisungen unter [Erste Schritte mit der Azure CLI](/cli/azure/get-started-with-azure-cli#sign-in), um Ihr Abonnement über die Befehlszeile festzulegen.

2. Installieren der Erweiterung für die Azure-Befehlszeilenschnittstelle

   Die Azure HPC Cache-Funktionen werden als Azure CLI-Erweiterung bereitgestellt. Sie sind noch nicht Teil des CLI-Kernpakets. Sie müssen den Erweiterungsverweis installieren, bevor Sie ihn verwenden können.

   Azure CLI-Erweiterungen ermöglichen Ihnen den Zugriff auf experimentelle und vorab veröffentlichte Befehle. Weitere Informationen zu Erweiterungen, u. a. zum Aktualisieren und Deinstallieren, finden Sie unter [Verwenden von Erweiterungen mit der Azure CLI](/cli/azure/azure-cli-extensions-overview).

   Installieren Sie die Erweiterung für Azure HPC Cache, indem Sie den folgenden Befehl ausführen:

    ```azurecli-interactive
    az extension add --name hpc-cache
   ```

## <a name="set-default-resource-group-optional"></a>Festlegen der Standardressourcengruppe (optional)

Die meisten hpc-cache-Befehle erfordern, dass Sie die Ressourcengruppe des Cache übergeben. Mit [az configure](/cli/azure/reference-index#az-configure) können Sie die Standardressourcengruppe festlegen.

## <a name="next-steps"></a>Nächste Schritte

Nachdem Sie die Azure CLI-Erweiterung installiert und sich angemeldet haben, können Sie die Azure CLI verwenden, um Azure HPC Cache-Systeme zu erstellen und zu verwalten.

* [Erstellen einer Azure HPC Cache-Instanz](hpc-cache-create.md)
* [Dokumentation zu den hpc-cache-Befehlen der Azure CLI](/cli/azure/ext/hpc-cache/hpc-cache)
