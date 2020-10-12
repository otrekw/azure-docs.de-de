---
title: Systemanforderungen
description: Listet die Systemanforderungen für Azure Remote Rendering auf.
author: florianborn71
ms.author: flborn
ms.date: 02/03/2020
ms.topic: article
ms.openlocfilehash: 31fde0c7af652bc50eb5f06743c5dd5807a1762e
ms.sourcegitcommit: 32c521a2ef396d121e71ba682e098092ac673b30
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/25/2020
ms.locfileid: "91323724"
---
# <a name="system-requirements"></a>Systemanforderungen

> [!IMPORTANT]
> **Azure Remote Rendering** befindet sich derzeit in der öffentlichen Vorschau.
> Diese Vorschauversion wird ohne Vereinbarung zum Servicelevel bereitgestellt und ist nicht für Produktionsworkloads vorgesehen. Manche Features werden möglicherweise nicht unterstützt oder sind nur eingeschränkt verwendbar. Weitere Informationen finden Sie unter [Zusätzliche Nutzungsbestimmungen für Microsoft Azure-Vorschauen](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).

In diesem Kapitel werden die Mindestsystemanforderungen für das Arbeiten mit *Azure Remote Rendering* (ARR) aufgeführt.

## <a name="development-pc"></a>Entwicklungs-PC

* Windows 10, Version 1903 oder höher.
* Aktuelle Grafiktreiber.
* Optional: [H265-Hardwarevideodecoder](https://www.microsoft.com/p/hevc-video-extensions/9nmzlz57r3t7), wenn Sie eine lokale Vorschau von remote gerendertem Inhalt verwenden möchten (z. B. in Unity).

> [!IMPORTANT]
> Windows Update stellt nicht immer die neuesten GPU-Treiber bereit. Überprüfen Sie die Website Ihres GPU-Herstellers auf die neuesten Treiber:
>
> * [AMD-Treiber](https://www.amd.com/en/support)
> * [Intel-Treiber](https://www.intel.com/content/www/us/en/support/detect.html)
> * [NVIDIA-Treiber](https://www.nvidia.com/Download/index.aspx)

In der folgenden Tabelle wird aufgeführt, welche GPUs H265-Hardwarevideodecodierung unterstützen.

| GPU-Hersteller | Unterstützte Modelle |
|-----------|:-----------|
| NVIDIA | Überprüfen Sie die **NVDEC-Unterstützungsmatrix** [unten auf dieser Seite](https://developer.nvidia.com/video-encode-decode-gpu-support-matrix). Ihre GPU benötigt die Angabe „Ja“ in der Spalte **H.265 4:2:0 8-Bit**. |
| AMD | GPUs mit mindestens Version 6 des [Unified Video Decoder](https://en.wikipedia.org/wiki/Unified_Video_Decoder#UVD_6) von AMD. |
| Intel | Skylake und neuere CPUs |

Obwohl der richtige H265-Codec möglicherweise installiert ist, können Sicherheitseigenschaften für die Codec-DLLs Fehler bei der Codecinitialisierung verursachen. Im [Leitfaden zur Problembehandlung](../resources/troubleshoot.md#h265-codec-not-available) werden Schritte zum Beheben dieses Problems beschrieben. Das DLL-Problem kann nur auftreten, wenn der Dienst in einer Desktopanwendung verwendet wird, z. B. in Unity.

## <a name="devices"></a>Geräte

Azure Remote Rendering unterstützt derzeit nur **HoloLens 2** und Windows-Desktops als Zielgerät. Weitere Informationen finden Sie im Abschnitt [Plattformeinschränkungen](../reference/limits.md#platform-limitations).

Es ist wichtig, den neuesten HEVC-Codec zu verwenden, da neuere Versionen beträchtliche Verbesserungen bei der Latenz aufweisen. So überprüfen Sie, welche Version auf Ihrem Gerät installiert ist:

1. Starten Sie den **Microsoft Store**.
1. Klicken Sie oben rechts auf die Schaltfläche mit den Auslassungspunkten ( **„...“** ).
1. Wählen Sie **Downloads und Updates** aus.
1. Suchen Sie in der Liste nach **HEVC-Videoerweiterungen vom Gerätehersteller**. Wenn dieser Punkt nicht unter Updates aufgeführt ist, ist die aktuellste Version bereits installiert.
1. Stellen Sie sicher, dass der aufgeführte Codec mindestens Version **1.0.21821.0** aufweist.
1. Klicken Sie auf die Schaltfläche **Updates abrufen**, und warten Sie, bis die Updates installiert wurden.

## <a name="network"></a>Netzwerk

Eine stabile Netzwerkverbindung mit geringer Latenz ist wichtig für eine gute Benutzererfahrung.

Weitere Informationen dazu finden Sie im speziellen Kapitel zu den [Netzwerkanforderungen](../reference/network-requirements.md).

Informationen zum Behandeln von Netzwerkproblemen finden Sie im [Problembehandlungsleitfaden](../resources/troubleshoot.md#unstable-holograms).

### <a name="network-ports"></a>Netzwerkports

Stellen Sie sicher, dass Ihre Firewalls (auf dem Gerät, in Routern usw.) die folgenden Ports nicht blockieren:

| Port              | Protocol | Allow    | Beschreibung |
|-------------------|----------|----------|-------------|
| 50051             | TCP      | Ausgehend | Anfängliche Verbindung (HTTP-Handshake) |
| 8266              | UDP      | Ausgehend | Datenübertragung |
| 5000, 5433, 8443  | TCP      | Ausgehend | Erforderlich für das [ArrInspector-Tool](../resources/tools/arr-inspector.md)|


## <a name="software"></a>Software

Die folgende Software muss installiert sein:

* Aktuellste Version von **Visual Studio 2019** [(Download)](https://visualstudio.microsoft.com/vs/older-downloads/)
* [Visual Studio-Tools für Mixed Reality](https://docs.microsoft.com/windows/mixed-reality/install-the-tools). Insbesondere sind die folgenden *Workloadinstallationen* obligatorisch:
  * **Desktopentwicklung mit C++**
  * **Entwicklung für die universelle Windows-Plattform (UWP)**
* **Windows SDK 10.0.18362.0** [(Download)](https://developer.microsoft.com/windows/downloads/windows-10-sdk)
* **GIT** [(Download)](https://git-scm.com/downloads)
* Optional: Um den Videostream vom Server auf einem Desktop-PC anzuzeigen, benötigen Sie die **HEVC-Videoerweiterungen** [(Microsoft Store-Link)](https://www.microsoft.com/p/hevc-video-extensions/9nmzlz57r3t7). Stellen Sie sicher, dass die neueste Version installiert ist, indem Sie im Store nach Updates suchen.

## <a name="unity"></a>Unity

Installieren Sie für die Entwicklung mit Unity

* Unity 2019.3.1 [(Download)](https://unity3d.com/get-unity/download)
* Installieren Sie in Unity diese Module:
  * **UWP**: Buildunterstützung für Universelle Windows-Plattform
  * **IL2CPP**: Windows-Buildunterstützung (IL2CPP)

## <a name="next-steps"></a>Nächste Schritte

* [Schnellstart: Rendern eines Modells mit Unity](../quickstarts/render-model.md)
