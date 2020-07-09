---
title: 'Microsoft Authenticator: Verfügbarkeit und Einschränkungen für Android in China | Microsoft-Dokumentation'
description: Hier erfahren Sie, wie Sie die Microsoft Authenticator-App erhalten (Informationen zur Verfügbarkeit in China).
services: active-directory
author: curtand
manager: daveba
ms.reviewer: sahenry
ms.service: active-directory
ms.subservice: user-help
ms.workload: identity
ms.topic: end-user-help
ms.date: 05/20/2020
ms.author: curtand
ms.openlocfilehash: 795c68fc063d98bdee6ccf59dba6ee718dc92d03
ms.sourcegitcommit: 58ff2addf1ffa32d529ee9661bbef8fbae3cddec
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/03/2020
ms.locfileid: "84323024"
---
# <a name="microsoft-authenticator-for-android-in-the-public-cloud-in-china"></a>Microsoft Authenticator für Android in der öffentlichen Cloud in China

Die Microsoft Authenticator-App für Android steht in China zum Download zur Verfügung. Da der Google Play Store in China nicht verfügbar ist, muss die App von anderen chinesischen App-Marketplaces heruntergeladen werden. Die Microsoft Authenticator-App für Android ist in China momentan in folgenden Stores verfügbar:

- [Baidu](https://shouji.baidu.com/software/26638379.html)
- [Lenovo](https://www.lenovomm.com/appdetail/com.azure.authenticator/20197724)
- [Huawei](https://appgallery.cloud.huawei.com/uowap/index.html#/detailApp/C100262999?source=appshare&subsource=C100262999&shareTo=weixin&locale=zh_CN)
- [Samsung Galaxy Store](http://apps.samsung.com/appquery/appDetail.as?appId=com.azure.authenticator)

Der neueste Build der App befindet sich zwar im Google Play Store, die App wird von uns jedoch so schnell wie möglich in allen anderen App-Stores aktualisiert. Da in keinem App-Store ein benutzerdefiniertes Android-Anwendungspaket (APK) bereitgestellt wird, kann die App nahtlos über einen der folgenden Orte aktualisiert werden:

- Store, aus dem sie heruntergeladen wurde
- Google Play Store, falls sich der Benutzer in eine andere Region wechselt

## <a name="limitations"></a>Einschränkungen

Die Microsoft Authenticator-App für Android verwendet das Firebase Cloud Messaging-System von Google sowie Google Play Services für den Empfang von Pushbenachrichtigungen. Da diese Dienste in China nicht verfügbar sind, gibt es bei der App gewisse Funktionseinschränkungen:

- Die Registrierung der Authenticator-App als MFA-Methode (Multi-Factor Authentication, mehrstufige Authentifizierung) mit Pushbenachrichtigungen funktioniert nicht.

- Die [Anmeldung per Telefon](../authentication/howto-authentication-sms-signin.md) kann nicht eingerichtet werden. Hierzu muss der Benutzer die App als MFA-Methode mit Pushbenachrichtigungen einrichten, was derzeit nicht möglich ist.

Wenn ein Benutzer zu einem früheren Zeitpunkt die Anmeldung per Telefon oder die mehrstufige Authentifizierung mithilfe der App einrichten konnte, kann er in der App eine manuelle Überprüfung auf Benachrichtigungsanforderungen durchführen und sie für die Identitätsüberprüfung verwenden.

## <a name="multi-factor-authentication-workaround"></a>Problemumgehung für die mehrstufige Authentifizierung

Anstelle von Pushbenachrichtigungen für die mehrstufige Authentifizierung können Benutzer [ihre Authenticator-App für den Empfang von Prüfcodes](multi-factor-authentication-setup-auth-app.md#set-up-the-microsoft-authenticator-app-to-use-verification-codes) auf ihrem Gerät einrichten, mit denen sie zur MFA ihre Identität bestätigen können. Diese Prüfcodes sind 30 Sekunden lang gültig. Damit sie verwendet werden können, müssen Administratoren ihrem Mandanten die Überprüfung mittels TOTP-Prüfcodes (Time-based One-Time Password, zeitbasiertes Einmalkennwort) ermöglichen.

## <a name="availability"></a>Verfügbarkeit

Microsoft Authenticator-Feature | Verfügbarkeit in China
------------------------------- | ---------------------
MFA-Registrierung mithilfe von Pushbenachrichtigungen | Nein
Überprüfung der Identität mithilfe von Pushbenachrichtigungen mit bereits vorhandenem MFA-Konto | Nein
Manuelle Überprüfung auf Benachrichtigungen mit bereits vorhandenem MFA-Konto | Ja
Nur MFA-Registrierung/Authentifizierung mit TOTP/Prüfcodes | Ja
Registrierung für die Anmeldung per Telefon | Nein
Vorhandene Anmeldung per Telefon mithilfe von Pushbenachrichtigungen | Nein
Überprüfung bei vorhandener Anmeldung per Telefon durch manuelle Überprüfung auf Authentifizierungsanforderungen | Ja

## <a name="next-steps"></a>Nächste Schritte

- [Herunterladen und Installieren der Microsoft Authenticator-App](user-help-auth-app-download-install.md)
