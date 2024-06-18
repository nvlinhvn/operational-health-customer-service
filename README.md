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

# Operational Health of Processing Team

![operational health](https://raw.githubusercontent.com/nvlinhvn/operational-health-customer-service/tree/main/img/Daily_KPI.png)

We can observe:
* Efficiency in case management (low backlog rate & efficient case closure)
* Generally responsive in addressing new cases (consistent first touch within 24h)
* Process case in a timely manner (handling time ~6 min). There're some spikes in mean handling time (e.g. 05 March) -> Probably some complicated cases causes outliers on that day
* Still some room for improvement in operators' decision-making where lastest `OPS_DECISION` differs from FINAL_STATE for a case

# Case Distribution

##### Based on Duration
![case distribution](https://raw.githubusercontent.com/nvlinhvn/operational-health-customer-service/tree/main/img/Case_Distribution_by_time.png)

##### Based on Actors Working Hour
![service 24/7](https://raw.githubusercontent.com/nvlinhvn/operational-health-customer-service/tree/main/img/Handle_Case_Heatmap.png)

We can see from the end-customer perspective:
* Instant: Responsive to customer inquiries or issues, and ~70% case closed within 24h created (Instant)
* Convenience: Actors' Service 24/7. Customers can get convenient support whenever needed
* Transparency: Miss some information about transparency from customer perspective (e.g. email/communicate customer with regular updates on the case status,and setting expectations for resolution timeframe)
* Low Cost: Team's efficiency in managing cases (stable duration, low backlog) indirectly contribute to cost optimization and low-cost services for customers

# Actor Workload, Performance and Efficiency

![actor general behavior](https://raw.githubusercontent.com/nvlinhvn/operational-health-customer-service/tree/main/img/General_Actor_Behavior.png)

##### Top 30 Actors by processed cases

![top 30](https://raw.githubusercontent.com/nvlinhvn/operational-health-customer-service/tree/main/img/Workload_top_30.png)

##### Bottom 30 Actors by processed cases

![bottom 30](https://raw.githubusercontent.com/nvlinhvn/operational-health-customer-service/tree/main/img/Workload_bottom_30.png)

We can see some improvement needed:
* Non-uniform Workdload Distribution (~50% cases processed by 10% actors)
* Potential mismatch between agent skills and case complexity
* Gaps in seniority/experience among agents

# Recommendation on Product Changes
A product team has a roll-out plan for a new product that would, as per estimations, increase the workload for the type-3 case 5 times when released to its 100% potential. If we wanna maintain the current workload in Jan-2022 for type-3 cases:

* Type-3 cases per customer ~= 1
* Avg Handling Minutes = ~6
* Avg 1st Touch Hours = ~55
* Total type-3 cases = 196k
* Actors = 130
* Productivity: 196k / 130 = 228 processed type-3 cases per actor

To keep productivity (cases per actor) with 5-times increase workload (196k * 5 = 981k), we'll need total ~4.3k actors (assume no actors leave, so we'll need new 3960 hires)

#### Hiring and Training Plan for New Resources within 6-month Linear Rampup

Assuming:
* A new hire would take 4 months before being fully onboarded from the time of the hire.
* Hiring, on average, takes 1 month.

The plan can be reviewed below:

![Hiring and Training Plan](https://raw.githubusercontent.com/nvlinhvn/operational-health-customer-service/tree/main/img/Plan_to_hire_and_training.png)

* Would take ~10 months for fully training new 3960 hires (660 hires/batch)
* If max capacity = 150 hires/batch -> would take 2 years
* It's noted we shouldn't only consider quantity, but also to improve agents' skills and experiences in decision making

Details need to discuss and negotiate with other teams, especially product:
* Detailed timeline for expected rollout of the new product
* Projected adoption rate and user growth projections over time
* Challenge product team about the workload estimation and assumption
* Rampup phase is linear or different based on seasonality?
* Maximum capacity to hire per batch

Potential Risks:
* Unforseen technical issues/bugs/compatibility with new product -> more cases
* User behavior changes with new product (churn ...)
* Hiring/training risks: unattractive package, limit budget ...
* External: regulation (GDPR), market, competition ...

Further suggestion in the next steps:
* Enhance actor training/onboarding program
* Predict case complexity and optimize case assignment based on agent skills and performance with Machine Learning or advanced optimization techniques
* Collect qualitative customer feedback for comprehensive view of customer experience
