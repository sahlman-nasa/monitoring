# ASTM NetRID DSS: Subscription Simple test scenario

## Overview

Perform basic operations on a single DSS instance to create, update and delete subscriptions.

## Resources

### dss

[`DSSInstanceResource`](../../../../../resources/astm/f3411/dss.py) to be tested in this scenario.

### id_generator

[`IDGeneratorResource`](../../../../../resources/interuss/id_generator.py) providing the Subscription IDs for this scenario.

### isa

[`ServiceAreaResource`](../../../../../resources/netrid/service_area.py) describing a service area for which to subscribe.

### problematically_big_area

[`VerticesResource`](../../../../../resources/vertices.py) describing an area designed to be too big to be accepted by the DSS.

## Setup test case

### Ensure clean workspace test step

This step ensures that no subscription with the known test ID exists in the DSS.

#### Search for all subscriptions in ISA area check

If the DSS fails to let us search in the area for which test subscriptions will be created, it is failing to properly implement **[astm.f3411.v19.DSS0030,f](../../../../../requirements/astm/f3411/v19.md)**.

#### Subscription can be deleted check

An attempt to delete a subscription when the correct version is provided should succeed, otherwise the DSS is in violation of **[astm.f3411.v19.DSS0030,d](../../../../../requirements/astm/f3411/v19.md)**.

#### Ensure subscription with test ID does not exist check

If the DSS cannot be queried for the existing test ID, or if a subscription with that ID exists and it cannot be removed,
the DSS is likely not implementing **[astm.f3411.v19.DSS0030,e](../../../../../requirements/astm/f3411/v19.md)** or **[astm.f3411.v19.DSS0030,d](../../../../../requirements/astm/f3411/v19.md)** properly.

## Subscription Simple test case

This test case creates multiple subscriptions, goes on to query and search for them, then deletes and searches for them again.

### Create subscription validation test step

This test step creates multiple subscriptions with different combinations of the optional end and start time parameters.

All subscriptions are left on the DSS when this step ends, as they are expected to be present for the subsequent step.

#### Create subscription check

As per **[astm.f3411.v19.DSS0030,c](../../../../../requirements/astm/f3411/v19.md)**, the DSS API must allow callers to create a subscription with either onr or both of the
start and end time missing, provided all the required parameters are valid.

#### Response to subscription creation contains a subscription check

As per **[astm.f3411.v19.DSS0030,c](../../../../../requirements/astm/f3411/v19.md)**, upon creation of a subscription,
the newly created subscription must be part of its response.

#### Returned subscription has an ID check

If the returned subscription has no ID, **[astm.f3411.v19.DSS0030,c](../../../../../requirements/astm/f3411/v19.md)** is not respected.

#### Returned subscription ID is correct check

If the returned subscription ID does not correspond to the one specified in the creation parameters,
**[astm.f3411.v19.DSS0030,c](../../../../../requirements/astm/f3411/v19.md)** is not respected.

#### Returned subscription has an owner check

If the returned subscription has no owner set, **[astm.f3411.v19.DSS0030,c](../../../../../requirements/astm/f3411/v19.md)** is not respected.

#### Returned subscription owner is correct check

If the returned subscription's owner does not correspond to the uss_qualifier, **[astm.f3411.v19.DSS0030,c](../../../../../requirements/astm/f3411/v19.md)** is not respected.


#### Returned notification index is 0 if present check

The notification index of a newly created subscription must be 0, otherwise the DSS is in violation of **[astm.f3411.v19.DSS0030,c](../../../../../requirements/astm/f3411/v19.md)**.

#### Returned subscription has an ISA URL check

If the returned subscription has no ISA URL defined, **[astm.f3411.v19.DSS0030,c](../../../../../requirements/astm/f3411/v19.md)** is not respected.

#### Returned ISA URL has correct base URL check

The returned ISA URL must be prefixed with the USS base URL that was provided at subscription creation, otherwise the DSS is in violation of **[astm.f3411.v19.DSS0030,c](../../../../../requirements/astm/f3411/v19.md)**.

