# Student Management — Spring Boot & Swagger

Application Spring Boot de gestion des étudiants (CRUD + statistiques) exposant une API REST documentée avec Swagger / OpenAPI.

## Sommaire

- [Présentation](#présentation)
- [Technologies](#technologies)
- [Architecture du projet](#architecture-du-projet)
- [Prérequis](#prérequis)
- [Installation](#installation)
- [Configuration de la base de données](#configuration-de-la-base-de-données)
- [Lancement de l'application](#lancement-de-lapplication)
- [Endpoints REST](#endpoints-rest)
- [Documentation Swagger](#documentation-swagger)
- [Tests unitaires](#tests-unitaires)

## Présentation

Ce projet illustre une architecture Spring Boot en couches :

- **Entité** : représentation métier persistée en base de données.
- **Repository** : accès aux données via Spring Data JPA (aucun SQL manuel).
- **Service** : logique métier.
- **Controller REST** : exposition des endpoints HTTP au format JSON.

## Technologies

| Composant | Version |
| --- | --- |
| Java | 8 |
| Spring Boot | 2.7.18 |
| Spring Web | starter-web |
| Spring Data JPA / Hibernate | starter-data-jpa |
| Spring Data REST | starter-data-rest |
| Bean Validation | starter-validation |
| MySQL Connector/J | `com.mysql:mysql-connector-j` |
| springdoc-openapi (Swagger UI) | 1.6.15 |
| JUnit 5 / Mockito | starter-test |
| Build | Maven |

## Architecture du projet

```
student-management
├── pom.xml
└── src
    ├── main
    │   ├── java/com/example/studentmanagement
    │   │   ├── StudentManagementApplication.java
    │   │   ├── controllers/StudentController.java
    │   │   ├── entities/Student.java
    │   │   ├── repositories/StudentRepository.java
    │   │   └── services/StudentService.java
    │   └── resources/application.properties
    └── test/java/com/example/studentmanagement/StudentControllerTest.java
```

## Prérequis

- JDK 8 ou supérieur
- Maven 3.6+
- MySQL 5.7+ (ou 8.x) démarré en local

## Installation

Cloner le dépôt puis compiler le projet :

```bash
git clone https://github.com/lahlalimouad6-ux/Spring-Boot-et-Swagger.git
cd Spring-Boot-et-Swagger
mvn clean install
```

## Configuration de la base de données

1. Créer la base de données :

```sql
CREATE DATABASE studentdb;
```

2. Renseigner les paramètres de connexion dans `src/main/resources/application.properties` :

```properties
spring.datasource.url=jdbc:mysql://localhost:3306/studentdb?serverTimezone=UTC
spring.datasource.username=root
spring.datasource.password=

spring.jpa.show-sql=true
spring.jpa.properties.hibernate.format_sql=true
spring.jpa.hibernate.ddl-auto=update
```

- `spring.jpa.hibernate.ddl-auto=update` : Hibernate crée ou met à jour automatiquement la table `student` à partir de l'entité `Student`.
- En cas d'erreur de connexion, vérifier le mot de passe MySQL et le port utilisé.

## Lancement de l'application

Depuis la racine du projet :

```bash
mvn spring-boot:run
```

Ou lancer la classe `StudentManagementApplication` directement depuis l'IDE.

Un démarrage réussi affiche dans la console :

```
Started StudentManagementApplication in X seconds (JVM running for X.X)
```

L'application écoute par défaut sur `http://localhost:8080`.

## Endpoints REST

| Méthode | Endpoint | Code attendu | Description |
| --- | --- | --- | --- |
| POST | `/students/save` | 201 Created | Ajouter un étudiant |
| GET | `/students/all` | 200 OK | Liste complète des étudiants |
| DELETE | `/students/delete/{id}` | 204 No Content / 404 Not Found | Supprimer un étudiant |
| GET | `/students/count` | 200 OK | Nombre total d'étudiants |
| GET | `/students/byYear` | 200 OK | Nombre d'étudiants par année de naissance |

### Exemples

Ajouter un étudiant :

```bash
curl -X POST http://localhost:8080/students/save \
  -H "Content-Type: application/json" \
  -d '{"nom":"LACHGAR","prenom":"Mohamed","dateNaissance":"1985-09-01"}'
```

Réponse :

```json
{
  "id": 1,
  "nom": "LACHGAR",
  "prenom": "Mohamed",
  "dateNaissance": "1985-09-01"
}
```

Liste des étudiants :

```bash
curl http://localhost:8080/students/all
```

Répartition par année de naissance :

```json
[
  [1985, 2],
  [1990, 1]
]
```

## Documentation Swagger

L'interface Swagger UI est générée automatiquement par springdoc-openapi :

- Swagger UI : http://localhost:8080/swagger-ui.html
- Spécification OpenAPI (JSON) : http://localhost:8080/v3/api-docs

Elle permet de consulter et tester tous les endpoints directement depuis le navigateur.

## Tests unitaires

Les tests du contrôleur utilisent JUnit 5 et Mockito (le service est simulé, aucune base de données n'est requise) :

```bash
mvn test
```

Le rapport est également visible dans l'IDE (Eclipse, IntelliJ IDEA, VS Code).
