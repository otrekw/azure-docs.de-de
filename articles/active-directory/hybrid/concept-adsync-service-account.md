---
title: 'Azure AD Connect: ADSync-Dienstkonto | Microsoft-Dokumentation'
description: In diesem Thema werden das ADSync-Dienstkonto beschrieben und bewährte Methoden für das Konto vorgestellt.
services: active-directory
documentationcenter: ''
author: billmath
manager: daveba
editor: ''
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 03/17/2021
ms.subservice: hybrid
ms.author: billmath
ms.collection: M365-identity-device-management
ms.openlocfilehash: dfa329999cb7b53835907196ceaa9b02920da149
ms.sourcegitcommit: 4a54c268400b4158b78bb1d37235b79409cb5816
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/28/2021
ms.locfileid: "108124255"
---
# <a name="adsync-service-account"></a>ADSync-Dienstkonto
Azure AD Connect installiert einen lokalen Dienst, der die Synchronisierung zwischen Active Directory und Azure Active Directory orchestriert.  Der Synchronisierungsdienst Microsoft Azure AD Sync (ADSync) wird auf einem Server in Ihrer lokalen Umgebung ausgeführt.  Die Anmeldeinformationen für den Dienst sind in den Express-Installationen standardmäßig festgelegt, können aber an die Sicherheitsanforderungen Ihrer Organisation angepasst werden.  Diese Anmeldeinformationen werden nicht verwendet, um eine Verbindung mit Ihren lokalen Gesamtstrukturen oder Azure Active Directory herzustellen.

Die Wahl des ADSync-Dienstkontos ist eine wichtige Planungsentscheidung, die vor der Installation von Azure AD Connect getroffen werden muss.  Jeder Versuch, die Anmeldeinformationen nach der Installation zu ändern, führt dazu, dass der Dienst nicht gestartet werden kann, den Zugriff auf die Synchronisierungsdatenbank verliert und sich nicht bei Ihren verbundenen Verzeichnissen (Azure und AD DS) authentifiziert.  Es erfolgt keine Synchronisierung, bis die ursprünglichen Anmeldeinformationen wiederhergestellt sind.

Der Synchronisierungsdienst kann unter verschiedenen Konten ausgeführt werden. Er kann unter einem virtuellen Dienstkonto (VSA), einem verwalteten Dienstkonto (gMSA/sMSA) oder einem normalen Benutzerkonto ausgeführt werden. Die unterstützten Optionen wurden mit der Azure AD Connect-Version von April 2017 und März 2021 geändert und treten in Kraft, wenn Sie eine Neuinstallation durchführen. Wenn Sie ein Upgrade von einer früheren Version von Azure AD Connect durchführen, sind diese zusätzlichen Optionen nicht verfügbar. 


|Kontotyp|Installationsoption|BESCHREIBUNG| 
|-----|------|-----|
|virtuellen Dienstkonto|Express und benutzerdefiniert, April 2017 und höher| Ein virtuelles Dienstkonto wird für alle Expressinstallationen mit Ausnahme von Installationen auf einem Domänencontroller verwendet. Für benutzerdefinierte Installationen ist es die Standardoption, sofern keine andere Option verwendet wird.| 
|Verwaltetes Dienstkonto|Benutzerdefiniert, April 2017 und höher|Wenn Sie einen Remoteserver mit SQL Server verwenden, empfehlen wir den Einsatz eines gruppenverwalteten Dienstkontos. |
|Verwaltetes Dienstkonto|Express und benutzerdefiniert, März 2021 und höher|Ein eigenständig verwaltetes Dienstkonto mit dem Präfix „ADSyncMSA_“ wird während einer Expressinstallation auf einem Domänencontroller erstellt. Für benutzerdefinierte Installationen ist es die Standardoption, sofern keine andere Option verwendet wird.|
|Benutzerkonto|Express und benutzerdefiniert, April 2017 bis März 2021|Ein Benutzerkonto mit dem Präfix „AAD_“ wird während einer Expressinstallation auf einem Domänencontroller erstellt. Für benutzerdefinierte Installationen ist es die Standardoption, sofern keine andere Option verwendet wird.|
|Benutzerkonto|Express und benutzerdefiniert, März 2017 und früher|Ein Benutzerkonto mit dem Präfix „AAD_“ wird während einer Expressinstallation erstellt. Bei der benutzerdefinierten Installation kann ein anderes Konto angegeben werden.| 

