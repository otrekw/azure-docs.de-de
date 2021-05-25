---
title: Integrieren von Azure DNS in Ihre Azure-Ressourcen – Azure DNS
description: In diesem Artikel erfahren Sie, wie Sie mit Azure DNS für Ihre Azure-Ressourcen DNS bereitstellen.
services: dns
author: rohinkoul
ms.service: dns
ms.topic: how-to
ms.date: 05/06/2021
ms.author: rohink
ms.openlocfilehash: c09c1840c9976a2ac14cb5ef73f8c4e15000c54f
ms.sourcegitcommit: ba8f0365b192f6f708eb8ce7aadb134ef8eda326
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/08/2021
ms.locfileid: "109634935"
---
# <a name="use-azure-dns-to-provide-custom-domain-settings-for-an-azure-service"></a>Bereitstellen von benutzerdefinierten Domäneneinstellungen für einen Azure-Dienst mit Azure DNS

Azure DNS ermöglicht die Namensauflösung für Azure-Ressourcen, die benutzerdefinierte Domänen unterstützen oder einen vollqualifizierten Domänennamen (FQDN) aufweisen. Beispiel: Sie möchten, dass Benutzer auf Ihre Azure-Web-App mit dem FQDN `contoso.com` oder `www.contoso.com` zugreifen. Dieser Artikel führt Sie durch die Konfiguration Ihres Azure-Diensts mit Azure DNS für die Verwendung von benutzerdefinierten Domänen.

## <a name="prerequisites"></a>Voraussetzungen

Zur Verwendung von Azure DNS für Ihre benutzerdefinierte Domäne müssen Sie zuerst Ihre Domäne an Azure DNS delegieren. Anweisungen zum Konfigurieren Ihrer Namenserver für die Delegierung finden Sie unter [Delegieren einer Domäne an Azure DNS](./dns-delegate-domain-azure-dns.md). Nachdem die Domäne an Ihre Azure DNS-Zone delegiert wurde, können Sie die erforderlichen DNS-Einträge konfigurieren.

Sie können eine Vanity oder eine benutzerdefinierte Domäne für Azure-Funktionen-Apps, öffentliche IP-Adressen, App Service (Web-Apps), Blobspeicher und Azure CDN konfigurieren.

## <a name="azure-function-app"></a>Azure-Funktionen-App

Bei der Konfiguration einer benutzerdefinierten Domäne für Azure-Funktions-Apps wird ein CNAME-Eintrag erstellt und für die Funktions-App selbst konfiguriert.
 
1. Navigieren Sie zu **Funktionen-App**, und wählen Sie Ihre Funktionen-App aus. Klicken Sie unter *Einstellungen* auf **Benutzerdefinierte Domänen**. Beachten Sie die **aktuelle URL** unter *Zugewiesene benutzerdefinierte Domänen*. Diese Adresse wird als Alias für den erstellten DNS-Datensatz verwendet.

    :::image type="content" source="./media/dns-custom-domain/function-app.png" alt-text="Screenshot: Benutzerdefinierte Domänen für Funktions-App.":::

1. Navigieren Sie zu Ihrer DNS-Zone, und klicken Sie auf **+ Datensatzgruppe**. Geben Sie auf der Seite **Datensatzgruppe hinzufügen** die folgenden Informationen an, und klicken Sie auf **OK**, um sie zu erstellen.

    :::image type="content" source="./media/dns-custom-domain/function-app-record.png" alt-text="Screenshot: Seite „Datensatzgruppe hinzufügen“ in Funktions-App":::

    |Eigenschaft  |Wert  |BESCHREIBUNG  |
    |---------|---------|---------|
    | Name | myfunctionapp | Dieser Wert bildet zusammen mit der Domänennamenbezeichnung den FQDN für den Namen der benutzerdefinierten Domäne. |
    | type | CNAME | Verwenden Sie einen CNAME-Datensatz als Alias. |
    | TTL | 1 | 1 steht für 1 Stunde.  |
    | TTL-Einheit | Stunden | Stunden werden als Maßeinheit für die Zeit verwendet.  |
    | Alias | contosofunction.azurewebsites.net | Der DNS-Name, für den Sie den Alias erstellen, ist in diesem Beispiel der standardmäßig für die Funktions-App bereitgestellte DNS-Name „contosofunction.azurewebsites.net“.        |
    
1. Navigieren Sie zurück zur Funktions-App, und klicken Sie unter *Einstellungen* auf die Option **Benutzerdefinierte Domänen**. Klicken Sie dann auf **+ Benutzerdefinierte Domäne hinzufügen**.

    :::image type="content" source="./media/dns-custom-domain/function-app-add-domain.png" alt-text="Screenshot: Schaltfläche „Benutzerdefinierte Domäne hinzufügen“ für Funktions-App":::    

