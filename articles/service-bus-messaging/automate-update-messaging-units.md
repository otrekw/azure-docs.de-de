---
title: 'Azure Service Bus: Automatisches Aktualisieren von Messagingeinheiten'
description: In diesem Artikel erfahren Sie, wie Sie Messagingeinheiten eines Service Bus-Namespace automatisch aktualisieren.
ms.topic: how-to
ms.date: 09/15/2020
ms.openlocfilehash: 932c7bb1235cb54aefe67253e38e1683187f4d2c
ms.sourcegitcommit: e559daa1f7115d703bfa1b87da1cf267bf6ae9e8
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/17/2021
ms.locfileid: "100581646"
---
# <a name="automatically-update-messaging-units-of-an-azure-service-bus-namespace"></a>Automatisches Aktualisieren von Messagingeinheiten eines Azure Service Bus-Namespace 
Bei der automatischen Skalierung können Sie jeweils die richtige Menge an Ressourcen ausführen, um die Lasten für Ihre Anwendung zu bewältigen. Sie können Ressourcen hinzufügen, um auf einen Anstieg der Last zu reagieren, und Kosten sparen, indem Sie ungenutzte Ressourcen entfernen. Weitere Informationen zur Funktion „Autoskalierung“ von Azure Monitor finden Sie unter [Übersicht über die Autoskalierung in Microsoft Azure](../azure-monitor/autoscale/autoscale-overview.md). 

Service Bus Premium-Messaging bietet Ressourcenisolierung auf CPU- und Arbeitsspeicherebene, sodass die Workloads der einzelnen Kunden isoliert ausgeführt werden. Dieser Ressourcencontainer wird als **Messaging-Einheit** bezeichnet. Weitere Informationen zu Messagingeinheiten finden Sie unter [Service Bus Premium für Messaging](service-bus-premium-messaging.md). 

Wenn Sie die Autoskalierung für Service Bus Premium-Namespaces verwenden, können Sie eine Mindestanzahl und eine maximale Anzahl von [Messagingeinheiten](service-bus-premium-messaging.md) angeben. Außerdem können Sie Messagingeinheiten basierend auf einer Reihe von Regeln automatisch hinzufügen oder entfernen. 

Mithilfe der Autoskalierung können Sie z. B. die folgenden Skalierungsszenarien für Service Bus-Namespaces implementieren. 

- Erhöhen der Anzahl von Messagingeinheiten für einen Service Bus-Namespace, wenn die CPU-Auslastung des Namespaces 75 % übersteigt. 
- Senken der Anzahl von Messagingeinheiten für einen Service Bus-Namespace, wenn die CPU-Auslastung des Namespaces 25 % unterschreitet. 
- Verwenden einer größeren Anzahl von Messagingeinheiten während der Geschäftszeiten und einer geringeren Anzahl außerhalb der Geschäftszeiten. 

In diesem Artikel erfahren Sie, wie Sie einen Service Bus-Namespace automatisch über das Azure-Portal skalieren (Aktualisieren der [Messagingeinheiten](service-bus-premium-messaging.md)). 

> [!IMPORTANT]
> Dieser Artikel gilt nur für den **Premium-Tarif** von Azure Service Bus. 

## <a name="autoscale-setting-page"></a>Seite mit der Einstellung für die Autoskalierung
Führen Sie zunächst folgende Schritte aus, um zur Seite **Einstellungen für die Autoskalierung** für Ihren Service Bus-Namespace zu wechseln.

1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com)an. 
2. Geben Sie in der Suchleiste **Service Bus** ein, wählen Sie in der Dropdownliste **Service Bus** aus, und drücken Sie die **EINGABETASTE**. 
1. Wählen Sie in der Liste der Namespaces Ihren **Premium-Namespace** aus. 
1. Wechseln Sie zur Seite **Skalieren**. 

    :::image type="content" source="./media/automate-update-messaging-units/scale-page.png" alt-text="Service Bus-Namespace: Seite „Skalieren“":::

## <a name="manual-scale"></a>Manuelles Skalieren 
Über diese Einstellung können Sie eine feste Anzahl von Messagingeinheiten für den Namespace festlegen. 

