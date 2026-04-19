# HBnB – Technical Documentation  

## TASK 0 - High-Level Package Diagram  

---

## Overview  
This document presents a high-level package diagram of the HBnB Evolution application. It describes a three-layer architecture and illustrates how these layers communicate using the Facade design pattern.  

The purpose of this documentation is to provide a clear understanding of the system structure, its components, and how different parts interact with each other.

---

## Architecture Overview  
The HBnB application is designed using a layered architecture consisting of three main layers:  

1. Presentation Layer  
2. Business Logic Layer  
3. Persistence Layer  

Each layer has a specific responsibility and interacts with other layers in a structured and controlled manner.

---

## Layer Descriptions  

### 1. Presentation Layer  
This layer represents the interface between the user and the system. It includes API endpoints and services that handle incoming user requests.  

When a user performs an action, the request is received in this layer and passed through the Facade.

---
### 2. Business Logic Layer  
This layer contains the core logic of the application. It includes the main entities:  

- User  
- Place  
- Review  
- Amenity  

Responsibilities include applying business rules, validating data, and controlling application behavior.

---

### 3. Persistence Layer  
This layer is responsible for storing and retrieving data from the database.  

It includes:  
- UserRepository  
- PlaceRepository  
- ReviewRepository  
- AmenityRepository  

---

## Facade Pattern  

The Facade acts as an intermediary between layers and provides a unified interface.

### Benefits:  
- Single entry point  
- Reduced complexity  
- Improved maintainability  

---

## Package Diagram

```mermaid
classDiagram

class PresentationLayer {
    +API Endpoints
    +Services
}

class Facade {
    +createUser()
    +getPlaces()
    +addReview()
    +addAmenity()
}

class BusinessLogicLayer {
    +User
    +Place
    +Review
    +Amenity
}

class PersistenceLayer {
    +UserRepository
    +PlaceRepository
    +ReviewRepository
    +AmenityRepository
}

PresentationLayer --> Facade : Uses
Facade --> BusinessLogicLayer : Handles business logic
BusinessLogicLayer --> PersistenceLayer : Database operations

```

------


# TASK 1 -Business Logic Layer

   ## Overview
   
      This document describes the **Business Logic layer** of the HBnB application. It provides a detailed UML 
      class diagram representing the core entities of the system, their attributes, methods, and relationships.
      The main goal is to clearly model how the business logic of the application is structured and how the main 
      entities interact with each other.
   
---
   
   ## Business Logic Layer
   
      The Business Logic layer contains the core entities of the application:
      - User
      - Place
      - Review
      - Amenity
      These entities define the main functionality of the system and enforce business rules.
   
---

## Class Diagram

```mermaid
classDiagram
    class BaseModel {
        +UUID4 id
        +DateTime created_at
        +DateTime updated_at
        +save()
        +update(data)
    }

    class User {
        +String first_name
        +String last_name
        +String email
        +String password
        +Boolean is_admin
        +register()
        +update_profile()
    }

    class Place {
        +String title
        +String description
        +Float price
        +Float latitude
        +Float longitude
        +UUID4 owner_id
        +create()
        +update()
    }

    class Review {
        +Int rating
        +String comment
        +UUID4 place_id
        +UUID4 user_id
        +post()
    }

    class Amenity {
        +String name
        +String description
        +create()
    }

    %% Relationships
    User --|> BaseModel : Inherits
    Place --|> BaseModel : Inherits
    Review --|> BaseModel : Inherits
    Amenity --|> BaseModel : Inherits

    User "1" --> "0..*" Place : Owns
    Place "1" --> "0..*" Review : Has
    User "1" --> "0..*" Review : Writes
    Place "0..*" -- "0..*" Amenity : Includes

```

---


# TASK 2 - API Calls

   ## Overview
      This document shows 2 main API flows in the HBnB application using sequence diagrams.  
      Each diagram illustrates how the Presentation, Business Logic, and Persistence layers interact.
   
---

# User Registration

```mermaid
sequenceDiagram
participant User
participant API
participant Facade
participant BusinessLogic
participant Database

User->>API: POST /users
API->>Facade: createUser(data)
Facade->>BusinessLogic: validate & create
BusinessLogic->>Database: save user
Database-->>BusinessLogic: OK
BusinessLogic-->>Facade: user created
Facade-->>API: success
API-->>User: 201 Created
```
# Review Submission Flow

```mermaid
sequenceDiagram
participant User
participant API
participant Facade
participant BusinessLogic
participant Database

User->>API: POST /reviews
API->>Facade: submitReview(data)
Facade->>BusinessLogic: validate review
BusinessLogic->>Database: save review
Database-->>BusinessLogic: OK
BusinessLogic-->>Facade: review saved
Facade-->>API: success
API-->>User: 201 Created
```
 # Place Creation
 ```mermaid
sequenceDiagram
participant User
participant API
participant Facade
participant BusinessLogic
participant Database

User->>API: POST /places
API->>Facade: createPlace(data)
Facade->>BusinessLogic: validate place
BusinessLogic->>Database: save place
Database-->>BusinessLogic: OK
BusinessLogic-->>Facade: place created
Facade-->>API: success
API-->>User: 201 Created
```
# Fetching a List of Places
 ```mermaid
sequenceDiagram
participant User
participant API
participant Facade
participant BusinessLogic
participant Database

User->>API: GET /places
API->>Facade: getPlaces(filters)
Facade->>BusinessLogic: fetch data
BusinessLogic->>Database: query places
Database-->>BusinessLogic: results
BusinessLogic-->>Facade: places list
Facade-->>API: response
API-->>User: 200 OK + data
```

 ---