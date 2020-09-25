---
title: Konfigurieren der Sicherheit für Ihre Azure Arc-fähige PostgreSQL Hyperscale-Servergruppe
description: Konfigurieren der Sicherheit für Ihre Azure Arc-fähige PostgreSQL Hyperscale-Servergruppe
services: azure-arc
ms.service: azure-arc
ms.subservice: azure-arc-data
author: TheJY
ms.author: jeanyd
ms.reviewer: mikeray
ms.date: 09/22/2020
ms.topic: how-to
ms.openlocfilehash: b166348031e9f72e8005e866a198855db9c01a9c
ms.sourcegitcommit: 53acd9895a4a395efa6d7cd41d7f78e392b9cfbe
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/22/2020
ms.locfileid: "90931336"
---
# <a name="configure-security-for-your-azure-arc-enabled-postgresql-hyperscale-server-group"></a>Konfigurieren der Sicherheit für Ihre Azure Arc-fähige PostgreSQL Hyperscale-Servergruppe

In dieser Dokumentation werden verschiedene Aspekte der Sicherheit Ihrer Servergruppe beschrieben:
- Verschlüsselung ruhender Daten
- Benutzerverwaltung
   - Allgemeine Perspektiven
   - Ändern des Kennworts des _postgres_-Administrators

[!INCLUDE [azure-arc-data-preview](../../../includes/azure-arc-data-preview.md)]

## <a name="encryption-at-rest"></a>Verschlüsselung ruhender Daten
Sie können die Verschlüsselung ruhender Daten implementieren, indem Sie entweder die Datenträger verschlüsseln, auf denen Sie Ihre Datenbanken speichern, und/oder indem Sie Datenbankfunktionen zum Verschlüsseln der Daten verwenden, die Sie einfügen oder aktualisieren.

