# Logistics-and-Delivery-System

To design and implement a robust, normalized, and scalable MySQL database that models a real-world logistics and delivery system. The system must track parcels, their statuses, delivery attempts, agent assignments, delivery routes, and international customs data with support for complex logistics workflows.




## Acknowledgements

I would like to sincerely thank the DevifyX Assignment Team for providing such a well-structured and practical project. The assignment helped me think critically and apply concepts in a real-world scenario, which also made my understanding of SQL fundamentals and database design much stronger.

I’m also grateful for the tools and resources provided by the SQL community and MySQL Workbench, which made it easier to design, test, and visualize the entire system effectively.



## Features



###  Core Features

1. **Parcel Management**

   Tracks essential details like sender, weight, dimensions, and insurance.

2. **Recipient Handling**

   Supports **multiple recipients per parcel** (group shipments).

3. **Delivery Status Tracking**

    Tracks each parcel’s **status history** with timestamps and it also                                                                      Includes predefined statuses: Dispatched, In Transit, Delivered, Delayed.
                                                                               

4. **Agent Assignment and History**

   Assigns delivery agents to parcels.
    Maintains **assignment history** for audit and workload tracking.

5. **Geographical Route Tracking**

   Logs each parcel’s route through **multiple locations**.
   records **arrival and departure times** at each location.

6. **Delivery Attempt Logging**

    Captures delivery attempts with **success/failure outcomes**.
    Supports detailed notes (e.g., customer unavailable).

7. **Location Metadata**

   Stores detailed location info (address, city, state, coordinates).
   Includes international support via `CountryCode`.
---

###  Bonus Features

1. **Customs and International Shipping Support**

    Integrated table `CustomsInfo` to store HS Codes, declared value, currency, and documentation.
   Allows classification of parcels as **international shipments**.

2. **Parcel Insurance**

   Each parcel includes an **insurance amount** for loss/damage tracking.

3. **Automated Agent Reassignment Trigger**

    On a **failed delivery**, a trigger automatically assigns a new agent.
   Ensures **no manual intervention** is needed for rescheduling.

4. **Agent Workload Optimization**

    Detects and lists agents handling **multiple concurrent parcels**.

5. **Real-Time Parcel Tracking Simulation**

    Route and status updates simulate a **real-time tracking environment**.

### ER Diagram 
| Entity/Table Name            | Description                                                                                       |
| ---------------------------- | ------------------------------------------------------------------------------------------------- |
| **`parcels`**                | Central table storing parcel metadata including weight, dimensions, and insurance.                |
| **`parcelrecipients`**       | Supports one-to-many relationship with `parcels`, allowing multiple recipients per parcel.        |
| **`statuses`**               | Master table for delivery status types (e.g., Dispatched, In Transit).                            |
| **`parcelstatushistory`**    | Logs the full history of each parcel’s status changes, including timestamp and responsible agent. |
| **`deliveryagents`**         | Stores delivery agent information (name, contact, region).                                        |
| **`agentassignment`**        | Records which agent was assigned to which parcel and when.                                        |
| **`deliveryattemptlogging`** | Captures each delivery attempt outcome and notes, supports trigger-based reassignment on failure. |
| **`locations`**              | Stores addresses, city/state/postal info, and coordinates for tracking parcel movement.           |
| **`routetracking`**          | Links parcels to a sequence of locations, capturing arrival and departure times.                                                                                              |

### Key Relationships
**One-to-many between parcels and**:

parcelrecipients (1 parcel → many recipients)

parcelstatushistory, agentassignment, deliveryattemptlogging, routetracking, and customsinfo

**Many-to-one from**:

parcelstatushistory → statuses

parcelstatushistory → deliveryagents

agentassignment → deliveryagents

routetracking → locations
![image](https://github.com/user-attachments/assets/6c4fd191-c350-492d-9d5e-2d986475e90f)


### Sample Queries

1. **Show all parcels currently "In Transit"**

SELECT p.ParcelID, p.SenderName, s.StatusDescription, h.Timestamp
FROM Parcels p
JOIN ParcelStatusHistory h ON p.ParcelID = h.ParcelID
JOIN Statuses s ON h.StatusID = s.StatusID
WHERE s.StatusDescription = 'In Transit';

Screenshot : ![image](https://github.com/user-attachments/assets/c4282334-ca94-4239-959a-a60ab604e0b2)

2. **Delivery history of AgentID = 1**

SELECT h.ParcelID, s.StatusDescription, h.Timestamp
FROM ParcelStatusHistory h
JOIN Statuses s ON h.StatusID = s.StatusID
WHERE h.AgentID = 1
ORDER BY h.Timestamp DESC;

Screenshot : ![image](https://github.com/user-attachments/assets/a44bc750-e74c-432c-9141-3fb115e635d9)

3. **List all parcels with failed delivery attempts**

SELECT DISTINCT ParcelID, AttemptTime, Notes
FROM DeliveryAttemptLogging
WHERE Outcome = 'Failure';

Screenshot  : ![image](https://github.com/user-attachments/assets/bca8f8d0-7311-40c1-904f-e26d81e0feb9)

4. **Show all recipients of ParcelID = 2**

SELECT RecipientName, ContactNumber
FROM ParcelRecipients
WHERE ParcelID = 2; 


![image](https://github.com/user-attachments/assets/aea99b36-b0a3-41d9-8c7c-df1bdb6e6dcc)

5. **Find parcels sent internationally**

SELECT DISTINCT p.ParcelID, l.City, l.CountryCode
FROM Parcels p
JOIN RouteTracking r ON p.ParcelID = r.ParcelID
JOIN Locations l ON r.LocationID = l.LocationID
WHERE l.CountryCode <> 'IN';

Screenshot: ![image](https://github.com/user-attachments/assets/49105765-efec-45c7-836e-a4a2878d5188)













