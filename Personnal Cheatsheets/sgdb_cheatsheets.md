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
| Firebird | Relationnel | Firebird est un SGBDR open source qui supporte les transactions ACID et est connu pour sa légèreté et sa flexibilité. |
| Ingres | Relationnel | Ingres est un SGBDR open source qui offre des fonctionnalités avancées et est utilisé pour des applications critiques. |
| SAP HANA | Relationnel | SAP HANA est une plateforme de données en mémoire qui permet des analyses en temps réel et est utilisée dans des applications d'entreprise. |
| Amazon RDS | Hybride | Amazon RDS permet de déployer des SGBDR relationnels dans le cloud, supportant MySQL, PostgreSQL, MariaDB, Oracle et SQL Server. |
| CockroachDB | Hybride | CockroachDB, bien que classée comme NewSQL, offre des capacités relationnelles avec une architecture distribuée, supportant des transactions ACID. |
| Google Cloud SQL | Hybride | Google Cloud SQL est un service de base de données relationnelle entièrement géré, supportant MySQL, PostgreSQL et SQL Server. |
| Azure SQL Database | Hybride | Azure SQL Database est un service cloud relationnel géré par Microsoft, offrant des fonctionnalités de scalabilité et de sécurité. |
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

## SQL ou NoSQL
### Utiliser du SQL
- **Données structurés :** Données organisées dans des tables avec des schémas bien définis, facilitant la gestion des relations entre les différentes entités.
- **Transactions critiques :** Applications qui nécessitent des transactions sécurisées et conformes aux normes ACID (systèmes bancaires ou de gestion de la chaîne d'approvisionnement).
- **Intégrité des données :** Contraintes de validation, garantissant l'intégrité et la cohérence des données tout au long de leur cycle de vie.
- **Analytique et reporting :** Exécuter des requêtes analytiques complexes ou des rapports basés sur des jointures. Offrir des outils robustes pour la génération de rapports.
- **Scalabilité verticale :** Augmenter la puissance d'une seule machine. Les bases de données relationnelles ont historiquement été conçues pour se développer verticalement.

### Ne pas utiliser du SQL
- **Documents complexes :** JSON, PDF, images codées en base64, et autres formats complexes.
- **Données hétérogènes :** Données qui changent fréquemment en structure ou en format.
- **Données non structurées :** Informations sans format fixe ou organisation définie (journaux de serveurs, données issues de capteurs).
- **Volumes massifs de données (plus de 3M d'entrées) :** Données massives nécessitant un stockage et une gestion distribuée.
- **Flux de données en temps réel :** Appareils générant des données en continu et en grande volumétrie.

### Utiliser du NoSQL
- **Scalabilité horizontale :** Se développer facilement en ajoutant des serveurs supplémentaires (idéal pour des applications à grande échelle).
- **Flexibilité des données :** Stocker des données non structurées ou semi-structurées comme du JSON + documents complexes et hétérogènes.
- **Haute disponibilité :** Assurer une disponibilité continue même en cas de panne d'un ou plusieurs noeuds.
- **Performance en lecture/écriture :** Nécessité d'écritures ou de lectures rapides sur de grands volumes de données.
- **Clés/valeurs** : Données accessibles via des paires clé-valeur.

### Ne pas utiliser du NoSQL
- **Transactions complexes :** Applications nécessitant des transactions ACID.
- **Relations de données strictes :** Modèle de données fortement structuré avec de nombreuses relations entre les entités.
- **Reporting et analytics avancés :** Des requêtes analytiques complexes et des rapports basés sur des jointures multiples.
- **Standardisation et conformité :** Environnements où des normes strictes de conformité et de sécurité des données doivent être respectées.

⭐⭐⭐ I hope you enjoy it, if so don't hesitate to leave a like on this repository and on the "Settings" one (click on the "Star" button at the top right of the repository page). Thanks 🤗