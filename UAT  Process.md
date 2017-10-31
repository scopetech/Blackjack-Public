#User acceptance testing

**UAT** - User acceptance testing, development process stage when client representatives have an access to a newly created product and can provide developers with a feedback. Process of user's feedback gathering described below.

## Inception

1. UAT Initiator, normally development team member, warns all Team members about start of UAT
2. UAT Initiator creates trello board for UAT tracking
3. UAT Initiator shares trello board with all stake-holders including developers who will be involved in UAT
4. UAT Initiator places new active UAT board link to project's readme.

## Work in UAT Trello Board

Trello Board consist of lists and cards. Cards can be marked with label and placed into list.

Each list represents particular processing state of the card.

There are two types of list organization: simple and advanced. Simple is primarelly for question-answer type of communication between Scope Technology developers and external developers trying to utilize portal API. Advanced caters for proper UAT with bug reporting and feature requests

# Simple

| List Name | Description | Who insert? |  Who monitor? | Card pick-up |
|---|---|---|---|---|
| **Start** | Card is just created or card is returned from **Processed** because card processing result is unsatisfactory | Client Representative | Developer | Developer review card. If developers see that he or she is capable to assist with this card he or she places card into **In Progress** and starts to process card content |
| **In Progress** | Card is actively processed by Developer  | Developer | Developer | Card content is resolved, card goes to **Processed**. Developer who was processing card is not capable to process it further, card goes to **Start**, appropriate comments and labels must be added and or removed |
| **Processed** | Card content is resolved | Developer | Developer |  Processing result is satisfactory, card goes to  **Closed**. Processing result are not satisfactory, clarification comment is added to card, card goes to **Start**.  |
| **Closed** | Card content is resolved and verified | Client Representative | Nobody |   |

# Advanced 

| List Name | Description | Who insert? |  Who monitor? | Card pick-up |
|---|---|---|---|---|
| **Start** | Card is just created or card is returned from **Delivered** because card processing result is unsatisfactory | Client Representative | Developer | Developer review card. If developers see that he or she is capable to assist with this card he or she places card into **In Progress** and starts to process card content |
| **Question** | Client Representative have to answer the question stated by Developer in card | Developer | Client Representative  | Question is answered in card's comments, card goes to **Answered** |
| **Answered** | Client Representative answered a question stated by Developer  | Client Representative | Developer | Developer review card and places it into **In Progress**  |
| **Backlog** | Card content is clear for Developer, card content is reasonable. Card is not actively processed by Developer now or card processing is organized in internal tracking system | Developer | Developer | Card content is processed in internal tracking system and resolved, card goes to **Processed**. Card was not actively processed but now Developer started to process it actively, card goes to **In Progress**  |
| **In Progress** | Card is actively processed by Developer  | Developer | Developer | Card content is resolved, card goes to **Processed**. Card content will not be processed further, card goes to **Rejected**. Card content will not be actively processed right now or will be processed in internal tracking system, card goes to **Backlog**. Developer who was processing card is not capable to process it further, card goes to **Start**, appropriate comments and labels must be added and or removed |
| **Processed** | Card content is resolved, but processing results is not yet available to Client Representatives | Developer | Developer | Deployment to environment available for Client Representatives is done, card goes to **Delivered** |
| **Delivered** | Card content is resolved and can be verified | Developer | Client Representative | Processing result is satisfactory, card goes to  **Verified**. Processing result are not satisfactory, clarification comment is added to card, card goes to **Start**.   |
| **Rejected** | Card content will not be processed | Developer / Client Representative | Nobody |   |
| **Verified** | Card content is resolved and verified | Client Representative | Nobody |   |

Card can be marked with label to classify card. 

| Label | Description | 
|---|---|
| **portal-api** | Card is related to Portal API |  
| **iOS** | Card is related to iOS application | 
| **android** | Card is related Android application |
| **tracked** | Card processing is organized in internal tracking system | 

Labels must not be used to store historical information. Labels describes card's current state e. g. if card processing is not related to Android application anymore but portal is now needed to resolve card then **android** label must be removed and **portal-api** must be added.

Labels **portal-api**, **iOS** and **android** must be added to card by Client Representatives or Developers as soon as possible, these labels are used to identify who is capable with processing of card.
 
## UAT Process

Current Blackjack contact person monitors Trello Board to pick up cases from there and accepts requests from other Scope employees regarding particular UAT, these request must contain reference to Trello case.

If case can be resolved on the fly, current Blackjack contact person resolves it and marks or moves trello case accordingly.

If case can't be resolved on the fly, current Blackjack contact person creates issue in internal tracking system with reference to initial Trello case, and marks newly created issue with **uat** label. 
Current Blackjack contact person responsible of updating Trello case with a results of GitHub issue processing

Current Blackjack contact person can be multiple developers.

## Completion 

Responsible person, removes corresponding UAT board link from project's readme. And creates finished UAT description block in [Finished UATs list](https://github.com/scopetech/Blackjack/wiki/Finished-UATs) using existing finished UAT description block as a template.

 
  
