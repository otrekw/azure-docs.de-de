---
title: Konfigurieren von Containern – Textanalyse
titleSuffix: Azure Cognitive Services
description: Die Textanalyse stellt für jeden Container ein allgemeines Konfigurationsframework bereit, sodass Sie ganz einfach Aspekte wie Speicher, Protokollierung und Telemetrie sowie Sicherheitseinstellungen für Ihre Container konfigurieren und verwalten können.
services: cognitive-services
author: aahill
manager: nitinme
ms.custom: seodec18
ms.service: cognitive-services
ms.subservice: text-analytics
ms.topic: conceptual
ms.date: 04/01/2020
ms.author: aahi
ms.openlocfilehash: f6a1bc652125990a7daf3414895f34b95c544912
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/09/2020
ms.locfileid: "83590553"
---
# <a name="configure-text-analytics-docker-containers"></a>Konfigurieren von Docker-Containern für die Textanalyse

Die Textanalyse stellt für jeden Container ein allgemeines Konfigurationsframework bereit, sodass Sie ganz einfach Aspekte wie Speicher, Protokollierung und Telemetrie sowie Sicherheitseinstellungen für Ihre Container konfigurieren und verwalten können. Mehrere [Beispiele für docker run-Befehle](how-tos/text-analytics-how-to-install-containers.md#run-the-container-with-docker-run) sind ebenfalls verfügbar.

## <a name="configuration-settings"></a>Konfigurationseinstellungen

[!INCLUDE [Container shared configuration settings table](../../../includes/cognitive-services-containers-configuration-shared-settings-table.md)]

> [!IMPORTANT]
> Die Einstellungen [`ApiKey`](#apikey-configuration-setting), [`Billing`](#billing-configuration-setting) und [`Eula`](#eula-setting) werden gemeinsam verwendet, und Sie müssen gültige Werte für alle drei angeben, da der Container andernfalls nicht startet. Weitere Informationen zum Instanziieren eines Containers mithilfe dieser Konfigurationseinstellungen finden Sie unter [Abrechnung](how-tos/text-analytics-how-to-install-containers.md#billing).

## <a name="apikey-configuration-setting"></a>Konfigurationseinstellung „ApiKey“

Die `ApiKey`-Einstellung gibt den Schlüssel der Azure-Ressourcen an, mit dem die Abrechnungsinformationen für den Container verfolgt werden. Sie müssen einen Wert für „ApiKey“ angeben. Bei diesem Wert muss es sich um einen gültigen Schlüssel für die Ressource vom Typ _Textanalyse_ handeln, die für die Konfigurationseinstellung [`Billing`](#billing-configuration-setting) angegeben wurde.

Diese Einstellung finden Sie hier:

* Azure-Portal: Ressourcenverwaltung der **Textanalyse** (unter **Schlüssel**)

## <a name="applicationinsights-setting"></a>ApplicationInsights-Einstellung

[!INCLUDE [Container shared configuration ApplicationInsights settings](../../../includes/cognitive-services-containers-configuration-shared-settings-application-insights.md)]

## <a name="billing-configuration-setting"></a>Konfigurationseinstellung „Billing“

Die Einstellung `Billing` gibt den Endpunkt-URI der Ressource vom Typ _Textanalyse_ in Azure an, der zum Erfassen von Abrechnungsinformationen für den Container verwendet wird. Sie müssen einen Wert für diese Konfigurationseinstellung angeben. Bei diesem Wert muss es sich um einen gültigen URI-Endpunkt für eine Ressource vom Typ _Textanalyse_ in Azure handeln. Der Container meldet die Nutzung etwa alle 10 bis 15 Minuten.

Diese Einstellung finden Sie hier:

* Azure-Portal: Übersicht über die **Textanalyse** mit der Bezeichnung `Endpoint`

|Erforderlich| Name | Datentyp | BESCHREIBUNG |
|--|------|-----------|-------------|
|Ja| `Billing` | String | URI des Abrechnungsendpunkts. Weitere Informationen zum Erhalt eines Abrechnungs-URI finden Sie unter [Ermitteln erforderlicher Parameter](how-tos/text-analytics-how-to-install-containers.md#gathering-required-parameters). Weitere Informationen und eine vollständige Liste mit regionalen Endpunkten finden Sie unter [Benutzerdefinierte Unterdomänennamen für Cognitive Services](../cognitive-services-custom-subdomains.md). |

## <a name="eula-setting"></a>Eula-Einstellung

[!INCLUDE [Container shared configuration eula settings](../../../includes/cognitive-services-containers-configuration-shared-settings-eula.md)]

## <a name="fluentd-settings"></a>Fluentd-Einstellungen

[!INCLUDE [Container shared configuration fluentd settings](../../../includes/cognitive-services-containers-configuration-shared-settings-fluentd.md)]

## <a name="http-proxy-credentials-settings"></a>Anmeldeinformationseinstellungen für HTTP-Proxy

[!INCLUDE [Container shared configuration fluentd settings](../../../includes/cognitive-services-containers-configuration-shared-settings-http-proxy.md)]

## <a name="logging-settings"></a>Logging-Einstellungen
 
[!INCLUDE [Container shared configuration logging settings](../../../includes/cognitive-services-containers-configuration-shared-settings-logging.md)]

## <a name="mount-settings"></a>Einbindungseinstellungen

Verwenden Sie Bindungsbereitstellungen zum Lesen und Schreiben von Daten im Container. Sie können eine Eingabe- oder Ausgabebereitstellung über die Option `--mount` im Befehl [docker run](https://docs.docker.com/engine/reference/commandline/run/) angeben.

Die Container für die Textanalyse verwenden keine Eingabe- oder Ausgabeeinbindungen zum Speichern von Trainings- oder Dienstdaten. 

Die genaue Syntax für den Bereitstellungspunkt auf dem Host variiert je nach Betriebssystem des Hosts. Darüber hinaus ist es eventuell nicht möglich, auf den Bereitstellungspunkt auf dem [Hostcomputer](how-tos/text-analytics-how-to-install-containers.md#the-host-computer) zuzugreifen, wenn ein Konflikt zwischen den vom Docker-Dienstkonto verwendeten Berechtigungen und den für den Bereitstellungspunkt auf dem Host verwendeten Berechtigungen besteht. 

|Optional| Name | Datentyp | BESCHREIBUNG |
|-------|------|-----------|-------------|
|Nicht zulässig| `Input` | String | Wird von Containern für die Textanalyse nicht verwendet.|
|Optional| `Output` | String | Das Ziel der Ausgabeeinbindung. Standardwert: `/output`. Dies ist der Speicherort der Protokolle. Beinhaltet Containerprotokolle. <br><br>Beispiel:<br>`--mount type=bind,src=c:\output,target=/output`|

## <a name="next-steps"></a>Nächste Schritte

* Weitere Informationen finden Sie unter [Installieren und Ausführen von Containern](how-tos/text-analytics-how-to-install-containers.md).
* Verwenden weiterer [Cognitive Services-Container](../cognitive-services-container-support.md)
