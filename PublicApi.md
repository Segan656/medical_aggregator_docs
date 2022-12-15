# Public API

1. [Specialist](#specialist)
   1. [GET specialities](#get-specialities)
   1. [GET specialists](#get-specialists)
   1. [GET specialists/{specialist\_id}](#get-specialistsspecialist_id)
   1. [POST specialists/{specialist\_id}/review](#post-specialistsspecialist_idreview)
1. [Clinic](#clinic)
   1. [POST clinic](#post-clinic)
1. [Service](#service)
   1. [POST Service](#post-service)

## Specialist

### GET specialities

Allow to get list specialities

| Attribute | Value |
| :- | :- |
Current version | GET v1/specialities
Min supported version | GET v1/specialities
Availability | opened
Auth Access | JWT token

#### Enter data

none

#### Logic

1. EDCg
1. Return values
   1. For each Speciality, sorting in ascending of Speciality.name
      1. Speciality.id
      1. Speciality.name

### GET specialists

Allow to get list of specialists

| Attribute | Value |
| :- | :- |
Current version | GET v1/specialists
Min supported version | GET v1/specialists
Availability | opened
Auth Access | JWT token

#### Enter data

Attribute | Value | Data type | Required
| :- | :- | :- | :- |
speciality | {speciality_id} | int | no

#### Logic

1. EDCg
1. BLAC
   1. If {speciality_id} exists, identify Speciality by {speciality_id}
      1. Define it as CurrentSpeciality
      1. If CurrentSpeciality does not exist, return an error
1. If CurrentSpeciality exists
   1. Define FiltredSpecialists[] as a collection of Specialist, which Specialist.specialities[] includes CurrentSpeciality
1. Else
   1. Define FiltredSpecialists[] as a collection of all Specialist
1. Sort elements of FiltredSpecialists[] in descending of Specialist.rating
1. Return values
   1. For each element in FiltredSpecialists[] (Define it as CurrentSpecialist)
      1. CurrentSpecialist.full_name
      1. CurrentSpecialist.photo
      1. CurrentSpecialist.rating
      1. CurrentSpecialist.reviews[].count
      1. For each element in CurrentSpecialist.specialities[]
         1. Speciality.id
         1. Speciality.name
      1. CurrentSpecialist.experience
      1. For each element in CurrentSpecialist.clinics[]
         1. Clinic.name
         1. Clinic.id

### GET specialists/{specialist_id}

Allow to get general information about specialist

| Attribute | Value |
| :- | :- |
Current version | GET v1/specialists/{specialist_id}
Min supported version | GET v1/specialists/{specialist_id}
Availability | opened
Auth Access | JWT token

#### Enter data

Attribute | Value | Data type | Required
| :- | :- | :- | :- |
id | {specialist_id} | int | yes

#### Logic

1. EDCg
1. BLAC
   1. Identify Specialist by {specialist_id}
      1. Define it as CurrentSpecialist
      1. If CurrentSpecialist does not found, return an error
1. Return values
   1. CurrentSpecialist.full_name
   1. CurrentSpecialist.photo
   1. CurrentSpecialist.rating
   1. CurrentSpecialist.reviews[].count
   1. For each element in CurrentSpecialist.specialities[]
      1. Speciality.id
      1. Speciality.name
   1. CurrentSpecialist.description
   1. CurrentSpecialist.experience
   1. CurrentSpecialist.education
   1. CurrentSpecialist.courses
   1. CurrentSpecialist.jobs
   1. CurrentSpecialist.labels[]
   1. CurrentSpecialist.languages
   1. For each element in CurrentSpecialist.services[], sorting in ascending of SpecialistService.position
      1. SpecialistService.type.id
      1. SpecialistService.type.name
      1. SpecialistService.description exists ? SpecialistService.description : SpecialistService.type.description
      1. SpecialistService.base_price
      1. SpecialistService.new_price
      1. SpecialistService.discount
      1. SpecialistService.clinic_branch.clinic.name
      1. SpecialistService.clinic_branch.name
      1. SpecialistService.clinic_branch.full_address
      1. SpecialistService.clinic_branch.metro_stations[]
      1. SpecialistService.clinic_branch.contacts[]
   1. For each element in Specialist.reviews[]
      1. Review.id
      1. Last 4 symbols of Review.author.phone
      1. Review.author.name
      1. Review.visit_date
      1. Review.story
      1. Review.rating
      1. Review.liked
      1. Review.not_liked
      1. For each element in Review.services[]
         1. SpecialistService.type.name
         1. SpecialistService.type.id

### POST specialists/{specialist_id}/review

Allow to create review for specialist

| Attribute | Value |
| :- | :- |
Current version | POST v1/specialists/{id}/review
Min supported version | POST v1/specialists/{id}/review
Availability | rescheduling
Auth Access | JWT token

#### Enter data

Attribute | Value | Data type | Required
| :- | :- | :- | :- |
JWT token | {jwt} | text | yes
id | {specialist_id} | int | yes
services | {services_ids} | int [] | yes
clinic_branch | {clinic_branch_id} | int | yes
rating | {rating} | int | yes
visit_date | {visit_date} | timestamp | yes
story | {story} | text | yes
liked | {liked} | text | no
not_liked | {not_liked} | text | no

#### Logic

1. EDCg
   1. If {rating} > 5 or {rating} < 1, return an error
1. ATC
1. BLAC
   1. Identify Specialist by {specialist_id}
      1. Define it as CurrentSpecialist
      1. If CurrentSpecialist does not found, return an error
   1. For each element in {services_ids} identify Service
      1. Define it as CurrentService
      1. If CurrentService does not found, return an error
      1. If CurrentService.specialist <> CurrentSpecialist or does not exist, return an error
   1. Identify ClinicBranch by {clinic_branch_id}
      1. Define it as CurrentBranch
      1. If CurrentBranch does not exist, return an error
1. Create Review with parameters
   1. Review.status = Draft
   1. Review.visit_date = {visit_date}
   1. For each element in {services_ids} identify Service
      1. Add service into Review.services[]
   1. Review.clinic_branch = CurrentBranch
   1. Review.rating = {rating}
   1. Review.story = {story}
   1. Review.liked = {liked}
   1. Review.not_liked = {not_liked}
1. Return created Review.id

## Clinic

### POST clinic

Allow to create clinic

| Attribute | Value |
| :- | :- |
Current version | POST v1/clinic
Min supported version | POST v1/clinic
Availability | rescheduling
Auth Access | JWT token

#### Enter data

Attribute | Value | Data type | Required
| :- | :- | :- | :- |
JWT token | {jwt} | text | yes
name | {clinic_name} | text | yes
address | {address} | text | yes

#### Logic

1. EDCg
1. ATC
1. Identify Clinic by {clinic_name}
   1. If Clinic exists, define it as CurrentClinic
   1. Else, create new Clinic with attributes
      1. Define it as CurrentClinic
      1. CurrentClinic.name = {clinic_name}
      1. CurrentClinic.status = Draft
1. Create ClinicBranch with attributes
   1. ClinicBranch.full_address = {address}
   1. ClinicBranch.clinic = CurrentClinic
   1. ClinicBranch.name = {clinic_name} + " (" + {address} + " )"
   1. ClinicBranch.status = Draft
1. Return ClinicBranch.id

## Service

### POST Service

Allow to create service

| Attribute | Value |
| :- | :- |
Current version | POST v1/service
Min supported version | POST v1/service
Availability | rescheduling
Auth Access | JWT token

#### Enter data

Attribute | Value | Data type | Required
| :- | :- | :- | :- |
JWT token | {jwt} | text | yes
name | {service_name} | text | yes
specialist | {specialist_id} | int | yes
clinic_branch | {clinic_branch_id} | int | yes

#### Logic

1. EDCg
1. ATC
1. BLAC
   1. Identify Specialist by {specialist_id}
      1. Define it as CurrentSpecialist
      1. If CurrentSpecialist does not exist, return an error
   1. Identify ClinicBranch by {clinic_branch_id}
      1. Define it as CurrentBranch
      1. If CurrentBranch does not exist, return an error
1. Create new ServiceType with attributes
   1. Define it as CurrentServiceType
   1. CurrentServiceType.name = {service_name}
   1. CurrentServiceType.provider = ServiceProvider.specialist
   1. CurrentServiceType.is_active = false
1. Create Service with attributes
   1. Service.type = CurrentServiceType
   1. Service.specialist = CurrentSpecialist
   1. Service.clinic_branch = CurrentBranch
1. Return Service.id
