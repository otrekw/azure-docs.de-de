---
title: Verwenden privater Endpunkte zum Integrieren von Azure Functions in ein virtuelles Netzwerk
description: Ein Tutorial mit detaillierten Anweisungen, in dem erläutert wird, wie Sie eine Funktion mit einem virtuellen Azure-Netzwerk verbinden und mithilfe privater Endpunkte sperren.
ms.topic: article
ms.date: 2/22/2021
ms.openlocfilehash: a7bad58167009b4089724165813eb061996f1e6b
ms.sourcegitcommit: dda0d51d3d0e34d07faf231033d744ca4f2bbf4a
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/05/2021
ms.locfileid: "102200205"
---
# <a name="tutorial-integrate-azure-functions-with-an-azure-virtual-network-using-private-endpoints"></a>Tutorial: Integrieren von Azure Functions in ein virtuelles Azure-Netzwerk mithilfe privater Endpunkte

In diesem Tutorial wird erläutert, wie Sie mithilfe von Azure Functions eine Verbindung mit Ressourcen in einem virtuellen Azure-Netzwerk mit privaten Endpunkten herstellen. Sie erstellen eine Funktion mit einem hinter einem virtuellen Netzwerk gesperrten Speicherkonto, die einen Service Bus-Warteschlangentrigger verwendet.

> [!div class="checklist"]
> * Erstellen einer Funktions-App im Premium-Plan
> * Erstellen von Azure-Ressourcen (Service Bus, Speicherkonto, virtuelles Netzwerk)
> * Sperren des Speicherkontos hinter einem privaten Endpunkt
> * Sperren der Service Bus-Instanz hinter einem privaten Endpunkt
> * Bereitstellen einer Funktions-App mit Service Bus- und HTTP-Triggern
> * Sperren der Funktions-App hinter einem privaten Endpunkt
> * Testen, ob die Funktions-App hinter dem virtuellen Netzwerk geschützt ist
> * Bereinigen von Ressourcen

## <a name="create-a-function-app-in-a-premium-plan"></a>Erstellen einer Funktions-App in einem Premium-Tarif

Zunächst erstellen Sie eine .NET-Funktions-App im [Premium-Tarif], da in diesem Tutorial C# verwendet wird. Andere Programmiersprachen werden in Windows auch unterstützt. Dieser Plan ermöglicht eine Skalierung ohne Server und unterstützt gleichzeitig die Integration in ein virtuelles Netzwerk.

1. Wählen Sie im Menü des Azure-Portals oder auf der **Startseite** die Option **Ressource erstellen** aus.

1. Wählen Sie auf der Seite **Neu** die Option **Compute** > **Funktions-App** aus.

