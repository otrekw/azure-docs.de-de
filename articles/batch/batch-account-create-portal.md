---
title: Erstellen eines Kontos über das Azure-Portal
description: Erfahren Sie, wie Sie ein Azure Batch-Konto im Azure-Portal erstellen, um umfangreiche parallele Workloads in der Cloud auszuführen.
ms.topic: how-to
ms.date: 06/10/2020
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 1205de2b800588b735aeb20d388ba4b64bc6b078
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/02/2020
ms.locfileid: "84711339"
---
# <a name="create-a-batch-account-with-the-azure-portal"></a>Erstellen eines Batch-Kontos mit dem Azure-Portal

In diesem Artikel erfahren Sie, wie Sie im [Azure-Portal](https://portal.azure.com) ein Azure Batch-Konto mit den geeigneten Kontoeigenschaften für Ihr Computeszenario erstellen. Außerdem erfahren Sie, wo Sie wichtige Kontoeigenschaften wie Zugriffsschlüssel und Konto-URLs finden.

Hintergrundinformationen zu Batch-Konten und -Szenarien finden Sie unter [Workflow und Ressourcen des Batch-Diensts](batch-service-workflow-features.md).

## <a name="create-a-batch-account"></a>Erstellen eines Batch-Kontos

[!INCLUDE [batch-account-mode-include](../../includes/batch-account-mode-include.md)]

1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com) an.

1. Klicken Sie auf **Ressource erstellen**, auf **Compute** und anschließend auf **Batch-Dienst**.