>[!IMPORTANT]
> Wenn Sie Connect mit einem Build von März 2017 und früher verwenden, setzen Sie das Kennwort für das Dienstkonto nicht zurück, da Windows die Verschlüsselungsschlüssel aus Sicherheitsgründen zerstört. Sie können das Konto nicht in ein anderes Konto ändern, ohne Azure AD Connect neu zu installieren. Wenn Sie ein Upgrade auf einen Build von April 2017 oder höher durchführen, wird das Ändern des Kennworts für das Dienstkonto unterstützt. Sie können jedoch nicht das verwendete Konto ändern. 

> [!IMPORTANT]
> Sie können das Dienstkonto nur bei der erstmaligen Installation festlegen. Das Ändern des Dienstkontos nach Abschluss der Installation wird nicht unterstützt. Das Ändern des Kennworts für das Dienstkonto wird unterstützt. Anweisungen finden Sie [hier](how-to-connect-sync-change-serviceacct-pass.md).

Im Folgenden sehen Sie eine Tabelle mit den Standard-, den empfohlenen und den unterstützten Optionen für das Synchronisierungsdienstkonto. 

Legende: 

- **Fettformatierung** kennzeichnet die Standardoption, die in den meisten Fällen der empfohlenen Option entspricht. 
- *Kursivformatierung* kennzeichnet die empfohlene Option, sofern diese nicht mit der Standardoption übereinstimmt. 
- Nicht fett formatiert: unterstützte Option 
- Lokales Konto: lokales Benutzerkonto auf dem Server 
- Domänenkonto: Domänenbenutzerkonto 
- sMSA: [eigenständig verwaltetes Dienstkonto](/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/dd548356(v=ws.10))
- gMSA: [gruppenverwaltetes Dienstkonto](/previous-versions/windows/it-pro/windows-server-2012-R2-and-2012/hh831782(v=ws.11)) 

|Computertyp |**LocalDB</br> Express**|**LocalDB/LocalSQL</br> Benutzerdefiniert**|**SQL-Remotecomputer </br> Benutzerdefiniert**|
|-----|-----|-----|-----|
|**In die Domäne eingebundener Computer**|**VSA**|**VSA**</br> *sMSA*</br> *gMSA*</br> Lokales Konto</br> Domänenkonto| *gMSA* </br>Domänenkonto|
|Domänencontroller| **sMSA**|**sMSA** </br>*gMSA*</br> Domänenkonto|*gMSA*</br>Domänenkonto| 

## <a name="virtual-service-account"></a>virtuellen Dienstkonto 

Ein virtuelles Dienstkonto ist ein besonderer Typ eines verwalteten lokalen Kontos, das nicht über ein Kennwort verfügt und von Windows automatisch verwaltet wird. 

 ![Virtuelles Dienstkonto](media/concept-adsync-service-account/account-1.png)

Das virtuelle Dienstkonto ist für den Einsatz in Szenarien vorgesehen, in denen das Synchronisierungsmodul und SQL sich auf demselben Server befinden. Wenn Sie einen SQL-Remotecomputer verwenden, empfehlen wir stattdessen den Einsatz eines gruppenverwalteten Dienstkontos. 

Das virtuelle Dienstkonto kann aufgrund von Problemen mit der [Windows-Datenschutz-API (DPAPI)](/previous-versions/ms995355(v=msdn.10)) nicht auf einem Domänencontroller verwendet werden. 

## <a name="managed-service-account"></a>Verwaltetes Dienstkonto 