1. Wählen Sie auf der Seite **Einstellung für die Autoskalierung** die Option **Manuelle Skalierung** aus (sofern noch nicht geschehen). 
1. Wählen Sie für die Einstellung **Messagingeinheiten** die Anzahl von Messagingeinheiten aus der Dropdownliste aus.
1. Wählen Sie auf der Symbolleiste **Speichern** aus, um die Einstellung zu speichern. 

    :::image type="content" source="./media/automate-update-messaging-units/manual-scale.png" alt-text="Manuelles Skalieren von Messagingeinheiten":::       


## <a name="custom-autoscale---default-condition"></a>Benutzerdefinierte Autoskalierung: Standardbedingung
Sie können die Autoskalierung von Messagingeinheiten mithilfe von Bedingungen konfigurieren. Diese Skalierungsbedingung wird ausgeführt, wenn keine der anderen Skalierungsbedingungen zutrifft. Sie können die Standardbedingung über eine der folgenden Methoden festlegen:

- Skalierung basierend auf einer Metrik (z. B. CPU- oder Speicherauslastung)
- Skalierung auf eine bestimmte Anzahl von Messagingeinheiten

Bei Verwendung der Standardbedingung kann kein fester Zeitplan festgelegt werden, um die Autoskalierung an bestimmten Tagen oder innerhalb eines Datumsbereichs durchzuführen. Diese Skalierungsbedingung wird ausgeführt, wenn keine der anderen Skalierungsbedingungen mit Zeitplänen zutrifft. 

### <a name="scale-based-on-a-metric"></a>Skalierung basierend auf einer Metrik
Im Folgenden sind die Schritte gezeigt, um eine Bedingung hinzuzufügen, mit der die Anzahl von Messagingeinheiten automatisch erhöht wird (aufskalieren), wenn die CPU-Auslastung mehr als 75 % beträgt, bzw. mit der die Anzahl von Messagingeinheiten reduziert wird (abskalieren), wenn die CPU-Auslastung bei weniger als 25 % liegt. Beim Erhöhen werden Inkrementierungsschritte von 1 zu 2, 2 zu 4 und 4 zu 8 vorgenommen. Beim Verringern erfolgen die Schritte von 8 zu 4, 4 zu 2 und 2 zu 1. 

1. Wählen Sie auf der Seite **Einstellung für die Autoskalierung** für **Auswählen, wie Ihre Ressource skaliert werden soll** die Option **Benutzerdefinierte Autoskalierung** aus. 
1. Geben Sie im Abschnitt **Standard** der Seite einen **Namen** für die Standardbedingung an. Wählen Sie das **Bleistift**-Symbol aus, um den Text zu bearbeiten. 
1. Wählen Sie für **Skalierungsmodus** die Option **Basierend auf einer Metrik skalieren** aus. 
1. Wählen Sie **+ Regel hinzufügen** aus. 

    :::image type="content" source="./media/automate-update-messaging-units/default-scale-metric-add-rule-link.png" alt-text="Standard: basierend auf einer Metrik skalieren":::    
1. Führen Sie auf der Seite **Skalierungsregel** die folgenden Schritte aus:
    1. Wählen Sie in der Dropdownliste **Metrikname** eine Metrik aus. In diesem Beispiel wird **CPU** verwendet. 
    1. Wählen Sie einen Operator und Schwellenwerte aus. In diesem Beispiel werden für **Metrikschwellenwert zum Auslösen von Skalierungsaktion** die Operatoren **Größer als** und **75** verwendet. 
    1. Wählen Sie im Abschnitt **Aktion** einen **Vorgang** aus. In diesem Beispiel wird **Erhöhen** verwendet. 
    1. Wählen Sie anschließend **Hinzufügen** aus.
    
        :::image type="content" source="./media/automate-update-messaging-units/scale-rule-cpu-75.png" alt-text="Standard: aufskalieren, wenn die CPU-Auslastung bei mehr als 75 % liegt":::       

        > [!NOTE]
        > In diesem Beispiel erhöht die Funktion zur Autoskalierung die Anzahl von Messagingeinheiten für den Namespace, wenn die CPU-Auslastung insgesamt bei über 75 % liegt. Beim Erhöhen werden Inkrementierungsschritte von 1 zu 2, 2 zu 4 und 4 zu 8 vorgenommen. 
