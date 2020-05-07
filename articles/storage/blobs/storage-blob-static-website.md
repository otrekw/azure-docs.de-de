---
title: Hosten von statischen Websites in Azure Storage
description: Das Hosten von statischen Websites in Azure Storage bietet eine kostengünstige, skalierbare Lösung zum Hosten von modernen Webanwendungen.
author: normesta
ms.service: storage
ms.topic: conceptual
ms.author: normesta
ms.reviewer: dineshm
ms.date: 05/29/2019
ms.subservice: blobs
ms.openlocfilehash: 57ba59288cbf65c1ef588302965d480ee357ea4d
ms.sourcegitcommit: 31236e3de7f1933be246d1bfeb9a517644eacd61
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/04/2020
ms.locfileid: "82779976"
---
# <a name="static-website-hosting-in-azure-storage"></a>Hosten von statischen Websites in Azure Storage

Sie können statische Inhalte (HTML, CSS, JavaScript und Bilddateien) direkt über einen Speichercontainer namens *$web* bereitstellen. Das Hosten von Inhalten in Azure Storage ermöglicht die Verwendung serverloser Architekturen mit [Azure Functions](/azure/azure-functions/functions-overview) und anderen PaaS-Diensten (Platform-as-a-Service).

[!INCLUDE [updated-for-az](../../../includes/storage-data-lake-gen2-support.md)]

> [!NOTE]
> Sollte Ihre Website von serverseitigem Code abhängig sein, verwenden Sie stattdessen [Azure App Service](/azure/app-service/overview).

## <a name="setting-up-a-static-website"></a>Einrichten einer statischen Website

Das Feature zum Hosten statischer Websites muss für das Speicherkonto aktiviert werden.

Wählen Sie hierzu den Namen Ihrer Standarddatei aus, und geben Sie optional einen Pfad zu einer benutzerdefinierten 404-Seite an. Sollte in Ihrem Konto noch kein Blobspeichercontainer namens **$web** vorhanden sein, wird automatisch einer erstellt. Fügen Sie diesem Container die Dateien Ihrer Website hinzu.

Eine entsprechende Schritt-für-Schritt-Anleitung finden Sie unter [Host a static website in Azure Storage](storage-blob-static-website-how-to.md) (Hosten einer statischen Website in Azure Storage).

![Metriken von statischen Websites in Azure Storage: Metrik](./media/storage-blob-static-website/storage-blob-static-website-blob-container.png)

Bei Dateien im Container **$web** wird die Groß-/Kleinschreibung beachtet. Sie werden über anonyme Zugriffsanforderungen bereitgestellt und sind nur über Lesevorgänge verfügbar.

## <a name="uploading-content"></a>Hochladen von Inhalten

Inhalte können mit einem der folgenden Tools in den Container **$web** hochgeladen werden:

