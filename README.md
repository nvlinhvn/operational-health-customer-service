# operational-health-customer-service
One of the client is to ask for the analysis of operational health of the processing team in customer service department. 

# Data Overview
* CASE_ID: Unique ID for a new type of review/complaints that needs to be done and can comprise several PROCESSING_IDs. Think of it as one problem that might need several times coming back to.
* HANDLING_TIME_START: Time when an operational associate started reviewing the case.
* HANDLING_TIME_END: Time when our operational associate finished reviewing the case (does not mean the problem was solved but rather that the review at this time was completed).
* ACTOR_ID: ID of a specific operational associate.
* OPS_DECISION - Operational associate's decision after his/her review.
* CUSTOMER_ID - Unique ID of a customer who create a case/complaints.
* CASE_TYPE - Type of review that was done.
* FINAL_STATE - Final/latest decision that has been taken over the CASE_ID.
* PROCESSING_ID - Unique ID of a review taken of the CASE_ID. It should be the primary key of your data.
* CASE_CREATED_TIME - Time when CASE_ID was created and ended up in the operational batch, ready to be reviewed.

### Basic Data Quality
* No Null
* ~294k cases (70% type 3, 7% type 2)
* Processed by 342 actors
* Number unique case ID = Number unique processing Id --> There's no re-opened cases
* 1 row where HANDLING_TIME_START > HANDLING_TIME_END. We assume it's data quality issue and ignore this row.
* A case is closed if its final state is: `APPROVED`, `CLOSED`, `REJECTED`, or `NO_REVIEW_NEEDED`
* When actors' decision (OPS_DECISION) differs from the FINAL_STATE of a case, then there's a discrepancy or inconsistency 