1. Geben Sie auf der Seite **Benutzerdefinierte Domäne hinzufügen** den CNAME-Eintrag in das Textfeld **Benutzerdefinierte Domäne** ein, und klicken Sie auf **Überprüfen**. Wenn der Eintrag gefunden wird, wird die Schaltfläche **Benutzerdefinierte Domäne hinzufügen** angezeigt. Wählen Sie **Benutzerdefinierte Domäne hinzufügen** aus, um den Alias hinzuzufügen.

    :::image type="content" source="./media/dns-custom-domain/function-app-cname.png" alt-text="Screenshot: Seite „Benutzerdefinierte Domäne hinzufügen“ für Funktions-App":::

## <a name="public-ip-address"></a>Öffentliche IP-Adresse

Um eine benutzerdefinierte Domäne für Dienste zu konfigurieren, die eine öffentliche IP-Adresse als Ressource verwenden (z.B. Application Gateway, Load Balancer, Clouddienst, Resource Manager-VMs und klassische VMs), wird ein A-Eintrag verwendet.

1. Navigieren Sie zur öffentlichen IP-Adresse, und klicken Sie auf **Konfiguration**. Notieren Sie sich die angegebene IP-Adresse.

    :::image type="content" source="./media/dns-custom-domain/public-ip.png" alt-text="Screenshot: Konfigurationsseite für öffentliche IP-Adressen":::

1. Navigieren Sie zu Ihrer DNS-Zone, und klicken Sie auf **+ Datensatzgruppe**. Geben Sie auf der Seite **Datensatzgruppe hinzufügen** die folgenden Informationen an, und klicken Sie auf **OK**, um sie zu erstellen.

    :::image type="content" source="./media/dns-custom-domain/public-ip-record.png" alt-text="Screenshot: Seite „Datensatzgruppe für öffentliche IP-Adressen“":::

    | Eigenschaft | Wert | BESCHREIBUNG |
    | -------- | ----- | ------------|
    | Name | webserver1 | Dieser Wert bildet zusammen mit der Domänennamenbezeichnung den FQDN für den Namen der benutzerdefinierten Domäne. |
    | type | Ein | Verwenden Sie einen A-Datensatz, da die Ressource eine IP-Adresse ist. |
    | TTL | 1 | 1 steht für 1 Stunde. |
    | TTL-Einheit | Stunden | Stunden werden als Maßeinheit für die Zeit verwendet. |
    | IP-Adresse | `<your ip address>` | Öffentliche IP-Adresse |

1. Nachdem der A-Datensatz erstellt wurde, führen Sie `nslookup` aus, um den Datensatz zu überprüfen.

    :::image type="content" source="./media/dns-custom-domain/public-ip-nslookup.png" alt-text="Screenshot: „nslookup“ in „cmd“ für öffentliche IP-Adressen":::

## <a name="app-service-web-apps"></a>App Service (Web-Apps)

Die folgenden Schritte führen Sie durch die Konfiguration einer benutzerdefinierten Domäne für eine App Service-Web-App.

1. Navigieren Sie zu **App Service**, und wählen Sie die Ressource aus, für die Sie einen benutzerdefinierten Domänennamen konfigurieren möchten. Klicken Sie dann unter *Einstellungen* auf **Benutzerdefinierte Domänen**. Beachten Sie die **aktuelle URL** unter *Zugewiesene benutzerdefinierte Domänen*. Diese Adresse wird als Alias für den erstellten DNS-Datensatz verwendet.

    :::image type="content" source="./media/dns-custom-domain/web-app.png" alt-text="Screenshot: Benutzerdefinierte Domänen für Web-App":::

1. Navigieren Sie zu Ihrer DNS-Zone, und klicken Sie auf **+ Datensatzgruppe**. Geben Sie auf der Seite **Datensatzgruppe hinzufügen** die folgenden Informationen an, und klicken Sie auf **OK**, um sie zu erstellen.

    :::image type="content" source="./media/dns-custom-domain/web-app.png" alt-text="Screenshot: Seite „Datensatzgruppe für Web-App“":::

    | Eigenschaft  | Wert | BESCHREIBUNG |
    |---------- | ----- | ----------- |
    | Name | mywebserver | Dieser Wert bildet zusammen mit der Domänennamenbezeichnung den FQDN für den Namen der benutzerdefinierten Domäne. |
    | type | CNAME | Verwenden Sie einen CNAME-Datensatz als Alias. Wenn für die Ressource eine IP-Adresse verwendet wurde, würde ein A-Eintrag verwendet werden. |
    | TTL | 1 | 1 steht für 1 Stunde. |
    | TTL-Einheit | Stunden | Stunden werden als Maßeinheit für die Zeit verwendet. |
    | Alias | contoso.azurewebsites.net | Der DNS-Name, für den Sie den Alias erstellen, ist in diesem Beispiel der standardmäßig für die Web-App bereitgestellte DNS-Name „contoso.azurewebsites.net“. |

