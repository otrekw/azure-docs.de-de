---
title: Problembehandlung für „Die Azure Functions-Runtime ist nicht erreichbar“.
description: Erfahren Sie, wie Sie Problembehandlung für ein ungültiges Speicherkonto durchführen.
author: alexkarcher-msft
ms.topic: article
ms.date: 09/05/2018
ms.author: alkarche
ms.openlocfilehash: 910b582cb40b9f8aff6a553621b4677d6b019826
ms.sourcegitcommit: 42517355cc32890b1686de996c7913c98634e348
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/02/2020
ms.locfileid: "76963885"
---
# <a name="how-to-troubleshoot-functions-runtime-is-unreachable"></a>Problembehandlung für „Die Functions-Runtime ist nicht erreichbar“.

Dieser Artikel ist für die Problembehandlung der Fehlermeldung „Die Azure Functions-Runtime ist nicht erreichbar“ vorgesehen, wenn diese im Azure-Portal angezeigt wird. Wenn dieser Fehler auftritt, wird folgende Fehlerzeichenfolge im Portal angezeigt.

`Error: Azure Functions Runtime is unreachable. Click here for details on storage configuration`

Hierzu kommt es, wenn die Azure Functions-Runtime nicht gestartet werden kann. Die häufigste Ursache für das Auftreten dieses Fehlers besteht darin, dass die Funktions-App den Zugriff auf ihr Speicherkonto verliert. Weitere Informationen finden Sie unter [Speicherkontoanforderungen](storage-considerations.md#storage-account-requirements).

Der Rest dieses Artikels hilft Ihnen bei der Problembehandlung der folgenden Ursachen dieses Fehlers, einschließlich der Vorgehensweise zum Identifizieren und Auflösen jedes Falls.

+ [Speicherkonto gelöscht](#storage-account-deleted)
+ [Anwendungseinstellungen des Speicherkontos gelöscht](#storage-account-application-settings-deleted)
+ [Ungültige Anmeldeinformationen des Speicherkontos](#storage-account-credentials-invalid)
+ [Kein Zugriff auf das Speicherkonto](#storage-account-inaccessible)
+ [Tägliches Ausführungskontingent überschritten](#daily-execution-quota-full)
+ [Ihre App ist hinter einer Firewall](#app-is-behind-a-firewall)


## <a name="storage-account-deleted"></a>Speicherkonto gelöscht

Jede Funktions-App benötigt für den Betrieb ein Speicherkonto. Wenn dieses Konto gelöscht wird, funktioniert Ihre Funktions-App nicht.

### <a name="how-to-find-your-storage-account"></a>Finden Ihres Speicherkontos

Schlagen Sie zunächst den Namen Ihres Speicherkontos in den Anwendungseinstellungen nach. Entweder `AzureWebJobsStorage` oder `WEBSITE_CONTENTAZUREFILECONNECTIONSTRING` enthält den Namen Ihres Speicherkontos, in eine Verbindungszeichenfolge eingeschlossen. Weitere spezifische Informationen finden Sie [hier in der Referenz zu Anwendungseinstellungen](https://docs.microsoft.com/azure/azure-functions/functions-app-settings#azurewebjobsstorage).

Suchen Sie im Azure-Portal nach Ihrem Speicherkonto, um festzustellen, ob es noch vorhanden ist. Wenn es gelöscht wurde, müssen Sie ein neues Speicherkonto erstellen und Ihre Verbindungszeichenfolgen für den Speicher ersetzen. Ihr Funktionscode ist verloren gegangen, und Sie müssen ihn erneut bereitstellen.

## <a name="storage-account-application-settings-deleted"></a>Anwendungseinstellungen des Speicherkontos gelöscht

Wenn im vorhergegangenen Schritt keine Verbindungszeichenfolge für das Speicherkonto vorhanden war, wurde sie wahrscheinlich gelöscht oder überschrieben. Das Löschen von App-Einstellungen erfolgt meistens bei der Verwendung von Bereitstellungsslots oder Azure Resource Manager-Skripts zum Festlegen von Anwendungseinstellungen.

### <a name="required-application-settings"></a>Erforderliche Anwendungseinstellungen

* Erforderlich
    * [`AzureWebJobsStorage`](https://docs.microsoft.com/azure/azure-functions/functions-app-settings#azurewebjobsstorage)
* Für Verbrauchsplanfunktionen erforderlich
    * [`WEBSITE_CONTENTAZUREFILECONNECTIONSTRING`](https://docs.microsoft.com/azure/azure-functions/functions-app-settings)
    * [`WEBSITE_CONTENTSHARE`](https://docs.microsoft.com/azure/azure-functions/functions-app-settings)

[Lesen Sie hier mehr über diese Anwendungseinstellungen](https://docs.microsoft.com/azure/azure-functions/functions-app-settings).

### <a name="guidance"></a>Anleitungen

* Aktivieren Sie für keine dieser Einstellungen „Sloteinstellung“. Wenn Sie die Bereitstellungsslots tauschen, wird die Funktions-App beschädigt.
* Ändern Sie diese Einstellungen im Rahmen von automatisierten Bereitstellungen nicht.
* Diese Einstellungen müssen zur Erstellungszeit angegeben werden und gültig sein. Eine automatisierte Bereitstellung, die diese Einstellungen nicht enthält, führt zu einer Funktions-App, die nicht läuft, selbst wenn die Einstellungen später hinzugefügt werden.

## <a name="storage-account-credentials-invalid"></a>Ungültige Anmeldeinformationen des Speicherkontos

Die oben erwähnten Verbindungszeichenfolgen für Speicherkonten müssen aktualisiert werden, wenn Sie Speicherschlüssel neu erstellen. [Erfahren Sie hier mehr über die Verwaltung von Speicherschlüsseln](https://docs.microsoft.com/azure/storage/common/storage-create-storage-account).

## <a name="storage-account-inaccessible"></a>Kein Zugriff auf das Speicherkonto

Ihre Funktions-App muss auf das Speicherkonto zugreifen können. Dies sind häufige Probleme, die den Zugriff einer Funktions-App auf ein Speicherkonto blockieren:

+ Funktions-Apps wurden in App Service-Umgebungen (App Service Environments, ASE) ohne ordnungsgemäße Netzwerkregeln bereitgestellt, die Datenverkehr zum und vom Speicherkonto zulassen.

+ Die Firewall des Speicherkontos ist aktiviert und nicht für das Zulassen von Datenverkehr von und zu Functions konfiguriert. Weitere Informationen finden Sie unter [Konfigurieren von Azure Storage-Firewalls und virtuellen Netzwerken](../storage/common/storage-network-security.md).

## <a name="daily-execution-quota-full"></a>Tägliches Ausführungskontingent voll

Wenn Sie ein tägliches Ausführungskontingent konfiguriert haben, wird Ihre Funktions-App vorübergehend deaktiviert, was dazu führt, dass viele der Portalsteuerelemente nicht mehr zur Verfügung stehen. 

+ Um dies im [Azure-Portal](https://portal.azure.com) zu überprüfen, öffnen Sie **Plattformfeatures** > **Funktions-App-Einstellungen** in Ihrer Funktions-App. Wenn Sie das von Ihnen festgelegte **Tägliche Nutzungskontingent** überschritten haben, wird folgende Meldung angezeigt:

    `The function app has reached daily usage quota and has been stopped until the next 24 hours time frame.`

+ Um dieses Problem zu beheben, entfernen oder erhöhen Sie das tägliche Kontingent, und starten Sie Ihre App neu. Andernfalls wird die Ausführung Ihrer App bis zum nächsten Tag blockiert.

## <a name="app-is-behind-a-firewall"></a>App ist hinter einer Firewall

Ihre Funktions-Runtime ist nicht erreichbar, wenn Ihre Funktions-App in einer [App Service-Umgebung mit internem Lastenausgleich gehostet wird](../app-service/environment/create-ilb-ase.md) und so konfiguriert ist, dass eingehender Internetdatenverkehr blockiert wird, oder wenn [eingehende IP-Einschränkungen konfiguriert](functions-networking-options.md#inbound-ip-restrictions) sind, um den Internetzugriff zu blockieren. Das Azure-Portal sendet Aufrufe direkt an die laufende App, um die Liste der Funktionen abzurufen, und führt außerdem HTTP-Aufrufe an den KUDU-Endpunkt aus. Das Ausführen von Einstellungen auf Plattformebene auf der Registerkarte `Platform Features` steht weiterhin zur Verfügung.

Zum Überprüfen Ihrer ASE-Konfiguration navigieren Sie zur NSG des Subnetzes, in dem sich die ASE befindet, und überprüfen eingehende Regeln, um Datenverkehr von der öffentlichen IP-Adresse des Computers zuzulassen, auf dem Sie auf die Anwendung zugreifen. Sie können das Portal auch auf einem Computer verwenden, der mit dem virtuellen Netzwerk verbunden ist, auf dem Ihre App ausgeführt wird, oder auf einem virtuellen Computer, der in Ihrem virtuellen Netzwerk ausgeführt wird. [Weitere Informationen zur Konfiguration eingehender Regeln finden Sie hier](../app-service/environment/network-info.md#network-security-groups).

## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen zur Überwachung Ihrer Funktions-Apps:

> [!div class="nextstepaction"]
> [Überwachen von Azure Functions](functions-monitoring.md)
