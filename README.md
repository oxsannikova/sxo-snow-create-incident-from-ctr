# SecureX orchestrtration workflow: Create ServiceNow incident from SecureX threat response insident

This SecureX orchestration workflow looks for SecureX Threat Response new incidents and then creates a ServiceNow incident for any found.

## Required Targets
- CTR_For_Access_Token (default)
- CTR_API (default)
- ServiceNow
- Webex Teams

## Required Account Keys
- CTR_Credentials
- ServiceNow
- Webex Teams Token

## Required Global Variables
- Last Run Timestamp

## Required Atomic Workflows
- Service Now - Create Incident
- CTRGenerateAccessToken
- Webex Teams - Post Message to Room

## Setup instructions

### Step 1. Configure ServiceNow Target

> Note: We have used free [developer ServiceNow instance](https://developer.servicenow.com/dev.do) to test this orchestration workflow.

In SecureX Orchestration, go to **Targets** in the left hand side menu, select **New Target**, and apply configuration according to the screenshot below.

> Note: CTR_API and CTR_For_Access_Token are the stardard targets that are pre-configured in SecureX orchestration out of the box.

### Step 2. Set up Webex Teams Target

In order to send notifications via Webex Teams, the target needs to be configured to reach it's API. In SecureX Orchestration, go to Targets in the left hand side menu, select New Target, and apply configuration according to the screenshot below.

> **Make sure the display name of the target matches exactly (Webex Teams).**

![](/assets/webex_teams_target.png)

### Step 3. Create Webex Teams Room for notifications

> It is assumed that Webex Teams Room for notifications will be created in advance (see prerequisites section).

Create Teams Room and add all necessary people. SXO will need to know the Room ID in order to send notifications. The easiest way to find it out - add `roomid@webex.bot` to your room and it will reply to you with the private message containing the room id.

RoomID bot is developed by Cisco to assist Developers with obtaining Room ID information.

1. Add Bot to your room:

![](/assets/add_roomid_bot.png)

2. The bot will send you the Room ID in a private message:

![](/assets/room_id.png)

In SecureX orchestration, choose in the menu on the left Variables -> Global Variables -> New Variable.

![](/assets/variables.png)

Create new variable of string type with a unique display name (e.g. "Santa Tracking Space"), paste Room ID as a value and click Submit.

![](/assets/new_variable.png)

### Step 4. Obtain Webex Teams API Token

In order to send the Webex Teams messages, you have two options:
  - **Option 1 (For tests only):** Use your own Webex Teams API token that will need to be updated manually every 12 hours.
  - **Option 2 (Recommended for production):** Use Webes Teams Bot to send messages.
      - Create Webex Teams Bot following these instructions: [Create a Bot](https://developer.webex.com/docs/bots)
      - Record its API Token
      - Add Bot to the Webex Teams Room so that it can send notifications.

### Step 5. Import Atomic Workflows

SXO workflow is represented by the file in JSON format, that contains definitions and description of all the activities, targets, variables and atomic workflows that are in use. In SecureX orchestration left hand-side menu, go to Workflows -> Atomic Actions -> Import -> Browse and import the atomic workflows.

https://ciscosecurity.github.io/sxo-05-security-workflows/importing

### Step 6. Import the main workflow

In SecureX orchestration left hand-side menu, go to Workflows -> My Workflows -> Import -> Browse and import the workflow called __sxo-santa-tracker-workflow__.

You will be presented with the following warning:

![](/assets/import_warning.png)

Don't get scared and click "Update" :)

4. This is where you will provide your Webex Token:

![](/assets/token_request.png)

Copy your personal Webex API Token or your Bots' API Token into the VALUE field. This is Secure String variable and it will be stored securely in the SXO.

5. You should see the new workflow being added to the list. Click on the workflow when import is complete.

6. If import was successful, you should see zero warnings at the top of the workflow canvas.

![](/assets/inside_workflow.png)

## Step 7. Run the workflow

1. To execute the workflow, click RUN at the right top corner in the action pane.

![](/assets/action_pane.png)

2.  Observe workflow execution.

> As the workflow progresses, you should see activities turning green. Don't be alarmed if some activities turn red, it is expected behavior.

3. Once execution is complete, examine the Webex Teams Room and Email Inbox for notifications and check if Threat Response Casebook has been created/updated in SecureX Ribbon.

> You can return to previous runs information by clicking `VIEW RUNS` inside the workflow or going to __Runs__ in the left hand-side menu.
