# Housing rest-api

We are building a neighbourhood collaboration site and we want to make a system to keep track of people, houses, and addresses of those houses.

    Each person has a name, age and number of people in their household
    Each house has an address and an owner
    Each address has a postcode and street address

The REST API will need to enable users to:

    Store people, houses and addresses
    Look up a house, it’s address and owner
    Look up people in our neighbourhood within certain age brackets and with specific household sizes

As the data is related (people own houses, houses have addresses) we opted for a relational database using SQL.
Our database schema will look like this:

|Table name owners   |    name    |    age    |    household_size    |   PRIMARY KEY owner_id    |    FOREIGN KEY house_id |
|--------------------|------------|-----------|----------------------|---------------------------|-------------------------|
|                    |    Kai     |    22     |          5           |          101              |             1           |
|                    |    Paola   |    25     |          6           |          102              |             2           |

|Table name addresses    |    PRIMARY KEY house_id   |    street_address    |    postcode   |
|------------------------|---------------------------|----------------------|---------------|
|                        |             1             |   22 Koala Street    |    HF12 4DE   |    
|                        |             2             |    23 Bear Road      |    JG24 2FW   |   

Data will be accessed using SQL queries. 

API will be able to handle GET, POST, PATCH/PUT and DELETE requests.

GET --> will read data from the DB as required 

    (SELECT X FROM Y;)

POST --> will add data to our DB 

    (INSERT INTO X VALUES (x,y,z);)

PATCH --> will allow users to update data; such as if they move houses 

    (SET X WHERE Y;)

DELETE --> will allow users to delete data from the DB if they do not wish the data to be stored anymore 

    (DELETE X FROM Y WHERE Z)

GET owner infomation: 

    SELECT * FROM owners WHERE owner_id=101;
    /owners/?101 --> returns all data about the owner with id 101

GET address of 101: 

    SELECT street_address AS address FROM(owners RIGHT OUTER JOIN addresses ON owners.house_id = addresses.house_id WHERE owner_id=101);
    /owners/?101/address --> returns street_address data of owner with id 101

GET people within specific age brackets:

    SELECT name FROM owners WHERE age BETWEEN 21 AND 26;
    /owners/?minage=21&maxage=26 --> retrurns the names of owners that have ages between 21 and 26

GET people with specific household sizes

    SELECT name from owners WHERE household_size=5;
    /owners/?household_size=5
