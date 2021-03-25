---
title: Problembehandlung bei Warnungen und Benachrichtigungen von Azure Monitor
description: In diesem Artikel werden gängige Probleme bei Azure Monitor-Warnungen und mögliche Lösungen behandelt.
author: ofirmanor
ms.author: ofmanor
ms.topic: reference
ms.date: 03/16/2020
ms.openlocfilehash: 08ac23fbb29829bf2a7a46e41f9a7f646ad97915
ms.sourcegitcommit: 772eb9c6684dd4864e0ba507945a83e48b8c16f0
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/19/2021
ms.locfileid: "104586317"
---
# <a name="troubleshooting-problems-in-azure-monitor-alerts"></a>Behandeln von Problemen bei Azure Monitor-Warnungen

In diesem Artikel werden gängige Probleme bei Azure Monitor-Warnungen und -Benachrichtigungen erläutert.

Azure Monitor-Warnungen informieren Sie proaktiv, wenn wichtige Bedingungen in Ihren Überwachungsdaten gefunden werden. Sie ermöglichen es Ihnen, Probleme zu identifizieren und zu beheben, bevor die Benutzer Ihres Systems sie bemerken. Weitere Informationen über Warnungen finden Sie unter [Überblick über Warnungen in Microsoft Azure](./alerts-overview.md).

Wenn bei Ihnen das Problem besteht, dass eine Warnung entweder nicht erwartungsgemäß ausgelöst wird oder unerwartet ausgelöst wird, finden Sie in den untenstehenden Artikeln weitere Informationen. Alle ausgelösten Warnungen können im Azure-Portal angezeigt werden.

- [Troubleshooting für Probleme mit Metrikwarnungen in Azure Monitor](alerts-troubleshoot-metric.md)  
- [Troubleshooting für Probleme mit Protokollwarnungen in Azure Monitor](alerts-troubleshoot-log.md)

Wenn die Warnung laut der Informationen im Azure-Portal wie beabsichtigt ausgelöst wird, die entsprechenden Benachrichtigungen jedoch nicht angezeigt werden, finden Sie im vorliegenden Artikel weitere Informationen für ein Troubleshooting dieses Problems.

## <a name="action-or-notification-on-my-alert-did-not-work-as-expected"></a>Eine Aktion oder Benachrichtigung für meine Warnung funktioniert nicht erwartungsgemäß

Wenn eine ausgelöste Warnung im Azure-Portal angezeigt wird, Sie aber ein Problem mit einigen ihrer Aktionen oder Benachrichtigungen haben, finden Sie weitere Informationen in den nachfolgenden Abschnitten.

## <a name="did-not-receive-expected-email"></a>Erwartete E-Mail ist nicht eingegangen

Führen Sie die folgenden Schritte aus, wenn eine ausgelöste Warnung im Azure-Portal angezeigt wird, aber Sie haben die konfigurierte E-Mail nicht empfangen:

1. **Wurde die E-Mail aufgrund einer [Aktionsregel](../alerts/alerts-action-rules.md) unterdrückt?**

    Überprüfen Sie dies, indem Sie im Portal auf die ausgelöste Warnung klicken und die Registerkarte „Verlauf“ auf unterdrückte [Aktionsgruppen](./action-groups.md) untersuchen:

    ![Verlauf mit unterdrückten Aktionsregeln bei einer Warnung](media/alerts-troubleshoot/history-action-rule.png)

1. **Handelt es sich um eine Aktion vom Typ „E-Mail an Azure Resource Manager-Rolle“?**

    Diese Aktion untersucht lediglich Azure Resource Manager-Rollenzuweisungen, die sich im Abonnementbereich befinden und den Typ *Benutzer* aufweisen.  Stellen Sie sicher, dass Sie die Rolle auf Abonnementebene zugewiesen haben und nicht auf Ressourcen- oder Ressourcengruppenebene.