### <a name="hardware-linux-host-volume-encryption"></a>Hardware: Verschlüsselung des Linux-Hostvolumes
Implementieren Sie die Systemdatenverschlüsselung, um jegliche Daten zu schützen, die sich auf den Datenträgern befinden, die von Ihren Azure Arc-fähigen Datendienste verwendet werden. Weitere Informationen hierzu finden Sie hier:
- [Verschlüsselung ruhender Daten](https://wiki.archlinux.org/index.php/Data-at-rest_encryption) unter Linux im Allgemeinen 
- Datenträgerverschlüsselung mit dem LUKS-Verschlüsselungsbefehl `cryptsetup` (Linux) (https://www.cyberciti.biz/security/howto-linux-hard-disk-encryption-with-luks-cryptsetup-command/) – da Azure Arc-fähige Datendienste auf der physischen Infrastruktur ausgeführt werden, die Sie bereitstellen, sind Sie für die Sicherheit der Infrastruktur verantwortlich.

### <a name="software-use-the-postgresql-pgcrypto-extension-in-your-server-group"></a>Software: Verwenden der PostgreSQL-Erweiterung `pgcrypto` in Ihrer Servergruppe
Zusätzlich zur Verschlüsselung der Datenträger, die zum Hosten Ihres Azure Arc-Setups verwendet werden, können Sie Ihre Azure Arc-fähige PostgreSQL Hyperscale-Servergruppe dazu konfigurieren, Mechanismen zur Verfügung zu stellen, die Ihre Anwendungen zum Verschlüsseln von Daten in Ihren Datenbanken verwenden können. Die `pgcrypto`-Erweiterung ist Teil der `contrib`-Erweiterungen von Postgres und ist in Ihrer Azure Arc-fähigen PostgreSQL Hyperscale-Servergruppe verfügbar. Weitere Informationen zur `pgcrypto`-Erweiterung finden Sie [hier](https://www.postgresql.org/docs/current/pgcrypto.html).
Zusammenfassend können Sie die folgenden Befehle zum Aktivieren, Erstellen und Verwenden der Erweiterung verwenden:


#### <a name="create-the-pgcrypto-extension"></a>Erstellen der `pgcrypto`-Erweiterung
Stellen Sie mit dem Clienttool Ihrer Wahl eine Verbindung mit Ihrer Servergruppe her, und führen Sie die PostgreSQL-Standardabfrage aus:
```console
CREATE EXTENSION pgcrypto;
```

> Ausführliche Informationen zum Herstellen einer Verbindung finden Sie [hier](get-connection-endpoints-and-connection-strings-postgres-hyperscale.md).

#### <a name="verify-the-list-the-extensions-ready-to-use-in-your-server-group"></a>Überprüfen den Liste der einsatzbereiten Erweiterungen in Ihrer Servergruppe
Sie können überprüfen, ob die `pgcrypto`-Erweiterung einsatzbereit ist, indem Sie die verfügbaren Erweiterungen in Ihrer Servergruppe auflisten.
Stellen Sie mit dem Clienttool Ihrer Wahl eine Verbindung mit Ihrer Servergruppe her, und führen Sie die PostgreSQL-Standardabfrage aus:
```console
select * from pg_extension;
```
`pgcrypto` sollte angezeigt werden, wenn Sie die Erweiterung mit den oben angegebenen Befehlen aktiviert und erstellt haben.

#### <a name="use-the-pgcrypto-extension"></a>Verwenden der `pgcrypto`-Erweiterung
Sie können nun den Code Ihrer Anwendungen so anpassen, dass sie von `pgcrypto` bereitgestellte Funktionen verwenden:
- Allgemeine Hashfunktionen
- Kennworthashfunktionen
- PGP-Verschlüsselungsfunktionen
- Rohverschlüsselungsfunktionen
- Funktionen für zufällige Daten

(Beispielsweise zum Generieren von Hashwerten) Führen Sie den folgenden Befehl aus:

```console
Select crypt('Les sanglots longs des violons de l_automne', gen_salt('md5'));
```

Daraufhin wird der folgende Hash zurückgegeben:

```console
              crypt
------------------------------------
 $1$/9ACBYOV$z52PAGjQ5WTU9xvEECBNv/   
```

Alternativ gibt folgendes Beispiel:

```console
select hmac('Les sanglots longs des violons de l_automne', 'md5', 'sha256');
```

den folgenden Hash zurück:

```console
                                hmac
--------------------------------------------------------------------
 \xd4e4790b69d2cc8dbce3385ee63272bc7760f1603640bb211a7b864e695570c5
```

Alternativ können Sie verschlüsselte Daten beispielsweise wie ein Kennwort speichern:

Angenommen, meine Anwendungen speichert Geheimnisse in der folgenden Tabelle:

```console
create table mysecrets(USERid int, USERname char(255), USERpassword char(512));
```

und ich verschlüssele das Kennwort beim Erstellen eines Benutzers:

```console
insert into mysecrets values (1, 'Me', crypt('MySecretPasswrod', gen_salt('md5')));
```

Jetzt sehe ich, dass mein Kennwort verschlüsselt ist:

```console
select * from mysecrets;
```

Ausgabe:

- USERid: 1
- USERname: Bei mir selbst
- USERpassword: $1$Uc7jzZOp$NTfcGo7F10zGOkXOwjHy31

Wenn ich eine Verbindung mit meiner Anwendung herstelle und ein Kennwort übergebe, wird in der Tabelle `mysecrets` gesucht und der Name des Benutzers zurückgegeben, sofern eine Übereinstimmung zwischen dem für die Anwendung angegebenen Kennwort und den in der Tabelle gespeicherten Kennwörtern vorliegt. Zum Beispiel:

- Ich übergebe das falsche Kennwort:
   ```console
   select USERname from mysecrets where (USERpassword = crypt('WrongPassword', USERpassword));
   ```

   Ausgabe 

   ```returns
    USERname
   ---------
   (0 rows)
   ```
- Ich übergebe das richtige Kennwort:

   ```console
   select USERname from mysecrets where (USERpassword = crypt('MySecretPasswrod', USERpassword));
   ``` 

   Ausgabe:

   ```output
    USERname
   ---------
   Me
   (1 row)
   ```

In diesem kleinen Beispiel wird veranschaulicht, dass Sie ruhende Daten (Speichern verschlüsselter Daten) in Azure Arc-fähigen PostgreSQL Hyperscale-Servergruppen mithilfe der Postgres-Erweiterung `pgcrypto` verschlüsseln und Ihre Anwendungen die von `pgcrypto` bereitgestellten Funktionen zum Bearbeiten dieser verschlüsselten Daten verwenden können.

## <a name="user-management"></a>Benutzerverwaltung
### <a name="general-perspectives"></a>Allgemeine Perspektiven
Sie können die Postgres-Standardvorgehensweise zum Erstellen von Benutzern und Rollen verwenden. Wenn Sie dies jedoch tun, stehen diese Artefakte nur der Coordinator-Rolle zur Verfügung. Während der Vorschau können diese Benutzer/Rollen noch nicht auf Daten zugreifen, die außerhalb des Koordinatorknotens und auf den Workerknoten Ihrer Servergruppe verteilt sind. Der Grund hierfür ist, dass die Benutzerdefinition während der Vorschau nicht auf die Workerknoten repliziert wird.

### <a name="change-the-password-of-the-_postgres_-administrative-user"></a>Ändern des Kennworts des _postgres_-Administrators
Azure Arc-fähiges PostgreSQL Hyperscale umfasst den Postgres-Standardadministrator _postgres_, für den Sie das Kennwort festlegen, wenn Sie Ihre Servergruppe erstellen.
Das allgemeine Format für den Befehl zum Ändern des Kennworts lautet wie folgt:
```console
azdata arc postgres server edit --name <server group name> --admin-password <new password>
```
Das Kennwort wird als Wert der Umgebungsvariable der AZDATA_PASSWORD-**Sitzung** festgelegt, sofern diese vorhanden ist. Wenn sie nicht vorhanden ist, wird der Benutzer zur Eingabe eines Werts aufgefordert.
Führen Sie den folgenden Befehl aus, um zu überprüfen, ob die Umgebungsvariable der AZDATA_PASSWORD-Sitzung vorhanden ist und/oder welcher Wert festgelegt ist:
```console
printenv AZDATA_PASSWORD
```
Sie sollten den Wert löschen, wenn Sie es bevorzugen, zur Eingabe eines neuen Kennworts aufgefordert zu werden.


## <a name="next-steps"></a>Nächste Schritte
- Ausführliche Informationen zur `pgcrypto`-Erweiterung finden Sie [hier](https://www.postgresql.org/docs/current/pgcrypto.html).
- Ausführliche Informationen zur Verwendung von Postgres-Erweiterungen finden Sie [hier](using-extensions-in-postgresql-hyperscale-server-group.md).

