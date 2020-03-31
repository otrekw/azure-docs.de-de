---
title: Einrichten eines Verschlüsselungszertifikats auf Linux-Clustern
description: Es wird beschrieben, wie Sie ein Verschlüsselungszertifikat einrichten und Geheimnisse in Linux-Clustern verschlüsseln.
author: shsha
ms.topic: conceptual
ms.date: 01/04/2019
ms.author: shsha
ms.openlocfilehash: b8e0a19e3f654fc561e7c7e26c6a2da463e24d5f
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/28/2020
ms.locfileid: "78969030"
---
# <a name="set-up-an-encryption-certificate-and-encrypt-secrets-on-linux-clusters"></a>Einrichten eines Verschlüsselungszertifikats und Verschlüsseln von Geheimnissen in Linux-Clustern
In diesem Artikel wird veranschaulicht, wie Sie ein Verschlüsselungszertifikat einrichten und zum Verschlüsseln von Geheimnissen in Linux-Clustern verwenden. Informationen zur Vorgehensweise für Windows-Cluster finden Sie unter [Einrichten eines Verschlüsselungszertifikats und Verschlüsseln von Geheimnissen in Windows-Clustern][secret-management-windows-specific-link].

## <a name="obtain-a-data-encipherment-certificate"></a>Abrufen eines Datenverschlüsselungszertifikats
Ein Datenverschlüsselungszertifikat wird ausschließlich für die Ver- und Entschlüsselung von [Parametern][parameters-link] in der Datei „Settings.xml“ eines Diensts und von [Umgebungsvariablen][environment-variables-link] in der Datei „ServiceManifest.xml“ eines Diensts verwendet. Es wird nicht für die Authentifizierung oder Signierung von Verschlüsselungstext verwendet. Das Zertifikat muss die folgenden Anforderungen erfüllen:

* Das Zertifikat muss einen privaten Schlüssel enthalten.
* Zu den wichtigsten Verwendungszwecken des Zertifikatschlüssels muss die Datenverschlüsselung (10) zählen. Der Zertifikatschlüssel darf nicht für die Server- oder Clientauthentifizierung verwendet werden.

  Beispielsweise können Sie die folgenden Befehle verwenden, um das erforderliche Zertifikat mit OpenSSL zu generieren:
  
  ```console
  user@linux:~$ openssl req -newkey rsa:2048 -nodes -keyout TestCert.prv -x509 -days 365 -out TestCert.pem
  user@linux:~$ cat TestCert.prv >> TestCert.pem
  ```

## <a name="install-the-certificate-in-your-cluster"></a>Installieren des Zertifikats in Ihrem Cluster
Das Zertifikat muss auf jedem Knoten innerhalb des Clusters unter `/var/lib/sfcerts` installiert werden. Das Benutzerkonto, unter dem der Dienst ausgeführt wird (standardmäßig „sfuser“), sollte über **Lesezugriff** auf das installierte Zertifikat verfügen (`/var/lib/sfcerts/TestCert.pem` im aktuellen Beispiel).

## <a name="encrypt-secrets"></a>Verschlüsseln von Geheimnissen
Der folgende Codeausschnitt kann zum Verschlüsseln eines Geheimnisses verwendet werden. Mit diesem Codeausschnitt wird nur der Wert verschlüsselt. Der Verschlüsselungstext wird **nicht** verschlüsselt. Zum Erstellen des Chiffretexts für geheime Werte **muss** das Verschlüsselungszertifikat verwendet werden, das auf Ihrem Cluster installiert ist.

```console
user@linux:$ echo "Hello World!" > plaintext.txt
user@linux:$ iconv -f ASCII -t UTF-16LE plaintext.txt | tr -d '\n' > plaintext_UTF-16.txt
user@linux:$ openssl smime -encrypt -in plaintext_UTF-16.txt -binary -outform der TestCert.pem | base64 > encrypted.txt
```
Die resultierende Base64-codierte Zeichenfolge, die in „encrypted.txt“ ausgegeben wird, enthält sowohl den geheimen Chiffretext als auch Informationen zum Zertifikat, das für die Verschlüsselung verwendet wurde. Sie können die Gültigkeit überprüfen, indem Sie mit OpenSSL die Entschlüsselung durchführen.
```console
user@linux:$ cat encrypted.txt | base64 -d | openssl smime -decrypt -inform der -inkey TestCert.prv
```

## <a name="next-steps"></a>Nächste Schritte
Informieren Sie sich über das [Angeben von verschlüsselten Geheimnissen in einer Anwendung][secret-management-specify-encrypted-secrets-link].

<!-- Links -->
[parameters-link]:service-fabric-how-to-parameterize-configuration-files.md
[environment-variables-link]: service-fabric-how-to-specify-environment-variables.md
[secret-management-windows-specific-link]: service-fabric-application-secret-management-windows.md
[secret-management-specify-encrypted-secrets-link]: service-fabric-application-secret-management.md#specify-encrypted-secrets-in-an-application
