---
title: Authentifizieren bei Azure Key Vault
description: Es wird beschrieben, wie Sie sich bei Azure Key Vault authentifizieren.
author: ShaneBala-keyvault
ms.author: sudbalas
ms.date: 06/08/2020
ms.service: key-vault
ms.subservice: general
ms.topic: how-to
ms.openlocfilehash: 6336a0d4d8aa9c781befed0470d9a190af5aa9eb
ms.sourcegitcommit: 62e1884457b64fd798da8ada59dbf623ef27fe97
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/26/2020
ms.locfileid: "88930858"
---
# <a name="authenticate-to-azure-key-vault"></a>Authentifizieren bei Azure Key Vault

## <a name="overview"></a>Übersicht

Azure Key Vault ist eine Lösung für die Verwaltung von Geheimnissen, mit der Sie die Speicherung von Anwendungsgeheimnissen zentralisieren und deren Verteilung steuern können. Mit Azure Key Vault entfällt die Notwendigkeit, Anmeldeinformationen in Anwendungen zu speichern. Ihre Anwendung kann sich beim Schlüsseltresor authentifizieren, um die erforderlichen Anmeldeinformationen abzurufen. In diesem Dokument werden die Grundlagen der Authentifizierung beim Schlüsseltresor behandelt.

Die Informationen führen zu einem besseren Verständnis der Funktionsweise der Schlüsseltresorauthentifizierung. In diesem Dokument wird der Authentifizierungsfluss beschrieben und das Gewähren des Zugriffs auf Ihren Schlüsseltresor veranschaulicht. Es enthält darüber hinaus ein Tutorial zum Abrufen eines im Schlüsseltresor gespeicherten Geheimnisses aus einer Python-Beispielanwendung.

In diesem Dokument wird Folgendes behandelt:

* Wichtige Konzepte
* Registrierung des Sicherheitsprinzipals
* Grundlegendes zum Key Vault-Authentifizierungsfluss
* Gewähren des Zugriffs auf Key Vault für einen Dienstprinzipal
* Tutorial (Python)

## <a name="key-concepts"></a>Wichtige Konzepte

### <a name="azure-active-directory-concepts"></a>Azure Active Directory-Konzepte

* Azure Active Directory: Azure Active Directory (Azure AD, AAD) ist der cloudbasierte Identitäts- und Zugriffsverwaltungsdienst von Microsoft, mit dem sich Ihre Mitarbeiter anmelden und auf Ressourcen zugreifen können.

* Rollendefinition: Eine Rollendefinition ist eine Sammlung mit Berechtigungen.  AAD verfügt über Standardrollen („Besitzer“, „Mitwirkender“ oder „Leser“) mit Berechtigungsebenen, um Vorgänge wie das Lesen, Schreiben und Löschen für eine Azure-Ressource durchführen zu können. Rollen können auch von Benutzern erstellte benutzerdefinierte Definitionen sein, die bestimmte genauere Berechtigungen aufweisen.

* Anwendungsregistrierung: Wenn Sie eine Azure AD-Anwendung registrieren, werden in Ihrem Azure AD-Mandanten zwei Objekte erstellt, und zwar ein Anwendungsobjekt und ein Dienstprinzipalobjekt. Stellen Sie sich das Anwendungsobjekt als globale Darstellung Ihrer Anwendung zur Verwendung über alle Mandanten hinweg und den Dienstprinzipal als lokale Darstellung zur Verwendung in einem bestimmten Mandanten vor.

### <a name="security-principal-concepts"></a>Konzepte von Sicherheitsprinzipalen

* Sicherheitsprinzipal: Ein Sicherheitsprinzipal ist ein Objekt, das einen Benutzer, eine Gruppe, einen Dienstprinzipal oder eine verwaltete Identität darstellt, der bzw. die Zugriff auf Azure-Ressourcen anfordert.

* Benutzer: eine Person, die über ein Profil in Azure Active Directory verfügt.

* Gruppe: eine Reihe von Benutzern, die in Azure Active Directory erstellt wurden. Wenn Sie einer Gruppe eine Rolle zuweisen, verfügen alle Benutzer in dieser Gruppe über diese Rolle.

* Dienstprinzipal: eine Sicherheitsidentität, die von Anwendungen oder Diensten für den Zugriff auf bestimmte Azure-Ressourcen verwendet wird. Sie können sich dies als Benutzeridentität (Benutzername und Kennwort oder Zertifikat) für eine Anwendung vorstellen.