#### Returned subscription has a start time check

If the returned subscription has no start time defined, **[astm.f3411.v19.DSS0030,c](../../../../../requirements/astm/f3411/v19.md)** is not respected.

#### Returned start time is correct check

The returned start time must be the same as the provided one, otherwise the DSS is in violation of **[astm.f3411.v19.DSS0030,c](../../../../../requirements/astm/f3411/v19.md)**.

#### Returned subscription has an end time check

Subscriptions need a defined end time in order to limit their duration: if the DSS omits to set the end time, it will be in violation of **[astm.f3411.v19.DSS0060](../../../../../requirements/astm/f3411/v19.md)**.

#### Returned end time is correct check

The returned end time must be the same as the provided one, otherwise the DSS is in violation of **[astm.f3411.v19.DSS0030,c](../../../../../requirements/astm/f3411/v19.md)**.

#### Returned subscription has a version check

If the returned subscription has no version defined, **[astm.f3411.v19.DSS0030,c](../../../../../requirements/astm/f3411/v19.md)** is not respected.

#### Generated subscription version has proper format check

The subscription version generated by the DSS must be a lower-case alphanumeric string of 10 characters or more, otherwise the DSS is in violation of **[astm.f3411.v19.DSS0030,c](../../../../../requirements/astm/f3411/v19.md)**.

### Query Existing Subscription test step

Query and search for the created subscription in various ways

#### Get Subscription by ID check

If the freshly created subscription cannot be queried using its ID, the DSS is failing to meet **[astm.f3411.v19.DSS0030,e](../../../../../requirements/astm/f3411/v19.md)**.

#### Search for all subscriptions in ISA area check

If the DSS fails to let us search in the area for which the subscription was just created, it is failing to meet **[astm.f3411.v19.DSS0030,f](../../../../../requirements/astm/f3411/v19.md)**.

#### Created Subscription is in search results check

If the created subscription is not returned in a search that covers the area it was created for, the DSS is not properly implementing **[astm.f3411.v19.DSS0030,f](../../../../../requirements/astm/f3411/v19.md)**.

#### No huge search area allowed check

In accordance with **[astm.f3411.v19.DSS0030,f](../../../../../requirements/astm/f3411/v19.md)**, the DSS should not allow searches for areas that are too big.

#### Returned subscription ID is correct check

If the returned subscription ID does not correspond to the one specified in the creation parameters,
**[astm.f3411.v19.DSS0030,c](../../../../../requirements/astm/f3411/v19.md)** is not respected.

#### Returned subscription has an owner check

If the returned subscription has no owner set, **[astm.f3411.v19.DSS0030,c](../../../../../requirements/astm/f3411/v19.md)** might not be respected.

#### Returned subscription owner is correct check

If the returned subscription's owner does not correspond to the uss_qualifier, **[astm.f3411.v19.DSS0030,c](../../../../../requirements/astm/f3411/v19.md)** might not be respected.



#### Returned notification index is equal to or greater than 0 check

If the notification index of the subscription is less than 0, the DSS fails to properly implement **[astm.f3411.v19.DSS0030,e](../../../../../requirements/astm/f3411/v19.md)**.

#### Returned subscription has an ID check

If the returned subscription has no ID, **[astm.f3411.v19.DSS0030,c](../../../../../requirements/astm/f3411/v19.md)** is not respected.

#### Returned subscription ID is correct check

If the returned subscription ID does not correspond to the one specified in the creation parameters,
**[astm.f3411.v19.DSS0030,c](../../../../../requirements/astm/f3411/v19.md)** is not respected.

#### Returned subscription has an ISA URL check

If the returned subscription has no ISA URL defined, **[astm.f3411.v19.DSS0030,c](../../../../../requirements/astm/f3411/v19.md)** is not respected.

#### Returned ISA URL has correct base URL check

The returned ISA URL must be prefixed with the USS base URL that was provided at subscription creation, otherwise the DSS is in violation of **[astm.f3411.v19.DSS0030,c](../../../../../requirements/astm/f3411/v19.md)**.

