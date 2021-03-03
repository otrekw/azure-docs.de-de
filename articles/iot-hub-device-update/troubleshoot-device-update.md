---
title: Problembehandlung bei häufigen Device Update für Azure IoT Hub-Problemen | Microsoft-Dokumentation
description: Dieses Dokument enthält eine Liste mit Tipps und Tricks als Hilfe bei der Behebung von vielen möglichen Problemen, die im Zusammenhang mit Device Update für IoT Hub möglicherweise auftreten.
author: lichris
ms.author: lichris
ms.date: 2/17/2021
ms.topic: troubleshooting
ms.service: iot-hub-device-update
ms.openlocfilehash: 6329e93bb5e628d68afbb2700ce0b9e3a2a711ad
ms.sourcegitcommit: b4647f06c0953435af3cb24baaf6d15a5a761a9c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/02/2021
ms.locfileid: "101678480"
---
# <a name="device-update-for-iot-hub-troubleshooting-guide"></a>Device Update für IoT Hub – Handbuch zur Problembehandlung

Dieses Dokument enthält einige häufig gestellte Fragen und Probleme, die von Device Update-Benutzern gemeldet wurden. Während Device Update Public Preview durchläuft, wird dieses Handbuch zur Problembehandlung regelmäßig mit neuen Fragen und Lösungen aktualisiert. Wenn bei Ihnen ein Problem auftritt, das in diesem Handbuch zur Problembehandlung nicht zu finden ist, lesen Sie den Abschnitt [Kontaktaufnahme mit dem Microsoft-Support](#contact), um Ihre Situation zu dokumentieren.

## <a name="importing-updates"></a><a name="import"></a>Importieren von Updates

### <a name="q-im-having-trouble-connecting-my-device-update-instance-to-my-iot-hub-instance"></a>F: Ich habe Probleme beim Verbinden meiner Device Update-Instanz mit meiner IoT Hub-Instanz.
_Bitte sorgen Sie dafür, dass Ihre IoT Hub-Nachrichtenrouten anhand der Dokumentation [Device Update-Ressourcen](./device-update-resources.md) richtig konfiguriert werden._

### <a name="q-im-encountering-a-role-related-error-error-message-in-azure-portal-or-a-403-api-error"></a>F: Bei mir ist ein rollenbezogener Fehler aufgetreten (eine Fehlermeldung im Azure-Portal oder ein API-Fehler 403).
_Möglicherweise haben Sie die Zugriffsberechtigungen nicht richtig konfiguriert. Bitte vergewissern Sie sich, dass Sie die Zugriffsberechtigungen anhand der Dokumentation [Device Update-Zugriffssteuerung](./device-update-control-access.md) richtig konfiguriert haben._

### <a name="q-im-encountering-a-500-type-error-when-importing-content-to-the-device-update-service"></a>F: Beim Importieren von Inhalten in den Device Update-Dienst ist ein Fehler vom Typ „500“ aufgetreten.
_Ein Fehlercode im Bereich „500“ kann auf ein Problem mit dem Device Update-Dienst hinweisen. Bitte warten Sie 5 Minuten, und versuchen Sie es dann erneut. Wenn derselbe Fehler weiterhin auftritt, führen Sie bitte die Anweisungen im Abschnitt [Kontaktaufnahme mit dem Microsoft-Support](#contact) aus, um eine Supportanfrage bei Microsoft einzureichen._

### <a name="q-im-encountering-an-error-code-when-importing-content-and-would-like-to-parse-it"></a>F: Beim Importieren von Inhalten wird mir ein Fehlercode angezeigt, den ich gerne analysieren möchte.
Informationen zum Analysieren von Fehlercodes _finden Sie in der [Dokumentation zu](./device-update-error-codes.md)Device Update-Fehlercodes._

## <a name="device-failures"></a><a name="device-failure"></a>Gerätefehler

### <a name="q-how-can-i-ensure-my-device-is-connected-to-device-update-for-iot-hub"></a>F: Wie kann ich mich vergewissern, dass mein Gerät mit Device Update für IoT Hub verbunden ist?
_Sie können sich vergewissern, dass Ihr Gerät mit Device Update verbunden ist, indem Sie überprüfen, ob es in der Konformitätsansicht des Azure-Portals im Abschnitt „Nicht gruppierte Geräte“ angezeigt wird._

### <a name="q-one-or-more-of-my-devices-is-failing-to-update"></a>F: Eines oder mehrere meiner Geräte kann/können nicht aktualisiert werden.
_Es gibt viele mögliche Grundursachen für Fehler bei einem Geräteupdate. Bitte überprüfen Sie, ob folgende Punkte zutreffen: 1) Das Gerät ist mit Ihrer IoT Hub-Instanz verbunden. 2) Das Gerät ist mit Ihrer Device Update-Instanz verbunden. 3) Der DO-Dienst (Delivery Optimization, Übermittlungsoptimierung) wird ausgeführt. Wenn alle drei Punkte für Ihr Gerät zutreffen, führen Sie bitte die Anweisungen im Abschnitt [Kontaktaufnahme mit dem Microsoft-Support](#contact) aus, um eine Supportanfrage bei Microsoft einzureichen._

## <a name="deploying-an-update"></a><a name="deploy"></a> Bereitstellen eines Updates

### <a name="q-ive-deployed-an-update-to-my-devices-but-the-compliance-status-says-it-isnt-on-the-latest-update-what-should-i-do"></a>F: Ich habe ein Update für mein(e) Gerät(e) bereitgestellt, aber für den Konformitätsstatus wird angezeigt, dass dies nicht das neueste Update ist. Wie sollte ich vorgehen?
_Es kann bis zu 5 Minuten dauern, bis der Gerätekonformitätsstatus aktualisiert wird. Bitte warten Sie so lange, und wiederholen Sie dann die Prüfung._
### <a name="q-my-devices-deployment-status-shows-incompatible-what-should-i-do"></a>F: Für den Bereitstellungsstatus meines Geräts wird „inkompatibel“ angezeigt. Wie sollte ich jetzt vorgehen?
_Die Hersteller- und Modelleigenschaften eines Zielgeräts wurden möglicherweise geändert, nachdem das Gerät mit IoT Hub verbunden wurde. Dies hat dazu geführt, dass das Gerät jetzt als inkompatibel mit dem Updateinhalt der aktuellen Bereitstellung betrachtet wird._

_Überprüfen Sie auf der [ADU Core-Schnittstelle](./device-update-plug-and-play), welchen Hersteller und welches Modell Ihr Gerät dem Device Update-Dienst meldet, und stellen Sie sicher, dass diese Angaben mit dem Hersteller und Modell übereinstimmen, den/das Sie im [Importmanifest](./import-concepts.md) des gerade bereitgestellten Updateinhalts angegeben haben. Sie können diese Eigenschaften für ein bestimmtes Gerät mithilfe der [Device Update-Konfigurationsdatei](./device-update-cofiguration-file.md) ändern._

### <a name="q-i-see-my-deployment-is-in-active-stage-but-none-of-my-devices-are-in-progress-with-the-update-what-should-i-do"></a>F: Ich sehe, dass meine Bereitstellung in der Phase „Aktiv“ ist, aber für keines meiner Geräte wird beim Update „In Bearbeitung“ angezeigt. Wie sollte ich vorgehen?
_Sorgen Sie dafür, dass Ihr festgelegtes Startdatum für die Bereitstellung nicht in der Zukunft liegt. Wenn Sie eine neue Bereitstellung erstellen, wird als Startdatum dafür standardmäßig sicherheitshalber der nächste Tag festgelegt, wenn Sie dies nicht explizit ändern. Sie können entweder warten, bis das Startdatum für die Bereitstellung erreicht ist, oder die laufende Bereitstellung abbrechen und eine neue Bereitstellung mit dem gewünschten Startdatum erstellen._

### <a name="q-im-trying-to-group-my-devices-but-i-dont-see-the-tag-in-the-drop-down-when-creating-a-group"></a>F: Ich versuche, meine Geräte zu gruppieren, aber beim Erstellen einer Gruppe wird das Tag im Dropdown nicht angezeigt.
_Vergewissern Sie sich, dass Sie die Nachrichtenrouten in Ihrem IoT Hub anhand der Dokumentation [Device Update-Ressourcen](./device-update-resources.md) richtig konfiguriert haben. Nach dem Konfigurieren der Route müssen Sie Ihr Gerät erneut kennzeichnen._

_Eine weitere Grundursache könnte sein, dass Sie das Tag vor dem Verbinden Ihres Geräts mit Device Update für IoT Hub angewendet haben. Vergewissern Sie sich, das Ihr Gerät bereits mit Device Update verbunden ist. Sie können sich vergewissern, dass Ihr Gerät mit Device Update für IoT Hub verbunden ist, indem Sie überprüfen, ob es in der Konformitätsansicht unter „Nicht gruppierte Geräte“ angezeigt wird. Fügen Sie vorübergehend ein Tag mit einem anderen Wert und – sobald das Gerät verbunden wurde – erneut Ihr vorgesehenes Tag hinzu._

_Wenn Sie den Gerätebereitstellungsdienst (Device Provisioning Service, DPS) nutzen, müssen Sie Ihre Geräte nach deren Bereitstellung (und nicht während des Prozesses zur Geräteerstellung) kennzeichnen. Wenn Sie Ihr Gerät bereits während des Schritts „Geräteerstellung“ gekennzeichnet haben, müssen Sie es nach seiner Bereitstellung vorübergehend mit einem anderen Wert kennzeichnen und dann Ihr vorgesehenes Tag erneut hinzufügen._

### <a name="q-my-deployment-completed-successfully-but-some-devices-failed-to-update"></a>F: Meine Bereitstellung wurde erfolgreich abgeschlossen, aber einige Geräte konnten nicht aktualisiert werden.
_Dies wurde möglicherweise durch einen clientseitigen Fehler auf den fehlerhaften Geräten verursacht. Bitte lesen Sie dazu in diesem Handbuch zur Problembehandlung den Abschnitt „Gerätefehler“._

### <a name="q-i-encountered-an-error-in-the-ux-when-trying-to-initiate-a-deployment"></a>F: Beim Versuch, eine Bereitstellung zu initiieren, ist ein Fehler in der Benutzeroberfläche aufgetreten.
_Dies wurde möglicherweise durch einen Dienstfehler/Benutzeroberflächenfehler oder ein API-Berechtigungsproblem verursacht. Bitte führen Sie die Anweisungen im Abschnitt [Kontaktaufnahme mit dem Microsoft-Support](#contact) aus, um eine Supportanfrage bei Microsoft einzureichen._

### <a name="q-i-started-a-deployment-but-it-isnt-reaching-an-end-state"></a>F: Ich habe eine Bereitstellung gestartet, doch sie erreicht keinen Endzustand.
_Die Ursache dafür kann ein Dienstleistungsproblem, ein Dienstfehler oder ein Clientfehler sein. Bitte wiederholen Sie Ihre Bereitstellung nach 10 Minuten. Wenn dasselbe Problem auftritt, pullen Sie bitte Ihre Geräteprotokolle, und lesen Sie in diesem Handbuch zur Problembehandlung den Abschnitt „Gerätefehler“. Wenn dasselbe Problem weiterhin auftritt, führen Sie bitte die Anweisungen im Abschnitt [Kontaktaufnahme mit dem Microsoft-Support](#contact) aus, um eine Supportanfrage bei Microsoft einzureichen._

## <a name="downloading-updates-onto-devices"></a><a name="download"></a> Herunterladen von Updates auf Geräte

### <a name="q-how-do-i-resume-a-download-when-a-device-has-reconnected-after-a-period-of-disconnection"></a>F: Wie kann ich einen Download fortsetzen, wenn ein Gerät nach einer Phase der Verbindungstrennung erneut verbunden wurde?
_Der Download wird wieder aufgenommen, wenn die Konnektivität innerhalb eines Zeitraums von 24 Stunden wiederhergestellt wird. Nach 24 Stunden muss der Download vom Benutzer erneut initiiert werden._
## <a name="using-microsoft-connected-cache-mcc"></a><a name="mcc"></a> Verwendung von Microsoft Connected Cache (MCC)

### <a name="q-i-am-encountering-an-issue-when-attempting-to-deploy-the-mcc-module-on-my-iot-edge-device"></a>F: Wenn ich versuche, das MCC-Modul auf meinem IoT Edge Gerät bereitzustellen, tritt ein Problem auf.
_Informationen zur Bereitstellung von Edge-Modulen auf IoT Edge-Geräten finden Sie in der [IoT Edge-Dokumentation](). Sie können überprüfen, ob das MCC-Modul auf Ihrem IoT Edge-Gerät erfolgreich ausgeführt wird, indem Sie zu http://localhost:5100/Summary navigieren._
### <a name="q-one-of-my-iot-devices-is-attempting-to-download-an-update-through-mcc-but-is-failing"></a>F: Eines meiner IoT-Geräte versucht, ein Update über MCC herunterzuladen, doch das gelingt nicht.
_Es gibt mehrere Probleme, die dazu führen können, dass ein IoT-Gerät keine Verbindung mit MCC herstellen kann. Sammeln Sie bitte zum Diagnostizieren des Problems die DO Client- und Nginx-Protokolle aus dem fehlgeschlagenen Gerät (Anleitungen zum Erfassen von Clientprotokollen finden Sie im Abschnitt [Kontaktaufnahme mit dem Microsoft-Support](#contact))._

_Möglicherweise kann Ihr Gerät keine Inhalte aus dem Internet zur Übergabe an sein MCC-Modul pullen, weil die von ihm verwendete URL nicht zulässig ist. Zur Ermittlung, ob dies zutrifft, müssen Sie Ihre IoT Edge-Umgebungsvariablen im Azure-Portal überprüfen._
## <a name="contacting-microsoft-support"></a><a name="contact"></a>Kontaktaufnahme mit dem Microsoft-Support

Wenn Probleme auftreten, die sich mithilfe der oben aufgeführten FAQs (Häufig gestellte Fragen) nicht lösen lassen, können Sie über die Benutzeroberfläche des Azure-Portals eine Supportanfrage beim Microsoft-Support einreichen. Je nach der Kategorie, zu der Ihr Problem entsprechend Ihrer Angabe gehört, werden Sie möglicherweise aufgefordert, zusätzliche Daten zu erfassen und freizugeben, damit der Microsoft-Support Ihr Problem untersuchen kann. 

Anleitungen zum Erfassen der einzelnen Datentypen finden Sie weiter unten. Mithilfe von [getDevices]() können Sie überprüfen, ob die Nutzlastantwort der API zusätzliche Informationen enthält.

Außerdem können die folgenden Informationen hilfreich sein, um die Grundursache Ihres Problems einzugrenzen:
* Der Typ des Geräts, das Sie zu aktualisieren versuchen (Azure Percept, IoT Edge Gateway, anderes)
* Der verwendete Device Update-Clienttyp (imagebasiert, paketbasiert, Simulator)
* Das Betriebssystem Ihres Geräts
* Details zur Architektur Ihres Geräts
* Die Mitteilung, ob Sie Device Update zum Aktualisieren eines Geräts vorher erfolgreich verwendet haben

Wenn Ihnen eine der oben genannten Informationen vorliegt, fügen Sie sie bitte in Ihre Beschreibung des Problems ein.

### <a name="collecting-client-logs"></a>Sammeln von Clientprotokollen

* Auf dem Raspberry Pi-Gerät finden Sie hier zwei Sätze von Protokollen:

    ```markdown
    /adu/logs
    ```

    ```markdown
    /var/cache/do-client-lite/log
    ```

* Die Protokolle für das Clientpaket finden Sie hier:

    ```markdown
    /var/log/adu
    ```

    ```markdown
    /var/cache/do-client-lite/log
    ```

* Die Protokolle für den Simulator finden Sie hier:

    ```markdown
    /tmp/aduc-logs
    ```

### <a name="error-codes"></a>Fehlercodes
Möglicherweise werden Sie zur Angabe von Fehlercodes aufgefordert, wenn Sie ein Problem im Zusammenhang mit dem Importieren eines Updates, einem Gerätefehler oder der Bereitstellung eines Updates melden.

Fehlercodes können über die Schnittstelle [ADUCoreInterface](./device-update-plug-and-play.md) abgerufen werden. Informationen zum Analysieren von Fehlercodes für Selbstdiagnose und Problembehandlung finden Sie in der Dokumentation zu den [Device Update-Fehlercodes](./device-update-error-codes.md).

### <a name="trace-id"></a>Ablaufverfolgungs-ID
Möglicherweise werden Sie zur Angabe einer Ablaufverfolgungs-ID aufgefordert, wenn Sie ein Problem im Zusammenhang mit dem Importieren oder der Bereitstellung eines Updates melden.

Die Ablaufverfolgungs-ID für eine bestimmte Benutzeraktion finden Sie in der API-Antwort oder im Abschnitt „Import History“ (Importverlauf) der Benutzeroberfläche des Azure-Portals. 

Zurzeit kann auf Ablaufverfolgungs-IDs für Bereitstellungsaktionen nur über die API-Antwort zugegriffen werden.

### <a name="deployment-id"></a>Bereitstellungs-ID
Möglicherweise werden Sie zur Angabe einer Bereitstellungs-ID aufgefordert, wenn Sie ein Problem im Zusammenhang mit der Bereitstellung eines Updates melden.

Die Bereitstellungs-ID wird vom Benutzer erstellt, wenn er die API zum Initiieren einer Bereitstellung aufruft.

Zurzeit werden Bereitstellungs-IDs für Bereitstellungen, die über die Benutzeroberfläche des Azure-Portals initiiert werden, automatisch generiert und dem Benutzer nicht angezeigt.

### <a name="iot-hub-instance-name"></a>IoT Hub-Instanzname
Möglicherweise werden Sie zur Angabe Ihres IoT Hub-Instanznamens aufgefordert, wenn Sie ein Problem im Zusammenhang mit Gerätefehlern oder der Bereitstellung eines Updates melden.

Der IoT Hub-Name wird vom Benutzer bei der ersten Bereitstellung ausgewählt.

### <a name="device-update-account-name"></a>Device Update-Kontoname
Möglicherweise werden Sie zur Angabe Ihres Device Update-Kontonamens aufgefordert, wenn Sie ein Problem im Zusammenhang mit dem Importieren eines Updates, Gerätefehlern oder der Bereitstellung eines Updates melden.

Der Device Update-Kontoname wird vom Benutzer ausgewählt, wenn er sich zum ersten Mal bei dem Dienst anmeldet. Weitere Informationen finden Sie in der Dokumentation zu [Device Update-Ressourcen](./device-update-resources.md).

### <a name="device-update-instance-name"></a>Device Update-Instanzname
Möglicherweise werden Sie zur Angabe Ihres Device Update-Instanznamens aufgefordert, wenn Sie ein Problem im Zusammenhang mit dem Importieren eines Updates, Gerätefehlern oder der Bereitstellung eines Updates melden.

Der Device Update-Instanzname wird vom Benutzer bei der ersten Bereitstellung ausgewählt. Weitere Informationen finden Sie in der Dokumentation zu [Device Update-Ressourcen](./device-update-resources.md).

### <a name="device-id"></a>Geräte-ID
Möglicherweise werden Sie zur Angabe einer Geräte-ID aufgefordert, wenn Sie ein Problem im Zusammenhang mit Gerätefehlern oder der Bereitstellung eines Updates melden.

Die Geräte-ID wird vom Kunden definiert, wenn das Gerät zum ersten Mal bereitgestellt wird. Sie kann auch aus dem Gerätezwilling des Geräts abgerufen werden.

### <a name="update-id"></a>Update-ID
Möglicherweise werden Sie zur Angabe einer Update-ID aufgefordert, wenn Sie ein Problem im Zusammenhang mit der Bereitstellung eines Updates melden.

Die Update-ID wird vom Kunden definiert, wenn er eine Bereitstellung initiiert.

### <a name="nginx-logs"></a>Nginx-Protokolle
Möglicherweise werden Sie zur Angabe von Nginx-Protokollen aufgefordert, wenn Sie ein Problem im Zusammenhang mit Microsoft Connected Cache melden.

### <a name="adu-conftxt"></a>ADU-conf.txt
Möglicherweise werden Sie zur Angabe der Device Update-Konfigurationsdatei („adu-conf.txt“) aufgefordert, wenn Sie ein Problem im Zusammenhang mit der Bereitstellung eines Updates melden.

Die Konfigurationsdatei ist optional und wird vom Benutzer anhand der Anleitungen in der Dokumentation zur [Device Update-Aktualisierung](./device-update-configuration-file.md) erstellt.

### <a name="import-manifest"></a>Importmanifest
Möglicherweise werden Sie zur Angabe Ihrer Importmanifestdatei aufgefordert, wenn Sie ein Problem im Zusammenhang mit dem Importieren oder der Bereitstellung eines Updates melden.

Das Importmanifest ist eine Datei, die vom Kunden beim Importieren von Updateinhalten in den Device Update-Dienst erstellt wird.

**[Nächster Schritt: Informieren Sie sich genauer über Device Update-Fehlercodes.](.\device-update-error-codes.md)**