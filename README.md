### CERNER

#### Onboard
- **URL:** [test1internal.yosicare.com/onboard](https://test1internal.yosicare.com/onboard)
- **Features:**
  - Enable both:
    - Enhanced scheduling
    - Old scheduling
- No cron API like elation in Cerner.
- Appointment type should be added at the DB level.
  - One practice can have multiple locations.
  - Providers should also be added at the DB level.
- Similar to Athena, appointment types.
  - Widget will show options.

#### General Settings
- Cancel & reschedule enable/disable.
  - Reschedule slots with different time slots.
  - Cancel slots.
  - Cancel slots with different time slots.

#### Practice Settings
- Allow existing patient in Cerner enable/disable.

#### Email Notifications
- Once appointment booked, we will receive an email.

#### Override EMR
- How long we should option (Pending not yet complete).
- Disallow scheduling hours (we can hide the slots for separate mentioned hours).
- We can edit weekdays slots.

#### Cerner Power Chart
- **URL:** [cernabcn.cerenerworks.com/Citrix](https://cernabcn.cerenerworks.com/Citrix)
  - Can see the ppt details but will not show the provider.
  - In Scheduling booking menu, all the details about appointments with provider details.
  - Download the desktop app from the menu.
  - Sandbox details shared in sheet.

#### Powerchart Organiser
- Can search patient.
- Patient details will show on the screen.

#### Additional IDs
- emr_practice_id
- emr_source_id
- encounter_organization

#### From Slot API
- Combination: appt type + date time + location + provider + patient.
- Get slot details & manually add them to DB.

#### File & Function Details
- Using R4 millennium oracle.
- **API:** [fhir.cerner.com/millienium/r4/base/workflow/slot/#search](https://fhir.cerner.com/millenium/r4/base/workflow/slot/#search)
- File Path: `Application-api/schedule/module/selfschedule/src/v2/Model/CernerTable`
- Function Name: `cernerOpenslots`
  - Get config details from emr_db, cerner_client_details table.
  - Get emr_id & encounter_id details from emr_db, cener_practice-field_value table.
  - Generate token with formatted scope.
  - Pass scope with practice_id, consumer key, consumer secret, and scope to function: `getcerneraccesstoken` to get token.
  - After token generation, use practice id, endpoint URL, accessToken to function: `fetchSlotData`.
- All params have separate format, read format from docs.
- Per API call, Cerner will provide only 100 appointments, so loop the API in `fetchSlotData` for pagination type data fetching.

### Additional Functionality

#### Patient Search API
- **Endpoint:** url_end_point+family=&given=&gender
- Get patient details by calling this API.

#### Booking Appointment
- If first time user is N, book appointment using Appointment URL.

#### Check and Store Patient Details
- Check if the patient exists or not and store in the database.

#### Check and Store Appointment Details
- Check if the appointment exists or not and store in the database.

#### Get Invite Details
- Go to get invite details after booking the appointment.

#### Cron Job for Storing Insurance
- Cron job to store insurance details.

#### Update Reschedule or Cancel
- Update the rest in function: `cerner_update_data`.

#### File Path for Insurance Details
- File Path: `pushemr/yositocerner/src/v1/model/yosicernerTable`
- Fetching all insurance details of the patient.
- One appointment has only one encounter so create insurance function only.

#### Function for Adding Insurance Master List
- File Path: `applcation-api/registration/module/Practice/src/v1/module/Practice/src/v1/Model/PracticeTable.php`
- Function Name: `addInsuranceMasterList`
- Store insurance details in the database manually in insurance_master table.
