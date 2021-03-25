---
title: 'Problembehandlung bei der Anwendungsänderungsanalyse: Azure Monitor'
description: Informationen zum Behandeln von Problemen bei der Anwendungsänderungsanalyse.
ms.topic: conceptual
author: cawams
ms.author: cawa
ms.date: 02/11/2021
ms.openlocfilehash: 7200978ce31a47f7bd0d023cecf359b10a1c96de
ms.sourcegitcommit: 867cb1b7a1f3a1f0b427282c648d411d0ca4f81f
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/19/2021
ms.locfileid: "100520785"
---
# <a name="troubleshoot-application-change-analysis-preview"></a>Problembehandlung bei der Anwendungsänderungsanalyse (Vorschau)

## <a name="having-trouble-registering-microsoft-change-analysis-resource-provider-from-change-history-tab"></a>Probleme beim Registrieren des Microsoft. ChangeAnalysis-Ressourcenanbieters auf der Registerkarte „Änderungsverlauf“

Wenn Sie den Änderungsverlauf nach der Integration in die Anwendungsänderungsanalyse zum ersten Mal anzeigen, werden Sie feststellen, dass automatisch ein **Microsoft.ChangeAnalysis**-Ressourcenanbieter registriert wird. In seltenen Fällen kann dabei aus folgenden Gründen ein Fehler auftreten:

- **Sie verfügen nicht über ausreichende Berechtigungen zum Registrieren des Microsoft.ChangeAnalysis-Ressourcenanbieters**. Diese Fehlermeldung bedeutet, dass Ihrer Rolle im aktuellen Abonnement der Bereich **Microsoft.Support/register/action** nicht zugeordnet ist. Dies kann vorkommen, wenn Sie nicht der Besitzer eines Abonnements sind und über einen Kollegen Berechtigungen für den gemeinsamen Zugriff erhalten haben (d. h. Ansichtszugriff auf eine Ressourcengruppe). Um dieses Problem zu beheben, können Sie sich an den Besitzer Ihres Abonnements wenden, um den **Microsoft.ChangeAnalysis**-Ressourcenanbieter registrieren zu lassen. Dies kann im Azure-Portal erfolgen, indem **Abonnements | Ressourcenanbieter** aufgerufen, nach ```Microsoft.ChangeAnalysis``` gesucht und der Ressourcenanbieter auf der Benutzeroberfläche registriert wird. Dies ist auch über Azure PowerShell oder die Azure CLI möglich.

    Registrieren des Ressourcenanbieters über PowerShell:
    ```PowerShell
    # Register resource provider
    Register-AzResourceProvider -ProviderNamespace "Microsoft.ChangeAnalysis"
    ```

- **Fehler beim Registrieren des Microsoft.ChangeAnalysis-Ressourcenanbieters**. Diese Meldung bedeutet, dass beim Senden der Registrierungsanforderung durch die Benutzeroberfläche sofort ein Fehler aufgetreten ist und dieser nicht mit einem Berechtigungsproblem im Zusammenhang steht. Wahrscheinlich handelt es sich um ein vorübergehendes Problem mit der Internetverbindung. Aktualisieren Sie die Seite, und überprüfen Sie Ihre Internetverbindung. Wenn der Fehler weiterhin auftritt, wenden Sie sich an changeanalysishelp@microsoft.com.

- **Der Vorgang dauert länger als erwartet**. Diese Meldung bedeutet, dass die Registrierung länger als zwei Minuten dauert. Dies ist ungewöhnlich, bedeutet aber nicht unbedingt, dass ein Fehler aufgetreten ist. Sie können zu **Abonnements | Ressourcenanbieter** navigieren, um den Registrierungsstatus für den **Microsoft.ChangeAnalysis**-Ressourcenanbieter zu prüfen. Sie können über die Benutzeroberfläche eine Aufhebung der Registrierung, Neuregistrierung oder Aktualisierung durchführen, um zu sehen, ob dadurch Abhilfe geschaffen wird. Wenn das Problem weiterhin besteht, wenden Sie sich an changeanalysishelp@microsoft.com, um Unterstützung zu erhalten.
    ![Problembehandlung bei zu lange dauernder Registrierung des Ressourcenanbieters](./media/change-analysis/troubleshoot-registration-taking-too-long.png)

![Screenshot des Diagnose- und Problembehandlungstools für einen virtuellen Computer mit ausgewählten Problembehandlungstools](./media/change-analysis/vm-dnsp-troubleshootingtools.png)

![Screenshot der Kachel für das Problembehandlungstool zum Analysieren von aktuellen Änderungen für einen virtuellen Computer](./media/change-analysis/analyze-recent-changes.png)

## <a name="azure-lighthouse-subscription-is-not-supported"></a>Azure Lighthouse-Abonnements werden nicht unterstützt.

