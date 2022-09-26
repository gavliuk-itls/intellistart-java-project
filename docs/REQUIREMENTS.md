# Functional requirements

## Actors:

*Guest*
Any user without authentication. All the actors start using the application as Guests.

*Interviewer*. 
He/she conducts an interview and provides time slots beforehand. All these
slots are fixed.

*Candidate*. 
He/she is interviewed and also provides his availability slots. These slots
might be changed.

*Coordinator*. 
He/she registers interviewer slots and searches for matching as soon as
candidate slots are received. if there is no matching, provide available Interviewer slots
(pre-booking).

## Use-cases

### Common

All time values and diapasons mean the timezone obtained from application configuration (configured before application starts).

### Interviewer

> The interviewer provides slots that are registered. It might be a fixed slot e.g 17:00,
> range 9:00-17:00, or both. In the case of range, it is essential that the interview must be finished 
> at the end of the range. The interview takes 1.5 hours.
>
> Interviewer slots cannot be changed and might be provided weekly or quarterly.
>
> In case of unexpected activities e.g ad-hoc meetings. The interviewer may contact the
> coordinator for slot changing or blocking.

1. Guest user can be authenticated by Facebook oauth2 and if the E-mail is granted by `INTERVIEWER` role, is identified as Interviewer.
1. Interviewer can create or update time slots for next week
    * can do it until end of Friday (00:00) of current week
    * time slot means day of week + time diapason (e.g. "Wed 17:30-20:00"), boundaries are rounded to 30 minutes, duration must be greather or equal 1.5 hours, start time cannot be less than 8:00, end time cannot be greater than 22:00
1. Interviewer can see own current week time slots
1. Interviewer can see own next week time slots

### Candidate

> The candidate provides his availability slots. In case of a full match, the interviewer 
> slot is marked as booked. Otherwise, the new interviewer slots are provided and 
> those slots are marked as pre-booked for a while. (Usually 2 hours). 
> The candidate may accept or refuse the new slots. And, a slot is marked as booked or 
> becomes free again, respectively.

1. Guest user can be authenticated by Facebook oauth2 and if the E-mail is not granted any special role, is identified as Candidate.
1. Candidate can create one or more availability Slots:
    * Slot must be in future
    * Slot has to be 1.5 hours or more and rounded to 30 minutes
1. Candidate 


## Coordinator

> When he receives interviewer slots they should be registered, in case of slot changing
> they should be updated.
> When the candidate slots are received, search for full matching, otherwise provides the
> alternative slots.

1. Coordinator can update any Interviewer's time slot (next week or current week)
    * all Bookings into these slots 

### Interviewer slot

> These kinds of slots may have the following statuses;
> New, Changed, Pre-booked, Booked, Deleted.

Slots are not exactly match Bookings. 

Example 1: slot 9:00-17:00 is 8 hours, which cannot be devided 
to 1.5 hours (8/1.5 = 5.3333 bookings?)
So, the Slot is a time period available for booking. In this example we would have bookings:
* 10:00-11:30: candidate A (so the 9:00-10:00 left "unbookable")
* 11:30-13:00: candidate B
* 14:00-15:30: candidate C (so 13:00-14:00 left "unbookable")
* 15:30-17:00: candiudate D

So, these statuses are for *Bookings*, which have the links to both Intervier and Candidate *Slots*.

### API specification

As the project is designated to be used as backend of a frontend teams, the strict API specification is key requirement.

Keep all the request and response data structures and error codes.

### Error responses

```json
HTTP 400
{
    "errorCode": "snake_case_meaningful_string",
    "errorMessage": "English user friendly message"
}
```

All business validation errors must have HTTP code `400`.

`GET` request with wrong ID must return HTTP code `404`.

Bad or expired authentication token leads to HTTP code `401`.

Trying proceed unauthorized operation (like Candidate changing Interviewer slot) will lead to HTTP code `403`.

### Dictionaries

```json
GET /weeks/current

Response:
{
    "weekNum": 11
}
```

```json
GET /weeks/next

Response:
{
    "weekNum": 12
}
```


### Create interviewer time slot

```json
POST /interviewers/<interviewerId>/slots

{
    "weekNum": 12,
    "dayOfWeek": "Fri",
    "from": "9:00",
    "to": "17:00"
}

Response:

{
    "id": "<slotId>",
    "dayOfWeek": "Fri",
    "from": "9:00",
    "to": "17:00"
}
```

Errors:

* `interviewer_not_found`
* `slot_is_overlaping`: is existing slot is already there
* `invalid_boundaries`: e.g. if "9:02" or the start is after the end or violates working time (8:00-22:00)
* `invalid_day_of_week`: e.g. "sundday"


### Update interviewer time slot

```json
POST /interviewers/<interviewerId>/slots/<slotId>

{
    "weekNum": 12,
    "dayOfWeek": "Fri",
    "from": "10:00",
    "to": "17:00"
}

Response:

{
    "id": "<slotId>",
    "weekNum": 12,
    "dayOfWeek": "Fri",
    "from": "9:00",
    "to": "17:00"
}
```

Errors:

* `interviewer_not_found`
* `slot_not_found`
* `slot_is_overlaping`: is existing slot is already there
* `invalid_boundaries`: e.g. if "9:02" or the start is after the end or violates working time (8:00-22:00)
* `invalid_day_of_week`: e.g. "sundday"
* `cannot_edit_this_week`: if Interviewer trying to change current (or previous) week slot, however Coordinator is allowed to do this

---------------------

> NOTE: 
> 
> As this is a test project, for now only above API have to be 
> implemented strictly by specification.
> 
> Try to keep all the rest API endpoints using same 
> style and practices