1. Verwenden Sie auf der Seite **Grundlagen** die Funktions-App-Einstellungen, die in der folgenden Tabelle angegeben sind:

    | Einstellung      | Vorgeschlagener Wert  | BESCHREIBUNG |
    | ------------ | ---------------- | ----------- |
    | **Abonnement** | Ihr Abonnement | Das Abonnement, unter dem diese neue Funktions-App erstellt wird. |
    | **[Ressourcengruppe](../azure-resource-manager/management/overview.md)** |  *myResourceGroup* | Der Name der neuen Ressourcengruppe, in der die Funktionen-App erstellt wird |
    | **Name der Funktions-App** | Global eindeutiger Name | Der Name, der Ihre neue Funktionen-App bezeichnet Gültige Zeichen sind `a-z` (Groß-/Kleinschreibung nicht beachtet), `0-9` und `-`.  |
    |**Veröffentlichen**| Code | Option zum Veröffentlichen von Codedateien oder eines Docker-Containers. |
    | **Laufzeitstapel** | .NET | In diesem Tutorial wird .NET verwendet. |
    |**Region**| Bevorzugte Region | Wählen Sie eine [Region](https://azure.microsoft.com/regions/) in Ihrer Nähe oder in der Nähe von anderen Diensten aus, auf die Ihre Funktionen zugreifen. |

1. Klicken Sie auf **Weiter: Hosting** aus. Geben Sie auf der Seite **Hosting** die folgenden Einstellungen ein:

    | Einstellung      | Vorgeschlagener Wert  | BESCHREIBUNG |
    | ------------ | ---------------- | ----------- |
    | **[Speicherkonto](../storage/common/storage-account-create.md)** |  Global eindeutiger Name |  Erstellen Sie ein Speicherkonto, das von Ihrer Funktions-App verwendet wird. Speicherkontonamen müssen zwischen 3 und 24 Zeichen lang sein und dürfen nur Zahlen und Kleinbuchstaben enthalten. Sie können auch ein vorhandenes Konto verwenden, das die [Anforderungen an das Speicherkonto](./storage-considerations.md#storage-account-requirements) erfüllen muss. |
    |**Betriebssystem**| Windows | In diesem Tutorial wird Windows verwendet. |
    | **[Plan](./functions-scale.md)** | Premium | Der Hostingplan, der definiert, wie Ihre Ressourcen der Funktionen-App zugewiesen werden Wählen Sie **Premium** aus. Standardmäßig wird ein neuer App Service Plan erstellt. Der Standardwert für **SKU und Größe** ist **EP1**, wobei EP für _Elastisch Premium_ steht. Weitere Informationen finden Sie in der [Liste der Premium-SKUs](./functions-premium-plan.md#available-instance-skus).<br/>Wenn Sie JavaScript-Funktionen im Rahmen eines Premium-Plans ausführen, sollten Sie eine Instanz auswählen, die weniger vCPUs hat. Weitere Informationen finden Sie unter [Auswählen von Premium-Plänen mit Einzelkern](./functions-reference-node.md#considerations-for-javascript-functions).  |

1. Klicken Sie auf **Weiter: Überwachung** aus. Geben Sie auf der Seite **Überwachung** die folgenden Einstellungen ein:

    | Einstellung      | Vorgeschlagener Wert  | BESCHREIBUNG |
    | ------------ | ---------------- | ----------- |
    | **[Application Insights](./functions-monitoring.md)** | Standard | Erstellt eine Application Insights-Ressource mit dem gleichen *App-Namen* in der nächstgelegenen unterstützten Region. Durch die Erweiterung dieser Einstellung können Sie den **neuen Ressourcennamen** ändern oder einen anderen **Standort** in einer [Azure-Region](https://azure.microsoft.com/global-infrastructure/geographies/) zum Speichern Ihrer Daten auswählen. |

1. Wählen Sie **Bewerten + erstellen** aus, um die App-Konfigurationsauswahl zu überprüfen.

1. Überprüfen Sie auf der Seite **Bewerten + erstellen** Ihre Einstellungen, und wählen Sie dann **Erstellen** aus, um die Funktions-App bereitzustellen.

1. Wählen Sie oben rechts im Portal das **Benachrichtigungssymbol** aus, und achten Sie auf die Meldung **Bereitstellung erfolgreich**.

1. Wählen Sie **Zu Ressource wechseln**, um Ihre neue Funktionen-App anzuzeigen. Sie können auch die Option **An Dashboard anheften** auswählen. Wenn Sie die Funktions-App anheften, können Sie einfacher über das Dashboard auf sie zugreifen.

1. Herzlichen Glückwunsch! Sie haben Ihre neue Premium-Funktions-App erfolgreich erstellt.

## <a name="create-azure-resources"></a>Erstellen von Azure-Ressourcen

### <a name="create-a-storage-account"></a>Speicherkonto erstellen

Neben dem bei der anfänglichen Erstellung der Funktions-App erstellten Konto ist ein separates Speicherkonto für virtuelle Netzwerke erforderlich.

1. Wählen Sie im Menü des Azure-Portals oder auf der **Startseite** die Option **Ressource erstellen** aus.

1. Suchen Sie auf der Seite „Neu“ nach **Speicherkonto**, und wählen Sie **Erstellen** aus.

1. Legen Sie auf der Registerkarte **Grundlagen** die in der folgenden Tabelle gezeigten Einstellungen fest. Für die restlichen Optionen können Sie die Standardeinstellungen übernehmen:

    | Einstellung      | Vorgeschlagener Wert  | BESCHREIBUNG      |
    | ------------ | ---------------- | ---------------- |
    | **Abonnement** | Ihr Abonnement | Das Abonnement, in dem Ihre Ressourcen erstellt werden. | 
    | **[Ressourcengruppe](../azure-resource-manager/management/overview.md)**  | myResourceGroup | Wählen Sie die Ressourcengruppe aus, die Sie mit Ihrer Funktions-App erstellt haben. |
    | **Name** | mysecurestorage| Der Name des Speicherkontos, auf das der private Endpunkt angewendet wird. |
    | **[Region](https://azure.microsoft.com/regions/)** | myFunctionRegion | Wählen Sie die Region aus, in der Sie Ihre Funktions-App erstellt haben. |

1. Klicken Sie auf **Überprüfen + erstellen**. Wenn die Überprüfung abgeschlossen ist, wählen Sie **Erstellen** aus.

### <a name="create-a-service-bus"></a>Erstellen einer Service Bus-Instanz

1. Wählen Sie im Menü des Azure-Portals oder auf der **Startseite** die Option **Ressource erstellen** aus.

1. Suchen Sie auf der Seite „Neu“ nach **Service Bus**, und wählen Sie **Erstellen** aus.

1. Legen Sie auf der Registerkarte **Grundlagen** die in der folgenden Tabelle gezeigten Einstellungen fest. Für die restlichen Optionen können Sie die Standardeinstellungen übernehmen:

    | Einstellung      | Vorgeschlagener Wert  | BESCHREIBUNG      |
    | ------------ | ---------------- | ---------------- |
    | **Abonnement** | Ihr Abonnement | Das Abonnement, in dem Ihre Ressourcen erstellt werden. |
    | **[Ressourcengruppe](../azure-resource-manager/management/overview.md)**  | myResourceGroup | Wählen Sie die Ressourcengruppe aus, die Sie mit Ihrer Funktions-App erstellt haben. |
    | **Name** | myServiceBus| Der Name der Service Bus-Instanz, auf die der private Endpunkt angewendet wird. |
    | **[Region](https://azure.microsoft.com/regions/)** | myFunctionRegion | Wählen Sie die Region aus, in der Sie Ihre Funktions-App erstellt haben. |
    | **Preisstufe** | Premium | Wählen Sie diesen Tarif aus, um private Endpunkte mit Service Bus verwenden zu können. |

1. Klicken Sie auf **Überprüfen + erstellen**. Wenn die Überprüfung abgeschlossen ist, wählen Sie **Erstellen** aus.

### <a name="create-a-virtual-network"></a>Erstellen eines virtuellen Netzwerks

Die Azure-Ressourcen in diesem Tutorial werden entweder in ein virtuelles Netzwerk integriert oder in einem virtuellen Netzwerk platziert. Sie verwenden private Endpunkte, um den Netzwerkdatenverkehr auf das virtuelle Netzwerk zu beschränken.

In diesem Tutorial werden zwei Subnetze erstellt:
- **default**: Subnetz für private Endpunkte. Private IP-Adressen werden aus diesem Subnetz vergeben.
- **functions**: Subnetz für die Integration von Azure Functions in das virtuelle Netzwerk. Dieses Subnetz wird an die Funktions-App delegiert.

Erstellen Sie nun das virtuelle Netzwerk, in das die Funktions-App integriert wird.

1. Wählen Sie im Menü des Azure-Portals oder auf der Startseite die Option **Ressource erstellen** aus.

1. Suchen Sie auf der Seite „Neu“ nach **Virtuelles Netzwerk**, und wählen Sie **Erstellen** aus.

1. Legen Sie auf der Registerkarte **Grundlagen** die unten gezeigten Einstellungen für das virtuelle Netzwerk fest:

    | Einstellung      | Vorgeschlagener Wert  | BESCHREIBUNG      |
    | ------------ | ---------------- | ---------------- |
    | **Abonnement** | Ihr Abonnement | Das Abonnement, in dem Ihre Ressourcen erstellt werden. | 
    | **[Ressourcengruppe](../azure-resource-manager/management/overview.md)**  | myResourceGroup | Wählen Sie die Ressourcengruppe aus, die Sie mit Ihrer Funktions-App erstellt haben. |
    | **Name** | myVirtualNet| Der Name des virtuellen Netzwerks, mit dem Ihre Funktions-App eine Verbindung herstellt. |
    | **[Region](https://azure.microsoft.com/regions/)** | myFunctionRegion | Wählen Sie die Region aus, in der Sie Ihre Funktions-App erstellt haben. |

1. Wählen Sie auf der Registerkarte **IP-Adressen** die Option **Subnetz hinzufügen** aus. Verwenden Sie beim Hinzufügen des Subnetzes die folgenden Einstellungen:

    :::image type="content" source="./media/functions-create-vnet/1-create-vnet-ip-address.png" alt-text="Screenshot der Konfigurationsansicht für das Erstellen eines virtuellen Netzwerks":::

    | Einstellung      | Vorgeschlagener Wert  | BESCHREIBUNG      |
    | ------------ | ---------------- | ---------------- |
    | **Subnetzname** | functions | Der Name des Subnetzes, mit dem Ihre Funktions-App eine Verbindung herstellt. | 
    | **Subnetzadressbereich** | 10.0.1.0/24 | Der IPv4-Adressraum in der obigen Abbildung ist 10.0.0.0/16. Beim Adressraum 10.1.0.0/16 wäre der empfohlene *Subnetzadressbereich* 10.1.1.0/24. |

1. Klicken Sie auf **Überprüfen + erstellen**. Wenn die Überprüfung abgeschlossen ist, wählen Sie **Erstellen** aus.

## <a name="lock-down-your-storage-account-with-private-endpoints"></a>Sperren des Speicherkontos mit privaten Endpunkten

Private Azure-Endpunkte werden verwendet, um über eine private IP-Adresse eine Verbindung mit bestimmten Azure-Ressourcen herzustellen. Diese Verbindung stellt sicher, dass der Netzwerkdatenverkehr auf das ausgewählte virtuelle Netzwerk beschränkt wird und der Zugriff nur für bestimmte Ressourcen möglich ist. Erstellen Sie nun die privaten Endpunkte für Azure File Storage und Azure Blob Storage mit Ihrem Speicherkonto.

1. Wählen Sie im neuen Speicherkonto im Menü auf der linken Seite die Option **Netzwerk** aus.

1. Wählen Sie die Registerkarte **Verbindungen mit privatem Endpunkt** und dann **Privater Endpunkt** aus.

    :::image type="content" source="./media/functions-create-vnet/2-navigate-private-endpoint-store.png" alt-text="Screenshot: Navigation zum Erstellen von privaten Endpunkten für das Speicherkonto":::

1. Verwenden Sie auf der Registerkarte **Grundlagen** die unten gezeigten Einstellungen für den privaten Endpunkt:

    | Einstellung      | Vorgeschlagener Wert  | BESCHREIBUNG      |
    | ------------ | ---------------- | ---------------- |
    | **Abonnement** | Ihr Abonnement | Das Abonnement, in dem Ihre Ressourcen erstellt werden. | 
    | **[Ressourcengruppe](../azure-resource-manager/management/overview.md)**  | myResourceGroup | Wählen Sie die Ressourcengruppe aus, die Sie mit Ihrer Funktions-App erstellt haben. | |
    | **Name** | file-endpoint | Der Name des privaten Endpunkts für Dateien aus Ihrem Speicherkonto. |
    | **[Region](https://azure.microsoft.com/regions/)** | myFunctionRegion | Wählen Sie die Region aus, in der Sie Ihr Speicherkonto erstellt haben. |

1. Verwenden Sie auf der Registerkarte **Ressource** die unten gezeigten Einstellungen für den privaten Endpunkt:

    | Einstellung      | Vorgeschlagener Wert  | BESCHREIBUNG      |
    | ------------ | ---------------- | ---------------- |
    | **Abonnement** | Ihr Abonnement | Das Abonnement, in dem Ihre Ressourcen erstellt werden. | 
    | **Ressourcentyp**  | Microsoft.Storage/storageAccounts | Dies ist der Ressourcentyp für Speicherkonten. |
    | **Ressource** | mysecurestorage | Das Speicherkonto, das Sie gerade erstellt haben. |
    | **Zielunterressource** | file | Dieser private Endpunkt wird für Dateien aus dem Speicherkonto verwendet. |

1. Wählen Sie auf der Registerkarte **Konfiguration** die Option **Standard** für die Subnetzeinstellung aus.

1. Klicken Sie auf **Überprüfen + erstellen**. Wenn die Überprüfung abgeschlossen ist, wählen Sie **Erstellen** aus. Ressourcen im virtuellen Netzwerk können nun mit Speicherdateien kommunizieren.

1. Erstellen Sie einen weiteren privaten Endpunkt für Blobs. Legen Sie auf der Registerkarte **Ressourcen** die folgenden Einstellungen fest. Verwenden Sie für alle anderen Optionen die gleichen Einstellungen wie in den obigen Schritten zum Erstellen des privaten Endpunkts für Dateien.

    | Einstellung      | Vorgeschlagener Wert  | BESCHREIBUNG      |
    | ------------ | ---------------- | ---------------- |
    | **Abonnement** | Ihr Abonnement | Das Abonnement, in dem Ihre Ressourcen erstellt werden. | 
    | **Ressourcentyp**  | Microsoft.Storage/storageAccounts | Dies ist der Ressourcentyp für Speicherkonten. |
    | **Ressource** | mysecurestorage | Das Speicherkonto, das Sie gerade erstellt haben. |
    | **Zielunterressource** | Blob | Dieser private Endpunkt wird für Blobs aus dem Speicherkonto verwendet. |

## <a name="lock-down-your-service-bus-with-a-private-endpoint"></a>Sperren der Service Bus-Instanz mit einem privaten Endpunkt

Erstellen Sie nun den privaten Endpunkt für Ihre Azure Service Bus-Instanz.

1. Wählen Sie in der neuen Service Bus-Instanz im Menü auf der linken Seite die Option **Netzwerk** aus.

1. Wählen Sie die Registerkarte **Verbindungen mit privatem Endpunkt** und dann **Privater Endpunkt** aus.

    :::image type="content" source="./media/functions-create-vnet/3-navigate-private-endpoint-service-bus.png" alt-text="Screenshot: Navigation zu privaten Endpunkten für Service Bus":::

1. Verwenden Sie auf der Registerkarte **Grundlagen** die unten gezeigten Einstellungen für den privaten Endpunkt:

    | Einstellung      | Vorgeschlagener Wert  | BESCHREIBUNG      |
    | ------------ | ---------------- | ---------------- |
    | **Abonnement** | Ihr Abonnement | Das Abonnement, in dem Ihre Ressourcen erstellt werden. | 
    | **[Ressourcengruppe](../azure-resource-manager/management/overview.md)**  | myResourceGroup | Wählen Sie die Ressourcengruppe aus, die Sie mit Ihrer Funktions-App erstellt haben. |
    | **Name** | sb-endpoint | Der Name des privaten Endpunkts für Dateien aus Ihrem Speicherkonto. |
    | **[Region](https://azure.microsoft.com/regions/)** | myFunctionRegion | Wählen Sie die Region aus, in der Sie Ihr Speicherkonto erstellt haben. |

1. Verwenden Sie auf der Registerkarte **Ressource** die unten gezeigten Einstellungen für den privaten Endpunkt:

    | Einstellung      | Vorgeschlagener Wert  | BESCHREIBUNG      |
    | ------------ | ---------------- | ---------------- |
    | **Abonnement** | Ihr Abonnement | Das Abonnement, in dem Ihre Ressourcen erstellt werden. | 
    | **Ressourcentyp**  | Microsoft.ServiceBus/namespaces | Dies ist der Ressourcentyp für Service Bus. |
    | **Ressource** | myServiceBus | Die Service Bus-Instanz, die Sie zuvor in diesem Tutorial erstellt haben. |
    | **Untergeordnete Zielressource** | Namespace | Dieser private Endpunkt wird für den Namespace der Service Bus-Instanz verwendet. |

1. Wählen Sie auf der Registerkarte **Konfiguration** die Option **Standard** für die Subnetzeinstellung aus.

1. Klicken Sie auf **Überprüfen + erstellen**. Wenn die Überprüfung abgeschlossen ist, wählen Sie **Erstellen** aus. Ressourcen im virtuellen Netzwerk können nun mit Service Bus kommunizieren.

## <a name="create-a-file-share"></a>Erstellen einer Dateifreigabe

1. Wählen Sie im Speicherkonto, das Sie erstellt haben, im Menü auf der linken Seite die Option **Dateifreigaben** aus.

1. Wählen Sie **+ Dateifreigaben** aus. Verwenden Sie für die Zwecke dieses Tutorials den Namen **files** für die Dateifreigabe.

    :::image type="content" source="./media/functions-create-vnet/4-create-file-share.png" alt-text="Screenshot: Erstellen einer Dateifreigabe im Speicherkonto":::

## <a name="get-storage-account-connection-string"></a>Beschaffen der Verbindungszeichenfolge für das Speicherkonto

1. Wählen Sie im Speicherkonto, das Sie erstellt haben, im Menü auf der linken Seite die Option **Zugriffsschlüssel** aus.

1. Wählen Sie **Schlüssel anzeigen** aus. Kopieren Sie die Verbindungszeichenfolge von „key1“, und speichern Sie sie. Diese Verbindungszeichenfolge wird später beim Konfigurieren der App-Einstellungen benötigt.

    :::image type="content" source="./media/functions-create-vnet/5-get-store-connection-string.png" alt-text="Screenshot: Abrufen der Verbindungszeichenfolge für ein Speicherkonto":::

## <a name="create-a-queue"></a>Erstellen einer Warteschlange

Dies ist die Warteschlange, aus der Ihr Azure Functions-Service Bus-Trigger Ereignisse abruft.

1. Wählen Sie in Ihrer Service Bus-Instanz im Menü auf der linken Seite die Option **Warteschlangen** aus.

1. Klicken Sie auf **Freigegebene Zugriffsrichtlinien**. Verwenden Sie für die Zwecke dieses Tutorials den Namen **queue** für die Warteschlange.

    :::image type="content" source="./media/functions-create-vnet/6-create-queue.png" alt-text="Screenshot: Erstellen einer Service Bus-Warteschlange":::

## <a name="get-service-bus-connection-string"></a>Abrufen der Service Bus-Verbindungszeichenfolge

1. Wählen Sie in Ihrer Service Bus-Instanz im Menü auf der linken Seite die Option **Richtlinien für gemeinsamen Zugriff** aus.

1. Wählen Sie **RootManageSharedAccessKey**. Kopieren Sie die **primäre Verbindungszeichenfolge**, und speichern Sie sie. Diese Verbindungszeichenfolge wird später beim Konfigurieren der App-Einstellungen benötigt.

    :::image type="content" source="./media/functions-create-vnet/7-get-service-bus-connection-string.png" alt-text="Screenshot: Abrufen der Service Bus-Verbindungszeichenfolge":::

## <a name="integrate-function-app-with-your-virtual-network"></a>Integrieren der Funktions-App in das virtuelle Netzwerk

Wenn Sie Ihre Funktions-App mit virtuellen Netzwerken verwenden möchten, müssen Sie sie einem Subnetz hinzufügen. Wir verwenden ein spezielles Subnetz für die Integration von Azure Functions in das virtuelle Netzwerk und das Standardsubnetz für alle anderen privaten Endpunkte, die in diesem Tutorial erstellt werden.

1. Wählen Sie in Ihrer Funktions-App im Menü auf der linken Seite die Option **Netzwerk** aus.

1. Wählen Sie unter „VNET-Integration“ die Option **Zum Konfigurieren hier klicken** aus.

    :::image type="content" source="./media/functions-create-vnet/8-connect-app-vnet.png" alt-text="Screenshot: Navigation zu „VNET-Integration“":::

1. Wählen Sie **VNET hinzufügen** aus.

1. Wählen Sie auf dem Blatt, das unter **Virtuelles Netzwerk** geöffnet wird, das zuvor erstellte virtuelle Netzwerk aus.

1. Wählen Sie das zuvor erstellte **Subnetz** mit dem Namen **functions** aus. Ihre Funktions-App ist jetzt in Ihr virtuelles Netzwerk integriert.

    :::image type="content" source="./media/functions-create-vnet/9-connect-app-subnet.png" alt-text="Screenshot: Verbinden einer Funktions-App mit einem Subnetz":::

## <a name="configure-your-function-app-settings-for-private-endpoints"></a>Konfigurieren der Funktions-App-Einstellungen für private Endpunkte

1. Wählen Sie in Ihrer Funktions-App im Menü auf der linken Seite die Option **Konfiguration** aus.

1. Damit Ihre Funktions-App mit virtuellen Netzwerken verwendet werden kann, müssen die folgenden App-Einstellungen aktualisiert werden. Wählen Sie **+ Neue Anwendungseinstellung** oder das Bleistiftsymbol neben **Bearbeiten** in der Spalte ganz rechts in der Tabelle mit den App-Einstellungen aus. Wählen Sie dann die Option **Schließen**.

    :::image type="content" source="./media/functions-create-vnet/10-configure-app-settings.png" alt-text="Screenshot: Konfigurieren der Funktions-App-Einstellungen für private Endpunkte":::

    | Einstellung      | Vorgeschlagener Wert  | BESCHREIBUNG      |
    | ------------ | ---------------- | ---------------- |
    | **AzureWebJobsStorage** | mysecurestorageConnectionString | Die Verbindungszeichenfolge des Speicherkontos, das Sie erstellt haben. Dies ist die Verbindungszeichenfolge für den Speicher, die Sie im Abschnitt [Beschaffen der Verbindungszeichenfolge für das Speicherkonto](#get-storage-account-connection-string) abgerufen haben. Wenn Sie diese Einstellung ändern, verwendet Ihre Funktions-App das sichere Speicherkonto für normale Vorgänge zur Laufzeit. | 
    | **WEBSITE_CONTENTAZUREFILECONNECTIONSTRING**  | mysecurestorageConnectionString | Die Verbindungszeichenfolge des Speicherkontos, das Sie erstellt haben. Wenn Sie diese Einstellung ändern, verwendet Ihre Funktions-App das sichere Speicherkonto für Dateien in Azure Files, die bei der Bereitstellung verwendet werden. |
    | **WEBSITE_CONTENTSHARE** | Dateien | Der Name der Dateifreigabe, die Sie im Speicherkonto erstellt haben. Diese App-Einstellung wird in Verbindung mit WEBSITE_CONTENTAZUREFILECONNECTIONSTRING verwendet. |
    | **SERVICEBUS_CONNECTION** | myServiceBusConnectionString | Erstellen Sie eine App-Einstellung für die Verbindungszeichenfolge Ihrer Service Bus-Instanz. Dies ist die Verbindungszeichenfolge für den Speicher, die Sie im Abschnitt [Abrufen der Service Bus-Verbindungszeichenfolge](#get-service-bus-connection-string) abgerufen haben.|
    | **WEBSITE_CONTENTOVERVNET** | 1 | Erstellen Sie diese App-Einstellung. Der Wert 1 ermöglicht die Skalierung Ihrer Funktions-App, wenn Sie Ihr Speicherkonto auf ein virtuelles Netzwerk beschränken. Sie sollten diese Einstellung aktivieren, wenn Sie Ihr Speicherkonto auf ein virtuelles Netzwerk einschränken. |
    | **WEBSITE_DNS_SERVER** | 168.63.129.16 | Erstellen Sie diese App-Einstellung. Sobald Ihre App in ein virtuelles Netzwerk integriert ist, verwendet sie den gleichen DNS-Server wie das virtuelle Netzwerk. Dies ist eine von zwei Einstellungen, die zur Verwendung Ihrer Funktions-App mit Azure DNS Private Zones und von privaten Endpunkten erforderlich sind. Mit diesen Einstellungen werden alle ausgehenden Aufrufe von Ihrer App an das virtuelle Netzwerk gesendet. |
    | **WEBSITE_VNET_ROUTE_ALL** | 1 | Erstellen Sie diese App-Einstellung. Sobald Ihre App in ein virtuelles Netzwerk integriert ist, verwendet sie den gleichen DNS-Server wie das virtuelle Netzwerk. Dies ist eine von zwei Einstellungen, die zur Verwendung Ihrer Funktions-App mit Azure DNS Private Zones und von privaten Endpunkten erforderlich sind. Mit diesen Einstellungen werden alle ausgehenden Aufrufe von Ihrer App an das virtuelle Netzwerk gesendet. |

1. Wählen Sie in der Ansicht **Konfiguration** die Registerkarte **Einstellungen der Funktionsruntime** aus.

1. Legen Sie **Überwachung der Runtimeskalierung** auf **Ein** fest, und wählen Sie **Speichern** aus. Die runtimegesteuerte Skalierung ermöglicht es Ihnen, HTTP-fremde Triggerfunktionen mit Diensten zu verbinden, die in Ihrem virtuellen Netzwerk ausgeführt werden.

    :::image type="content" source="./media/functions-create-vnet/11-enable-runtime-scaling.png" alt-text="Screenshot: Aktivieren der runtimegesteuerten Skalierung für Azure Functions":::

## <a name="deploy-a-service-bus-trigger-and-http-trigger-to-your-function-app"></a>Bereitstellen eines Service Bus-Triggers und HTTP-Triggers für die Funktions-App

1. Navigieren Sie in GitHub zum folgenden Beispielrepository, das eine Funktions-App mit zwei Funktionen, einem HTTP-Trigger und einem Service Bus-Warteschlangentrigger enthält.

    <https://github.com/Azure-Samples/functions-vnet-tutorial>

1. Wählen Sie oben auf der Seite die Schaltfläche **Verzweigen** aus, um eine Kopie dieses Repositorys in Ihrem eigenen GitHub-Konto oder Ihrer GitHub-Organisation zu erstellen.

1. Wählen Sie in Ihrer Funktions-App im Menü auf der linken Seite die Option **Bereitstellungscenter** aus. Wählen Sie dann **Einstellungen** aus.

1. Verwenden Sie auf der Registerkarte **Einstellungen** die unten gezeigten Bereitstellungseinstellungen:

    | Einstellung      | Vorgeschlagener Wert  | BESCHREIBUNG      |
    | ------------ | ---------------- | ---------------- |
    | **Quelle** | GitHub | Sie sollten in Schritt 2 ein GitHub-Repository mit dem Beispielcode erstellt haben. | 
    | **Organisation**  | myOrganization | Dies ist die Organisation, in die Ihr Repository eingecheckt wird (in der Regel Ihr Konto). |
    | **Repository** | myRepo | Das Repository, das Sie mit dem Beispielcode erstellt haben. |
    | **Branch** | Standard | Dies ist das Repository, das Sie soeben erstellt haben. Verwenden Sie daher den Mainbranch. |
    | **Laufzeitstapel** | .NET | Der Beispielcode ist in C# geschrieben. |

1. Wählen Sie **Speichern** aus. 

    :::image type="content" source="./media/functions-create-vnet/12-deploy-portal.png" alt-text="Screenshot: Bereitstellen des Azure Functions-Codes über das Portal":::

1. Die Erstbereitstellung kann mehrere Minuten in Anspruch nehmen. Wenn Ihre App erfolgreich bereitgestellt wurde, wird auf der Registerkarte **Protokolle** die Statusmeldung **Erfolgreich (aktiv)** angezeigt. Aktualisieren Sie ggf. die Seite. 

1. Herzlichen Glückwunsch! Sie haben Ihre Beispiel-Funktions-App erfolgreich bereitgestellt.

## <a name="lock-down-your-function-app-with-a-private-endpoint"></a>Sperren der Funktions-App mit einem privaten Endpunkt

Erstellen Sie nun den privaten Endpunkt für Ihre Funktions-App. Dieser private Endpunkt verbindet Ihre Funktions-App mithilfe einer privaten IP-Adresse über eine private und sichere Verbindung mit Ihrem virtuellen Netzwerk. Weitere Informationen zu privaten Endpunkten finden Sie in der [Dokumentation zu privaten Endpunkten](https://docs.microsoft.com/azure/private-link/private-endpoint-overview).

1. Wählen Sie in Ihrer Funktions-App im Menü auf der linken Seite die Option **Netzwerk** aus.

1. Wählen Sie unter „Private Endpunktverbindungen“ die Option **Zum Konfigurieren hier klicken** aus.

    :::image type="content" source="./media/functions-create-vnet/14-navigate-app-private-endpoint.png" alt-text="Screenshot: Navigation zum privaten Endpunkt einer Funktions-App":::

1. Wählen Sie **Hinzufügen**.

1. Legen Sie im Menü, das geöffnet wird, die folgenden Einstellungen für den privaten Endpunkt fest:

    :::image type="content" source="./media/functions-create-vnet/15-create-app-private-endpoint.png" alt-text="Screenshot: Erstellen eines privaten Endpunkts für eine Funktions-App":::

1. Wählen Sie **OK** aus, um den privaten Endpunkt hinzuzufügen. Herzlichen Glückwunsch! Sie haben Ihre Funktions-App, Ihre Service Bus-Instanz und Ihr Speicherkonto erfolgreich mit privaten Endpunkten geschützt.

### <a name="test-your-locked-down-function-app"></a>Testen der gesperrten Funktions-App

1. Wählen Sie in Ihrer Funktions-App im Menü auf der linken Seite die Option **Funktionen** aus.

1. Wählen Sie **ServiceBusQueueTrigger** aus.

1. Wählen Sie im Menü auf der linken Seite die Option **Überwachen** aus. Wie Sie sehen, können Sie die App nicht überwachen. Dies liegt daran, dass Ihr Browser keinen Zugriff auf das virtuelle Netzwerk hat und daher nicht direkt auf Ressourcen im virtuellen Netzwerk zugreifen kann. Im Folgenden zeigen wir eine weitere Methode, mit der Sie Ihre Funktions-App weiterhin überwachen können – Application Insights.

1. Wählen Sie in Ihrer Funktions-App im Menü auf der linken Seite die Option **Application Insights** und dann **Application Insights-Daten anzeigen** aus.

    :::image type="content" source="./media/functions-create-vnet/16-app-insights.png" alt-text="Screenshot: Anzeigen von Application Insights für eine Funktions-App":::

1. Wählen Sie im Menü auf der linken Seite die Option **Livemetriken** aus.

1. Öffnen Sie eine neue Registerkarte. Wählen Sie in Ihrer Service Bus-Instanz im Menü auf der linken Seite die Option **Warteschlangen** aus.

1. Wählen Sie Ihre Warteschlange aus.

1. Wählen Sie im Menü auf der linken Seite **Service Bus Explorer** aus. Wählen Sie unter **Senden** die Option **Text/unformatiert** als **Inhaltstyp** aus, und geben Sie eine Nachricht ein. 

1. Wählen Sie **Senden** aus, um die Nachricht zu senden.

    :::image type="content" source="./media/functions-create-vnet/17-send-service-bus-message.png" alt-text="Screenshot: Senden von Service Bus-Nachrichten über das Portal":::

1. Auf der Registerkarte mit den geöffneten **Livemetriken** sollte angezeigt werden, dass der Service Bus-Warteschlangentrigger ausgelöst wurde. Ist dies nicht der Fall, senden Sie die Nachricht erneut über **Service Bus Explorer**.

    :::image type="content" source="./media/functions-create-vnet/18-hello-world.png" alt-text="Screenshot: Anzeigen von Nachrichten mithilfe von Livemetriken für Funktions-Apps":::

1. Herzlichen Glückwunsch! Sie haben die Einrichtung Ihrer Funktions-App mit privaten Endpunkten erfolgreich getestet.

### <a name="private-dns-zones"></a>Was ist eine private Azure DNS-Zone?
Wenn Sie einen privaten Endpunkt zum Herstellen einer Verbindung mit Azure-Ressourcen verwenden, wird die Verbindung mit einer privaten IP-Adresse hergestellt und nicht mit dem öffentlichen Endpunkt. Vorhandene Azure-Dienste sind so konfiguriert, dass sie die bestehende DNS-Konfiguration zum Herstellen der Verbindung mit dem öffentlichen Endpunkt verwenden. Die DNS-Konfiguration muss überschrieben werden, damit eine Verbindung mit dem privaten Endpunkt hergestellt wird.

Für jede mit einem privaten Endpunkt konfigurierte Azure-Ressource wurde eine private DNS-Zone erstellt. Für jede private IP-Adresse, die dem privaten Endpunkt zugeordnet ist, wird ein DNS-A-Eintrag erstellt.

Die folgenden DNS-Zonen wurden in diesem Tutorial erstellt:

- privatelink.file.core.windows.net
- privatelink.blob.core.windows.net
- privatelink.servicebus.windows.net
- privatelink.azurewebsites.net

[!INCLUDE [clean-up-section-portal](../../includes/clean-up-section-portal.md)]

## <a name="next-steps"></a>Nächste Schritte

In diesem Tutorial haben Sie eine Premium-Funktions-App, ein Speicherkonto und eine Service Bus-Instanz erstellt und diese mit privaten Endpunkten geschützt. Weitere Informationen zu den verschiedenen verfügbaren Netzwerkfeatures finden Sie hier:

> [!div class="nextstepaction"]
> [Weitere Informationen zu den Netzwerkoptionen in Functions](./functions-networking-options.md)

[Premium-Plan]: functions-premium-plan.md
