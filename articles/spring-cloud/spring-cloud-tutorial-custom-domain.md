---
title: 'Tutorial: Zuordnen einer bereits vorhandenen benutzerdefinierten Domäne zu Azure Spring Cloud'
description: Hier erfahren Sie, wie Sie Azure Spring Cloud einen bereits vorhandenen benutzerdefinierten DNS-Namen (Distributed Name Service) zuordnen.
author: MikeDodaro
ms.service: spring-cloud
ms.topic: tutorial
ms.date: 03/19/2020
ms.author: brendm
ms.custom: devx-track-java
ms.openlocfilehash: d6e4b77c6eba976de052295e4a0001924ad90644
ms.sourcegitcommit: f353fe5acd9698aa31631f38dd32790d889b4dbb
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/29/2020
ms.locfileid: "87374200"
---
# <a name="map-an-existing-custom-domain-to-azure-spring-cloud"></a>Zuordnen einer bereits vorhandenen benutzerdefinierten Domäne zu Azure Spring Cloud
DNS (Domain Name Service) wird verwendet, um Namen von Netzwerkknoten im gesamten Netzwerk zu speichern. In diesem Tutorial wird eine Domäne (beispielsweise „www.contoso.com“) mithilfe eines CNAME-Eintrags zugeordnet. Die benutzerdefinierte Domäne wird durch ein Zertifikat geschützt, und Sie erfahren, wie Sie Transport Layer Security (TLS) erzwingen, was auch als Secure Sockets Layer (SSL) bezeichnet wird. 

Zertifikate dienen zur Verschlüsselung von Webdatenverkehr. Diese TLS-/SSL-Zertifikate können in Azure Key Vault gespeichert werden. 