- **Fehler beim Abfragen des Microsoft.ChangeAnalysis-Ressourcenanbieters** mit der Meldung, dass das *Azure Lighthouse-Abonnement nicht unterstützt wird und die Änderungen nur im Basismandanten des Abonnements verfügbar sind*. Derzeit besteht eine Einschränkung für die Registrierung des Änderungsanalyse-Ressourcenanbieters über das Azure Lighthouse-Abonnement für Benutzer, die nicht dem Basismandanten angehören. Wir arbeiten derzeit an einer Lösung für diese Einschränkung. Wenn Sie durch dieses Problem blockiert werden, gibt es eine Problemumgehung, die das Erstellen eines Dienstprinzipals und das explizite Zuweisen der Rolle zum Zulassen des Zugriffs umfasst.  Wenden Sie sich an changeanalysishelp@microsoft.com, um mehr darüber zu erfahren.

## <a name="an-error-occurred-while-getting-changes-please-refresh-this-page-or-come-back-later-to-view-changes"></a>Fehler beim Abrufen von Änderungen. Aktualisieren Sie diese Seite, oder kehren Sie später zurück, um Änderungen anzuzeigen.

Dies ist die allgemeine Fehlermeldung, die vom Anwendungsänderungsanalyse-Dienst angezeigt wird, wenn Änderungen nicht geladen werden konnten. Einige bekannte Ursachen sind:

- Internetverbindungsfehler vom Clientgerät
- Anwendungsänderungsanalyse-Dienst ist vorübergehend nicht verfügbar. Durch Aktualisieren der Seite nach einigen Minuten wird dieses Problem normalerweise behoben. Wenn der Fehler weiterhin auftritt, wenden Sie sich an changeanalysishelp@micorosoft.com.

## <a name="you-dont-have-enough-permissions-to-view-some-changes-contact-your-azure-subscription-administrator"></a>Sie verfügen nicht über ausreichende Berechtigungen zum Anzeigen einiger Änderungen. Wenden Sie sich an Ihren Azure-Abonnementadministrator.

Dies ist die allgemeine Fehlermeldung für fehlende Autorisierung, in der erläutert wird, dass der aktuelle Benutzer nicht über ausreichende Berechtigungen zum Anzeigen der Änderung verfügt. Es ist mindestens Lesezugriff für die Ressource erforderlich, um Infrastrukturänderungen anzuzeigen, die von Azure Resource Graph und Azure Resource Manager zurückgegeben werden. Bei Web-App-Dateiänderungen des Gastsystems und Konfigurationsänderungen ist mindestens die Rolle „Mitwirkender“ erforderlich.

## <a name="failed-to-register-microsoftchangeanalysis-resource-provider"></a>Fehler beim Registrieren des Microsoft.ChangeAnalysis-Ressourcenanbieters

Diese Meldung bedeutet, dass beim Senden der Registrierungsanforderung durch die Benutzeroberfläche sofort ein Fehler aufgetreten ist und dieser nicht mit einem Berechtigungsproblem im Zusammenhang steht. Wahrscheinlich handelt es sich um ein vorübergehendes Problem mit der Internetverbindung. Aktualisieren Sie die Seite, und überprüfen Sie Ihre Internetverbindung. Wenn der Fehler weiterhin auftritt, wenden Sie sich an changeanalysishelp@microsoft.com.

## <a name="you-dont-have-enough-permissions-to-register-microsoftchangeanalysis-resource-provider-contact-your-azure-subscription-administrator"></a>Sie verfügen nicht über ausreichende Berechtigungen zum Registrieren des Microsoft.ChangeAnalysis-Ressourcenanbieters. Wenden Sie sich an Ihren Azure-Abonnementadministrator.

Diese Fehlermeldung bedeutet, dass Ihrer Rolle im aktuellen Abonnement der Bereich **Microsoft.Support/register/action** nicht zugeordnet ist. Dies kann vorkommen, wenn Sie nicht der Besitzer eines Abonnements sind und über einen Kollegen Berechtigungen für den gemeinsamen Zugriff erhalten haben (d. h. Ansichtszugriff auf eine Ressourcengruppe). Um dieses Problem zu beheben, können Sie sich an den Besitzer Ihres Abonnements wenden, um den **Microsoft.ChangeAnalysis**-Ressourcenanbieter registrieren zu lassen. Dies kann im Azure-Portal erfolgen, indem **Abonnements | Ressourcenanbieter** aufgerufen, nach ```Microsoft.ChangeAnalysis``` gesucht und der Ressourcenanbieter auf der Benutzeroberfläche registriert wird. Dies ist auch über Azure PowerShell oder die Azure CLI möglich.

Registrieren des Ressourcenanbieters über PowerShell:

```PowerShell
# Register resource provider
Register-AzResourceProvider -ProviderNamespace "Microsoft.ChangeAnalysis"
```

## <a name="next-steps"></a>Nächste Schritte

- Erfahren Sie mehr über [Azure Resource Graph](../../governance/resource-graph/overview.md), der die Änderungsanalyse unterstützt.