1. **Akzeptieren Ihr E-Mail-Server und Postfach externe E-Mails?**

    Stellen Sie sicher, dass E-Mails von den folgenden drei Adressen nicht blockiert werden:
      - azure-noreply@microsoft.com  
      - azureemail-noreply@microsoft.com
      - alerts-noreply@mail.windowsazure.com

    Es ist üblich, dass interne Mailinglisten oder Verteilerlisten E-Mails von externen E-Mail-Adressen blockieren. Sie müssen E-Mails von den obigen E-Mail-Adressen zulassen.  
    Zum Testen können Sie eine herkömmliche geschäftliche E-Mail-Adresse (keine Mailingliste) zur Aktionsgruppe hinzufügen und überprüfen, ob Warnungen bei ihr eintreffen.

1. **Wurde die E-Mail von meinen Posteingangsregeln oder einem Spamfilter verarbeitet?**

    Stellen Sie sicher, dass keine Eingangsregeln vorliegen, die diese E-Mails löschen oder in einen separaten Ordner verschieben. Beispielsweise könnten Eingangsregeln spezifische Absender oder Wörter im Betreff abfangen.

    Überprüfen Sie außerdem:

   - die Spameinstellungen Ihres E-Mail-Clients (z. B. Outlook, Gmail).
      - die Absenderbeschränkungen/Spameinstellungen/Quarantäneeinstellungen Ihres E-Mail-Servers (z. B. Exchange, Microsoft 365, G-Suite).
      - die Einstellungen Ihres E-Mail-Sicherheitsgeräts, sofern eines vorliegt (z. B. Barracuda, Cisco).

1. **Haben Sie sich versehentlich von der Aktionsgruppe abgemeldet?**

    Die Warnungs-E-Mails enthalten einen Link zum Kündigen des Abonnements der Aktionsgruppe. Sie können einen der folgenden Schritte ausführen, um zu überprüfen, ob Sie versehentlich Ihr Abonnement der Aktionsgruppe gekündigt haben:

    1. Öffnen Sie die Aktionsgruppe im Portal, und überprüfen Sie die Statusspalte:

    ![Statusspalte der Aktionsgruppe](media/alerts-troubleshoot/action-group-status.png)

    2. Suchen Sie in Ihrem E-Mail-Postfach nach einer Bestätigung der Abonnementkündigung:

    ![Kündigung des Abonnements einer Warnungsaktionsgruppe](media/alerts-troubleshoot/unsubscribe-action-group.png)

    Sie können entweder den in der Bestätigungs-E-Mail für die Abonnementkündigung enthaltenen Link verwenden oder die E-Mail-Adresse aus der Aktionsgruppe entfernen und wieder hinzufügen, um sie wieder zu abonnieren. 
 
1. **Haben Sie aufgrund zu vieler E-Mails an eine einzige E-Mail-Adresse eine Mengenbeschränkung erhalten?**

    Es besteht die [Beschränkung](alerts-rate-limiting.md), dass pro Stunde nicht mehr als 100 E-Mails bei einer E-Mail-Adresse eingehen dürfen. Wenn Sie diesen Schwellenwert überschreiten, werden weitere E-Mail-Benachrichtigungen gelöscht.  Überprüfen Sie, ob Sie eine Nachricht erhalten haben, die angibt, dass eine vorübergehende Mengenbeschränkung für Ihre E-Mail-Adresse festgelegt wurde: 
 
   ![E-Mail-Adresse mit Mengenbeschränkung](media/alerts-troubleshoot/email-paused.png)

   Wenn Sie eine große Menge an Benachrichtigungen ohne Mengenbeschränkung erhalten möchten, sollten Sie eine andere Aktion in Betracht ziehen, z. B. einen Webhook, eine Logik-App, eine Azure-Funktion oder Automation-Runbooks. Für keine der genannten Optionen besteht eine Mengenbeschränkung. 

## <a name="did-not-receive-expected-sms-voice-call-or-push-notification"></a>SMS, Sprachanruf oder Pushbenachrichtigung ist nicht erwartungsgemäß eingegangen

Führen Sie die folgenden Schritte aus, wenn eine ausgelöste Warnung im Azure-Portal angezeigt wird, aber Sie haben die konfigurierte SMS, den Sprachanruf oder die Pushbenachrichtigung nicht empfangen: 

