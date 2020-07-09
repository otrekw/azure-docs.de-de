---
title: Authentifizieren von Azure Automation-Runbooks mit Amazon Web Services
description: In diesem Artikel wird das Authentifizieren von Runbooks mit Amazon Web Services beschrieben.
keywords: AWS-Authentifizierung, AWS konfigurieren
services: automation
ms.subservice: process-automation
ms.date: 04/23/2020
ms.topic: conceptual
ms.openlocfilehash: acb056fb959fda320a14059e2b36a0f5e3fb0b37
ms.sourcegitcommit: 0b80a5802343ea769a91f91a8cdbdf1b67a932d3
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/25/2020
ms.locfileid: "83837183"
---
# <a name="authenticate-runbooks-with-amazon-web-services"></a>Authentifizieren von Runbooks mit Amazon Web Services

Die Automatisierung von häufigen Aufgaben mit Ressourcen in Amazon Web Services (AWS) kann mit Automation-Runbooks in Azure erreicht werden. Sie können viele Aufgaben in AWS mit Automation-Runbooks automatisieren, wie dies auch mit Ressourcen in Azure möglich ist. Für die Authentifizierung benötigen Sie ein Azure-Abonnement.

## <a name="obtain-aws-subscription-and-credentials"></a>Erwerben eines AWS-Abonnements und von AWS-Anmeldeinformationen

Zum Authentifizieren mit AWS müssen Sie ein AWS-Abonnement erwerben und einen Satz mit AWS-Anmeldeinformationen angeben, um Ihre über Azure Automation ausgeführten Runbooks zu authentifizieren. Die erforderlichen Anmeldeinformationen sind der AWS-Zugriffsschlüssel und der geheime Schlüssel für AWS. Siehe [Verwenden von AWS-Anmeldeinformationen](https://docs.aws.amazon.com/powershell/latest/userguide/specifying-your-aws-credentials.html).

## <a name="configure-automation-account"></a>Konfigurieren des Automation-Kontos

Sie können ein vorhandenes Automation-Konto verwenden, um sich bei AWS zu authentifizieren. Alternativ können Sie ein Konto verwenden, das für Runbooks für AWS-Ressourcen vorgesehen ist. Erstellen Sie in diesem Fall ein neues [Automation-Konto](automation-create-standalone-account.md).  

## <a name="store-aws-credentials"></a>Speichern der AWS-Anmeldeinformationen

Sie müssen die AWS-Anmeldeinformationen als Objekte in Azure Automation speichern. Anweisungen zum Erstellen des Zugriffsschlüssels und des geheimen Schlüssels finden Sie unter [Verwalten von Zugriffsschlüsseln für den Stammbenutzer des AWS-Kontos](https://docs.aws.amazon.com/general/latest/gr/managing-aws-access-keys.html). Wenn die Schlüssel verfügbar sind, kopieren Sie die Zugriffsschlüssel-ID und die ID des geheimen Schlüssels, und bewahren Sie sie an einem sicheren Ort auf. Sie können die Schlüsseldatei herunterladen, um sie an einem sicheren Ort zu speichern.

## <a name="create-credential-asset"></a>Erstellen eines Anmeldeinformationsobjekts

Nachdem Sie Ihre AWS-Sicherheitsschlüssel erstellt und kopiert haben, müssen Sie ein Anmeldeinformationsobjekt für das Automation-Konto erstellen. Das Objekt ermöglicht es Ihnen, die AWS-Schlüssel sicher zu speichern und in Ihren Runbooks auf sie zu verweisen. Siehe [Erstellen eines neuen Anmeldeinformationsobjekts über das Azure-Portal](shared-resources/credentials.md#create-a-new-credential-asset-with-the-azure-portal). Geben Sie die folgenden AWS-Informationen in die angezeigten Felder ein:
    
* **Name** - **AWScred** oder einen anderen geeigneten Wert gemäß Ihren Benennungsvorgaben
* **Benutzername**: Ihre Zugriffs-ID
* **Kennwort**: Der Name Ihres geheimen Schlüssels 

## <a name="next-steps"></a>Nächste Schritte

* Informationen über das Erstellen von Runbooks zum Automatisieren von Aufgaben in AWS finden Sie unter [Azure Automation-Szenario – Bereitstellen eines virtuellen AWS-Computers](automation-scenario-aws-deployment.md).