Wenn Sie einen Remoteserver mit SQL Server verwenden, empfehlen wir den Einsatz eines gruppenverwalteten Dienstkontos. Weitere Informationen zum Vorbereiten von Active Directory für das gruppenverwaltete Dienstkonto finden Sie unter [Gruppenverwaltete Dienstkonten: Übersicht](/previous-versions/windows/it-pro/windows-server-2012-R2-and-2012/hh831782(v=ws.11)). 

Um diese Option zu verwenden, wählen Sie auf der Seite [Erforderliche Komponenten installieren](how-to-connect-install-custom.md#install-required-components) die Optionen **Vorhandenes Dienstkonto verwenden** und **Verwaltetes Dienstkonto**. 

 ![Verwaltetes Dienstkonto](media/concept-adsync-service-account/account-2.png)

Die Verwendung eines eigenständig verwalteten Dienstkontos wird ebenfalls unterstützt. Diese Konten können jedoch nur auf dem lokalen Computer verwendet werden, und es gibt keinen praktischen Vorteil gegenüber dem virtuellen Standarddienstkonto. 

### <a name="auto-generated-standalone-managed-service-account"></a>Automatisch generiertes eigenständig verwaltetes Dienstkonto 

Wenn Sie Azure AD Connect auf einem Domänencontroller installieren, wird vom Installations-Assistenten ein eigenständig verwaltetes Dienstkonto erstellt (es sei denn, Sie geben in den benutzerdefinierten Einstellungen das zu verwendende Konto an). Das Konto ist mit dem Präfix **ADSyncMSA_** versehen und wird zur Ausführung des eigentlichen Synchronisierungsdiensts verwendet. 

Bei diesem Konto handelt es sich um ein verwaltetes Domänenkonto, das nicht über ein Kennwort verfügt und von Windows automatisch verwaltet wird. 

Dieses Konto ist für den Einsatz in Szenarien vorgesehen, in denen das Synchronisierungsmodul und SQL sich auf dem Domänencontroller befinden. 

## <a name="user-account"></a>Benutzerkonto 

Der Installations-Assistent erstellt ein lokales Dienstkonto (sofern das Konto nicht zur Verwendung in benutzerdefinierten Einstellungen angegeben wird). Das Konto ist mit dem Präfix AAD_ versehen und wird zur Ausführung des eigentlichen Synchronisierungsdiensts verwendet. Wenn Sie Azure AD Connect auf einem Domänencontroller installieren, wird das Konto in der Domäne erstellt. Das Dienstkonto AAD_ muss sich in folgenden Fällen in der Domäne befinden: 
- Sie verwenden einen Remoteserver, auf dem SQL Server ausgeführt wird. 
- Sie verwenden einen Proxy, der Authentifizierung erfordert. 

 ![Benutzerkonto](media/concept-adsync-service-account/account-3.png)

Das Konto wird mit einem langen, komplexen Kennwort erstellt, das nicht abläuft. 

Dieses Konto wird verwendet, um die Kennwörter für die anderen Konten auf sichere Weise zu speichern. Die Kennwörter dieser Konten sind verschlüsselt in der Datenbank gespeichert. Die privaten Schlüssel für die Verschlüsselungsschlüssel sind mit der Verschlüsselung mit geheimem Schlüssel für kryptografische Dienste mithilfe der Windows-Datenschutz-API (DPAPI) geschützt. 

Bei Verwendung einer SQL Server-Instanz mit vollem Funktionsumfang wird das Dienstkonto zum DBO der Datenbank, die für das Synchronisierungsmodul erstellt wurde. Der Dienst funktioniert mit anderen Berechtigungen nicht wie vorgesehen. Darüber hinaus wird eine SQL-Anmeldung erstellt. 

Das Konto erhält auch eine Berechtigung für Dateien, Registrierungsschlüssel und andere Objekte im Zusammenhang mit dem Synchronisierungsmodul. 


## <a name="next-steps"></a>Nächste Schritte
Weitere Informationen zum [Integrieren lokaler Identitäten in Azure Active Directory](whatis-hybrid-identity.md).