#### Returned subscription has a start time check

If the returned subscription has no start time defined, **[astm.f3411.v19.DSS0030,c](../../../../../requirements/astm/f3411/v19.md)** is not respected.

#### Returned start time is correct check

The returned start time must be the same as the provided one, otherwise the DSS is in violation of **[astm.f3411.v19.DSS0030,c](../../../../../requirements/astm/f3411/v19.md)**.

#### Returned subscription has an end time check

Subscriptions need a defined end time in order to limit their duration: if the DSS omits to set the end time, it will be in violation of **[astm.f3411.v19.DSS0060](../../../../../requirements/astm/f3411/v19.md)**.

#### Returned ISA URL has correct base URL check

The returned ISA URL must be prefixed with the USS base URL that was provided at subscription creation, otherwise the DSS is in violation of **[astm.f3411.v19.DSS0030,c](../../../../../requirements/astm/f3411/v19.md)**.

#### Returned start time is correct check

The returned start time must be the same as the provided one, otherwise the DSS is in violation of **[astm.f3411.v19.DSS0030,c](../../../../../requirements/astm/f3411/v19.md)**.

#### Returned end time is correct check

The returned end time must be the same as the provided one, otherwise the DSS is in violation of **[astm.f3411.v19.DSS0030,c](../../../../../requirements/astm/f3411/v19.md)**.

#### Returned subscription has a version check

If the returned subscription has no version defined, **[astm.f3411.v19.DSS0030,c](../../../../../requirements/astm/f3411/v19.md)** is not respected.

#### Non-mutated subscription keeps the same version check

If the version of the subscription is updated without there having been any mutation of the subscription, the DSS is in violation of **[astm.f3411.v19.DSS0030,c](../../../../../requirements/astm/f3411/v19.md)**.

#### Generated subscription version has proper format check

The subscription version generated by the DSS must be a lower-case alphanumeric string of 10 characters or more, otherwise the DSS is in violation of **[astm.f3411.v19.DSS0030,c](../../../../../requirements/astm/f3411/v19.md)**.

### Mutate Subscription test step

This test step mutates the previously created subscription to verify that the DSS reacts properly: notably, it checks that the subscription version is updated,
including for changes that are not directly visible, such as changing the subscription's footprint.

#### Subscription can be mutated check

If a subscription cannot be modified with a valid set of parameters, the DSS is failing to meet **[astm.f3411.v19.DSS0030,c](../../../../../requirements/astm/f3411/v19.md)**.

#### Response to subscription mutation contains a subscription check

As per **[astm.f3411.v19.DSS0030,c](../../../../../requirements/astm/f3411/v19.md)**, upon creation of a subscription,
the newly created subscription must be part of its response.

#### Returned subscription ID is correct check

If the returned subscription ID does not correspond to the one specified in the creation parameters,
**[astm.f3411.v19.DSS0030,c](../../../../../requirements/astm/f3411/v19.md)** is not respected.

#### Returned subscription has an owner check

If the returned subscription has no owner set, **[astm.f3411.v19.DSS0030,c](../../../../../requirements/astm/f3411/v19.md)** might not be respected.

#### Returned subscription owner is correct check

If the returned subscription's owner does not correspond to the uss_qualifier, **[astm.f3411.v19.DSS0030,c](../../../../../requirements/astm/f3411/v19.md)** might not be respected.



#### Returned notification index is equal to or greater than 0 check

If the notification index of the subscription is less than 0, the DSS fails to properly implement **[astm.f3411.v19.DSS0030,e](../../../../../requirements/astm/f3411/v19.md)**.

#### Returned subscription has an ID check

If the returned subscription has no ID, **[astm.f3411.v19.DSS0030,c](../../../../../requirements/astm/f3411/v19.md)** is not respected.

#### Returned subscription ID is correct check

If the returned subscription ID does not correspond to the one specified in the creation parameters,
**[astm.f3411.v19.DSS0030,c](../../../../../requirements/astm/f3411/v19.md)** is not respected.

#### Returned subscription has an ISA URL check

