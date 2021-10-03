# Functional Requirements
- FR1 - The banks and their clients shall compose messages, which contain the text message body, the sender's identity and a timestamp for the message.
  - Priority: High
  - Relates to BR2
- FR2 - The banks and their clients shall send the messages they compose.
  - Priority: High
  - Relates to BR2
- FR3 - The banks and their clients shall view previous and incoming messages, which contain the text message body, the sender's identity and the message's timestamp. 
  - Priority: High
  - Relates to BR2

# Nonfunctional Requirements
- NR1 - The messages shall be sent via SMS
  - Priority: High
  - Relates to BR2
- NR2 - The messages shall sent and recieved using end to end encryption
  - Priority: Medium
  - Relates to BR1
- NR3 - The identity of senders will be validated using digital signatures
  - Priority: Medium
  - Relates to BR1
- NR4 - The system shall define an API that interfaces with Accutech's other software products. 
  - Priority: Medium 
  - BR2
