---
title: Vom Kunden verwaltete Schlüssel oder BYOK (Bring Your Own Key) mit Media Services
description: Dies ist ein Tutorial zur Verwendung von vom Kunden verwalteten Schlüsseln mit einem Media Services-Speicherkonto.
author: IngridAtMicrosoft
ms.author: inhenkel
ms.service: media-services
ms.topic: tutorial
ms.date: 10/18/2020
ms.openlocfilehash: c26da9d888bbcdf72c052fa4bd7fcdf443a2d8f7
ms.sourcegitcommit: ce8eecb3e966c08ae368fafb69eaeb00e76da57e
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/21/2020
ms.locfileid: "92325860"
---
# <a name="tutorial-use-customer-managed-keys-or-bring-your-own-key-byok-with-media-services"></a>Tutorial: Verwenden von vom Kunden verwalteten Schlüsseln oder BYOK (Bring Your Own Key) mit Media Services

Mit der 2020-05-01-API können Sie einen vom Kunden verwalteten RSA-Schlüssel mit einem Media Services-Konto verwenden, das eine vom System verwaltete Identität aufweist.  Dieses Tutorial enthält eine Postman-Sammlung und -Umgebung zum Senden von REST-Anforderungen an Azure-Dienste.  Verwendete Dienste:

- Azure Active Directory-Anwendungsregistrierung für Postman
- Microsoft Graph-API
- Azure-Speicher
- Azure Key Vault
- Media Services

In diesem Tutorial erfahren Sie, wie Sie Postman für die folgenden Zwecke nutzen:

> [!div class="checklist"]
> * Abrufen von Token für die Verwendung mit Azure-Diensten.
> * Erstellen einer Ressourcengruppe und eines Speicherkontos.
> * Erstellen eines Media Services-Kontos mit einer vom System verwalteten Identität
> * Erstellen eines Schlüsseltresors zum Speichern eines (vom Kunden verwalteten) RSA-Schlüssels zur Verwendung mit dem Speicherkonto.
> * Aktualisieren des Media Services-Kontos für die Verwendung des RSA-Schlüssels mit dem Speicherkonto.
> * Verwenden von Variablen in Postman.

