---
title: Konfigurieren von Microsoft Connected Cache für Device Update for Azure IoT Hub | Microsoft-Dokumentation
titleSuffix: Device Update for Azure IoT Hub
description: Übersicht über Microsoft Connected Cache für Device Update for Azure IoT Hub
author: andyriv
ms.author: andyriv
ms.date: 2/16/2021
ms.topic: conceptual
ms.service: iot-hub-device-update
ms.openlocfilehash: 2903407f88b57a7be948cdeb0610e6d65df975b0
ms.sourcegitcommit: 910a1a38711966cb171050db245fc3b22abc8c5f
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/19/2021
ms.locfileid: "101660416"
---
# <a name="configure-microsoft-connected-cache-for-device-update-for-azure-iot-hub"></a>Konfigurieren von Microsoft Connected Cache für Device Update for Azure IoT Hub

Microsoft Connected Cache wird als Azure IoT Edge-Modul für Azure IoT Edge-Gateways bereitgestellt. Wie andere Azure IoT Edge-Module werden MCC-Modulbereitstellungs-Umgebungsvariablen und Containererstellungsoptionen verwendet, um Microsoft Connected Cache-Module zu konfigurieren.  In diesem Abschnitt werden die Umgebungsvariablen und Optionen zum Erstellen von Containern definiert, die ein Kunde benötigt, um das Microsoft Connected Cache-Modul für die Verwendung durch Device Update for Azure IoT Hub erfolgreich bereitzustellen.

## <a name="microsoft-connected-cache-azure-iot-edge-module-deployment-details"></a>Details zur Bereitstellung des Microsoft Connected Cache Azure IoT Edge-Moduls

Die Benennung des Microsoft Connected Cache-Moduls liegt im Ermessen des Administrators. Es sind keine anderen Modul- oder Dienstinteraktionen vorhanden, die den Namen des Moduls für Kommunikation verwenden. Außerdem ist Beziehung der über- und untergeordneten Microsoft Connected Cache-Server nicht von diesem Modulnamen abhängig, sondern vom FQDN oder der IP-Adresse des Azure IoT Edge-Gateways, das wie zuvor beschrieben konfiguriert wurde.

Die Umgebungsvariablen des Microsoft Connected Cache Azure IoT Edge-Moduls werden verwendet, um grundlegende Modulidentitätsinformationen und funktionale Moduleinstellungen an den Container zu übergeben.