1. **Wurde die Aktion aufgrund einer [Aktionsregel](../alerts/alerts-action-rules.md) unterdrückt?**

    Überprüfen Sie dies, indem Sie im Portal auf die ausgelöste Warnung klicken und die Registerkarte „Verlauf“ auf unterdrückte [Aktionsgruppen](./action-groups.md) untersuchen: 

    ![Verlauf mit unterdrückten Aktionsregeln bei einer Warnung](media/alerts-troubleshoot/history-action-rule.png)

   Wenn dies nicht beabsichtigt ist, können Sie die Aktionsregel ändern, deaktivieren oder löschen.
 
1. **SMS/Sprachanruf:  Ist die angegebene Telefonnummer richtig?**

   Überprüfen Sie die SMS-Aktion auf Tippfehler im Ländercode und der Telefonnummer.
 
1. **SMS/Sprachanruf: Liegt eine Mengenbeschränkung vor?**

    Für SMS und Sprachanrufe gilt eine Beschränkung von nicht mehr als einer Benachrichtigung alle fünf Minuten pro Telefonnummer. Wenn Sie diesen Schwellenwert überschreiten, werden die Benachrichtigungen gelöscht.

      - Sprachanruf: Überprüfen Sie Ihren Anrufverlauf auf vorangegangene Anrufe von Azure in den letzten fünf Minuten.
      - SMS: Überprüfen Sie Ihren SMS-Verlauf auf eine Nachricht mit dem Hinweis, dass für Ihre Telefonnummer eine Beschränkung besteht.

    Wenn Sie eine große Menge an Benachrichtigungen ohne Mengenbeschränkung erhalten möchten, sollten Sie eine andere Aktion in Betracht ziehen, z. B. einen Webhook, eine Logik-App, eine Azure-Funktion oder Automation-Runbooks. Für keine der genannten Optionen besteht eine Mengenbeschränkung. 
 
1. **SMS: Haben Sie sich versehentlich von der Aktionsgruppe abgemeldet?**

    Öffnen Sie Ihren SMS-Verlauf, und überprüfen Sie, ob Sie sich von der SMS-Benachrichtigungen dieser spezifischen Aktionsgruppe (mit der Antwort „DISABLE action_group_short_name“) oder allen Aktionsgruppen (mit der Antwort „STOP“) abgemeldet haben. Senden Sie entweder den entsprechenden SMS-Befehl (ENABLE action_group_short_name oder START) oder entfernen Sie die SMS-Aktion aus der Aktionsgruppe, und fügen Sie sie dann wieder hinzu, um sie wieder zu abonnieren.  Weitere Informationen finden Sie unter [SMS-Warnungsverhalten in Aktionsgruppen](alerts-sms-behavior.md).

1. **Haben Sie die Benachrichtigungen versehentlich auf Ihrem Mobiltelefon blockiert?**

   Die meisten Mobiltelefone bieten die Möglichkeit, Anrufe oder SMS von spezifischen Telefonnummern oder kurzen Codes oder die Pushbenachrichtigungen von bestimmten Apps (z. B. der mobilen Azure-App) zu blockieren. Überprüfen Sie Dokumentation für das Betriebssystem und Modell Ihres Mobilgeräts, um zu erfahren, wie Sie überprüfen können, ob Sie die Benachrichtigungen auf Ihrem Gerät versehentlich blockiert haben. Alternativ können Sie dies auch mit einem anderen Mobilgerät und einer anderen Telefonnummer testen.

## <a name="expected-another-type-of-action-to-trigger-but-it-did-not"></a>Eine Andere Art von Aktion wurde erwartet, aber nicht ausgelöst 
   
Wenn eine ausgelöste Warnung im Portal angezeigt wird, deren konfigurierte Aktion jedoch nicht ausgelöst wurde, führen Sie die folgenden Schritte aus:

1. **Wurde die Aktion aufgrund einer Aktionsregel unterdrückt?**

    Überprüfen Sie dies, indem Sie im Portal auf die ausgelöste Warnung klicken und die Registerkarte „Verlauf“ auf unterdrückte [Aktionsgruppen](./action-groups.md) untersuchen:

    ![Verlauf mit unterdrückten Aktionsregeln bei einer Warnung](media/alerts-troubleshoot/history-action-rule.png)
 
    Wenn dies nicht beabsichtigt ist, können Sie die Aktionsregel ändern, deaktivieren oder löschen.

