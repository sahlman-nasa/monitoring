# Notification to operator: conflict notification from nonconforming flight external USS test scenario


## Description
This test aims at testing the strategic conflict detection requirements that relate to the scenario where a conflict is created 
between an existing planned flight and a nonconforming flight from another USS and the existing flight is notified of the conflict:
- **[astm.f3548.v21.OPIN0025](../../../../requirements/astm/f3548/v21.md)**
- **[astm.f3548.v21.SCD0035](../../../../requirements/astm/f3548/v21.md)**
- **[astm.f3548.v21.SCD0045](../../../../requirements/astm/f3548/v21.md)**
- **[astm.f3548.v21.SCD0095](../../../../requirements/astm/f3548/v21.md)**
- **[astm.f3548.v21.USS0005](../../../../requirements/astm/f3548/v21.md)**

It involves a tested USS and a control USS through which conflicting flights are injected.

It assumes that the area used in the scenario is already clear of any pre-existing flights (using, for instance, PrepareFlightPlanners scenario).

## Sequence

![Sequence diagram](assets/SCD95_external_with_nonconforming.png)

## Resources
### flight_intents
The FlightIntentsResource must provide the following flight intents:


<table>
  <tr>
    <th>Flight intent ID</th>
    <th>Flight name</th>
    <th>Priority</th>
    <th>State</th><!-- TODO: Update with usage_state and uas_state when new flight planning API is adopted -->
    <th>Must conflict with</th>
    <th>Must not conflict with</th>
  </tr>
  <tr>
    <td><code>flight1_planned</code></td>
    <td>Flight 1</td>
    <td rowspan="3">Any</td>
    <td>Nonconforming</td>
    <td>Flight 2</td>
    <td>N/A</td>
  </tr>
  <tr>
    <td><code>flight1c_planned</code></td>
    <td>Flight 1c</td>
    <td>Accepted</td>
    <td>N/A</td>
    <td>Flight 2</td>
  </tr>
  <tr>
    <td><code>equal_prio_flight2_planned</code></td>
    <td>Flight 2</td>
    <td>Activated</td>
    <td>Flight 1</td>
    <td>Flight 1c</td>
  </tr>
</table>

Because the scenario involves activation of intents and a nonconforming intent, all activated and nonconforming intents must be active during the execution of the
test scenario. Additionally, their end time must leave sufficient time for the execution of the test scenario. For the
sake of simplicity, it is recommended to set the start and end times of all the intents to the same range.

### tested_uss
FlightPlannerResource that will be used to inject control Flight 2. 

### control_uss
FlightPlannerResource that is under test and will manage conflicting Flight 1 and its variant. Note that this control USS needs to support the
CMSA role in order to transition a flight to the `Nonconforming` state.

### dss
DSSInstanceResource that provides access to a DSS instance where flight creation/sharing can be verified.


## Prerequisites check test case

### Verify area is clear test step

#### [Verify area is clear](../clear_area_validation.md)
While this scenario assumes that the area used is already clear of any pre-existing flights (using, for instance, PrepareFlightPlanners scenario) in order to avoid a large number of area-clearing operations, the scenario will not proceed correctly if the area was left in a dirty state following a previous scenario that was supposed to leave the area clear.  This test step verifies that the area is clear.


## Attempt to create new nonconforming flight into conflict test case
![Test case summary illustration](../nominal_planning/conflict_equal_priority_not_permitted/assets/attempt_to_plan_flight_into_conflict.svg)

### Plan Flight 2 test step

#### [Plan Flight 2](../../../flight_planning/plan_flight_intent.md)
Flight 2 should be successfully planned by the tested USS.

#### [Validate Flight 2 sharing](../validate_shared_operational_intent.md)

### Activate Flight 2 test step

#### [Activate Flight 2](../../../flight_planning/activate_flight_intent.md)
Flight 2 should be successfully activated by the tested USS.

#### [Validate Flight 2 sharing](../validate_shared_operational_intent.md)

### Create nonconforming Flight 1 test step
#### [Create nonconflorming Flight 1](test_steps/create_nonconforming_flight.md)
The test driver instructs the control USS to create Flight 1 as nonconforming. This makes nonconforming Flight 1 conflict with activated Flight 2.

#### [Validate Flight 1 sharing](../validate_shared_operational_intent.md)
Flight 1 should be successfully logged in the DSS and available to be shared with tested USS.

### Delete Flight 1 test step

#### [Delete Flight 1](../../../flight_planning/delete_flight_intent.md)
To prepare for the next test case, Flight 1 must be removed from the system.


## Attempt to modify planned flight into conflict test case
![Test case summary illustration](../nominal_planning/conflict_equal_priority_not_permitted/assets/attempt_to_modify_planned_flight_into_conflict.svg)

### Plan Flight 1c test step

#### [Plan Flight 1c](../../../flight_planning/plan_flight_intent.md)
The smaller Flight 1c form (which doesn't conflict with Flight 2) should be successfully planned by the control USS.

#### [Validate Flight 1c sharing](../validate_shared_operational_intent.md)

### Modify Flight 1c to Nonconforming state test step

#### [Modify Flight 1c to nonconforming](test_steps/modify_flight_to_nonconforming.md)
The test driver instructs the control USS to modify Flight 1c into a larger Nonconforming state. This makes nonconforming Flight 1 conflict with activated Flight 2.

#### [Validate Flight 1 sharing](../validate_shared_operational_intent.md)
Flight 1 should be successfully logged in the DSS and available to be shared with tested USS.

## Validate tested USS conflict notifications to user test case

### Validate tested USS conflict notifications to user test step

Wait a minimum of 12 seconds before continuing to give system time to process messages.

#### [Validate tested USS conflict notification to user for new flight](test_steps/validate_user_conflict_notification_from_other_flight.md)
The test driver checks conflict notification logs of tested USS to verify that notification was sent to Flight 2 due to conflict with Flight 1 from case "Attempt to create new nonconforming flight into conflict test case".

#### [Validate tested USS conflict notification to user for modified flight](test_steps/validate_user_conflict_notification_from_other_flight.md)
The test driver also checks conflict notification logs of tested USS to verify that notification was sent to Flight 2 due to conflict with Flight 1 from case "Attempt to modify planned flight into conflict test case".


## Cleanup test case

### Cleanup test step

#### [Clear flights from control USS and tested USS](test_steps/clear_flights.md)
The test driver clears flights from control USS and tested USS.