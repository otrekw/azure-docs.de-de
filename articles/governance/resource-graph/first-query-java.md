---
title: 'Schnellstart: Ihre erste Java-Abfrage'
description: In dieser Schnellstartanleitung führen Sie die Schritte zum Aktivieren des Resource Graph-Maven-Pakets für Java und zum Ausführen Ihrer ersten Abfrage aus.
ms.date: 03/30/2021
ms.topic: quickstart
ms.custom: devx-track-java
ms.openlocfilehash: 97c04cb8b8180034bdc5109446c79deb56e457c9
ms.sourcegitcommit: 3f684a803cd0ccd6f0fb1b87744644a45ace750d
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/02/2021
ms.locfileid: "106223852"
---
# <a name="quickstart-run-your-first-resource-graph-query-using-java"></a>Schnellstart: Ausführen Ihrer ersten Resource Graph-Abfrage mithilfe von Java

Wenn Sie Azure Resource Graph verwenden möchten, müssen Sie zuerst überprüfen, ob die erforderlichen Maven-Pakete für Java installiert sind. In dieser Schnellstartanleitung wird der Prozess zum Hinzufügen der Maven-Pakete zu Ihrer Java-Installation Schritt für Schritt beschrieben.

Am Ende dieses Prozesses haben Sie Ihrer Java-Installation die Maven-Pakete hinzugefügt und Ihre erste Resource Graph-Abfrage ausgeführt.

## <a name="prerequisites"></a>Voraussetzungen

