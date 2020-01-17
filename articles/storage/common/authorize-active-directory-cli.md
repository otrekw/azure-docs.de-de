---
title: Ausführen von Azure CLI-Befehlen mit Azure AD-Anmeldeinformationen für den Zugriff auf Blob- oder Warteschlangendaten
titleSuffix: Azure Storage
description: Die Azure-Befehlszeilenschnittstelle (Azure CLI) unterstützt die Anmeldung mit Azure AD-Anmeldeinformationen zum Ausführen von Befehlen für Daten in Azure Storage-Blobs und -Warteschlangen. Ein Zugriffstoken wird jeweils für die Sitzung bereitgestellt und zum Autorisieren von Aufrufvorgängen verwendet. Die Berechtigungen hängen von der RBAC-Rolle (Role-Based Access Control = rollenbasierte Zugriffssteuerung) ab, die dem Azure AD-Sicherheitsprinzipal zugewiesen ist.
services: storage
author: tamram
ms.service: storage
ms.topic: how-to
ms.date: 12/30/2019
ms.author: tamram
ms.reviewer: cbrooks
ms.subservice: common
ms.openlocfilehash: bfbb466b7679e841c9c0154dbe28de14574d4282
ms.sourcegitcommit: ec2eacbe5d3ac7878515092290722c41143f151d
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 12/31/2019
ms.locfileid: "75553263"
---
# <a name="run-azure-cli-commands-with-azure-ad-credentials-to-access-blob-or-queue-data"></a>Ausführen von Azure CLI-Befehlen mit Azure AD-Anmeldeinformationen für den Zugriff auf Blob- oder Warteschlangendaten

Azure Storage bietet Erweiterungen für die Azure-Befehlszeilenschnittstelle, mit deren Hilfe Sie sich mit Azure Active Directory-Anmeldeinformationen (Azure AD) anmelden und Skriptbefehle ausführen können. Wenn Sie sich mit Azure AD-Anmeldeinformationen bei der Azure-Befehlszeilenschnittstelle anmelden, wird ein OAuth 2.0-Zugriffstoken zurückgegeben. Dieses Token wird dann automatisch von der Befehlszeilenschnittstelle verwendet, um nachfolgende Vorgänge für Blob- oder Queue Storage-Daten zu autorisieren. Für unterstützte Vorgänge müssen Sie mit dem Befehl keinen Kontoschlüssel und kein SAS-Token mehr übergeben.

Sie können einem Azure AD-Sicherheitsprinzipal über die rollenbasierte Zugriffssteuerung (RBAC) Berechtigungen für Blob- und Warteschlangendaten zuweisen. Weitere Informationen zu RBAC-Rollen in Azure Storage finden Sie unter [Verwalten der Zugriffsrechte für Azure Storage-Daten mit RBAC](storage-auth-aad-rbac.md).

## <a name="supported-operations"></a>Unterstützte Vorgänge

Die Azure Storage-Erweiterungen werden für Vorgänge für Blob- und Warteschlangendaten unterstützt. Welche Vorgänge Sie aufrufen können, hängt von den Berechtigungen des Azure AD-Sicherheitsprinzipals ab, mit dem Sie sich bei der Azure-Befehlszeilenschnittstelle anmelden. Die Berechtigungen für Azure Storage-Container oder -Warteschlangen werden über die rollenbasierte Zugriffssteuerung zugewiesen. Wenn Sie beispielsweise die Rolle **Blobdatenleser** zugewiesen haben, können Sie Skriptbefehle ausführen, die Daten aus einem Container oder einer Warteschlange lesen. Haben Sie die Rolle **Mitwirkender an Blobdaten** zugewiesen, können Sie Skriptbefehle ausführen, die einen Container oder eine Warteschlange bzw. die darin enthaltenen Daten lesen, schreiben oder löschen.

