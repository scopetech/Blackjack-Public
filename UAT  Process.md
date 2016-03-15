_For list of Active UATs see [Active UATs list](https://github.com/scopetech/Blackjack/wiki/UATs#active-uats)_

**UAT** - User accepting testing, development process state when client representatives have an access to a newly created product and can provide developers with a feedback. Process of user's feedback gathering described below.

## Inception

1. UAT Initiator warns all Blackjack Team members about start of UAT
2. UAT Initiator creates trello board four UAT tracking
3. UAT Initiator shares trello board with all stake-holders including all Blackjack Team members
4. UAT Initiator creates new active UAT description block on [UATs page](https://github.com/scopetech/Blackjack/wiki/UATs#active-uats). Use existing UAT description blocks as a template. Be sure to fill
    1. Start Date
    2. Trello Board - link to Trello Board
    3. Current Blackjack contact person(s) - set Team Lead by default. During UAT Blackjack will dedicated responsible persons internally and will update value of this field. Only person or persons mentioned in this field at the moment should be contacted regarding particular UAT
    4. Trello Board Rules: short description how Blackjack developers should act, basically mark and/or move cases from list to list, in given Trello board. If Trello Board Rules is not provided Blackjack developers should act in Trello board following on Trello best practices and common sense 

## UAT Process

Current Blackjack contact person monitors Trello Board to pick up cases from there and accepts requests from other Scope employees regarding particular UAT, these request must contain reference to Trello case.

If case can be resolved on the fly, current Blackjack contact person resolves it and marks or moves trello case accordingly.

If case can't be resolved on the fly, current Blackjack contact person creates GitHub issue with reference to initial Trello case, and marks GitHub issue with **uat** label. Current Blackjack contact person adds reference to GitHub issue to Trello case.
GitHub issue will be processed under [Active Development Stage](https://github.com/scopetech/Blackjack/wiki/Issue-Tracking#active-development-stages) rules. Current Blackjack contact person responsible of updating Trello case with a results of GitHub issue processing

## Completion 

Responsible person, moves corresponding UAT description block from [Active UATs list](https://github.com/scopetech/Blackjack/wiki/UATs#active-uats) to [Finished UATs list](https://github.com/scopetech/Blackjack/wiki/UATs#finished-uats) adding UAT end date and removing Current Blackjack contact person field

 
  
