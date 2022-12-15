# Data Model

1. [State-Machines](#state-machines)
   1. [Base State-Machine (BSM)](#base-state-machine-bsm)
   1. [User](#user)
   1. [Specialist](#specialist)
   1. [SpecialistService](#specialistservice)
   1. [ClinicService](#clinicservice)
   1. [Clinic](#clinic)
   1. [ClinicBranch](#clinicbranch)
   1. [ClinicReview](#clinicreview)
   1. [SpecialistReview](#specialistreview)
1. [Dictionaries](#dictionaries)
   1. [Speciality](#speciality)
   1. [ServiceType](#servicetype)
   1. [TreatmentProfile](#treatmentprofile)
1. [Enums](#enums)
   1. [ServiceProvider](#serviceprovider)

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
 [*] --> Active 
 Deleted --> [*]
 Active --> Deleted
```

#### Attributes

| Attribute | Data type |
| :- | :- |
| full_name * | text |
| photo * | text |
| specialities * | Speciality [] |
| description | text |
| experience * | int |
| education | text |
| courses | text |
| contacts | text [] |
| jobs | text |
| languages | text |
| labels | text [] |
| rating | float |
| treatment_profiles | TreatmentProfile [] |
| clinics * | Clinic [] |

#### Calculated attributes

| Attribute | Data type | Description
| :- | :- | :- |
| reviews | Review[] | All Review with Review.specialist == CurrentSpecialist and Review.status == Active |
| services | SpecialistService[] | All Service with SpecialistService.specialist == CurrentSpecialist |

### SpecialistService

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
| type * | ServiceType |
| description | text |
| base_price | float |
| new_price | float |
| discount | int |
| sorting_position * | int |
| specialist * | Specialist |
| clinic_branch * | ClinicBranch |

### ClinicService

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
| type * | ServiceType |
| description | text |
| base_price | float |
| new_price | float |
| discount | int |
| sorting_position * | int |
| clinic_branch * | ClinicBranch |

### Clinic

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
| description | text |
| photos | text [] |
| contacts | text [] |
| branches | ClinicBranch [] |

### ClinicBranch

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
| clinic * | Clinic |
| full_address * | text |
| metro_stations | text [] |
| contacts | text [] |

### ClinicReview

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
| services * | ClinicService [] |
| rating * | int |
| story * | text |
| liked | text |
| not_liked | text |

### SpecialistReview

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
| services * | SpecialistService [] |
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
| id * | int |
| name * | text |

### ServiceType

| Attribute | Data type |
| :- | :- |
| id * | int |
| name * | text |
| description | text |
| is_active | bool |
| provider* | ServiceProvider |

### TreatmentProfile

| Attribute | Data type |
| :- | :- |
| id * | int |
| name * | text |
| description | text |

## Enums

### ServiceProvider

| id | value |
| :- | :- |
| 0 | clinik |
| 1 | specialist |
