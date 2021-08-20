---
title: Zugriffssteuerung und Sicherheit für IoT Hub | Microsoft-Dokumentation
description: Übersicht über die Steuerung des Zugriffs auf IoT Hub, mit Links zu ausführlichen Artikeln zur AAD-Integration und zu SAS-Optionen
author: wesmc7777
ms.author: wesmc
ms.service: iot-hub
services: iot-hub
ms.topic: conceptual
ms.date: 04/15/2021
ms.custom:
- amqp
- mqtt
- 'Role: Cloud Development'
- 'Role: IoT Device'
- 'Role: Operations'
- devx-track-js
- devx-track-csharp
ms.openlocfilehash: eb28d0384f0daa4029319597c5f3680a185c4ab6
ms.sourcegitcommit: 0046757af1da267fc2f0e88617c633524883795f
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/13/2021
ms.locfileid: "122354954"
---
# <a name="control-access-to-iot-hub"></a>Verwalten des Zugriffs auf IoT Hub

In diesem Artikel werden die Optionen zum Sichern Ihres IoT Hubs beschrieben. IoT Hub verwendet *Berechtigungen*, um Zugriff auf den Endpunkt jedes IoT Hubs zu gewähren. Berechtigungen beschränkten den Zugriff auf einen IoT Hub basierend auf der Funktionalität.

Es gibt drei verschiedene Möglichkeiten, den Zugriff auf IoT Hub zu steuern:

- **Azure Active Directory-Integration (Azure AD)** für Dienst-APIs. Azure bietet eine identitätsbasierte Authentifizierung mit AAD und eine fein abgestufte Autorisierung über die rollenbasierte Zugriffssteuerung von Azure (Azure Role-Based Access Control, Azure RBAC). Die Integration in Azure AD und Azure RBAC wird nur für IoT Hub-Dienst-APIs unterstützt. Weitere Informationen finden Sie unter [Steuern des Zugriffs auf IoT Hub mithilfe von Azure Active Directory](iot-hub-dev-guide-azure-ad-rbac.md).
- **Shared Access Signatures (SAS)** ermöglichen es, Berechtigungen zu gruppieren und sie mithilfe von Zugriffsschlüsseln und signierten Sicherheitstoken für Anwendungen zu gewähren. Weitere Informationen finden Sie unter [Steuern des Zugriffs auf IoT Hub mit SAS- (Shared Access Signatures) und Sicherheitstoken](iot-hub-dev-guide-sas.md). 
- **Sicherheitsanmeldeinformationen auf Gerätebasis**. Jeder IoT-Hub enthält eine [Identitätsregistrierung](iot-hub-devguide-identity-registry.md). Sie können für jedes Gerät in dieser Identitätsregistrierung Sicherheitsanmeldeinformationen konfigurieren, die DeviceConnect-Berechtigungen erteilen, deren Gültigkeitsbereich auf den Endpunkt dieses Geräts festgelegt ist. Weitere Informationen finden Sie unter [Authentifizieren eines Geräts für IoT Hub](iot-hub-dev-guide-sas.md#authenticating-a-device-to-iot-hub).

## <a name="next-steps"></a>Nächste Schritte

- [Steuern des Zugriffs auf IoT Hub mithilfe von Azure Active Directory](iot-hub-dev-guide-azure-ad-rbac.md)
- [Steuern des Zugriffs auf IoT Hub mit SAS- (Shared Access Signatures) und Sicherheitstoken](iot-hub-dev-guide-sas.md)
- [Authentifizieren eines Geräts für IoT Hub](iot-hub-dev-guide-sas.md#authenticating-a-device-to-iot-hub)