1. Wählen Sie erneut **+ Regel hinzufügen** aus, und führen Sie auf der Seite **Skalierungsregel** die folgenden Schritte aus:
    1. Wählen Sie in der Dropdownliste **Metrikname** eine Metrik aus. In diesem Beispiel wird **CPU** verwendet. 
    1. Wählen Sie einen Operator und Schwellenwerte aus. In diesem Beispiel werden für **Metrikschwellenwert zum Auslösen von Skalierungsaktion** die Operatoren **Weniger als** und **25** verwendet. 
    1. Wählen Sie im Abschnitt **Aktion** einen **Vorgang** aus. In diesem Beispiel wird **Verringern** verwendet. 
    1. Wählen Sie anschließend **Hinzufügen** aus. 

        :::image type="content" source="./media/automate-update-messaging-units/scale-rule-cpu-25.png" alt-text="Standard: abskalieren, wenn die CPU-Auslastung bei weniger als 25 % liegt":::       

        > [!NOTE]
        > In diesem Beispiel verringert die Funktion zur Autoskalierung die Anzahl von Messagingeinheiten für den Namespace, wenn die CPU-Auslastung insgesamt bei weniger als 25 % liegt. Beim Verringern erfolgen die Schritte von 8 zu 4, 4 zu 2 und 2 zu 1. 
1. Legen Sie für **Minimum**, **Maximum** und **Standard** die jeweilige Anzahl von Messagingeinheiten fest.

    :::image type="content" source="./media/automate-update-messaging-units/default-scale-metric-based.png" alt-text="Standardregel basierend auf einer Metrik":::
1. Wählen Sie auf der Symbolleiste **Speichern** aus, um die Einstellung für die Autoskalierung zu speichern. 
        
### <a name="scale-to-specific-number-of-messaging-units"></a>Skalierung auf eine bestimmte Anzahl von Messagingeinheiten
Führen Sie die folgenden Schritte aus, um die Regel zur Skalierung des Namespace so zu konfigurieren, dass eine bestimmte Anzahl von Messagingeinheiten verwendet wird. Auch hier wird die Standardbedingung angewendet, wenn keine der anderen Skalierungsbedingungen zutrifft. 

1. Wählen Sie auf der Seite **Einstellung für die Autoskalierung** für **Auswählen, wie Ihre Ressource skaliert werden soll** die Option **Benutzerdefinierte Autoskalierung** aus. 
1. Geben Sie im Abschnitt **Standard** der Seite einen **Namen** für die Standardbedingung an. 
1. Wählen Sie für **Skalierungsmodus** die Option **Auf bestimmte Anzahl von Messagingeinheiten skalieren** aus. 
1. Wählen Sie für **Messagingeinheiten** die Anzahl von Standardmessagingeinheiten aus. 

    :::image type="content" source="./media/automate-update-messaging-units/default-scale-messaging-units.png" alt-text="Standard: auf bestimmte Anzahl von Messagingeinheiten skalieren":::       

## <a name="custom-autoscale---additional-conditions"></a>Benutzerdefinierte Autoskalierung: zusätzliche Bedingungen
Im vorherigen Abschnitt haben Sie erfahren, wie Sie eine Standardbedingung für die Einstellung für die Autoskalierung hinzufügen. In diesem Abschnitt wird gezeigt, wie Sie weitere Bedingungen zur Einstellung für die Autoskalierung hinzufügen. Für diese zusätzlichen Nicht-Standardbedingungen kann ein Zeitplan festgelegt werden, der auf bestimmten Wochentagen oder einem Datumsbereich basiert. 

### <a name="scale-based-on-a-metric"></a>Skalierung basierend auf einer Metrik
1. Wählen Sie auf der Seite **Einstellung für die Autoskalierung** für **Auswählen, wie Ihre Ressource skaliert werden soll** die Option **Benutzerdefinierte Autoskalierung** aus. 
1. Wählen Sie im Block **Standard** die Option **Skalierungsbedingung hinzufügen** aus. 

    :::image type="content" source="./media/automate-update-messaging-units/add-scale-condition-link.png" alt-text="Benutzerdefiniert: Link zum Hinzufügen einer Skalierungsbedingung":::    
