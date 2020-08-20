---
title: Behandeln von Problemen mit Schlüsseltresor-Zugriffsrichtlinien
description: Behandeln von Problemen mit Schlüsseltresor-Zugriffsrichtlinien
author: sebansal
ms.author: sebansal
ms.date: 08/10/2020
ms.service: key-vault
ms.subservice: general
ms.topic: how-to
ms.openlocfilehash: f31782fb4e91b72f51d6f0550fe9010cba7ca3d6
ms.sourcegitcommit: 02ca0f340a44b7e18acca1351c8e81f3cca4a370
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/19/2020
ms.locfileid: "88585456"
---
# <a name="troubleshooting-azure-key-vault-access-policy-issues"></a>Behandeln von Problemen mit Schlüsseltresor-Zugriffsrichtlinien

## <a name="frequently-asked-questions"></a>Häufig gestellte Fragen

### <a name="how-can-i-identify-how-and-when-key-vaults-are-accessed"></a>Wie kann ich ermitteln, wie und wann auf Schlüsseltresore zugegriffen wird?
Nachdem Sie einen oder mehrere Schlüsseltresore erstellt haben, möchten Sie vermutlich überwachen, wie, wann und von wem auf die Schlüsseltresore zugegriffen wird. Hierzu können Sie die Protokollierung für Azure Key Vault aktivieren. Eine ausführliche Anleitung zum Aktivieren der Protokollierung finden Sie [hier](https://docs.microsoft.com/azure/key-vault/general/logging).

### <a name="how-can-i-monitor-vault-availability-service-latency-periods-or-other-performance-metrics-for-key-vault"></a>Wie kann ich die Tresorverfügbarkeit, Dienstwartezeiten oder andere Leistungsmetriken für einen Schlüsseltresor überwachen?
Wenn Sie Ihren Dienst skalieren, steigt die Anzahl von Anforderungen, die an Ihren Schlüsseltresor gesendet werden. Dies kann die Latenzzeit Ihrer Anforderungen erhöhen und in extremen Fällen zu einer Drosselung Ihrer Anforderungen führen – was sich auf die Leistung Ihres Diensts auswirkt. Sie können Schlüsseltresor-Leistungsmetriken überwachen und Warnungen für bestimmte Schwellenwerte konfigurieren. Eine ausführliche Anleitung zum Konfigurieren der Überwachung finden Sie [hier](https://docs.microsoft.com/azure/key-vault/general/alert).

### <a name="how-can-i-assign-access-control-per-key-vault-object"></a>Wie kann ich die Zugriffssteuerung pro Schlüsseltresorobjekt zuweisen? 
Informationen zur Verfügbarkeit des geheimnis-/schlüssel-/zertifikatspezifischen Zugriffssteuerungsfeatures finden Sie [hier](https://feedback.azure.com/forums/906355-azure-key-vault/suggestions/.32213176-per-secret-key-certificate-access-control).

### <a name="how-can-i-provide-key-vault-authenticate-using-access-control-policy"></a>Wie kann ich die Schlüsseltresorauthentifizierung per Zugriffssteuerungsrichtlinie bereitstellen?
Die Authentifizierung einer cloudbasierten Anwendung bei Key Vault erfolgt am einfachsten mit einer verwalteten Identität. Einzelheiten hierzu finden Sie unter [Verwenden einer verwalteten App Service-Identität für den Zugriff auf Azure Key Vault]( https://docs.microsoft.com/azure/key-vault/general/managed-identity).
Wenn Sie eine lokale Anwendung erstellen, lokal entwickeln oder aus anderen Gründen keine verwaltete Identität verwenden können, können Sie stattdessen manuell einen Dienstprinzipal registrieren und mithilfe einer Zugriffssteuerungsrichtlinie Zugriff auf Ihren Schlüsseltresor bereitstellen. Weitere Informationen finden Sie [hier](https://docs.microsoft.com/azure/key-vault/general/group-permissions-for-apps).


### <a name="how-can-i-give-the-ad-group-access-to-the-key-vault"></a>Wie kann ich der AD-Gruppe Zugriff auf den Schlüsseltresor gewähren?
Verwenden Sie entweder den Azure CLI-Befehl „az keyvault set-policy“ oder das Azure PowerShell-Cmdlet „Set-AzKeyVaultAccessPolicy“, um der AD-Gruppe Berechtigungen für Ihren Schlüsseltresor zu erteilen. Beispiele finden Sie im Abschnitt [Gewähren des Zugriffs auf Ihren Schlüsseltresor für den Prinzipal](https://docs.microsoft.com/azure/key-vault/general/group-permissions-for-apps#give-the-principal-access-to-your-key-vault).

Für die Anwendung muss dem Schlüsseltresor darüber hinaus mindestens eine IAM-Rolle (Identity and Access Management, Identitäts- und Zugriffsverwaltung) zugewiesen werden. Andernfalls ist die Anmeldung nicht möglich, und es tritt ein Fehler aufgrund unzureichender Zugriffsberechtigungen für das Abonnement auf. Azure AD-Gruppen mit verwalteten Identitäten benötigen möglicherweise bis zu 8 Stunden, bis das Token aktualisiert ist und wirksam wird.

### <a name="how-can-i-redeploy-key-vault-with-arm-template-without-deleting-existing-access-policies"></a>Wie kann ich Key Vault per ARM-Vorlage erneut bereitstellen, ohne vorhandene Zugriffsrichtlinien zu löschen?
Aktuell werden bei der erneuten ARM-basierten Key Vault-Bereitstellung alle Zugriffsrichtlinien in Key Vault gelöscht und durch die Zugriffsrichtlinie aus der ARM-Vorlage ersetzt. Für Key Vault-Zugriffsrichtlinien steht keine inkrementelle Option zur Verfügung. Wenn die Zugriffsrichtlinien in Key Vault erhalten bleiben sollen, müssen Sie die vorhandene Zugriffsrichtlinien in Key Vault lesen und die ARM-Vorlage mit diesen Richtlinien auffüllen, um Zugriffsausfälle zu vermeiden.

### <a name="recommended-troubleshooting-steps-for-following-error-types"></a>Empfohlene Schritte zur Problembehandlung bei folgenden Fehlertypen
* HTTP 401: Nicht authentifizierte Anforderung: [Schritte zur Problembehandlung](https://docs.microsoft.com/azure/key-vault/general/rest-error-codes#http-401-unauthenticated-request)
* HTTP 403: Unzureichende Berechtigungen: [Schritte zur Problembehandlung](https://docs.microsoft.com/azure/key-vault/general/rest-error-codes#http-403-insufficient-permissions)
* HTTP 429: Zu viele Anforderungen: [Schritte zur Problembehandlung](https://docs.microsoft.com/azure/key-vault/general/rest-error-codes#http-429-too-many-requests)
* Überprüfen Sie, ob Sie für den Schlüsseltresor über die Zugriffsberechtigung zum Löschen verfügen: [Bereitstellen der Key Vault-Authentifizierung mit einer Zugriffssteuerungsrichtlinie](https://docs.microsoft.com/azure/key-vault/general/group-permissions-for-apps)
* Verwenden Sie das [Authentifizierungs-SDK](https://azure.github.io/azure-sdk/posts/2020-02-25/defaultazurecredentials.html), wenn bei Ihnen ein Problem mit der Authentifizierung für den Schlüsseltresor im Code besteht.

### <a name="what-are-the-best-practices-i-should-implement-when-key-vault-is-getting-throttled"></a>Welche bewährten Methoden sollten implementiert werden, wenn der Schlüsseltresor gedrosselt wird?
Bewährte Methoden für diesen Fall finden Sie [hier](https://docs.microsoft.com/azure/key-vault/general/overview-throttling#how-to-throttle-your-app-in-response-to-service-limits).

## <a name="next-steps"></a>Nächste Schritte

Informieren Sie sich, wie Sie Fehler beheben, die bei der Authentifizierung beim Schlüsseltresor auftreten. [Leitfaden zur Problembehandlung für Key Vault](https://docs.microsoft.com/azure/key-vault/general/rest-error-codes)
