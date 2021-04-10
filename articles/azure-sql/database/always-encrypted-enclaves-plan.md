---
title: Planen im Hinblick auf Intel SGX-Enclaves und Azure Attestation in Azure SQL-Datenbank
description: Planen der Bereitstellung von Always Encrypted mit Secure Enclaves in Azure SQL-Datenbank.
keywords: Verschlüsseln von Daten, SQL-Verschlüsselung, Datenbankverschlüsselung, vertrauliche Daten, Always Encrypted, Secure Enclaves, SGX, Nachweis
services: sql-database
ms.service: sql-database
ms.subservice: security
ms.devlang: ''
ms.topic: conceptual
author: jaszymas
ms.author: jaszymas
ms.reviwer: vanto
ms.date: 01/15/2021
ms.openlocfilehash: 4448ce051b0c9e73865e8057cc4f224c9cbeb571
ms.sourcegitcommit: f28ebb95ae9aaaff3f87d8388a09b41e0b3445b5
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/29/2021
ms.locfileid: "98732743"
---
# <a name="plan-for-intel-sgx-enclaves-and-attestation-in-azure-sql-database"></a>Planen im Hinblick auf Intel SGX-Enclaves und Azure Attestation in Azure SQL-Datenbank

[!INCLUDE[appliesto-sqldb](../includes/appliesto-sqldb.md)]

> [!NOTE]
> Always Encrypted mit Secure Enclaves für Azure SQL-Datenbank befindet sich derzeit in der **öffentlichen Vorschau**.

[Always Encrypted mit Secure Enclaves](/sql/relational-databases/security/encryption/always-encrypted-enclaves) in Azure SQL-Datenbank nutzt [Intel SGX](https://itpeernetwork.intel.com/microsoft-azure-confidential-computing/)-Enclaves (Intel Software Guard Extensions) und erfordert [Microsoft Azure Attestation](/sql/relational-databases/security/encryption/always-encrypted-enclaves#secure-enclave-attestation).

## <a name="plan-for-intel-sgx-in-azure-sql-database"></a>Planen im Hinblick auf Intel SGX in Azure SQL-Datenbank

Intel SGX ist eine hardwarebasierte TEE-Technologie (Trusted Execution Environment, vertrauenswürdige Ausführungsumgebung). Intel SGX ist für Datenbanken verfügbar, die das [vCore-basierte Kaufmodell](service-tiers-vcore.md) und die Hardwaregeneration der [DC-Serie](service-tiers-vcore.md?#dc-series) verwenden. Um sicherzustellen, dass Sie Always Encrypted mit Secure Enclaves in der Datenbank verwenden können, müssen Sie beim Erstellen der Datenbank die Hardwaregeneration der DC-Serie auswählen, oder Sie können eine vorhandene Datenbank aktualisieren, sodass sie die Hardwaregeneration der DC-Serie verwendet.

> [!NOTE]
> Intel SGX ist für andere Hardwaregenerationen als die DC-Serie nicht verfügbar. So ist Intel SGX beispielsweise nicht für Gen5-Hardware oder für Datenbanken verfügbar, die das [DTU-Modell](service-tiers-dtu.md) verwenden.

> [!IMPORTANT]
> Bevor Sie die Hardwaregeneration der DC-Serie für Ihre Datenbank konfigurieren, überprüfen Sie die regionale Verfügbarkeit der DC-Serie, und stellen Sie sicher, dass die Leistungseinschränkungen kein Problem darstellen. Einzelheiten dazu finden Sie unter [DC-Serie](service-tiers-vcore.md#dc-series).

## <a name="plan-for-attestation-in-azure-sql-database"></a>Planen im Hinblick auf Attestation in Azure SQL-Datenbank

[Microsoft Azure Attestation](../../attestation/overview.md) (Vorschau) ist eine Lösung für Nachweise in Trusted Execution Environments (TEEs), einschließlich Intel SGX-Enclaves in Instanzen von Azure SQL-Datenbank auf Hardware der DC-Seriengeneration.

Um Azure Attestation für Nachweise von Intel SGX-Enclaves in Azure SQL-Datenbank zu verwenden, führen Sie folgende Schritte aus:

1. Erstellen Sie einen [Nachweisanbieter](../../attestation/basic-concepts.md#attestation-provider), und konfigurieren Sie ihn mit einer Nachweisrichtlinie. 

2. Gewähren Sie dem logischen Azure SQL-Server Zugriff auf den erstellten Nachweisanbieter.

## <a name="roles-and-responsibilities-when-configuring-sgx-enclaves-and-attestation"></a>Rollen und Verantwortlichkeiten beim Konfigurieren von SGX-Enclaves und von Attestation

Das Konfigurieren Ihrer Umgebung für die Unterstützung von Intel SGX-Enclaves und von Attestation für Always Encrypted in Azure SQL-Datenbank umfasst das Einrichten von Komponenten unterschiedlicher Typen: Microsoft Azure Attestation, Azure SQL-Datenbank und Anwendungen, die den Enclave-Nachweis auslösen. Die Konfiguration von Komponenten der einzelnen Typen wird von Benutzern durchgeführt, die eine der folgenden unterschiedlichen Rollen haben:

- Nachweisadministrator: erstellt einen Nachweisanbieter in Microsoft Azure Attestation und die Nachweisrichtlinie, erteilt dem logischen Azure SQL-Server Zugriff auf den Nachweisanbieter und gibt die Nachweis-URL, die auf die Richtlinie verweist, für Anwendungsadministratoren frei.
- Azure SQL-Datenbank-Administrator: aktiviert SGX-Enclaves in Datenbanken durch Auswahl der Hardwaregeneration der DC-Serie und übergibt dem Nachweisadministrator die Identität des logischen Azure SQL-Servers, der Zugriff auf den Nachweisanbieter benötigt.
- Anwendungsadministrator: konfiguriert die Anwendungen mit der Nachweis-URL, die er vom Nachweisadministrator erhalten hat.

In Produktionsumgebungen, in denen echte vertrauliche Daten verarbeitet werden, ist es wichtig, dass Ihre Organisation beim Konfigurieren des Nachweises die Rollentrennung einhält, wobei jede einzelne Rolle von unterschiedlichen Personen angenommen wird. Azure SQL-Datenbank-Administratoren sollten insbesondere dann nicht die Nachweisrichtlinien verwalten, wenn das Ziel der Always Encrypted-Bereitstellung in Ihrer Organisation darin besteht, Administratoren von Azure SQL-Datenbank keinen Zugriff auf vertrauliche Daten zu gewähren und so die Angriffsfläche zu verringern.

## <a name="next-steps"></a>Nächste Schritte

- [Aktivieren von Intel SGX für Azure SQL-Datenbank](always-encrypted-enclaves-enable-sgx.md)

## <a name="see-also"></a>Weitere Informationen

- [Tutorial: Erste Schritte mit Always Encrypted mit Secure Enclaves in Azure SQL-Datenbank](always-encrypted-enclaves-getting-started.md)