1. Geben Sie unter **Neues Batch-Konto** die Einstellungen ein. Orientieren Sie sich an den folgenden Details:

    ![Erstellen eines Batch-Kontos][account_portal]

    a. **Abonnement**: Das Abonnement, in dem das Batch-Konto erstellt werden soll. Wenn Sie nur über ein Abonnement verfügen, ist es standardmäßig ausgewählt.

    b. **Ressourcengruppe**: Eine vorhandene Ressourcengruppe für Ihr neues Batch-Konto. Optional können Sie auch eine neue Ressourcengruppe erstellen.

    c. **Kontoname**: Der ausgewählte Name muss innerhalb der Azure-Region, in der das Konto erstellt wird, eindeutig sein (siehe **Standort** weiter unten). Der Kontoname darf nur Kleinbuchstaben und Zahlen enthalten und muss 3 bis 24 Zeichen lang sein.

    d. **Standort**: Die Azure-Region, in der das Batch-Konto erstellt werden soll. Nur die von Ihrem Abonnement und der Ressourcengruppe unterstützten Regionen werden als Optionen angezeigt.

    e. **Speicherkonto**: Ein optionales Azure Storage-Konto, das Sie dem Batch-Konto zuordnen. Die beste Leistung erzielen Sie mit einem Speicherkonto vom Typ „Allgemein v2“. Informationen zu allen Speicherkontooptionen in Batch finden Sie unter [Entwickeln von parallelen Computelösungen in größerem Umfang mit Batch](accounts.md#azure-storage-accounts). Wählen Sie im Portal ein vorhandenes Speicherkonto aus, oder erstellen Sie ein neues.

      ![Speicherkonto erstellen][storage_account]

    f. **Poolzuordnungsmodus**: Auf der Registerkarte mit den **erweiterten** Einstellungen können Sie **Batch-Dienst** oder **Benutzerabonnement** als Poolzuordnungsmodus angeben. In den meisten Szenarien übernehmen Sie die Standardeinstellung **Batch-Dienst**.

      ![Batch-Poolzuordnungsmodus][pool_allocation]

1. Wählen Sie **Erstellen**, um das Konto zu erstellen.

## <a name="view-batch-account-properties"></a>Anzeigen der Eigenschaften des Batch-Kontos

Klicken Sie nach der Kontoerstellung auf das Konto, um auf dessen Einstellungen und Eigenschaften zuzugreifen. Über das linke Menü können Sie auf alle Kontoeinstellungen und -eigenschaften zugreifen.

> [!NOTE]
> Der Name des Batch-Kontos ist seine ID und kann nicht geändert werden. Wenn Sie den Namen eines Batch-Kontos ändern müssen, müssen Sie das Konto löschen und ein neues mit dem gewünschten Namen erstellen.

![Seite „Batch-Konto“ im Azure-Portal][account_blade]

* **Name, URL und Schlüssel des Batch-Kontos**: Wenn Sie eine Anwendung mit den [Batch-APIs](batch-apis-tools.md#azure-accounts-for-batch-development) entwickeln, benötigen Sie eine Konto-URL und einen Schlüssel für den Zugriff auf Ihre Batch-Ressourcen. (Batch unterstützt auch die Azure Active Directory-Authentifizierung.)

    Klicken Sie zum Anzeigen der Zugriffsinformationen für das Batch-Konto auf **Schlüssel**.

    ![Batch-Kontoschlüssel im Azure-Portal][account_keys]

* Klicken Sie auf **Speicherkonto**, um den Namen und die Schlüssel des mit Ihrem Batch-Konto verknüpften Speicherkontos anzuzeigen.

* Klicken Sie zum Anzeigen der für das Batch-Konto geltenden Ressourcenkontingente auf **Kontingente**. Ausführliche Informationen finden Sie unter [Batch-Dienst – Kontingente und Limits](batch-quota-limit.md).

## <a name="additional-configuration-for-user-subscription-mode"></a>Zusätzliche Konfiguration für den Benutzerabonnementmodus

Wenn Sie ein Batch-Konto im Benutzerabonnementmodus erstellen möchten, führen Sie vor der Erstellung des Kontos die folgenden zusätzlichen Schritte aus.

### <a name="allow-azure-batch-to-access-the-subscription-one-time-operation"></a>Gewähren des Zugriffs auf Ihr Abonnement für Azure Batch (einmaliger Vorgang)

Wenn Sie Ihr erstes Batch-Konto im Modus „Benutzerabonnement“ erstellen, müssen Sie Ihr Abonnement bei Batch registrieren. (Falls Sie diesen Schritt bereits ausgeführt haben, fahren Sie mit dem nächsten Abschnitt fort.)

1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com) an.

1. Klicken Sie auf **Alle Dienste** > **Abonnements** und anschließend auf das Abonnement, das Sie für das Batch-Konto verwenden möchten.

1. Klicken Sie auf der Seite **Abonnement** auf **Ressourcenanbieter**, und suchen Sie nach **Microsoft.Batch**. Überprüfen Sie, ob der Ressourcenanbieter **Microsoft.Batch** im Abonnement registriert ist. Wenn er nicht registriert ist, klicken Sie auf den Link **Registrieren**.

    ![Registrieren des Microsoft.Batch-Anbieters][register_provider]

1. Wählen Sie auf der Seite **Abonnement** die Optionen **Zugriffssteuerung (IAM)**  > **Rollenzuweisungen** > **Rollenzuweisung hinzufügen** aus.

    ![Abonnementzugriffssteuerung][subscription_access]

1. Wählen Sie auf der Seite **Rollenzuweisung hinzufügen** die Rolle **Mitwirkender** oder **Besitzer** aus, und suchen Sie die Batch-API. Suchen Sie diese Zeichenfolgen, bis Sie die API gefunden haben:
    1. **MicrosoftAzureBatch**.
    1. **Microsoft Azure Batch**. Neuere Azure AD-Mandanten verwenden diesen Namen unter Umständen.
    1. **ddbf3205-c6bd-46ae-8127-60eb93363864** ist die ID für die Batch-API.

1. Wenn Sie die Batch-API gefunden haben, wählen Sie diese aus, und klicken Sie auf **Speichern**.

    ![Hinzufügen von Batch-Berechtigungen][add_permission]

### <a name="create-a-key-vault"></a>Erstellen eines Schlüsseltresors

Im Modus „Benutzerabonnement“ ist ein [Azure-Schlüsseltresor](../key-vault/general/overview.md) erforderlich. Der Schlüsseltresor muss sich im gleichen Abonnement und in der gleichen Region befinden wie das Batch-Konto, das erstellt werden soll. 

1. Wählen Sie im [Azure-Portal](https://portal.azure.com)**Neu** > **Sicherheit** > **Key Vault** aus.

1. Geben Sie auf der Seite **Schlüsseltresor erstellen** einen Namen für den Schlüsseltresor ein. Erstellen Sie eine Ressourcengruppe in der Region, in der sich das Batch-Konto befinden soll. Behalten Sie bei den übrigen Einstellungen die Standardwerte bei, und klicken Sie auf **Erstellen**.

Geben Sie beim Erstellen des Batch-Kontos im Modus „Benutzerabonnement“ die Option **Benutzerabonnement** als Poolzuordnungsmodus an. Wählen Sie den Schlüsseltresor aus, und aktivieren Sie das Kontrollkästchen, um Azure Batch Zugriff auf den Schlüsseltresor zu gewähren.

Wenn Sie den Zugriff auf den Schlüsseltresor lieber manuell gewähren möchten, wechseln Sie zum Abschnitt **Zugriffsrichtlinien** des Schlüsseltresors. Wählen Sie **Zugriffsrichtlinie hinzufügen** aus, und suchen Sie nach **Microsoft Azure Batch**. Nach Auswahl dieser Option müssen Sie mithilfe des Dropdownmenüs die **Berechtigungen für Geheimnis** konfigurieren. Azure Batch muss mindestens die Berechtigungen **Abrufen**, **Auflisten**, **Festlegen** und **Löschen** erhalten.

![Berechtigungen für Geheimnis für Azure Batch](./media/batch-account-create-portal/secret-permissions.png)

> [!NOTE]
> Stellen Sie sicher, dass unter **Zugriffsrichtlinien** für die verknüpfte **Key Vault**-Ressource die Kontrollkästchen **Azure Virtual Machines für Bereitstellung** und **Azure Resource Manager für Vorlagenbereitstellung** aktiviert sind.
>
> ![Obligatorische Schlüsseltresor-Zugriffsrichtlinie](./media/batch-account-create-portal/key-vault-access-policy.png)

### <a name="configure-subscription-quotas"></a>Konfigurieren von Abonnementkontingenten

Für Batch-Konten im Modus „Benutzerabonnement“ müssen Kernspeicherkontingente manuell festgelegt werden. Batch-Standardkernkontingente gelten nicht für Konten im Modus „Benutzerabonnement“.

1. Wählen Sie im [Azure-Portal](https://portal.azure.com) Ihr Batch-Konto im Benutzerabonnementmodus aus, um die dazugehörigen Einstellungen und Eigenschaften anzuzeigen.
1. Wählen Sie im linken Menü die Option **Kontingente** aus, um die Kernkontingente für Ihr Batch-Konto anzuzeigen und zu konfigurieren.

Weitere Informationen zu Kernspeicherkontingenten für den Modus „Benutzerabonnement“ finden Sie unter [Batch-Dienst – Kontingente und Limits](batch-quota-limit.md).

## <a name="other-batch-account-management-options"></a>Weitere Optionen für die Verwaltung von Batch-Konten

Neben der Verwendung des Azure-Portals stehen Ihnen zum Erstellen und Verwalten von Batch-Konten die folgenden Tools zur Verfügung:

* [Batch-PowerShell-Cmdlets](batch-powershell-cmdlets-get-started.md)
* [Azure-Befehlszeilenschnittstelle](batch-cli-get-started.md)
* [Batch Management .NET](batch-management-dotnet.md)

## <a name="next-steps"></a>Nächste Schritte

* Erfahren Sie mehr über den [Workflow des Batch-Diensts und primäre Ressourcen](batch-service-workflow-features.md) wie Pools, Knoten, Aufträge und Aufgaben.
* Informieren Sie sich über die Grundlagen der Entwicklung einer Batch-fähigen Anwendung mit der [Batch-.NET-Clientbibliothek](quick-run-dotnet.md) oder mit [Python](quick-run-python.md). In diesen Schnellstarts werden Sie durch eine Beispielanwendung geführt, die den Batch-Dienst zum Ausführen einer Workload auf mehreren Computeknoten verwendet und Azure Storage zum Bereitstellen und Abrufen von Workloaddateien nutzt.

[account_blade]: ./media/batch-account-create-portal/batch_blade.png
[account_portal]: ./media/batch-account-create-portal/batch-account-portal.png
[pool_allocation]: ./media/batch-account-create-portal/batch-pool-allocation.png
[account_keys]: ./media/batch-account-create-portal/batch-account-keys.png
[storage_account]: ./media/batch-account-create-portal/storage_account.png
[subscription_access]: ./media/batch-account-create-portal/subscription_iam.png
[add_permission]: ./media/batch-account-create-portal/add_permission.png
[register_provider]: ./media/batch-account-create-portal/register_provider.png