If the returned subscription has no ISA URL defined, **[astm.f3411.v19.DSS0030,c](../../../../../requirements/astm/f3411/v19.md)** is not respected.

#### Returned ISA URL has correct base URL check

The returned ISA URL must be prefixed with the USS base URL that was provided at subscription creation, otherwise the DSS is in violation of **[astm.f3411.v19.DSS0030,c](../../../../../requirements/astm/f3411/v19.md)**.

#### Returned subscription has a start time check

If the returned subscription has no start time defined, **[astm.f3411.v19.DSS0030,c](../../../../../requirements/astm/f3411/v19.md)** is not respected.

#### Returned start time is correct check

The returned start time must be the same as the provided one, otherwise the DSS is in violation of **[astm.f3411.v19.DSS0030,c](../../../../../requirements/astm/f3411/v19.md)**.

#### Returned subscription has an end time check

Subscriptions need a defined end time in order to limit their duration: if the DSS omits to set the end time, it will be in violation of **[astm.f3411.v19.DSS0060](../../../../../requirements/astm/f3411/v19.md)**.

#### Returned ISA URL has correct base URL check

The returned ISA URL must be prefixed with the USS base URL that was provided at subscription creation, otherwise the DSS is in violation of **[astm.f3411.v19.DSS0030,c](../../../../../requirements/astm/f3411/v19.md)**.

#### Returned start time is correct check

The returned start time must be the same as the provided one, otherwise the DSS is in violation of **[astm.f3411.v19.DSS0030,c](../../../../../requirements/astm/f3411/v19.md)**.

#### Returned end time is correct check

The returned end time must be the same as the provided one, otherwise the DSS is in violation of **[astm.f3411.v19.DSS0030,c](../../../../../requirements/astm/f3411/v19.md)**.

#### Returned subscription has a version check

If the returned subscription has no version defined, **[astm.f3411.v19.DSS0030,c](../../../../../requirements/astm/f3411/v19.md)** is not respected.

#### Mutated subscription version is updated check

Following a mutation, the DSS needs to update the subscription version, otherwise it is in violation of **[astm.f3411.v19.DSS0030,c](../../../../../requirements/astm/f3411/v19.md)**.

#### Generated subscription version has proper format check

The subscription version generated by the DSS must be a lower-case alphanumeric string of 10 characters or more, otherwise the DSS is in violation of **[astm.f3411.v19.DSS0030,c](../../../../../requirements/astm/f3411/v19.md)**.

### Delete Subscription test step

Attempt to delete the subscription in various ways and ensure that the DSS reacts properly.

This also checks that the subscription data returned by a successful deletion is correct.

#### Missing version prevents deletion check

An attempt to delete a subscription without providing a version should fail, otherwise the DSS is in violation of **[astm.f3411.v19.DSS0030,d](../../../../../requirements/astm/f3411/v19.md)**.

#### Incorrect version prevents deletion check

An attempt to delete a subscription while providing an incorrect version should fail, otherwise the DSS is in violation of **[astm.f3411.v19.DSS0030,d](../../../../../requirements/astm/f3411/v19.md)**.

#### Subscription can be deleted check

An attempt to delete a subscription when the correct version is provided should succeed, otherwise the DSS is in violation of **[astm.f3411.v19.DSS0030,d](../../../../../requirements/astm/f3411/v19.md)**.



#### Returned notification index is equal to or greater than 0 check

If the notification index of the subscription is less than 0, the DSS fails to properly implement **[astm.f3411.v19.DSS0030,e](../../../../../requirements/astm/f3411/v19.md)**.

#### Returned subscription has an ID check

If the returned subscription has no ID, **[astm.f3411.v19.DSS0030,c](../../../../../requirements/astm/f3411/v19.md)** is not respected.

#### Returned subscription ID is correct check

If the returned subscription ID does not correspond to the one specified in the creation parameters,
**[astm.f3411.v19.DSS0030,c](../../../../../requirements/astm/f3411/v19.md)** is not respected.

#### Returned subscription has an owner check