| Variablenname                 | Wertformat                           | Erforderlich/Optional | Funktionalität                                    |
| ----------------------------- | ---------------------------------------| ----------------- | ------------------------------------------------ |
| CUSTOMER_ID                   | GUID der Azure-Abonnement-ID             | Erforderlich          | Dies ist der Schlüssel des Kunden, der sichere<br>Authentifizierung des Cacheknotens für Übermittlungsoptimierungsdienste<br>bereitstellt. Erforderlich, damit das Modul funktioniert. |
| CACHE_NODE_ID                 | GUID der Cacheknoten-ID                     | Erforderlich          | Identifiziert den Microsoft Connected Cache-Knoten<br>für Übermittlungsoptimierungsdienste eindeutig. Erforderlich,<br> damit das Modul funktioniert. |
| CUSTOMER_KEY                  | GUID des Kundenschlüssels                     | Erforderlich          | Dies ist der Schlüssel des Kunden, der sichere<br>Authentifizierung des Cacheknotens für Übermittlungsoptimierungsdienste bereitstellt.<br>Erforderlich, damit das Modul funktioniert.|
| STORAGE_ *N* _SIZE_GB           | Dabei ist N die erforderliche Anzahl GB.   | Erforderlich          | Geben Sie bis zu neun Laufwerke zum Zwischenspeichern von Inhalten an, und geben Sie<br>den maximalen Speicherplatz in GB an, der für Inhalte auf den einzelnen Cachelaufwerken zugeordnet werden soll. Beispiele:<br>STORAGE_1_SIZE_GB = 150<br>STORAGE_2_SIZE_GB = 50<br>Die Nummer des Laufwerks muss den angegebenen Cachelaufwerk-Bindungswerten entsprechen, die<br>im Wert der Containererstellungsoption MicrosoftConnectedCache *N* angegeben werden.|
| UPSTREAM_HOST                 | FQDN/IP                                | Optional          | Dieser Wert kann einen Microsoft Connected<br>Cache-Upstreamknoten angeben, der als Proxy fungiert, wenn der Connected Cache-Knoten<br> vom Internet getrennt ist. Diese Einstellung wird zur Unterstützung des<br> Nested IoT-Szenarios verwendet.<br>**Hinweis:** Microsoft Connected Cache lauscht am HTTP-Standardport 80.|
| UPSTREAM_PROXY                | FQDN/IP:PORT                           | Optional          | Der ausgehende Internetproxy.<br>Dies kann auch der OT DMZ-Proxy sein, wenn es sich um ein ISA 95-Netzwerk handelt. |
| CACHEABLE_CUSTOM_ *N* _HOST     | HOST/IP<br>FQDN                        | Optional          | Erforderlich zur Unterstützung benutzerdefinierter Paketrepositorys.<br>Repositorys können lokal oder im Internet gehostet werden.<br>Es gibt keine Obergrenze für die Anzahl benutzerdefinierter Hosts, die konfiguriert werden können.<br><br>Beispiele:<br>Name = CACHEABLE_CUSTOM_1_HOST Value = packages.foo.com<br> Name = CACHEABLE_CUSTOM_2_HOST Value = packages.bar.com    |
| CACHEABLE_CUSTOM_ *N* _CANONICAL| Alias                                  | Optional          | Erforderlich zur Unterstützung benutzerdefinierter Paketrepositorys.<br>Dieser Wert kann als Alias verwendet werden und wird vom Cacheserver zum Verweisen auf<br>verschiedene DNS-Namen verwendet. Der Hostname des Repositoryinhalts kann z. B. packages.foo.com lauten,<br>für verschiedene Regionen kann dem Hostnamen jedoch ein zusätzliches Präfix hinzugefügt werden,<br>etwa westuscdn.packages.foo.com und eastuscdn.packages.foo.com.<br>Wenn Sie den kanonischen Alias festlegen, stellen Sie sicher, dass Inhalte nicht dupliziert werden,<br>wenn der Inhalt von demselben Host, aber aus unterschiedlichen CDN-Quellen stammt.<br>Das Format des kanonischen Werts ist nicht wichtig, muss jedoch für den Host eindeutig sein.<br>Möglicherweise ist es am einfachsten, den Wert so festzulegen, dass er dem Hostwert entspricht.<br><br>Beispiele auf der Grundlage von benutzerdefinierten Hostbeispielen oben:<br>Name = CACHEABLE_CUSTOM_1_CANONICAL Value = foopackages<br> Name = CACHEABLE_CUSTOM_2_CANONICAL Value = packages.bar.com  |
| IS_SUMMARY_PUBLIC             | „true“ oder „false“                          | Optional          | Ermöglicht das Anzeigen des Zusammenfassungsberichts im lokalen Netzwerk oder im Internet.<br>Die Verwendung eines API-Schlüssels (wird später erläutert) ist erforderlich, um den Zusammenfassungsbericht anzuzeigen, wenn der Wert auf TRUE festgelegt ist. |
| IS_SUMMARY_ACCESS_UNRESTRICTED| „true“ oder „false“                          | Optional          | Ermöglicht das Anzeigen des Zusammenfassungsberichts im lokalen Netzwerk oder im Internet, ohne<br>den API-Schlüssel von einem beliebigen Gerät im Netzwerk zu verwenden. Verwenden Sie diese Option, wenn der Zugriff<br>auf die Anzeige von Zusammenfassungsdaten des Cacheservers über den Browser nicht gesperrt werden soll. |
            
