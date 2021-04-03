---
title: 'Azure AD: Sichern und Wiederherstellen von Konten mit der Microsoft Authenticator-App'
description: Hier erfahren Sie, wie Sie Ihre gesicherten Kontoanmeldeinformationen mithilfe der Microsoft Authenticator-App sichern und wiederherstellen.
services: active-directory
author: curtand
manager: daveba
ms.subservice: user-help
ms.service: active-directory
ms.workload: identity
ms.topic: end-user-help
ms.date: 06/03/2020
ms.author: curtand
ms.reviewer: olhaun
ms.openlocfilehash: e7608196c962a6918a90b91457f856f3e11b04f0
ms.sourcegitcommit: f28ebb95ae9aaaff3f87d8388a09b41e0b3445b5
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/29/2021
ms.locfileid: "91530900"
---
# <a name="back-up-and-recover-account-credentials-using-the-microsoft-authenticator-app"></a>Sichern und Wiederherstellen von Kontoanmeldeinformationen mithilfe der Microsoft Authenticator-App

**Anwendungsbereich:**

- iOS-Geräte, auf denen Version 5.7.0 und höher ausgeführt wird

- Android-Geräte, auf denen Version 6.6.0 und höher ausgeführt wird

Die Microsoft Authenticator-App sichert Ihre Kontoanmeldeinformationen und zugehörige App-Einstellungen, etwa die Reihenfolge Ihrer Konten, in der Cloud. Nach dem Sichern können Sie die App auch zum Wiederherstellen der Informationen auf einem neuen Gerät verwenden und dadurch möglicherweise vermeiden, dass Sie gesperrt werden oder Konten neu erstellen müssen.

Jeder Sicherungsspeicherort erfordert, dass Sie über ein persönliches Microsoft-Konto verfügen, und für iOS ist auch ein iCloud-Konto erforderlich. An diesem einen Speicherort können mehrere Konten gespeichert werden. Sie können beispielsweise ein persönliches Konto, ein Geschäfts-, Schul- oder Unikonto und ein persönliches Nicht-Microsoft-Konto (z. B. für Facebook, Google usw.) verwenden.

> [!IMPORTANT]
> Es werden nur Ihre persönlichen und Ihre Drittanbieter-Kontoanmeldeinformationen gespeichert. Dazu gehören Ihr Benutzernamen und der Kontoprüfcode, der zum Nachweisen Ihrer Identität erforderlich ist. Es werden keine weiteren Informationen zu Ihrem Konto wie E-Mails oder Dateien gespeichert. Darüber hinaus werden Ihre Konten keinesfalls mit einem anderen Produkt oder Dienst verknüpft oder geteilt. Ihr IT-Administrator erhält ebenfalls keine Informationen zu diesen Konten.

## <a name="back-up-your-account-credentials"></a>Sichern Ihrer Anmeldeinformationen

Damit Sie Ihre Anmeldeinformationen sichern können, benötigen Sie Folgendes:

