# FHIR Resources
Here, we present the schema of each resource type and its link ([reference](https://hl7.org/fhir/resourcelist.html)).
> Based on HL7 FHIR Version 5.0 (as of 2025-06-11) 

&nbsp;

## Resource Types
### 1. [Patient](https://hl7.org/fhir/patient.html)
<details>
<summary>Patient scheme</summary>

For more details for each data type of the schema, please see [here](https://hl7.org/fhir/patient.html).
```json
{
  "resourceType" : "Patient",
  // from Resource: id, meta, implicitRules, and language
  // from DomainResource: text, contained, extension, and modifierExtension
  "identifier" : [{ Identifier }], // An identifier for this patient
  "active" : <boolean>, // Whether this patient's record is in active use
  "name" : [{ HumanName }], // A name associated with the patient
  "telecom" : [{ ContactPoint }], // A contact detail for the individual
  "gender" : "<code>", // male | female | other | unknown
  "birthDate" : "<date>", // The date of birth for the individual
  // deceased[x]: Indicates if the individual is deceased or not. One of these 2:
  "deceasedBoolean" : <boolean>,
  "deceasedDateTime" : "<dateTime>",
  "address" : [{ Address }], // An address for the individual
  "maritalStatus" : { CodeableConcept }, // Marital (civil) status of a patient
  // multipleBirth[x]: Whether patient is part of a multiple birth. One of these 2:
  "multipleBirthBoolean" : <boolean>,
  "multipleBirthInteger" : <integer>,
  "photo" : [{ Attachment }], // Image of the patient
  "contact" : [{ // A contact party (e.g. guardian, partner, friend) for the patient
    "relationship" : [{ CodeableConcept }], // The kind of relationship
    "name" : { HumanName }, // I A name associated with the contact person
    "telecom" : [{ ContactPoint }], // I A contact detail for the person
    "address" : { Address }, // I Address for the contact person
    "gender" : "<code>", // male | female | other | unknown
    "organization" : { Reference(Organization) }, // I Organization that is associated with the contact
    "period" : { Period } // The period during which this contact person or organization is valid to be contacted relating to this patient
  }],
  "communication" : [{ // A language which may be used to communicate with the patient about his or her health
    "language" : { CodeableConcept }, // R!  The language which can be used to communicate with the patient about his or her health
    "preferred" : <boolean> // Language preference indicator
  }],
  "generalPractitioner" : [{ Reference(Organization|Practitioner|
   PractitionerRole) }], // Patient's nominated primary care provider
  "managingOrganization" : { Reference(Organization) }, // Organization that is the custodian of the patient record
  "link" : [{ // Link to a Patient or RelatedPerson resource that concerns the same actual individual
    "other" : { Reference(Patient|RelatedPerson) }, // R!  The other patient or related person resource that the link refers to
    "type" : "<code>" // R!  replaced-by | replaces | refer | seealso
  }]
}
```
</details>

&nbsp;

### 2. [Schedule](https://hl7.org/fhir/schedule.html)
<details>
<summary>Schedule scheme</summary>

For more details for each data type of the schema, please see [here](https://hl7.org/fhir/schedule.html).
```json
{
  "resourceType" : "Schedule",
  // from Resource: id, meta, implicitRules, and language
  // from DomainResource: text, contained, extension, and modifierExtension
  "identifier" : [{ Identifier }], // External Ids for this item
  "active" : <boolean>, // Whether this schedule is in active use
  "serviceCategory" : [{ CodeableConcept }], // High-level category
  "serviceType" : [{ CodeableReference(HealthcareService) }], // Specific service
  "specialty" : [{ CodeableConcept }], // Type of specialty needed
  "name" : "<string>", // Human-readable label
  "actor" : [{ Reference(CareTeam|Device|HealthcareService|Location|Patient|
   Practitioner|PractitionerRole|RelatedPerson) }], // R!  Resource(s) that availability information is being provided for
  "planningHorizon" : { Period }, // Period of time covered by schedule
  "comment" : "<markdown>" // Comments on availability
}
```
</details>

&nbsp;

### 3. [Location](https://hl7.org/fhir/location.html)
<details>
<summary>Location scheme</summary>

For more details for each data type of the schema, please see [here](https://hl7.org/fhir/location.html).
```json
{
  "resourceType" : "Location",
  // from Resource: id, meta, implicitRules, and language
  // from DomainResource: text, contained, extension, and modifierExtension
  "identifier" : [{ Identifier }], // Unique code or number identifying the location to its users
  "status" : "<code>", // active | suspended | inactive
  "operationalStatus" : { Coding }, // The operational status of the location (typically only for a bed/room) icon
  "name" : "<string>", // Name of the location as used by humans
  "alias" : ["<string>"], // A list of alternate names that the location is known as, or was known as, in the past
  "description" : "<markdown>", // Additional details about the location that could be displayed as further information to identify the location beyond its name
  "mode" : "<code>", // instance | kind
  "type" : [{ CodeableConcept }], // Type of function performed icon
  "contact" : [{ ExtendedContactDetail }], // Official contact details for the location
  "address" : { Address }, // Physical location
  "form" : { CodeableConcept }, // Physical form of the location
  "position" : { // The absolute geographic location
    "longitude" : <decimal>, // R!  Longitude with WGS84 datum
    "latitude" : <decimal>, // R!  Latitude with WGS84 datum
    "altitude" : <decimal> // Altitude with WGS84 datum
  },
  "managingOrganization" : { Reference(Organization) }, // Organization responsible for provisioning and upkeep
  "partOf" : { Reference(Location) }, // Another Location this one is physically a part of
  "characteristic" : [{ CodeableConcept }], // Collection of characteristics (attributes)
  "hoursOfOperation" : [{ Availability }], // What days/times during a week is this location usually open (including exceptions)
  "virtualService" : [{ VirtualServiceDetail }], // Connection details of a virtual service (e.g. conference call)
  "endpoint" : [{ Reference(Endpoint) }] // Technical endpoints providing access to services operated for the location
}
```
</details>

