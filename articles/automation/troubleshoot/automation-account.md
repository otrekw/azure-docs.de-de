---
title: Problembehandlung bei Automation-Konten
description: Erfahren Sie, wie Sie Probleme mit einem Azure-Konto behandeln und beheben können.
services: automation
author: mgoedtel
ms.author: magoedte
ms.date: 03/24/2020
ms.topic: conceptual
ms.service: automation
manager: carmonm
ms.openlocfilehash: 7b5e7171ac679384966e9dce79425cd1fa881c53
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/28/2020
ms.locfileid: "81679380"
---
# <a name="troubleshoot-the-automation-account"></a>Problembehandlung bei Automation-Konten

In diesem Artikel werden Lösungen für Probleme beschrieben, die bei Verwendung eines Automation-Kontos auftreten können. Die folgenden Abschnitte enthalten spezifische Fehlermeldungen und passende mögliche Lösungsvorschläge. Allgemeine Informationen zu Automation-Konten finden Sie unter [Erstellen eines Azure-Kontos](../automation-quickstart-create-account.md).

## <a name="scenario-unable-to-register-automation-resource-provider-for-subscriptions"></a><a name="rp-register"></a>Szenario: Der Automation-Ressourcenanbieter für Abonnements kann nicht registriert werden.

### <a name="issue"></a>Problem

Beim Arbeiten mit Verwaltungslösungen in Ihrem Automation-Konto können die folgenden Fehler auftreten:

```error
Error details: Unable to register Automation Resource Provider for subscriptions:
```

### <a name="cause"></a>Ursache

Der Automation-Ressourcenanbieter ist nicht im Abonnement registriert.

### <a name="resolution"></a>Lösung

Führen Sie im Azure-Portal die folgenden Schritte aus, um den Automation-Ressourcenanbieter zu registrieren:

1. Navigieren Sie in Ihrem Browser zum [Azure-Portal](https://portal.azure.com).

2. Navigieren Sie zu **Abonnements**, und wählen Sie Ihr Abonnement auf der Seite „Abonnements“ aus.   

3. Wählen Sie unter **Einstellungen** die Option **Ressourcenanbieter** aus.

4. Überprüfen Sie in der Liste der Ressourcenanbieter, ob der Ressourcenanbieter **Microsoft.Automation** registriert ist.

5. Wenn der Anbieter nicht aufgeführt ist, registrieren Sie ihn, so wie unter [Beheben von Fehlern bei der Ressourcenanbieterregistrierung](/azure/azure-resource-manager/resource-manager-register-provider-errors) beschrieben.

## <a name="next-steps"></a>Nächste Schritte

Wenn Ihr Problem nicht aufgeführt wird oder Sie es nicht lösen können, besuchen Sie einen der folgenden Kanäle, um weitere Unterstützung zu erhalten:

* Erhalten Sie Antworten von Azure-Experten über [Azure-Foren](https://azure.microsoft.com/support/forums/).
* Nutzen Sie [@AzureSupport](https://twitter.com/azuresupport) – das offizielle Microsoft Azure-Konto zur Verbesserung der Benutzerfreundlichkeit. Hierüber hat die Azure-Community Zugriff auf die richtigen Ressourcen: Antworten, Support und Experten.
* Erstellen Sie einen Azure-Supportfall. Rufen Sie die [Azure-Support-Website](https://azure.microsoft.com/support/options/) auf, und wählen Sie **Support erhalten**aus.