1. Navigieren Sie zurück zur Web-App, und klicken Sie unter *Einstellungen* auf die Option **Benutzerdefinierte Domänen**. Klicken Sie dann auf **+ Benutzerdefinierte Domäne hinzufügen**.

    :::image type="content" source="./media/dns-custom-domain/web-app-add-domain.png" alt-text="Screenshot: Schaltfläche „Benutzerdefinierte Domäne hinzufügen“ für Web-App":::  

1. Geben Sie auf der Seite **Benutzerdefinierte Domäne hinzufügen** den CNAME-Eintrag in das Textfeld **Benutzerdefinierte Domäne** ein, und klicken Sie auf **Überprüfen**. Wenn der Eintrag gefunden wird, wird die Schaltfläche **Benutzerdefinierte Domäne hinzufügen** angezeigt. Wählen Sie **Benutzerdefinierte Domäne hinzufügen** aus, um den Alias hinzuzufügen.

    :::image type="content" source="./media/dns-custom-domain/web-app-cname.png" alt-text="Screenshot: Seite „Benutzerdefinierte Domäne hinzufügen“ für Web-App":::

1. Nachdem der Vorgang durchgeführt wurde, führen Sie **nslookup** aus, um sicherzustellen, dass die Namensauflösung funktioniert.

    :::image type="content" source="./media/dns-custom-domain/app-service-nslookup.png" alt-text="Screenshot: „nslookup“ für Web-App"::: 

Weitere Informationen zum Zuordnen einer benutzerdefinierten Domäne zu App Service finden Sie unter [Zuordnen eines vorhandenen benutzerdefinierten DNS-Namens zu Azure-Web-Apps](../app-service/app-service-web-tutorial-custom-domain.md?toc=%dns%2ftoc.json).

Weitere Informationen zum Migrieren eines aktiven DNS-Namens finden Sie unter [Migrieren eines aktiven DNS-Namens zu Azure App Service](../app-service/manage-custom-dns-migrate-domain.md).

Informationen zum Erwerb einer benutzerdefinierten Domäne für Ihre App Service-Instanz finden Sie unter [Erwerben eines Namens für eine benutzerdefinierte Domäne für Azure-Web-Apps](../app-service/manage-custom-dns-buy-domain.md).

## <a name="blob-storage"></a>Blob Storage

Die folgenden Schritte führen Sie durch die Konfiguration eines CNAME-Datensatzes für ein Blob Storage-Konto mit der asverify-Methode. Durch diese Methode wird sichergestellt, dass keine Ausfallzeiten auftreten.

1. Navigieren Sie zu **Speicherkonten**, wählen Sie Ihr Speicherkonto aus, und klicken Sie unter **Einstellungen** auf die Option *Netzwerk*. Wählen Sie dann die Registerkarte **Benutzerdefinierte Domäne** aus. Beachten Sie den FQDN in Schritt 2. Dieser Name wird für die Erstellung des ersten CNAME-Eintrags verwendet.

    :::image type="content" source="./media/dns-custom-domain/blob-storage.png" alt-text="Screenshot: Benutzerdefinierte Domänen für Speicherkonto":::

1. Navigieren Sie zu Ihrer DNS-Zone, und klicken Sie auf **+ Datensatzgruppe**. Geben Sie auf der Seite **Datensatzgruppe hinzufügen** die folgenden Informationen an, und klicken Sie auf **OK**, um sie zu erstellen.

    :::image type="content" source="./media/dns-custom-domain/storage-account-record.png" alt-text="Screenshot: Seite „Datensatzgruppe für Speicherkonto“":::

    | Eigenschaft | Wert | BESCHREIBUNG |
    | -------- | ----- | ----------- |
    | Name | asverify.mystorageaccount | Dieser Wert bildet zusammen mit der Domänennamenbezeichnung den FQDN für den Namen der benutzerdefinierten Domäne. |
    | type | CNAME | Verwenden Sie einen CNAME-Datensatz als Alias. |
    | TTL | 1 | 1 steht für 1 Stunde. |
    | TTL-Einheit | Stunden | Stunden werden als Maßeinheit für die Zeit verwendet. |
    | Alias | asverify.contoso.blob.core.windows.net | Der DNS-Name, für den Sie den Alias erstellen, ist in diesem Beispiel der standardmäßig für das Speicherkonto bereitgestellte DNS-Name „asverify.contoso.blob.core.windows.net“. |