- Ein Azure-Abonnement. Wenn Sie kein Azure-Abonnement besitzen, können Sie ein [kostenloses Konto](https://azure.microsoft.com/free/) erstellen, bevor Sie beginnen.

- Überprüfen Sie, ob die aktuelle Azure CLI-Version (mindestens **2.21.0**) installiert ist. Falls sie noch nicht installiert ist, lesen Sie den Artikel [Installieren der Azure CLI](/cli/azure/install-azure-cli).

  > [!NOTE]
  > Azure CLI ist erforderlich, um Azure SDK für Java für die Verwendung der **CLI-basierten Authentifizierung** in den folgenden Beispielen zu aktivieren. Weitere Informationen über anderen Optionen finden Sie unter [Azure Identity-Clientbibliothek für Java](https://github.com/Azure/azure-sdk-for-java/tree/master/sdk/identity/azure-identity).

- [Java Developer Kit](/azure/developer/java/fundamentals/java-jdk-long-term-support), Version
  8.

- [Apache Maven](https://maven.apache.org/) Version 3.6 oder höher.

## <a name="create-the-resource-graph-project"></a>Erstellen des Resource Graph-Projekts

Erstellen und Konfigurieren Sie mit Maven eine neue Anwendung, und installieren Sie die erforderlichen Maven-Pakete, um für Java das Abfragen von Azure Resource Graph zu ermöglichen.

1. Initialisieren Sie eine neue Java-Anwendung mit dem Namen „argQuery“ und einem [Maven-Archetyp](https://maven.apache.org/guides/introduction/introduction-to-archetypes.html):

   ```cmd
   mvn -B archetype:generate -DarchetypeGroupId="org.apache.maven.archetypes" -DgroupId="com.Fabrikam" -DartifactId="argQuery"
   ```

1. Verwenden Sie anstelle der bisherigen Verzeichnisse den neuen Projektordner `argQuery`, und öffnen Sie `pom.xml` in Ihrem bevorzugten Editor. Fügen Sie unter dem vorhandenen Knoten `<dependencies>` die folgenden Knoten `<dependency>` hinzu :

   ```xml
    <dependency>
        <groupId>com.azure</groupId>
        <artifactId>azure-identity</artifactId>
        <version>1.2.4</version>
    </dependency>
    <dependency>
        <groupId>com.azure.resourcemanager</groupId>
        <artifactId>azure-resourcemanager-resourcegraph</artifactId>
        <version>1.0.0-beta.1</version>
    </dependency>
   ```

1. Fügen Sie in der Datei `pom.xml` unter dem `<project>`-Basisknoten den folgenden Knoten `<properties>` hinzu, um die Quell- und Zielversionen zu aktualisieren:

   ```xml
   <properties>
        <maven.compiler.source>1.8</maven.compiler.source>
        <maven.compiler.target>1.8</maven.compiler.target>
   </properties>
   ```

1. Fügen Sie in der Datei `pom.xml` unter dem `<project>`-Basisknoten den folgenden Knoten `<build>` hinzu, um das Ziel und die Hauptklasse für das auszuführende Projekt zu konfigurieren.

   ```xml
   <build>
       <plugins>
           <plugin>
               <groupId>org.codehaus.mojo</groupId>
               <artifactId>exec-maven-plugin</artifactId>
               <version>1.2.1</version>
               <executions>
                   <execution>
                       <goals>
                           <goal>java</goal>
                       </goals>
                   </execution>
               </executions>
               <configuration>
                   <mainClass>com.Fabrikam.App</mainClass>
               </configuration>
           </plugin>
       </plugins>
   </build>
   ```

1. Ersetzen Sie mit dem folgenden Code den Standardwert `App.java` in `\argQuery\src\main\java\com\Fabrikam`, und speichern Sie die aktualisierte Datei:

   ```java
   package com.Fabrikam;
   
   import java.util.Arrays;
   import java.util.List;
   import com.azure.core.management.AzureEnvironment;
   import com.azure.core.management.profile.AzureProfile;
   import com.azure.identity.DefaultAzureCredentialBuilder;
   import com.azure.resourcemanager.resourcegraph.ResourceGraphManager;
   import com.azure.resourcemanager.resourcegraph.models.QueryRequest;
   import com.azure.resourcemanager.resourcegraph.models.QueryRequestOptions;
   import com.azure.resourcemanager.resourcegraph.models.QueryResponse;
   import com.azure.resourcemanager.resourcegraph.models.ResultFormat;
   
   public class App
   {
       public static void main( String[] args )
       {
           List<String> listSubscriptionIds = Arrays.asList(args[0]);
           String strQuery = args[1];
   
           ResourceGraphManager manager = ResourceGraphManager.authenticate(new DefaultAzureCredentialBuilder().build(), new AzureProfile(AzureEnvironment.AZURE));
   
           QueryRequest queryRequest = new QueryRequest()
               .withSubscriptions(listSubscriptionIds)
               .withQuery(strQuery);
           
           QueryResponse response = manager.resourceProviders().resources(queryRequest);
   
           System.out.println("Records: " + response.totalRecords());
           System.out.println("Data:\n" + response.data());
       }
   }
   ```

1. Kompilieren Sie die Konsolenanwendung `argQuery`:

   ```bash
   mvn package
   ```

## <a name="run-your-first-resource-graph-query"></a>Ausführen Ihrer ersten Resource Graph-Abfrage

Nachdem die Java-Konsolenanwendung kompiliert wurde, können Sie eine einfache Resource Graph-Abfrage ausprobieren. Die Abfrage gibt die ersten fünf Azure-Ressourcen mit dem **Namen** und **Ressourcentyp** der einzelnen Ressourcen zurück.

Jeder Aufruf von `argQuery` enthält Variablen, die Sie durch Ihre eigenen Werte ersetzen müssen:

- Ersetzen Sie `{subscriptionId}` durch Ihre Abonnement-ID.
- Ersetzen Sie `{query}` durch die Azure Resource Graph-Abfrage

1. Verwenden Sie die Azure CLI, um sich mit `az login` zu authentifizieren.

1. Ändern Sie die Verzeichnisse in den `argQuery`-Projektordner, den Sie mit dem vorherigen `mvn -B archetype:generate`-Befehl erstellt haben.

1. Führen Sie Ihre erste Azure Resource Graph-Abfrage mithilfe von Maven aus, um die Konsolenanwendung zu kompilieren und die Argumente zu übergeben. Die `exec.args`-Eigenschaft erkennt die Argumente anhand von Leerzeichen. Um die Abfrage als einzelnes Argument zu identifizieren, umschließen wir sie mit einfachen Anführungszeichen (`'`) ein.

   ```bash
   mvn compile exec:java -Dexec.args "{subscriptionId} 'Resources | project name, type | limit 5'"
   ```

   > [!NOTE]
   > Da dieses Abfragebeispiel keinen Sortierungsmodifizierer wie `order by` umfasst, ergibt die mehrfache Ausführung dieser Abfrage unter Umständen pro Anforderung einen anderen Satz von Ressourcen.

1. Ändern Sie das Argument in `argQuery.exe`, und ändern Sie die Abfrage so, dass die Sortierung (`order by`) nach der Eigenschaft **Name** erfolgt:

   ```bash
   mvn compile exec:java -Dexec.args "{subscriptionId} 'Resources | project name, type | limit 5 | order by name asc'"
   ```

   > [!NOTE]
   > Genau wie bei der ersten Abfrage ergibt die mehrfache Ausführung dieser Abfrage vermutlich pro Anforderung einen anderen Satz von Ressourcen. Die Reihenfolge der Abfragebefehle ist wichtig. In diesem Beispiel kommt `order by` nach `limit`. Durch diese Befehlsreihenfolge werden die Abfrageergebnisse zuerst eingeschränkt und dann sortiert.

1. Ändern Sie den letzten Parameter in `argQuery.exe`, und ändern Sie die Abfrage so, dass zuerst die Sortierung (`order by`) nach der Eigenschaft **Name** und dann die Begrenzung (`limit`) auf die ersten fünf Ergebnisse erfolgt:

   ```bash
   mvn compile exec:java -Dexec.args "{subscriptionId} 'Resources | project name, type | order by name asc | limit 5'"
   ```

Wenn die letzte Abfrage mehrmals ausgeführt wird und in Ihrer Umgebung keine Änderungen vorgenommenen werden, sind die zurückgegebenen Ergebnisse konsistent und nach der Eigenschaft **Name** sortiert, aber immer noch auf die ersten fünf Ergebnisse begrenzt.

## <a name="clean-up-resources"></a>Bereinigen von Ressourcen

Falls Sie die Java-Konsolenanwendung und die installierten Pakete entfernen möchten, können Sie dies erreichen, indem Sie den Projektordner `argQuery` löschen.

## <a name="next-steps"></a>Nächste Schritte

In dieser Schnellstartanleitung haben Sie eine Java-Konsolenanwendung mit den erforderlichen Resource Graph-Paketen erstellt und Ihre erste Abfrage ausgeführt. Wenn Sie mehr über die Resource Graph-Sprache erfahren möchten, fahren Sie mit der Seite mit den Details zur Abfragesprache fort.

> [!div class="nextstepaction"]
> [Erfahren Sie mehr über die Abfragesprache](./concepts/query-language.md).