---
title: include file
description: include file
ms.topic: include
ms.custom: include file
ms.date: 03/11/2020
ms.openlocfilehash: b34d3ed2481600cde7ffb3275d635b8a57926bd6
ms.sourcegitcommit: 867cb1b7a1f3a1f0b427282c648d411d0ca4f81f
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/19/2021
ms.locfileid: "83665921"
---
Die folgenden Rollen werden für die Kollaboration bereitgestellt:

|Role|Funktionen|API-Zugriff|API-Berechtigungen|
|--|--|--|--|
|Besitzer|All|Authentifizierungsschlüssel|All|
|Mitwirkender|Alle, mit Ausnahme der Möglichkeit, Rollen neue Mitglieder hinzuzufügen|Authentifizierungsschlüssel|Alle, mit Ausnahme der Möglichkeit, Rollen neue Mitglieder hinzuzufügen|
|QnA Maker lesen<br>(lesen)|Exportieren/herunterladen<br>Test|Bearertoken|1. KB-API herunterladen<br>2. KBs für Benutzer-API auflisten<br>3. Abrufen der Details der Wissensdatenbank<br>4. Änderungen herunterladen<br>Antwort generieren |
|QnA Maker Editor<br>(lesen/schreiben)|Exportieren/herunterladen<br>Test<br>KB aktualisieren<br>KB exportieren<br>KB importieren<br>KB ersetzen<br>Erstellen der Knowledge Base|Bearertoken|1. KB-API erstellen<br>2. KB-API aktualisieren<br>3. KB-API ersetzen<br>4. Änderungen ersetzen<br>5. „API trainieren“ [im neuen Dienstmodell Version 5]|
|Cognitive Service-Benutzer<br>(lesen/schreiben/veröffentlichen)|All|Bearertoken|All|