## <a name="microsoft-connected-cache-azure-iot-edge-module-container-create-options"></a>Containererstellungsoptionen des Microsoft Connected Cache Azure IoT Edge-Moduls

Containererstellungsoptionen für die MCC-Modulbereitstellung bieten Kontrolle über die Einstellungen im Zusammenhang mit dem vom MCC-Modul verwendeten Speicher und den Ports. Dies ist die Liste der erforderlichen, vom Container erstellten Variablen, die zum Bereitstellen von MCC verwendet werden.

### <a name="container-to-host-os-drive-mappings"></a>Container zum Hosten von Betriebssystem-Laufwerkzuordnungen

Erforderlich, um den Speicherort des Containers dem Speicherort auf dem Datenträger zuzuordnen. Bis zu neun Speicherorte können angegeben werden.

>[!Note]
>Die Nummer des Laufwerks muss mit den Bindungswerten des Cachelaufwerks übereinstimmen, die im Wert ```/MicrosoftConnectedCache*N*/:/nginx/cache*N*/``` der Umgebungsvariablen STORAGE_ *N* _SIZE_GB Wert angegeben werden.

### <a name="container-to-host-tcp-port-mappings"></a>Container zum Hosten von TCP-Portzuordnungen

Mit dieser Option wird der HTTP-Port des externen Computers angegeben, auf dem MCC auf Inhaltsanforderungen lauscht. Der Standard-HostPort ist Port 80, und andere Ports werden zurzeit nicht unterstützt, da der ADU-Client aktuell Anforderungen an Port 80 übermittelt. TCP-Port 8081 ist der interne Containerport, an dem MCC lauscht. Dieser Port kann nicht geändert werden.

```markdown
8081/tcp": [
   {
       "HostPort": "80"
   }
]
```

### <a name="container-service-tcp-port-mappings"></a>TCP-Portzuordnungen des Containerdiensts

Das Microsoft Connected Cache-Modul verfügt über einen .NET Core-Dienst, der von der Caching-Engine für verschiedene Funktionen verwendet wird.

>[!Note]
>Um Azure IoT Nested Edge zu unterstützen, darf der HostPort nicht auf 5000 festgelegt werden, da das Registrierungsproxymodul bereits auf Hostport 5000 lauscht.

```markdown
5000/tcp": [
   {
       "HostPort": "5001"
   }
]
```

## <a name="microsoft-connected-cache-summary-report"></a>Microsoft Connected Cache-Zusammenfassungsbericht

Der Zusammenfassungsbericht ist derzeit die einzige Möglichkeit für Kunden, Cachedaten für die Microsoft Connected Cache-Instanzen anzuzeigen, die auf Azure IoT Edge-Gateways bereitgestellt werden. Der Bericht wird in Intervallen von 15 Sekunden erstellt und enthält gemittelte Statistiken für diesen Zeitraum sowie aggregierte Statistiken für die Lebensdauer des Moduls. Die wichtigsten Statistiken, an denen Kunden interessiert sind, sind diese:

* **hitBytes**: Dies ist die Summe der übermittelten Bytes, die direkt aus dem Cache stammen.
* **missBytes**: Dies ist die Summe der übermittelten Bytes, die Microsoft Connected Cache aus dem CDN herunterladen musste, um den Cache anzuzeigen.
* **eggressBytes**: Dies ist die Summe der hitBytes und missBytes und die Gesamtzahl der Bytes, die an Clients übermittelt werden.
* **hitRatioBytes**: Dies ist das Verhältnis von hitBytes zu egressBytes.  Wenn 100 % der in einem Zeitraum übermittelten eggressBytes gleich den hitBytes sind, wäre dieserWert z. B. 1.

Der Zusammenfassungsbericht ist unter `http://<FQDN/IP of Azure IoT Edge Gateway hosting MCC>:5001/summary` verfügbar (weitere Informationen zur Sichtbarkeit dieses Berichts finden Sie in den Details zur Umgebungsvariablen weiter unten).
