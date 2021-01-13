---
title: 'Glossar: IoT Plug & Play | Microsoft-Dokumentation'
description: 'Konzepte: Ein Glossar mit häufig verwendeten Begriffen in Zusammenhang mit IoT Plug & Play'
author: dominicbetts
ms.author: dobett
ms.date: 07/22/2020
ms.topic: conceptual
ms.service: iot-pnp
services: iot-pnp
ms.openlocfilehash: d44866e2d04ab1bab5d2eca01374350a7d73a0ea
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/09/2020
ms.locfileid: "91577338"
---
# <a name="glossary-of-terms-for-iot-plug-and-play"></a>Glossar für IoT Plug & Play

Definitionen häufig verwendeter Begriffe, die in den Artikeln zu IoT Plug & Play verwendet werden.

## <a name="azure-iot-explorer-tool"></a>Azure IoT-Explorer-Tool

Der Azure IoT-Explorer ist ein grafisches Tool, das Sie verwenden können, um mit Ihren [IoT Plug & Play-Geräten](#iot-plug-and-play-device) zu interagieren und sie zu testen. Nachdem Sie das Tool auf Ihrem lokalen Computer installiert haben, können Sie es für folgende Aufgaben verwenden:

- Anzeigen der mit Ihrem [IoT-Hub](#azure-iot-hub) verbundenen Geräte.
- Herstellen einer Verbindung mit einem IoT Plug & Play-Gerät.
- Anzeigen der [Gerätekomponenten](#component).
- Anzeigen der vom Gerät gesendeten [Telemetriedaten](#telemetry).
- Arbeiten mit [Geräteeigenschaften](#properties).
- Aufrufen von [Gerätebefehlen](#commands).

## <a name="azure-iot-hub"></a>Azure IoT Hub

IoT Hub ist ein in der Cloud gehosteter, verwalteter Dienst, der als zentraler Nachrichtenhub für die bidirektionale Kommunikation zwischen Ihrer IoT-Anwendung und den Geräten dient, die von der Anwendung verwaltet werden. [IoT Plug & Play-Geräte](#iot-plug-and-play-device) können eine Verbindung mit einer IoT Hub-Instanz herstellen. Eine IoT-Lösung verwendet einen IoT Hub, um Folgendes zu ermöglichen:

- Geräten das Senden von Telemetriedaten an eine cloudbasierte Lösung.
- Einer cloudbasierten Lösung das Verwalten von verbundenen Geräten.

## <a name="azure-iot-device-sdk"></a>Azure IoT-Geräte-SDK

Es gibt Geräte-SDKs für mehrere Sprachen, mit denen Sie IoT Plug & Play-Geräteclientanwendungen erstellen können. Verwenden Sie **DeviceClient** für Geräte und **ModuleClient** für Module und IoT Edge-Module.

## <a name="commands"></a>Befehle

Die in einer [Schnittstelle](#interface) definierten Befehle stellen Methoden dar, die auf dem [digitalen Zwilling](#digital-twin) ausgeführt werden können. Dies kann beispielsweise ein Befehl zum Neustart eines Geräts sein.

## <a name="component"></a>Komponente

Mithilfe von Komponenten können Sie eine [Modellschnittstelle](#interface) als eine Assembly anderer Schnittstellen erstellen. Ein [Gerätemodell](#device-model) kann mehrere Schnittstellen als Komponenten kombinieren. Ein Modell könnte beispielsweise eine Schalterkomponente und eine Thermostatkomponente enthalten. Mehrere Komponenten in einem Modell können auch denselben Schnittstellentyp verwenden. Beispielsweise könnte ein Modell zwei Thermostatkomponenten enthalten.

## <a name="connection-string"></a>Verbindungszeichenfolge

Eine Verbindungszeichenfolge kapselt die Informationen, die zum Herstellen einer Verbindung mit einem Endpunkt erforderlich sind. Eine Verbindungszeichenfolge enthält üblicherweise die Adresse des Endpunkts und Sicherheitsinformationen, aber das Format der Verbindungszeichenfolge variiert für die verschiedenen Dienste. Im Zusammenhang mit dem IoT Hub-Dienst gibt es zwei Arten von Verbindungszeichenfolgen:

- Geräteverbindungszeichenfolgen ermöglichen es [IoT Plug & Play-Geräten](#iot-plug-and-play-device), eine Verbindung mit den geräteseitigen Endpunkten einer IoT Hub-Instanz herzustellen. Der Clientcode auf einem Gerät verwendet die Verbindungszeichenfolge zum Herstellen einer sicheren Verbindung mit einer IoT Hub-Instanz.
- IoT Hub-Verbindungszeichenfolgen ermöglichen es Back-End-Lösungen und -Tools, eine sichere Verbindung mit den dienstseitigen Endpunkten einer IoT Hub-Instanz herzustellen. Diese Lösungen und Tools verwalten die IoT Hub-Instanz und die damit verbundenen Geräte.

## <a name="default-component"></a>Standardkomponente

Alle [Gerätemodelle](#device-model) verfügen über eine Standardkomponente. Ein einfaches Gerätemodell verfügt nur über eine Standardkomponente. Ein solches Modell wird auch als Gerät ohne Komponente bezeichnet. Bei einem komplexeren Modell sind mehrere Komponenten unterhalb der Standardkomponente geschachtelt.

## <a name="device-certification"></a>Gerätezertifizierung

Mit dem IoT Plug & Play-Gerätezertifizierungsprogramm wird überprüft, ob ein Gerät die IoT Plug & Play-Zertifizierungsanforderungen erfüllt. Sie können ein zertifiziertes Gerät zum öffentlichen [Katalog mit Certified for Azure IoT-Geräten](https://aka.ms/devicecatalog) hinzufügen.

## <a name="device-model"></a>Gerätemodell

Ein Gerätemodell beschreibt ein [IoT Plug & Play-Gerät](#iot-plug-and-play-device) und definiert die [Komponenten](#component), aus denen es besteht. Ein einfaches Gerätemodell hat keine separaten Komponenten und enthält eine Definition für eine einzelne Schnittstelle. Das Azure IoT-Explorer-Tool zeigt ein einfaches Modell, das über eine einzelne [Standardkomponente](#default-component) verfügt.

Ein komplexeres Gerätemodell besteht aus mehreren Komponenten. Ein Gerätemodell entspricht normalerweise einem physischen Gerät, einem Produkt oder einer SKU. Sie verwenden die [Digital Twins Definition Language, Version 2](#digital-twins-definition-language) zum Definieren eines Gerätemodells.

## <a name="device-builder"></a>Geräteentwickler

Ein Geräteentwickler verwendet ein [Gerätemodell](#device-model) und [Schnittstellen](#interface) beim Implementieren von Code zur Ausführung auf einem [IoT Plug & Play-Gerät](#iot-plug-and-play-device). Geräteentwickler verwenden normalerweise eines der [Azure IoT-Geräte-SDKs](#azure-iot-device-sdk) zum Implementieren des Geräteclients, doch dies ist nicht unbedingt erforderlich.

## <a name="device-modeling"></a>Gerätemodellierung

Ein [Geräteentwickler](#device-builder) oder [Modulentwickler](#module-builder) verwendet die [Digital Twins Definition Language](#digital-twins-definition-language) zum Modellieren der Funktionen eines [IoT Plug & Play-Geräts](#iot-plug-and-play-device). Ein [Lösungsentwickler](#solution-builder) kann eine IoT-Lösung aus dem Modell konfigurieren.

## <a name="digital-twin"></a>Digitaler Zwilling

Ein digitaler Zwilling ist ein Modell eines [IoT Plug & Play-Geräts](#iot-plug-and-play-device). Er wird mithilfe der [Digital Twins Definition Language](#digital-twins-definition-language) modelliert. Sie können zur Laufzeit mithilfe der [Azure IoT-Geräte-SDKs](#azure-iot-device-sdk) mit digitalen Zwillingen interagieren. So können Sie beispielsweise einen Eigenschaftswert in einem digitalen Zwilling auf einem Gerät festlegen; dann kommuniziert das SDK diese Änderung an Ihrer IoT-Lösung in der Cloud.

## <a name="digital-twin-change-events"></a>Änderungsereignisse bei digitalen Zwillingen

Wenn ein [IoT Plug & Play-Gerät](#iot-plug-and-play-device) mit einem [IoT-Hub](#azure-iot-hub) verbunden wird, kann der Hub dessen Routingfunktionen nutzen, um Benachrichtigungen über Änderungen bei digitalen Zwillingen zu senden. Beispiel: Immer wenn sich ein [Eigenschaftswert](#properties) auf einem Gerät ändert, kann IoT Hub eine Benachrichtigung an einen Endpunkt, z. B. einen Event Hub, senden.

## <a name="digital-twins-definition-language"></a>Digital Twins Definition Language

Eine Sprache zum Beschreiben von Modellen und Schnittstellen für [IoT Plug & Play-Geräte](#iot-plug-and-play-device). Verwenden Sie die [Digital Twins Definition Language, Version 2](https://github.com/Azure/opendigitaltwins-dtdl), um die Funktionen eines [digitalen Zwillings](#digital-twin) zu beschreiben sowie es der IoT-Plattform und den IoT-Lösungen zu ermöglichen, die Semantik der Entität zu nutzen.

## <a name="digital-twin-route"></a>Route für digitale Zwillinge

Eine Route, die in einem [IoT-Hub](#azure-iot-hub) eingerichtet wurde, um [Änderungsereignisse bei digitalen Zwillingen](#digital-twin-change-events) an einen Endpunkt, z. B. einen Event Hub, zu übermitteln.

## <a name="interface"></a>Schnittstelle

Eine Schnittstelle beschreibt verwandte Funktionen, die durch ein [IoT Plug & Play-Gerät](#iot-plug-and-play-device) oder einen [digitalen Zwilling](#digital-twin) implementiert werden. Sie können Schnittstellen in verschiedenen [Gerätemodellen](#device-model) wiederverwenden. Wenn eine Schnittstelle in einem Gerätemodell verwendet wird, definiert sie eine [Komponente](#component) des Geräts. Ein einfaches Gerät enthält nur eine Standardschnittstelle.

## <a name="iot-hub-query-language"></a>IoT Hub-Abfragesprache

Die IoT Hub-Abfragesprache wird für mehrere Zwecke verwendet. Beispielsweise können Sie mithilfe dieser Sprache nach bei Ihrem IoT-Hub registrierten Geräten suchen oder das Verhalten des [Routings bei digitalen Zwillingen](#digital-twin-route) verfeinern.

## <a name="iot-plug-and-play-bridge"></a>IoT Plug & Play-Brücke

IoT Plug & Play-Brücke ist eine Open-Source-Anwendung, die es vorhandenen, an Windows- oder Linux-Gateways angefügten Sensoren und Peripheriegeräten ermöglicht, eine Verbindung als [IoT Plug & Play-Geräte](#iot-plug-and-play-device) herzustellen.

## <a name="iot-plug-and-play-device"></a>IoT Plug & Play-Gerät

Ein IoT Plug & Play-Gerät ist normalerweise ein kleines, eigenständiges Computergerät, das Daten sammelt oder andere Geräte steuert. Auf einem solchen Gerät wird Software oder Firmware ausgeführt, die ein [Gerätemodell](#device-model) implementiert.  Ein IoT Plug & Play-Gerät könnte z.B. ein Gerät zur Überwachung von Umweltdaten oder ein Regler für ein intelligentes Bewässerungssystem in der Landwirtschaft sein. Ein IoT Plug & Play-Gerät kann direkt oder als IoT Edge-Modul implementiert werden. Sie können eine in der Cloud gehostete IoT-Lösung zum Steuern, Überwachen und Empfangen von Daten von IoT Plug & Play-Geräten schreiben.

## <a name="iot-plug-and-play-conventions"></a>IoT Plug & Play-Konventionen

Von IoT Plug & Play-[Geräten](#iot-plug-and-play-device) wird erwartet, dass sie beim Datenaustausch mit einer Lösung eine Reihe von [Konventionen](concepts-convention.md) einhalten.

## <a name="model-id"></a>Modell-ID

Wenn ein IoT Plug & Play-Gerät eine Verbindung mit einem IoT-Hub herstellt, sendet es die **Modell-ID** des von ihm implementierten[DTDL](#digital-twins-definition-language)-Modells. Dies ermöglicht der Lösung, das Gerätemodell zu finden.

## <a name="model-repository"></a>Modellrepository

In einem [Modellrepository](concepts-model-repository.md) werden [Gerätemodelle](#device-model) und [Schnittstellen](#interface) gespeichert.

## <a name="model-repository-rest-api"></a>Modellrepository-REST-API

Eine API zum Verwalten von und Interagieren mit dem Modellrepository. So können Sie beispielsweise mithilfe der API [Gerätemodelle](#device-model) hinzufügen und danach suchen.

## <a name="module-builder"></a>Modulentwickler

Ein Modulentwickler verwendet ein [Gerätemodell](#device-model) und [Schnittstellen](#interface) beim Implementieren von Code zur Ausführung auf einem [IoT Plug & Play-Gerät](#iot-plug-and-play-device). Modulentwickler implementieren den Code als Modul oder IoT Edge-Modul, das für die IoT Edge-Runtime auf einem Gerät bereitgestellt werden soll.

## <a name="properties"></a>Eigenschaften

Eigenschaften sind Datenfelder, die in einer [Schnittstelle](#interface) definiert sind und einen bestimmten Zustand eines digitalen Zwillings repräsentieren. Sie können Eigenschaften als schreibgeschützt oder beschreibbar deklarieren. Schreibgeschützte Eigenschaften, z.B. eine Seriennummer, werden durch Code festgelegt, der auf dem [IoT Plug & Play-Gerät](#iot-plug-and-play-device) selbst ausgeführt wird.  Schreibbare Eigenschaften, z.B. ein Alarmschwellenwert, werden normalerweise aus der cloudbasierten IoT-Lösung festgelegt.

## <a name="shared-access-signature"></a>Shared Access Signature (SAS)

Shared Access Signatures (SAS) sind ein Authentifizierungsmechanismus, der auf mit SHA-256 gesicherten Hashes oder URIs basiert. Die SAS-Authentifizierung besteht aus zwei Komponenten: einer SAS-Richtlinie und einer Shared Access Signature (oft als ein „Token“ bezeichnet). Ein [IoT Plug & Play-Gerät](#iot-plug-and-play-device) verwendet eine Shared Access Signature zum Authentifizieren bei einem [IoT-Hub](#azure-iot-hub).

## <a name="solution-builder"></a>Lösungsentwickler

Ein Lösungsentwickler erstellt das Lösungs-Back-End. Ein Lösungsentwickler arbeitet normalerweise mit Azure-Ressourcen wie [IoT Hub](#azure-iot-hub) und [Modellrepositorys](#model-repository).

## <a name="telemetry"></a>Telemetrie

In einer [Schnittstelle](#interface) definierte Telemetriefelder stellen Messungen dar. Diese Messungen sind normalerweise Werte wie Sensormesswerte, die von dem [IoT Plug & Play-Gerät](#iot-plug-and-play-device) als Datenstrom gesendet werden.
