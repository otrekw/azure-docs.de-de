---
title: Markieren einer App als durch den Herausgeber verifiziert – Microsoft Identity Platform | Azure
description: Hier wird beschrieben, wie Sie eine App als durch den Herausgeber verifiziert markieren. Wenn eine Anwendung als durch den Herausgeber verifiziert markiert ist, bedeutet dies, dass der Herausgeber seine Identität mithilfe eines Microsoft Partner Network-Kontos überprüft hat, für das der Überprüfungsvorgang abgeschlossen wurde, und dieses MPN-Konto seiner Anwendungsregistrierung zugeordnet hat.
services: active-directory
author: rwike77
manager: CelesteDG
ms.service: active-directory
ms.subservice: develop
ms.topic: how-to
ms.workload: identity
ms.date: 05/08/2020
ms.author: ryanwi
ms.custom: aaddev
ms.reviewer: jesakowi
ms.openlocfilehash: d25a3831897ff8ad2e7dfb0c69910fee246aec21
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/02/2020
ms.locfileid: "85477684"
---
# <a name="mark-your-app-as-publisher-verified-preview"></a>Markieren Ihrer App als durch den Herausgeber verifiziert (Vorschau)

Wenn eine Anwendung als durch den Herausgeber verifiziert markiert ist, bedeutet dies, dass der Herausgeber seine Identität mithilfe seines Microsoft Partner Network-Kontos überprüft und dieses MPN-Konto seiner Anwendungsregistrierung zugeordnet hat. In diesem Artikel wird beschrieben, wie Sie die [Herausgeberüberprüfung (Vorschauversion)](publisher-verification-overview.md) ausführen.

## <a name="quickstart"></a>Schnellstart
Wenn Sie bereits bei Microsoft Partner Network (MPN) registriert sind und die [Voraussetzungen](publisher-verification-overview.md#requirements) erfüllt haben, können Sie sofort loslegen: 

1. Navigieren Sie zur Vorschauversion des [App-Registrierungsportals](https://aka.ms/PublisherVerificationPreview).

1. Wählen Sie eine App aus, und klicken Sie auf **Branding**. 

1. Klicken Sie auf **MPN-ID zum Überprüfen des Herausgebers hinzufügen**, und überprüfen Sie die aufgeführten Anforderungen.

1. Geben Sie Ihre MPN-ID ein, und klicken Sie auf **Überprüfen und speichern**.

Ausführlichere Informationen zu bestimmten Vorteilen und Anforderungen sowie häufig gestellte Fragen finden Sie in der [Übersicht](publisher-verification-overview.md).


## <a name="mark-your-app-as-publisher-verified"></a>Markieren Ihrer App als durch den Herausgeber verifiziert
Stellen Sie sicher, dass Sie die [Voraussetzungen](publisher-verification-overview.md#requirements) erfüllt haben, und führen Sie dann die folgenden Schritte aus, um Ihre App(s) als durch den Herausgeber verifiziert zu markieren.  

1. Stellen Sie sicher, dass Sie mit einem Organisationskonto (Azure AD) angemeldet sind, das autorisiert ist, Änderungen an der/den App(s), die Sie als durch den Herausgeber verifiziert markieren möchten, und am MPN-Konto in Partner Center vorzunehmen. 

    - In Azure AD muss dieser Benutzer entweder der Besitzer der App sein oder über eine der folgenden Rollen verfügen: „Anwendungsadministrator“, „Cloudanwendungsadministrator“ oder „Globaler Administrator“. 

    - In Partner Center muss dieser Benutzer über die folgenden Rollen verfügen: „MPN-Administrator“, „Kontoadministrator“ oder „Globaler Administrator“ (dies ist eine freigegebene Rolle, die in Azure AD verwaltet wird). 

1. Navigieren Sie zur Vorschauversion des App-Registrierungsportals:  

1. Klicken Sie auf eine App, die Sie als durch den Herausgeber verifiziert markieren möchten, und öffnen Sie das Blatt „Branding“. 

1. Stellen Sie sicher, dass die Herausgeberdomäne der App entsprechend festgelegt ist. Für diese Domäne gilt Folgendes: 

    - Sie muss dem Azure AD-Mandanten als DNS-verifizierte benutzerdefinierte Domäne hinzugefügt werden.  

    - Sie muss der Domäne der E-Mail-Adresse entsprechen, die bei der Überprüfung Ihres MPN-Kontos verwendet wurde. 

1. Klicken Sie unten auf der Seite auf **MPN-ID zum Überprüfen des Herausgebers hinzufügen**. 

1. Geben Sie Ihre **MPN-ID** ein. Diese MPN-ID muss für Folgendes verwendet werden: 

    - Ein gültiges Microsoft Partner Network-Konto, für das der Überprüfungsvorgang abgeschlossen wurde.  

    - Das globale Partnerkonto (Partner Global Account, PGA) für Ihre Organisation. 

1. Klicken Sie auf **Überprüfen und speichern**. 

1. Warten Sie, bis die Anforderung verarbeitet wurde. Dieser Vorgang kann einige Minuten dauern. 

1. Wenn die Überprüfung erfolgreich war, wird das Fenster für die Herausgeberüberprüfung geschlossen, und Sie kehren zum Blatt „Branding“ zurück. Neben dem überprüften **Anzeigenamen des Herausgebers** wird ein blauer Überprüfungsbadge angezeigt. 

1. Benutzer, die zur Zustimmung zu Ihrer App aufgefordert werden, sehen den Badge kurz nachdem der Prozess erfolgreich durchlaufen wurde. Allerdings kann es einige Zeit dauern, bis dies im gesamten System repliziert wird. 

1. Testen Sie diese Funktion, indem Sie sich bei Ihrer Anwendung anmelden und sich vergewissern, dass der Überprüfungsbadge auf dem Zustimmungsbildschirm angezeigt wird. Wenn Sie als Benutzer angemeldet sind, der dieser App bereits zugestimmt hat, können Sie den Abfrageparameter *prompt=consent* verwenden, um eine Zustimmungsaufforderung zu erzwingen. 

1. Wiederholen Sie diesen Vorgang nach Bedarf für alle weiteren Apps, für die der Badge angezeigt werden soll. Sie können Microsoft Graph verwenden, um dies in einem Massenvorgang schneller auszuführen. PowerShell-Cmdlets werden bald verfügbar sein. Weitere Informationen finden Sie unter [Ausführen von Microsoft Graph-API-Aufrufen](troubleshoot-publisher-verification.md#making-microsoft-graph-api-calls). 

Fertig! Lassen Sie uns wissen, ob Sie ggf. Feedback zum Vorgang, zu den Ergebnissen oder zur Funktion im Allgemeinen haben. 

## <a name="next-steps"></a>Nächste Schritte
Wenn Probleme auftreten, lesen Sie die [Informationen zur Problembehandlung](troubleshoot-publisher-verification.md).