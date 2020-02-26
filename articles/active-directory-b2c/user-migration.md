---
title: Benutzermigrationsansätze
titleSuffix: Azure AD B2C
description: Hier erfahren Sie, wie Sie Benutzerkonten mittels Massenimport oder nahtloser Migration von einem anderen Identitätsanbieter zu Azure AD B2C migrieren.
services: active-directory-b2c
author: mmacy
manager: celestedg
ms.service: active-directory
ms.workload: identity
ms.topic: conceptual
ms.date: 02/14/2020
ms.author: marsma
ms.subservice: B2C
ms.openlocfilehash: 759379d99bbd422c43998997be1d52ecbf34966d
ms.sourcegitcommit: 98a5a6765da081e7f294d3cb19c1357d10ca333f
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/20/2020
ms.locfileid: "77484286"
---
# <a name="migrate-users-to-azure-ad-b2c"></a>Migrieren von Benutzern zu Azure AD B2C

Bei der Migration von einem anderen Identitätsanbieter zu Azure Active Directory B2C (Azure AD B2C) müssen unter Umständen auch bereits vorhandene Benutzerkonten migriert werden. Hier werden zwei Migrationsmethoden behandelt: *Massenimport* und *nahtlose Migration*. Bei beiden Methoden muss eine Anwendung oder ein Skript geschrieben werden, um Benutzerkonten unter Verwendung der [Microsoft Graph-API](manage-user-accounts-graph-api.md) in Azure AD B2C zu erstellen.

## <a name="bulk-import"></a>Massenimport

Beim Massenimport werden von Ihrer Migrationsanwendung für jedes Benutzerkonto die folgenden Schritte ausgeführt:

1. Lesen des Benutzerkontos und der aktuellen Anmeldeinformationen (Benutzername und Kennwort) des alten Identitätsanbieters
1. Erstellen eines entsprechenden Kontos mit den aktuellen Anmeldeinformationen in Ihrem Azure AD B2C-Verzeichnis

Der Massenimport kann in folgenden Situationen verwendet werden:

- Sie haben Zugriff auf die Klartextanmeldeinformationen (Benutzername und Kennwort) eines Benutzers.
- Die Anmeldeinformationen sind verschlüsselt, können aber von Ihnen entschlüsselt werden.

Informationen zum programmgesteuerten Erstellen von Benutzerkonten finden Sie unter [Verwalten von Azure AD B2C-Benutzerkonten mit Microsoft Graph](manage-user-accounts-graph-api.md).

## <a name="seamless-migration"></a>Nahtlose Migration

Verwenden Sie die nahtlose Migration, wenn im alten Identitätsanbieter nicht auf Klartextkennwörter zugegriffen werden kann. Dies kann beispielsweise in folgenden Situationen der Fall sein:

- Das Kennwort ist in einem unidirektionalen verschlüsselten Format gespeichert (etwa mit einer Hashfunktion).
- Das Kennwort wurde vom alten Identitätsanbieter so gespeichert, dass Sie keinen Zugriff darauf haben. Dies kann beispielsweise der Fall sein, wenn der Identitätsanbieter Anmeldeinformationen per Webdienstaufruf überprüft.

Bei der nahtlose Migration ist zwar ebenfalls eine Massenmigration von Benutzerkonten erforderlich, anschließend wird jedoch eine [benutzerdefinierte Richtlinie](restful-technical-profile.md) verwendet, um eine (von Ihnen erstellte) [REST-API](rest-api-claims-exchange-dotnet.md) abzufragen und das Kennwort der einzelnen Benutzer jeweils bei der ersten Anmeldung festzulegen.

Die nahtlose Migration umfasst somit zwei Phasen: *Massenimport* und *Festlegen der Anmeldeinformationen*.

### <a name="phase-1-bulk-import"></a>Phase 1: Massenimport

1. Ihre Migrationsanwendung liest die Benutzerkonten des alten Identitätsanbieters.
1. Die Migrationsanwendung erstellt entsprechende Benutzerkonten in Ihrem Azure AD B2C-Verzeichnis, *legt aber keine Kennwörter fest*.

### <a name="phase-2-set-credentials"></a>Phase 2: Festlegen von Anmeldeinformationen

Nach Abschluss der Massenmigration der Konten werden durch Ihre benutzerdefinierte Richtlinie und durch Ihre REST-API die folgenden Schritte ausgeführt, wenn sich ein Benutzer anmeldet:

1. Lesen des entsprechenden Azure AD B2C-Benutzerkontos für die eingegebene E-Mail-Adresse
1. Auswerten eines booleschen Erweiterungsattributs, um zu überprüfen, ob das Konto für die Migration gekennzeichnet ist
    - Gibt das Erweiterungsattribut `True` zurück, wird Ihre REST-API aufgerufen, um das Kennwort anhand der Angabe des alten Identitätsanbieters zu überprüfen.
      - Ist das Kennwort nicht korrekt, wird eine benutzerfreundliche Fehlermeldung zurückgegeben.
      - Ist das Kennwort korrekt, wird es in das Azure AD B2C-Konto geschrieben, und das boolesche Erweiterungsattribut wird in `False` geändert.
    - Gibt das boolesche Erweiterungsattribut `False`zurück, wird der Anmeldevorgang normal fortgesetzt.

