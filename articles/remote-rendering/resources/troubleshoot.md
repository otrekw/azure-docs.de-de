---
title: Problembehandlung
description: Informationen zum Troubleshooting bei Azure Remote Rendering
author: florianborn71
ms.author: flborn
ms.date: 02/25/2020
ms.topic: troubleshooting
ms.openlocfilehash: c1b807c6e4fa269ac2ab8d7eacd3ca1d4f81a1ca
ms.sourcegitcommit: e0330ef620103256d39ca1426f09dd5bb39cd075
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/05/2020
ms.locfileid: "82792614"
---
# <a name="troubleshoot"></a>Problembehandlung

Auf dieser Seite finden Sie allgemeine Probleme, die Azure Remote Rendering beeinträchtigen, sowie Möglichkeiten, diese zu beheben.

## <a name="client-cant-connect-to-server"></a>Der Client kann keine Verbindung mit dem Server herstellen.

Stellen Sie sicher, dass Ihre Firewalls (auf dem Gerät, in Routern usw.) die folgenden Ports nicht blockieren:

* **50051 (TCP):** für die erste Verbindung (HTTP-Handshake) erforderlich
* **8266 (TCP + UDP):** für die Datenübertragung erforderlich
* **5000 (TCP)** , **5433 (TCP)** , **8443 (TCP)** : für [ArrInspector](tools/arr-inspector.md) erforderlich

## <a name="error-disconnected-videoformatnotavailable"></a>Fehler „Getrennt: VideoFormatNotAvailable“