* Verwaltete Identität: Eine Identität in Azure Active Directory, die automatisch von Azure verwaltet wird.

* Objekt-ID (Client-ID): Ein eindeutiger von Azure AD generierter Bezeichner, der während der ersten Bereitstellung an einen Dienstprinzipal gebunden wird.

## <a name="security-principal-registration"></a>Registrierung des Sicherheitsprinzipals

1. Der Administrator registriert einen Benutzer oder eine Anwendung (Dienstprinzipal) in Azure Active Directory.

2. Der Administrator erstellt eine Azure Key Vault-Instanz und konfiguriert die Zugriffsrichtlinien (ACLs).

3. (Optional) Der Administrator konfiguriert die Azure Key Vault-Firewall.

![IMAGE](../media/authentication-1.png)

## <a name="understand-the-key-vault-authentication-flow"></a>Grundlegendes zum Key Vault-Authentifizierungsfluss

1. Ein Dienstprinzipal führt einen Aufruf zur Authentifizierung bei AAD durch. Dies kann auf unterschiedliche Arten geschehen:
    * Ein Benutzer kann sich beim Azure-Portal mit einem Benutzernamen und einem Kennwort anmelden.
    * Von einer Anwendung wird eine Client-ID verwendet und für AAD ein geheimer Clientschlüssel oder ein Clientzertifikat vorgewiesen.
    * Eine Azure-Ressource, z. B. ein virtueller Computer, verfügt über eine zugewiesene MSI und nimmt Kontakt mit dem IMDS-REST-Endpunkt auf, um ein Zugriffstoken zu erhalten.