1. Geben Sie einen **Namen** für die Bedingung an. 
1. Überprüfen Sie, ob die Option **Basierend auf einer Metrik skalieren** ausgewählt ist. 
1. Wählen Sie **+ Regel hinzufügen** aus, um die Anzahl von Messagingeinheiten zu erhöhen, wenn die CPU-Auslastung insgesamt bei mehr als 75 % liegt. Führen Sie die im Abschnitt [Standardbedingung](#custom-autoscale---default-condition) beschriebenen Schritte aus. 
5. Legen Sie für **Minimum**, **Maximum** und **Standard** die jeweilige Anzahl von Messagingeinheiten fest.
6. Im Gegensatz zur Standardbedingung kann bei benutzerdefinierten Bedingungen zudem ein **Zeitplan** festgelegt werden. Dabei können Sie entweder ein Start- und ein Enddatum für die Bedingung oder bestimmte Wochentage (Montag, Dienstag usw.) angeben. 
    1. Wenn Sie **Start-/Enddatum angeben** auswählen, treffen Sie für die zu verwendende Bedingung eine Auswahl für **Zeitzone**, **Startdatum und -uhrzeit** sowie **Enddatum und -uhrzeit** (wie in der folgenden Abbildung gezeigt). 

       :::image type="content" source="./media/automate-update-messaging-units/custom-min-max-default.png" alt-text="Mindest-, Höchst- und Standardwerte für die Anzahl von Messagingeinheiten":::
    1. Bei Auswahl von **An bestimmten Tagen wiederholen** wählen Sie die Wochentage, die Zeitzone, die Startzeit und die Endzeit für die Bedingung aus. 

        :::image type="content" source="./media/automate-update-messaging-units/repeat-specific-days.png" alt-text="An bestimmten Tagen wiederholen":::
  
### <a name="scale-to-specific-number-of-messaging-units"></a>Skalierung auf eine bestimmte Anzahl von Messagingeinheiten
1. Wählen Sie auf der Seite **Einstellung für die Autoskalierung** für **Auswählen, wie Ihre Ressource skaliert werden soll** die Option **Benutzerdefinierte Autoskalierung** aus. 
1. Wählen Sie im Block **Standard** die Option **Skalierungsbedingung hinzufügen** aus. 

    :::image type="content" source="./media/automate-update-messaging-units/add-scale-condition-link.png" alt-text="Benutzerdefiniert: Link zum Hinzufügen einer Skalierungsbedingung":::    
1. Geben Sie einen **Namen** für die Bedingung an. 
2. Wählen Sie für **Skalierungsmodus** die Option **Auf bestimmte Anzahl von Messagingeinheiten skalieren** aus. 
1. Wählen Sie die Anzahl von **Messagingeinheiten** aus der Dropdownliste aus. 
6. Geben Sie für **Zeitplan** entweder ein Start- und ein Enddatum für die Bedingung oder bestimmte Wochentage (Montag, Dienstag usw.) an. 
    1. Wenn Sie **Start-/Enddatum angeben** auswählen, treffen Sie für die zu verwendende Bedingung eine Auswahl für **Zeitzone**, **Startdatum und -uhrzeit** sowie **Enddatum und -uhrzeit**. 
    
    :::image type="content" source="./media/automate-update-messaging-units/scale-specific-messaging-units-start-end-dates.png" alt-text="Auf bestimmte Anzahl von Messagingeinheiten skalieren: Start- und Enddatum":::        
    1. Bei Auswahl von **An bestimmten Tagen wiederholen** wählen Sie die Wochentage, die Zeitzone, die Startzeit und die Endzeit für die Bedingung aus.
    
    :::image type="content" source="./media/automate-update-messaging-units/repeat-specific-days-2.png" alt-text="Auf bestimmte Anzahl von Messagingeinheiten skalieren: an bestimmten Tagen wiederholen":::

> [!IMPORTANT]
> Weitere Informationen zur Funktionsweise der Einstellungen für die Autoskalierung – insbesondere dazu, wie ein Profil oder eine Bedingung ausgewählt und mehrere Regeln ausgewertet werden – finden Sie unter [Grundlegendes zu Einstellungen für die automatische Skalierung](../azure-monitor/autoscale/autoscale-understanding-settings.md).          

## <a name="next-steps"></a>Nächste Schritte
Weitere Informationen zu Messagingeinheiten finden Sie unter [Service Bus Premium- und Standard-Preisstufe für Messaging](service-bus-premium-messaging.md).

