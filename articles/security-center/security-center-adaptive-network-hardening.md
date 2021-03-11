---
title: Adaptive Netzwerkhärtung in Azure Security Center | Microsoft-Dokumentation
description: Erfahren Sie, wie Sie anhand echter Datenverkehrsmuster Ihre NSG-Regeln (Netzwerksicherheitsgruppen) härten und den Sicherheitsstatus weiter verbessern.
author: memildin
manager: rkarlin
ms.service: security-center
ms.topic: how-to
ms.date: 03/11/2020
ms.author: memildin
ms.openlocfilehash: 14523e814c85469aa02a860e87b86defd7823c16
ms.sourcegitcommit: 5bbc00673bd5b86b1ab2b7a31a4b4b066087e8ed
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/07/2021
ms.locfileid: "102439594"
---
# <a name="improve-your-network-security-posture-with-adaptive-network-hardening"></a>Verbessern des Netzwerksicherheitsstatus mit adaptiver Netzwerkhärtung
Hier erfahren Sie, wie Sie die adaptive Netzwerkhärtung in Security Center konfigurieren.

## <a name="availability"></a>Verfügbarkeit
|Aspekt|Details|
|----|:----|
|Status des Release:|Allgemeine Verfügbarkeit (General Availability, GA)|
|Preise:|Erfordert [Azure Defender für Server](defender-for-servers-introduction.md)|
|Erforderliche Rollen und Berechtigungen:|Schreibberechtigungen für die NSGs des Computers|
|Clouds:|![Ja](./media/icons/yes-icon.png) Kommerzielle Clouds<br>![Nein](./media/icons/no-icon.png) National/Sovereign (US Gov, China Gov, andere Gov-Clouds)|
|||

## <a name="what-is-adaptive-network-hardening"></a>Was ist die adaptive Netzwerkhärtung?
Der Einsatz von [Netzwerksicherheitsgruppen (NSGs)](../virtual-network/network-security-groups-overview.md) zur Filterung von ein- und ausgehendem Datenverkehr für Ressourcen verbessert den Sicherheitsstatus Ihres Netzwerks. Es gibt jedoch Situationen, in denen es sich bei dem Datenverkehr, der die NSG durchläuft, um eine Teilmenge der definierten NSG-Regeln handelt. In diesen Fällen lässt sich der Sicherheitsstatus durch eine Härtung der NSG-Regeln auf der Grundlage tatsächlicher Datenverkehrsmuster noch weiter verbessern.

Die adaptive Netzwerkhärtung liefert Empfehlungen zur weiteren Härtung der NSG-Regeln. Dabei kommt ein Machine Learning-Algorithmus zum Einsatz, der Faktoren wie tatsächlichen Datenverkehr, bekannte vertrauenswürdige Konfiguration und Bedrohungsinformationen sowie weitere Anzeichen einer Kompromittierung berücksichtigt und auf der Grundlage dieser Faktoren Empfehlungen abgibt, um nur Datenverkehr von bestimmten IP-/Port-Tupeln zuzulassen.

Ein Beispiel: Angenommen, die vorhandene NSG-Regel lässt Datenverkehr von 140.20.30.10/24 an Port 22 zu. Basierend auf der Analyse des Datenverkehrs empfiehlt die Netzwerkhärtung möglicherweise eine Einschränkung des Bereichs, um den Datenverkehr von 140.23.30.10/29 zuzulassen und sämtlichen weiteren Datenverkehr an diesem Port zu verweigern.

>[!Note]
> Empfehlungen der adaptiven Netzwerkhärtung werden nur an den folgenden Ports unterstützt (sowohl für UDP als auch für TCP): 13, 17, 19, 22, 23, 53, 69, 81, 111, 119, 123, 135, 137, 138, 139, 161, 162, 389, 445, 512, 514, 593, 636, 873, 1433, 1434, 1900, 2049, 2301, 2323, 2381, 3268, 3306, 3389, 4333, 5353, 5432, 5555, 5800, 5900, 5900, 5985, 5986, 6379, 6379, 7000, 7001, 7199, 8081, 8089, 8545, 9042, 9160, 9300, 11211, 16379, 26379, 27017, 37215


## <a name="view-and-manage-hardening-alerts-and-rules"></a>Anzeigen und Verwalten von Härtungswarnungen und -regeln

1. Öffnen Sie im Menü von Security Center das Dashboard **Azure Defender**, und wählen Sie die Kachel für die adaptive Netzwerkhärtung (1) oder das Insights-Panelelement für die adaptive Netzwerkhärtung (2) aus. 

    :::image type="content" source="./media/security-center-adaptive-network-hardening/traffic-hardening.png" alt-text="Zugriff auf die Tools für die adaptive Netzwerkhärtung" lightbox="./media/security-center-adaptive-network-hardening/traffic-hardening.png":::

    > [!TIP]
    > Das Insights-Panel zeigt den Prozentsatz Ihrer VMs, die derzeit mit adaptiver Netzwerkhärtung geschützt sind. 

