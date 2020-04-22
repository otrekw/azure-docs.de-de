---
title: Verwalten von Richtlinien zum automatischen Herunterfahren in Azure DevTest Labs | Microsoft-Dokumentation
description: Erfahren Sie, wie Sie eine Richtlinie zum automatischen Herunterfahren für ein Lab festlegen, sodass virtuelle Computer automatisch heruntergefahren werden, wenn sie nicht verwendet werden.
services: devtest-lab,virtual-machines,lab-services
documentationcenter: na
author: spelluru
manager: femila
editor: ''
ms.assetid: ''
ms.service: lab-services
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/10/2020
ms.author: spelluru
ms.openlocfilehash: 7cdc9f9a4503c786065b6d514f61fe17eae4ce5e
ms.sourcegitcommit: 530e2d56fc3b91c520d3714a7fe4e8e0b75480c8
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/14/2020
ms.locfileid: "81270909"
---
# <a name="configure-autoshutdown-for-lab-and-compute-virtual-machines-in-azure-devtest-labs"></a>Konfigurieren des automatischen Herunterfahrens für Lab- und Compute-VMs in Azure DevTest Labs

In diesem Artikel wird beschrieben, wie Einstellungen für automatisches Herunterfahren für Lab-VMs in DevTest Labs und Compute-VMs konfiguriert werden. 

## <a name="configure-autoshutdown-for-lab-vms-devtest-labs"></a>Konfigurieren von automatischem Herunterfahren für Lab-VMs (DevTest Labs)
Mit Azure DevTest Labs können Sie Kosten und unnötigen Aufwand in Ihren Labs minimieren, indem Sie Richtlinien (Einstellungen) für jedes Lab verwalten. In diesem Artikel wird das Konfigurieren einer Richtlinie zum automatischen Herunterfahren für ein Labkonto und der Einstellungen für das automatische Herunterfahren für ein Lab im Labkonto veranschaulicht. Informationen zum Einrichten der einzelnen Labrichtlinien finden Sie unter [Definieren von Labrichtlinien in Azure DevTest Labs](devtest-lab-set-lab-policy.md).  

### <a name="set-auto-shutdown-policy-for-a-lab"></a>Festlegen einer Richtlinie zum automatischen Herunterfahren für ein Lab
Als Labbesitzer können Sie einen Zeitplan für das Herunterfahren für alle virtuellen Computer in Ihrem Lab konfigurieren. Auf diese Weise können Sie Kosten für aktive Computer sparen, die nicht verwendet werden (Leerlauf). Sie können eine Richtlinie für das Herunterfahren für alle virtuellen Computer Ihres Labs zentral erzwingen und Ihren Lab-Benutzern die Arbeit abnehmen, einen Zeitplan für ihre Computer festzulegen. Mit diesem Feature können Sie die Richtlinie für den Labzeitplan festlegen und den Lab-Benutzern keine, eine teilweise oder die volle Kontrolle anbieten. Als Labbesitzer können Sie diese Richtlinie mit den folgenden Schritten konfigurieren:

1. Wählen Sie auf der Startseite Ihres Labs **Konfiguration und Richtlinien** aus.
2. Wählen Sie im linken Menü im Abschnitt **Zeitpläne** die Option **Richtlinien zum automatischen Herunterfahren** aus.
3. Wählen Sie eine der Optionen aus. Die folgenden Abschnitte enthalten weitere Informationen zu diesen Optionen: Die festgelegte Richtlinie gilt nur für neu im Lab erstellte virtuelle Computer und nicht für bereits vorhandene virtuelle Computer. 

    ![Optionen für die Richtlinie zum automatischen Herunterfahren](./media/devtest-lab-set-lab-policy/auto-shutdown-policy-options.png)

### <a name="configure-auto-shutdown-settings"></a>Konfigurieren der Einstellungen für das automatische Herunterfahren
Die Richtlinie zum automatischen Herunterfahren hilft dabei, unnötigen Aufwand im Lab zu minimieren, indem sie Ihnen ermöglicht, die Uhrzeit anzugeben, zu der die virtuellen Computer für dieses Lab heruntergefahren werden.

