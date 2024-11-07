# SQL

## SOMMAIRE
- [INTRODUCTION](#introduction)
- [DATA DEFINITION LANGUAGE (DDL)](#data-definition-language-ddl)
- [DATA MANIPULATION LANGUAGE (DML)](#data-manipulation-language-dml)
- [DATA QUERY LANGUAGE (DQL)](#data-query-language-dql)
- [DATA CONTROL LANGUAGE (DCL)](#data-control-language-dcl)
- [TRANSACTION CONTROL LANGUAGE (TCL)](#transaction-control-language-tcl)
- [SYSTEM COMMANDS](#system-commands)
- [USER COMMANDS](#system-commands)

## INTRODUCTION
SQL (Structured Query Language) est un langage informatique standardisé utilisé pour opérer des bases de données relationnelles. La partie langage de manipulation des données de SQL permet de rechercher, d'ajouter, de modifier ou de supprimer des données dans des bases de données relationnelles.

## DATA DEFINITION LANGUAGE (DDL)
| Command | Description |
| :---: | :---: |
| `CREATE DATABASE <db>` | Créer une nouvelle base de données |
| `DROP DATABASE <db>` | Supprimer une base de données |
| `CREATE TABLE <table> (<column_1> <column_1_type>, <column_2> <column_2_type>)` | Créer une nouvelle table |
| `DROP TABLE <table>` | Supprimer une table |
| `ALTER TABLE <table> ADD COLUMN <column> <column_type>` | Ajouter une nouvelle colonne à une table |
| `ALTER TABLE <table> DROP COLUMN <column>` | Supprimer une colonne d'une table |
| `ALTER TABLE <table> RENAME COLUMN <column> TO <new_column>` | Renommer une colonne dans une table |
| `ALTER TABLE <table> RENAME TO <new_column>` | Renommer une table |
| `TRUNCATE TABLE <table>` | Supprimer toutes les lignes d'une table |
| `CREATE INDEX INDEXNAME ON <table> (<column>)` | Créer un index sur une table |
| `DROP INDEX INDEXNAME` | Supprimer un index d'une table |

## DATA MANIPULATION LANGUAGE (DML)
| Command | Description |
| :---: | :---: |
| `INSERT INTO <table> (<column_1>, <column_2>) VALUES (<value_1>, <value_2>)` | Insérer une nouvelle ligne dans une table |
| `SELECT * FROM <table>` | Récupérer toutes les lignes d'une table |
| `UPDATE <table> SET <column_1> = <value_1> WHERE <column_2> = <value_2>` | Mettre à jour des lignes dans une table |
| `DELETE FROM <table> WHERE <column> = <value>` | Supprimer des lignes d'une table |

## DATA QUERY LANGUAGE (DQL)
| Command | Description |
| :---: | :---: |
| `SELECT <column_1>, <column_2> FROM <table>` | Récupérer des colonnes spécifiques d'une table |
| `SELECT * FROM <table> WHERE <column> = VALUE` | Récupérer des lignes correspondant à une condition |
| `SELECT * FROM <table> ORDER BY <column>` | Récupérer des lignes triées par une colonne |
| `SELECT * FROM <table> LIMIT N` | Récupérer les N premières lignes d'une table |
| `SELECT * FROM <table> OFFSET N` | Récupérer les lignes à partir de la Nème ligne |
| `SELECT * FROM <table> JOIN <table_2> ON <table_1>.<column_1> = <table_2>.<column_2>` | Récupérer des lignes de deux tables correspondant à une condition |
| `SELECT * FROM <table> UNION SELECT * FROM <table_2>` | Récupérer des lignes de deux tables sans doublons |
| `SELECT * FROM <table> GROUP BY <column>` | Regrouper les lignes ayant la même valeur dans une colonne |
| `SELECT * FROM <table> HAVING COUNT(<column>) > N` | Filtrer les lignes regroupées ayant plus de N lignes |
| `SELECT * FROM <table> WHERE <column> IN (<value_1>, <value_2>)` | Récupérer des lignes où la valeur d'une colonne correspond à une valeur d'une liste |
| `SELECT * FROM <table> WHERE <column> BETWEEN <value_1> AND <value_2>` | Récupérer des lignes où la valeur d'une colonne est dans une plage |
| `SELECT * FROM <table> WHERE <column> LIKE '<value>%'` | Récupérer des lignes où la valeur d'une colonne correspond à un modèle |
| `SELECT * FROM <table> WHERE <column> IS NULL` | Récupérer des lignes où la valeur d'une colonne est NULL |
| `SELECT * FROM <table> WHERE <column> IS NOT NULL` | Récupérer des lignes où la valeur d'une colonne n'est pas NULL |

## DATA CONTROL LANGUAGE (DCL)
| Command | Description |
| :---: | :---: |
| `GRANT PERMISSIONS ON <table> TO <user>` | Accorder des permissions à un utilisateur |
| `REVOKE PERMISSIONS ON <table> FROM <user>` | Révoquer des permissions d'un utilisateur |

## TRANSACTION CONTROL LANGUAGE (TCL)
| Command | Description |
| :---: | :---: |
| `BEGIN TRANSACTION` | Démarrer une nouvelle transaction |
| `COMMIT` | Enregistrer les modifications dans la base de données |
| `ROLLBACK` | Annuler les modifications dans la base de données |
| `SAVEPOINT <savepoint>` | Créer un point de sauvegarde dans une transaction |
| `ROLLBACK TO SAVEPOINT <savepoint>` | Rétablir à un point de sauvegarde dans une transaction |
| `RELEASE SAVEPOINT <savepoint>` | Supprimer un point de sauvegarde dans une transaction |

## SYSTEM COMMANDS
| Command | Description |
| :---: | :---: |
| `SHOW DATABASES` | Lister toutes les bases de données |
| `SHOW TABLES` | Lister toutes les tables dans la base de données actuelle |
| `SHOW COLUMNS FROM <table>` | Lister toutes les colonnes dans une table |
| `SHOW INDEXES FROM <table>` | Lister tous les index dans une table |
| `SHOW CREATE TABLE <table>` | Afficher la déclaration SQL qui crée une table |
| `DESCRIBE <table>` | Afficher la structure d'une table |
| `EXPLAIN SELECT * FROM <table>` | Afficher le plan d'exécution d'une requête |
| `SET autocommit = 0` | Désactiver le mode d'autocommit |
| `SET autocommit = 1` | Activer le mode d'autocommit |
| `SET FOREIGN_KEY_CHECKS = 0` | Désactiver les vérifications de clés étrangères |

## USER COMMANDS
| Command | Description |
| :---: | :---: |
| `CREATE <user> '<user>'@'<host>' IDENTIFIED BY '<password>'` | Créer un nouvel utilisateur |
| `DROP <user> '<user>'@'<host>'` | Supprimer un utilisateur |
| `ALTER <user> '<user>'@'<host>' IDENTIFIED BY '<password>'` | Changer le mot de passe d'un utilisateur |
| `GRANT ALL PRIVILEGES ON <db> TO '<user>'@'<host>'` | Accorder tous les privilèges à un utilisateur |
| `REVOKE ALL PRIVILEGES ON <db> FROM '<user>'@'<host>'` | Révoquer tous les privilèges d'un utilisateur |
| `FLUSH PRIVILEGES` | Recharger les privilèges des tables de droits |

***

⭐⭐⭐ I hope you enjoy it, if so don't hesitate to leave a like on this repository and on the "Settings" one (click on the "Star" button at the top right of the repository page). Thanks 🤗