1. Die Detailseite für die Empfehlung **Auf VMs mit Internetausrichtung müssen Empfehlungen zur adaptiven Netzwerkhärtung angewendet werden** wird geöffnet. Ihre Netzwerk-VMs sind auf drei Registerkarten gruppiert:
   * **Fehlerhafte Ressourcen:** Virtuelle Computer, für die Empfehlungen und Warnungen vorliegen, die durch Ausführen des Algorithmus der adaptiven Netzwerkhärtung ausgelöst wurden. 
   * **Fehlerfreie Ressourcen:** Virtuelle Computer ohne Warnungen oder Empfehlungen.
   * **Nicht überprüfte Ressourcen:** Virtuelle Computer, auf denen der Algorithmus der adaptiven Netzwerkhärtung aus einem der folgenden Gründe nicht ausgeführt werden kann:
      * **Virtuelle Computer sind klassische virtuelle Computer**: Es werden nur virtuelle Azure Resource Manager-Computer unterstützt.
      * **Nicht genügend Daten verfügbar:** Security Center benötigt mindestens Datenverkehrsdaten eines Zeitraums von 30 Tagen, um fundierte Härtungsempfehlungen abgeben zu können.
      * **Virtueller Computer nicht durch Azure Defender geschützt:** Nur virtuelle Computer, die mit [Azure Defender für Server](defender-for-servers-introduction.md) geschützt sind, können dieses Feature nutzen.

    :::image type="content" source="./media/security-center-adaptive-network-hardening/recommendation-details-page.png" alt-text="Detailseite der Empfehlung „Auf VMs mit Internetausrichtung müssen Empfehlungen zur adaptiven Netzwerkhärtung angewendet werden“":::

1. Wählen Sie auf der Registerkarte **Fehlerhafte Ressourcen** einen virtuellen Computer aus, um dessen Warnungen und die empfohlenen Härtungsregeln anzuzeigen.

    - Auf der Registerkarte **Regeln** werden die Regeln aufgeführt, die laut adaptiver Netzwerkhärtung hinzugefügt werden sollten.
    - Auf der Registerkarte **Warnungen** werden die Warnungen aufgeführt, die aufgrund von eingehendem Datenverkehr für die Ressource generiert wurden, der nicht innerhalb des durch die empfohlenen Regeln zugelassenen IP-Adressbereichs liegt.