Um die Richtlinien für ein Lab anzuzeigen (und zu ändern), gehen Sie folgendermaßen vor:

1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com) an.
2. Wählen Sie **Alle Dienste** und dann in der Liste die Option **DevTest Labs**.
3. Wählen Sie in der Liste der Labs das gewünschte Lab aus.   
4. Wählen Sie **Konfiguration und Richtlinien** aus.

    ![Bereich „Richtlinieneinstellungen“](./media/devtest-lab-set-lab-policy/policies-menu.png)
5. Wählen Sie im Bereich **Konfiguration und Richtlinien** des Labs unter **Zeitpläne** die Option **Automatisch herunterfahren** aus.
   
    ![Automatisches Herunterfahren](./media/devtest-lab-set-lab-policy/auto-shutdown.png)
6. Wählen Sie **Ein**, um diese Richtlinie zu aktivieren, und **Aus**, um sie zu deaktivieren.
7. Wenn Sie diese Richtlinie aktivieren, geben Sie die Uhrzeit (und die Zeitzone) an, zu der alle virtuellen Computer im aktuellen Lab heruntergefahren werden sollen.
8. Geben Sie für die Option, mit der 30 Minuten vor der angegebenen Uhrzeit des automatischen Herunterfahrens eine Benachrichtigung gesendet wird, entweder **Ja** oder **Nein** an. Wenn Sie **Ja** auswählen, geben Sie einen Webhook-URL-Endpunkt oder eine E-Mail-Adresse ein, der bzw. die angibt, wo die Benachrichtigung veröffentlicht bzw. an wen diese gesendet wird. Der Benutzer erhält eine Benachrichtigung und hat die Möglichkeit, das Herunterfahren zu verzögern. Weitere Informationen finden Sie im Abschnitt [Benachrichtigungen](#notifications). 
9. Wählen Sie **Speichern** aus.

    Standardmäßig gilt diese Richtlinie nach der Aktivierung für alle virtuellen Computer im aktuellen Lab. Um diese Einstellung von einem bestimmten virtuellen Computer zu entfernen, öffnen Sie den Verwaltungsbereich des virtuellen Computers, und ändern Sie die Einstellung **Automatisch herunterfahren**.
    
> [!NOTE]
> Wenn Sie den Zeitplan für das automatische Herunterfahren Ihres Labs oder eines bestimmten virtuellen Lab-Computers innerhalb von 30 Minuten des aktuellen Zeitplans aktualisieren, wird die aktualisierte Zeit für das Herunterfahren auf den Zeitplan des nächsten Tags angewendet. 

### <a name="user-sets-a-schedule-and-can-opt-out"></a>Benutzer legt Zeitplan fest und kann diesen deaktivieren
Wenn Sie für Ihr Lab diese Richtlinie festlegen, können die Lab-Benutzer den Labzeitplan außer Kraft setzen oder deaktivieren. Diese Option gewährt Lab-Benutzern die vollständige Kontrolle über den Zeitplan für das automatische Herunterfahren ihrer virtuellen Computer. Lab-Benutzer sehen keine Änderungen auf der Seite mit dem Zeitplan zum automatischen Herunterfahren ihrer virtuellen Computer.

![Optionen für die Richtlinie zum automatischen Herunterfahren – 1](./media/devtest-lab-set-lab-policy/auto-shutdown-policy-option-1.png)

### <a name="user-sets-a-schedule-and-cannot-opt-out"></a>Benutzer legt Zeitplan fest und kann diesen nicht deaktivieren
Wenn Sie für Ihr Lab diese Richtlinie festlegen, können die Lab-Benutzer den Labzeitplan außer Kraft setzen. Sie können die Richtlinie zum automatischen Herunterfahren allerdings nicht deaktivieren. Diese Option stellt sicher, dass es für jeden Computer in Ihrem Lab einen Zeitplan für das automatische Herunterfahren gibt. Lab-Benutzer können den Zeitplan für das automatische Herunterfahren ihrer virtuellen Computer aktualisieren und Benachrichtigungen über das Herunterfahren einrichten.

![Optionen für die Richtlinie zum automatischen Herunterfahren – 2](./media/devtest-lab-set-lab-policy/auto-shutdown-policy-option-2.png)

### <a name="user-has-no-control-over-the-schedule-set-by-lab-admin"></a>Benutzer kann den vom Labadministrator festgelegten Zeitplan nicht steuern
Wenn Sie für Ihr Lab diese Richtlinie festlegen, können die Lab-Benutzer den Labzeitplan nicht außer Kraft setzen oder deaktivieren. Diese Option bietet dem Labadministrator die vollständige Kontrolle über den Zeitplan für jeden Computer im Lab. Lab-Benutzer können nur Benachrichtigungen zum automatischen Herunterfahren für ihre virtuellen Computer einrichten.

![Optionen für die Richtlinie zum automatischen Herunterfahren – 3](./media/devtest-lab-set-lab-policy/auto-shutdown-policy-option-3.png)

### <a name="notifications"></a>Benachrichtigungen
Nachdem das automatische Herunterfahren eingerichtet wurde, werden 30 Minuten vor dem automatischen Herunterfahren Benachrichtigungen an die Lab-Benutzer gesendet, sofern deren virtuelle Computer betroffen sind. Mit dieser Option erhalten die Lab-Benutzer die Möglichkeit, ihre Arbeit vor dem Herunterfahren zu speichern. Die Benachrichtigung enthält auch Links für jeden virtuellen Computer für die folgenden Aktionen:

- Überspringen des automatischen Herunterfahrens für dieses Mal
- Das automatische Herunterfahren wird um eine oder zwei Stunden verschoben, damit sie auf dem virtuellen Computer weiterarbeiten können.

Die Benachrichtigung wird über den konfigurierten Webhook-Endpunkt oder eine E-Mail-Adresse gesendet, die von den Labbesitzern in den Einstellungen für das automatische Herunterfahren angegeben wurde. Webhooks ermöglichen Ihnen das Erstellen oder Einrichten von Integrationen, die bestimmte Ereignisse abonnieren. Wenn ein solches Ereignis ausgelöst wird, sendet DevTest Labs eine HTTP POST-Nutzlast an die für den Webhook konfigurierte URL. Weitere Informationen zu Webhooks finden Sie unter [Erstellen eines Webhooks oder einer API-Azure-Funktion](../azure-functions/functions-create-a-web-hook-or-api-function.md). 

Es wird empfohlen, Webhooks zu verwenden, da sie von verschiedenen Apps (z. B. Slack, Azure Logic Apps) umfassend unterstützt werden und Ihnen ermöglichen, eigene Methoden zum Senden von Benachrichtigungen zu implementieren. Als Beispiel wird in diesem Artikel das Senden von Benachrichtigungen zum automatischen Herunterfahren mithilfe von Azure Logic Apps beschrieben. Sehen Sie sich zunächst kurz die grundlegenden Schritte zum Aktivieren von Benachrichtigungen zum automatischen Herunterfahren in Ihrem Lab an.   

### <a name="create-a-logic-app-that-receives-email-notifications"></a>Erstellen einer Logik-App, die E-Mail-Benachrichtigungen empfängt
[Azure Logic Apps](../logic-apps/logic-apps-overview.md) bietet viele vorkonfigurierte Connectors, die es einfach machen, einen Dienst für andere Clients wie Office 365 und Twitter zu integrieren. Ganz allgemein können die Schritte zum Einrichten einer Logik-App für E-Mail-Benachrichtigungen in vier Phasen unterteilt werden: 

- Erstellen Sie eine Logik-App. 
- Konfigurieren der integrierten Vorlage
- Integrieren in den E-Mail-Client
- Abrufen der Webhook-URL

### <a name="create-a-logic-app"></a>Erstellen einer Logik-App
Erstellen Sie zunächst mithilfe der folgenden Schritte eine Logik-App in Ihrem Azure-Abonnement:

1. Wählen Sie im Menü links **+ Ressource erstellen**, dann **Integration** und schließlich **Logik-App** aus. 

    ![Menü „Neue Logik-App“](./media/devtest-lab-auto-shutdown/new-logic-app.png)
2. Führen Sie auf der Seite **Logik-App erstellen** die folgenden Schritte aus: 
    1. Geben Sie einen **Namen** für die Logik-App ein.
    2. Wählen Sie Ihr Azure- **Abonnement**aus.
    3. Erstellen Sie eine neue **Ressourcengruppe**, oder wählen Sie eine vorhandene Ressourcengruppe aus. 
    4. Wählen Sie einen **Speicherort** für die Logik-App aus. 

        ![Neue Logik-App – Einstellungen](./media/devtest-lab-auto-shutdown/new-logic-app-page.png)
3. Wählen Sie in **Benachrichtigungen** die Option **Zu Ressource wechseln** für die Benachrichtigung aus. 

    ![Zu Ressource wechseln](./media/devtest-lab-auto-shutdown/go-to-resource.png)
4. Wählen Sie **Logik-App-Designer** unter der Kategorie **Bereitstellungstools** aus.

    ![Auswählen der HTTP-Anforderung/-Antwort](./media/devtest-lab-auto-shutdown/select-http-request-response-option.png)
5. Wählen Sie auf der Seite **HTTP-Anforderung/-Antwort** die Option **Diese Vorlage verwenden** aus. 

    ![Auswählen der Option „Diese Vorlage verwenden“](./media/devtest-lab-auto-shutdown/select-use-this-template.png)
6. Kopieren Sie den folgenden JSON-Code in den Abschnitt **JSON-Schema für Anforderungstext**: 

    ```json
    {
        "$schema": "http://json-schema.org/draft-04/schema#",
        "properties": {
            "delayUrl120": {
                "type": "string"
            },
            "delayUrl60": {
                "type": "string"
            },
            "eventType": {
                "type": "string"
            },
            "guid": {
                "type": "string"
            },
            "labName": {
                "type": "string"
            },
            "owner": {
                "type": "string"
            },
            "resourceGroupName": {
                "type": "string"
            },
            "skipUrl": {
                "type": "string"
            },
            "subscriptionId": {
                "type": "string"
            },
            "text": {
                "type": "string"
            },
            "vmName": {
                "type": "string"
            }
        },
        "required": [
            "skipUrl",
            "delayUrl60",
            "delayUrl120",
            "vmName",
            "guid",
            "owner",
            "eventType",
            "text",
            "subscriptionId",
            "resourceGroupName",
            "labName"
        ],
        "type": "object"
    }
    ```
    
    ![JSON-Schema für Anforderungstext](./media/devtest-lab-auto-shutdown/request-json.png)
7. Wählen Sie im Designer **+ Neuer Schritt** aus, und führen Sie die folgenden Schritte aus:
    1. Suchen Sie nach **Office 365 Outlook – E-Mail senden**. 
    2. Wählen Sie unter **Aktionen** die Option **E-Mail senden** aus. 
    
        ![Option „E-Mail senden“](./media/devtest-lab-auto-shutdown/select-send-email.png)
    3. Wählen Sie **Anmelden** aus, um sich bei Ihrem E-Mail-Konto anzumelden. 
    4. Wählen Sie das Feld **An** und dann den Besitzer aus.
    5. Wählen Sie **Betreff** aus, und geben Sie einen Betreff für die E-Mail-Benachrichtigung ein. Beispiel: „Herunterfahren der VM vmName für das Lab: labName“
    6. Wählen Sie **Text** aus, und geben Sie den Inhalt der E-Mail-Benachrichtigung ein. Beispiel: „Das Herunterfahren der VM ‚vmName‘ erfolgt in 15 Minuten. Klicken Sie zum Überspringen dieses Vorgangs auf: URL. Herunterfahren um eine Stunde verschieben: delayUrl60. Herunterfahren um zwei Stunden verschieben: delayUrl120.“

        ![JSON-Schema für Anforderungstext](./media/devtest-lab-auto-shutdown/email-options.png)
1. Wählen Sie auf der Symbolleiste **Speichern** aus. Sie können nun die **HTTP POST-URL** kopieren. Wählen Sie die Schaltfläche „Kopieren“ aus, um die URL in die Zwischenablage zu kopieren. 

    ![Webhook-URL](./media/devtest-lab-auto-shutdown/webhook-url.png)

## <a name="configure-autoshutdown-for-compute-vms"></a>Konfigurieren von automatischem Herunterfahren für Compute-VMs

1. Wählen Sie auf der Seite **Virtueller Computer** im linken Menü im Abschnitt **Vorgänge** die Option **Automatisches Herunterfahren** aus. 
2. Wählen Sie auf der Seite **Automatisches Herunterfahren** die Option **Ein** aus, um diese Richtlinie zu aktivieren, und **Aus**, um sie zu deaktivieren.
3. Wenn Sie diese Richtlinie aktivieren, geben Sie die **Uhrzeit** (und die **Zeitzone**) an, zu der die VM heruntergefahren werden soll.
4. Geben Sie für die Option, mit der 30 Minuten vor der angegebenen Uhrzeit des automatischen Herunterfahrens eine Benachrichtigung gesendet wird, entweder **Ja** oder **Nein** an. Wenn Sie **Ja** auswählen, geben Sie einen Webhook-URL-Endpunkt oder eine E-Mail-Adresse ein, der bzw. die angibt, wo die Benachrichtigung veröffentlicht bzw. an wen diese gesendet wird. Der Benutzer erhält eine Benachrichtigung und hat die Möglichkeit, das Herunterfahren zu verzögern. Weitere Informationen finden Sie im Abschnitt [Benachrichtigungen](#notifications). 
9. Wählen Sie **Speichern** aus.

    ![Konfigurieren von automatischem Herunterfahren für eine Compute-VM](./media/devtest-lab-auto-shutdown/comnpute-auto-shutdown.png)

### <a name="view-activity-logs-for-auto-shutdown-updates"></a>Anzeigen von Aktivitätsprotokollen für Aktualisierungen der Einstellung zum automatischen Herunterfahren
Wenn Sie die Einstellung zum automatischen Herunterfahren aktualisieren, wird die Aktivität im Aktivitätsprotokoll der VM aufgezeichnet. 

1. Navigieren Sie im [Azure-Portal](https://portal.azure.com) zur Startseite für Ihre VM.
2. Wählen Sie im Menü links **Aktivitätsprotokoll** aus. 
3. Entfernen Sie **Ressource: mycomputevm** aus den Filtern.
3. Bestätigen Sie, dass der Vorgang **Zeitpläne hinzufügen oder ändern** im Aktivitätsprotokoll angezeigt wird. Wenn Sie diesen Vorgang nicht sehen, warten Sie einen Moment, und aktualisieren Sie das Aktivitätsprotokoll

    ![Aktivitätsprotokolleintrag](./media/devtest-lab-auto-shutdown/activity-log-entry.png)
4. Wählen Sie den Vorgang **Zeitpläne hinzufügen oder ändern** aus, um die folgenden Informationen auf der Seite **Zusammenfassung** anzuzeigen:

    - Name des Vorgangs (Zeitpläne hinzufügen oder ändern)
    - Datum und Uhrzeit, zu der die Einstellung zum automatischen Herunterfahren geändert wurde
    - Die E-Mail-Adresse des Benutzers, der die Einstellung aktualisiert hat 

        ![Zusammenfassung zum Aktivitätsprotokolleintrag](./media/devtest-lab-auto-shutdown/activity-log-entry-summary.png)
5. Wechseln Sie auf der Seite **Zeitpläne hinzufügen oder ändern** zur Registerkarte **Änderungsverlauf**, um den Änderungsverlauf für die Einstellung anzuzeigen. Im folgenden Beispiel wurde die Zeit für das Herunterfahren am 10. April 2020 um 15:18:47 EST von 19 Uhr auf 18 Uhr geändert. Und um 15:25:09 EST wurde die Einstellung deaktiviert. 

    ![Aktivitätsprotokoll – Änderungsverlauf](./media/devtest-lab-auto-shutdown/activity-log-entry-change-history.png)
6. Um weitere Details zum Vorgang anzuzeigen, wechseln Sie auf der Seite **Zeitpläne hinzufügen oder ändern** zur Registerkarte **JSON**.

## <a name="next-steps"></a>Nächste Schritte
Informationen zum Festlegen aller Richtlinien finden Sie unter [Definieren von Labrichtlinien in Azure DevTest Labs](devtest-lab-set-lab-policy.md).