Ein Beispiel für eine benutzerdefinierte Richtlinie und eine REST-API finden Sie im [Beispiel für die nahtlose Benutzermigration](https://aka.ms/b2c-account-seamless-migration) auf GitHub.

![Flussdiagramm: Nahtlose Benutzermigration](./media/user-migration/diagram-01-seamless-migration.png)<br />*Diagramm: Ablauf der dynamischen Migration*

## <a name="best-practices"></a>Bewährte Methoden

### <a name="security"></a>Sicherheit

Bei der nahtlosen Migration wird Ihre eigene benutzerdefinierte REST-API verwendet, um die Anmeldeinformationen eines Benutzers anhand der Angaben des alten Identitätsanbieters zu überprüfen.

**Die REST-API muss vor Brute-Force-Angriffen geschützt sein.** Ein Angreifer kann mehrere Kennwörter übermitteln und so unter Umständen die Anmeldeinformationen eines Benutzers erraten. Beenden Sie zum Schutz vor derartigen Angriffen die Verarbeitung von Anforderungen für Ihre REST-API, wenn die Anzahl von Anmeldeversuchen einen bestimmten Schwellenwert übersteigt. Schützen Sie außerdem die Kommunikation zwischen Azure AD B2C und Ihrer REST-API mithilfe eines [Clientzertifikats](secure-rest-api-dotnet-certificate-auth.md).

### <a name="user-attributes"></a>Benutzerattribute

Nicht alle Informationen des alten Identitätsanbieter sollten zu Ihrem Azure AD B2C-Verzeichnis migriert werden. Ermitteln Sie vor der Migration, welche Benutzerattribute in Azure AD B2C gespeichert werden sollen.

- Folgendes **sollte** in Azure AD B2C gespeichert werden:
  - Benutzername, Kennwort, E-Mail-Adressen, Telefonnummern, Mitgliedschaftsnummern/-IDs
  - Einwilligungsmarker für Datenschutzrichtlinie und Lizenzbedingungen
- Folgendes sollte **nicht** in Azure AD B2C gespeichert werden:
  - Vertrauliche Daten wie Kreditkartennummern, Sozialversicherungsnummern, Krankenakten oder andere Daten, die behördlichen oder branchenspezifischen Complianceregeln unterliegen
  - Marketing- oder Kommunikationspräferenzen, Benutzerverhalten und Erkenntnisse

### <a name="directory-clean-up"></a>Verzeichnisbereinigung

Es empfiehlt sich, vor Beginn der Migration Ihr Verzeichnis zu bereinigen.

- Identifizieren Sie die Benutzerattribute, die in Azure AD B2C gespeichert werden sollen, und migrieren Sie nur das, was Sie benötigen. Bei Bedarf können Sie [benutzerdefinierte Attribute](custom-policy-custom-attributes.md) erstellen, um weitere Benutzerdaten zu speichern.
- Wenn Sie aus einer Umgebung mit mehreren Authentifizierungsquellen migrieren, in der beispielsweise jede Anwendung über ein eigenes Benutzerverzeichnis verfügt, verwenden Sie als Migrationsziel ein einheitliches Konto in Azure AD B2C.
- Wenn mehrere Anwendungen über unterschiedliche Benutzernamen verfügen, können diese mithilfe der Identitätensammlung in einem Azure AD B2C-Benutzerkonto gespeichert werden. Lassen Sie den Benutzer ein Kennwort wählen, und legen Sie dieses im Verzeichnis fest. Bei der nahtlosen Migration sollte beispielsweise nur das gewählte Kennwort im Azure AD B2C-Konto gespeichert werden.
- Entfernen Sie nicht verwendete Benutzerkonten vor der Migration, oder migrieren Sie keine veralteten Konten.

### <a name="password-policy"></a>Kennwortrichtlinie

Wenn die Konten, die Sie migrieren möchten, nicht über die [hohe Kennwortsicherheit](../active-directory/authentication/concept-sspr-policy.md) verfügen, die von Azure AD B2C erzwungen wird, können Sie die Erzwingung sicherer Kennwörter deaktivieren. Weitere Informationen finden Sie unter [Eigenschaft „passwordPolicies“ (Kennwortrichtlinien)](manage-user-accounts-graph-api.md#password-policy-property).

## <a name="next-steps"></a>Nächste Schritte

Das Repository [azure-ad-b2c/user-migration](https://github.com/azure-ad-b2c/user-migration) auf GitHub enthält eine benutzerdefinierte Beispielrichtlinie für die nahtlose Migration sowie ein REST-API-Codebeispiel:

[Benutzerdefinierte Richtlinie für die nahtlose Benutzermigration und REST-API-Codebeispiel](https://aka.ms/b2c-account-seamless-migration)
