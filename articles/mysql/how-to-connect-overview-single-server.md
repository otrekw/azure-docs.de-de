---
title: Verbinden und Abfragen – Single Server MySQL
description: Links zu Azure My SQL-Datenbank-Schnellstarts zeigen, wie Sie eine Verbindung mit Ihrem Server herstellen und Abfragen ausführen.
services: mysql
ms.service: mysql
ms.topic: how-to
author: mksuni
ms.author: sumuth
ms.date: 09/22/2020
ms.openlocfilehash: 229011f11ad6898555f59b063910d80a679070e0
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/09/2020
ms.locfileid: "90931742"
---
# <a name="connect-and-query-overview-for-azure-database-for-mysql--single-server"></a>Überblick über Verbinden und Abfragen für Azure Database for MySQL – Single Server
Das folgende Dokument enthält Links zu Beispielen, die zeigen, wie Sie eine Verbindung mit Azure Database for MySQL Single Server herstellen und Daten abfragen. Dieser Leitfaden enthält auch TLS-Empfehlungen und -Bibliotheken, die Sie zum Herstellen einer Verbindung mit dem Server in den nachstehend aufgeführten unterstützten Sprachen verwenden können.

## <a name="quickstarts"></a>Schnellstarts

| Schnellstart | BESCHREIBUNG |
|---|---|
|[MySQL Workbench](connect-workbench.md)|Diese Schnellstartanleitung zeigt, wie Sie mithilfe des MySQL Workbench-Clients eine Verbindung mit einer Datenbank herstellen können. Sie können dann mithilfe von MySQL-Anweisungen die Daten in der Datenbank abfragen, einfügen, aktualisieren und löschen.|
|[Azure Cloud Shell](https://docs.microsoft.com/azure/mysql/quickstart-create-mysql-server-database-using-azure-cli#connect-to-azure-database-for-mysql-server-using-mysql-command-line-client)|In diesem Artikel wird gezeigt, wie Sie **mysql.exe** in [Azure Cloud Shell](https://docs.microsoft.com/azure/cloud-shell/overview) ausführen, um eine Verbindung mit Ihrem Server herzustellen, und dann Anweisungen ausführen, um Daten in der Datenbank abzufragen, einzufügen, zu aktualisieren und zu löschen.|
|[MySQL mit Visual Studio](https://www.mysql.com/why-mysql/windows/visualstudio)|Sie können MySQL für Visual Studio zum Herstellen einer Verbindung mit Ihrem MySQL-Server verwenden. MySQL für Visual Studio lässt sich direkt in den Server-Explorer integrieren, was die Einrichtung neuer Verbindungen und die Arbeit mit Datenbankobjekten erleichtert.|
|[PHP](connect-php.md)|In dieser Schnellstartanleitung erfahren Sie, wie Sie unter Verwendung von PHP ein Programm erstellen, das eine Verbindung mit einer Datenbank herstellt, und mithilfe von MySQL-Anweisungen Daten abfragen.|
|[Java](connect-java.md)|In dieser Schnellstartanleitung erfahren Sie, wie Sie mithilfe von Java eine Verbindung mit einer Datenbank herstellen und anschließend Daten mithilfe von MySQL-Anweisungen abfragen.|
|[Node.js](connect-nodejs.md)|In dieser Schnellstartanleitung erfahren Sie, wie Sie unter Verwendung von Node.js ein Programm erstellen, das eine Verbindung mit einer Datenbank herstellt, und mithilfe von MySQL-Anweisungen Daten abfragen.|
|[.NET(C#)](connect-csharp.md)|In dieser Schnellstartanleitung erfahren Sie, wie Sie unter Verwendung von .NET (C#) ein C#-Programm erstellen, das eine Verbindung mit einer Datenbank herstellt, und mithilfe von MySQL-Anweisungen Daten abfragen.|
|[Go](connect-go.md)|In dieser Schnellstartanleitung erfahren Sie, wie Sie mithilfe von Go eine Verbindung mit einer Datenbank herstellen. Außerdem werden Transact-SQL-Anweisungen zum Abfragen und Ändern von Daten veranschaulicht.|
|[Python](connect-python.md)|In dieser Schnellstartanleitung erfahren Sie, wie Sie mithilfe von Python eine Verbindung mit einer Datenbank herstellen und anschließend Daten mithilfe von MySQL-Anweisungen abfragen. |
|[Ruby](connect-ruby.md)|In dieser Schnellstartanleitung erfahren Sie, wie Sie unter Verwendung von Ruby ein Programm erstellen, das eine Verbindung mit einer Datenbank herstellt, und mithilfe von MySQL-Anweisungen Daten abfragen.|
|[C++](connect-cpp.md)|In dieser Schnellstartanleitung erfahren Sie, wie Sie unter Verwendung von C++ ein Programm erstellen, das eine Verbindung mit einer Datenbank herstellt und Abfragedaten verwendet.|


## <a name="tls-considerations-for-database-connectivity"></a>Überlegungen zu TLS für Datenbankverbindungen

Transport Layer Security (TLS) wird von allen Treibern verwendet, die Microsoft zum Herstellen einer Verbindung mit Datenbanken in Azure Database for MySQL bereitstellt oder unterstützt. Eine besondere Konfiguration ist nicht erforderlich, aber erzwingen Sie unbedingt TLS 1.2 für neu erstellte Server. Wenn Sie TLS 1.0 und 1.1 verwenden, empfehlen wir, die TLS-Version für Ihre Server zu aktualisieren. Weitere Informationen finden Sie unter [ Konfigurieren von TLS](howto-tls-configurations.md).


## <a name="libraries"></a>Bibliotheken

Azure Database for MySQL verwendet die weltweit am häufigsten verwendete Communityedition von MySQL-Datenbank. Daher ist es mit einer Vielzahl von Programmiersprachen und Treibern kompatibel. Das Ziel besteht darin, die drei letzten Versionen von MySQL-Treibern zu unterstützen. Außerdem soll durch einen ständigen Austausch mit den Autoren der Open-Source-Community eine kontinuierliche Verbesserung von Funktionalität und Verwendbarkeit der MySQL-Treibern erreicht werden.

Erfahren Sie, welche [Treiber](concepts-compatibility.md) mit Azure Database for MySQL Single Server kompatibel sind. 


## <a name="next-steps"></a>Nächste Schritte 
- [Migrieren von Daten durch Sicherungen und Wiederherstellungen](concepts-migrate-dump-restore.md)
- [Migrieren von Daten durch Import und Export](concepts-migrate-import-export.md)
