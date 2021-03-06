### Required Searches

To ensure that the most common data requests are supported by all participants, Data Sources SHALL be able to return information in response to the following search strings--whether responding to RESTful search requests or the Specialty Rx Query message. The required search parameters match those required for each resource by the US Core.

*Note: When specifying searches in the Specialty Rx Query message, the patient parameter is omitted (see [below](http://build.fhir.org/ig/HL7/fhir-specialty-rx/branches/master/searches.html#searches-in-the-specialty-rx-query-message))*

<table class="grid">
<thead>
<tr>
<th>Resource</th>
<th style="width:150px">Parameters</th>
<th>Example</th>
</tr>
</thead>
<tbody>
<tr>
<td>Condition</td>
<td>category<br/>clinical-status<br/>onset-date<br/>code<br/>patient</td>
<td><pre>Condition?patient=123</pre>Returns all patient conditions</td>
</tr>
<tr>
<td>AllergyIntolerance</td>
<td>clinical-status<br/>patient</td>
    <td><pre>AllergyIntolerance?patient=123&amp;clinical-status=active</pre>Returns all active patient allergies and intolerances
    </td></tr>
<tr>
<td>Observation</td>
<td>status<br/>category <br/>code<br/>date<br/>patient</td>
    <td><pre>Observation?patient=123&amp;category=vital-signs&amp;date=ge2020-01-01</pre>Returns all patient vital signs recorded in the specified date period</td>
</tr>
<tr>
<td>Coverage</td>
<td>patient</td>
<td><pre>Coverage?patient=123</pre>Returns all patient insurance coverages</td>
</tr>
<tr>
<td>MedicationRequest</td>
<td>status<br/>intent<br/>encounter<br/>authoredon<br/>patient<br/>_include</td>
<td><pre>MedicationRequest?patient=123&amp;status=active&amp;_include=MedicationRequest:Medication</pre>Returns all active patient MedicationRequests and the associated Medications</td>
</tr>
</tbody>
</table>

<p></p>

### Recommended Searches

In addition to the required searches above, implementers SHOULD support DocumentReference searches and retrieval, to enable those participating in the fulfillment of specialty medication prescriptions to access patient information recorded in patient documents.

<table class="grid">
<thead>
<tr>
<th>Resource</th>
<th style="width:150px">Parameters</th>
<th>Example</th>
</tr>
</thead>
<tbody>
<tr>
<td>DocumentReference</td>
<td>category<br/>date<br/>type<br/>patient<br/>_id</td>
<td><pre>DocumentReference?patient=123 &amp;category=http://hl7.org/fhir/us/core/CodeSystem/us-core-documentreference-category|clinical-note</pre>Returns all patient clinical notes</td>
</tr>
</tbody>
</table>
<p></p>
### Optional Searches

Requesters MAY include searches other than those outlined above. 

- Data Source Systems SHALL return a [search-result](StructureDefinition-specialty-rx-bundle-search-result.html) for each--even if the search is not supported--which SHALL include an OperationOutcome indicating when the responder does not support the submitted search or search parameter.  

<p></p>
### Further search guidance

#### US Core

This guide leverages search guidance given in the US Core FHIR Implementation Guide. Below are particular sections in that IG that detail certain search aspects:

- [Search syntax](https://www.hl7.org/fhir/us/core/general-guidance.html#search-syntax)
- [AllergyIntolerance search parameters](https://www.hl7.org/fhir/us/core/StructureDefinition-us-core-allergyintolerance.html#mandatory-search-parameters)
- [Condition search parameters](https://www.hl7.org/fhir/us/core/StructureDefinition-us-core-condition.html#mandatory-search-parameters)
- [DocumentReference search parameters](https://www.hl7.org/fhir/us/core/StructureDefinition-us-core-documentreference.html#mandatory-search-parameters)
- [Lab Observation search parameters](https://www.hl7.org/fhir/us/core/StructureDefinition-us-core-observation-lab.html#mandatory-search-parameters)
- [MedicationRequest search parameters](https://www.hl7.org/fhir/us/core/StructureDefinition-us-core-medicationrequest.html#mandatory-search-parameters)

#### Base FHIR

- [FHIR Search](http://hl7.org/fhir/R4/search.html)

<p></p>
### Search conventions in the Specialty Rx Query Message

The Specialty Rx Query message requests specific information from a given patient's records in the responding system. The desired information is specified as FHIR search statements which are sent as parameters in the message. Responders are expected to execute these searches for the patient supplied in the request and return the results in a Specialty Rx Query Response message.

#### Specifying a query string

Specialty Rx messaging enables requesters to submit requests without performing a patient match ahead of time. This accommodates a common situation where the requester hasn't participated in previous FHIR exchanges about this patient with the responder and doesn't possess the responder's Patient resource ID. This also addresses challenges that may arise if the requester interacts with the responder via an intermediary--for example if the requester lacks details about the responder's endpoint or is otherwise unable to submit patient match requests directly to the responder.

To support this scenario, the Specialty Rx Query message omits the patient parameter from submitted search strings, with the expectation that the responder will append it after locating the desired patient in its system. 

For example, a search for a given patient's vital signs that that would ordinarily be stated as:

`Observation?patient=[patient id]&category=vital-signs`

... is submitted in the Specialty Rx Query without the patient search parameter:

`Observation?category=vital-signs`

#### Identifying the desired patient

The requester identifies the desired patient by including one or both the following in the Specialty Rx Query message:

- a Patient resource representing the patient in the requester's system (mandatory)
- a Patient resource from the responder's system (if available)

The responder locates the desired patient and completes the query string as follows, based on the submitted patient information:

- If the **request includes only the requester's Patient**, the responder performs a matching process and locates the associated patient in its own system. It appends its Patient ID to each submitted search string.
- If the **request includes both the requester's Patient and responder's Patient**, the responder confirms the match and then appends its Patient ID to each submitted search string. 

#### Returning the executed search string in the response message

In the Specialty Rx Query Response, the responder returns the full search string that was ultimately executed--including patient parameter--in the .link element of each returned searchset Bundle.

<pre>
Bundle
    .link
        .relation = "self"
        .url = "MedicationRequest?patient=12345&status=active"
    .entry
</pre>
#### Patient references within search results

All searches performed within a request/response exchange flow relate to a single patient, who is represented in the *responder-patient* parameter in the Query Response message bundle. Patient references contained in resources in searchset bundles SHOULD be resolvable to the *responder-patient* entry in the outer, Query Response message bundle.

<br />