Vergewissern Sie sich, dass Ihre GPU-Hardware Videodecodierung unterstützt. Weitere Informationen finden Sie unter [Entwicklungs-PC](../overview/system-requirements.md#development-pc).

Wenn Sie an einem Laptop mit zwei GPUs arbeiten, kann es vorkommen, dass die GPU, die Sie standardmäßig für Ausführungen verwenden, keine Funktion für die Hardwarevideodecodierung bietet. Versuchen Sie in diesem Fall, die Verwendung der anderen GPU durch die App zu erzwingen. Dies ist häufig in den Einstellungen des GPU-Treibers möglich.

## <a name="h265-codec-not-available"></a>H.265-Codec nicht verfügbar

Der Server kann aus zwei Gründen die Verbindung mit dem Fehler **Codec nicht verfügbar** ablehnen.

**Der H.265-Codec ist nicht installiert:**

Stellen Sie zunächst sicher, dass die **HEVC-Videoerweiterungen** wie im Abschnitt [Software](../overview/system-requirements.md#software) der Systemanforderungen beschrieben installiert wurde.

Wenn weiterhin Probleme auftreten, stellen Sie sicher, dass Ihre Grafikkarte H.265 unterstützt und der neueste Grafiktreiber installiert ist. Herstellerspezifische Informationen finden Sie im Abschnitt [Entwicklungs-PC](../overview/system-requirements.md#development-pc) der Systemanforderungen.

**Der Codec ist installiert, kann aber nicht verwendet werden:**

Der Grund für dieses Problem ist eine falsche Sicherheitseinstellung in den DLLs. Dieses Problem tritt beim Versuch, mit H.265 codierte Videos anzusehen, nicht auf. Das Problem wird durch eine Neuinstallation des Codecs auch nicht behoben. Führen Sie stattdessen die folgenden Schritte aus:

1. Öffnen Sie eine **PowerShell-Instanz mit Administratorrechten**, und führen Sie folgenden Befehl aus:

    ```PowerShell
    Get-AppxPackage -Name Microsoft.HEVCVideoExtension
    ```
  
    Dieser Befehl sollte die `InstallLocation` des Codecs ausgeben, z. B.:
  
    ```cmd
    InstallLocation   : C:\Program Files\WindowsApps\Microsoft.HEVCVideoExtension_1.0.23254.0_x64__5wasdgertewe
    ```

1. Öffnen Sie diesen Ordner in Windows-Explorer.
1. Es sollten die Unterordner **x86** und **x64** vorhanden sein. Klicken Sie mit der rechten Maustaste auf einen der Ordner, und wählen Sie **Eigenschaften** aus.
    1. Wählen Sie die Registerkarte **Sicherheit** aus, und klicken Sie auf die Schaltfläche **Erweitert**.
    1. Klicken Sie für **Besitzer** auf **Ändern**.
    1. Geben Sie in das Textfeld **Administratoren** ein.
    1. Klicken Sie auf **Namen überprüfen** und auf **OK**.
1. Wiederholen Sie die angegebenen Schritte für den anderen Ordner.
1. Wiederholen Sie außerdem die angegebenen Schritte für jede DLL-Datei in beiden Ordnern. Insgesamt sollten vier DLL-Dateien vorhanden sein.

Um zu überprüfen, ob die Einstellungen jetzt richtig sind, gehen Sie für jede der vier DLL-Dateien folgendermaßen vor:

1. Wählen Sie **Eigenschaften > Sicherheit > Bearbeiten** aus.
1. Gehen Sie die Liste aller **Gruppen/Benutzer** durch, und stellen Sie sicher, dass jeder Eintrag über die Berechtigung **Lesen und ausführen** verfügt (das Kontrollkästchen in der Spalte **Zulassen** muss aktiviert sein).

## <a name="low-video-quality"></a>Geringe Videoqualität

Die Videoqualität kann durch die Netzwerkqualität oder einen fehlenden H.265-Videocodec beeinträchtigt werden.

* Weitere Informationen finden Sie in den Schritten zum [Identifizieren von Netzwerkproblemen](#unstable-holograms).
* Informationen zum Installieren des aktuellen Grafiktreibers finden Sie in den [Systemanforderungen](../overview/system-requirements.md#development-pc).

## <a name="video-recorded-with-mrc-does-not-reflect-the-quality-of-the-live-experience"></a>Mit MRC aufgezeichnetes Video spiegelt nicht die Qualität des Liveerlebnisses wider

Ein Video kann mit Hololens mithilfe von [Mixed Reality Capture (MRC)](https://docs.microsoft.com/windows/mixed-reality/mixed-reality-capture-for-developers) aufgezeichnet werden. Das resultierende Video weist jedoch aus zwei Gründen eine schlechtere Qualität als das Liveerlebnis auf:
* Die Videobildfrequenz ist auf 30 Hz begrenzt, im Gegensatz zu 60 Hz beim Liveerlebnis.
* Die Videobilder durchlaufen nicht den Verarbeitungsschritt [Late Stage Reprojection](../overview/features/late-stage-reprojection.md), daher wirkt das Video weniger flüssig.

Beides sind inhärente Einschränkungen der Aufzeichnungstechnik.

## <a name="black-screen-after-successful-model-loading"></a>Schwarzer Bildschirm nach erfolgreichem Laden des Modells

Wenn Sie mit der Renderingruntime verbunden sind und erfolgreich ein Modell geladen haben, aber danach nur ein schwarzer Bildschirm angezeigt wird, kann dies verschiedene Gründe haben.

Es wird empfohlen, die folgenden Tests auszuführen, bevor Sie eine ausführlichere Analyse durchführen:

* Ist der H.265-Codec installiert? Obwohl ein Fallback auf den H.264-Codec erfolgen sollte, wurden bereits Fälle beobachtet, in denen dieses Fallback nicht ordnungsgemäß funktionierte. Informationen zum Installieren des aktuellen Grafiktreibers finden Sie in den [Systemanforderungen](../overview/system-requirements.md#development-pc).
* Wenn Sie ein Unity-Projekt verwenden, schließen Sie Unity, löschen Sie die temporären Ordner *library* und *obj* im Projektverzeichnis, laden Sie das Projekt erneut, und erstellen Sie es neu. In einigen Fällen konnte das Beispiel aufgrund von zwischengespeicherten Daten nicht ordnungsgemäß ausgeführt werden, ohne dass ein offensichtlicher Grund vorlag.

Wenn diese beiden Schritte nicht hilfreich waren, müssen Sie herausfinden, ob der Client Videoframes empfängt. Dies kann programmgesteuert abgefragt werden, wie im Kapitel zu [serverseitigen Leistungsabfragen](../overview/features/performance-queries.md) erläutert. Das `FrameStatistics struct` weist einen Member auf, der angibt, wie viele Videoframes empfangen wurden. Wenn diese Zahl größer als 0 ist und im Lauf der Zeit zunimmt, empfängt der Client Videoframes vom Server. Es muss sich demzufolge um ein Problem auf der Clientseite handeln.

### <a name="common-client-side-issues"></a>Häufige clientseitige Fehler

**Das Modell überschreitet die Grenzwerte des ausgewählten virtuellen Computers, insbesondere die maximale Anzahl von Polygonen:**

Weitere Informationen finden Sie bei den spezifischen [VM-Größenbeschränkungen](../reference/limits.md#overall-number-of-polygons).

**Das Modell befindet sich nicht im Ansichtsfrustum:**

In vielen Fällen wird das Modell ordnungsgemäß dargestellt, befindet sich jedoch außerhalb des Kamerafrustums. Ein häufiger Grund dafür ist, dass das Modell mit einem Pivot deutlich außerhalb des Mittelpunkts exportiert wurde, sodass es von der Entfernungsclippingebene der Kamera abgeschnitten wird. Es hilft, den Begrenzungsrahmen des Modells programmgesteuert abzufragen und mit Unity als Linienrahmen zu visualisieren oder die entsprechenden Werte im Debugprotokoll auszugeben.

Der Konvertierungsprozess generiert außerdem neben dem konvertierten Modell eine [JSON-Ausgabedatei](../how-tos/conversion/get-information.md). Beim Debuggen von Problemen mit der Modellpositionierung ist es sinnvoll, einen Blick auf den `boundingBox`-Eintrag im [outputStatistics-Abschnitt](../how-tos/conversion/get-information.md#the-outputstatistics-section) zu werfen:

```JSON
{
    ...
    "outputStatistics": {
        ...
        "boundingBox": {
            "min": [
                -43.52,
                -61.775,
                -79.6416
            ],
            "max": [
                43.52,
                61.775,
                79.6416
            ]
        }
    }
}
```

Das Begrenzungsfeld wird als `min`- und `max`-Position in Metern im 3D-Raum beschrieben. Eine Koordinate von 1000,0 bedeutet daher eine Entfernung von einem Kilometer vom Ursprung.

Bei diesem Begrenzungsfeld können zwei Probleme auftreten, die zu unsichtbarer Geometrie führen:
* **Das Feld kann stark dezentriert sein**, sodass das Objekt aufgrund von Verdeckung durch die entfernte Ebene in seiner Gesamtheit beschnitten wird. Die `boundingBox`-Werte für diesen Fall würden etwa so aussehen: `min = [-2000, -5,-5], max = [-1990, 5,5]`, mit einem großen Offset auf der X-Achse als Beispiel für diesen Fall. Um diese Art Problem zu lösen, aktivieren Sie die Option `recenterToOrigin` in der [Konfiguration der Modellkonvertierung](../how-tos/conversion/configure-model-conversion.md).
* **Das Feld kann zentriert sein, ist aber um Größenordnungen zu groß**. Das bedeutet, dass die Geometrie des Modells in allen Richtungen abgeschnitten wird, obwohl die Kamera in der Mitte es Modells startet. Typische `boundingBox`-Werte für diesen Fall sehen etwa so aus: `min = [-1000,-1000,-1000], max = [1000,1000,1000]`. Der Grund für diese Art Problem ist normalerweise ein nicht angepasster Einheitenbereich. Geben Sie zum Ausgleich einen [Skalierungswert für die Konvertierung](../how-tos/conversion/configure-model-conversion.md#geometry-parameters) an, oder nehmen Sie ein Markup des Quellmodells mit den richtigen Einheiten vor. Skalierung kann auch auf den Stammknoten angewendet werden, wenn das Modell zur Laufzeit geladen wird.

**Die Unity-Renderpipeline enthält nicht die Renderhooks:**

Azure Remote Rendering erstellt Hooks in der Unity-Renderpipeline, um die Framezusammenstellung mit dem Video und die Neuprojektion durchzuführen. Um zu überprüfen, ob diese Hooks vorhanden sind, öffnen Sie das Menü *Window > Analysis > Frame debugger* (Fenster > Analyse > Framedebugger). Aktivieren Sie ihn, und stellen Sie sicher, dass zwei Einträge für `HolographicRemotingCallbackPass` in der Pipeline vorhanden sind:

![Unity-Framedebugger](./media/troubleshoot-unity-pipeline.png)

## <a name="unity-code-using-the-remote-rendering-api-doesnt-compile"></a>Unity-Code, der die Remote Rendering-API verwendet, wird nicht kompiliert.

### <a name="use-debug-when-compiling-for-unity-editor"></a>Verwenden von „Debuggen“ beim Kompilieren für den Unity-Editor

Stellen Sie den *Buildtyp* der Unity-Lösung auf **Debug** um. Beim Testen von ARR im Unity-Editor ist die Definition `UNITY_EDITOR` nur in Debugbuilds verfügbar. Beachten Sie, dass dies nicht mit dem Buildtyp in Zusammenhang steht, der für [bereitgestellte Anwendungen](../quickstarts/deploy-to-hololens.md) verwendet wird. Hierbei sollten Sie Releasebuilds verwenden.

### <a name="compile-failures-when-compiling-unity-samples-for-hololens-2"></a>Kompilierungsfehler beim Kompilieren von Unity-Beispielen für HoloLens 2

Bei dem Versuch, Unity-Beispiele ( Schnellstart, ShowCaseApp, ...) für HoloLens 2 zu kompilieren, sind fälschlicherweise Fehler aufgetreten. Visual Studio beanstandet, dass einige Dateien nicht kopiert werden können, obwohl sie vorhanden sind. Wenn Sie auf dieses Problem treffen:
* Entfernen Sie alle temporären Unity-Dateien aus dem Projekt, und versuchen Sie es erneut.
* Stellen Sie sicher, dass sich die Projekte in einem Verzeichnis auf dem Datenträger mit einem relativ kurzen Pfad befinden, da es beim Kopieren gelegentlich zu Problemen mit langen Dateinamen zu kommen scheint.
* Wenn das nicht hilft, könnte es sein, dass zwischen MS Sense und dem Kopierschritt ein Konflikt besteht. Führen Sie diesen Registrierungsbefehl über die Befehlszeile aus (erfordert Administratorrechte), um eine Ausnahme einzurichten:
    ```cmd
    reg.exe ADD "HKLM\SOFTWARE\Policies\Microsoft\Windows Advanced Threat Protection" /v groupIds /t REG_SZ /d "Unity”
    ```
    
## <a name="unstable-holograms"></a>Instabile Hologramme

Wenn bei Kopfbewegungen auch gerenderte Objekte bewegt werden, liegen möglicherweise Probleme mit der *Late Stage Reprojection* (LSR) vor. Hinweise zur Vorgehensweise in einer solchen Situation finden Sie im Abschnitt zur [Late Stage Reprojection](../overview/features/late-stage-reprojection.md).

Ein weiterer Grund für instabile Hologramme (wabernde, verzerrte, zitternde oder springende Hologramme) kann eine schlechte Netzwerkkonnektivität sein, insbesondere unzureichende Netzwerkbandbreite oder zu hohe Latenz. Ein guter Indikator für die Qualität der Netzwerkverbindung ist der [Leistungsstatistikwert](../overview/features/performance-queries.md) `ARRServiceStats.VideoFramesReused`. Wiederverwendete Frames deuten auf Situationen hin, in denen ein altes Videoframe auf der Clientseite wiederverwendet werden musste, da kein neues Videoframe verfügbar war – beispielsweise aufgrund von Paketverlusten oder von Schwankungen der Netzwerklatenz. Wenn `ARRServiceStats.VideoFramesReused` häufig größer als null ist, deutet dies auf ein Netzwerkproblem hin.

Ein anderer Wert, der untersucht werden kann, ist `ARRServiceStats.LatencyPoseToReceiveAvg`. Er sollte konstant weniger als 100 ms betragen. Wenn höhere Werte angezeigt werden, bedeutet dies, dass Sie mit einem zu weit entfernten Rechenzentrum verbunden sind.

Eine Liste der möglichen Entschärfungen finden Sie in den [Richtlinien für die Netzwerkkonnektivität](../reference/network-requirements.md#guidelines-for-network-connectivity).

## <a name="next-steps"></a>Nächste Schritte

* [Systemanforderungen](../overview/system-requirements.md)
* [Netzwerkanforderungen](../reference/network-requirements.md)
