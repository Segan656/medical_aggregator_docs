# Memorandum

## Arrangements

1. By default deleted state-machines (status == deleted) should be ignored by searching logic. If we need to include deleted aggregates into searching logic, we have to describe it in specification.
2. While Transaction is creating, it should change the balance on Transaction.recipient and Transaction.sender automatically. We do not describe this logic.
3. A = B is the same as "make A equal B". A == B is the same as "is A equal B? true/false"
4. By default User who creates state-machine or fact must be defined as author of this entity

### Frequently Used Construction Keys (FUCK)

#### ATC - access JWT check

Verify structure, expired period and sign of access JWT. If we have an error in this block of logic, return 401.

#### EDCg - enter data check (general)

Verify existence of required enter parameters and format of all enter parameters. We cant use database or external sources to implement this check. If we have an error in this block of logic, return 400.
All routing parameters are required.
All body parameters with * after name are required.

#### EDCa - enter data check (additional)

Verify enter parameters by using data from database or different external sources. This check should be executed after EDCg. If we have an error in this block of logic, return 409.

#### BLAC - business logic accessing check

Custom business logic to verify access to specific data. If we have an error in this block of logic, return 403.

## Data types

### General

1. text
2. bool
3. int
4. uint
5. real
6. ureal
7. jsonb
8. collection
9. entity
10. timestamp

### Entities

1. State-Machine - represent entity with some lifecycle and can change in time
2. Fact - represent an event (phenomenon) that happened a place in the past and cannot be changed at any time in the future
3. Structure - group of attributes
4. Dictionary - flat handbook (key-value) which values can be changed in runtime (via CP for example)
5. ENUM -  flat handbook (key-value) which values can not be changed in runtime. Developer define ENUM in code and you can change it just with new release/deploy.

## Writing style

1. Entities - camelcase
2. Attributes and ENUM values - snakecase.
3. Variables - {{value}}
4. Statuses - one word starting with capital letter
5. Obligatory attribute in entity description must be marked by * after the name
6. One empty line (minimum) before and after each title
7. Attribute type of collection must be named in plural form

### Naming handlers

1. Used types of queries:
   1. **POST** - to create entity.
   2. **DELETE** - to delete entity.
   3. **PATCH** - to modify existing entity.
2. Used plural in the name of the entity collection query.
