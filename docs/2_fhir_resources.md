# FHIR Resources
Here, we present the schema of each resource type and its link ([reference](https://hl7.org/fhir/resourcelist.html)).
> Based on HL7 FHIR Version 5.0 (as of 2025-06-11) 

&nbsp;

## Resource Type Examples
### Workflow
#### 1. [Appointment](https://hl7.org/fhir/appointment.html)
Reference resource type:
* `Slot`
* `CareTeam|Device|Group|HealthcareService|Location|Patient|Practitioner|PractitionerRole|RelatedPerson`
* ...
><details>
><summary>Appointment scheme</summary>
>A booking of a healthcare event among patient(s), practitioner(s), related person(s) and/or device(s) for a specific date/time. This may result in one or more Encounter(s).
>
><br>Appointment resources are used to provide information about a planned meeting that may be in the future or past.
>The resource only describes a single meeting, a series of repeating visits would require multiple appointment resources to be created for each instance.
>Examples include a scheduled surgery, a follow-up for a clinical visit, a scheduled conference call between clinicians to discuss a case (where the patient is a subject, but not a participant), the reservation of a piece of diagnostic equipment for a particular use, etc.
>The visit scheduled by an appointment may be in person or remote (by phone, video conference, etc.)
>All that matters is that the time and usage of one or more individuals, locations and/or pieces of equipment is being fully or partially reserved for a designated period of time.
>
><br>This definition takes the concepts of appointments in a clinical setting and also extends them to be relevant in the community healthcare space, and to ease exposure to other appointment / calendar standards widely used outside of healthcare.
>
>For more details for each data type of the schema, please see [here](https://hl7.org/fhir/appointment.html).
>```json
>{
>  "resourceType" : "Appointment",
>  // from Resource: id, meta, implicitRules, and language
>  // from DomainResource: text, contained, extension, and modifierExtension
>  "identifier" : [{ Identifier }], // External Ids for this item
>  "status" : "<code>", // I R!  proposed | pending | booked | arrived | fulfilled | cancelled | noshow | entered-in-error | checked-in | waitlist
>  "cancellationReason" : { CodeableConcept }, // I The coded reason for the appointment being cancelled
>  "class" : [{ CodeableConcept }], // Classification when becoming an encounter icon
>  "serviceCategory" : [{ CodeableConcept }], // A broad categorization of the service that is to be performed during this appointment
>  "serviceType" : [{ CodeableReference(HealthcareService) }], // The specific service that is to be performed during this appointment
>  "specialty" : [{ CodeableConcept }], // The specialty of a practitioner that would be required to perform the service requested in this appointment
>  "appointmentType" : { CodeableConcept }, // The style of appointment or patient that has been booked in the slot (not service type) icon
>  "reason" : [{ CodeableReference(Condition|ImmunizationRecommendation|
>   Observation|Procedure) }], // Reason this appointment is scheduled
>  "priority" : { CodeableConcept }, // Used to make informed decisions if needing to re-prioritize icon
>  "description" : "<string>", // Shown on a subject line in a meeting request, or appointment list
>  "replaces" : [{ Reference(Appointment) }], // Appointment replaced by this Appointment
>  "virtualService" : [{ VirtualServiceDetail }], // Connection details of a virtual service (e.g. conference call)
>  "supportingInformation" : [{ Reference(Any) }], // Additional information to support the appointment
>  "previousAppointment" : { Reference(Appointment) }, // The previous appointment in a series
>  "originatingAppointment" : { Reference(Appointment) }, // I The originating appointment in a recurring set of appointments
>  "start" : "<instant>", // I When appointment is to take place
>  "end" : "<instant>", // I When appointment is to conclude
>  "minutesDuration" : "<positiveInt>", // Can be less than start/end (e.g. estimate)
>  "requestedPeriod" : [{ Period }], // Potential date/time interval(s) requested to allocate the appointment within
>  "slot" : [{ Reference(Slot) }], // The slots that this appointment is filling
>  "account" : [{ Reference(Account) }], // The set of accounts that may be used for billing for this Appointment
>  "created" : "<dateTime>", // The date that this appointment was initially created
>  "cancellationDate" : "<dateTime>", // I When the appointment was cancelled
>  "note" : [{ Annotation }], // Additional comments
>  "patientInstruction" : [{ CodeableReference(Binary|Communication|
>   DocumentReference) }], // Detailed information and instructions for the patient
>  "basedOn" : [{ Reference(CarePlan|DeviceRequest|MedicationRequest|
>   ServiceRequest) }], // The request this appointment is allocated to assess
>  "subject" : { Reference(Group|Patient) }, // The patient or group associated with the appointment
>  "participant" : [{ // R!  Participants involved in appointment
>    "type" : [{ CodeableConcept }], // I Role of participant in the appointment
>    "period" : { Period }, // Participation period of the actor
>    "actor" : { Reference(CareTeam|Device|Group|HealthcareService|Location|
>    Patient|Practitioner|PractitionerRole|RelatedPerson) }, // I The individual, device, location, or service participating in the appointment
>    "required" : <boolean>, // The participant is required to attend (optional when false)
>    "status" : "<code>" // R!  accepted | declined | tentative | needs-action
>  }],
>  "recurrenceId" : "<positiveInt>", // The sequence number in the recurrence
>  "occurrenceChanged" : <boolean>, // Indicates that this appointment varies from a recurrence pattern
>  "recurrenceTemplate" : [{ // I Details of the recurrence pattern/template used to generate occurrences
>    "timezone" : { CodeableConcept }, // The timezone of the occurrences
>    "recurrenceType" : { CodeableConcept }, // R!  The frequency of the recurrence
>    "lastOccurrenceDate" : "<date>", // The date when the recurrence should end
>    "occurrenceCount" : "<positiveInt>", // The number of planned occurrences
>    "occurrenceDate" : ["<date>"], // Specific dates for a recurring set of appointments (no template)
>    "weeklyTemplate" : { // Information about weekly recurring appointments
>      "monday" : <boolean>, // Recurs on Mondays
>      "tuesday" : <boolean>, // Recurs on Tuesday
>      "wednesday" : <boolean>, // Recurs on Wednesday
>      "thursday" : <boolean>, // Recurs on Thursday
>      "friday" : <boolean>, // Recurs on Friday
>      "saturday" : <boolean>, // Recurs on Saturday
>      "sunday" : <boolean>, // Recurs on Sunday
>      "weekInterval" : "<positiveInt>" // Recurs every nth week
>    },
>    "monthlyTemplate" : { // Information about monthly recurring appointments
>      "dayOfMonth" : "<positiveInt>", // Recurs on a specific day of the month
>      "nthWeekOfMonth" : { Coding }, // Indicates which week of the month the appointment should occur
>      "dayOfWeek" : { Coding }, // Indicates which day of the week the appointment should occur
>      "monthInterval" : "<positiveInt>" // R!  Recurs every nth month
>    },
>    "yearlyTemplate" : { // Information about yearly recurring appointments
>      "yearInterval" : "<positiveInt>" // R!  Recurs every nth year
>    },
>    "excludingDate" : ["<date>"], // Any dates that should be excluded from the series
>    "excludingRecurrenceId" : ["<positiveInt>"] // Any recurrence IDs that should be excluded from the recurrence
>  }]
>}
>```
>
>Real data example
>```json
>{
>  "resourceType" : "Appointment",
>  "id" : "example",
>  "status" : "booked",
>  "class" : [{
>    "coding" : [{
>      "system" : "http://terminology.hl7.org/CodeSystem/v3-ActCode",
>      "code" : "AMB",
>      "display" : "ambulatory"
>    }]
>  }],
>  "serviceCategory" : [{
>    "coding" : [{
>      "system" : "http://example.org/service-category",
>      "code" : "gp",
>      "display" : "General Practice"
>    }]
>  }],
>  "serviceType" : [{
>    "concept" : {
>      "coding" : [{
>        "code" : "52",
>        "display" : "General Discussion"
>      }]
>    }
>  }],
>  "specialty" : [{
>    "coding" : [{
>      "system" : "http://snomed.info/sct",
>      "code" : "394814009",
>      "display" : "General practice"
>    }]
>  }],
>  "appointmentType" : {
>    "coding" : [{
>      "system" : "http://terminology.hl7.org/CodeSystem/v2-0276",
>      "code" : "FOLLOWUP",
>      "display" : "A follow up visit from a previous appointment"
>    }]
>  },
>  "reason" : [{
>    "reference" : {
>      "reference" : "Condition/example",
>      "display" : "Severe burn of left ear"
>    }
>  }],
>  "description" : "Discussion on the results of your recent MRI",
>  "start" : "2013-12-10T09:00:00Z",
>  "end" : "2013-12-10T11:00:00Z",
>  "created" : "2013-10-10",
>  "note" : [{
>    "text" : "Further expand on the results of the MRI and determine the next actions that may be appropriate."
>  }],
>  "patientInstruction" : [{
>    "concept" : {
>      "text" : "Please avoid excessive travel (specifically flying) before this appointment"
>    }
>  }],
>  "basedOn" : [{
>    "reference" : "ServiceRequest/myringotomy"
>  }],
>  "subject" : {
>    "reference" : "Patient/example",
>    "display" : "Peter James Chalmers"
>  },
>  "participant" : [{
>    "actor" : {
>      "reference" : "Patient/example",
>      "display" : "Peter James Chalmers"
>    },
>    "required" : true,
>    "status" : "accepted"
>  },
>  {
>    "type" : [{
>      "coding" : [{
>        "system" : "http://terminology.hl7.org/CodeSystem/v3-ParticipationType",
>        "code" : "ATND"
>      }]
>    }],
>    "actor" : {
>      "reference" : "Practitioner/example",
>      "display" : "Dr Adam Careful"
>    },
>    "required" : true,
>    "status" : "accepted"
>  },
>  {
>    "actor" : {
>      "reference" : "Location/1",
>      "display" : "South Wing, second floor"
>    },
>    "required" : true,
>    "status" : "accepted"
>  }]
>}
>```
></details>

&nbsp;

#### 2. [AppointmentResponse](https://hl7.org/fhir/appointmentresponse.html)
Reference resource type:
* `Appointment`
><details>
><summary>AppointmentResponse scheme</summary>
>A reply to an appointment request for a patient and/or practitioner(s), such as a confirmation or rejection.
>
><br>Appointment resources are used to provide information about a planned meeting that may be in the future or past. They may be for a single meeting or for a series of repeating visits. Examples include a scheduled surgery, a follow-up for a clinical visit, a scheduled conference call between clinicians to discuss a case, the reservation of a piece of diagnostic equipment for a particular use, etc. The visit scheduled by an appointment may be in person or remote (by phone, video conference, etc.) All that matters is that the time and usage of one or more individuals, locations and/or pieces of equipment is being fully or partially reserved for a designated period of time.
>
><br>This definition takes the concepts of appointments in a clinical setting and also extends them to be relevant in the community healthcare space, and also ease exposure to other appointment / calendar standards widely used outside of Healthcare.
>
>For more details for each data type of the schema, please see [here](https://hl7.org/fhir/appointmentresponse.html).
>```json
>{
>  "resourceType" : "AppointmentResponse",
>  // from Resource: id, meta, implicitRules, and language
>  // from DomainResource: text, contained, extension, and modifierExtension
>  "identifier" : [{ Identifier }], // External Ids for this item
>  "appointment" : { Reference(Appointment) }, // R!  Appointment this response relates to
>  "proposedNewTime" : <boolean>, // Indicator for a counter proposal
>  "start" : "<instant>", // Time from appointment, or requested new start time
>  "end" : "<instant>", // Time from appointment, or requested new end time
>  "participantType" : [{ CodeableConcept }], // I Role of participant in the appointment
>  "actor" : { Reference(Device|Group|HealthcareService|Location|Patient|
>   Practitioner|PractitionerRole|RelatedPerson) }, // I Person(s), Location, HealthcareService, or Device
>  "participantStatus" : "<code>", // R!  accepted | declined | tentative | needs-action | entered-in-error
>  "comment" : "<markdown>", // Additional comments
>  "recurring" : <boolean>, // This response is for all occurrences in a recurring request
>  "occurrenceDate" : "<date>", // Original date within a recurring request
>  "recurrenceId" : "<positiveInt>" // The recurrence ID of the specific recurring request
>}
>```
>
>Real data example
>```json
>{
>  "resourceType" : "AppointmentResponse",
>  "id" : "example",
>  "appointment" : {
>    "reference" : "Appointment/example",
>    "display" : "Brian MRI results discussion"
>  },
>  "actor" : {
>    "reference" : "Patient/example",
>    "display" : "Peter James Chalmers"
>  },
>  "participantStatus" : "accepted"
>}
>```
></details>

&nbsp;

#### 3. [Schedule](https://hl7.org/fhir/schedule.html)
Reference resource type:
* `CareTeam|Device|HealthcareService|Location|Patient|Practitioner|PractitionerRole|RelatedPerson`
><details>
><summary>Schedule scheme</summary>
>A container for slots of time that may be available for booking appointments.
>
><br>Schedule resources provide a container for time-slots that can be booked using an appointment.
>It provides the window of time (period) that slots are defined for and what type of appointments can be booked. The schedule does not provide any information about actual appointments.
>This separation greatly assists where access to the appointments would not be permitted for security or privacy reasons, while still being able to determine if an appointment might be available.
>
><br>Note: A schedule is not used for the delivery of medication, the Timing data type should be used for that purpose.
>
>For more details for each data type of the schema, please see [here](https://hl7.org/fhir/schedule.html).
>```json
>{
>  "resourceType" : "Schedule",
>  // from Resource: id, meta, implicitRules, and language
>  // from DomainResource: text, contained, extension, and modifierExtension
>  "identifier" : [{ Identifier }], // External Ids for this item
>  "active" : <boolean>, // Whether this schedule is in active use
>  "serviceCategory" : [{ CodeableConcept }], // High-level category
>  "serviceType" : [{ CodeableReference(HealthcareService) }], // Specific service
>  "specialty" : [{ CodeableConcept }], // Type of specialty needed
>  "name" : "<string>", // Human-readable label
>  "actor" : [{ Reference(CareTeam|Device|HealthcareService|Location|Patient|
>   Practitioner|PractitionerRole|RelatedPerson) }], // R!  Resource(s) that availability information is being provided for
>  "planningHorizon" : { Period }, // Period of time covered by schedule
>  "comment" : "<markdown>" // Comments on availability
>}
>```
>
>Real data example
>```json
>{
>  "resourceType" : "Schedule",
>  "id" : "example",
>  "identifier" : [{
>    "use" : "usual",
>    "system" : "http://example.org/scheduleid",
>    "value" : "45"
>  }],
>  "active" : true,
>  "serviceCategory" : [{
>    "coding" : [{
>      "system" : "http://terminology.hl7.org/CodeSystem/service-category",
>      "code" : "17",
>      "display" : "General Practice"
>    }]
>  }],
>  "serviceType" : [{
>    "concept" : {
>      "coding" : [{
>        "system" : "http://terminology.hl7.org/CodeSystem/service-type",
>        "code" : "57",
>        "display" : "Immunization"
>      }]
>    }
>  }],
>  "specialty" : [{
>    "coding" : [{
>      "system" : "http://snomed.info/sct",
>      "code" : "408480009",
>      "display" : "Clinical immunology"
>    }]
>  }],
>  "name" : "Burgers UMC, South Wing - Immunizations",
>  "actor" : [{
>    "reference" : "Location/1",
>    "display" : "Burgers UMC, South Wing, second floor"
>  }],
>  "planningHorizon" : {
>    "start" : "2013-12-25T09:15:00Z",
>    "end" : "2013-12-25T09:30:00Z"
>  },
>  "comment" : "The slots attached to this schedule should be specialized to cover immunizations within the clinic"
>}
>```
></details>


&nbsp;

#### 4. [Slot](https://hl7.org/fhir/slot.html)
Reference resource type:
* `Schehdule`
><details>
><summary>Slot scheme</summary>
>A slot of time on a schedule that may be available for booking appointments.
>
><br>Slot resources are used to provide time-slots that can be booked using an appointment.
>They do not provide any information about appointments that are available, just the time, and optionally what the time can be used for.
>These are effectively spaces of free/busy time.
>Slots can also be marked as busy without having appointments associated.
>
>For more details for each data type of the schema, please see [here](https://hl7.org/fhir/slot.html).
>```json
>{
>  "resourceType" : "Slot",
>  // from Resource: id, meta, implicitRules, and language
>  // from DomainResource: text, contained, extension, and modifierExtension
>  "identifier" : [{ Identifier }], // External Ids for this item
>  "serviceCategory" : [{ CodeableConcept }], // A broad categorization of the service that is to be performed during this appointment
>  "serviceType" : [{ CodeableReference(HealthcareService) }], // The type of appointments that can be booked into this slot (ideally this would be an identifiable service - which is at a location, rather than the location itself). If provided then this overrides the value provided on the Schedule resource
>  "specialty" : [{ CodeableConcept }], // The specialty of a practitioner that would be required to perform the service requested in this appointment
>  "appointmentType" : [{ CodeableConcept }], // The style of appointment or patient that may be booked in the slot (not service type) icon
>  "schedule" : { Reference(Schedule) }, // R!  The schedule resource that this slot defines an interval of status information
>  "status" : "<code>", // R!  busy | free | busy-unavailable | busy-tentative | entered-in-error
>  "start" : "<instant>", // R!  Date/Time that the slot is to begin
>  "end" : "<instant>", // R!  Date/Time that the slot is to conclude
>  "overbooked" : <boolean>, // This slot has already been overbooked, appointments are unlikely to be accepted for this time
>  "comment" : "<string>" // Comments on the slot to describe any extended information. Such as custom constraints on the slot
>}
>```
>
>Real data example
>```json
>{
>  "resourceType" : "Slot",
>  "id" : "example",
>  "serviceCategory" : [{
>    "coding" : [{
>      "code" : "17",
>      "display" : "General Practice"
>    }]
>  }],
>  "serviceType" : [{
>    "concept" : {
>      "coding" : [{
>        "code" : "57",
>        "display" : "Immunization"
>      }]
>    }
>  }],
>  "specialty" : [{
>    "coding" : [{
>      "code" : "408480009",
>      "display" : "Clinical immunology"
>    }]
>  }],
>  "appointmentType" : [{
>    "coding" : [{
>      "system" : "http://terminology.hl7.org/CodeSystem/v2-0276",
>      "code" : "WALKIN",
>      "display" : "A previously unscheduled walk-in visit"
>    }]
>  }],
>  "schedule" : {
>    "reference" : "Schedule/example"
>  },
>  "status" : "free",
>  "start" : "2013-12-25T09:15:00Z",
>  "end" : "2013-12-25T09:30:00Z",
>  "comment" : "Assessments should be performed before requesting appointments in this slot."
>}
>```
></details>

&nbsp;

#### 5. [VerificationResult](https://hl7.org/fhir/verificationresult.html)
><details>
><summary>VerificationResult scheme</summary>
>Describes validation requirements, source(s), status and dates for one or more elements.  
>
><br>The VerificationResult can be used where content (such as found in a directory) is aggregated between systems, and the details and results of this verification process needs to be recorded, to determine the likely accuracy/confidence in the content.
>It does not represent the workflows or tasks related, but does cover the result of who did what when, why, and when it needs to be done again.
>
>There are often multiple instances of the VerificationResult over time that reference the same resource, even if the resource has not changed, as the content was verified as still current. Alternately the process may discover that the content was no longer valid (i.e. the practitioner was not able to be verified was still working at the location) and therefore the instance could be updated to not be active, or even removed from the directory.
>
>For more details for each data type of the schema, please see [here](https://hl7.org/fhir/verificationresult.html).
>```json
>{
>  "resourceType" : "VerificationResult",
>  // from Resource: id, meta, implicitRules, and language
>  // from DomainResource: text, contained, extension, and modifierExtension
>  "target" : [{ Reference(Any) }], // A resource that was validated
>  "targetLocation" : ["<string>"], // The fhirpath location(s) within the resource that was validated
>  "need" : { CodeableConcept }, // none | initial | periodic
>  "status" : "<code>", // R!  attested | validated | in-process | req-revalid | val-fail | reval-fail | entered-in-error
>  "statusDate" : "<dateTime>", // When the validation status was updated
>  "validationType" : { CodeableConcept }, // nothing | primary | multiple
>  "validationProcess" : [{ CodeableConcept }], // The primary process by which the target is validated (edit check; value set; primary source; multiple sources; standalone; in context)
>  "frequency" : { Timing }, // Frequency of revalidation
>  "lastPerformed" : "<dateTime>", // The date/time validation was last completed (including failed validations)
>  "nextScheduled" : "<date>", // The date when target is next validated, if appropriate
>  "failureAction" : { CodeableConcept }, // fatal | warn | rec-only | none
>  "primarySource" : [{ // Information about the primary source(s) involved in validation
>    "who" : { Reference(Organization|Practitioner|PractitionerRole) }, // Reference to the primary source
>    "type" : [{ CodeableConcept }], // Type of primary source (License Board; Primary Education; Continuing Education; Postal Service; Relationship owner; Registration Authority; legal source; issuing source; authoritative source)
>    "communicationMethod" : [{ CodeableConcept }], // Method for exchanging information with the primary source
>    "validationStatus" : { CodeableConcept }, // successful | failed | unknown
>    "validationDate" : "<dateTime>", // When the target was validated against the primary source
>    "canPushUpdates" : { CodeableConcept }, // yes | no | undetermined
>    "pushTypeAvailable" : [{ CodeableConcept }] // specific | any | source
>  }],
>  "attestation" : { // Information about the entity attesting to information
>    "who" : { Reference(Organization|Practitioner|PractitionerRole) }, // The individual or organization attesting to information
>    "onBehalfOf" : { Reference(Organization|Practitioner|PractitionerRole) }, // When the who is asserting on behalf of another (organization or individual)
>    "communicationMethod" : { CodeableConcept }, // The method by which attested information was submitted/retrieved
>    "date" : "<date>", // The date the information was attested to
>    "sourceIdentityCertificate" : "<string>", // A digital identity certificate associated with the attestation source
>    "proxyIdentityCertificate" : "<string>", // A digital identity certificate associated with the proxy entity submitting attested information on behalf of the attestation source
>    "proxySignature" : { Signature }, // Proxy signature (digital or image)
>    "sourceSignature" : { Signature } // Attester signature (digital or image)
>  },
>  "validator" : [{ // Information about the entity validating information
>    "organization" : { Reference(Organization) }, // R!  Reference to the organization validating information
>    "identityCertificate" : "<string>", // A digital identity certificate associated with the validator
>    "attestationSignature" : { Signature } // Validator signature (digital or image)
>  }]
>}
>```
>
>Real data example
>```json
>{
>  "resourceType" : "VerificationResult",
>  "id" : "example",
>  "status" : "attested"
>}
>```
></details>

&nbsp;

#### 6. [Transport](https://hl7.org/fhir/transport.html)
><details>
><summary>Transport scheme</summary>
>Record of transport of item.
>
>For more details for each data type of the schema, please see [here](https://hl7.org/fhir/transport.html).
>```json
>{
>  "resourceType" : "Transport",
>  // from Resource: id, meta, implicitRules, and language
>  // from DomainResource: text, contained, extension, and modifierExtension
>  "identifier" : [{ Identifier }], // External identifier
>  "instantiatesCanonical" : "<canonical(ActivityDefinition)>", // Formal definition of transport
>  "instantiatesUri" : "<uri>", // Formal definition of transport
>  "basedOn" : [{ Reference(Any) }], // Request fulfilled by this transport
>  "groupIdentifier" : { Identifier }, // Requisition or grouper id
>  "partOf" : [{ Reference(Transport) }], // Part of referenced event
>  "status" : "<code>", // in-progress | completed | abandoned | cancelled | planned | entered-in-error
>  "statusReason" : { CodeableConcept }, // Reason for current status
>  "intent" : "<code>", // R!  unknown | proposal | plan | order | original-order | reflex-order | filler-order | instance-order | option
>  "priority" : "<code>", // routine | urgent | asap | stat
>  "code" : { CodeableConcept }, // Transport Type
>  "description" : "<string>", // Human-readable explanation of transport
>  "focus" : { Reference(Any) }, // What transport is acting on
>  "for" : { Reference(Any) }, // Beneficiary of the Transport
>  "encounter" : { Reference(Encounter) }, // Healthcare event during which this transport originated
>  "completionTime" : "<dateTime>", // Completion time of the event (the occurrence)
>  "authoredOn" : "<dateTime>", // Transport Creation Date
>  "lastModified" : "<dateTime>", // Transport Last Modified Date
>  "requester" : { Reference(Device|Organization|Patient|Practitioner|
>   PractitionerRole|RelatedPerson) }, // Who is asking for transport to be done
>  "performerType" : [{ CodeableConcept }], // Requested performer
>  "owner" : { Reference(CareTeam|Device|HealthcareService|Organization|
>   Patient|Practitioner|PractitionerRole|RelatedPerson) }, // Responsible individual
>  "location" : { Reference(Location) }, // Where transport occurs
>  "insurance" : [{ Reference(ClaimResponse|Coverage) }], // Associated insurance coverage
>  "note" : [{ Annotation }], // Comments made about the transport
>  "relevantHistory" : [{ Reference(Provenance) }], // Key events in history of the Transport
>  "restriction" : { // Constraints on fulfillment transports
>    "repetitions" : "<positiveInt>", // How many times to repeat
>    "period" : { Period }, // When fulfillment sought
>    "recipient" : [{ Reference(Group|Organization|Patient|Practitioner|
>    PractitionerRole|RelatedPerson) }] // For whom is fulfillment sought?
>  },
>  "input" : [{ // Information used to perform transport
>    "type" : { CodeableConcept }, // R!  Label for the input
>    // value[x]: Content to use in performing the transport. One of these 54:
>    "valueBase64Binary" : "<base64Binary>"
>    "valueBoolean" : <boolean>,
>    "valueCanonical" : "<canonical>",
>    "valueCode" : "<code>",
>    "valueDate" : "<date>",
>    "valueDateTime" : "<dateTime>",
>    "valueDecimal" : <decimal>,
>    "valueId" : "<id>",
>    "valueInstant" : "<instant>",
>    "valueInteger" : <integer>,
>    "valueInteger64" : "<integer64>",
>    "valueMarkdown" : "<markdown>",
>    "valueOid" : "<oid>",
>    "valuePositiveInt" : "<positiveInt>",
>    "valueString" : "<string>",
>    "valueTime" : "<time>",
>    "valueUnsignedInt" : "<unsignedInt>",
>    "valueUri" : "<uri>",
>    "valueUrl" : "<url>",
>    "valueUuid" : "<uuid>",
>    "valueAddress" : { Address },
>    "valueAge" : { Age },
>    "valueAnnotation" : { Annotation },
>    "valueAttachment" : { Attachment },
>    "valueCodeableConcept" : { CodeableConcept },
>    "valueCodeableReference" : { CodeableReference },
>    "valueCoding" : { Coding },
>    "valueContactPoint" : { ContactPoint },
>    "valueCount" : { Count },
>    "valueDistance" : { Distance },
>    "valueDuration" : { Duration },
>    "valueHumanName" : { HumanName },
>    "valueIdentifier" : { Identifier },
>    "valueMoney" : { Money },
>    "valuePeriod" : { Period },
>    "valueQuantity" : { Quantity },
>    "valueRange" : { Range },
>    "valueRatio" : { Ratio },
>    "valueRatioRange" : { RatioRange },
>    "valueReference" : { Reference },
>    "valueSampledData" : { SampledData },
>    "valueSignature" : { Signature },
>    "valueTiming" : { Timing },
>    "valueContactDetail" : { ContactDetail },
>    "valueDataRequirement" : { DataRequirement },
>    "valueExpression" : { Expression },
>    "valueParameterDefinition" : { ParameterDefinition },
>    "valueRelatedArtifact" : { RelatedArtifact },
>    "valueTriggerDefinition" : { TriggerDefinition },
>    "valueUsageContext" : { UsageContext },
>    "valueAvailability" : { Availability },
>    "valueExtendedContactDetail" : { ExtendedContactDetail },
>    "valueDosage" : { Dosage },
>    "valueMeta" : { Meta },
>  }],
>  "output" : [{ // Information produced as part of transport
>    "type" : { CodeableConcept }, // R!  Label for output
>    // value[x]: Result of output. One of these 54:
>    "valueBase64Binary" : "<base64Binary>"
>    "valueBoolean" : <boolean>,
>    "valueCanonical" : "<canonical>",
>    "valueCode" : "<code>",
>    "valueDate" : "<date>",
>    "valueDateTime" : "<dateTime>",
>    "valueDecimal" : <decimal>,
>    "valueId" : "<id>",
>    "valueInstant" : "<instant>",
>    "valueInteger" : <integer>,
>    "valueInteger64" : "<integer64>",
>    "valueMarkdown" : "<markdown>",
>    "valueOid" : "<oid>",
>    "valuePositiveInt" : "<positiveInt>",
>    "valueString" : "<string>",
>    "valueTime" : "<time>",
>    "valueUnsignedInt" : "<unsignedInt>",
>    "valueUri" : "<uri>",
>    "valueUrl" : "<url>",
>    "valueUuid" : "<uuid>",
>    "valueAddress" : { Address },
>    "valueAge" : { Age },
>    "valueAnnotation" : { Annotation },
>    "valueAttachment" : { Attachment },
>    "valueCodeableConcept" : { CodeableConcept },
>    "valueCodeableReference" : { CodeableReference },
>    "valueCoding" : { Coding },
>    "valueContactPoint" : { ContactPoint },
>    "valueCount" : { Count },
>    "valueDistance" : { Distance },
>    "valueDuration" : { Duration },
>    "valueHumanName" : { HumanName },
>    "valueIdentifier" : { Identifier },
>    "valueMoney" : { Money },
>    "valuePeriod" : { Period },
>    "valueQuantity" : { Quantity },
>    "valueRange" : { Range },
>    "valueRatio" : { Ratio },
>    "valueRatioRange" : { RatioRange },
>    "valueReference" : { Reference },
>    "valueSampledData" : { SampledData },
>    "valueSignature" : { Signature },
>    "valueTiming" : { Timing },
>    "valueContactDetail" : { ContactDetail },
>    "valueDataRequirement" : { DataRequirement },
>    "valueExpression" : { Expression },
>    "valueParameterDefinition" : { ParameterDefinition },
>    "valueRelatedArtifact" : { RelatedArtifact },
>    "valueTriggerDefinition" : { TriggerDefinition },
>    "valueUsageContext" : { UsageContext },
>    "valueAvailability" : { Availability },
>    "valueExtendedContactDetail" : { ExtendedContactDetail },
>    "valueDosage" : { Dosage },
>    "valueMeta" : { Meta },
>  }],
>  "requestedLocation" : { Reference(Location) }, // R!  The desired location
>  "currentLocation" : { Reference(Location) }, // R!  The entity current location
>  "reason" : { CodeableReference(Any) }, // Why transport is needed
>  "history" : { Reference(Transport) } // Parent (or preceding) transport
>}
>```
>
>Real data example
>```json
>{
>  "resourceType" : "Transport",
>  "id" : "simpledelivery",
>  "identifier" : [{
>    "value" : "Transport1234"
>  }],
>  "basedOn" : [{
>    "reference" : "SupplyRequest/simpleorder"
>  }],
>  "partOf" : [{
>    "display" : "Central Supply Restock"
>  }],
>  "status" : "completed",
>  "intent" : "order",
>  "requestedLocation" : {
>    "reference" : "Transport/location-hospitalLab",
>    "display" : "Requested location for item at City Hospital Lab"
>  },
>  "currentLocation" : {
>    "reference" : "Transport/location-labA",
>    "display" : "Current location for item at Lab A"
>  }
>}
>```
></details>

&nbsp;

#### 7. [Task](https://hl7.org/fhir/task.html)
><details>
><summary>Task scheme</summary>
>A task to be performed.
>
><br>A task resource describes an activity that can be performed and tracks the state of completion of that activity. It is a representation that an activity should be or has been initiated, and eventually, represents the successful or unsuccessful completion of that activity.
>
>Note that there are a variety of processes associated with making and processing orders. Some orders may be handled immediately by automated systems but most require real-world actions by one or more humans. Some orders can only be processed when other real-world actions happen, such as a patient presenting themselves so that the action to be performed can actually be performed. Often these real-world dependencies are only implicit in the order details.
>
>For more details for each data type of the schema, please see [here](https://hl7.org/fhir/task.html).
>```json
>{
>  "resourceType" : "Task",
>  // from Resource: id, meta, implicitRules, and language
>  // from DomainResource: text, contained, extension, and modifierExtension
>  "identifier" : [{ Identifier }], // Task Instance Identifier
>  "instantiatesCanonical" : "<canonical(ActivityDefinition)>", // Formal definition of task
>  "instantiatesUri" : "<uri>", // Formal definition of task
>  "basedOn" : [{ Reference(Any) }], // Request fulfilled by this task
>  "groupIdentifier" : { Identifier }, // Requisition or grouper id
>  "partOf" : [{ Reference(Task) }], // Composite task
>  "status" : "<code>", // R!  draft | requested | received | accepted | +
>  "statusReason" : { CodeableReference }, // Reason for current status
>  "businessStatus" : { CodeableConcept }, // E.g. "Specimen collected", "IV prepped"
>  "intent" : "<code>", // R!  unknown | proposal | plan | order | original-order | reflex-order | filler-order | instance-order | option
>  "priority" : "<code>", // routine | urgent | asap | stat
>  "doNotPerform" : <boolean>, // True if Task is prohibiting action
>  "code" : { CodeableConcept }, // I Task Type
>  "description" : "<string>", // Human-readable explanation of task
>  "focus" : { Reference(Any) }, // I What task is acting on
>  "for" : { Reference(Any) }, // Beneficiary of the Task
>  "encounter" : { Reference(Encounter) }, // Healthcare event during which this task originated
>  "requestedPeriod" : { Period }, // When the task should be performed
>  "executionPeriod" : { Period }, // Start and end time of execution
>  "authoredOn" : "<dateTime>", // I Task Creation Date
>  "lastModified" : "<dateTime>", // I Task Last Modified Date
>  "requester" : { Reference(Device|Organization|Patient|Practitioner|
>   PractitionerRole|RelatedPerson) }, // Who is asking for task to be done
>  "requestedPerformer" : [{ CodeableReference(CareTeam|Device|
>   HealthcareService|Organization|Patient|Practitioner|PractitionerRole|
>   RelatedPerson) }], // Who should perform Task
>  "owner" : { Reference(CareTeam|Organization|Patient|Practitioner|
>   PractitionerRole|RelatedPerson) }, // Responsible individual
>  "performer" : [{ // Who or what performed the task
>    "function" : { CodeableConcept }, // Type of performance
>    "actor" : { Reference(CareTeam|Organization|Patient|Practitioner|
>    PractitionerRole|RelatedPerson) } // R!  Who performed the task
>  }],
>  "location" : { Reference(Location) }, // Where task occurs
>  "reason" : [{ CodeableReference }], // Why task is needed
>  "insurance" : [{ Reference(ClaimResponse|Coverage) }], // Associated insurance coverage
>  "note" : [{ Annotation }], // Comments made about the task
>  "relevantHistory" : [{ Reference(Provenance) }], // Key events in history of the Task
>  "restriction" : { // I Constraints on fulfillment tasks
>    "repetitions" : "<positiveInt>", // How many times to repeat
>    "period" : { Period }, // When fulfillment is sought
>    "recipient" : [{ Reference(Group|Organization|Patient|Practitioner|
>    PractitionerRole|RelatedPerson) }] // For whom is fulfillment sought?
>  },
>  "input" : [{ // Information used to perform task
>    "type" : { CodeableConcept }, // R!  Label for the input
>    // value[x]: Content to use in performing the task. One of these 54:
>    "valueBase64Binary" : "<base64Binary>"
>    "valueBoolean" : <boolean>,
>    "valueCanonical" : "<canonical>",
>    "valueCode" : "<code>",
>    "valueDate" : "<date>",
>    "valueDateTime" : "<dateTime>",
>    "valueDecimal" : <decimal>,
>    "valueId" : "<id>",
>    "valueInstant" : "<instant>",
>    "valueInteger" : <integer>,
>    "valueInteger64" : "<integer64>",
>    "valueMarkdown" : "<markdown>",
>    "valueOid" : "<oid>",
>    "valuePositiveInt" : "<positiveInt>",
>    "valueString" : "<string>",
>    "valueTime" : "<time>",
>    "valueUnsignedInt" : "<unsignedInt>",
>    "valueUri" : "<uri>",
>    "valueUrl" : "<url>",
>    "valueUuid" : "<uuid>",
>    "valueAddress" : { Address },
>    "valueAge" : { Age },
>    "valueAnnotation" : { Annotation },
>    "valueAttachment" : { Attachment },
>    "valueCodeableConcept" : { CodeableConcept },
>    "valueCodeableReference" : { CodeableReference },
>    "valueCoding" : { Coding },
>    "valueContactPoint" : { ContactPoint },
>    "valueCount" : { Count },
>    "valueDistance" : { Distance },
>    "valueDuration" : { Duration },
>    "valueHumanName" : { HumanName },
>    "valueIdentifier" : { Identifier },
>    "valueMoney" : { Money },
>    "valuePeriod" : { Period },
>    "valueQuantity" : { Quantity },
>    "valueRange" : { Range },
>    "valueRatio" : { Ratio },
>    "valueRatioRange" : { RatioRange },
>    "valueReference" : { Reference },
>    "valueSampledData" : { SampledData },
>    "valueSignature" : { Signature },
>    "valueTiming" : { Timing },
>    "valueContactDetail" : { ContactDetail },
>    "valueDataRequirement" : { DataRequirement },
>    "valueExpression" : { Expression },
>    "valueParameterDefinition" : { ParameterDefinition },
>    "valueRelatedArtifact" : { RelatedArtifact },
>    "valueTriggerDefinition" : { TriggerDefinition },
>    "valueUsageContext" : { UsageContext },
>    "valueAvailability" : { Availability },
>    "valueExtendedContactDetail" : { ExtendedContactDetail },
>    "valueDosage" : { Dosage },
>    "valueMeta" : { Meta },
>  }],
>  "output" : [{ // Information produced as part of task
>    "type" : { CodeableConcept }, // R!  Label for output
>    // value[x]: Result of output. One of these 54:
>    "valueBase64Binary" : "<base64Binary>"
>    "valueBoolean" : <boolean>,
>    "valueCanonical" : "<canonical>",
>    "valueCode" : "<code>",
>    "valueDate" : "<date>",
>    "valueDateTime" : "<dateTime>",
>    "valueDecimal" : <decimal>,
>    "valueId" : "<id>",
>    "valueInstant" : "<instant>",
>    "valueInteger" : <integer>,
>    "valueInteger64" : "<integer64>",
>    "valueMarkdown" : "<markdown>",
>    "valueOid" : "<oid>",
>    "valuePositiveInt" : "<positiveInt>",
>    "valueString" : "<string>",
>    "valueTime" : "<time>",
>    "valueUnsignedInt" : "<unsignedInt>",
>    "valueUri" : "<uri>",
>    "valueUrl" : "<url>",
>    "valueUuid" : "<uuid>",
>    "valueAddress" : { Address },
>    "valueAge" : { Age },
>    "valueAnnotation" : { Annotation },
>    "valueAttachment" : { Attachment },
>    "valueCodeableConcept" : { CodeableConcept },
>    "valueCodeableReference" : { CodeableReference },
>    "valueCoding" : { Coding },
>    "valueContactPoint" : { ContactPoint },
>    "valueCount" : { Count },
>    "valueDistance" : { Distance },
>    "valueDuration" : { Duration },
>    "valueHumanName" : { HumanName },
>    "valueIdentifier" : { Identifier },
>    "valueMoney" : { Money },
>    "valuePeriod" : { Period },
>    "valueQuantity" : { Quantity },
>    "valueRange" : { Range },
>    "valueRatio" : { Ratio },
>    "valueRatioRange" : { RatioRange },
>    "valueReference" : { Reference },
>    "valueSampledData" : { SampledData },
>    "valueSignature" : { Signature },
>    "valueTiming" : { Timing },
>    "valueContactDetail" : { ContactDetail },
>    "valueDataRequirement" : { DataRequirement },
>    "valueExpression" : { Expression },
>    "valueParameterDefinition" : { ParameterDefinition },
>    "valueRelatedArtifact" : { RelatedArtifact },
>    "valueTriggerDefinition" : { TriggerDefinition },
>    "valueUsageContext" : { UsageContext },
>    "valueAvailability" : { Availability },
>    "valueExtendedContactDetail" : { ExtendedContactDetail },
>    "valueDosage" : { Dosage },
>    "valueMeta" : { Meta },
>  }]
>}
>```
>
>Real data example
>```json
>{
>  "resourceType" : "Task",
>  "id" : "example1",
>  "contained" : [{
>    "resourceType" : "Provenance",
>    "id" : "signature",
>    "target" : [{
>      "reference" : "ServiceRequest/physiotherapy/_history/1"
>    }],
>    "recorded" : "2016-10-31T08:25:05+10:00",
>    "agent" : [{
>      "role" : [{
>        "coding" : [{
>          "system" : "http://terminology.hl7.org/CodeSystem/v3-ParticipationType",
>          "code" : "AUT"
>        }]
>      }],
>      "who" : {
>        "reference" : "Practitioner/f202",
>        "display" : "Luigi Maas"
>      }
>    }],
>    "signature" : [{
>      "type" : [{
>        "system" : "urn:iso-astm:E1762-95:2013",
>        "code" : "1.2.840.10065.1.12.1.1",
>        "display" : "Author's Signature"
>      }],
>      "when" : "2016-10-31T08:25:05+10:00",
>      "who" : {
>        "reference" : "Practitioner/example",
>        "display" : "Dr Adam Careful"
>      },
>      "targetFormat" : "application/fhir+xml",
>      "sigFormat" : "application/signature+xml",
>      "data" : "dGhpcyBibG9iIGlzIHNuaXBwZWQ="
>    }]
>  }],
>  "identifier" : [{
>    "use" : "official",
>    "system" : "http:/goodhealth.org/identifiers",
>    "value" : "20170201-001"
>  }],
>  "basedOn" : [{
>    "display" : "General Wellness Careplan"
>  }],
>  "groupIdentifier" : {
>    "use" : "official",
>    "system" : "http:/goodhealth.org/accession/identifiers",
>    "value" : "G20170201-001"
>  },
>  "status" : "in-progress",
>  "businessStatus" : {
>    "text" : "waiting for specimen"
>  },
>  "intent" : "order",
>  "priority" : "routine",
>  "code" : {
>    "coding" : [{
>      "system" : "http://hl7.org/fhir/CodeSystem/task-code",
>      "code" : "fulfill"
>    }],
>    "text" : "Lipid Panel"
>  },
>  "description" : "Create order for getting specimen, Set up inhouse testing,  generate order for any sendouts and submit with specimen",
>  "focus" : {
>    "reference" : "ServiceRequest/lipid",
>    "display" : "Lipid Panel Request"
>  },
>  "for" : {
>    "reference" : "Patient/example",
>    "display" : "Peter James Chalmers"
>  },
>  "encounter" : {
>    "reference" : "Encounter/example",
>    "display" : "Example In-Patient Encounter"
>  },
>  "executionPeriod" : {
>    "start" : "2016-10-31T08:25:05+10:00"
>  },
>  "authoredOn" : "2016-10-31T08:25:05+10:00",
>  "lastModified" : "2016-10-31T09:45:05+10:00",
>  "requester" : {
>    "reference" : "Practitioner/example",
>    "display" : "Dr Adam Careful"
>  },
>  "requestedPerformer" : [{
>    "concept" : {
>      "coding" : [{
>        "system" : "http://snomed.info/sct",
>        "code" : "18850004",
>        "display" : "Laboratory hematologist"
>      }],
>      "text" : "Performer"
>    }
>  }],
>  "owner" : {
>    "reference" : "Organization/1832473e-2fe0-452d-abe9-3cdb9879522f",
>    "display" : "Clinical Laboratory @ Acme Hospital"
>  },
>  "reason" : [{
>    "concept" : {
>      "text" : "The Task.reason should only be included if there is no Task.focus or if it differs from the reason indicated on the focus"
>    }
>  }],
>  "note" : [{
>    "text" : "This is an example to demonstrate using task for actioning a servicerequest and to illustrate how to populate many of the task elements - this is the parent task that will be broken into subtask to grab the specimen and a sendout lab test"
>  }],
>  "relevantHistory" : [{
>    "reference" : "#signature",
>    "display" : "Author's Signature"
>  }],
>  "restriction" : {
>    "repetitions" : 1,
>    "period" : {
>      "end" : "2016-11-02T09:45:05+10:00"
>    }
>  }
>}
>```
></details>


&nbsp;

&nbsp;




### Others
#### 1. [Patient](https://hl7.org/fhir/patient.html)
><details>
><summary>Patient scheme</summary>
>Demographics and other administrative information about an individual or animal receiving care or other health-related services.
>
>For more details for each data type of the schema, please see [here](https://hl7.org/fhir/patient.html).
>```json
>{
>  "resourceType" : "Patient",
>  // from Resource: id, meta, implicitRules, and language
>  // from DomainResource: text, contained, extension, and modifierExtension
>  "identifier" : [{ Identifier }], // An identifier for this patient
>  "active" : <boolean>, // Whether this patient's record is in active use
>  "name" : [{ HumanName }], // A name associated with the patient
>  "telecom" : [{ ContactPoint }], // A contact detail for the individual
>  "gender" : "<code>", // male | female | other | unknown
>  "birthDate" : "<date>", // The date of birth for the individual
>  // deceased[x]: Indicates if the individual is deceased or not. One of these 2:
>  "deceasedBoolean" : <boolean>,
>  "deceasedDateTime" : "<dateTime>",
>  "address" : [{ Address }], // An address for the individual
>  "maritalStatus" : { CodeableConcept }, // Marital (civil) status of a patient
>  // multipleBirth[x]: Whether patient is part of a multiple birth. One of these 2:
>  "multipleBirthBoolean" : <boolean>,
>  "multipleBirthInteger" : <integer>,
>  "photo" : [{ Attachment }], // Image of the patient
>  "contact" : [{ // A contact party (e.g. guardian, partner, friend) for the patient
>    "relationship" : [{ CodeableConcept }], // The kind of relationship
>    "name" : { HumanName }, // I A name associated with the contact person
>    "telecom" : [{ ContactPoint }], // I A contact detail for the person
>    "address" : { Address }, // I Address for the contact person
>    "gender" : "<code>", // male | female | other | unknown
>    "organization" : { Reference(Organization) }, // I Organization that is associated with the contact
>    "period" : { Period } // The period during which this contact person or organization is valid to be contacted relating to this patient
>  }],
>  "communication" : [{ // A language which may be used to communicate with the patient about his or her health
>    "language" : { CodeableConcept }, // R!  The language which can be used to communicate with the patient about his or her health
>    "preferred" : <boolean> // Language preference indicator
>  }],
>  "generalPractitioner" : [{ Reference(Organization|Practitioner|
>   PractitionerRole) }], // Patient's nominated primary care provider
>  "managingOrganization" : { Reference(Organization) }, // Organization that is the custodian of the patient record
>  "link" : [{ // Link to a Patient or RelatedPerson resource that concerns the same actual individual
>    "other" : { Reference(Patient|RelatedPerson) }, // R!  The other patient or related person resource that the link refers to
>    "type" : "<code>" // R!  replaced-by | replaces | refer | seealso
>  }]
>}
>```
>
>Real data example
>```json
>{
>  "resourceType" : "Patient",
>  "id" : "example",
>  "identifier" : [{
>    "use" : "usual",
>    "type" : {
>      "coding" : [{
>        "system" : "http://terminology.hl7.org/CodeSystem/v2-0203",
>        "code" : "MR"
>      }]
>    },
>    "system" : "urn:oid:1.2.36.146.595.217.0.1",
>    "value" : "12345",
>    "period" : {
>      "start" : "2001-05-06"
>    },
>    "assigner" : {
>      "display" : "Acme Healthcare"
>    }
>  }],
>  "active" : true,
>  "name" : [{
>    "use" : "official",
>    "family" : "Chalmers",
>    "given" : ["Peter",
>    "James"]
>  },
>  {
>    "use" : "usual",
>    "given" : ["Jim"]
>  },
>  {
>    "use" : "maiden",
>    "family" : "Windsor",
>    "given" : ["Peter",
>    "James"],
>    "period" : {
>      "end" : "2002"
>    }
>  }],
>  "telecom" : [{
>    "use" : "home"
>  },
>  {
>    "system" : "phone",
>    "value" : "(03) 5555 6473",
>    "use" : "work",
>    "rank" : 1
>  },
>  {
>    "system" : "phone",
>    "value" : "(03) 3410 5613",
>    "use" : "mobile",
>    "rank" : 2
>  },
>  {
>    "system" : "phone",
>    "value" : "(03) 5555 8834",
>    "use" : "old",
>    "period" : {
>      "end" : "2014"
>    }
>  }],
>  "gender" : "male",
>  "birthDate" : "1974-12-25",
>  "_birthDate" : {
>    "extension" : [{
>      "url" : "http://hl7.org/fhir/StructureDefinition/patient-birthTime",
>      "valueDateTime" : "1974-12-25T14:35:45-05:00"
>    }]
>  },
>  "deceasedBoolean" : false,
>  "address" : [{
>    "use" : "home",
>    "type" : "both",
>    "text" : "534 Erewhon St PeasantVille, Rainbow, Vic  3999",
>    "line" : ["534 Erewhon St"],
>    "city" : "PleasantVille",
>    "district" : "Rainbow",
>    "state" : "Vic",
>    "postalCode" : "3999",
>    "period" : {
>      "start" : "1974-12-25"
>    }
>  }],
>  "contact" : [{
>    "relationship" : [{
>      "coding" : [{
>        "system" : "http://terminology.hl7.org/CodeSystem/v2-0131",
>        "code" : "N"
>      }]
>    }],
>    "name" : {
>      "family" : "du Marché",
>      "_family" : {
>        "extension" : [{
>          "url" : "http://hl7.org/fhir/StructureDefinition/humanname-own-prefix",
>          "valueString" : "VV"
>        }]
>      },
>      "given" : ["Bénédicte"]
>    },
>    "telecom" : [{
>      "system" : "phone",
>      "value" : "+33 (237) 998327"
>    }],
>    "address" : {
>      "use" : "home",
>      "type" : "both",
>      "line" : ["534 Erewhon St"],
>      "city" : "PleasantVille",
>      "district" : "Rainbow",
>      "state" : "Vic",
>      "postalCode" : "3999",
>      "period" : {
>        "start" : "1974-12-25"
>      }
>    },
>    "gender" : "female",
>    "period" : {
>      "start" : "2012"
>    }
>  }],
>  "managingOrganization" : {
>    "reference" : "Organization/1"
>  }
>}
>```
></details>

&nbsp;

#### 2. [Practitioner](https://hl7.org/fhir/practitioner.html)
><details>
><summary>Practitioner scheme</summary>
>A person who is directly or indirectly involved in the provisioning of healthcare or related services.
>
><br>Practitioner covers all individuals who are engaged in the healthcare process and healthcare-related services as part of their formal responsibilities and this Resource is used for attribution of activities and responsibilities to these individuals.
>
>For more details for each data type of the schema, please see [here](https://hl7.org/fhir/practitioner.html).
>```json
>{
>  "resourceType" : "Practitioner",
>  // from Resource: id, meta, implicitRules, and language
>  // from DomainResource: text, contained, extension, and modifierExtension
>  "identifier" : [{ Identifier }], // An identifier for the person as this agent
>  "active" : <boolean>, // Whether this practitioner's record is in active use
>  "name" : [{ HumanName }], // The name(s) associated with the practitioner
>  "telecom" : [{ ContactPoint }], // A contact detail for the practitioner (that apply to all roles)
>  "gender" : "<code>", // male | female | other | unknown
>  "birthDate" : "<date>", // The date  on which the practitioner was born
>  // deceased[x]: Indicates if the practitioner is deceased or not. One of these 2:
>  "deceasedBoolean" : <boolean>,
>  "deceasedDateTime" : "<dateTime>",
>  "address" : [{ Address }], // Address(es) of the practitioner that are not role specific (typically home address)
>  "photo" : [{ Attachment }], // Image of the person
>  "qualification" : [{ // Qualifications, certifications, accreditations, licenses, training, etc. pertaining to the provision of care
>    "identifier" : [{ Identifier }], // An identifier for this qualification for the practitioner
>    "code" : { CodeableConcept }, // R!  Coded representation of the qualification icon
>    "period" : { Period }, // Period during which the qualification is valid
>    "issuer" : { Reference(Organization) } // Organization that regulates and issues the qualification
>  }],
>  "communication" : [{ // A language which may be used to communicate with the practitioner
>    "language" : { CodeableConcept }, // R!  The language code used to communicate with the practitioner
>    "preferred" : <boolean> // Language preference indicator
>  }]
>}
>```
>
>Real data example
>```json
>{
>  "resourceType" : "Practitioner",
>  "id" : "example",
>  "text" : {
>    "status" : "generated",
>    "div" : "<div xmlns=\"http://www.w3.org/1999/xhtml\">\n      <p>Dr Adam Careful is a Referring Practitioner for Acme Hospital from 1-Jan 2012 to 31-Mar\n        2012</p>\n    </div>"
>  },
>  "identifier" : [{
>    "system" : "http://www.acme.org/practitioners",
>    "value" : "23"
>  }],
>  "active" : true,
>  "name" : [{
>    "family" : "Careful",
>    "given" : ["Adam"],
>    "prefix" : ["Dr"]
>  }],
>  "address" : [{
>    "use" : "home",
>    "line" : ["534 Erewhon St"],
>    "city" : "PleasantVille",
>    "state" : "Vic",
>    "postalCode" : "3999"
>  }],
>  "qualification" : [{
>    "identifier" : [{
>      "system" : "http://example.org/UniversityIdentifier",
>      "value" : "12345"
>    }],
>    "code" : {
>      "coding" : [{
>        "system" : "http://terminology.hl7.org/CodeSystem/v2-0360/2.7",
>        "code" : "BS",
>        "display" : "Bachelor of Science"
>      }],
>      "text" : "Bachelor of Science"
>    },
>    "period" : {
>      "start" : "1995"
>    },
>    "issuer" : {
>      "display" : "Example University"
>    }
>  }]
>}
>```
></details>

&nbsp;

#### 3. [PractitionerRole](https://hl7.org/fhir/practitionerrole.html)
><details>
><summary>PractitionerRole scheme</summary>
>A specific set of Roles/Locations/specialties/services that a practitioner may perform at an organization for a period of time.
>
><br>The PractitionerRole describes the types of services that practitioners provide for an organization at specific location(s).
>
>For more details for each data type of the schema, please see [here](https://hl7.org/fhir/practitionerrole.html).
>```json
>{
>  "resourceType" : "PractitionerRole",
>  // from Resource: id, meta, implicitRules, and language
>  // from DomainResource: text, contained, extension, and modifierExtension
>  "identifier" : [{ Identifier }], // Identifiers for a role/location
>  "active" : <boolean>, // Whether this practitioner role record is in active use
>  "period" : { Period }, // The period during which the practitioner is authorized to perform in these role(s)
>  "practitioner" : { Reference(Practitioner) }, // Practitioner that provides services for the organization
>  "organization" : { Reference(Organization) }, // Organization where the roles are available
>  "code" : [{ CodeableConcept }], // Roles which this practitioner may perform
>  "specialty" : [{ CodeableConcept }], // Specific specialty of the practitioner
>  "location" : [{ Reference(Location) }], // Location(s) where the practitioner provides care
>  "healthcareService" : [{ Reference(HealthcareService) }], // Healthcare services provided for this role's Organization/Location(s)
>  "contact" : [{ ExtendedContactDetail }], // Official contact details relating to this PractitionerRole
>  "characteristic" : [{ CodeableConcept }], // Collection of characteristics (attributes)
>  "communication" : [{ CodeableConcept }], // A language the practitioner (in this role) can use in patient communication
>  "availability" : [{ Availability }], // Times the Practitioner is available at this location and/or healthcare service (including exceptions)
>  "endpoint" : [{ Reference(Endpoint) }] // Endpoints for interacting with the practitioner in this role
>}
>```
>
>Real data example
>```json
>{
>  "resourceType" : "PractitionerRole",
>  "id" : "example",
>  "text" : {
>    "status" : "generated",
>    "div" : "<div xmlns=\"http://www.w3.org/1999/xhtml\">\n\t\t\t<p>\n\t\t\t\tDr Adam Careful is a Referring Practitioner for Acme Hospital from 1-Jan 2012 to 31-Mar\n\t\t\t\t2012\n\t\t\t</p>\n\t\t</div>"
>  },
>  "identifier" : [{
>    "system" : "http://www.acme.org/practitioners",
>    "value" : "23"
>  }],
>  "active" : true,
>  "period" : {
>    "start" : "2012-01-01",
>    "end" : "2012-03-31"
>  },
>  "practitioner" : {
>    "reference" : "Practitioner/example",
>    "display" : "Dr Adam Careful"
>  },
>  "organization" : {
>    "reference" : "Organization/f001"
>  },
>  "code" : [{
>    "coding" : [{
>      "system" : "http://terminology.hl7.org/CodeSystem/v2-0286",
>      "code" : "RP"
>    }]
>  }],
>  "specialty" : [{
>    "coding" : [{
>      "system" : "http://snomed.info/sct",
>      "code" : "408443003",
>      "display" : "General medical practice"
>    }]
>  }],
>  "location" : [{
>    "reference" : "Location/1",
>    "display" : "South Wing, second floor"
>  }],
>  "healthcareService" : [{
>    "reference" : "HealthcareService/example"
>  }],
>  "contact" : [{
>    "telecom" : [{
>      "system" : "phone",
>      "value" : "(03) 5555 6473",
>      "use" : "work"
>    },
>    {
>      "system" : "email",
>      "value" : "adam.southern@example.org",
>      "use" : "work"
>    }]
>  }],
>  "characteristic" : [{
>    "coding" : [{
>      "system" : "http://hl7.org/fhir/service-mode",
>      "code" : "in-person",
>      "display" : "In Person"
>    },
>    {
>      "system" : "http://hl7.org/fhir/service-mode",
>      "code" : "videoconference",
>      "display" : "Video Conference"
>    }]
>  }],
>  "communication" : [{
>    "coding" : [{
>      "system" : "urn:ietf:bcp:47",
>      "code" : "en"
>    }]
>  }],
>  "availability" : [{
>    "availableTime" : [{
>      "daysOfWeek" : ["mon",
>      "tue",
>      "wed"],
>      "availableStartTime" : "09:00:00",
>      "availableEndTime" : "16:30:00"
>    },
>    {
>      "daysOfWeek" : ["thu",
>      "fri"],
>      "availableStartTime" : "09:00:00",
>      "availableEndTime" : "12:00:00"
>    }],
>    "notAvailableTime" : [{
>      "description" : "Adam will be on extended leave during May 2017",
>      "during" : {
>        "start" : "2017-05-01",
>        "end" : "2017-05-20"
>      }
>    },
>    {
>      "description" : "Adam is generally unavailable on public holidays and during the Christmas/New Year break"
>    }]
>  }],
>  "endpoint" : [{
>    "reference" : "Endpoint/example"
>  }]
>}
>```
></details>

&nbsp;


#### 4. [Location](https://hl7.org/fhir/location.html)
><details>
><summary>Location scheme</summary>
>Details and position information for a place where services are provided and resources and participants may be stored, found, contained, or accommodated.
>
><br>A Location includes both incidental locations (a place which is used for healthcare without prior designation or authorization) and dedicated, formally appointed locations.
>Locations may be private, public, mobile or fixed and scale from small freezers to full hospital buildings or parking garages.
>
>For more details for each data type of the schema, please see [here](https://hl7.org/fhir/location.html).
>```json
>{
>  "resourceType" : "Location",
>  // from Resource: id, meta, implicitRules, and language
>  // from DomainResource: text, contained, extension, and modifierExtension
>  "identifier" : [{ Identifier }], // Unique code or number identifying the location to its users
>  "status" : "<code>", // active | suspended | inactive
>  "operationalStatus" : { Coding }, // The operational status of the location (typically only for a bed/room) icon
>  "name" : "<string>", // Name of the location as used by humans
>  "alias" : ["<string>"], // A list of alternate names that the location is known as, or was known as, in the past
>  "description" : "<markdown>", // Additional details about the location that could be displayed as further information to identify the location beyond its name
>  "mode" : "<code>", // instance | kind
>  "type" : [{ CodeableConcept }], // Type of function performed icon
>  "contact" : [{ ExtendedContactDetail }], // Official contact details for the location
>  "address" : { Address }, // Physical location
>  "form" : { CodeableConcept }, // Physical form of the location
>  "position" : { // The absolute geographic location
>    "longitude" : <decimal>, // R!  Longitude with WGS84 datum
>    "latitude" : <decimal>, // R!  Latitude with WGS84 datum
>    "altitude" : <decimal> // Altitude with WGS84 datum
>  },
>  "managingOrganization" : { Reference(Organization) }, // Organization responsible for provisioning and upkeep
>  "partOf" : { Reference(Location) }, // Another Location this one is physically a part of
>  "characteristic" : [{ CodeableConcept }], // Collection of characteristics (attributes)
>  "hoursOfOperation" : [{ Availability }], // What days/times during a week is this location usually open (including exceptions)
>  "virtualService" : [{ VirtualServiceDetail }], // Connection details of a virtual service (e.g. conference call)
>  "endpoint" : [{ Reference(Endpoint) }] // Technical endpoints providing access to services operated for the location
>}
>```
>
>Real data example
>```json
>{
>  "resourceType" : "Location",
>  "id" : "1",
>  "identifier" : [{
>    "value" : "B1-S.F2"
>  }],
>  "status" : "active",
>  "name" : "South Wing, second floor",
>  "alias" : ["BU MC, SW, F2",
>  "Burgers University Medical Center, South Wing, second floor"],
>  "description" : "Second floor of the Old South Wing, formerly in use by Psychiatry",
>  "mode" : "instance",
>  "contact" : [{
>    "telecom" : [{
>      "system" : "phone",
>      "value" : "2328",
>      "use" : "work"
>    },
>    {
>      "system" : "fax",
>      "value" : "2329",
>      "use" : "work"
>    },
>    {
>      "system" : "email",
>      "value" : "second wing admissions"
>    }]
>  },
>  {
>    "telecom" : [{
>      "system" : "url",
>      "value" : "http://sampleorg.com/southwing",
>      "use" : "work"
>    }]
>  }],
>  "address" : {
>    "use" : "work",
>    "line" : ["Galapagosweg 91, Building A"],
>    "city" : "Den Burg",
>    "postalCode" : "9105 PZ",
>    "country" : "NLD"
>  },
>  "form" : {
>    "coding" : [{
>      "system" : "http://terminology.hl7.org/CodeSystem/location-physical-type",
>      "code" : "wi",
>      "display" : "Wing"
>    }]
>  },
>  "position" : {
>    "longitude" : -83.6945691,
>    "latitude" : 42.25475478,
>    "altitude" : 0
>  },
>  "managingOrganization" : {
>    "reference" : "Organization/f001"
>  },
>  "characteristic" : [{
>    "coding" : [{
>      "system" : "http://hl7.org/fhir/location-characteristic",
>      "code" : "wheelchair",
>      "display" : "Wheelchair accessible"
>    }]
>  }],
>  "endpoint" : [{
>    "reference" : "Endpoint/example"
>  }]
>}
>```
></details>

&nbsp;



#### 5. [ActivityDefinition](https://hl7.org/fhir/activitydefinition.html)
><details>
><summary>ActivityDefinition scheme</summary>
>This resource allows for the definition of some activity to be performed, independent of a particular patient, practitioner, or other performance context.
>
><br>An ActivityDefinition is a shareable, consumable description of some activity to be performed. It may be used to specify actions to be taken as part of a workflow, order set, or protocol, or it may be used independently as part of a catalog of activities such as orderables.
>
>For more details for each data type of the schema, please see [here](https://hl7.org/fhir/activitydefinition.html).
>```json
>{
>  "resourceType" : "ActivityDefinition",
>  // from Resource: id, meta, implicitRules, and language
>  // from DomainResource: text, contained, extension, and modifierExtension
>  "url" : "<uri>", // Canonical identifier for this activity definition, represented as a URI (globally unique)
>  "identifier" : [{ Identifier }], // Additional identifier for the activity definition
>  "version" : "<string>", // Business version of the activity definition
>  // versionAlgorithm[x]: How to compare versions. One of these 2:
>  "versionAlgorithmString" : "<string>",
>  "versionAlgorithmCoding" : { Coding },
>  "name" : "<string>", // I Name for this activity definition (computer friendly)
>  "title" : "<string>", // Name for this activity definition (human friendly)
>  "subtitle" : "<string>", // Subordinate title of the activity definition
>  "status" : "<code>", // R!  draft | active | retired | unknown
>  "experimental" : <boolean>, // For testing purposes, not real usage
>  // subject[x]: Type of individual the activity definition is intended for. One of these 3:
>  "subjectCodeableConcept" : { CodeableConcept },
>  "subjectReference" : { Reference(AdministrableProductDefinition|Group|
>   ManufacturedItemDefinition|MedicinalProductDefinition|
>   PackagedProductDefinition|SubstanceDefinition) },
>  "subjectCanonical" : "<canonical(EvidenceVariable)>",
>  "date" : "<dateTime>", // Date last changed
>  "publisher" : "<string>", // Name of the publisher/steward (organization or individual)
>  "contact" : [{ ContactDetail }], // Contact details for the publisher
>  "description" : "<markdown>", // Natural language description of the activity definition
>  "useContext" : [{ UsageContext }], // The context that the content is intended to support
>  "jurisdiction" : [{ CodeableConcept }], // Intended jurisdiction for activity definition (if applicable)
>  "purpose" : "<markdown>", // Why this activity definition is defined
>  "usage" : "<markdown>", // Describes the clinical usage of the activity definition
>  "copyright" : "<markdown>", // Use and/or publishing restrictions
>  "copyrightLabel" : "<string>", // Copyright holder and year(s)
>  "approvalDate" : "<date>", // When the activity definition was approved by publisher
>  "lastReviewDate" : "<date>", // When the activity definition was last reviewed by the publisher
>  "effectivePeriod" : { Period }, // When the activity definition is expected to be used
>  "topic" : [{ CodeableConcept }], // E.g. Education, Treatment, Assessment, etc
>  "author" : [{ ContactDetail }], // Who authored the content
>  "editor" : [{ ContactDetail }], // Who edited the content
>  "reviewer" : [{ ContactDetail }], // Who reviewed the content
>  "endorser" : [{ ContactDetail }], // Who endorsed the content
>  "relatedArtifact" : [{ RelatedArtifact }], // Additional documentation, citations, etc
>  "library" : ["<canonical(Library)>"], // Logic used by the activity definition
>  "kind" : "<code>", // Kind of resource
>  "profile" : "<canonical(StructureDefinition)>", // What profile the resource needs to conform to
>  "code" : { CodeableConcept }, // Detail type of activity
>  "intent" : "<code>", // proposal | plan | directive | order | original-order | reflex-order | filler-order | instance-order | option
>  "priority" : "<code>", // routine | urgent | asap | stat
>  "doNotPerform" : <boolean>, // True if the activity should not be performed
>  // timing[x]: When activity is to occur. One of these 4:
>  "timingTiming" : { Timing },
>  "timingAge" : { Age },
>  "timingRange" : { Range },
>  "timingDuration" : { Duration },
>  // asNeeded[x]: Preconditions for service. One of these 2:
>  "asNeededBoolean" : <boolean>,
>  "asNeededCodeableConcept" : { CodeableConcept },
>  "location" : { CodeableReference(Location) }, // Where it should happen
>  "participant" : [{ // Who should participate in the action
>    "type" : "<code>", // careteam | device | group | healthcareservice | location | organization | patient | practitioner | practitionerrole | relatedperson
>    "typeCanonical" : "<canonical(CapabilityStatement)>", // Who or what can participate
>    "typeReference" : { Reference(CareTeam|Device|DeviceDefinition|Endpoint|
>    Group|HealthcareService|Location|Organization|Patient|Practitioner|
>    PractitionerRole|RelatedPerson) }, // Who or what can participate
>    "role" : { CodeableConcept }, // E.g. Nurse, Surgeon, Parent, etc icon
>    "function" : { CodeableConcept } // E.g. Author, Reviewer, Witness, etc
>  }],
>  // product[x]: What's administered/supplied. One of these 2:
>  "productReference" : { Reference(Ingredient|Medication|Substance|
>   SubstanceDefinition) },
>  "productCodeableConcept" : { CodeableConcept },
>  "quantity" : { Quantity(SimpleQuantity) }, // How much is administered/consumed/supplied
>  "dosage" : [{ Dosage }], // Detailed dosage instructions
>  "bodySite" : [{ CodeableConcept }], // What part of body to perform on
>  "specimenRequirement" : ["<canonical(SpecimenDefinition)>"], // What specimens are required to perform this action
>  "observationRequirement" : ["<canonical(ObservationDefinition)>"], // What observations are required to perform this action
>  "observationResultRequirement" : ["<canonical(ObservationDefinition)>"], // What observations must be produced by this action
>  "transform" : "<canonical(StructureMap)>", // Transform to apply the template
>  "dynamicValue" : [{ // Dynamic aspects of the definition
>    "path" : "<string>", // R!  The path to the element to be set dynamically
>    "expression" : { Expression } // R!  An expression that provides the dynamic value for the customization
>  }]
>}
>```
>
>Real data example
>```json
>{
>  "resourceType" : "ActivityDefinition",
>  "id" : "referralPrimaryCareMentalHealthEx",
>  "text" : {
>    "status" : "generated",
>    "div" : "<div xmlns=\"http://www.w3.org/1999/xhtml\">\n         <table class=\"grid dict\">\n            <tr>\n               <td>\n                  <b>Id: </b>\n               </td>\n            </tr>\n            <tr>\n               <td style=\"padding-left: 25px; padding-right: 25px;\">ActivityDefinition/referralPrimaryCareMentalHealth</td>\n            </tr>\n         </table>\n         <p/>\n         <table class=\"grid dict\">\n            <tr>\n               <td>\n                  <b>Status: </b>\n               </td>\n            </tr>\n            <tr>\n               <td style=\"padding-left: 25px; padding-right: 25px;\">draft</td>\n            </tr>\n         </table>\n         <p/>\n         <table class=\"grid dict\">\n            <tr>\n               <td>\n                  <b>Description: </b>\n               </td>\n            </tr>\n            <tr>\n               <td style=\"padding-left: 25px; padding-right: 25px;\">refer to primary care mental-health integrated care program for evaluation and treatment of mental health conditions now</td>\n            </tr>\n         </table>\n         <p/>\n         <table class=\"grid dict\">\n            <tr>\n               <td>\n                  <b>Category: </b>\n               </td>\n            </tr>\n            <tr>\n               <td style=\"padding-left: 25px; padding-right: 25px;\">referral</td>\n            </tr>\n         </table>\n         <p/>\n         <table class=\"grid dict\">\n            <tr>\n               <td>\n                  <b>Code: </b>\n               </td>\n            </tr>\n            <tr>\n               <td style=\"padding-right: 25px;\">\n                  <span style=\"padding-left: 25px;\">\n                     <b>text: </b>\n                     <span>Referral to service (procedure)</span>\n                     <br/>\n                  </span>\n                  <span>\n                     <span>\n                        <span style=\"padding-left: 25px;\">\n                           <b>system: </b>\n                           <span>http://snomed.info/sct</span>\n                           <br/>\n                        </span>\n                        <span style=\"padding-left: 25px;\">\n                           <b>code: </b>\n                           <span>306206005</span>\n                           <br/>\n                        </span>\n                     </span>\n                  </span>\n               </td>\n            </tr>\n         </table>\n         <p/>\n         <table class=\"grid dict\">\n            <tr>\n               <td>\n                  <b>Participant: </b>\n               </td>\n            </tr>\n            <tr style=\"vertical-align: top;\">\n               <td style=\"padding-left: 25px; padding-right: 25px;\">practitioner</td>\n            </tr>\n         </table>\n      </div>"
>  },
>  "url" : "http://motivemi.com/artifacts/ActivityDefinition/referralPrimaryCareMentalHealthEx",
>  "identifier" : [{
>    "system" : "urn:ietf:rfc:3986",
>    "value" : "urn:oid:2.16.840.1.113883.4.642.19.8"
>  },
>  {
>    "use" : "official",
>    "system" : "http://motivemi.com/artifacts",
>    "value" : "referralPrimaryCareMentalHealth"
>  }],
>  "version" : "1.1.0",
>  "name" : "ReferralPrimaryCareMentalHealth",
>  "title" : "Referral to Primary Care Mental Health",
>  "status" : "active",
>  "experimental" : true,
>  "date" : "2017-03-03T14:06:00Z",
>  "publisher" : "Motive Medical Intelligence",
>  "contact" : [{
>    "telecom" : [{
>      "system" : "phone",
>      "value" : "415-362-4007",
>      "use" : "work"
>    },
>    {
>      "system" : "email",
>      "value" : "info@motivemi.com",
>      "use" : "work"
>    }]
>  }],
>  "description" : "refer to primary care mental-health integrated care program for evaluation and treatment of mental health conditions now",
>  "useContext" : [{
>    "code" : {
>      "system" : "http://terminology.hl7.org/CodeSystem/usage-context-type",
>      "code" : "age"
>    },
>    "valueCodeableConcept" : {
>      "coding" : [{
>        "system" : "https://meshb.nlm.nih.gov",
>        "code" : "D000328",
>        "display" : "Adult"
>      }]
>    }
>  },
>  {
>    "code" : {
>      "system" : "http://terminology.hl7.org/CodeSystem/usage-context-type",
>      "code" : "focus"
>    },
>    "valueCodeableConcept" : {
>      "coding" : [{
>        "system" : "http://snomed.info/sct",
>        "code" : "87512008",
>        "display" : "Mild major depression"
>      }]
>    }
>  },
>  {
>    "code" : {
>      "system" : "http://terminology.hl7.org/CodeSystem/usage-context-type",
>      "code" : "focus"
>    },
>    "valueCodeableConcept" : {
>      "coding" : [{
>        "system" : "http://snomed.info/sct",
>        "code" : "40379007",
>        "display" : "Major depression, recurrent, mild"
>      }]
>    }
>  },
>  {
>    "code" : {
>      "system" : "http://terminology.hl7.org/CodeSystem/usage-context-type",
>      "code" : "focus"
>    },
>    "valueCodeableConcept" : {
>      "coding" : [{
>        "system" : "http://snomed.info/sct",
>        "code" : "225444004",
>        "display" : "At risk for suicide (finding)"
>      }]
>    }
>  },
>  {
>    "code" : {
>      "system" : "http://terminology.hl7.org/CodeSystem/usage-context-type",
>      "code" : "focus"
>    },
>    "valueCodeableConcept" : {
>      "coding" : [{
>        "system" : "http://snomed.info/sct",
>        "code" : "306206005",
>        "display" : "Referral to service (procedure)"
>      }]
>    }
>  },
>  {
>    "code" : {
>      "system" : "http://terminology.hl7.org/CodeSystem/usage-context-type",
>      "code" : "user"
>    },
>    "valueCodeableConcept" : {
>      "coding" : [{
>        "system" : "http://snomed.info/sct",
>        "code" : "309343006",
>        "display" : "Physician"
>      }]
>    }
>  },
>  {
>    "code" : {
>      "system" : "http://terminology.hl7.org/CodeSystem/usage-context-type",
>      "code" : "venue"
>    },
>    "valueCodeableConcept" : {
>      "coding" : [{
>        "system" : "http://snomed.info/sct",
>        "code" : "440655000",
>        "display" : "Outpatient environment"
>      }]
>    }
>  }],
>  "jurisdiction" : [{
>    "coding" : [{
>      "system" : "urn:iso:std:iso:3166",
>      "code" : "US"
>    }]
>  }],
>  "purpose" : "Defines a referral to a mental-health integrated care program for use in suicide risk order sets. The definition is independent of the order set in which it appears to allow reuse of the general definition of the referrral.",
>  "usage" : "This activity definition is used as the definition of a referral request within various suicide risk order sets. Elements that apply universally are defined here, while elements that apply to the specific setting of a referral within a particular order set are defined in the order set.",
>  "copyright" : "© Copyright 2016 Motive Medical Intelligence. All rights reserved.",
>  "approvalDate" : "2017-03-01",
>  "lastReviewDate" : "2017-03-01",
>  "effectivePeriod" : {
>    "start" : "2017-03-01",
>    "end" : "2017-12-31"
>  },
>  "topic" : [{
>    "text" : "Mental Health Referral"
>  }],
>  "author" : [{
>    "name" : "Motive Medical Intelligence",
>    "telecom" : [{
>      "system" : "phone",
>      "value" : "415-362-4007",
>      "use" : "work"
>    },
>    {
>      "system" : "email",
>      "value" : "info@motivemi.com",
>      "use" : "work"
>    }]
>  }],
>  "relatedArtifact" : [{
>    "type" : "citation",
>    "display" : "Practice Guideline for the Treatment of Patients with Major Depressive Disorder",
>    "document" : {
>      "url" : "http://psychiatryonline.org/pb/assets/raw/sitewide/practice_guidelines/guidelines/mdd.pdf"
>    }
>  },
>  {
>    "type" : "predecessor",
>    "resource" : "http://example.org/fhir/ActivityDefinition/referralPrimaryCareMentalHealth-initial"
>  }],
>  "kind" : "ServiceRequest",
>  "code" : {
>    "coding" : [{
>      "system" : "http://snomed.info/sct",
>      "code" : "306206005"
>    }],
>    "text" : "Referral to service (procedure)"
>  },
>  "timingTiming" : {
>    "_event" : [{
>      "extension" : [{
>        "url" : "http://hl7.org/fhir/StructureDefinition/cqf-expression",
>        "valueExpression" : {
>          "language" : "text/cql",
>          "expression" : "Now()"
>        }
>      }]
>    }]
>  },
>  "participant" : [{
>    "type" : "practitioner"
>  }]
>}
>```
></details>


&nbsp;




#### 6. [Device](https://hl7.org/fhir/device.html)
><details>
><summary>Device scheme</summary>
>A type of a manufactured item that is used in the provision of healthcare without being substantially changed through that activity. The device may be a medical or non-medical device.
>
><br>This is a base resource that tracks individual instances of a device and their location. It is referenced by other resources for recording which device performed an action such as a procedure or an observation, which device was implanted in or explanted from a patient, dispensing a device to a patient for their use, managing inventory, or when requesting a specific device for a patient's use. Medical devices include durable (reusable) medical equipment, implantable devices, as well as disposable equipment used for diagnostic, treatment, and research for healthcare and public health. Medical devices may also include some types of software.
>
>For more details for each data type of the schema, please see [here](https://hl7.org/fhir/device.html).
>```json
>{
>  "resourceType" : "Device",
>  // from Resource: id, meta, implicitRules, and language
>  // from DomainResource: text, contained, extension, and modifierExtension
>  "identifier" : [{ Identifier }], // Instance identifier
>  "displayName" : "<string>", // The name used to display by default when the device is referenced
>  "definition" : { CodeableReference(DeviceDefinition) }, // The reference to the definition for the device
>  "udiCarrier" : [{ // Unique Device Identifier (UDI) Barcode string
>    "deviceIdentifier" : "<string>", // R!  Mandatory fixed portion of UDI
>    "issuer" : "<uri>", // R!  UDI Issuing Organization
>    "jurisdiction" : "<uri>", // Regional UDI authority
>    "carrierAIDC" : "<base64Binary>", // UDI Machine Readable Barcode String
>    "carrierHRF" : "<string>", // UDI Human Readable Barcode String
>    "entryType" : "<code>" // barcode | rfid | manual | card | self-reported | electronic-transmission | unknown
>  }],
>  "status" : "<code>", // active | inactive | entered-in-error
>  "availabilityStatus" : { CodeableConcept }, // lost | damaged | destroyed | available
>  "biologicalSourceEvent" : { Identifier }, // An identifier that supports traceability to the event during which material in this product from one or more biological entities was obtained or pooled
>  "manufacturer" : "<string>", // Name of device manufacturer
>  "manufactureDate" : "<dateTime>", // Date when the device was made
>  "expirationDate" : "<dateTime>", // Date and time of expiry of this device (if applicable)
>  "lotNumber" : "<string>", // Lot number of manufacture
>  "serialNumber" : "<string>", // Serial number assigned by the manufacturer
>  "name" : [{ // I The name or names of the device as known to the manufacturer and/or patient
>    "value" : "<string>", // R!  The term that names the device
>    "type" : "<code>", // R!  registered-name | user-friendly-name | patient-reported-name
>    "display" : <boolean> // I The preferred device name
>  }],
>  "modelNumber" : "<string>", // The manufacturer's model number for the device
>  "partNumber" : "<string>", // The part number or catalog number of the device
>  "category" : [{ CodeableConcept }], // Indicates a high-level grouping of the device
>  "type" : [{ CodeableConcept }], // The kind or type of device
>  "version" : [{ // The actual design of the device or software version running on the device
>    "type" : { CodeableConcept }, // The type of the device version, e.g. manufacturer, approved, internal
>    "component" : { Identifier }, // The hardware or software module of the device to which the version applies
>    "installDate" : "<dateTime>", // The date the version was installed on the device
>    "value" : "<string>" // R!  The version text
>  }],
>  "conformsTo" : [{ // Identifies the standards, specifications, or formal guidances for the capabilities supported by the device
>    "category" : { CodeableConcept }, // Describes the common type of the standard, specification, or formal guidance.  communication | performance | measurement
>    "specification" : { CodeableConcept }, // R!  Identifies the standard, specification, or formal guidance that the device adheres to
>    "version" : "<string>" // Specific form or variant of the standard
>  }],
>  "property" : [{ // Inherent, essentially fixed, characteristics of the device.  e.g., time properties, size, material, etc.
>    "type" : { CodeableConcept }, // R!  Code that specifies the property being represented
>    // value[x]: Value of the property. One of these 7:
>    "valueQuantity" : { Quantity },
>    "valueCodeableConcept" : { CodeableConcept },
>    "valueString" : "<string>",
>    "valueBoolean" : <boolean>,
>    "valueInteger" : <integer>,
>    "valueRange" : { Range },
>    "valueAttachment" : { Attachment }
>  }],
>  "mode" : { CodeableConcept }, // The designated condition for performing a task
>  "cycle" : { Count }, // The series of occurrences that repeats during the operation of the device
>  "duration" : { Duration }, // A measurement of time during the device's operation (e.g., days, hours, mins, etc.)
>  "owner" : { Reference(Organization) }, // Organization responsible for device
>  "contact" : [{ ContactPoint }], // Details for human/organization for support
>  "location" : { Reference(Location) }, // Where the device is found
>  "url" : "<uri>", // Network address to contact device
>  "endpoint" : [{ Reference(Endpoint) }], // Technical endpoints providing access to electronic services provided by the device
>  "gateway" : [{ CodeableReference(Device) }], // Linked device acting as a communication/data collector, translator or controller
>  "note" : [{ Annotation }], // Device notes and comments
>  "safety" : [{ CodeableConcept }], // Safety Characteristics of Device
>  "parent" : { Reference(Device) } // The higher level or encompassing device that this device is a logical part of
>}
>```
>
>Real data example
>```json
>{
>  "resourceType" : "Device",
>  "id" : "example",
>  "text" : {
>    "status" : "generated",
>    "div" : "<div xmlns=\"http://www.w3.org/1999/xhtml\"><p><b>Generated Narrative: Device</b><a name=\"example\"> </a></p><div style=\"display: inline-block; background-color: #d9e0e7; padding: 6px; margin: 4px; border: 1px solid #8da1b4; border-radius: 5px; line-height: 60%\"><p style=\"margin-bottom: 0px\">Resource Device &quot;example&quot; </p></div><p><b>identifier</b>: id:\u00a0345675</p></div>"
>  },
>  "identifier" : [{
>    "system" : "http://goodcare.org/devices/id",
>    "value" : "345675"
>  }]
>}
>```
></details>


&nbsp;



><details>
><summary>ActivityDefinition scheme</summary>
>This resource allows for the definition of some activity to be performed, independent of a particular patient, practitioner, or other performance context.
>
><br>An ActivityDefinition is a shareable, consumable description of some activity to be performed. It may be used to specify actions to be taken as part of a workflow, order set, or protocol, or it may be used independently as part of a catalog of activities such as orderables.
>
>For more details for each data type of the schema, please see [here](https://hl7.org/fhir/activitydefinition.html).
>```json
>{
>  "resourceType" : "ActivityDefinition",
>  // from Resource: id, meta, implicitRules, and language
>  // from DomainResource: text, contained, extension, and modifierExtension
>  "url" : "<uri>", // Canonical identifier for this activity definition, represented as a URI (globally unique)
>  "identifier" : [{ Identifier }], // Additional identifier for the activity definition
>  "version" : "<string>", // Business version of the activity definition
>  // versionAlgorithm[x]: How to compare versions. One of these 2:
>  "versionAlgorithmString" : "<string>",
>  "versionAlgorithmCoding" : { Coding },
>  "name" : "<string>", // I Name for this activity definition (computer friendly)
>  "title" : "<string>", // Name for this activity definition (human friendly)
>  "subtitle" : "<string>", // Subordinate title of the activity definition
>  "status" : "<code>", // R!  draft | active | retired | unknown
>  "experimental" : <boolean>, // For testing purposes, not real usage
>  // subject[x]: Type of individual the activity definition is intended for. One of these 3:
>  "subjectCodeableConcept" : { CodeableConcept },
>  "subjectReference" : { Reference(AdministrableProductDefinition|Group|
>   ManufacturedItemDefinition|MedicinalProductDefinition|
>   PackagedProductDefinition|SubstanceDefinition) },
>  "subjectCanonical" : "<canonical(EvidenceVariable)>",
>  "date" : "<dateTime>", // Date last changed
>  "publisher" : "<string>", // Name of the publisher/steward (organization or individual)
>  "contact" : [{ ContactDetail }], // Contact details for the publisher
>  "description" : "<markdown>", // Natural language description of the activity definition
>  "useContext" : [{ UsageContext }], // The context that the content is intended to support
>  "jurisdiction" : [{ CodeableConcept }], // Intended jurisdiction for activity definition (if applicable)
>  "purpose" : "<markdown>", // Why this activity definition is defined
>  "usage" : "<markdown>", // Describes the clinical usage of the activity definition
>  "copyright" : "<markdown>", // Use and/or publishing restrictions
>  "copyrightLabel" : "<string>", // Copyright holder and year(s)
>  "approvalDate" : "<date>", // When the activity definition was approved by publisher
>  "lastReviewDate" : "<date>", // When the activity definition was last reviewed by the publisher
>  "effectivePeriod" : { Period }, // When the activity definition is expected to be used
>  "topic" : [{ CodeableConcept }], // E.g. Education, Treatment, Assessment, etc
>  "author" : [{ ContactDetail }], // Who authored the content
>  "editor" : [{ ContactDetail }], // Who edited the content
>  "reviewer" : [{ ContactDetail }], // Who reviewed the content
>  "endorser" : [{ ContactDetail }], // Who endorsed the content
>  "relatedArtifact" : [{ RelatedArtifact }], // Additional documentation, citations, etc
>  "library" : ["<canonical(Library)>"], // Logic used by the activity definition
>  "kind" : "<code>", // Kind of resource
>  "profile" : "<canonical(StructureDefinition)>", // What profile the resource needs to conform to
>  "code" : { CodeableConcept }, // Detail type of activity
>  "intent" : "<code>", // proposal | plan | directive | order | original-order | reflex-order | filler-order | instance-order | option
>  "priority" : "<code>", // routine | urgent | asap | stat
>  "doNotPerform" : <boolean>, // True if the activity should not be performed
>  // timing[x]: When activity is to occur. One of these 4:
>  "timingTiming" : { Timing },
>  "timingAge" : { Age },
>  "timingRange" : { Range },
>  "timingDuration" : { Duration },
>  // asNeeded[x]: Preconditions for service. One of these 2:
>  "asNeededBoolean" : <boolean>,
>  "asNeededCodeableConcept" : { CodeableConcept },
>  "location" : { CodeableReference(Location) }, // Where it should happen
>  "participant" : [{ // Who should participate in the action
>    "type" : "<code>", // careteam | device | group | healthcareservice | location | organization | patient | practitioner | practitionerrole | relatedperson
>    "typeCanonical" : "<canonical(CapabilityStatement)>", // Who or what can participate
>    "typeReference" : { Reference(CareTeam|Device|DeviceDefinition|Endpoint|
>    Group|HealthcareService|Location|Organization|Patient|Practitioner|
>    PractitionerRole|RelatedPerson) }, // Who or what can participate
>    "role" : { CodeableConcept }, // E.g. Nurse, Surgeon, Parent, etc icon
>    "function" : { CodeableConcept } // E.g. Author, Reviewer, Witness, etc
>  }],
>  // product[x]: What's administered/supplied. One of these 2:
>  "productReference" : { Reference(Ingredient|Medication|Substance|
>   SubstanceDefinition) },
>  "productCodeableConcept" : { CodeableConcept },
>  "quantity" : { Quantity(SimpleQuantity) }, // How much is administered/consumed/supplied
>  "dosage" : [{ Dosage }], // Detailed dosage instructions
>  "bodySite" : [{ CodeableConcept }], // What part of body to perform on
>  "specimenRequirement" : ["<canonical(SpecimenDefinition)>"], // What specimens are required to perform this action
>  "observationRequirement" : ["<canonical(ObservationDefinition)>"], // What observations are required to perform this action
>  "observationResultRequirement" : ["<canonical(ObservationDefinition)>"], // What observations must be produced by this action
>  "transform" : "<canonical(StructureMap)>", // Transform to apply the template
>  "dynamicValue" : [{ // Dynamic aspects of the definition
>    "path" : "<string>", // R!  The path to the element to be set dynamically
>    "expression" : { Expression } // R!  An expression that provides the dynamic value for the customization
>  }]
>}
>```
>
>Real data example
>```json
>{
>  "resourceType" : "ActivityDefinition",
>  "id" : "referralPrimaryCareMentalHealthEx",
>  "text" : {
>    "status" : "generated",
>    "div" : "<div xmlns=\"http://www.w3.org/1999/xhtml\">\n         <table class=\"grid dict\">\n            <tr>\n               <td>\n                  <b>Id: </b>\n               </td>\n            </tr>\n            <tr>\n               <td style=\"padding-left: 25px; padding-right: 25px;\">ActivityDefinition/referralPrimaryCareMentalHealth</td>\n            </tr>\n         </table>\n         <p/>\n         <table class=\"grid dict\">\n            <tr>\n               <td>\n                  <b>Status: </b>\n               </td>\n            </tr>\n            <tr>\n               <td style=\"padding-left: 25px; padding-right: 25px;\">draft</td>\n            </tr>\n         </table>\n         <p/>\n         <table class=\"grid dict\">\n            <tr>\n               <td>\n                  <b>Description: </b>\n               </td>\n            </tr>\n            <tr>\n               <td style=\"padding-left: 25px; padding-right: 25px;\">refer to primary care mental-health integrated care program for evaluation and treatment of mental health conditions now</td>\n            </tr>\n         </table>\n         <p/>\n         <table class=\"grid dict\">\n            <tr>\n               <td>\n                  <b>Category: </b>\n               </td>\n            </tr>\n            <tr>\n               <td style=\"padding-left: 25px; padding-right: 25px;\">referral</td>\n            </tr>\n         </table>\n         <p/>\n         <table class=\"grid dict\">\n            <tr>\n               <td>\n                  <b>Code: </b>\n               </td>\n            </tr>\n            <tr>\n               <td style=\"padding-right: 25px;\">\n                  <span style=\"padding-left: 25px;\">\n                     <b>text: </b>\n                     <span>Referral to service (procedure)</span>\n                     <br/>\n                  </span>\n                  <span>\n                     <span>\n                        <span style=\"padding-left: 25px;\">\n                           <b>system: </b>\n                           <span>http://snomed.info/sct</span>\n                           <br/>\n                        </span>\n                        <span style=\"padding-left: 25px;\">\n                           <b>code: </b>\n                           <span>306206005</span>\n                           <br/>\n                        </span>\n                     </span>\n                  </span>\n               </td>\n            </tr>\n         </table>\n         <p/>\n         <table class=\"grid dict\">\n            <tr>\n               <td>\n                  <b>Participant: </b>\n               </td>\n            </tr>\n            <tr style=\"vertical-align: top;\">\n               <td style=\"padding-left: 25px; padding-right: 25px;\">practitioner</td>\n            </tr>\n         </table>\n      </div>"
>  },
>  "url" : "http://motivemi.com/artifacts/ActivityDefinition/referralPrimaryCareMentalHealthEx",
>  "identifier" : [{
>    "system" : "urn:ietf:rfc:3986",
>    "value" : "urn:oid:2.16.840.1.113883.4.642.19.8"
>  },
>  {
>    "use" : "official",
>    "system" : "http://motivemi.com/artifacts",
>    "value" : "referralPrimaryCareMentalHealth"
>  }],
>  "version" : "1.1.0",
>  "name" : "ReferralPrimaryCareMentalHealth",
>  "title" : "Referral to Primary Care Mental Health",
>  "status" : "active",
>  "experimental" : true,
>  "date" : "2017-03-03T14:06:00Z",
>  "publisher" : "Motive Medical Intelligence",
>  "contact" : [{
>    "telecom" : [{
>      "system" : "phone",
>      "value" : "415-362-4007",
>      "use" : "work"
>    },
>    {
>      "system" : "email",
>      "value" : "info@motivemi.com",
>      "use" : "work"
>    }]
>  }],
>  "description" : "refer to primary care mental-health integrated care program for evaluation and treatment of mental health conditions now",
>  "useContext" : [{
>    "code" : {
>      "system" : "http://terminology.hl7.org/CodeSystem/usage-context-type",
>      "code" : "age"
>    },
>    "valueCodeableConcept" : {
>      "coding" : [{
>        "system" : "https://meshb.nlm.nih.gov",
>        "code" : "D000328",
>        "display" : "Adult"
>      }]
>    }
>  },
>  {
>    "code" : {
>      "system" : "http://terminology.hl7.org/CodeSystem/usage-context-type",
>      "code" : "focus"
>    },
>    "valueCodeableConcept" : {
>      "coding" : [{
>        "system" : "http://snomed.info/sct",
>        "code" : "87512008",
>        "display" : "Mild major depression"
>      }]
>    }
>  },
>  {
>    "code" : {
>      "system" : "http://terminology.hl7.org/CodeSystem/usage-context-type",
>      "code" : "focus"
>    },
>    "valueCodeableConcept" : {
>      "coding" : [{
>        "system" : "http://snomed.info/sct",
>        "code" : "40379007",
>        "display" : "Major depression, recurrent, mild"
>      }]
>    }
>  },
>  {
>    "code" : {
>      "system" : "http://terminology.hl7.org/CodeSystem/usage-context-type",
>      "code" : "focus"
>    },
>    "valueCodeableConcept" : {
>      "coding" : [{
>        "system" : "http://snomed.info/sct",
>        "code" : "225444004",
>        "display" : "At risk for suicide (finding)"
>      }]
>    }
>  },
>  {
>    "code" : {
>      "system" : "http://terminology.hl7.org/CodeSystem/usage-context-type",
>      "code" : "focus"
>    },
>    "valueCodeableConcept" : {
>      "coding" : [{
>        "system" : "http://snomed.info/sct",
>        "code" : "306206005",
>        "display" : "Referral to service (procedure)"
>      }]
>    }
>  },
>  {
>    "code" : {
>      "system" : "http://terminology.hl7.org/CodeSystem/usage-context-type",
>      "code" : "user"
>    },
>    "valueCodeableConcept" : {
>      "coding" : [{
>        "system" : "http://snomed.info/sct",
>        "code" : "309343006",
>        "display" : "Physician"
>      }]
>    }
>  },
>  {
>    "code" : {
>      "system" : "http://terminology.hl7.org/CodeSystem/usage-context-type",
>      "code" : "venue"
>    },
>    "valueCodeableConcept" : {
>      "coding" : [{
>        "system" : "http://snomed.info/sct",
>        "code" : "440655000",
>        "display" : "Outpatient environment"
>      }]
>    }
>  }],
>  "jurisdiction" : [{
>    "coding" : [{
>      "system" : "urn:iso:std:iso:3166",
>      "code" : "US"
>    }]
>  }],
>  "purpose" : "Defines a referral to a mental-health integrated care program for use in suicide risk order sets. The definition is independent of the order set in which it appears to allow reuse of the general definition of the referrral.",
>  "usage" : "This activity definition is used as the definition of a referral request within various suicide risk order sets. Elements that apply universally are defined here, while elements that apply to the specific setting of a referral within a particular order set are defined in the order set.",
>  "copyright" : "© Copyright 2016 Motive Medical Intelligence. All rights reserved.",
>  "approvalDate" : "2017-03-01",
>  "lastReviewDate" : "2017-03-01",
>  "effectivePeriod" : {
>    "start" : "2017-03-01",
>    "end" : "2017-12-31"
>  },
>  "topic" : [{
>    "text" : "Mental Health Referral"
>  }],
>  "author" : [{
>    "name" : "Motive Medical Intelligence",
>    "telecom" : [{
>      "system" : "phone",
>      "value" : "415-362-4007",
>      "use" : "work"
>    },
>    {
>      "system" : "email",
>      "value" : "info@motivemi.com",
>      "use" : "work"
>    }]
>  }],
>  "relatedArtifact" : [{
>    "type" : "citation",
>    "display" : "Practice Guideline for the Treatment of Patients with Major Depressive Disorder",
>    "document" : {
>      "url" : "http://psychiatryonline.org/pb/assets/raw/sitewide/practice_guidelines/guidelines/mdd.pdf"
>    }
>  },
>  {
>    "type" : "predecessor",
>    "resource" : "http://example.org/fhir/ActivityDefinition/referralPrimaryCareMentalHealth-initial"
>  }],
>  "kind" : "ServiceRequest",
>  "code" : {
>    "coding" : [{
>      "system" : "http://snomed.info/sct",
>      "code" : "306206005"
>    }],
>    "text" : "Referral to service (procedure)"
>  },
>  "timingTiming" : {
>    "_event" : [{
>      "extension" : [{
>        "url" : "http://hl7.org/fhir/StructureDefinition/cqf-expression",
>        "valueExpression" : {
>          "language" : "text/cql",
>          "expression" : "Now()"
>        }
>      }]
>    }]
>  },
>  "participant" : [{
>    "type" : "practitioner"
>  }]
>}
>```
></details>


&nbsp;




#### 7. [HealthcareService](https://hl7.org/fhir/healthcareservice.html)
><details>
><summary>Device scheme</summary>
>The details of a healthcare service available at a location.
>
><br>The HealthcareService resource is used to describe a single healthcare service or category of services that are provided by an organization at a location.
The location of the services could be virtual, as with telemedicine services.
>
>For more details for each data type of the schema, please see [here](https://hl7.org/fhir/healthcareservice.html).
>```json
>{
>  "resourceType" : "HealthcareService",
>  // from Resource: id, meta, implicitRules, and language
>  // from DomainResource: text, contained, extension, and modifierExtension
>  "identifier" : [{ Identifier }], // External identifiers for this item
>  "active" : <boolean>, // Whether this HealthcareService record is in active use
>  "providedBy" : { Reference(Organization) }, // Organization that provides this service
>  "offeredIn" : [{ Reference(HealthcareService) }], // The service within which this service is offered
>  "category" : [{ CodeableConcept }], // Broad category of service being performed or delivered
>  "type" : [{ CodeableConcept }], // Type of service that may be delivered or performed
>  "specialty" : [{ CodeableConcept }], // Specialties handled by the HealthcareService
>  "location" : [{ Reference(Location) }], // Location(s) where service may be provided
>  "name" : "<string>", // Description of service as presented to a consumer while searching
>  "comment" : "<markdown>", // Additional description and/or any specific issues not covered elsewhere
>  "extraDetails" : "<markdown>", // Extra details about the service that can't be placed in the other fields
>  "photo" : { Attachment }, // Facilitates quick identification of the service
>  "contact" : [{ ExtendedContactDetail }], // Official contact details for the HealthcareService
>  "coverageArea" : [{ Reference(Location) }], // Location(s) service is intended for/available to
>  "serviceProvisionCode" : [{ CodeableConcept }], // Conditions under which service is available/offered
>  "eligibility" : [{ // Specific eligibility requirements required to use the service
>    "code" : { CodeableConcept }, // Coded value for the eligibility
>    "comment" : "<markdown>" // Describes the eligibility conditions for the service
>  }],
>  "program" : [{ CodeableConcept }], // Programs that this service is applicable to
>  "characteristic" : [{ CodeableConcept }], // Collection of characteristics (attributes)
>  "communication" : [{ CodeableConcept }], // The language that this service is offered in
>  "referralMethod" : [{ CodeableConcept }], // Ways that the service accepts referrals
>  "appointmentRequired" : <boolean>, // If an appointment is required for access to this service
>  "availability" : [{ Availability }], // Times the healthcare service is available (including exceptions)
>  "endpoint" : [{ Reference(Endpoint) }] // Technical endpoints providing access to electronic services operated for the healthcare service
>}
>```
>
>Real data example
>```json
>{
>  "resourceType" : "HealthcareService",
>  "id" : "example",
>  "text" : {
>    "status" : "generated",
>    "div" : "<div xmlns=\"http://www.w3.org/1999/xhtml\">\n\t\t\t25 Dec 2013 9:15am - 9:30am: <b>Busy</b> Physiotherapy\n\t\t</div>"
>  },
>  "contained" : [{
>    "resourceType" : "Location",
>    "id" : "DenBurg",
>    "description" : "Greater Denburg area",
>    "mode" : "instance",
>    "form" : {
>      "coding" : [{
>        "code" : "area",
>        "display" : "Area"
>      }]
>    }
>  }],
>  "identifier" : [{
>    "system" : "http://example.org/shared-ids",
>    "value" : "HS-12"
>  }],
>  "active" : true,
>  "providedBy" : {
>    "reference" : "Organization/f001",
>    "display" : "Burgers University Medical Center"
>  },
>  "category" : [{
>    "coding" : [{
>      "system" : "http://terminology.hl7.org/CodeSystem/service-category",
>      "code" : "8",
>      "display" : "Counselling"
>    }],
>    "text" : "Counselling"
>  }],
>  "type" : [{
>    "coding" : [{
>      "system" : "http://snomed.info/sct",
>      "code" : "394913002",
>      "display" : "Psychotherapy"
>    }]
>  },
>  {
>    "coding" : [{
>      "system" : "http://snomed.info/sct",
>      "code" : "394587001",
>      "display" : "Psychiatry"
>    }]
>  }],
>  "specialty" : [{
>    "coding" : [{
>      "system" : "http://snomed.info/sct",
>      "code" : "47505003",
>      "display" : "Posttraumatic stress disorder"
>    }]
>  }],
>  "location" : [{
>    "reference" : "Location/1"
>  }],
>  "name" : "Consulting psychologists and/or psychology services",
>  "comment" : "Providing Specialist psychology services to the greater Den Burg area, many years of experience dealing with PTSD issues",
>  "extraDetails" : "Several assessments are required for these specialist services, and the waiting times can be greater than 3 months at times. Existing patients are prioritized when requesting appointments on the schedule.",
>  "contact" : [{
>    "telecom" : [{
>      "system" : "phone",
>      "value" : "(555) silent",
>      "use" : "work"
>    },
>    {
>      "system" : "email",
>      "value" : "directaddress@example.com",
>      "use" : "work"
>    }]
>  }],
>  "coverageArea" : [{
>    "reference" : "#DenBurg",
>    "display" : "Greater Denburg area"
>  }],
>  "serviceProvisionCode" : [{
>    "coding" : [{
>      "system" : "http://terminology.hl7.org/CodeSystem/service-provision-conditions",
>      "code" : "cost",
>      "display" : "Fees apply"
>    }]
>  }],
>  "eligibility" : [{
>    "code" : {
>      "coding" : [{
>        "display" : "DVA Required"
>      }]
>    },
>    "comment" : "Evidence of application for DVA status may be sufficient for commencing assessment"
>  }],
>  "program" : [{
>    "text" : "PTSD outreach"
>  }],
>  "characteristic" : [{
>    "coding" : [{
>      "display" : "Wheelchair access"
>    }]
>  }],
>  "referralMethod" : [{
>    "coding" : [{
>      "code" : "phone",
>      "display" : "Phone"
>    }]
>  },
>  {
>    "coding" : [{
>      "code" : "fax",
>      "display" : "Fax"
>    }]
>  },
>  {
>    "coding" : [{
>      "code" : "elec",
>      "display" : "Secure Messaging"
>    }]
>  },
>  {
>    "coding" : [{
>      "code" : "semail",
>      "display" : "Secure Email"
>    }]
>  }],
>  "appointmentRequired" : false,
>  "availability" : [{
>    "availableTime" : [{
>      "daysOfWeek" : ["wed"],
>      "allDay" : true
>    },
>    {
>      "daysOfWeek" : ["mon",
>      "tue",
>      "thu",
>      "fri"],
>      "availableStartTime" : "08:30:00",
>      "availableEndTime" : "05:30:00"
>    },
>    {
>      "daysOfWeek" : ["sat",
>      "fri"],
>      "availableStartTime" : "09:30:00",
>      "availableEndTime" : "04:30:00"
>    }],
>    "notAvailableTime" : [{
>      "description" : "Christmas/Boxing Day, Reduced capacity is available during the Christmas period",
>      "during" : {
>        "start" : "2015-12-25",
>        "end" : "2015-12-26"
>      }
>    },
>    {
>      "description" : "New Years Day",
>      "during" : {
>        "start" : "2016-01-01",
>        "end" : "2016-01-01"
>      }
>    }]
>  }],
>  "endpoint" : [{
>    "reference" : "Endpoint/example"
>  }]
>}
>```
></details>


&nbsp;