1. Optional können Sie die Regeln bearbeiten:

    - [Ändern einer Regel](#modify-rule)
    - [Löschen einer Regel](#delete-rule) 
    - [Regel hinzufügen](#add-rule)

3. Wählen Sie die Regeln aus, die Sie auf die NSG anwenden möchten, und klicken Sie auf **Erzwingen**.

    > [!TIP]
    > Wenn die zulässigen Quell-IP-Adressbereiche als „Keine“ angezeigt werden, bedeutet dies, dass die empfohlene Regel eine *Ablehnungsregel* ist. Andernfalls handelt es sich um eine *Zulassungsregel*.

    :::image type="content" source="./media/security-center-adaptive-network-hardening/hardening-alerts.png" alt-text="Verwalten der Regeln der adaptiven Netzwerkhärtung":::

      > [!NOTE]
      > Die erzwungenen Regeln werden den Netzwerksicherheitsgruppen hinzugefügt, die den virtuellen Computer schützen. (Ein virtueller Computer kann durch eine Netzwerksicherheitsgruppe geschützt werden, die der Schnittstellenkarte zugeordnet ist, oder das Subnetz, in dem der virtuelle Computer befindet, oder beides.)

### <a name="modify-a-rule"></a>Ändern einer Regel <a name ="modify-rule"> </a>

Die Parameter einer empfohlenen Regel können bei Bedarf geändert werden. So können Sie beispielsweise die empfohlenen IP-Adressbereiche ändern.

Im Folgenden finden Sie einige wichtige Richtlinien, die Sie beim Ändern einer Regel der adaptiven Netzwerkhärtung berücksichtigen sollten:

* Es können nur die Parameter von Regeln des Typs „Zulassen“ geändert werden. 
* Regeln können nicht von „Zulassen“ in „Verweigern“ geändert werden. 

  > [!NOTE]
  > Das Erstellen und Ändern von Verweigerungsregeln erfolgt direkt in der NSG. Weitere Informationen finden Sie unter [Erstellen, Ändern oder Löschen einer Netzwerksicherheitsgruppe](../virtual-network/manage-network-security-group.md).

* Die einzige Regel vom Typ „Verweigern“, die hier ggf. angezeigt wird, ist eine Regel zum **Verweigern des gesamten Datenverkehrs**, und diese Regel kann nicht geändert werden. Sie kann allerdings gelöscht werden (siehe [Löschen einer Regel](#delete-rule)).
  > [!NOTE]
  > Eine Regel zum **Verweigern des gesamten Datenverkehrs** wird empfohlen, wenn Security Center nach Ausführung des Algorithmus keinen Datenverkehr erkennt, der zugelassen werden soll (basierend auf der vorhandenen NSG-Konfiguration). Aus diesem Grund ist die empfohlene Regel, jeglichen Datenverkehr für den angegebenen Port zu verweigern. Der Name dieser Art von Regel wird als *Vom System generiert* angezeigt. Nach Erzwingung dieser Regel ist ihr tatsächlicher Name in der NSG eine Zeichenfolge, die sich aus dem Protokoll, der Datenverkehrsrichtung, „DENY“ und einer Zufallszahl zusammensetzt.

*So ändern Sie eine Regel der adaptiven Netzwerkhärtung:*

1. Um Parameter einer Regel zu ändern, klicken Sie auf der Registerkarte **Regeln** auf die drei Punkte (...) am Ende der Zeile mit der gewünschten Regel, und klicken Sie anschließend auf **Bearbeiten**.

   ![Bearbeiten von Regeln](./media/security-center-adaptive-network-hardening/edit-hard-rule.png)

1. Aktualisieren Sie im Fenster **Regel bearbeiten** die Details, die Sie ändern möchten, und klicken Sie anschließend auf **Speichern**.

   > [!NOTE]
   > Nach dem Klicken auf **Speichern** haben Sie die Regel erfolgreich geändert. *Allerdings haben Sie sie noch nicht auf die NSG angewendet.* Um sie anzuwenden, müssen Sie die Regel in der Liste auswählen und **Erzwingen** auswählen (wie im nächsten Schritt beschrieben).

   ![Auswählen von „Speichern“](./media/security-center-adaptive-network-hardening/edit-hard-rule3.png)

3. Wählen Sie in der Liste die aktualisierte Regel aus, und klicken Sie auf **Erzwingen**, um die aktualisierte Regel anzuwenden.

    ![Erzwingen einer Regel](./media/security-center-adaptive-network-hardening/enforce-hard-rule.png)

### <a name="add-a-new-rule"></a>Hinzufügen einer neuen Rolle <a name ="add-rule"> </a>

Sie können eine Regel vom Typ „Zulassen“ hinzufügen, die nicht von Security Center empfohlen wurde.

> [!NOTE]
> Nur Regeln vom Typ „Zulassen“ können hier hinzugefügt werden. Regeln vom Typ „Verweigern“ können direkt in der Netzwerksicherheitsgruppe hinzugefügt werden. Weitere Informationen finden Sie unter [Erstellen, Ändern oder Löschen einer Netzwerksicherheitsgruppe](../virtual-network/manage-network-security-group.md).

*So fügen Sie eine Regel der adaptiven Netzwerkhärtung hinzu:*

1. Klicken Sie in der linken oberen Ecke auf **Regel hinzufügen**.

   ![Hinzufügen einer Regel](./media/security-center-adaptive-network-hardening/add-hard-rule.png)

1. Geben Sie im Fenster **Neue Regel** die Details ein, und klicken Sie auf **Hinzufügen**.

   > [!NOTE]
   > Nach dem Klicken auf **Hinzufügen** haben Sie die Regel erfolgreich hinzugefügt, und sie wird zusammen mit den anderen empfohlenen Regeln aufgeführt. Allerdings haben Sie sie noch nicht auf die NSG angewendet. Um sie zu aktivieren, müssen Sie die Regel in der Liste auswählen und auf **Erzwingen** klicken (wie im nächsten Schritt beschrieben).

3. Wählen Sie in der Liste die neue Regel aus, und klicken Sie auf **Erzwingen**, um die neue Regel anzuwenden.

    ![Erzwingen einer Regel](./media/security-center-adaptive-network-hardening/enforce-hard-rule.png)


### <a name="delete-a-rule"></a>Löschen einer Regel <a name ="delete-rule"> </a>

Bei Bedarf können Sie eine empfohlene Regel für die aktuelle Sitzung löschen. Dies kann beispielsweise erforderlich sein, wenn Sie feststellen, dass eine vorgeschlagene Regel die Blockierung von zulässigem Datenverkehr zur Folge hätte.

*So löschen Sie eine Regel der adaptiven Netzwerkhärtung für Ihre aktuelle Sitzung:*

1. Klicken Sie auf der Registerkarte **Regeln** auf die drei Punkte (...) am Ende der Zeile mit der Regel, und klicken Sie anschließend auf **Löschen**.  

    ![Löschen einer Regel](./media/security-center-adaptive-network-hardening/delete-hard-rule.png)