1. **Wurde ein Webhook nicht ausgelöst?**

    1. **Wurden die Quell-IP-Adressen blockiert?**
    
       Fügen Sie die [IP-Adressen](./action-groups.md#action-specific-information), von denen der Webhook aufgerufen wird, zur Whitelist hinzu.

    1. **Funktioniert Ihr Webhookendpunkt ordnungsgemäß?**

       Überprüfen Sie, ob der von Ihnen konfigurierte Webhookendpunkt korrekt ist und ob der Endpunkt ordnungsgemäß funktioniert. Überprüfen Sie Ihre Webhookprotokolle, oder instrumentieren Sie den Code so, dass Sie Untersuchungen anstellen können (protokollieren Sie beispielsweise die eingehende Nutzlast).

    1. **Rufen Sie Slack oder Microsoft Teams auf?**  
    Jeder dieser Endpunkte erwartet ein spezifischen JSON-Format. Befolgen Sie [diese Anweisungen](../alerts/action-groups-logic-app.md), um stattdessen eine Logik-App-Aktion zu konfigurieren.

    1. **Reagiert Ihr Webhook nicht, oder hat er Fehler zurückgegeben?** 

        Das Timeout für eine Antwort vom Webhook beträgt 10 Sekunden. Es wird bis zu zwei weitere Male versucht, den Webhookaufruf durchzuführen, wenn die HTTP-Statuscodes 408, 429, 503 oder 504 zurückgegeben werden oder der HTTP-Endpunkt nicht reagiert. Der erste Wiederholungsversuch erfolgt nach 10 Sekunden. Der zweite und letzte Wiederholungsversuch erfolgt nach 100 Sekunden. Wenn der zweite Wiederholungsversuch nicht erfolgreich ist, wird der Endpunkt für Aktionsgruppen 30 Minuten lang nicht erneut aufgerufen.

## <a name="action-or-notification-happened-more-than-once"></a>Die Aktion oder Benachrichtigung ist mehrmals aufgetreten 

Wenn Sie eine Benachrichtigung für eine Warnung (z. B. E-Mail oder SMS) mehrmals erhalten haben, oder die Aktion der Warnung (z. B. ein Webhook oder eine Azure-Funktion) mehrmals ausgelöst wurde, führen Sie die folgenden Schritte aus: 

1. **Handelt es sich wirklich um dieselbe Warnung?** 

    In einigen Fällen werden mehrere ähnliche Warnungen etwa zur gleichen Zeit ausgelöst. Es kann also so aussehen, als hätte dieselbe Warnung ihre Aktionen mehrmals ausgelöst. Eine Warnungsregel des Aktivitätsprotokolls könnte beispielsweise so konfiguriert sein, dass sie ausgelöst wird, wenn ein Ereignis beginnt und wenn es abgeschlossen ist (erfolgreich oder fehlerhaft), indem nicht nach dem Ereignisstatusfeld gefiltert wird. 

    Überprüfen Sie die Warnungsdetails, z. B. den Zeitstempel und die Warnungs-ID oder die Korrelations-ID, um zu überprüfen, ob diese Aktionen oder Benachrichtigungen von unterschiedlichen Warnungen stammen. Überprüfen Sie alternativ die Liste der ausgelösten Warnungen im Portal. Wenn dies der Fall ist, müssen Sie die Logik der Warnungsregel anpassen oder die Warnungsquelle anderweitig konfigurieren. 

1. **Wird die Aktion in mehreren Aktionsgruppen wiederholt?** 

    Wenn eine Warnung ausgelöst wird, werden alle ihrer Aktionsgruppen unabhängig verarbeitet. Wenn also eine Aktion (z. B. eine E-Mail-Adresse) in mehreren ausgelösten Aktionsgruppen angezeigt wird, würde sie für jede Aktionsgruppe einmal aufgerufen werden. 

    Überprüfen Sie auf der Registerkarte „Warnungsverlauf“, welche Aktionsgruppen ausgelöst wurden. Dort würden beide in der Warnungsregel definierten Aktionsgruppen sowie Aktionsgruppen angezeigt werden, die von Aktionsregeln zu der Warnung hinzugefügt wurden: 

    ![Aktion wird in mehreren Aktionsgruppen wiederholt](media/alerts-troubleshoot/action-repeated-multi-action-groups.png)

## <a name="action-or-notification-has-an-unexpected-content"></a>Aktion oder Benachrichtigung weist einen unerwarteten Inhalt auf

Wenn Sie die Warnung erhalten haben, aber glauben, dass einige ihrer Felder fehlen oder fehlerhaft sind, führen Sie die folgenden Schritte aus: 

1. **Haben Sie das richtige Format für die Aktion ausgewählt?** 

    Alle Aktionstypen (E-Mail, Webhook usw.) verfügen über zwei Formate: das Standardformat (Legacyformat) und das [neuere allgemeine Schemaformat](../alerts/alerts-common-schema.md). Beim Erstellen einer Aktionsgruppe legen Sie für jede Aktion das gewünschte Format an. Verschiedene Aktionen in den Aktionsgruppen können unterschiedliche Formate aufweisen. 

    Zum Beispiel die Webhook-Aktion: 

    ![Webhook-Aktionsschemaoption](media/alerts-troubleshoot/webhook.png)

    Überprüfen Sie, ob das auf der Aktionsebene angegebene Format Ihren Erwartungen entspricht. Beispielsweise haben Sie womöglich Code entwickelt, der auf Warnungen reagiert (Webhooks, Funktionen, Logik-Apps usw.) und ein Format erwartet, aber später in der Aktion haben Sie oder eine andere Person ein anderes Format angegeben.  

    Überprüfen Sie außerdem das Format der Nutzlast (JSON) für [Aktivitätsprotokollwarnungen](../alerts/activity-log-alerts-webhook.md), [Protokollsuchwarnungen](../alerts/alerts-log-webhook.md) (sowohl für Application Insights als auch für die Protokollanalyse), [Metrikwarnungen](alerts-metric-near-real-time.md#payload-schema), [das allgemeine Warnungsschema](../alerts/alerts-common-schema-definitions.md) und die veralteten [klassischen Metrikwarnungen](./alerts-webhooks.md).

 
1. **Aktivitätsprotokollwarnungen: Ist die Information im Aktivitätsprotokoll verfügbar?** 

    [Aktivitätsprotokollwarnungen](./activity-log-alerts.md) sind Warnungen, die auf Ereignissen basieren, die in das Azure-Aktivitätsprotokoll geschrieben werden, z. B. Ereignisse zum Erstellen, Aktualisieren oder Löschen von Azure-Ressourcen, Ereignisse zur Dienst- und Ressourcenintegrität oder Ergebnisse von Azure Advisor und Azure Policy. Wenn Sie eine Warnung basierend auf dem Aktivitätsprotokoll erhalten haben, aber einige der erforderlichen Felder fehlen oder sind fehlerhaft, dann überprüfen Sie die Ereignisse direkt im Aktivitätsprotokoll. Wenn die Azure-Ressource die gesuchten Felder nicht in das Aktivitätsprotokollereignis geschrieben hat, sind diese Felder nicht in der entsprechenden Warnung enthalten. 

## <a name="action-rule-is-not-working-as-expected"></a>Die Aktionsregel funktioniert nicht erwartungsgemäß 

Wenn eine ausgelöste Warnung im Portal angezeigt wird, aber eine dazugehörige Aktionsregel funktioniert nicht erwartungsgemäß, dann führen Sie die folgenden Schritte aus: 

1. **Ist die Aktionsregel aktiviert?** 

    Überprüfen Sie die Statusspalte der Aktionsregel, um sicherzustellen, dass die dazugehörige Aktionsregel aktiviert ist. 

    ![Grafik](media/alerts-troubleshoot/action-rule-status.png) 

    Wenn sie nicht aktiviert ist, können Sie die Aktionsregel aktivieren, indem Sie sie auswählen und auf „Aktivieren“ klicken. 

1. **Handelt es sich um eine Dienstintegritätswarnung?** 

    Dienstintegritätswarnungen (Überwachungsdienst = „Service Health“) sind nicht von Aktionsregeln betroffen. 

1. **Hat die Aktionsregel auf Ihre Warnung reagiert?** 

    Überprüfen Sie, ob die Aktionsregel Ihre Warnung verarbeitet hat, indem Sie im Portal auf die ausgelöste Warnung klicken und die Registerkarte „Verlauf“ untersuchen.

    Im Folgenden finden Sie ein Beispiel für eine Aktionsregel, die alle Aktionsgruppen unterdrückt: 
 
     ![Verlauf mit unterdrückten Aktionsregeln bei einer Warnung](media/alerts-troubleshoot/history-action-rule.png)

    Im Folgenden finden Sie ein Beispiel für eine Aktionsregel, die eine andere Aktionsgruppe hinzufügt:

    ![Aktion wird in mehreren Aktionsgruppen wiederholt](media/alerts-troubleshoot/action-repeated-multi-action-groups.png)
 

1. **Entsprechen der Bereich und Filter der Aktionsregel der ausgelösten Warnung?** 

    Wenn Sie der Meinung sind, die Aktionsregel hätte ausgelöst werden sollen, aber sie wurde nicht ausgelöst, oder die Aktionsregel hätte nicht ausgelöst werden sollen, aber sie wurde ausgelöst, dann untersuchen Sie die Bereichs- und Filterbedingungen der Aktionsregel im Zusammenhang mit den Eigenschaften der ausgelösten Warnung gründlich. 


## <a name="how-to-find-the-alert-id-of-a-fired-alert"></a>Ermitteln der Warnungs-ID einer ausgelösten Warnung

Beim Öffnen eines Falls zu einer spezifischen ausgelösten Warnung (z. B. wenn Sie keine E-Mail-Benachrichtigung erhalten haben) müssen Sie die Warnungs-ID angeben. 

Führen Sie die folgenden Schritte aus, um diese zu ermitteln:

1. Navigieren Sie im Azure-Portal zur Liste der ausgelösten Warnungen, und suchen Sie die spezifische Warnung. Dazu können Sie die Filter verwenden. 

1. Klicken Sie auf die Warnung, um die Warnungsdetails zu öffnen. 

1. Scrollen Sie in den Warnungsfeldern der ersten Registerkarte (Registerkarte „Zusammenfassung“) nach unten, bis Sie die ID finden, und kopieren Sie diese. Das Feld enthält außerdem die Schaltfläche „In Zwischenablage kopieren“, die Sie verwenden können.  

    ![Suchen nach der Warnungs-ID](media/alerts-troubleshoot/get-alert-id.png)

## <a name="problem-creating-updating-or-deleting-action-rules-in-the-azure-portal"></a>Problem beim Erstellen, Aktualisieren oder Löschen von Aktionsregeln im Azure-Portal

Wenn Sie einen Fehler beim Erstellen, Aktualisieren oder Löschen einer [Aktionsregel](../alerts/alerts-action-rules.md) erhalten, führen Sie die folgenden Schritte aus: 

1. **Haben Sie einen Berechtigungsfehler erhalten?**  

    Sie sollten entweder über die  [integrierte Rolle „Mitwirkender an der Überwachung“](../../role-based-access-control/built-in-roles.md#monitoring-contributor) oder die spezifischen Berechtigungen im Bezug zu Aktionsregeln und Warnungen verfügen.

1. **Haben Sie die Aktionsregelparameter überprüft?**  

    Weitere Informationen hierzu finden Sie in der [Dokumentation zu Aktionsregeln](../alerts/alerts-action-rules.md) oder dem Artikel zum Aktionsregelbefehl [PowerShell Set-AzActionRule](/powershell/module/az.alertsmanagement/set-azactionrule). 


## <a name="next-steps"></a>Nächste Schritte
- Weitere Informationen zur Verwendung von Protokollwarnungen finden Sie unter [Behandeln von Problemen bei Protokollwarnungen](./alerts-troubleshoot-log.md).
- Rufen Sie das [Azure-Portal](https://portal.azure.com) wieder auf, um zu überprüfen, ob Sie das Problem mithilfe der oben genannten Anweisungen erfolgreich beheben konnten.