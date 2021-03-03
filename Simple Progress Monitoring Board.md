## Simple Progress Monitoring Board.md

Trello Board consist of lists and cards. Cards can be marked with label and placed into list.

Each list represents particular processing state of the card.

### Simple

| List Name | Description | Who insert? |  Who monitor? | Card pick-up |
|---|---|---|---|---|
| **Start** | Card is just created or card is returned from **Processed** because card processing result is unsatisfactory | Client Representative | Developer | Developer review card. If developers see that he or she is capable to assist with this card he or she places card into **In Progress** and starts to process card content |
| **In Progress** | Card is actively processed by Developer  | Developer | Developer | Card content is resolved, card goes to **Processed**. Developer who was processing card is not capable to process it further, card goes to **Start**, appropriate comments and labels must be added and or removed |
| **Processed** | Card content is resolved | Developer | Client Representative |  Processing result is satisfactory, card goes to  **Closed**. Processing result are not satisfactory, clarification comment is added to card, card goes to **Start**.  |
| **Closed** | Card content is resolved and verified | Client Representative | Nobody |   |
