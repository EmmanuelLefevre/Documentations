# SGBD

## SQL
| SQL | Type | Description |
| :---: | :---: | :--- |
| MySQL | Relationnel | MySQL est un SGBDR open source populaire, largement utilisé pour le développement d'applications web, offrant des transactions ACID. |
| PostgreSQL | Relationnel | PostgreSQL est un SGBDR open source avancé qui supporte des types de données variés, des transactions ACID et des extensions pour des fonctionnalités avancées. |
| Microsoft SQL Server | Relationnel | SQL Server est un SGBDR développé par Microsoft, souvent utilisé dans les environnements d'entreprise, offrant des fonctionnalités robustes de sécurité et d'analyse. |
| Oracle Database | Relationnel | Oracle Database est un SGBDR commercial puissant, connu pour sa scalabilité, sa sécurité et ses capacités de gestion des données complexes. |
| SQLite | Relationnel | SQLite est une base de données relationnelle légère, souvent intégrée dans des applications mobiles ou embarquées, supportant une grande partie des fonctionnalités SQL. |
| MariaDB | Relationnel | MariaDB est un fork de MySQL qui offre des fonctionnalités améliorées et est conçu pour être compatible avec les applications MySQL existantes. |
| IBM Db2 | Relationnel | Db2 est un SGBDR développé par IBM, utilisé dans les environnements d'entreprise pour le traitement de grandes quantités de données. |
| Teradata | Relationnel | Teradata est un SGBDR spécialisé dans l'analyse des données, souvent utilisé pour des applications de big data et de data warehousing. |
| Amazon RDS | Hybride | Amazon RDS permet de déployer des SGBDR relationnels dans le cloud, supportant MySQL, PostgreSQL, MariaDB, Oracle et SQL Server. |
| CockroachDB | Hybride | CockroachDB, bien que classée comme NewSQL, offre des capacités relationnelles avec une architecture distribuée, supportant des transactions ACID. |
| Google Cloud SQL | Hybride | Google Cloud SQL est un service de base de données relationnelle entièrement géré, supportant MySQL, PostgreSQL et SQL Server. |
| Azure SQL Database | Hybride | Azure SQL Database est un service cloud relationnel géré par Microsoft, offrant des fonctionnalités de scalabilité et de sécurité. |
| Firebird | Relationnel | Firebird est un SGBDR open source qui supporte les transactions ACID et est connu pour sa légèreté et sa flexibilité. |
| Ingres | Relationnel | Ingres est un SGBDR open source qui offre des fonctionnalités avancées et est utilisé pour des applications critiques. |
| SAP HANA | Relationnel | SAP HANA est une plateforme de données en mémoire qui permet des analyses en temps réel et est utilisée dans des applications d'entreprise. |
| NuoDB | Hybride | NuoDB est une base de données relationnelle distribuée qui combine les caractéristiques des SGBDR traditionnels avec une architecture distribuée. |
| TimescaleDB | Hybride | TimescaleDB est une extension de PostgreSQL optimisée pour le traitement de séries temporelles, utilisée pour des applications de monitoring et d'analyse de données temporelles. |

## NoSQL
| BDD | Type | Description |
| :---: | :---: | :---: |
| MongoDB | Document | 	MongoDB utilise le modèle document, stockant les données sous forme de documents JSON, ce qui est flexible pour les données semi-structurées. |
| Cassandra | Colonne | Basée sur un modèle de colonnes, Cassandra est très populaire pour la gestion de données distribuées à grande échelle. |
| Elasticsearch | Document | Elasticsearch est un moteur de recherche et d'analyse basé sur Apache Lucene, utilisé pour la recherche plein texte et l'analyse des données. |
| GraphDB | Graphe | GraphDB est une base de données orientée graphe optimisée pour la gestion de données RDF et les requêtes SPARQL. |
| HBase | Colonne | Basée sur le modèle de colonnes, HBase est souvent utilisée pour des données massives (Big Data). |
| Zookeeper | Dictionnaire | Zookeeper est un magasin clé-valeur distribué, principalement utilisé pour la configuration et le consensus dans les systèmes distribués. |
| Google Spanner | Hybride | Bien qu'elle supporte le modèle relationnel avec des transactions ACID, elle offre aussi des capacités distribuées et tolérantes aux pannes, donc souvent classée en NewSQL. |
| NewSQL | Hybride | NewSQL regroupe des bases de données qui fournissent des transactions ACID dans des systèmes distribués, avec des structures similaires aux bases SQL traditionnelles. |
| CockroachDB	 | Hybride | Inspirée de Google Spanner, elle est transactionnelle, orientée vers les systèmes distribués, donc classée NewSQL. |
| DynamoDB | Dictionnaire | 	DynamoDB est un magasin clé-valeur orienté vers la haute disponibilité, offrant une bonne flexibilité pour des données simples et répliquées. |
| Riak | Dictionnaire | Riak est un magasin clé-valeur avec une bonne tolérance aux pannes, offrant une cohérence éventuelle. |
| Couchbase | Document | Couchbase est une base de données orientée document, optimisée pour les performances et souvent utilisée dans les applications web. |
| Neo4j | Graphe |  Neo4j est une base de données orientée graphe qui permet de gérer des relations complexes entre les données via un modèle de nœuds et d'arêtes. |
| Amazon Neptune | Graphe | Amazon Neptune est une base de données orientée graphe entièrement gérée, prenant en charge les modèles de graphe de type Property Graph et RD. |
| ArangoDB | Hybride | ArangoDB est une base de données multimodèle NoSQL qui prend en charge les documents, les graphes et les clés-valeurs, offrant une flexibilité élevée. |
| OrientDB | Hybride | OrientDB est une base de données multimodèle qui supporte à la fois les modèles de graphe et de document. |
| FaunaDB | Hybride | FaunaDB est une base de données distribuée supportant les modèles document, relationnel et clé-valeur, avec des transactions ACID. |
| RavenDB | Document | RavenDB est une base de données orientée document qui offre des fonctionnalités de réplication et de sharding. |
| Couchbase (support de graphes) | Hybride | Couchbase offre des capacités de graphe en plus de son modèle orienté document, permettant une gestion flexible des relations entre les données. |
| Amazon Neptune (configuration lecture seule) | Graphe | En mode lecture seule, Amazon Neptune assure une haute disponibilité et peut être utilisé pour interroger des données de manière fiable, même en cas de partitionnement. |
