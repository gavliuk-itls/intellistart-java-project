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

1. All time values and diapasons mean the timezone obtained from application configuration (configured before application starts).
2. Any user can get current and previous week numbers (to use them when proceeding other operations)

### Interviewer

> The interviewer provides slots that are registered. It might be a fixed slot e.g 17:00,
> range 9:00-17:00, or both. In the case of range, it is essential that the interview must be finished 
> at the end of the range. The interview takes 1.5 hours.
>
> Interviewer slots cannot be changed and might be provided weekly or quarterly.
>
> In case of unexpected activities e.g ad-hoc meetings. The interviewer may contact the
> coordinator for slot changing or blocking.

Update:

* Interviewer have the limit of interviews (bookings) which can be conducted per week

1. Guest user can be authenticated by Facebook oauth2 and if the E-mail is granted by `INTERVIEWER` role, is identified as Interviewer.
1. Interviewer can create or update time slots for next week
    * can do it until end of Friday (00:00) of current week
    * time slot means day of week + time diapason (e.g. "Wed 17:30-20:00"), boundaries are rounded to 30 minutes, duration must be greather or equal 1.5 hours, start time cannot be less than 8:00, end time cannot be greater than 22:00
1. Interviewer can see own current week time slots
    * each slot contains the list of bookings in this slot, if any
1. Interviewer can see own next week time slots
    * each slot contains the list of bookings in this slot, if any
1. Interviewer can set the maximum number of bookings for next week
    * maximum bookings means system will not allow to create more than that number of bookings (even if there are some free time slots)
    * if maximum number of bookings is not set for certain week, the previous week limit is actual (or previous of previous, so on)
        * if limit was never set, any number of bookings can be assigned to interviewer

### Candidate

> The candidate provides his availability slots. In case of a full match, the interviewer 
> slot is marked as booked. Otherwise, the new interviewer slots are provided and 
> those slots are marked as pre-booked for a while. (Usually 2 hours). 
> The candidate may accept or refuse the new slots. And, a slot is marked as booked or 
> becomes free again, respectively.

Update:

* We have decided do not to apply any booking automatically after sending slots; all bookings are created by Coordinator

Cases:

1. Guest user can be authenticated by Facebook oauth2 and if the E-mail is not granted any special role, is identified as Candidate.
1. Candidate can create one or more availability Slots (`POST /candidates/current/slots`):
    * Slot must be in future
    * Slot has to be 1.5 hours or more and rounded to 30 minutes
    * Slot is defined as exact date and time diapason (not like Interviewer slot, where day of week and week number are used)
1. Candidate can update own slots only if there is no bookings (`POST /candidates/current/slots/{slotId}`)
1. Candidate can see all own slots: `GET /candidates/current/slots`
    * each slot contains the list of bookings in this slot, if any


## Coordinator

> When he receives interviewer slots they should be registered, in case of slot changing
> they should be updated.
> When the candidate slots are received, search for full matching, otherwise provides the
> alternative slots.

1. Coordinator can see all the candidates and interviewers slots by `GET /weeks/{weekId}/dashboard`
    * groupped by days, each day:
        * contains all interviewers slots with booking's IDs inside 
        * contains all candidates slots with booking's IDs inside
        * contains map of bookings as map bookingId => bookingData
1. Coordinator can update any Interviewer's time slot (next week or current week): `POST /interviewers/{interviewerId}/slots/{slotId}`
1. Coordinator can create booking: `POST /bookings`, providing:
    * interviewer slot ID
    * candidate slot ID
    * start and end time (must be 1.5 hours inside both slots)
    * subject (0-255 chars) and description (up to 4000 chars)
1. Coordinator can update any booking: `POST /bookings/{bookingId}`
1. Coordinator can delete booking: `DELETE /bookings/{bookingId}`
1. Coordinator can grant the Interviewer role: `POST /users/interviewers` providing the E-mail of user
1. Coordinator can list the granted Interviewers: `GET /users/interviewers`
1. Coordinator can revoke the Interviewer role: `DELETE /users/interviewers/<interviewer-id>`
1. Coordinator can grant the Coordinator role: `POST /users/coordinators` providing the E-mail of user
1. Coordinator can list the granted Coordinators: `GET /users/coordinators`
1. Coordinator can revoke the Coordinator role: `DELETE /users/coordinators/<coordinator-id>`
    * Corrdinator cannot revoke himself
1. The first coordinarot must be created automatically in DB on application deployment


### Interviewer slot

> These kinds of slots may have the following statuses;
> New, Changed, Pre-booked, Booked, Deleted.

Update: 

* Slots are not exactly match Bookings, so these statuses are not rerquired; both slots and booking might be there or just deleted; but every booking must be linked to two slots (Interviewer's and Candidate's)

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

### Me endpoint

```json
GET /me

Response:
{
    "email": "your@email.com",
    "role": "INTERVIEWER",
    "id": "some-UUID-only-for-COORDINATOR-or-INTERVIEWER"
}
```

For the Guest, response should be same as for any other endpoint, not allowed for Guests:
```json
HTTP 403
{
    "errorCode": "not_authorized",
    "errorMessage": "You are not authorized to use this functionality"
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
    "weekNum": 12,
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