If the returned subscription has no owner set, **[astm.f3411.v19.DSS0030,c](../../../../../requirements/astm/f3411/v19.md)** might not be respected.

#### Returned subscription owner is correct check

If the returned subscription's owner does not correspond to the uss_qualifier, **[astm.f3411.v19.DSS0030,c](../../../../../requirements/astm/f3411/v19.md)** might not be respected.


#### Returned subscription has an ISA URL check

If the returned subscription has no ISA URL defined, **[astm.f3411.v19.DSS0030,c](../../../../../requirements/astm/f3411/v19.md)** is not respected.

#### Returned ISA URL has correct base URL check

The returned ISA URL must be prefixed with the USS base URL that was provided at subscription creation, otherwise the DSS is in violation of **[astm.f3411.v19.DSS0030,c](../../../../../requirements/astm/f3411/v19.md)**.

#### Returned subscription has a start time check

If the returned subscription has no start time defined, **[astm.f3411.v19.DSS0030,c](../../../../../requirements/astm/f3411/v19.md)** is not respected.

#### Returned start time is correct check

The returned start time must be the same as the provided one, otherwise the DSS is in violation of **[astm.f3411.v19.DSS0030,c](../../../../../requirements/astm/f3411/v19.md)**.

#### Returned subscription has an end time check

Subscriptions need a defined end time in order to limit their duration: if the DSS omits to set the end time, it will be in violation of **[astm.f3411.v19.DSS0060](../../../../../requirements/astm/f3411/v19.md)**.

#### Returned end time is correct check

The returned end time must be the same as the provided one, otherwise the DSS is in violation of **[astm.f3411.v19.DSS0030,c](../../../../../requirements/astm/f3411/v19.md)**.

#### Returned subscription has a version check

If the returned subscription has no version defined, **[astm.f3411.v19.DSS0030,c](../../../../../requirements/astm/f3411/v19.md)** is not respected.

#### Non-mutated subscription keeps the same version check

If the version of the subscription is updated without there having been any mutation of the subscription, the DSS is in violation of **[astm.f3411.v19.DSS0030,c](../../../../../requirements/astm/f3411/v19.md)**.

#### Generated subscription version has proper format check

The subscription version generated by the DSS must be a lower-case alphanumeric string of 10 characters or more, otherwise the DSS is in violation of **[astm.f3411.v19.DSS0030,c](../../../../../requirements/astm/f3411/v19.md)**.

### Query Deleted Subscription test step

Attempt to query and search for the deleted subscription in various ways

#### Query by subscription ID should fail check

If the DSS provides a successful reply to a direct query for the deleted subscription, it is in violation of **[astm.f3411.v19.DSS0030,e](../../../../../requirements/astm/f3411/v19.md)**.

#### Search for all subscriptions in ISA area check

If the DSS fails to let us search in the area for which the subscription was just created, it is failing to meet **[astm.f3411.v19.DSS0030,f](../../../../../requirements/astm/f3411/v19.md)**.

#### Search area that represents a loop is not allowed check

The DSS should not allow us to search for subscriptions using a list of vertices describing a loop (first and last points in the list of vertices are the same),
otherwise it is failing to meet **[astm.f3411.v19.DSS0030,f](../../../../../requirements/astm/f3411/v19.md)**.

#### Deleted subscription should not be present in search results check

If the DSS returns the deleted subscription in a search that covers the area it was originally created for, the DSS is not properly implementing **[astm.f3411.v19.DSS0030,f](../../../../../requirements/astm/f3411/v19.md)**.

## Cleanup

The cleanup phase of this test scenario removes the subscription with the known test ID if it has not been removed before.

#### Ensure subscription with test ID does not exist check

If the DSS cannot be queried for the existing test ID, or if a subscription with that ID exists and it cannot be removed,
the DSS is likely not implementing **[astm.f3411.v19.DSS0030,e](../../../../../requirements/astm/f3411/v19.md)** or **[astm.f3411.v19.DSS0030,d](../../../../../requirements/astm/f3411/v19.md)** properly.