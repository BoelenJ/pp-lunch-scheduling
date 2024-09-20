# Lunch scheduling

This repository contains a Power Platform solution for scheduling lunches for your organization. The idea of these lunches is to provide your employees or team mates the opportunity to meet new people/managers. As this scheduling process can be cumbersome, this application will help to schedule these lunches in a smarter way. The solution consists of the following components:
- A model-driven app for the organizer: this application is used for creating the lunches and for getting insights into attendance and feedback of the participants.
- A "lunch copilot": this copilot will send the participants proactive communication about the lunch and also allows the organizer to quickly get insights into their lunch via chat.
- All automation is created using cloud flows, in which the Dataverse, Outlook, Teams, 365 Users and 365 groups connectors are used.

## Functionality

#### Creating lunches
The model-driven app allows the organizer to create a lunch. To create a lunch, you should provide the following information:
| Information | Description|
| ------ | ----- |
| Lunch name | Will be used in the invitations |
| Description | Will be used in the invitations |
| Organizer | Indicates who is the organizer and is needed for the copilot |
| How to create groups | This dictates how participants are added, can be based on a security/M365 group or based on direct reporting structure |
| Group name | Name of the security/M365 group, only applicable when you use groups for generating participants |
| Top-level manager | The top-level manager in case of direct reporting structure, the app will get all (in)direct reports as participants based on this manager |
| Timeslots | You need to provide multiple timeslots, with a capacity, date and location. These will be used for grouping participants. |

#### Creating groups
Using an AI prompt in AI Builder and the configuration you did in step one, the tool will automatically suggest diverse groups of participants and assign them to a timeslot. After creating groups, the organizer must evaluate the groups in the model driven app (and potentially modify if necessary). When you are satisfied with the groups, you can continue by going to the "Sending Invitations" step in the business process flow.

> Note: the AI prompt feature is not available in all regions yet, currently the solution was built in the US region. Make sure your region supports this feature when importing the solution.

#### Managing invitations
After the above steps, the app automates the entire process of managing invitations. This includes the following functionality:
- Initial invitations are sent to the participants via an adaptive card. This gives them the option to 1) accept, 2) decline, or 3) request a new timeslot. When accepting, an outlook calendar event will automatically be forwarded to the participant with their status as accepted. When requesting a new timeslot, the tool will try to find a suitable timeslot, if found, a new invitation is sent. If not found, this request ends up in a manual queue for the organizer.
- Reminders are sent to participants who did not RSVP yet and have a lunch coming up in the next X days. This is again an adaptive card.
- Similar reminders are sent to the managers of the participants, who will receive a list of team members who did not RSVP yet.
- A week before a lunch timeslot, all participants who accepted will receive a reminder.

#### Gathering feedback and evaluation
- After a timeslot passed, the participants will receive a feedback form via the lunch copilot. Their feedback is saved in Dataverse so that the organizer can evaluate the lunches.
- The organizer can use the model driven app to get insights into attendance and feedback.
- The organizer can also use the copilot to get information about their lunches, timeslots and attendance.

## How to install
The installation is relatively straightforward. This repository contains the managed and unmanaged solution zips, you can choose either one to install depending on your needs. To install the solution completely, follow the below steps:
1. Create a service account or shared mailbox that will be used for the communication with the participants.
2. In a Power Platform environment, import one of the solution.zip files by going to Solutions -> Import solution.
3. During the import, you need to create several connections. Create these connections using the service account.
4. You also need to provide some environment variable values, see the table below for more details on these:
 
| Environment variable | Description|
| ------ | ----- |
| Test mode | Setting this to true will send all notifications to a single email address (the address provided in test mode email). Useful for when you are still developing the solution. |
| Test mode email | The email that will receive the notifications during test mode. |
| Test mode participant id | An id of a participant that you want to mimic during test mode. |
| Outlook calendar ID | The Id of the calendar that needs to be used for scheduling, leave empty on import. After import, run the Calendar Get Calendar IDs cloud flow to get a list of valid calendars, use one of these ids as the value of this variable. |
| Reminder period | Dictates when an RSVP reminder is sent, configured in amount of days before start of an event. |

5. The solution also contains a bot, this bot needs to be published and shared in teams before you start working with the solution.
6. Now you can start organizing your lunches!
