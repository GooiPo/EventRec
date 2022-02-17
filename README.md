# Event Recommendation System
- A personalization based event recommendation systems for event search
![](/img/demo.gif)

## Table of contents
* [1. Instruction](#1-instruction)
* [2. Infrastructure Design](#2-infrastructure-design)
* [3. API Design](#3-api-design)
* [4. Database](#4-database)
* [5. Design pattern](#5-design-pattern)

## 1. Instruction 
- Frontend: an interactive web page with `AJAX` technology implemented with `HTML`, `CSS` and `JavaScript`. This application has 3 main functions:
   * **Search** events around users based on geolocation
   * Users can save **Favorite** events
   * Show **recommendation of events** based on favorite history of users
- Backend: utilize `Java` servlet with `RESTful APIs` in `Apache Tomcat` to handle HTTP requests and responses
   * Utilized relational databases (`MySQL`) to store data from TicketMaster API
   * Design **content-based recommendation algorithm** for event recommendation
   * Deployed server on `Amazon EC2`: [Event Recommendation System](http://35.182.150.69/EventRec/)

## 2. Infrastructure Design
- 3-tier architecture
   * Presentation tier: HTML, CSS, JavaScript
   * Data tier: MySQL, MongoDB
   * Logic tier: Java
- Local and remote development environment

![local environment](https://raw.githubusercontent.com/MoonSulong/EventRecommendation/master/img/local.png)
> Local development environment

![remote environment](https://raw.githubusercontent.com/MoonSulong/EventRecommendation/master/img/remote.png)
> Remote development environment

## 3. API Design
- Logic tier(Java Servlet to RPC)
   * Search
      * searchItems
      * Ticketmaster API
      * parse and clean data, saveItems
      * return response
   * History
      * get, set, delete favorite items
      * query database
      * return response
   * Recommendation
      * recommendItems
      * get favorite history
      * search similar events, sorting
      * return response
   * Login
      * GET: check if the session is logged in
      * POST: verify the user name and password, set session time and marked as logged in
      * query database to verify
      * return response
   * Logout
      * GET: invalid the session if exists and redirect to `index.html`
      * POST: the same as GET
      * return response
   * Register
      * Set a new user into users table/collection in database
      * return response

![APIs design](https://raw.githubusercontent.com/MoonSulong/EventRecommendation/master/img//APIs.png)
> APIs design in logic tier

- TicketMasterAPI
[Official Doc - Discovery API](https://developer.ticketmaster.com/products-and-docs/apis/discovery-api/v2/)
- Recommendation Algorithms design
   * **Content-based Recommendation**: find categories from item profile from a user’s favorite, and recommend the similar items with same categories.
   * Present recommended items with ranking based on distance (geolocation of users)

![recommendation algorithm](https://raw.githubusercontent.com/MoonSulong/EventRecommendation/master/img/recommendation.png)
> Process of recommend request

## 4. Database
- MySQL
   * **users** - store user information.
   * **items** - store item information.
   * **category** - store item-category relationship
   * **history** - store user favorite history

![mysql](https://raw.githubusercontent.com/MoonSulong/EventRecommendation/master/img/mysql.png)
> MySQL database design

- MongoDB
   * **users** - store user information and favorite history. = (users + history)
   * **items** - store item information and item-category relationship. = (items + category)
   * **logs** – store log information

## 5. Design pattern
   * **Builder pattern**: `Item.java`
      * When convert events from TicketMasterAPI to java Items, use builder pattern to freely add fields.
   * **Factory pattern**: `ExternalAPIFactory.java`, `DBConnectionFactory.java`
      * `ExternalAPIFactory.java`: support multiple function like recommendation of event, restaurant, news, jobs… just link to different public API like TicketMasterAPI. Improve extension ability.
      * `DBConnectionFactory.java`: support multiple database like MySQL and MongoDB. Improve extension ability.
   * **Singleton pattern**: `MySQLConnection.java`, `MongoDBConnection.java`
      * Only create specific number of instance of database, and the class can control the instance itself, and give the global access to outerclass

## Acknowledgement
- Above figures credit to Hannah Wang
