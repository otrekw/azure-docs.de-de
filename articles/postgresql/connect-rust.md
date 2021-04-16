---
title: 'Schnellstart: Herstellen einer Verbindung mit Rust: Azure Database for PostgreSQL-Einzelserver'
description: Diese Schnellstartanleitung enthält Rust-Codebeispiele, die Sie verwenden können, um eine Verbindung mit einem Azure Database for PostgreSQL-Einzelserver herzustellen und Daten daraus abzufragen.
author: abhirockzz
ms.author: abhishgu
ms.service: postgresql
ms.devlang: rust
ms.topic: quickstart
ms.date: 03/26/2021
ms.openlocfilehash: 00adc50ac14e627eb356a3e62ce0faa5a9716aa3
ms.sourcegitcommit: c2a41648315a95aa6340e67e600a52801af69ec7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/06/2021
ms.locfileid: "106505552"
---
# <a name="quickstart-use-rust-to-connect-and-query-data-in-azure-database-for-postgresql---single-server"></a>Schnellstart: Verwenden von Rust zum Herstellen einer Verbindung mit einem Azure Database for PostgreSQL-Einzelserver sowie zum Abfragen von Daten

In diesem Artikel erfahren Sie, wie Sie den [PostgresSQL-Treiber für Rust](https://github.com/sfackler/rust-postgres) verwenden, um mit Azure Database for PostgreSQL zu interagieren. Zu diesem Zweck werden CRUD-Vorgänge (Erstellen, Lesen, Aktualisieren, Löschen) betrachtet, die im Beispielcode implementiert sind. Zum Abschluss können Sie die Anwendung lokal ausführen, um sie in Aktion zu sehen.

## <a name="prerequisites"></a>Voraussetzungen
Für diese Schnellstartanleitung benötigen Sie Folgendes:

- Ein Azure-Konto mit einem aktiven Abonnement. Sie können [kostenlos ein Konto erstellen](https://azure.microsoft.com/free).
- Eine aktuelle Version von [Rust](https://www.rust-lang.org/tools/install) ist installiert.
- Erstellen Sie einen Azure Database for PostgreSQL-Einzelserver. Verwenden Sie dazu das [Azure-Portal](./quickstart-create-server-database-portal.md) <br/> oder die [Azure-CLI](./quickstart-create-server-database-azure-cli.md).
- Je nachdem, ob Sie öffentlichen oder privaten Zugriff verwenden, führen Sie **EINE** der folgenden Aktionen aus, um Konnektivität herzustellen:

  |Action| Konnektivitätsmethode|Schrittanleitung|
  |:--------- |:--------- |:--------- |
  | **Konfigurieren von Firewallregeln** | Öffentlich | [Portal](./howto-manage-firewall-using-portal.md) <br/> [BEFEHLSZEILENSCHNITTSTELLE (CLI)](./howto-manage-firewall-using-cli.md)|
  | **Konfigurieren des Dienstendpunkts** | Öffentlich | [Portal](./howto-manage-vnet-using-portal.md) <br/> [BEFEHLSZEILENSCHNITTSTELLE (CLI)](./howto-manage-vnet-using-cli.md)|
  | **Konfigurieren von Private Link** | Privat | [Portal](./howto-configure-privatelink-portal.md) <br/> [BEFEHLSZEILENSCHNITTSTELLE (CLI)](./howto-configure-privatelink-cli.md) |

- [Git](https://git-scm.com/downloads) installiert.

## <a name="get-database-connection-information"></a>Abrufen von Datenbankverbindungsinformationen
Für die Verbindungsherstellung mit einer Azure Database for PostgreSQL-Datenbank sind der vollqualifizierte Servername und Anmeldeinformationen erforderlich. Diese Informationen finden Sie im Azure-Portal.

1. Suchen Sie im [Azure-Portal](https://portal.azure.com/) nach dem Namen Ihres Azure Database for PostgreSQL-Servers, und wählen Sie ihn aus.
1. Kopieren Sie auf der **Übersichtsseite** des Servers unter **Servername** den vollqualifizierten Servernamen und unter **Administratorbenutzername** den Administratorbenutzernamen. Der vollqualifizierte **Servername** hat immer das Format *\<my-server-name>.postgres.database.azure.com*. Der **Administratorbenutzername** hat immer das Format *\<my-admin-username>@\<my-server-name>* .

## <a name="review-the-code-optional"></a>Überprüfen des Codes (optional)

Falls Sie erfahren möchten, wie der Code funktioniert, können Sie sich die folgenden Codeausschnitte ansehen. Andernfalls können Sie mit [Ausführen der Anwendung](#run-the-application) fortfahren.

### <a name="connect"></a>Verbinden

Die Funktion `main` beginnt mit dem Herstellen einer Verbindung mit Azure Database for PostgreSQL. Sie ist abhängig davon, dass die folgenden Umgebungsvariablen die Konnektivitätsinformationen `POSTGRES_HOST`, `POSTGRES_USER`, `POSTGRES_PASSWORD` und `POSTGRES_DBNAME` zur Verfügung stellen. Standardmäßig ist der PostgreSQL-Datenbankdienst so konfiguriert, dass `TLS`-Verbindungen erforderlich sind. Sie können das Erfordern von `TLS` deaktivieren, wenn Ihre Clientanwendung keine `TLS`-Verbindungen unterstützt. Weitere Informationen finden Sie unter [Konfigurieren der TLS-Konnektivität in Azure Database for PostgreSQL (Einzelserver)](./concepts-ssl-connection-security.md).

Die Beispielanwendung in diesem Artikel verwendet TLS mit [postgres-openssl crate](https://crates.io/crates/postgres-openssl/). Die [Postgres::Client::connect](https://docs.rs/postgres/0.19.0/postgres/struct.Client.html#method.connect)-Funktion wird verwendet, um die Verbindung zu initiieren, und das Programm wird beendet, wenn dies fehlschlägt.

```rust
fn main() {
    let pg_host = std::env::var("POSTGRES_HOST").expect("missing environment variable POSTGRES_HOST");
    let pg_user = std::env::var("POSTGRES_USER").expect("missing environment variable POSTGRES_USER");
    let pg_password = std::env::var("POSTGRES_PASSWORD").expect("missing environment variable POSTGRES_PASSWORD");
    let pg_dbname = std::env::var("POSTGRES_DBNAME").unwrap_or("postgres".to_string());

    let builder = SslConnector::builder(SslMethod::tls()).unwrap();
    let tls_connector = MakeTlsConnector::new(builder.build());

    let url = format!(
        "host={} port=5432 user={} password={} dbname={} sslmode=require",
        pg_host, pg_user, pg_password, pg_dbname
    );
    let mut pg_client = postgres::Client::connect(&url, tls_connector).expect("failed to connect to postgres");
...
}
```

### <a name="drop-and-create-table"></a>Löschen und Erstellen von Tabellen

Die Beispielanwendung verwendet eine einfache `inventory`-Tabelle, um die CRUD-Vorgänge (erstellen, lesen, aktualisieren, löschen) zu veranschaulichen.

```sql
CREATE TABLE inventory (id serial PRIMARY KEY, name VARCHAR(50), quantity INTEGER);
```

Die Funktion `drop_create_table` versucht anfänglich, die `inventory`-Tabelle zu `DROP`, bevor sie eine neue erstellt. Dies erleichtert das Lernen und Experimentieren, da Sie immer mit einem bekannten (sauberen) Zustand beginnen. Die [execute](https://docs.rs/postgres/0.19.0/postgres/struct.Client.html#method.execute)-Methode wird für Vorgänge zum Erstellen und Löschen verwendet. 

```rust
const CREATE_QUERY: &str =
    "CREATE TABLE inventory (id serial PRIMARY KEY, name VARCHAR(50), quantity INTEGER);";

const DROP_TABLE: &str = "DROP TABLE inventory";

fn drop_create_table(pg_client: &mut postgres::Client) {
    let res = pg_client.execute(DROP_TABLE, &[]);
    match res {
        Ok(_) => println!("dropped table"),
        Err(e) => println!("failed to drop table {}", e),
    }
    pg_client
        .execute(CREATE_QUERY, &[])
        .expect("failed to create 'inventory' table");
}
```

### <a name="insert-data"></a>Einfügen von Daten

`insert_data` fügt Einträge zur Tabelle `inventory` hinzu. Mit der [prepare](https://docs.rs/postgres/0.19.0/postgres/struct.Client.html#method.prepare)-Funktion wird eine [vorbereitete Anweisung](https://docs.rs/postgres/0.19.0/postgres/struct.Statement.html) erstellt. 


```rust
const INSERT_QUERY: &str = "INSERT INTO inventory (name, quantity) VALUES ($1, $2) RETURNING id;";

fn insert_data(pg_client: &mut postgres::Client) {

    let prep_stmt = pg_client
        .prepare(&INSERT_QUERY)
        .expect("failed to create prepared statement");

    let row = pg_client
        .query_one(&prep_stmt, &[&"item-1", &42])
        .expect("insert failed");

    let id: i32 = row.get(0);
    println!("inserted item with id {}", id);
...
}
```

Beachten Sie auch die Verwendung der [prepare_typed](https://docs.rs/postgres/0.19.0/postgres/struct.Client.html#method.prepare_typed)-Methode, mit der die Arten der Abfrageparameter explizit angegeben werden können.

```rust
...
let typed_prep_stmt = pg_client
        .prepare_typed(&INSERT_QUERY, &[Type::VARCHAR, Type::INT4])
        .expect("failed to create prepared statement");

let row = pg_client
        .query_one(&typed_prep_stmt, &[&"item-2", &43])
        .expect("insert failed");

let id: i32 = row.get(0);
println!("inserted item with id {}", id);
...
```

Schließlich wird eine `for`Schleife verwendet, um `item-3`, `item-4` und `item-5` mit jeweils zufällig generierter Menge hinzuzufügen.

```rust
...
    for n in 3..=5 {
        let row = pg_client
            .query_one(
                &typed_prep_stmt,
                &[
                    &("item-".to_owned() + &n.to_string()),
                    &rand::thread_rng().gen_range(10..=50),
                ],
            )
            .expect("insert failed");

        let id: i32 = row.get(0);
        println!("inserted item with id {} ", id);
    }
...
```

### <a name="query-data"></a>Daten abfragen

Die `query_data`-Funktion zeigt, wie Daten aus der Tabelle `inventory` abgerufen werden. Die [query_one](https://docs.rs/postgres/0.19.0/postgres/struct.Client.html#method.query_one)-Methode wird verwendet, um ein Element mittels seiner `id` zu erhalten. 

```rust
const SELECT_ALL_QUERY: &str = "SELECT * FROM inventory;";
const SELECT_BY_ID: &str = "SELECT name, quantity FROM inventory where id=$1;";

fn query_data(pg_client: &mut postgres::Client) {

    let prep_stmt = pg_client
        .prepare_typed(&SELECT_BY_ID, &[Type::INT4])
        .expect("failed to create prepared statement");

    let item_id = 1;

    let c = pg_client
        .query_one(&prep_stmt, &[&item_id])
        .expect("failed to query item");

    let name: String = c.get(0);
    let quantity: i32 = c.get(1);
    println!("quantity for item {} = {}", name, quantity);
...
}
```

Alle Zeilen in der Bestandstabelle werden mithilfe einer `select * from` Abfrage mit der [query](https://docs.rs/postgres/0.19.0/postgres/struct.Client.html#method.query)-Methode abgerufen. Die zurückgegebenen Zeilen werden nacheinander durchsucht, um den Wert für jede Spalte mithilfe von [get](https://docs.rs/postgres/0.19.0/postgres/row/struct.Row.html#method.get) zu extrahieren. 

> [!TIP]
> Beachten Sie, wie `get` es ermöglicht, die Spalte entweder durch ihren numerischen Index in der Zeile oder durch ihren Spaltennamen anzugeben.

```rust
...
    let items = pg_client
        .query(SELECT_ALL_QUERY, &[])
        .expect("select all failed");

    println!("listing items...");

    for item in items {
        let id: i32 = item.get("id");
        let name: String = item.get("name");
        let quantity: i32 = item.get("quantity");
        println!(
            "item info: id = {}, name = {}, quantity = {} ",
            id, name, quantity
        );
    }
...
```

### <a name="update-data"></a>Aktualisieren von Daten

Die `update_date`-Funktion aktualisiert die Menge für alle Elemente nach dem Zufallsprinzip. Da die `insert_data`-Funktion `5` Zeilen hinzugefügt hat, wird dasselbe in der `for`-Schleife berücksichtigt: `for n in 1..=5`

> [!TIP]
> Beachten Sie, dass wir `query` anstelle von `execute` verwenden, da wir beabsichtigen, den `id` und den neu generierten `quantity` (mithilfe der [RETURNING-Klausel](https://www.postgresql.org/docs/current/dml-returning.html)) zurückzuholen.

```rust
const UPDATE_QUERY: &str = "UPDATE inventory SET quantity = $1 WHERE name = $2 RETURNING quantity;";

fn update_data(pg_client: &mut postgres::Client) {
    let stmt = pg_client
        .prepare_typed(&UPDATE_QUERY, &[Type::INT4, Type::VARCHAR])
        .expect("failed to create prepared statement");

    for id in 1..=5 {
        let row = pg_client
            .query_one(
                &stmt,
                &[
                    &rand::thread_rng().gen_range(10..=50),
                    &("item-".to_owned() + &id.to_string()),
                ],
            )
            .expect("update failed");
        
        let quantity: i32 = row.get("quantity");
        println!("updated item id {} to quantity = {}", id, quantity);
    }
}
```

### <a name="delete-data"></a>Löschen von Daten

Schließlich wird durch die `delete`-Funktion veranschaulicht, wie ein Element durch sein `id` aus der Tabelle `inventory` entfernt wird. `id` wird nach dem Zufallsprinzip ausgewählt. Dies ist eine zufällige ganze Zahl zwischen `1` und `5` (einschließlich `5`), da die `insert_data`-Funktion anfänglich `5` Zeilen hinzugefügt hat. 

> [!TIP]
> Beachten Sie, dass wir `query` anstelle von `execute` verwenden, da wir beabsichtigen, die Informationen über das soeben gelöschte Element zurückzuholen (mithilfe der [RETURNING-Klausel](https://www.postgresql.org/docs/current/dml-returning.html)).

```rust
const DELETE_QUERY: &str = "DELETE FROM inventory WHERE id = $1 RETURNING id, name, quantity;";

fn delete(pg_client: &mut postgres::Client) {
    let stmt = pg_client
        .prepare_typed(&DELETE_QUERY, &[Type::INT4])
        .expect("failed to create prepared statement");

    let item = pg_client
        .query_one(&stmt, &[&rand::thread_rng().gen_range(1..=5)])
        .expect("delete failed");

    let id: i32 = item.get(0);
    let name: String = item.get(1);
    let quantity: i32 = item.get(2);
    println!(
        "deleted item info: id = {}, name = {}, quantity = {} ",
        id, name, quantity
    );
}
```

## <a name="run-the-application"></a>Ausführen der Anwendung

1. Beginnen Sie damit, dass Sie den folgenden Befehl ausführen, um das Beispielrepository zu klonen:

    ```bash
    git clone https://github.com/Azure-Samples/azure-postgresql-rust-quickstart.git
    ```

2. Legen Sie die erforderlichen Umgebungsvariablen mit den Werten fest, die Sie aus dem Azure-Portal kopiert haben:

    ```bash
    export POSTGRES_HOST=<server name e.g. my-server.postgres.database.azure.com>
    export POSTGRES_USER=<admin username e.g. my-admin-user@my-server>
    export POSTGRES_PASSWORD=<admin password>
    export POSTGRES_DBNAME=<database name. it is optional and defaults to postgres>
    ```

3. Wechseln Sie zum Ausführen der Anwendung in das Verzeichnis, in dem Sie es geklont haben, und führen Sie `cargo run` aus:

    ```bash
    cd azure-postgresql-rust-quickstart
    cargo run
    ```

    Daraufhin sollte eine Ausgabe ähnlich der folgenden angezeigt werden:

    ```bash
    dropped 'inventory' table
    inserted item with id 1
    inserted item with id 2
    inserted item with id 3 
    inserted item with id 4 
    inserted item with id 5 
    quantity for item item-1 = 42
    listing items...
    item info: id = 1, name = item-1, quantity = 42 
    item info: id = 2, name = item-2, quantity = 43 
    item info: id = 3, name = item-3, quantity = 11 
    item info: id = 4, name = item-4, quantity = 32 
    item info: id = 5, name = item-5, quantity = 24 
    updated item id 1 to quantity = 27
    updated item id 2 to quantity = 14
    updated item id 3 to quantity = 31
    updated item id 4 to quantity = 16
    updated item id 5 to quantity = 10
    deleted item info: id = 4, name = item-4, quantity = 16 
    ```

4. Zur Bestätigung können Sie auch mithilfe von [psql](./quickstart-create-server-database-portal.md#connect-to-the-server-with-psql) eine Verbindung mit Azure Database for PostgreSQL herstellen und Abfragen auf die Datenbank ausführen, z. B.:

    ```sql
    select * from inventory;
    ```

[Treten Probleme auf? Informieren Sie uns darüber.](https://aka.ms/postgres-doc-feedback)

## <a name="clean-up-resources"></a>Bereinigen von Ressourcen

Löschen Sie die Ressourcengruppe mit dem folgenden Befehl, um alle in dieser Schnellstartanleitung verwendeten Ressourcen zu bereinigen:

```azurecli
az group delete \
    --name $AZ_RESOURCE_GROUP \
    --yes
```

## <a name="next-steps"></a>Nächste Schritte
> [!div class="nextstepaction"]
> [Verwalten eines Azure Database for PostgreSQL-Servers mithilfe des Portals](./howto-create-manage-server-portal.md)<br/>

> [!div class="nextstepaction"]
> [Verwalten eines Azure Database for PostgreSQL-Servers mithilfe der CLI](./how-to-manage-server-cli.md)<br/>

[Können Sie die gesuchten Informationen nicht finden? Informieren Sie uns darüber.](https://aka.ms/postgres-doc-feedback)
