### Overview
This FHIR implementation Guide describes the exchange of patient demographic, clinical and coverage data to support fulfillment of specialty medication prescriptions by pharmacies, and enrollment of patients into related support programs offered by parties such as as Hub vendors and pharmaceutical manufacturers.

This guide is co-branded between HL7 and the National Council for Prescription Drug Programs (NCPDP).

### Background

The current process for exchanging information to support specialty medication fulfillment is complex and manual, often resulting in delays of days to weeks before patients can begin their therapy. This is caused largely by the absence of an industry standard for exchanging clinical data needed to dispense specialty medications and enroll the patient into associated support and savings programs.

NCPDP created a task group to focus on the exchange of this data, with the goal of reducing the time to therapy for a patient who has been prescribed a specialty medication. The group identified demographic, clinical and coverage information needed to support the process, and defined data exchange workflows between the clinic, pharmacy and other fulfilling parties to efficiently deliver the data to the parties that need it.

This information is outside of the current NCPDP e-prescription standard that conveys specialty orders to pharmacies in the US today. After an extensive analysis of the types of information required beyond that carried in the prescription, the task group determined that developing an implementation guide using HL7 FHIR would be the best approach for enabling the exchange of this information. 

### Scope of This Guide

This guide addresses the exchange of information to support specialty medication fulfillment functions, including:

- medication dispensing and billing
- specialty enrollment for programs or services
- facilitation of clinical care management, education, and training.

The guide does not address transmission of the original prescription from the prescriber to dispensing pharmacy, which is typically accomplished in the US using the NCPDP SCRIPT standard.

This implementation guide is intended be used in the United States. It reflects US pharmacy processes and conventions.

### Content and Organization

The guide is organized into the following sections:

- [Use Cases and Roles](roles.html) gives an overview of the guide's goals and participants.
- [Systematic Query Workflows](systematic-queries.html) describes the exchanges that retrieve information systematically from the patient's records.
- [Information Flows Requiring Human Interaction](human-interaction.html) describes a process enabling the prescriber or staff to provide information through a SMART application launched from the EHR.
- [Search Conventions](searches.html) defines required and recommended searches and conventions for specifying searches in Specialty Rx messages.
- [Patient Matching](patient-matching.html) describes methods for identifying the patient for whom information is being exchanged.
- [Message Structures](message-structure.html) and [Message Submission](message-submission.html) give details related to the Specialty Rx messaging options.
- [Error Handling](error-handling.html) defines expectations for handling exception situations.
- [Conformance Expectations](missing-data.html) defines use of Must Support elements and also describes conventions for situations where information is not available.
- [Intermediary Facilitation](intermediary.html) describes how an intermediary can enable exchange between implementers of the guide's RESTful and messaging options.
- [Security](security.html) provides information for implementers related to security and privacy.
- [Specialty Rx Profiles, Terminology and Examples](artifacts.html) lists all profiles and terminology defined in this guide. It also provides examples instances that conform to the guide's profiles.
- [Applicable US Core Profiles](us-core-profiles.html) defines expectations for the use of US Core profiles in the guide's searches.
- [Capability Statements](capability-statements.html) defines sets of guide functionality that can be implemented individually or in combination.

### FHIR Basics 

For those new to FHIR, the material below describes basic FHIR principles and gives guidance for reading FHIR specifications.

- [FHIR overview](http://hl7.org/fhir/R4/overview.html)
- [Developer’s introduction](http://hl7.org/fhir/R4/overview-dev.html) (or [Clinical introduction](http://hl7.org/fhir/R4/overview-clinical.html))
- [FHIR data types](http://hl7.org/fhir/R4/datatypes.html)
- [Using codes](http://hl7.org/fhir/R4/terminologies.html)
- [References between resources](http://hl7.org/fhir/R4/references.html)
- [How to read resource & profile definitions](http://hl7.org/fhir/R4/formats.html)
- [Base resource](http://hl7.org/fhir/R4/resource.html)

### Authors

  <table class="grid">
    <tbody>
	  <tr>
		<td>HL7 Pharmacy Workgroup</td>
		<td></td>
  	  </tr>
	  <tr>
		<td>NCPDP Workgroup 18</td>
		<td>Specialty Requirements for ePrescribing Taskgroup</td>
  	  </tr>
	  <tr>
		<td>Frank McKinney</td>
		<td><a href="mailto:frank.mckinney@pocp.com">frank.mckinney@pocp.com</a></td>
	  </tr>
	</tbody>
  </table>
<br />
















