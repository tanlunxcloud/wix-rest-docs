SortOrder: 0
# Workflows

## What is it?
Workflows is part of the Ascend project. The service that allows managing business-related processes.

## What can users do with it?
A user can manage a process related to his business, by defining a workflow as a sequence of
phases and tracking the progress of cards through these phases.
Cards can be linked to Contacts, set with a due date and hold a description of what the user is tracking.

Multiple workflows can be defined for the same site, in order to track different processes.

Automated actions can be defined through the "Automations UI" in order to trigger workflow-related actions when some 
events occur.
For instance, when a job application form is filled, a new card can be created automatically in "Recruitment Workflow"
under the "new candidate" phase.

### Examples
- Sales Workflow with the following phases:
  - Customer Contacted Me
  - Contact Made With Customer
  - Price Quote Sent
  - Invoice Sent
  - Invoice Paid

![Sales worflow](https://s3.amazonaws.com/wixplorer-readme-images/workflows%2Fsales_workflow.png)

## The API
Workflows functionality is exposed via the [com.wixpress.workflows.api](https://github.com/wix-private/crm/tree/master/workflow-server/workflow-server-api/src/main/proto/com/wixpress/workflow/api/v1)
package, which includes the following components:

#### [Workflow](https://github.com/wix-private/crm/blob/master/workflow-server/workflow-server-api/src/main/proto/com/wixpress/workflow/api/v1/messages/workflow.proto)
A workflow is a defined process that has a beginning, possibly several steps and an end.

A user can manage every flow in his business through a workflow, by defining the different steps of the workflow (phases),
and tracking the progress of managed-items (cards) through the different steps.

**WorkflowInfo** is a flat object (no fields of type `list`) that holds metadata about the workflow and has the following fields:
- `id` - a read-only field that serves as a unique identifier of each workflow
- `name` - the name of the workflow
- `description` - a description of the process that the workflow represents and it's purpose
- `createdAt` - a read only field that represents the creation time of the workflow
- `lastAccess` - a read only field that represents the last access time to the workflow

This component is managed via [workflowsService](https://github.com/wix-private/crm/blob/master/workflow-server/workflow-server-api/src/main/proto/com/wixpress/workflow/api/v1/workflowsService.proto)

**Workflow** is the full object that holds both metadata about the workflow, and some nested collections that
complete the whole tree that the workflow contains.  
It has the following fields:
- `info` - the **WorkflowInfo** object from above
- `winPhase` - an implicitly generated phase that each workflow has. This phase is intended to hold cards that complete the flow.
- `phasesList` - an ordered list of phases that defines the sequence of steps each card should go through in order to complete the process  
                The list can be partial (in case there are many phases) and includes the total number of phases in the workflow
- `labels` - a list of labels that defines an additional info (or category) that can be added to cards in this workflow

The nested collections (`phasesList`, `labels`) are managed by separate APIs as shown below.

#### [Phase](https://github.com/wix-private/crm/blob/master/workflow-server/workflow-server-api/src/main/proto/com/wixpress/workflow/api/v1/messages/phase.proto)
A phase is a single step in a workflow that defines a specific state in the process.

A user can set phases in the context of a specific workflow, in order to define the different steps required
for managing the workflow-process. 

**PhaseInfo** is a flat object (no fields of type `list`) that holds metadata about the phase and has the following fields:
- `id` - a read-only field that serves as a unique identifier of each phase
- `name` - the name of the phase
- `description` - a description of the step that the phase represents and it's purpose

This component is managed via [phasesService](https://github.com/wix-private/crm/blob/master/workflow-server/workflow-server-api/src/main/proto/com/wixpress/workflow/api/v1/phasesService.proto)

**Phase** is the full object that holds both metadata about the phase, and a nested collection of cards that the phase contains.  
It has the following fields:
- `info` - the **PhaseInfo** object from above
- `cardsList` - an ordered list of managed items (cards) in the process, whose state is the current phase   
                The list can be partial (in case there are many cards) and includes the total number of cards in the phase

The list of cards is managed by a separate API as shown below.

#### [Card](https://github.com/wix-private/crm/blob/master/workflow-server/workflow-server-api/src/main/proto/com/wixpress/workflow/api/v1/messages/card.proto)
A card is an abstract entity that represents whatever item that needs to be managed through a workflow-process. 

A user can add, update, delete and move cards between the different phases of a workflow,
in order to track their progress.

**CardInfo** is a flat object (no fields of type `list`) that holds metadata about the card and has the following fields:
- `id` - a read-only field that serves as a unique identifier of each card
- `name` - the name of the card
- `description` - a description of the managed item that the card represents and it's purpose
- `primaryAttachment` - a pointer to some attachment that represents the card. Currently there's only one type of
attachment (`ContactAttachment`) which points to an existing contact of the site.  
In the future there will be some other types of attachments, such as `Product`, `Appointment` and more.
- `dueDate` - the due date for the card to reach the end of the workflow-process
- `source` - the source that triggered the creation of the card

This component is managed via [cardsService](https://github.com/wix-private/crm/blob/master/workflow-server/workflow-server-api/src/main/proto/com/wixpress/workflow/api/v1/cardsService.proto)

**Card** is the full object that holds both metadata about the card, and a nested collection of labels that the card contains.  
It has the following fields:
- `info` - the **CardInfo** object from above
- `labels` - a list of labels that categorise the card

The list of labels is managed by a separate API as shown below.