## <a name="prerequisites"></a>Voraussetzungen
* Eine in Azure Spring Cloud bereitgestellte Anwendung. Lesen Sie dazu entweder den Artikel [Schnellstart: Starten einer vorhandenen Azure Spring Cloud-Anwendung über das Azure-Portal](spring-cloud-quickstart-launch-app-portal.md), oder verwenden Sie eine bereits vorhandene App.
* Ein Domänenname mit Zugriff auf die DNS-Registrierung für den Domänenanbieter (beispielsweise GoDaddy).
* Ein privates Zertifikat (d. h. ihr selbst signiertes Zertifikat) von einem Drittanbieter. Das Zertifikat muss der Domäne entsprechen.
* Eine bereitgestellte Instanz von [Azure Key Vault](https://docs.microsoft.com/azure/key-vault/key-vault-overview).

## <a name="import-certificate"></a>Importieren des Zertifikats 
Zum Importieren eines Zertifikats muss sich die PEM- oder PFX-codierte Datei auf dem Datenträger befinden, und Sie müssen über den privaten Schlüssel verfügen. 

So laden Sie Ihr Zertifikat in den Schlüsseltresor hoch:
1. Navigieren Sie zu Ihrer Schlüsseltresorinstanz.
1. Klicken Sie im linken Navigationsbereich auf **Zertifikate**.
1. Klicken Sie im oberen Menü auf **Generieren/importieren**.
1. Wählen Sie im Dialogfeld **Zertifikat erstellen** unter **Methode der Zertifikaterstellung** die Option `Import` aus.
1. Navigieren Sie unter **Zertifikatdatei hochladen** zum Speicherort des Zertifikats, und wählen Sie die Datei aus.
1. Geben Sie unter **Kennwort** den privaten Schlüssel für Ihr Zertifikat ein.
1. Klicken Sie auf **Erstellen**.

    ![Importieren des Zertifikats 1](./media/custom-dns-tutorial/import-certificate-a.png)

So gewähren Sie Azure Spring Cloud Zugriff auf Ihren Schlüsseltresor, bevor Sie das Zertifikat importieren
1. Navigieren Sie zu Ihrer Schlüsseltresorinstanz.
1. Klicken Sie im linken Navigationsbereich auf **Zugriffsrichtlinie**.
1. Klicken Sie im oberen Menü auf **Zugriffsrichtlinie hinzufügen**.
1. Geben Sie die Informationen ein, und klicken Sie auf die Schaltfläche **Hinzufügen**. Dann **speichern** Sie die Zugriffsrichtlinie.

| Geheimnisberechtigung | Zertifikatsberechtigung | Prinzipal auswählen |
|--|--|--|
| Get, List | Get, List | Azure Spring Cloud-Domänenverwaltung |

![Importieren des Zertifikats 2](./media/custom-dns-tutorial/import-certificate-b.png)

Sie können auch die Azure-Befehlszeilenschnittstelle verwenden, um Azure Spring Cloud Zugriff auf den Schlüsseltresor zu gewähren.

Die Objekt-ID erhalten Sie mit dem folgenden Befehl.
```
az ad sp show --id 03b39d0f-4213-4864-a245-b1476ec03169 --query objectId
```

Gewähren Sie Azure Spring Cloud Lesezugriff auf den Schlüsseltresor, ersetzen Sie die Objekt-ID im folgenden Befehl.
```
az keyvault set-policy -g <key vault resource group> -n <key vault name>  --object-id <object id> --certificate-permissions get list --secret-permissions get list
``` 

So importieren Sie das Zertifikat in Azure Spring Cloud:
1. Navigieren Sie zu Ihrer Dienstinstanz. 
1. Wählen Sie im linken Navigationsbereich Ihrer App die Option **TLS-/SSL-Einstellungen** aus.
1. Klicken Sie anschließend auf **Key Vault-Zertifikat importieren**.

    ![Importieren des Zertifikats](./media/custom-dns-tutorial/import-certificate.png)

Sie können auch die Azure-Befehlszeilenschnittstelle verwenden, um das Zertifikat zu importieren:

```
az spring-cloud certificate add --name <cert name> --vault-uri <key vault uri> --vault-certificate-name <key vault cert name>
```

> [!IMPORTANT] 
> Stellen Sie sicher, dass Sie Azure Spring Cloud Zugriff auf Ihren Schlüsseltresor gewähren, bevor Sie den vorherigen Befehl zum Importieren von Zertifikaten ausführen. Wenn Sie dies noch nicht erledigt haben, können Sie den folgenden Befehl ausführen, um die Zugriffsrechte zu gewähren.

```
az keyvault set-policy -g <key vault resource group> -n <key vault name>  --object-id 938df8e2-2b9d-40b1-940c-c75c33494239 --certificate-permissions get list
``` 

Wenn das Zertifikat erfolgreich importiert wurde, wird es in der Liste **Private Schlüsselzertifikate** angezeigt.

![Privates Schlüsselzertifikat](./media/custom-dns-tutorial/key-certificates.png)

Sie können auch die Azure-Befehlszeilenschnittstelle verwenden, um eine Liste von Zertifikaten anzuzeigen:

```
az spring-cloud certificate list
```

> [!IMPORTANT] 
> Das Zertifikat muss noch an eine benutzerdefinierte Domäne gebunden werden, um sie mit diesem Zertifikat zu schützen. Führen Sie dazu die in diesem Dokument unter **Hinzufügen von SSL-Bindung** beschriebenen Schritte aus.

## <a name="add-custom-domain"></a>Hinzufügen einer benutzerdefinierten Domäne
Sie können einen CNAME-Eintrag verwenden, um Azure Spring Cloud einen benutzerdefinierten DNS-Namen zuzuordnen. 

> [!NOTE] 
> Der A-Eintrag wird nicht unterstützt. 

### <a name="create-the-cname-record"></a>Erstellen des CNAME-Eintrags
Navigieren Sie zu Ihrem DNS-Anbieter, und fügen Sie einen CNAME-Eintrag hinzu, um Ihre Domäne Folgendem zuzuordnen: <Dienstname>.azuremicroservices.io. „<Dienstname>“ ist hierbei der Name Ihrer Azure Spring Cloud-Instanz. Für Domäne und Unterdomäne werden Platzhalter unterstützt. Nach dem Hinzufügen des CNAME-Eintrags sieht die Seite mit den DNS-Einträgen in etwa wie folgt aus: 

![Seite der DNS-Einträge](./media/custom-dns-tutorial/dns-records.png)

## <a name="map-your-custom-domain-to-azure-spring-cloud-app"></a>Zuordnen Ihrer benutzerdefinierten Domäne zur Azure Spring Cloud-App
Sollten Sie über keine Anwendung in Azure Spring Cloud verfügen, gehen Sie wie unter [Schnellstart: Starten einer vorhandenen Azure Spring Cloud-Anwendung über das Azure-Portal](https://review.docs.microsoft.com/azure/spring-cloud/spring-cloud-quickstart-launch-app-portal?branch=master) vor.

Navigieren Sie zur Anwendungsseite.

1. Wählen Sie **Benutzerdefinierte Domäne** aus.
2. Wählen Sie **Benutzerdefinierte Domäne hinzufügen** aus. 

    ![Benutzerdefinierte Domäne](./media/custom-dns-tutorial/custom-domain.png)

3. Geben Sie den vollqualifizierten Domänennamen ein, für den Sie einen CNAME-Eintrag hinzugefügt haben (beispielsweise „www.contoso.com“). Vergewissern Sie sich, dass der Typ des Hostnamenseintrags auf „CNAME“ (<Dienstname>.azuremicroservices.io) festgelegt ist.
4. Klicken Sie auf **Überprüfen**, um die Schaltfläche **Hinzufügen** zu aktivieren.
5. Klicken Sie auf **Hinzufügen**.

    ![Hinzufügen einer benutzerdefinierten Domäne](./media/custom-dns-tutorial/add-custom-domain.png)

Sie können auch die Azure-Befehlszeilenschnittstelle verwenden, um eine benutzerdefinierte Domäne hinzuzufügen:
```
az spring-cloud app custom-domain bind --domain-name <domain name> --app <app name> 
```

Eine App kann über mehrere Domänen verfügen, eine Domäne kann jedoch nur einer einzelnen App zugeordnet werden. Wenn Sie Ihre benutzerdefinierte Domäne erfolgreich der App zugeordnet haben, wird sie in der Tabelle mit den benutzerdefinierten Domänen angezeigt.

![Tabelle mit benutzerdefinierten Domänen](./media/custom-dns-tutorial/custom-domain-table.png)

Sie können auch die Azure-Befehlszeilenschnittstelle verwenden, um eine Liste von benutzerdefinierten Domänen anzuzeigen:
```
az spring-cloud app custom-domain list --app <app name> 
```

> [!NOTE]
> Ist Ihre benutzerdefinierte Domäne mit **Nicht sicher** gekennzeichnet, wurde sie noch nicht an ein SSL-Zertifikat gebunden. Bei HTTPS-Anforderungen, die von einem Browser an Ihre benutzerdefinierte Domäne gerichtet werden, wird ein Fehler oder eine Warnung zurückgegeben.

## <a name="add-ssl-binding"></a>Hinzufügen von SSL-Bindung
Wählen Sie in der Tabelle mit den benutzerdefinierten Domänen die Option **Add ssl binding** (SSL-Bindung hinzufügen) aus, wie in der vorherigen Abbildung zu sehen.  
1. Wählen Sie unter **Zertifikat** Ihr Zertifikat aus, oder importieren Sie es.
1. Klicken Sie auf **Speichern**.

    ![Hinzufügen von SSL-Bindung](./media/custom-dns-tutorial/add-ssl-binding.png)

Sie können auch die Azure-Befehlszeilenschnittstelle verwenden, um die **SSL-Bindung hinzuzufügen**:
```
az spring-cloud app custom-domain update --domain-name <domain name> --certificate <cert name> --app <app name> 
```

Nach erfolgreichem Hinzufügen der SSL-Bindung ist die Domäne sicher und wird mit folgendem Zustand gekennzeichnet: **Fehlerfrei**. 

![Hinzufügen von SSL-Bindung](./media/custom-dns-tutorial/secured-domain-state.png)

## <a name="enforce-https"></a>Erzwingen von HTTPS
Standardmäßig können Benutzer weiterhin über HTTP auf Ihre App zugreifen, alle HTTP-Anforderungen können jedoch an den HTTPS-Port umgeleitet werden.

Wählen Sie im linken Navigationsbereich Ihrer App-Seite die Option **Benutzerdefinierte Domäne** aus. Legen Sie **Nur HTTPS** auf *True* fest.

![Hinzufügen von SSL-Bindung](./media/custom-dns-tutorial/enforce-http.png)

Sie können auch die Azure-Befehlszeilenschnittstelle verwenden, um HTTPS zu erzwingen:
```
az spring-cloud app update -name <app-name> --https-only <true|false> -g <resource group> --service <service-name>
```

Navigieren Sie nach Abschluss des Vorgangs zu einer beliebigen HTTPS-URL, die auf Ihre App verweist. Beachten Sie, dass HTTP-URLs nicht funktionieren.

## <a name="see-also"></a>Weitere Informationen
* [Was ist der Azure-Schlüsseltresor?](https://docs.microsoft.com/azure/key-vault/key-vault-overview)
* [Importieren eines Zertifikats](https://docs.microsoft.com/azure/key-vault/certificate-scenarios#import-a-certificate)
* [Starten einer Spring Cloud-App mit der Azure CLI](https://docs.microsoft.com/azure/spring-cloud/spring-cloud-quickstart-launch-app-cli)

