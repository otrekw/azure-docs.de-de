---
title: Azure Attestation-Workflow
description: Der Workflow von Azure Attestation.
services: attestation
author: msmbaldwin
ms.service: attestation
ms.topic: overview
ms.date: 08/31/2020
ms.author: mbaldwin
ms.openlocfilehash: 240b27f897d8e7a34026701cf7fdc844eb9d4086
ms.sourcegitcommit: eb6bef1274b9e6390c7a77ff69bf6a3b94e827fc
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/05/2020
ms.locfileid: "89236964"
---
# <a name="workflow"></a>Workflow

Microsoft Azure Attestation erhält Beweise von Enclaves und wertet sie auf der Grundlage von Azure-Sicherheitsbaseline und konfigurierbaren Richtlinien aus. Nach erfolgreicher Überprüfung wird von Azure Attestation ein Nachweistoken generiert, um die Vertrauenswürdigkeit der Enclave zu bestätigen.

An einem Azure Attestation-Workflow sind folgenden Akteure beteiligt:

- **Vertrauende Seite:** Die Komponente, für die Azure Attestation die Enclave-Gültigkeit überprüft. 
- **Client:** Die Komponente, von der Informationen aus einer Enclave erfasst und Anforderungen an Azure Attestation gesendet werden. 
- **Azure Attestation:** Die Komponente, die den Enclave-Nachweis vom Client entgegennimmt, ihn überprüft und das Nachweistoken an den Client zurückgibt.


## <a name="enclave-validation-work-flow"></a>Enclave-Überprüfungsworkflow

Ein typischer SGX-Enclave-Nachweisworkflow (mit Azure Attestation) umfasst folgende allgemeine Schritte:

1. Der Client sammelt Beweise aus einer Enclave. Beweise sind Informationen zur Enclave-Umgebung und zu der Clientbibliothek, die innerhalb der Enclave ausgeführt wird.
1. Der Client verfügt über einen URI, der auf eine Azure Attestation-Instanz verweist. Der Client authentifiziert sich bei Azure AD und erhält ein Zugriffstoken.
1. Der Client sendet Beweise und das Zugriffstoken an Azure Attestation. Welche Informationen genau an den Anbieter übermittelt werden, hängt von der Art der Enclave ab.
1. Azure Attestation überprüft die übermittelten Informationen und wertet sie auf der Grundlage einer konfigurierten Richtlinie aus. War die Überprüfung erfolgreich, gibt Azure Attestation ein Nachweistoken an den Client zurück. Andernfalls meldet Azure Attestation einen Fehler an den Client. 
1. Der Client sendet das Nachweistoken an die vertrauende Seite. Die vertrauende Seite ruft den Azure Attestation-Metadatenendpunkt für den öffentlichen Schlüssel auf, um Signaturzertifikate abzurufen. Die vertrauende Seite überprüft die Signatur des Nachweistokens sowie die Vertrauenswürdigkeit der Enclave. 

![Enclave-Überprüfung](./media/validation-flow.png)


## <a name="next-steps"></a>Nächste Schritte
- [Erstellen und Signieren einer Nachweisrichtlinie](author-sign-policy.md)
- [Einrichten von Azure Attestation mithilfe von PowerShell](quickstart-powershell.md)
