---
title: Entwickeln benutzerdefinierter U-SQL-Operatoren in Azure Data Lake Analytics
description: Hier erfahren Sie, wie Sie benutzerdefinierte Operatoren zur Verwendung und Wiederverwendung in Azure Data Lake Analytics-Aufträgen entwickeln.
ms.service: data-lake-analytics
ms.reviewer: jasonh
ms.topic: how-to
ms.date: 12/05/2016
ms.openlocfilehash: 11efdb727bacadb674fb49374ef1c70fcc788ecc
ms.sourcegitcommit: 8d8deb9a406165de5050522681b782fb2917762d
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/20/2020
ms.locfileid: "92219988"
---
# <a name="develop-u-sql-user-defined-operators-udos"></a>Entwickeln von benutzerdefinierten Operatoren von U-SQL (UDOs)
In diesem Artikel erfahren Sie, wie Sie benutzerdefinierte Operatoren entwickeln, um Daten in einem U-SQL-Auftrag zu verarbeiten.

## <a name="define-and-use-a-user-defined-operator-in-u-sql"></a>Definieren und Verwenden eines benutzerdefinierten Operators in U-SQL

### <a name="to-create-and-submit-a-u-sql-job"></a>So erstellen und übermitteln Sie einen U-SQL-Auftrag

1. Wählen Sie in Visual Studio **Datei > Neu > Projekt > U-SQL-Projekt** aus.
2. Klicken Sie auf **OK**. Visual Studio erstellt eine Projektmappe mit der Datei „Script.usql“.
3. Erweitern Sie im **Projektmappen-Explorer** die Datei „Script.usql“, und doppelklicken Sie dann auf **Script.usql.cs**.
4. Fügen Sie den folgenden Code in die Datei ein:

   ```usql
   using Microsoft.Analytics.Interfaces;
   using System.Collections.Generic;
   namespace USQL_UDO
   {
       public class CountryName : IProcessor
       {
           private static IDictionary<string, string> CountryTranslation = new Dictionary<string, string>
           {
               {
                   "Deutschland", "Germany"
               },
               {
                   "Suisse", "Switzerland"
               },
               {
                   "UK", "United Kingdom"
               },
               {
                   "USA", "United States of America"
               },
               {
                   "中国", "PR China"
               }
           };
           public override IRow Process(IRow input, IUpdatableRow output)
           {
               string UserID = input.Get<string>("UserID");
               string Name = input.Get<string>("Name");
               string Address = input.Get<string>("Address");
               string City = input.Get<string>("City");
               string State = input.Get<string>("State");
               string PostalCode = input.Get<string>("PostalCode");
               string Country = input.Get<string>("Country");
               string Phone = input.Get<string>("Phone");
               if (CountryTranslation.Keys.Contains(Country))
               {
                   Country = CountryTranslation[Country];
               }
               output.Set<string>(0, UserID);
               output.Set<string>(1, Name);
               output.Set<string>(2, Address);
               output.Set<string>(3, City);
               output.Set<string>(4, State);
               output.Set<string>(5, PostalCode);
               output.Set<string>(6, Country);
               output.Set<string>(7, Phone);
               return output.AsReadOnly();
           }
       }
   }
   ```

5. Öffnen Sie die Datei **Script.usql**, und fügen Sie das folgende U-SQL-Skript ein:

   ```usql
   @drivers =
       EXTRACT UserID      string,
               Name        string,
               Address     string,
               City        string,
               State       string,
               PostalCode  string,
               Country     string,
               Phone       string
       FROM "/Samples/Data/AmbulanceData/Drivers.txt"
       USING Extractors.Tsv(Encoding.Unicode);

   @drivers_CountryName =
       PROCESS @drivers
       PRODUCE UserID string,
               Name string,
               Address string,
               City string,
               State string,
               PostalCode string,
               Country string,
               Phone string
       USING new USQL_UDO.CountryName();

   OUTPUT @drivers_CountryName
       TO "/Samples/Outputs/Drivers.csv"
       USING Outputters.Csv(Encoding.Unicode);
   ```

6. Geben Sie das Data Lake Analytics-Konto, die -Datenbank und das -Schema an.
7. Klicken Sie im **Projektmappen-Explorer** mit der rechten Maustaste auf **Script.usql**, und klicken Sie dann auf **Skript erstellen**.
8. Klicken Sie im **Projektmappen-Explorer** mit der rechten Maustaste auf **Script.usql**, und klicken Sie dann auf **Skript senden**.
9. Wenn Sie noch nicht mit Ihrem Azure-Abonnement verbunden sind, werden Sie zur Eingabe der Anmeldeinformationen für Ihr Azure-Konto aufgefordert.
10. Klicken Sie auf **Submit**(Senden). Nach Abschluss der Übermittlung werden Ergebnisse und ein Auftragslink im Fenster „Ergebnisse“ angezeigt.
11. Sie müssen auf die Schaltfläche **Aktualisieren** klicken, um den letzten Auftragsstatus anzuzeigen und den Bildschirm zu aktualisieren.

### <a name="to-see-the-output"></a>So zeigen Sie die Ausgabe an

1. Erweitern Sie im **Server-Explorer** nacheinander **Azure**, **Data Lake Analytics**, Ihr Data Lake Analytics-Konto und **Speicherkonten**. Klicken Sie mit der rechten Maustaste auf den Standardspeicher, und klicken Sie dann auf **Explorer**.

2. Erweitern Sie „Beispielcode“ und „Ausgaben“, und doppelklicken Sie dann auf **Drivers.csv**.

## <a name="next-steps"></a>Nächste Schritte

* [Erweitern von U-SQL-Ausdrücken mit Benutzercode](/u-sql/concepts/extending-u-sql-expressions-with-user-code)
* [Verwenden von Data Lake-Tools für Visual Studio zum Entwickeln von U-SQL-Anwendungen](data-lake-analytics-data-lake-tools-get-started.md)
