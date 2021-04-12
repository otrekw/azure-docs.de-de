---
title: 'Tutorial: Analysieren von betrügerischen Anrufdaten mit Azure Stream Analytics und Visualisieren der Ergebnisse in einem Power BI-Dashboard'
description: Dieses Tutorial zeigt ausführlich, wie Sie mithilfe von Azure Stream Analytics betrügerische Anrufe in einem Telefonanruf-Datenstrom analysieren können.
author: enkrumah
ms.author: ebnkruma
ms.service: stream-analytics
ms.topic: tutorial
ms.custom: contperf-fy21q2
ms.date: 12/17/2020
ms.openlocfilehash: 1e26159c07ca551a78ee2f83a0ca64779c60f7b4
ms.sourcegitcommit: f28ebb95ae9aaaff3f87d8388a09b41e0b3445b5
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/29/2021
ms.locfileid: "98018869"
---
# <a name="tutorial-analyze-fraudulent-call-data-with-stream-analytics-and-visualize-results-in-power-bi-dashboard"></a>Tutorial: Analysieren von betrügerischen Anrufdaten mit Stream Analytics und Visualisieren der Ergebnisse in einem Power BI-Dashboard

In diesem Tutorial erfahren Sie, wie Sie Telefonanrufdaten mithilfe von Azure Stream Analytics analysieren. Die von einer Clientanwendung generierten Telefonanrufdaten enthalten betrügerische Anrufe, die durch den Stream Analytics-Auftrag herausgefiltert werden. Die Verfahren aus diesem Tutorial lassen sich jedoch auch bei anderen Arten von Betrugserkennungsszenarien einsetzen, z. B. bei Kreditkartenbetrug oder Identitätsdiebstahl.

In diesem Tutorial lernen Sie Folgendes:

> [!div class="checklist"]
> * Generieren exemplarischer Telefonanrufdaten und Senden der Daten an Azure Event Hubs
> * Erstellen Sie einen Stream Analytics-Auftrag.
> * Konfigurieren der Auftragseingabe und -ausgabe
> * Definieren von Abfragen zum Filtern von betrügerischen Anrufen
> * Testen und Starten des Auftrags
> * Visualisieren der Ergebnisse in Power BI

## <a name="prerequisites"></a>Voraussetzungen

Stellen Sie zunächst sicher, dass Sie folgende Schritte abgeschlossen haben:

* Wenn Sie kein Azure-Abonnement besitzen, können Sie ein [kostenloses Konto](https://azure.microsoft.com/free/) erstellen.
* Laden Sie die App [TelcoGenerator.zip](https://download.microsoft.com/download/8/B/D/8BD50991-8D54-4F59-AB83-3354B69C8A7E/TelcoGenerator.zip) zum Generieren von Anrufereignissen aus dem Microsoft Download Center herunter, oder rufen Sie den Quellcode von [GitHub](https://aka.ms/azure-stream-analytics-telcogenerator) ab.
* Sie benötigen ein Power BI-Konto.

## <a name="sign-in-to-azure"></a>Anmelden bei Azure

Melden Sie sich beim [Azure-Portal](https://portal.azure.com) an.

## <a name="create-an-azure-event-hub"></a>Erstellen eines Azure Event Hubs

Damit Stream Analytics den Datenstrom mit den betrügerischen Anrufen analysieren kann, müssen die Daten an Azure gesendet werden. In diesem Tutorial senden Sie Daten an Azure, indem Sie [Azure Event Hubs](../event-hubs/event-hubs-about.md) verwenden.

Gehen Sie wie folgt vor, um einen Event Hub zu erstellen und Anrufdaten an diesen Event Hub zu senden:

1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com/) an.
2. Wählen Sie **Ressource erstellen** > **Internet der Dinge (IoT)**  > **Event Hubs**.

   ![Erstellen eines Azure Event Hubs im Portal](media/stream-analytics-real-time-fraud-detection/find-event-hub-resource.png)
3. Geben Sie im Bereich **Namespace erstellen** die folgenden Werte an:

   |**Einstellung**  |**Empfohlener Wert** |**Beschreibung**  |
   |---------|---------|---------|
   |Name     | asaTutorialEventHub        |  Ein eindeutiger Name zum Identifizieren des Event Hub-Namespace.       |
   |Subscription     |   \<Your subscription\>      |   Wählen Sie ein Azure-Abonnement aus, unter dem Sie den Event Hub erstellen möchten.      |
   |Resource group     |   MyASADemoRG      |  Wählen Sie **Neu erstellen** aus, und geben Sie einen neuen Ressourcengruppennamen für Ihr Konto ein.       |
   |Position     |   USA, Westen 2      |    Der Standort, an dem der Event Hub-Namespace bereitgestellt werden kann.     |

4. Verwenden Sie für die verbleibenden Einstellungen die Standardoptionen, und wählen Sie **Überprüfen und erstellen** aus. Wählen Sie anschließend **Erstellen** aus, um die Bereitstellung zu starten.

   ![Erstellen eines Event Hub-Namespace im Azure-Portal](media/stream-analytics-real-time-fraud-detection/create-event-hub-namespace.png)

5. Navigieren Sie nach Abschluss der Namespacebereitstellung zu **Alle Ressourcen**, und suchen Sie in der Liste mit den Azure-Ressourcen nach *asaTutorialEventHub*. Wählen Sie *asaTutorialEventHub* aus, um die Ressource zu öffnen.

6. Wählen Sie als Nächstes **+Event Hub** aus, und geben Sie unter **Name** einen Namen für den Event Hub ein. Legen Sie den Wert für **Partitionsanzahl** auf 2 fest.  Verwenden Sie für die restlichen Einstellungen die Standardoptionen, und wählen Sie **Erstellen** aus. Warten Sie, bis die Bereitstellung erfolgreich abgeschlossen wurde.

   ![Event Hub-Konfiguration im Azure-Portal](media/stream-analytics-real-time-fraud-detection/create-event-hub-portal.png)

### <a name="grant-access-to-the-event-hub-and-get-a-connection-string"></a>Gewähren des Zugriffs auf den Event Hub und Abrufen einer Verbindungszeichenfolge

Bevor eine Anwendung Daten an Azure Event Hubs senden kann, muss der Event Hub mit einer Richtlinie versehen werden, die einen Zugriff ermöglicht. Die Zugriffsrichtlinie erzeugt eine Verbindungszeichenfolge, die Autorisierungsinformationen enthält.

1. Navigieren Sie zum Event Hub *MyEventHub*, den Sie im vorherigen Schritt erstellt haben. Wählen Sie unter **Einstellungen** die Option **SAS-Richtlinien** und anschließend **+ Hinzufügen** aus.

2. Nennen Sie die Richtlinie **MyPolicy**, und vergewissern Sie sich, dass das Kontrollkästchen **Verwalten** aktiviert ist. Klicken Sie anschließend auf **Erstellen**.

   ![Erstellen einer SAS-Richtlinie für den Event Hub](media/stream-analytics-real-time-fraud-detection/create-event-hub-access-policy.png)

3. Wählen Sie nach der Erstellung der Richtlinie den Richtliniennamen aus, um die Richtlinie zu öffnen. Suchen Sie nach **Verbindungszeichenfolge – Primärschlüssel**. Wählen Sie neben der Verbindungszeichenfolge die Schaltfläche zum **Kopieren** aus.

   ![Speichern der Verbindungszeichenfolge der SAS-Richtlinie](media/stream-analytics-real-time-fraud-detection/save-connection-string.png)

4. Fügen Sie die Verbindungszeichenfolge in einen Texteditor ein. Sie benötigen diese Verbindungszeichenfolge im nächsten Abschnitt.

   Die Verbindungszeichenfolge sieht wie folgt aus:

   `Endpoint=sb://<Your event hub namespace>.servicebus.windows.net/;SharedAccessKeyName=<Your shared access policy name>;SharedAccessKey=<generated key>;EntityPath=<Your event hub name>`

   Beachten Sie, dass die Verbindungszeichenfolge mehrere durch ein Semikolon getrennte Schlüssel/Wert-Paare enthält: **Endpoint**, **SharedAccessKeyName**, **SharedAccessKey** und **EntityPath**.

## <a name="start-the-event-generator-application"></a>Starten der Ereignisgenerator-Anwendung

Vor dem Starten der TelcoGenerator-App sollten Sie diese so konfigurieren, dass Daten an die zuvor erstellten Azure Event Hubs gesendet werden.

1. Extrahieren Sie den Inhalt der Datei [TelcoGenerator.zip](https://download.microsoft.com/download/8/B/D/8BD50991-8D54-4F59-AB83-3354B69C8A7E/TelcoGenerator.zip).
2. Öffnen Sie die Datei `TelcoGenerator\TelcoGenerator\telcodatagen.exe.config` in einem Text-Editor Ihrer Wahl. Da mehr als eine `.config`-Datei vorhanden ist, sollten Sie darauf achten, dass Sie die richtige Datei öffnen.

3. Aktualisieren Sie das `<appSettings>`-Element in der CONFIG-Datei mit den folgenden Details:

   * Legen Sie den Wert des Schlüssels *EventHubName* auf den Wert von „EntityPath“ in der Verbindungszeichenfolge fest.
   * Legen Sie den Wert des Schlüssels *Microsoft.ServiceBus.ConnectionString* auf die Verbindungszeichenfolge ohne EntityPath-Wert fest. Vergessen Sie nicht, das Semikolon zu entfernen, das dem EntityPath-Wert vorangestellt ist.

4. Speichern Sie die Datei.

5. Öffnen Sie nun ein Befehlsfenster, und wechseln Sie zu dem Ordner, in dem Sie die TelcoGenerator-Anwendung entzippt haben. Geben Sie dann den folgenden Befehl ein:

   ```cmd
   .\telcodatagen.exe 1000 0.2 2
   ```

   Für diesen Befehl werden die folgenden Parameter verwendet:
   * Anzahl von Anrufdatensätzen pro Stunde.
   * Betrugswahrscheinlichkeit in Prozent (gibt an, wie oft die App einen betrügerischen Anruf simulieren soll). Der Wert „0.2“ bedeutet, dass etwa 20 Prozent der Anrufdatensätze betrügerische Anrufe simulieren.
   * Dauer in Stunden (also wie lange die App ausgeführt werden soll). Sie können die App auch jederzeit beenden, indem Sie den Prozess über die Befehlszeile beenden (**STRG+C**).

   Nach wenigen Sekunden werden auf dem Bildschirm in der App die Telefonanrufdatensätze angezeigt, während diese an den Event Hub gesendet werden. Die Telefonanrufdaten enthalten die folgenden Felder:

   |**Datensatz**  |**Definition**  |
   |---------|---------|
   |CallrecTime    |  Der Zeitstempel für die Startzeit des Anrufs.       |
   |SwitchNum     |  Die für die Anrufverbindung verwendete Vermittlungsstelle. In diesem Beispiel werden die Vermittlungen durch Zeichenfolgen ausgedrückt, die das Ursprungsland/die Ursprungsregion (USA, China, Großbritannien, Deutschland oder Australien) darstellen.       |
   |CallingNum     |  Die Telefonnummer des Anrufers.       |
   |CallingIMSI     |  Die IMSI (International Mobile Subscriber Identity). Dies ist eine eindeutige ID des Anrufers.       |
   |CalledNum     |   Die Telefonnummer des Angerufenen.      |
   |CalledIMSI     |  International Mobile Subscriber Identity (IMSI). Dies ist eine eindeutige ID des Empfängers.       |

## <a name="create-a-stream-analytics-job"></a>Erstellen eines Stream Analytics-Auftrags

Nachdem Sie nun über einen Datenstrom mit Anrufereignissen verfügen, können Sie einen Stream Analytics-Auftrag erstellen, mit dem Daten aus dem Event Hub gelesen werden.

1. Navigieren Sie zum [Azure-Portal](https://portal.azure.com/), um einen Stream Analytics-Auftrag zu erstellen.

2. Wählen Sie **Ressource erstellen** aus, und suchen Sie nach **Stream Analytics-Auftrag**. Wählen Sie die Kachel **Stream Analytics-Auftrag** und dann *Erstellen** aus.

3. Fügen Sie die folgenden Werte in das Formular **Neuer Stream Analytics-Auftrag** ein:

   |**Einstellung**  |**Empfohlener Wert**  |**Beschreibung**  |
   |---------|---------|---------|
   |Auftragsname     |  ASATutorial       |   Ein eindeutiger Name zum Identifizieren des Event Hub-Namespace.      |
   |Subscription    |  \<Your subscription\>   |   Wählen Sie ein Azure-Abonnement aus, unter dem Sie den Auftrag erstellen möchten.       |
   |Resource group   |   MyASADemoRG      |   Wählen Sie **Vorhandene verwenden**, und geben Sie einen neuen Ressourcengruppennamen für Ihr Konto ein.      |
   |Position   |    USA, Westen 2     |      Der Standort, an dem der Auftrag bereitgestellt werden kann. Es empfiehlt sich, den Auftrag und den Event Hub in derselben Region zu platzieren, damit Sie die optimale Leistung erzielen und Ihnen keine Kosten für die Übertragung von Daten zwischen Regionen entstehen.      |
   |Hosting-Umgebung    | Cloud        |     Für Stream Analytics-Aufträge ist eine Cloud- oder Edge-Bereitstellung möglich. Mit der Option „Cloud“ können die Aufträge in Azure Cloud und mit der Option „Edge“ auf einem IoT Edge-Gerät bereitgestellt werden.    |
   |Streamingeinheiten     |    1       |      Streamingeinheiten sind die Computingressourcen, die für die Ausführung eines Auftrags erforderlich sind. Standardmäßig ist dieser Wert auf 1 festgelegt. Informationen zum Skalieren von Streamingeinheiten finden Sie im Artikel [Überblick über Streamingeinheiten und Informationen zu Anpassungen](stream-analytics-streaming-unit-consumption.md).      |

4. Verwenden Sie für die verbleibenden Einstellungen die Standardoptionen, wählen Sie **Erstellen** aus, und warten Sie auf den erfolgreichen Abschluss der Bereitstellung.

   ![Erstellen eines Azure Stream Analytics-Auftrags](media/stream-analytics-real-time-fraud-detection/create-stream-analytics-job.png)

## <a name="configure-job-input"></a>Konfigurieren einer Auftragseingabe

Im nächsten Schritt wird für den Auftrag eine Eingabequelle zum Lesen von Daten definiert. Dabei wird der Event Hub verwendet, den Sie im vorherigen Abschnitt erstellt haben.

1. Öffnen Sie im Azure-Portal die Seite **Alle Ressourcen**, und suchen Sie nach dem Stream Analytics-Auftrag *ASATutorial*.

2. Wählen Sie im Abschnitt **Auftragstopologie** des Stream Analytics-Auftrags die Option **Eingaben** aus.

3. Wählen Sie **Datenstromeingabe hinzufügen** und anschließend **Event Hub** aus. Geben Sie in dem Eingabeformular die folgenden Werte an:

   |**Einstellung**  |**Empfohlener Wert**  |**Beschreibung**  |
   |---------|---------|---------|
   |Eingabealias     |  CallStream       |  Geben Sie einen Anzeigenamen zum Identifizieren Ihrer Eingabe an. Der Eingabealias darf nur alphanumerische Zeichen, Bindestriche und Unterstriche enthalten und muss zwischen 3 und 63 Zeichen lang sein.       |
   |Subscription    |   \<Your subscription\>      |   Wählen Sie das Azure-Abonnement aus, unter dem Sie den Event Hub erstellt haben. Der Event Hub kann sich unter demselben oder einem anderen Abonnement wie der Stream Analytics-Auftrag befinden.       |
   |Event Hub-Namespace    |  asaTutorialEventHub       |  Wählen Sie den Event Hub-Namespace aus, den Sie im vorherigen Abschnitt erstellt haben. Alle Event Hub-Namespaces, die unter Ihrem aktuellen Abonnement verfügbar sind, sind in der Dropdownliste aufgeführt.       |
   |Event Hub-Name    |   MyEventHub      |  Wählen Sie den Event Hub aus, den Sie im vorherigen Abschnitt erstellt haben. Alle Event Hubs, die unter Ihrem aktuellen Abonnement verfügbar sind, sind in der Dropdownliste aufgeführt.       |
   |Event Hub-Richtlinienname   |  MyPolicy       |  Wählen Sie die Event Hub-SAS-Richtlinie aus, die Sie im vorherigen Abschnitt erstellt haben. Alle Event Hub-Richtlinien, die unter Ihrem aktuellen Abonnement verfügbar sind, sind in der Dropdownliste aufgeführt.       |

4. Verwenden Sie für die restlichen Einstellungen die Standardoptionen, und klicken Sie auf **Speichern**.

   ![Konfigurieren der Azure Stream Analytics-Eingabe](media/stream-analytics-real-time-fraud-detection/configure-stream-analytics-input.png)

## <a name="configure-job-output"></a>Konfigurieren der Auftragsausgabe

Der letzte Schritt umfasst das Definieren einer Ausgabesenke, in die der Auftrag die transformierten Daten schreiben kann. In diesem Tutorial verwenden Sie Power BI, um Daten auszugeben und zu visualisieren.

1. Öffnen Sie im Azure-Portal **Alle Ressourcen**, und wählen Sie den Stream Analytics-Auftrag *ASATutorial* aus.

2. Wählen Sie im Abschnitt **Auftragstopologie** des Stream Analytics-Auftrags die Option **Ausgaben** aus.

3. Wählen Sie **+ Hinzufügen** > **Power BI** aus. Wählen Sie anschließend **Autorisieren** aus, und befolgen Sie die Eingabeaufforderungen zum Authentifizieren von Power BI.

:::image type="content" source="media/stream-analytics-real-time-fraud-detection/authorize-power-bi.png" alt-text="Autorisierungsschaltfläche für Power BI":::

4. Geben Sie folgende Informationen in das Ausgabeformular ein, und wählen Sie **Speichern** aus:

   |**Einstellung**  |**Empfohlener Wert**  |
   |---------|---------|
   |Ausgabealias  |  MyPBIoutput  |
   |Gruppenarbeitsbereich| Mein Arbeitsbereich |
   |Datasetname  |   ASAdataset  |
   |Tabellenname |  ASATable  |
   | Authentifizierungsmodus | Benutzertoken |

   ![Konfigurieren der Azure Stream Analytics-Ausgabe](media/stream-analytics-real-time-fraud-detection/configure-stream-analytics-output.png)

   In diesem Tutorial wird der Authentifizierungsmodus *Benutzertoken* verwendet. Informationen zur Verwendung von verwalteten Identitäten finden Sie unter [Verwenden von verwalteten Identitäten zum Authentifizieren von Azure Stream Analytics-Aufträgen in Power BI](powerbi-output-managed-identity.md).

## <a name="create-queries-to-transform-real-time-data"></a>Erstellen von Abfragen zum Transformieren von Daten in Echtzeit

An diesem Punkt haben Sie einen Stream Analytics-Auftrag zum Lesen eines eingehenden Datenstroms eingerichtet. Der nächste Schritt besteht darin, eine Abfrage zu erstellen, die die Daten in Echtzeit analysiert. Die Abfragen verwenden eine SQL-ähnliche Sprache, die einige Erweiterungen speziell für Stream Analytics aufweist.

In diesem Abschnitt des Tutorials erstellen und testen Sie verschiedene Abfragen, um einige Möglichkeiten kennenzulernen, mit denen Sie einen Eingabedatenstrom zur Analyse transformieren können. 

Die Abfragen, die Sie hier erstellen, zeigen nur die transformierten Daten auf dem Bildschirm an. In einem späteren Abschnitt werden Sie die transformierten Daten in Power BI schreiben.

Weitere Informationen zur Sprache finden Sie in der [Azure Stream Analytics-Abfragesprachreferenz](/stream-analytics-query/stream-analytics-query-language-reference).

### <a name="test-using-a-pass-through-query"></a>Testen durch eine Pass-Through-Abfrage

Wenn Sie alle Ereignisse archivieren möchten, können Sie eine Pass-Through-Abfrage verwenden, um alle Felder in der Nutzlast des Ereignisses zu lesen.

1. Navigieren Sie im Azure-Portal zu Ihrem Stream Analytics-Auftrag, und klicken Sie unter *Auftragstopologie* auf **Abfrage**. 

2. Geben Sie im Abfragefenster die folgende Abfrage ein:

   ```SQL
   SELECT 
       *
   FROM 
       CallStream
   ```

    >[!NOTE]
    >Wie bei SQL muss die Groß-/Kleinschreibung bei Schlüsselwörtern nicht beachtet werden, und auch Leerräume spielen keine Rolle.

    In dieser Abfrage ist `CallStream` der Alias, den Sie beim Erstellen der Eingabe angegeben haben. Wenn Sie einen anderen Alias verwendet haben, verwenden Sie stattdessen diesen Namen.

3. Wählen Sie **Testabfrage** aus.

    Der Stream Analytics-Auftrags führt die Abfrage für die Beispieldaten der Eingabe aus und zeigt die Ausgabe im unteren Bereich des Fensters an. Die Ergebnisse weisen darauf hin, dass der Event Hub und der Stream Analytics-Auftrag korrekt konfiguriert sind.

    :::image type="content" source="media/stream-analytics-real-time-fraud-detection/sample-output-passthrough.png" alt-text="Beispielausgabe der Testabfrage":::

    Die genaue Anzahl der angezeigten Datensätze hängt davon ab, wie viele Datensätze in der Stichprobe erfasst wurden.

### <a name="reduce-the-number-of-fields-using-a-column-projection"></a>Reduzieren der Anzahl von Feldern durch eine Spaltenprojektion

In vielen Fällen sind bei Ihrer Analyse nicht alle Spalten der Datenstromeingabe erforderlich. Mithilfe einer Abfrage können Sie kleinere Sätze von zurückgegebenen Feldern projizieren als in der Pass-Through-Abfrage.

Führen Sie die folgende Abfrage aus, und beachten Sie die Ausgabe.

```SQL
SELECT CallRecTime, SwitchNum, CallingIMSI, CallingNumCalledNum 
FROM 
    CallStream
```

### <a name="count-incoming-calls-by-region-tumbling-window-with-aggregation"></a>Zählen von eingehenden Anrufen nach Region: Rollierendes Fenster mit Aggregation

Nehmen wir an, Sie möchten die Anzahl der eingehenden Anrufe pro Region zählen. Wenn Sie bei Streamingdaten Aggregatfunktionen wie eine Zählung durchführen möchten, müssen Sie den Datenstrom in temporale Einheiten segmentieren, da der Datenstrom selbst faktisch endlos ist. Hierzu müssen Sie eine Stream Analytics-[Fensterfunktion](stream-analytics-window-functions.md) verwenden. Sie können dann mühelos in diesem Fenster mit den Daten arbeiten.

Für diese Transformation sollte eine Sequenz von temporalen Fenstern erzeugt werden, die sich nicht überlappen – jedes Fenster weist einen separaten Satz von Daten auf, den Sie gruppieren und aggregieren können. Diese Art von Fenster wird als *rollierendes Fenster* bezeichnet. In einem rollierenden Fenster können Sie die Anzahl eingehender Anrufe gruppiert nach `SwitchNum` abrufen, die für das Land bzw. die Region steht, aus dem bzw. der der Anruf stammt.

1. Fügen Sie die folgende Abfrage in den Abfrage-Editor ein:

    ```SQL
    SELECT 
        System.Timestamp as WindowEnd, SwitchNum, COUNT(*) as CallCount 
    FROM
        CallStream TIMESTAMP BY CallRecTime 
    GROUP BY TUMBLINGWINDOW(s, 5), SwitchNum
    ```

    Diese Abfrage verwendet das Schlüsselwort `Timestamp By` in der Klausel `FROM`, um anzugeben, welches Zeitstempelfeld in der Datenstromeingabe für die Definition des rollierenden Fensters verwendet werden soll. In diesem Fall werden die Daten im Fenster in Segmente unterteilt, und zwar anhand des Felds `CallRecTime` in jedem Datensatz. (Wird kein Feld angegeben, verwendet der Windowingvorgang die Zeit, zu der jedes Ereignis beim Event Hub eingeht.) Weitere Informationen finden Sie unter „Ankunftszeit vs. Anwendungszeit“ in der [Referenz zur Stream Analytics-Abfragesprache](/stream-analytics-query/stream-analytics-query-language-reference). 

    Die Projektion enthält `System.Timestamp`, die einen Zeitstempel für das Ende jedes Fensters zurückgibt. 

    Um anzugeben, dass Sie ein rollierendes Fenster verwenden möchten, verwenden Sie die Funktion [TUMBLINGWINDOW](/stream-analytics-query/tumbling-window-azure-stream-analytics) in der Klausel `GROUP BY`. In der Funktion geben Sie eine Zeiteinheit (von einer Mikrosekunde bis zu einem Tag) und eine Fenstergröße (Anzahl der Einheiten) an. In diesem Beispiel besteht das rollierende Fenster aus 5-Sekunden-Intervallen, sodass Sie alle 5 Sekunden der Anrufe eine Zählung pro Land/Region erhalten.

2. Wählen Sie **Testabfrage** aus. Beachten Sie, dass die Zeitstempel in den Ergebnissen unter **WindowEnd** in 5-Sekunden-Schritten angegeben werden.

### <a name="detect-sim-fraud-using-a-self-join"></a>Erkennen von SIM-Kartenbetrug durch Selbstverknüpfung

In diesem Beispiel kann eine betrügerische Verwendung als Anrufe definiert werden, die vom selben Benutzer stammen, jedoch innerhalb von 5 Sekunden von verschiedenen Standorten aus getätigt werden. Beispielsweise kann derselbe Benutzer nicht gleichzeitig einen legitimen Anruf aus den USA und aus Australien tätigen.

In diesen Fällen können Sie den Datenstrom durch eine Selbstverknüpfung der Streamingdaten basierend auf dem Wert `CallRecTime` mit sich selbst verknüpfen. Sie können dann nach Anrufdatensätzen suchen, bei denen der Wert `CallingIMSI` (die ursprüngliche Anzahl) identisch ist, jedoch nicht der Wert `SwitchNum` (Ursprungsland/-region).

Wenn Sie eine Verknüpfung mit Streamingdaten durchführen, müssen bei der Verknüpfung einige Beschränkungen dazu festgelegt werden, wie stark die übereinstimmenden Zeilen zeitlich getrennt werden können. Wie bereits erwähnt wurde, sind Streamingdaten faktisch endlos. Die Zeitgrenzen für die Beziehung werden in der `ON`-Klausel der Verknüpfung mit der `DATEDIFF`-Funktion angegeben. In diesem Fall basiert die Verknüpfung auf Anrufdaten mit einem Intervall von 5 Sekunden.

1. Fügen Sie die folgende Abfrage in den Abfrage-Editor ein:

    ```SQL
    SELECT System.Timestamp AS WindowEnd, COUNT(*) AS FraudulentCalls
    INTO "MyPBIoutput"
    FROM "CallStream" CS1 TIMESTAMP BY CallRecTime
    JOIN "CallStream" CS2 TIMESTAMP BY CallRecTime
    ON CS1.CallingIMSI = CS2.CallingIMSI
    AND DATEDIFF(ss, CS1, CS2) BETWEEN 1 AND 5
    WHERE CS1.SwitchNum != CS2.SwitchNum
    GROUP BY TumblingWindow(Duration(second, 1))
    ```

    Diese Abfrage ähnelt einer beliebigen SQL-Verknüpfung mit Ausnahme der Funktion `DATEDIFF` in der Verknüpfung. Diese Version von `DATEDIFF` ist spezifisch für Stream Analytics und muss in der `ON...BETWEEN`-Klausel aufgeführt werden. Die Parameter umfassen eine Zeiteinheit (in diesem Beispiel Sekunden) und die Aliase der beiden Quellen für die Verknüpfung. Dies unterscheidet sich von der standardmäßigen SQL-Funktion `DATEDIFF`.

    Die Klausel `WHERE` enthält die Bedingung, die den betrügerischen Anruf kennzeichnet: die ursprünglichen Vermittlungen sind nicht identisch.

2. Wählen Sie **Testabfrage** aus. Überprüfen Sie die Ausgabe, und wählen Sie dann **Abfrage speichern** aus.

## <a name="start-the-job-and-visualize-output"></a>Starten des Auftrags und Visualisieren der Ausgabe

1. Navigieren Sie zum Starten des Auftrags zu seiner **Übersicht**, und wählen Sie **Starten** aus.

2. Wählen Sie als Startzeit für die Auftragsausgabe die Option **Jetzt** und anschließend **Starten**. Der Auftragsstatus kann über die Benachrichtigungsleiste angezeigt werden.

3. Navigieren Sie nach erfolgreichem Abschluss des Auftrags zu [Power BI](https://powerbi.com/), und melden Sie sich mit Ihrem Geschäfts-, Schul- oder Unikonto an. Wenn die Abfrage des Stream Analytics-Auftrags Ergebnisse ausgibt, befindet sich das von Ihnen erstellte Dataset *ASAdataset* auf der Registerkarte **Datasets**.

4. Wählen Sie in Ihrem Power BI-Arbeitsbereich die Option **+ Erstellen** aus, um ein neues Dashboard namens *Fraudulent Calls* zu erstellen.

5. Wählen Sie im oberen Fensterbereich **Bearbeiten** und **Kachel hinzufügen** aus. Wählen Sie dann **Benutzerdefinierte Streamingdaten** und anschließend **Weiter** aus. Wählen Sie unter **Ihre Datasets** das Dataset **ASAdataset** aus. Wählen Sie in der Dropdownliste **Visualisierungstyp** die Option **Karte** aus, und fügen Sie unter **Felder** die Option **fraudulent calls** hinzu. Wählen Sie **Weiter** aus, um einen Namen für die Kachel einzugeben, und wählen Sie dann **Übernehmen** aus, um die Kachel zu erstellen.

   ![Erstellen von Power BI-Dashboard-Kacheln](media/stream-analytics-real-time-fraud-detection/create-power-bi-dashboard-tiles.png)

6. Führen Sie Schritt 5 mit den folgenden Optionen erneut aus:
   * Wählen Sie unter „Visualisierungstyp“ die Option „Liniendiagramm“.
   * Fügen Sie eine Achse hinzu, und wählen Sie **windowend** aus.
   * Fügen Sie einen Wert hinzu, und wählen Sie **fraudulentcalls**.
   * Wählen Sie für **Das anzuzeigende Zeitfenster** die letzten zehn Minuten aus.

7. Nach dem Hinzufügen beider Kacheln sollte Ihr Dashboard wie im folgenden Beispiel aussehen. Beachten Sie Folgendes: Wenn Ihre Event Hub-Absenderanwendung und Ihre Streaming Analytics-Anwendung ausgeführt werden, wird Ihr Power BI-Dashboard regelmäßig aktualisiert, wenn neue Daten eintreffen.

   ![Anzeigen der Ergebnisse im Power BI-Dashboard](media/stream-analytics-real-time-fraud-detection/power-bi-results-dashboard.png)

## <a name="embedding-your-power-bi-dashboard-in-a-web-application"></a>Einbetten Ihres Power BI-Dashboards in eine Webanwendung

Für diesen Teil des Tutorials verwenden Sie eine [ASP.NET](https://asp.net/)-Beispielwebanwendung, die vom Power BI-Team zum Einbetten Ihres Dashboards erstellt wurde. Weitere Informationen zum Einbetten von Dashboards finden Sie im Artikel [Einbetten mit Power BI](/power-bi/developer/embedding).

Navigieren Sie zum Einrichten der Anwendung zum GitHub-Repository [PowerBI-Developer-Samples](https://github.com/Microsoft/PowerBI-Developer-Samples), und befolgen Sie die Anweisungen im Abschnitt **User Owns Data** (Benutzer ist Besitzer der Daten). (Verwenden Sie die Umleitungs- und Startseiten-URLs im Unterabschnitt **integrate-web-app**.) Da wir das Dashboardbeispiel verwenden, können Sie den Beispielcode für **integrate-web-app** aus dem [GitHub-Repository](https://github.com/microsoft/PowerBI-Developer-Samples/tree/master/.NET%20Framework/Embed%20for%20your%20organization/) nutzen.
Nachdem die Anwendung in Ihrem Browser ausgeführt wird, können Sie das Dashboard, das Sie zuvor erstellt haben, mit diesen Schritten in die Webseite einbetten:

1. Wählen Sie die Option **Bei Power BI anmelden**, mit der der Anwendung Zugriff auf die Dashboards in Ihrem Power BI-Konto gewährt wird.

2. Wählen Sie die Schaltfläche **Get Dashboards** (Dashboards abrufen), mit der die Dashboards Ihres Kontos in einer Tabelle angezeigt werden. Suchen Sie nach dem Namen des zuvor erstellten Dashboards (**powerbi-embedded-dashboard**), und kopieren Sie die entsprechende **EmbedUrl**.

3. Fügen Sie die **EmbedUrl** abschließend in das entsprechende Textfeld ein, und wählen Sie **Embed Dashboard** (Dashboard einbetten). Sie können das Dashboard jetzt als eingebettetes Dashboard einer Webanwendung anzeigen.

## <a name="next-steps"></a>Nächste Schritte

In diesem Tutorial haben Sie einen einfachen Stream Analytics-Auftrag erstellt, die eingehenden Daten analysiert und Ergebnisse in einem Power BI-Dashboard dargestellt. Weitere Informationen zu Stream Analytics-Aufträgen erhalten Sie im nächsten Tutorial:

> [!div class="nextstepaction"]
> [Ausführen von Azure Functions in Azure Stream Analytics-Aufträgen](stream-analytics-with-azure-functions.md)
