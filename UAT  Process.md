_For list of Active UATs see [Active UATs list](https://github.com/scopetech/Blackjack/wiki/UATs#active-uats)_

**UAT** - User acceptance testing, development process stage when client representatives have an access to a newly created product and can provide developers with a feedback. Process of user's feedback gathering described below.

## Inception

1. UAT Initiator warns all Team members about start of UAT
2. UAT Initiator creates trello board for UAT tracking
3. UAT Initiator shares trello board with all stake-holders including developers who will be involved in UAT
4. UAT Initiator creates new active UAT description block on [UATs list page](https://github.com/scopetech/Blackjack/wiki/UATs#active-uats). Use existing UAT description blocks as a template. Be sure to fill
    1. Start Date
    2. Trello Board - link to Trello Board
    3. Current Blackjack contact person(s) - set Team Lead by default. During UAT Blackjack will dedicated responsible persons internally and will update value of this field. Only person or persons mentioned in this field at the moment should be contacted regarding particular UAT

## Work in UAT Trello Board

Trello Board consist of lists and cards. Cards can be marked with label and placed into list.

Each list represents particular processing state of the card.

| List Name | Description | Who insert? |  Who monitor? | Card pick-up |
|---|---|---|---|---|
| **Start** | Card is just created or card is returned from **Deployed** because card processing result is unsatisfactory | Client Representative | Developer | Developer review card. If developers see that he or she is capable to assist with this card he or she places card into **In Progress** and starts to process card content |
| **Question** | Client Representative have to answer the question stated by Developer in card | Developer | Client Representative  | Question is answered in card's comments, card goes to **Answered** |
| **Answered** | Client Representative answered a question stated by Developer  | Client Representative | Developer | Developer review card and places it into **In Progress**  |
| **Backlog** | Card content is clear for Developer, card content is reasonable. Card is not actively processed by Developer now or card processing is organized in internal tracking system | Developer | Developer | Card content is processed in internal tracking system and resolved, card goes to **Done**. Card was not actively processed but now Developer started to process it actively, card goes to **In Progress**  |
| **In Progress** | Card is actively processed by Developer  | Developer | Developer | Card content is resolved, card goes to **Done**. Card content will not be processed further, card goes to **Rejected**. Card content will not be actively processed right now or will be processed in internal tracking system, card goes to **Backlog**. Developer who was processing card is not capable to process it further, card goes to **Start**, appropriate comments and labels must be added and or removed |
| **Done** | Card content is resolved, but processing results is not yet available to Client Representatives | Developer | Developer | Deployment to environment available for Client Representatives is done, card goes to **Deployed** |
| **Deployed** | Card content is resolved and can be verified | Developer | Client Representative | Processing result is satisfactory, card goes to  **Verified**. Processing result are not satisfactory, clarification comment is added to card, card goes to **Start**.   |
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

If case can't be resolved on the fly, current Blackjack contact person creates GitHub issue with reference to initial Trello case, and marks GitHub issue with **uat** label. Current Blackjack contact person adds reference to GitHub issue to Trello case.
GitHub issue will be processed under [Active Development Stage](https://github.com/scopetech/Blackjack/wiki/Issue-Tracking#active-development-stages) rules. Current Blackjack contact person responsible of updating Trello case with a results of GitHub issue processing

Current Blackjack contact person can be multiple developers.

## Completion 

Responsible person, moves corresponding UAT description block from [Active UATs list](https://github.com/scopetech/Blackjack/wiki/UATs#active-uats) to [Finished UATs list](https://github.com/scopetech/Blackjack/wiki/UATs#finished-uats) adding UAT end date and removing Current Blackjack contact person field

 
  