Falls Sie noch nicht über ein Azure-Abonnement verfügen, können Sie ein [kostenloses Testkonto](https://azure.microsoft.com/free/) erstellen.

## <a name="prerequisites"></a>Voraussetzungen

- Registrieren Sie einen Dienstprinzipal mit den entsprechenden Berechtigungen.
- Installieren Sie [Postman](https://www.postman.com).
- Laden Sie die Postman-Sammlung für dieses Tutorial unter [Azure-Beispiele: media-services-customer-managed-keys-byok](https://github.com/Azure-Samples/media-services-customer-managed-keys-byok) herunter.

### <a name="register-a-service-principal-with-the-needed-permissions"></a>Registrieren Sie einen Dienstprinzipal mit den erforderlichen Berechtigungen

[Erstellen Sie einen Dienstprinzipal](https://docs.microsoft.com/azure/active-directory/develop/howto-create-service-principal-portal).  Unter [Option zwei](https://docs.microsoft.com/azure/active-directory/develop/howto-create-service-principal-portal#authentication-two-options) finden Sie Informationen zum Abrufen des Dienstprinzipalgeheimnisses.  Notieren Sie sich das Geheimnis an einem geeigneten Ort, da Sie darauf nicht mehr zugreifen können, nachdem Sie die Geheimnisseite im Portal einmal verlassen haben.

Der Dienstprinzipal benötigt die folgenden Berechtigungen zum Durchführen der Aufgaben in diesem Tutorial.

![Die erforderlichen Berechtigungen für den Dienstprinzipal](./media/tutorial-byok/service-principal-permissions-1.png)

### <a name="install-postman"></a>Installieren von Postman

Wenn Sie Postman noch nicht für die Verwendung mit Azure installiert haben, können Sie es unter [postman.com](https://www.postman.com/) abrufen.

### <a name="download-and-import-the-collection"></a>Herunterladen und Importieren der Sammlung

Laden Sie die Postman-Sammlung für dieses Tutorial unter [Azure-Beispiele: media-services-customer-managed-keys-byok](https://github.com/Azure-Samples/media-services-customer-managed-keys-byok) herunter.

## <a name="installation-of-collection-and-environment"></a>Installation von Sammlung und Umgebung

1. Führen Sie Postman aus.
1. Wählen Sie **Importieren** aus.
1. Wählen Sie **Upload files** (Dateien hochladen) aus.
1. Navigieren Sie zum Speicherort der Sammlungs- und Umgebungsdateien.
1. Wählen Sie die Sammlungs- und Umgebungsdatei aus.
1. Wählen Sie **Open** (Öffnen).  (Es wird eine Warnung angezeigt, dass die Dateien nicht als API sondern als Sammlungen importiert werden.  Das geht ist in Ordnung.  Es ist das gewünschte Verhalten.)
1. Diese Sammlung wird nun in ihren Sammlungen als BYOK angezeigt.
1. Die Umgebungsvariablen werden nun in ihren Umgebungen angezeigt.

### <a name="understand-the-rest-api-requests-in-the-collection"></a>Informationen zu den REST-API-Anforderungen in der Sammlung

Die Sammlung stellt die folgenden REST-API-Anforderungen zur Verfügung. Die Anforderungen müssen in der bestehenden Reihenfolge gesendet werden, da die meisten von ihnen über Testskripts verfügen, die dynamisch globale Variablen für die nächste (oder nachfolgende) Anforderung in der Reihe erstellen. Es ist nicht erforderlich, die globalen Variablen manuell zu erstellen.

In Postman werden diese Variablen in `{{ }}` eckigen Klammern angezeigt.  Beispiel: `{{bearerToken}}`.

1. Get AAD token (AAD-Token abrufen): Der Test legt die globale Variable *bearerToken* fest
2. Get Graph token (Graph-Token abrufen): Der Test legt die globale Variable *graphToken* fest
3. Get Service Principal Details (Dienstprinzipaldetails abrufen): Der Text legt die globale Variable *servicePrincipalObjectId* fest
4. Create a storage account (Speicherkonto erstellen): Der Test legt die globale Variable *storageAccountId* fest
5. Create a Media Services Account with a System Managed Identity (Media Services Konto mit einer vom System verwalteten Identität erstellen): Der Test legt die globale Variable *principalId* fest
6. Create a Key Vault, granting access to the service principal (Schlüsseltresor erstellen und Zugriff auf den Dienstprinzipal erteilen): Der Test legt die globale Variable *keyVaultId* fest
7. Get a Key Vault token (Schlüsseltresortoken abrufen): Der Test legt die globale Variable *keyVaultToken* fest
8. Create RSA key in the key vault (RSA-Schlüssel im Schlüsseltresor erstellen): Der Test legt die globale Variable *keyId* fest
9. Update the Media Services account to use the key with the storage account (Media Services-Konto zum Verwenden des Schlüssels mit dem Speicherkonto aktualisieren): Es gibt kein Testskript für diese Anforderung.

## <a name="define-environment-variables"></a>Definieren von Umgebungsvariablen

1. Wechseln Sie zu der heruntergeladenen Umgebung, indem Sie sie in der Dropdownliste der Umgebungen auswählen.
1. Richten Sie Ihre Umgebungsvariablen in Postman ein. Sie werden ebenfalls als in eckige Klammern `{{ }}` eingeschlossene Variablen verwendet.  Beispiel: `{{tenantId}}`.

    * *tenantId* = Ihre Mandanten-ID
    * *servicePrincipalId* = die ID des Dienstprinzipals, den Sie mit Ihrer bevorzugten Methode einrichten: Portal, CLI usw.
    * *servicePrincipalSecret* = das für den Dienstprinzipal erstellte Geheimnis
    * *subscription* = Ihre Abonnement-ID
    * *storageName* = der Name, den Sie Ihrem Speicher geben möchten
    * *accountName* = der Name des Media Service-Kontos, den Sie verwenden möchten
    * *keyVaultName* = der Schlüsseltresorname, den Sie verwenden möchten
    * *resourceLocation* = centralus (oder wo immer Sie Ihre Ressourcen platzieren möchten.  Diese Sammlung wurde nur mit centralus (USA, Mitte) getestet.)
    * *resourceGroup* = der Name der Ressourcengruppe

    Die folgenden Ressourcen stellen die Norm für das Arbeiten mit Azure-Ressourcen dar, daher gibt es keinen Grund, sie zu ändern.

    * *armResource* = `https://management.core.windows.net`
    * *graphResource* = `https://graph.windows.net/`
    * *keyVaultResource* = `https://vault.azure.net`
    * *armEndpoint* = `management.azure.com`
    * *graphEndpoint* = `graph.windows.net`
    * *aadEndpoint* = `login.microsoftonline.com`
    * *keyVaultDomainSuffix* = `vault.azure.net`

## <a name="send-the-requests"></a>Senden der Anforderungen

Nachdem Sie Ihre Umgebungsvariablen definiert haben, können Sie die Anforderungen entweder einzeln in der oben angegebenen Reihenfolge ausführen oder das Ausführungsprogramm von Postman verwenden, um die Sammlung auszuführen.

## <a name="change-the-key"></a>Ändern des Schlüssels

Media Services erkennt automatisch, wenn der Schlüssel geändert wurde.  Um dies zu testen, erstellen Sie eine weitere Schlüsselversion für denselben Schlüssel. Media Services sollte diesen Schlüssel in weniger als 15 Minuten erkennen.

## <a name="clean-up-resources"></a>Bereinigen von Ressourcen

Wenn Sie die von Ihnen erstellten Ressourcen nicht weiter verwenden möchten und **sie Ihnen nicht mehr in Rechnung gestellt werden sollen** , löschen Sie sie.

## <a name="next-steps"></a>Nächste Schritte

Fahren Sie mit dem nächsten Artikel fort, und lernen Sie das...
> [!div class="nextstepaction"]
> [Codieren einer Remotedatei anhand einer URL und Streamen des Videos über REST](stream-files-tutorial-with-rest.md)
