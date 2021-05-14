---
title: Problembehandlung für Azure Spring Cloud in virtuellen Netzwerken
description: Leitfaden zur Problembehandlung für Azure Spring Cloud in virtuellen Netzwerken.
author: mikedodaro
ms.service: spring-cloud
ms.topic: how-to
ms.date: 09/19/2020
ms.author: brendm
ms.custom: devx-track-java
ms.openlocfilehash: 6a8be63c134c25373f85a146ed3df7316e1eb7dc
ms.sourcegitcommit: 4a54c268400b4158b78bb1d37235b79409cb5816
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/28/2021
ms.locfileid: "108132403"
---
# <a name="troubleshooting-azure-spring-cloud-in-virtual-networks"></a>Problembehandlung für Azure Spring Cloud in virtuellen Netzwerken

Dieses Dokument hilft Ihnen bei der Behandlung verschiedener Probleme, die bei der Verwendung von Azure Spring Cloud in virtuellen Netzwerken auftreten können.

## <a name="i-encountered-a-problem-with-creating-an-azure-spring-cloud-service-instance"></a>Bei mir ist ein Problem beim Erstellen einer Azure Spring Cloud-Dienstinstanz aufgetreten

Zum Erstellen einer Instanz von Azure Spring Cloud müssen Sie über ausreichende Berechtigungen zum Bereitstellen der Instanz im virtuellen Netzwerk verfügen.  Die Spring Cloud-Dienstinstanz muss sich selbst die [Azure Spring Cloud-Dienstberechtigung für das virtuelle Netzwerk erteilen](./how-to-deploy-in-azure-virtual-network.md#grant-service-permission-to-the-virtual-network).

Wenn Sie das Azure-Portal zum Einrichten der Azure Spring Cloud-Dienstinstanz verwenden, werden die Berechtigungen vom Azure-Portal überprüft.

Überprüfen Sie Folgendes, um die Azure Spring Cloud-Dienstinstanz mithilfe der [Azure CLI](/cli/azure/get-started-with-azure-cli) einzurichten:

- Das Abonnement ist aktiv.
- der Standort von Azure Spring Cloud unterstützt wird.
- Die Ressourcengruppe für die Instanz wurde bereits erstellt.
- Der Ressourcenname entspricht der Benennungsregel. die Instanz nur Kleinbuchstaben, Zahlen und Bindestriche enthält. Das erste Zeichen muss ein Buchstabe sein. Das letzte Zeichen muss ein Buchstabe oder eine Zahl sein. Der Wert muss zwischen 2 und 32 Zeichen lang sein.

Wenn Sie die Azure Spring Cloud-Dienstinstanz mithilfe der Resource Manager-Vorlage einrichten möchten, lesen Sie zunächst den Artikel [Verstehen der Struktur und Syntax von Azure Resource Manager-Vorlagen](../azure-resource-manager/templates/template-syntax.md).

### <a name="common-creation-issues"></a>Häufig auftretende Probleme bei der Erstellung

| Fehlermeldung | So behebt man den Fehler |
|------|------|
| Von Azure Spring Cloud erstellte Ressourcen wurden von der Richtlinie nicht zugelassen. | Netzwerkressourcen werden erstellt, wenn Sie Azure Spring Cloud in Ihrem eigenen virtuellen Netzwerk bereitstellen. Überprüfen Sie, ob Sie in [Azure Policy](../governance/policy/overview.md) definiert haben, diese Erstellung zu blockieren. Eine Fehlermeldung teilt mit, dass Ressourcen nicht erstellt werden konnten. |
| Der erforderliche Datenverkehr ist nicht in der Positivliste enthalten. | Weitere Informationen dazu, wie Sie sicherstellen, dass der gewünschte Datenverkehr in der Positivliste enthalten ist, finden Sie unter [Kundenzuständigkeiten für die Ausführung von Azure Spring Cloud im VNet](./vnet-customer-responsibilities.md). |

## <a name="my-application-cant-be-registered"></a>Meine Anwendung kann nicht registriert werden

Dieses Problem tritt auf, wenn Ihr virtuelles Netzwerk mit benutzerdefinierten DNS-Einstellungen konfiguriert ist. In diesem Fall ist die von Azure Spring Cloud verwendete private DNS-Zone nicht wirksam. Fügen Sie die Azure DNS-IP-Adresse 168.63.129.16 als Upstream-DNS-Server im benutzerdefinierten DNS-Server hinzu.

## <a name="other-issues"></a>Andere Probleme

[Behandlung von allgemeinen Problemen mit Azure Spring Cloud](./troubleshoot.md).