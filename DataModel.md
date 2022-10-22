# Data Model

<!---------------------------
---------AGGREGATES----------
 --------------------------->

## State-Machines

### Base State-Machine (BSM)

This is generic entity for other machines. Other machines can modify BMS lifecycle but must to inherit its attributes.

#### Lifecycle

```mermaid
stateDiagram
 [*] --> Active
 Deleted --> [*]

 Active --> Deleted 
```

#### Attributes

| Attribute | Data type |
| :- | :- |
| status * | int |
| status_message | text |
| created_at * | timestamp |
| updated_at * | timestamp |
| author * | User |

### User

#### Lifecycle

```mermaid
stateDiagram
 [*] --> Active
 Deleted --> [*]

 Active --> Deleted
```

#### Attributes

| Attribute | Data type |
| :- | :- |
| name * | text |
| phone * | text |

### Specialist

#### Lifecycle

```mermaid
stateDiagram
 [*] --> Draft 
 Deleted --> [*]
 Draft --> Active
 Active --> Draft
 Active --> Deleted
```

#### Attributes

| Attribute | Data type |
| :- | :- |
| name * | text |
| photo * | text |
| specialities * | Speciality [] |
| description | text |
| experience * | int |
| education | text |
| courses | text |
| contacts | text [] |
| jobs | text |
| labels | text [] |
| treatment_profiles | TreatmentProfile [] |
| clinics * | ClinicBranch [] |

### Service

#### Lifecycle

```mermaid
stateDiagram
 [*] --> Draft
 Deleted --> [*]
 Draft --> Active
 Active --> Draft
 Active --> Deleted
```

#### Attributes

| Attribute | Data type |
| :- | :- |
| type * | ServiceType |
| description | text |
| price * | float |
| discount | int |
| specialist | Specialist |
| clinic | ClinicBranch |

### Clinic

#### Lifecycle

```mermaid
stateDiagram
 [*] --> Draft
 Deleted --> [*]
 Draft --> Active
 Active --> Draft
 Active --> Deleted
```

#### Attributes

| Attribute | Data type |
| :- | :- |
| name * | text |
| description | text |
| photos * | text [] |
| references | text [] |
| branches * | ClinicBranch [] |

### ClinicBranch

#### Lifecycle

```mermaid
stateDiagram
 [*] --> Draft
 Deleted --> [*]
 Draft --> Active
 Active --> Draft
 Active --> Deleted
```

#### Attributes

| Attribute | Data type |
| :- | :- |
| name * | text |
| clinic * | Clinic |
| full_address * | text |
| metro_stations | text [] |
| contacts | text [] |

#### Review

#### Lifecycle

```mermaid
stateDiagram
 [*] --> Draft
 Deleted --> [*]
 Draft --> Active
 Active --> Draft
 Active --> Deleted
```

#### Attributes

| Attribute | Data type |
| :- | :- |
| visit_date * | timestamp |
| service * | Service |
| rating * | int |
| story * | text |
| liked | text |
| not_liked | text |

<!---------------------------
---------DICTIONARIES--------
---------------------------->

## Dictionaries

There is no generic entity

### Speciality

| Attribute | Data type |
| :- | :- |
| code * | int |
| name * | text |

### ServiceType

| Attribute | Data type |
| :- | :- |
| code * | int |
| name * | text |
| specialist * | bool |
| clinic * | bool |
| description | text |

### TreatmentProfile

| Attribute | Data type |
| :- | :- |
| code * | int |
| name * | text |
| description | text |