1. Navigieren Sie zurück zum Speicherkonto, und klicken Sie auf **Netzwerk** und dann auf die Registerkarte **Benutzerdefinierte Domäne**. Geben Sie den von Ihnen erstellten Alias ohne das Präfix „asverify“ ein, aktivieren Sie **Indirekte CNAME-Überprüfung verwenden**, und klicken Sie auf **Speichern**. 

    :::image type="content" source="./media/dns-custom-domain/blob-storage-add-domain.png" alt-text="Screenshot: Seite &quot;Benutzerdefinierte Domäne hinzufügen&quot; im Speicherkonto":::

1. Kehren Sie zur DNS-Zone zurück, und erstellen Sie einen CNAME-Eintrag ohne das Präfix „asverify“.  Nach diesem Schritt können Sie den CNAME-Eintrag mit dem Präfix „asverify“ bedenkenlos löschen.

    :::image type="content" source="./media/dns-custom-domain/storage-account-record-set.png" alt-text="Screenshot: Speicherkontodatensatz ohne das Präfix „asverify“":::

1. Überprüfen Sie die DNS-Auflösung, indem Sie `nslookup` ausführen.

Weitere Informationen zum Zuordnen einer benutzerdefinierten Domäne zu einem Blob Storage-Endpunkt finden Sie unter [Konfigurieren eines benutzerdefinierten Domänennamens für Ihren Blob Storage-Endpunkt](../storage/blobs/storage-custom-domain-name.md?toc=%dns%2ftoc.json).

## <a name="azure-cdn"></a>Azure CDN

Die folgenden Schritte führen Sie durch die Konfiguration eines CNAME-Datensatzes für einen CDN-Endpunkt mit der cdnverify-Methode. Durch diese Methode wird sichergestellt, dass keine Ausfallzeiten auftreten.

1. Navigieren Sie zu Ihrem CDN-Profil, und wählen Sie den verwendeten Endpunkt aus. Wählen Sie **+ Benutzerdefinierte Domäne** aus. Beachten Sie den **Endpunkthostnamen**, da dieser Wert der Datensatz ist, auf den der CNAME-Datensatz zeigt.

    :::image type="content" source="./media/dns-custom-domain/cdn.png" alt-text="Screenshot: Seite „Benutzerdefinierte CDN-Domäne“":::

1. Navigieren Sie zu Ihrer DNS-Zone, und klicken Sie auf **+ Datensatzgruppe**. Geben Sie auf der Seite **Datensatzgruppe hinzufügen** die folgenden Informationen an, und klicken Sie auf **OK**, um sie zu erstellen.

    :::image type="content" source="./media/dns-custom-domain/cdn-record.png" alt-text="Screenshot: Seite „CDN-Datensatzgruppe“":::

    | Eigenschaft | Wert | BESCHREIBUNG |
    | -------- | ----- | ----------- |
    | Name | cdnverify.mycdnendpoint | Dieser Wert bildet zusammen mit der Domänennamenbezeichnung den FQDN für den Namen der benutzerdefinierten Domäne. |
    | type | CNAME | Verwenden Sie einen CNAME-Datensatz als Alias. |
    | TTL | 1 | 1 steht für 1 Stunde. |
    | TTL-Einheit | Stunden | Stunden werden als Maßeinheit für die Zeit verwendet. |
    | Alias | cdnverify.contoso.azureedge.net | Der DNS-Name, für den Sie den Alias erstellen, ist in diesem Beispiel der standardmäßig für den CDN-Endpunkt bereitgestellte DNS-Name „cdnverify.contoso.azureedge.net“. |

1. Navigieren Sie zurück zum CDN-Endpunkt, und klicken Sie auf **+ Benutzerdefinierte Domäne**. Geben Sie den Alias des CNAME-Eintrags ohne das Präfix „cdnverify“ ein, und klicken Sie auf **Hinzufügen**.

    :::image type="content" source="./media/dns-custom-domain/cdn-add.png" alt-text="Screenshot: Seite „Benutzerdefinierte Domäne hinzufügen“ für einen CDN-Endpunkt":::

1. Kehren Sie zur DNS-Zone zurück, und erstellen Sie einen CNAME-Eintrag ohne das Präfix „cdnverify“.  Nach diesem Schritt können Sie den CNAME-Eintrag mit dem Präfix „cdnverify“ bedenkenlos löschen.

    :::image type="content" source="./media/dns-custom-domain/cdn-record-set.png" alt-text="Screenshot: CDN-Datensatz ohne Präfix „cdnverify“":::

Weitere Informationen über CDN und die Konfiguration einer benutzerdefinierten Domäne ohne den zwischengeschalteten Registrierungsschritt finden Sie unter [Zuordnen von Azure CDN-Inhalten zu einer benutzerdefinierten Domäne](../cdn/cdn-map-content-to-custom-domain.md?toc=%dns%2ftoc.json).

## <a name="next-steps"></a>Nächste Schritte

Erfahren Sie, wie Sie [Reverse-DNS-Lookups für in Azure gehostete Dienste konfigurieren](dns-reverse-dns-for-azure-services.md).