> [!div class="checklist"]
> * [Azure-Befehlszeilenschnittstelle](storage-blob-static-website-how-to.md?tabs=azure-cli)
> * [Azure PowerShell-Modul](storage-blob-static-website-how-to.md?tabs=azure-powershell)
> * [AzCopy](../common/storage-use-azcopy-v10.md)
> * [Azure Storage-Explorer](https://azure.microsoft.com/features/storage-explorer/)
> * [Azure Pipelines](https://azure.microsoft.com/services/devops/pipelines/)
> * [Visual Studio Code-Erweiterung](/azure/javascript/tutorial-vscode-static-website-node-01)

## <a name="viewing-content"></a>Anzeigen von Inhalten

Benutzer können Websiteinhalte in einem Browser unter der öffentlichen URL der Website anzeigen. Die URL können Sie im Azure-Portal, über die Azure-Befehlszeilenschnittstelle oder mithilfe von PowerShell ermitteln. Verwenden Sie diese Tabelle als Richtlinie.

|Tool| Anleitungen |
|----|----|
|**Azure portal** | [Find the website URL by using the Azure portal](storage-blob-static-website-how-to.md#portal-find-url) (Ermitteln der Website-URL über das Azure-Portal) |
|**Azure-Befehlszeilenschnittstelle** | [Find the website URL by using the Azure CLI](storage-blob-static-website-how-to.md#cli-find-url) (Ermitteln der Website-URL mithilfe der Azure-Befehlszeilenschnittstelle) |
|**Azure PowerShell-Modul** | [Find the website URL by using PowerShell](storage-blob-static-website-how-to.md#powershell-find-url) (Ermitteln der Website-URL mithilfe von PowerShell) |

Die URL Ihrer Website enthält einen Regionscode. Die URL `https://contosoblobaccount.z22.web.core.windows.net/` enthält beispielsweise den Regionscode `z22`.

Dieser Code muss zwar in der URL verbleiben, wird jedoch nur intern genutzt und muss nicht anderweitig verwendet werden.

Das Indexdokument, das Sie beim Aktivieren des Hostens von statischen Websites angeben, wird angezeigt, wenn Benutzer beim Aufrufen der Website keine bestimmte Datei angeben (Beispiel: `https://contosoblobaccount.z22.web.core.windows.net`).  

Wenn der Server einen 404-Fehler zurückgibt und Sie beim Aktivieren der Website kein Fehlerdokument angegeben haben, wird an den Benutzer eine 404-Standardseite zurückgegeben.

> [!NOTE]
> [CORS](https://docs.microsoft.com/rest/api/storageservices/cross-origin-resource-sharing--cors--support-for-the-azure-storage-services) wird für die statische Website nicht unterstützt.

## <a name="impact-of-the-setting-the-public-access-level-of-the-web-container"></a>Auswirkungen der Einstellung der öffentlichen Zugriffsebene des Webcontainers

Sie können zwar die öffentliche Zugriffsebene des Containers **$web** ändern, dies hat jedoch keinerlei Auswirkungen auf den primären Endpunkt der statischen Website, da diese Dateien über anonyme Zugriffsanforderungen bereitgestellt werden. Das bedeutet öffentlichen (schreibgeschützten) Zugriff auf alle Dateien.

Der folgende Screenshot zeigt die Einstellung der öffentlichen Zugriffsebene im Azure-Portal:

![Screenshot: Festlegen der öffentlichen Zugriffsebene im Azure-Portal](./media/storage-manage-access-to-resources/storage-manage-access-to-resources-0.png)

Der primäre Endpunkt der statischen Website ist von einer Änderung der öffentlichen Zugriffsebene zwar nicht betroffen, eine solche Änderung hat jedoch Auswirkungen auf den primären Endpunkt des Blobdiensts.

Wenn Sie also beispielsweise die öffentliche Zugriffsebene des Containers **$web** von **Privat (kein anonymer Zugriff)** in **Blob (anonymer Lesezugriff nur für Blobs)** ändern, bleibt die öffentliche Zugriffsebene für den primären Endpunkt der statischen Website (`https://contosoblobaccount.z22.web.core.windows.net/index.html`) unverändert.

Der öffentliche Zugriff auf den primären Endpunkt des Blobdiensts (`https://contosoblobaccount.blob.core.windows.net/$web/index.html`) ändert sich hingegen von privat in öffentlich. Nun können Benutzer die Datei unter Verwendung eines der beiden Endpunkte öffnen.

## <a name="mapping-a-custom-domain-to-a-static-website-url"></a>Zuordnen einer benutzerdefinierten Domäne zu einer URL einer statischen Website

Sie können Ihre statische Website über eine benutzerdefinierte Domäne verfügbar machen. 

Es ist einfacher, den HTTP-Zugriff für Ihre benutzerdefinierte Domäne zu aktivieren, weil dies von Azure Storage nativ unterstützt wird. Zum Aktivieren von HTTPS müssen Sie Azure CDN verwenden, weil Azure Storage HTTPS bei benutzerdefinierten Domänen noch nicht nativ unterstützt. Eine Schritt-für-Schritt-Anleitung finden Sie unter [Zuordnen einer benutzerdefinierten Domäne zu einem Azure Blob Storage-Endpunkt](storage-custom-domain-name.md).

Wenn das Speicherkonto für die [sichere Übertragung](../common/storage-require-secure-transfer.md) über HTTPS konfiguriert ist, müssen Benutzer den HTTPS-Endpunkt verwenden. 

> [!TIP]
> Ziehen Sie das Hosten Ihrer Domäne in Azure in Betracht. Weitere Informationen finden Sie unter [Hosten Ihrer Domäne in Azure DNS](../../dns/dns-delegate-domain-azure-dns.md).

## <a name="adding-http-headers"></a>Hinzufügen von HTTP-Headern

Es gibt keine Möglichkeit, Header im Rahmen des Features für die statische Website zu konfigurieren. Sie können jedoch Azure CDN zum Hinzufügen von Headern und Anfügen (oder Überschreiben) von Headerwerten verwenden. Informationen dazu finden Sie in der [Referenz zur Standardregel-Engine für Azure CDN](https://docs.microsoft.com/azure/cdn/cdn-standard-rules-engine-reference).

Wenn Sie Header zum Steuern der Zwischenspeicherung verwenden möchten, finden Sie Informationen dazu unter [Steuern des Azure CDN-Zwischenspeicherverhaltens mit Chacheregeln](https://docs.microsoft.com/azure/cdn/cdn-caching-rules).

## <a name="pricing"></a>Preise

Die Aktivierung des Hostings statischer Websites ist kostenlos. Ihnen werden lediglich der von Ihrer Website genutzte Blobspeicher sowie die Betriebskosten in Rechnung gestellt. Weitere Informationen zu Preisen für Azure Blob Storage finden Sie auf der Seite [Preise für Azure Blob Storage](https://azure.microsoft.com/pricing/details/storage/blobs/).

## <a name="metrics"></a>metrics

Sie können Metriken für Seiten der statischen Website aktivieren. Nachdem Sie Metriken aktiviert haben, werden Datenverkehrsstatistiken zu Dateien im Container **$web** im Dashboard für Metriken gemeldet.

Informationen zum Aktivieren von Metriken für Seiten Ihrer statischen Website finden Sie unter [Enable metrics on static website pages](storage-blob-static-website-how-to.md#metrics) (Aktivieren von Metriken auf Seiten der statischen Website).

## <a name="next-steps"></a>Nächste Schritte

* [Host a static website in Azure Storage](storage-blob-static-website-how-to.md) (Hosten einer statischen Website in Azure Storage)
* [Zuordnen einer benutzerdefinierten Domäne zu einem Azure Blob Storage-Endpunkt](storage-custom-domain-name.md)
* [Azure-Funktionen](/azure/azure-functions/functions-overview)
* [Azure App Service](/azure/app-service/overview)
* [Build a serverless web app in Azure](https://docs.microsoft.com/azure/functions/tutorial-static-website-serverless-api-with-database) (Erstellen einer serverlosen Web-App in Azure)
* [Tutorial: Hosten Ihrer Domäne in Azure DNS](../../dns/dns-delegate-domain-azure-dns.md)
