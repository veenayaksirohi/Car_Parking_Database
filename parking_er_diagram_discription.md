## Entity-Relationship (ER) Diagram Description

This ER diagram models a parking management system with the following entities and their relationships:

### Entities:

* **parkinglots\_details:** Stores comprehensive information about each parking lot.
    * `parkinglot_id` (INT, Primary Key): Unique identifier for the parking lot.
    * `parking_name` (TEXT): Name of the parking lot.
    * ... (rest of the attributes)

* **floors:** Details the different floors within a parking lot.
    * `parkinglot_id` (INT, Primary Key, Foreign Key): References `parkinglots_details.parkinglot_id`. Part of the composite primary key.
    * `floor_id` (INT, Primary Key): Unique identifier for the floor within a parking lot. Part of the composite primary key.
    * `floor_name` (VARCHAR): Name of the floor.

* **rows:** Represents rows of parking slots on each floor.
    * `parkinglot_id` (INT, Primary Key, Foreign Key): References `parkinglots_details.parkinglot_id`. Part of the composite primary key.
    * `floor_id` (INT, Primary Key, Foreign Key): References `floors.floor_id`. Part of the composite primary key.
    * `row_id` (INT, Primary Key): Unique identifier for the row on a floor. Part of the composite primary key.
    * `row_name` (VARCHAR): Name of the row.

* **slots:** Defines individual parking slots.
    * `parkinglot_id` (INT, Primary Key, Foreign Key): References `parkinglots_details.parkinglot_id`. Part of the composite primary key.
    * `floor_id` (INT, Primary Key, Foreign Key): References `floors.floor_id`. Part of the composite primary key.
    * `row_id` (INT, Primary Key, Foreign Key): References `rows.row_id`. Part of the composite primary key.
    * `slot_id` (INT, Primary Key): Unique identifier for the slot in a row. Part of the composite primary key.
    * `slot_name` (VARCHAR): Name of the slot.
    * `status` (INT): Indicates the slot status (0=Free, 1=Occupied).
    * `vehicle_reg_no` (VARCHAR, Nullable): Registration number of the vehicle currently parked in the slot.
    * `ticket_id` (VARCHAR, Foreign Key, Nullable): References `parking_sessions.ticket_id`. The current active parking ticket for this slot.

* **users:** Stores information about registered users of the parking system.
    * `user_id` (INT, Primary Key, Auto-increment): Unique identifier for the user.
    * `user_name` (VARCHAR): Name of the user.
    * ... (rest of the attributes)

* **parking\_sessions:** Records details of each parking session.
    * `ticket_id` (VARCHAR, Primary Key): Unique identifier for the parking session ticket.
    * `parkinglot_id` (INT, Foreign Key, Nullable): References `parkinglots_details.parkinglot_id`.
    * ... (rest of the attributes)

### Relationships:

* A `parkinglots_details` entity **has** one or more `floors` (one-to-many).
* A `floors` entity **contains** one or more `rows` (one-to-many).
* A `rows` entity **contains** one or more `slots` (one-to-many).
* A `users` entity **initiates** zero or more `parking_sessions` (one-to-many).
* A `slots` entity **hosts** zero or more `parking_sessions` over its lifetime (one-to-many).
* A `slots` entity is **currently occupied by** zero or one `parking_sessions` at any given time (one-to-zero-or-one), indicated by the `ticket_id` foreign key.