2. Wenn die Authentifizierung bei AAD erfolgreich ist, wird dem Dienstprinzipal ein OAuth-Token gewährt.
3. Der Dienstprinzipal sendet einen Aufruf an Key Vault.
4. Von der Azure Key Vault-Firewall wird ermittelt, ob der Aufruf zulässig ist.
    * Szenario 1: Die Key Vault-Firewall ist deaktiviert, und der öffentliche Endpunkt (URI) des Schlüsseltresors ist über das öffentliche Internet erreichbar. Der Aufruf ist zulässig.
    * Szenario 2: Die aufrufende Funktion ist ein vertrauenswürdiger Azure Key Vault-Dienst. Bestimmte Azure-Dienste können die Firewall des Schlüsseltresors umgehen, wenn die Option ausgewählt ist. [Azure Key Vault: Liste mit vertrauenswürdigen Diensten](https://docs.microsoft.com/azure/key-vault/general/overview-vnet-service-endpoints#trusted-services)
    * Szenario 3: Die aufrufende Funktion ist in der Azure Key Vault-Firewall nach IP-Adresse, virtuellem Netzwerk oder Dienstendpunkt aufgelistet.
    * Szenario 4: Die aufrufende Funktion kann Azure Key Vault über eine konfigurierte Private Link-Verbindung erreichen.
    * Szenario 5: Die aufrufende Funktion ist nicht autorisiert, und die Antwort „Unzulässig“ wird zurückgegeben.
5. Von Key Vault wird AAD aufgerufen, um das Zugriffstoken des Dienstprinzipals zu überprüfen.
6. Key Vault überprüft, ob der Dienstprinzipal über ausreichende Zugriffsrichtlinienberechtigungen verfügt, um den angeforderten Vorgang durchführen zu können. In diesem Beispiel ist der Vorgang der Abruf des Geheimnisses.
7. Key Vault stellt das Geheimnis für den Dienstprinzipal bereit.

![IMAGE](../media/authentication-2.png)

## <a name="grant-a-service-principal-access-to-key-vault"></a>Gewähren des Zugriffs auf Key Vault für einen Dienstprinzipal

1. Erstellen Sie einen Dienstprinzipal, falls Sie noch keinen besitzen. [Erstellen eines Dienstprinzipals](https://docs.microsoft.com/azure/active-directory/develop/howto-create-service-principal-portal)
2. Fügen Sie Ihrem Dienstprinzipal in den IAM-Einstellungen von Azure Key Vault eine Rollenzuweisung hinzu. Sie können die vorab zugewiesenen Rollen „Besitzer“, „Mitwirkender“ oder „Leser“ hinzufügen. Darüber hinaus können Sie auch benutzerdefinierte Rollen für Ihren Dienstprinzipal erstellen. Sie sollten das Prinzip der geringsten Rechte befolgen und jeweils nur die minimalen Zugriffsrechte gewähren, die für Ihren Dienstprinzipal benötigt werden. 
3.  Konfigurieren Sie die Firewall für den Schlüsseltresor. Sie können die Firewall des Schlüsseltresors deaktiviert lassen und den Zugriff aus dem öffentlichen Internet zulassen (weniger sicher, einfacher zu konfigurieren). Sie können auch den Zugriff auf bestimmte IP-Adressbereiche, Dienstendpunkte, virtuelle Netzwerke oder private Endpunkte einschränken (sicherer).
4.  Fügen Sie eine Zugriffsrichtlinie für Ihren Dienstprinzipal hinzu. Dies ist eine Liste mit Vorgängen, die von Ihrem Dienstprinzipal im Schlüsseltresor durchgeführt werden können. Sie sollten das Prinzip der geringsten Rechte befolgen und die Vorgänge begrenzen, die vom Dienstprinzipal durchgeführt werden können. Falls Sie aber nicht über ausreichende Berechtigungen verfügen, wird der Zugriff für den Dienstprinzipal verweigert.

## <a name="tutorial"></a>Lernprogramm

In diesem Tutorial wird beschrieben, wie Sie einen Dienstprinzipal für das Authentifizieren beim Schlüsseltresor und das Abrufen eines Geheimnisses einrichten. 

### <a name="part-1--create-a-service-principal-in-the-azure-portal"></a>Teil 1:  Erstellen eines Dienstprinzipals im Azure-Portal

1. Anmelden beim Azure-Portal
1. Suchen Sie nach „Azure Active Directory“.
1. Klicken Sie auf die Registerkarte „App-Registrierungen“.
1. Klicken Sie auf „+ Neue Registrierung“.
1. Erstellen Sie einen Namen für den Dienstprinzipal.
1. Wählen Sie „Registrieren“ aus.

Sie verfügen nun über einen registrierten Dienstprinzipal. Sie können ihn anzeigen, indem Sie die Option „App-Registrierungen“ auswählen. Ihr Dienstprinzipal wird nun einer Client-ID-GUID zugewiesen. Sie können sich dies wie einen „Benutzernamen“ für Ihren Dienstprinzipal vorstellen. Nun müssen Sie ein „Kennwort“ für Ihren Dienstprinzipal erstellen. Hierfür können Sie einen geheimen Clientschlüssel oder ein Clientzertifikat verwenden. Beachten Sie, dass die Verwendung eines geheimen Clientschlüssels für die Authentifizierung nicht sicher ist und nur zu Testzwecken erfolgen sollte. In diesem Tutorial wird veranschaulicht, wie Sie ein Clientzertifikat verwenden.

### <a name="part-2-create-a-client-certificate-for-your-service-principal"></a>Teil 2: Erstellen eines Clientzertifikats für Ihren Dienstprinzipal

1. Erstellen eines Zertifikats

    * Option 1: Erstellen Sie ein Zertifikat über [OpenSSL](https://www.openssl.org/) (nur für Testzwecke, selbstsignierte Zertifikate sollten nicht in der Produktion verwendet werden).
    * Option 2: Erstellen Sie über den Schlüsseltresor ein Zertifikat. [Erstellen eines Zertifikats in Azure Key Vault](https://docs.microsoft.com/azure/key-vault/certificates/certificate-scenarios#creating-your-first-key-vault-certificate)

1. Herunterladen des Zertifikats im PEM-/PFX-Format
1. Melden Sie sich beim Azure-Portal an, und navigieren Sie zu Azure Active Directory.
1. Klicken Sie auf „App-Registrierungen“.
1. Wählen Sie den in Teil 1 erstellten Dienstprinzipal aus.
1. Klicken Sie auf die Registerkarte „Zertifikate und Geheimnisse“ Ihres Dienstprinzipals.
1. Laden Sie das Zertifikat hoch, indem Sie die Schaltfläche „Zertifikat hochladen“ verwenden.

### <a name="part-3-configure-an-azure-key-vault"></a>Teil 3: Konfigurieren einer Azure Key Vault-Instanz

1. Erstellen Sie eine Azure Key Vault-Instanz ([Link](https://docs.microsoft.com/azure/key-vault/secrets/quick-create-portal#create-a-vault)).

2. Konfigurieren Sie die IAM-Berechtigungen für Key Vault.
    1. Navigieren zum Schlüsseltresor
    1. Wählen Sie die Registerkarte „Zugriffssteuerung (IAM)“ aus.
    1. Klicken Sie auf „Rollenzuweisung hinzufügen“.
    1. Wählen Sie in der Dropdownliste die Option „Mitwirkender“ aus.
    1. Geben Sie den Namen oder die Client-ID des von Ihnen erstellten Dienstprinzipals ein.
    1. Klicken Sie auf „Rollenzuweisungen anzeigen“, um zu überprüfen, ob Ihr Dienstprinzipal aufgeführt ist.

3. Konfigurieren Sie die Zugriffsrichtlinienberechtigungen für Key Vault.
    1. Navigieren zum Schlüsseltresor
    1. Wählen Sie unter „Einstellungen“ die Registerkarte „Zugriffsrichtlinien“ aus.
    1. Wählen Sie den Link „+ Zugriffsrichtlinie hinzufügen“ aus.
    1. Aktivieren Sie in der Dropdownliste „Geheimnisberechtigungen“ die Berechtigungen „Abrufen“ und „Auflisten“.
    1. Wählen Sie Ihren Dienstprinzipal anhand des Namens oder der Client-ID aus.
    1. Wählen Sie „Hinzufügen“ aus.
    1. Wählen Sie „Speichern“ aus.

4. Erstellen Sie in Ihrem Schlüsseltresor ein Geheimnis.
    1. Navigieren zum Schlüsseltresor
    1. Klicken Sie unter „Einstellungen“ auf die Registerkarte „Geheimnisse“.
    1. Klicken Sie auf „+ Generieren/Importieren“.
    1. Erstellen Sie einen Namen für das Geheimnis. In diesem Beispiel verwenden wir „test“ als Namen.
    1. Erstellen Sie einen Wert für das Geheimnis. In diesem Beispiel wird der Wert auf „password123“ festgelegt.

Wenn Sie nun Code über Ihren lokalen Computer ausführen, können Sie sich gegenüber dem Schlüsseltresor authentifizieren, indem Sie ein Zugriffstoken abrufen. Hierfür müssen Sie die Client-ID und einen Pfad zum Zertifikat vorweisen.

### <a name="part-4-retrieve-the-secret-from-your-azure-key-vault-in-an-application-python"></a>Teil 4: Abrufen des Geheimnisses aus Ihrer Azure Key Vault-Instanz in einer Anwendung (Python)

Verwenden Sie das folgende Codebeispiel, um zu testen, ob Ihre Anwendung mit dem von Ihnen konfigurierten Dienstprinzipal ein Geheimnis aus Ihrem Schlüsseltresor abrufen kann. 

```python
from azure.keyvault.secrets import SecretClient
from azure.identity import CertificateCredential


tenant_id = ""                                             ##ENTER AZURE TENANT ID
vault_url = "https://{VAULT NAME}.vault.azure.net/"        ##ENTER THE URL OF YOUR KEY VAULT
client_id = ""                                             ##ENTER CLIENT ID OF SERVICE PRINCIPAL
cert_path = r"C:\Users\{USERNAME}\{PATH}\{CERT_NAME}.pem"  ##ENTER PATH TO CERTIFICATE

def main():

    #AUTHENTICATION TO AAD USING CLIENT ID AND CLIENT CERTIFICATE
    token = CertificateCredential(tenant_id= tenant_id, client_id=client_id, certificate_path=cert_path)

    #AUTHENTICATION TO KEY VAULT PRESENTING AAD TOKEN
    client = SecretClient(vault_url=vault_url, credential=token)

    #CALL TO KEY VAULT TO GET SECRET
    secret = client.get_secret('{SECRET_NAME}')            ##ENTER NAME OF SECRET IN KEY VAULT

    #GET PLAINTEXT OF SECRET
    print(secret.value)

#CALL MAIN()
if __name__ == "__main__":
    main()
```

![IMAGE](../media/authentication-3.png)


## <a name="next-steps"></a>Nächste Schritte

1. Informieren Sie sich, wie Sie Fehler beheben, die bei der Authentifizierung beim Schlüsseltresor auftreten. [Leitfaden zur Problembehandlung für Key Vault](https://docs.microsoft.com/azure/key-vault/general/rest-error-codes)
