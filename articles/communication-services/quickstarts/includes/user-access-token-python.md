---
title: include file
description: include file
services: azure-communication-services
author: matthewrobertson
manager: nimag
ms.service: azure-communication-services
ms.subservice: azure-communication-services
ms.date: 08/20/2020
ms.topic: include
ms.custom: include file
ms.author: marobert
ms.openlocfilehash: 4be8821a949527fefcc9005b1de7f4f7c438c568
ms.sourcegitcommit: eb6bef1274b9e6390c7a77ff69bf6a3b94e827fc
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/05/2020
ms.locfileid: "90944585"
---
## <a name="prerequisites"></a>Voraussetzungen

- Ein Azure-Konto mit einem aktiven Abonnement. Sie können [kostenlos ein Konto erstellen](https://azure.microsoft.com/free/?WT.mc_id=A261C142F).
- [Python](https://www.python.org/downloads/) 2.7, 3.5 oder höher.
- Eine aktive Communication Services-Ressource und eine Verbindungszeichenfolge. [Erstellen Sie eine Communication Services-Ressource.](../create-communication-resource.md)

## <a name="setting-up"></a>Einrichten

### <a name="create-a-new-python-application"></a>Erstellen einer neuen Python-Anwendung

1. Öffnen Sie Ihr Terminal- oder Befehlsfenster, erstellen Sie ein neues Verzeichnis für Ihre App, und navigieren Sie zu diesem Verzeichnis.

   ```console
   mkdir user-tokens-quickstart && cd user-tokens-quickstart
   ```

1. Erstellen Sie mithilfe eines Text-Editors eine Datei mit dem Namen **issue-tokens.py** im Stammverzeichnis des Projekts, und fügen Sie die Struktur für das Programm hinzu (einschließlich einer einfachen Ausnahmebehandlung). In den folgenden Abschnitten wird dieser Datei der gesamte Quellcode für diese Schnellstartanleitung hinzugefügt.

   ```python
   import os
   from azure.communication.administration import CommunicationIdentityClient

   try:
      print('Azure Communication Services - User Access Tokens Quickstart')
      # Quickstart code goes here
   except Exception as ex:
      print('Exception:')
      print(ex)
   ```

### <a name="install-the-package"></a>Installieren des Pakets

Installieren Sie im Anwendungsverzeichnis mithilfe des Befehls `pip install` das Python-Paket der Azure Communication Services-Clientbibliothek „Administration“.

```console
pip install azure-communication-administration
```

[!INCLUDE [User Access Tokens Object Model](user-access-tokens-object-model.md)]

## <a name="authenticate-the-client"></a>Authentifizieren des Clients

Instanziieren Sie einen Kommunikationsidentitätsclient (`CommunicationIdentityClient`) mit Ihrer Verbindungszeichenfolge. Im folgenden Code wird die Verbindungszeichenfolge für die Ressource aus einer Umgebungsvariablen namens `COMMUNICATION_SERVICES_CONNECTION_STRING` abgerufen. Informationen zur Verwaltung der Verbindungszeichenfolge Ihrer Ressource finden Sie [hier](../create-communication-resource.md#store-your-connection-string).

Fügen Sie diesen Code im `try`-Block hinzu:

```python
# This code demonstrates how to fetch your connection string
# from an environment variable.
connection_string = os.environ['COMMUNICATION_SERVICES_CONNECTION_STRING']

# Instantiate the identity client
client = CommunicationIdentityClient.from_connection_string(connection_string)
```

## <a name="create-a-user"></a>Erstellen eines Benutzers

Von Azure Communication Services wird ein einfaches Identitätsverzeichnis gepflegt. Verwenden Sie die Methode `create_user`, um in dem Verzeichnis einen neuen Eintrag mit einer eindeutigen `Id` zu erstellen. Es empfiehlt sich, eine Zuordnung zwischen den Benutzern Ihrer Anwendung und den von Communication Services generierten Identitäten zu pflegen (etwa durch Speichern in der Datenbank Ihres Anwendungsservers).

```python
user = client.create_user()
print("\nCreated a user with ID: " + user.identifier + ":")
```

## <a name="issue-user-access-tokens"></a>Ausstellen von Benutzerzugriffstoken

Verwenden Sie die Methode `issue_token`, um ein Zugriffstoken für einen Communication Services-Benutzer auszustellen. Ohne Angabe des optionalen Parameters `user` wird ein neuer Benutzer erstellt und mit dem Token zurückgegeben.

```python
# Issue an access token with the "voip" scope for a new user
token_result = client.issue_token(user, ["voip"])
expires_on = token_result.expires_on.strftime('%d/%m/%y %I:%M %S %p')
print("\nIssued a token with 'voip' scope that expires at " + expires_on + ":")
print(token_result.token)
```

Benutzerzugriffstoken sind kurzlebige Anmeldeinformationen, die neu ausgestellt werden müssen, um Dienstunterbrechungen für Ihre Benutzer zu vermeiden. Die Antworteigenschaft `expires_on` gibt die Lebensdauer des Tokens an.

## <a name="revoke-user-access-tokens"></a>Widerrufen von Benutzerzugriffstoken

Manchmal müssen Benutzerzugriffstoken ggf. explizit widerrufen werden – etwa, wenn ein Benutzer das Kennwort ändert, mit dem er sich bei Ihrem Dienst authentifiziert. Diese Funktion wird über die Azure Communication Services-Clientbibliothek „Administration“ bereitgestellt.

```python  
client.revoke_tokens(user)
print("\nSuccessfully revoked all tokens for user with ID: " + user.identifier)
```

## <a name="delete-a-user"></a>Löschen eines Benutzers

Wird eine Identität gelöscht, werden alle aktiven Token widerrufen, und für die Identitäten können keine weiteren Token mehr ausgestellt werden. Außerdem werden alle gespeicherten Inhalte entfernt, die dem Benutzer zugeordnet sind.

```python
client.delete_user(user)
print("\nDeleted the user with ID: " + user.identifier)
```

## <a name="run-the-code"></a>Ausführen des Codes

Navigieren Sie an einer Konsoleneingabeaufforderung zum Verzeichnis mit der Datei *issue-token.py*, und führen Sie anschließend den folgenden Befehl vom Typ `python` aus, um die App auszuführen:

```console
python ./issue-token.py
```