- Ein persönliches [Microsoft-Konto](https://account.microsoft.com/account), das als Wiederherstellungskonto fungiert

- **Nur für iOS** müssen Sie über ein [iCloud-Konto](https://www.icloud.com/) für den tatsächlichen Speicherort verfügen.

### <a name="to-turn-on-cloud-backup-for-ios-devices"></a>So aktivieren Sie die Cloudsicherung für iOS-Geräte

- Wählen Sie auf Ihrem iOS-Gerät **Settings** (Einstellungen) und **Backup** (Sicherung), und aktivieren Sie dann **iCloud backup** (iCloud-Sicherung).

    Die Anmeldeinformationen für Ihr Konto werden in Ihrem iCloud-Konto gesichert.

    ![Bildschirm mit den iOS-Einstellungen und den Einstellungen für die iCloud-Sicherung](./media/user-help-auth-app-backup-recovery/backup-and-recovery-turn-on.png)

### <a name="to-turn-on-cloud-backup-for-android-devices"></a>So aktivieren Sie die Cloudsicherung für Android-Geräte

- Wählen Sie auf Ihrem Android-Gerät die Option **Einstellungen** aus, wählen Sie **Sicherung** aus, und aktivieren Sie dann **Cloudsicherung**.

    Ihre Kontoanmeldeinformationen werden in Ihrem Cloudkonto gesichert.

    ![Bildschirm der Android-Einstellungen mit dem Speicherort der Sicherungseinstellungen](./media/user-help-auth-app-backup-recovery/backup-and-recovery-turn-on-android.png)

## <a name="recover-your-account-credentials-on-your-new-device"></a>Wiederherstellen Ihrer Kontoanmeldeinformationen auf einem neuen Gerät

Sie können Ihre Kontoanmeldeinformationen aus Ihrem Cloudkonto wiederherstellen, aber Sie müssen zuerst sicherstellen, dass das Konto, das Sie wiederherstellen, nicht in der Microsoft Authenticator-App vorhanden ist. Wenn Sie z. B. Ihr persönliches Microsoft-Konto wiederherstellen, müssen Sie sicherstellen, dass in der Authenticator-App nicht bereits ein persönliches Microsoft-Konto eingerichtet ist. Diese Überprüfung ist wichtig, damit wir sicherstellen können, dass ein vorhandenes Konto nicht versehentlich überschrieben oder gelöscht wird.

### <a name="to-recover-your-information"></a>So stellen Sie Ihre Informationen wieder her

1. Öffnen Sie auf Ihrem mobilen Gerät die Microsoft Authenticator-App, und wählen Sie unten auf dem Bildschirm die Option **Wiederherstellung starten** aus.

    ![Microsoft Authenticator-App mit hervorgehobener Option „Wiederherstellung starten“](./media/user-help-auth-app-backup-recovery/backup-and-recovery-begin-recovery.png)

2. Melden Sie sich bei Ihrem Wiederherstellungskonto mit dem gleichen persönlichen Microsoft-Konto an, das Sie während des Sicherungsprozesses verwendet haben.

    Ihre Kontoanmeldeinformationen werden auf dem neuen Gerät wiederhergestellt.

Nach der Wiederherstellung stellen Sie unter Umständen fest, dass die Prüfcodes des persönlichen Microsoft-Kontos in der Microsoft Authenticator-App auf dem alten und dem neuen Smartphone voneinander abweichen. Die Codes unterscheiden sich, da jedes Gerät über eigene eindeutige Anmeldeinformationen verfügt. Beide sind jedoch gültig und können bei der Anmeldung mit dem zugehörigen Smartphone verwendet werden.

## <a name="recover-accounts-requiring-more-verification"></a>Wiederherstellen von Konten, für die ein zusätzlicher Überprüfungsschritt erforderlich ist

Wenn Sie Pushbenachrichtigungen für persönliche Konten oder Geschäfts-, Schul- oder Unikonten verwenden, wird eine Bildschirmmeldung mit dem Hinweis angezeigt, dass zum Wiederherstellen der Informationen eine zusätzliche Überprüfung erforderlich ist. Da für Pushbenachrichtigungen die Verwendung von Anmeldeinformationen erforderlich ist, die mit einem bestimmten Gerät verknüpft sind und nie über das Netzwerk gesendet werden, müssen Sie Ihre Identität bestätigen, bevor die Anmeldeinformationen auf dem Gerät erstellt werden.

Bei persönlichen Microsoft-Konten können Sie Ihre Identität bestätigen, indem Sie Ihr Kennwort zusammen mit einer alternativen E-Mail-Adresse oder Telefonnummer eingeben. Bei Geschäfts-, Schul- oder Unikonten müssen Sie einen QR-Code scannen, den Sie vom Kontoanbieter erhalten.

### <a name="to-provide-more-verification-for-personal-accounts"></a>So führen Sie einen zusätzlichen Überprüfungsschritt für persönliche Konten aus

1. Tippen Sie in der Microsoft Authenticator-App im Bildschirm **Konten** auf das Konto, das Sie wiederherstellen möchten, um eine Vollbildansicht des Kontos anzuzeigen.

    :::image type="content" source="media/user-help-auth-app-backup-recovery/backup-and-recovery-arrow.png" alt-text="Screenshot mit der Microsoft Authenticator-App mit verfügbaren Kontokacheln" border="true":::

1. Tippen Sie auf die Kachel für das Konto, das Sie wiederherstellen möchten, und tippen Sie dann auf die Anmeldeoption, um sich für die Wiederherstellung anzumelden. Geben Sie Ihr Kennwort ein, und bestätigen Sie in einem zusätzlichen Überprüfungsschritt Ihre E-Mail-Adresse oder Telefonnummer.

    :::image type="content" source="media/user-help-auth-app-backup-recovery/backup-and-recovery-codes.png" alt-text="Screenshot mit dem einmaligen Kennwortcode für Microsoft Authenticator" border="true":::

### <a name="to-provide-more-verification-for-work-or-school-accounts"></a>So führen Sie einen zusätzlichen Überprüfungsschritt für Geschäfts-, Schul- oder Unikonten aus

1. Tippen Sie in der Microsoft Authenticator-App im Bildschirm **Konten** auf das Konto, das Sie wiederherstellen möchten, um eine Vollbildansicht des Kontos anzuzeigen.

    :::image type="content" source="media/user-help-auth-app-backup-recovery/work-or-school-recovery-arrow.png" alt-text="Microsoft Authenticator-App mit den verfügbaren Kontokacheln" border="true":::

1. Tippen Sie für eine vollständige Wiederherstellung in der Vollbildansicht auf die Option zum Scannen eines QR-Codes.

    :::image type="content" source="media/user-help-auth-app-backup-recovery/work-or-school-recovery-qr-code.png" alt-text="Microsoft Authenticator stellt ein Einmalkennwort als Prüfcode bereit" border="true":::

>[!NOTE]
>Weitere Informationen zu QR-Codes und zum Abrufen eines QR-Codes finden Sie unter [Erste Schritte mit der Microsoft Authenticator-App](./user-help-auth-app-download-install.md) oder [Einrichten der Sicherheitsinformationen zur Verwendung einer Authentifikator-App](./security-info-setup-auth-app.md) – basierend darauf, ob Ihr Administrator die Sicherheitsinformation aktiviert hat.
>
>Wenn Sie die Microsoft Authenticator-App zum ersten Mal einrichten, werden Sie möglicherweise in einer Meldung gefragt, ob Sie der App den Zugriff auf Ihre Kamera (iOS) oder die Aufnahme von Foto- und Videodateien (Android) erlauben möchten. Sie müssen **Zulassen** auswählen, damit die Authentifikator-App im nächsten Schritt auf Ihre Kamera zugreifen und den QR-Code aufnehmen kann. Wenn Sie den Zugriff auf die Kamera nicht zulassen, können Sie die Authentifikator-App auch einrichten, müssen die Codeinformationen aber manuell hinzufügen. Informationen zum manuellen Hinzufügen des Codes finden Sie unter [Manuelles Hinzufügen eines Kontos zur App](user-help-auth-app-add-account-manual.md).

## <a name="troubleshoot-backup-and-recovery-problems"></a>Beheben von Problemen bei der Sicherung und Wiederherstellung

Es gibt einige Gründe, warum die Sicherung möglicherweise nicht verfügbar ist.

- **Änderung der Betriebssysteme**: Ihre Sicherung wird in der iCloud für iOS und bei Microsofts Cloudspeicheranbieter für Android gespeichert. Dies bedeutet, dass die Sicherung nicht verfügbar ist, wenn Sie zwischen Android- und iOS-Geräten wechseln. Bei einem Wechsel müssen Sie Ihre Konten in der Microsoft Authenticator-App manuell neu erstellen.

- **Netzwerkprobleme**: Wenn netzwerkbezogene Probleme auftreten, stellen Sie sicher, dass Sie mit dem Netzwerk verbunden und ordnungsgemäß bei Ihrem Konto angemeldet sind.

- **Kontoprobleme**: Wenn kontobezogene Probleme auftreten, stellen Sie sicher, dass Sie ordnungsgemäß bei Ihrem Konto angemeldet sind. Unter iOS bedeutet dies, dass Sie mit dem gleichen AppleID-Konto wie Ihr iPhone bei iCloud angemeldet sein müssen.

- **Versehentliche Löschung**: Möglicherweise haben Sie Ihr Sicherungskonto auf dem alten Gerät oder beim Verwalten des Cloudspeicherkontos gelöscht. In diesem Fall müssen Sie Ihr Konto in der App manuell neu erstellen.

- **Vorhandene Microsoft Authenticator-Konten**: Wenn Sie in der Microsoft Authenticator-App bereits Konten eingerichtet haben, kann die App die gesicherten Konten nicht wiederherstellen. Durch das Verhindern der Wiederherstellung wird sichergestellt, dass Ihre Kontodetails nicht mit veralteten Informationen überschrieben werden. In diesem Fall müssen Sie alle vorhandenen Kontoinformationen aus den in der Authenticator-App eingerichteten Konten entfernen, damit Sie die Sicherung wiederherstellen können.

- **Sicherung ist veraltet**: Wenn Ihre Sicherungsinformationen veraltet sind, werden Sie möglicherweise aufgefordert, die Informationen zu aktualisieren, indem Sie sich erneut bei Ihrem Microsoft-Wiederherstellungskonto anmelden. Bei Ihrem Wiederherstellungskonto handelt es sich um das persönliche Microsoft-Konto, das Sie ursprünglich zum Speichern Ihrer Sicherung verwendet haben. Wenn eine Anmeldung erforderlich ist, wird ein roter Punkt im Menü oder in der Aktionsleiste oder ein Ausrufezeichen angezeigt. Dadurch werden Sie aufgefordert, sich anzumelden, um die Wiederherstellung aus der Sicherung abzuschließen. Nachdem Sie das entsprechende Symbol ausgewählt haben, werden Sie aufgefordert, sich erneut anzumelden, um Ihre Informationen zu aktualisieren.

## <a name="next-steps"></a>Nächste Schritte

Sie haben Ihre Kontoinformationen gesichert und auf dem neuen Gerät wiederhergestellt und können nun mit der Microsoft Authenticator-App Ihre Identität bestätigen. Weitere Informationen finden Sie unter [Anmelden bei Ihren Konten mit der Microsoft Authenticator-App](user-help-sign-in.md).

## <a name="related-articles"></a>Verwandte Artikel

- [Wozu dient die Microsoft Authenticator-App?](user-help-auth-app-overview.md)

- [Microsoft Authenticator-App – häufig gestellte Fragen](user-help-auth-app-faq.md)

- [Multi-Factor Authentication](/azure/multi-factor-authentication/)