Einzelheiten zu den Berechtigungen, die für die einzelnen Azure Storage-Vorgänge in einem Container oder einer Warteschlange erforderlich sind, finden Sie unter [Call storage operations with OAuth tokens](/rest/api/storageservices/authorize-with-azure-active-directory#call-storage-operations-with-oauth-tokens) (Aufrufen von Speichervorgängen mit OAuth-Tokens).  

## <a name="call-azure-cli-commands-using-azure-ad-credentials"></a>Aufrufen von Azure-Befehlszeilenschnittstellenbefehlen mithilfe von Azure AD-Anmeldeinformationen

Die Azure-Befehlszeilenschnittstelle unterstützt den Parameter `--auth-mode` für Datenvorgänge in Blobs und Warteschlangen:

- Legen Sie den Parameter `--auth-mode` auf `login` fest, um sich mit einem Azure AD-Sicherheitsprinzipal anzumelden.
- Legen Sie den Parameter `--auth-mode` auf den älteren Wert `key` fest, um zu versuchen, einen Kontoschlüssel abzurufen, wenn keine Authentifizierungsparameter für das Konto bereitgestellt werden.

Im folgenden Beispiel sehen Sie, wie in einem neuen Speicherkonto mithilfe Ihrer Azure AD-Anmeldeinformationen über die Azure-Befehlszeilenschnittstelle ein Container erstellt wird. Denken Sie daran, die Platzhalterwerte in eckigen Klammern durch Ihre eigenen Werte zu ersetzen:

1. Vergewissern Sie sich, dass mindestens Version 2.0.46 der Azure-Befehlszeilenschnittstelle installiert ist. Führen Sie `az --version` aus, um Ihre installierte Version zu überprüfen.

1. Führen Sie `az login` aus, und authentifizieren Sie sich im Browserfenster:

    ```azurecli
    az login
    ```

1. Geben Sie das gewünschte Abonnement an. Erstellen Sie mit [az group create](https://docs.microsoft.com/cli/azure/group?view=azure-cli-latest#az-group-create) eine Ressourcengruppe. Erstellen Sie mithilfe des Befehls [az storage account create](https://docs.microsoft.com/cli/azure/storage/account?view=azure-cli-latest#az-storage-account-create) ein Speicherkonto in dieser Ressourcengruppe:

    ```azurecli
    az account set --subscription <subscription-id>

    az group create \
        --name sample-resource-group-cli \
        --location eastus

    az storage account create \
        --name <storage-account> \
        --resource-group sample-resource-group-cli \
        --location eastus \
        --sku Standard_LRS \
        --encryption-services blob
    ```

1. Weisen Sie sich vor der Erstellung des Containers selbst die Rolle [Mitwirkender an Storage-Blobdaten](../../role-based-access-control/built-in-roles.md#storage-blob-data-contributor) zu. Obwohl Sie der Kontobesitzer sind, benötigen Sie explizite Berechtigungen, um Datenvorgänge in Ihrem Speicherkonto ausführen zu können. Weitere Informationen zum Zuweisen von RBAC-Rollen finden Sie unter [Gewähren von Zugriff auf Azure-Blob- und -Warteschlangendaten mit RBAC über das Azure-Portal](storage-auth-aad-rbac.md).

    > [!IMPORTANT]
    > Die RBAC-Rollenzuweisungen können einige Minuten dauern.

1. Rufen Sie mithilfe des `--auth-mode`-Parameters, für den `login` festgelegt wurde, den Befehl [az storage container create](https://docs.microsoft.com/cli/azure/storage/container?view=azure-cli-latest#az-storage-container-create) auf, um den Container mithilfe Ihrer Azure AD-Anmeldeinformationen zu erstellen:

    ```azurecli
    az storage container create \ 
        --account-name <storage-account> \ 
        --name sample-container \
        --auth-mode login
    ```

Dem Parameter `--auth-mode` ist die Umgebungsvariable `AZURE_STORAGE_AUTH_MODE` zugeordnet. Sie können den passenden Wert in der Umgebungsvariable angeben, um zu umgehen, ihn in jedem Aufruf eines Azure Storage-Datenvorgangs einschließen zu müssen.

## <a name="next-steps"></a>Nächste Schritte

- [Verwenden der Azure-Befehlszeilenschnittstelle zum Zuweisen einer RBAC-Rolle für den Zugriff auf Blob- und Warteschlangendaten](storage-auth-aad-rbac-cli.md)
- [Autorisieren des Zugriffs auf Blob- und Warteschlangendaten mit verwalteten Identitäten für Azure-Ressourcen](storage-auth-aad-msi.md)
