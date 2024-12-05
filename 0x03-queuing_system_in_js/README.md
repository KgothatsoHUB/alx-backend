# Queueing System In JavaScript
This project contains tasks for learning to create a queueing system in JavaScript.

      ```
    + Create a queue named `push_notification_code`, and create a job with the object created before.
    + When the job is created without error, log to the console `Notification job created: JOB ID`.
    + When the job is completed, log to the console `Notification job completed`.
    + When the job is failing, log to the console `Notification job failed`.
  + Nothing else will happen - to process the job, go to the next task!
  + If you execute multiple time this file, you will see the `JOB ID` increasing - it means you are storing new job to process…

+ [x] 7. **Create the Job processor**
  + In a file named [`6-job_processor.js`](6-job_processor.js):
  + Create a queue with `Kue`.
  + Create a function named `sendNotification`:
    + It will take two arguments `phoneNumber` and `message`.
    + It will log to the console `Sending notification to PHONE_NUMBER, with message: MESSAGE`.
  + Write the queue process that will listen to new jobs on `push_notification_code`:
    + Every new job should call the `sendNotification` function with the phone number and the message contained within the job data.
  + **Requirements:**
    + You only need one Redis server to execute the program.
    + You will need to have two node processes to run each script at the same time.
    + You must use `Kue` to set up the queue.
  + [`6-job_processor.js`](6-job_processor.js) and [`6-job_creator.js`](6-job_creator.js) are the same as [`5-subscriber.js`](5-subscriber.js) and [`5-publisher.js`](5-publisher.js) respectively but with a module to manage jobs.

+ [x] 8. **Track progress and errors with Kue: Create the Job creator**
  + In a file named [`7-job_creator.js`](7-job_creator.js):
    + Create an array `jobs` with the following data inside:
      ```js
      const jobs = [
        {
          phoneNumber: '4153518780',
          message: 'This is the code 1234 to verify your account'
        },
        {
          phoneNumber: '4153518781',
          message: 'This is the code 4562 to verify your account'
        },
        {
          phoneNumber: '4153518743',
          message: 'This is the code 4321 to verify your account'
        },
        {
          phoneNumber: '4153538781',
          message: 'This is the code 4562 to verify your account'
        },
        {
          phoneNumber: '4153118782',
          message: 'This is the code 4321 to verify your account'
        },
        {
          phoneNumber: '4153718781',
          message: 'This is the code 4562 to verify your account'
        },
        {
          phoneNumber: '4159518782',
          message: 'This is the code 4321 to verify your account'
        },
        {
          phoneNumber: '4158718781',
          message: 'This is the code 4562 to verify your account'
        },
        {
          phoneNumber: '4153818782',
          message: 'This is the code 4321 to verify your account'
        },
        {
          phoneNumber: '4154318781',
          message: 'This is the code 4562 to verify your account'
        },
        {
          phoneNumber: '4151218782',
          message: 'This is the code 4321 to verify your account'
        }
      ];
      ```
    + After this array has been created:
      + Create a queue with `Kue`.
      + Write a loop that will go through the array `jobs` and for each object:
        + Create a new job to the queue `push_notification_code_2` with the current object.
        + If there is no error, log to the console `Notification job created: JOB_ID`.
        + On the job completion, log to the console `Notification job JOB_ID completed`.
        + On the job failure, log to the console `Notification job JOB_ID failed: ERROR`.
        + On the job progress, log to the console `Notification job JOB_ID PERCENTAGE% complete`.

+ [x] 9. **Track progress and errors with Kue: Create the Job processor**
  + In a file named [`7-job_processor.js`](7-job_processor.js):
    + Create an array that will contain the blacklisted phone numbers. Add in it `4153518780` and `4153518781` - these 2 numbers will be blacklisted by our jobs processor.
    + Create a function `sendNotification` that takes 4 arguments: `phoneNumber`, `message`, `job`, and `done`:
      + When the function is called, track the progress of the `job` of `0` out of `100`.
      + If `phoneNumber` is included in the "blacklisted array", fail the job with an `Error` object and the message: `Phone number PHONE_NUMBER is blacklisted`.
      + Otherwise:
        + Track the progress to 50%.
        + Log to the console `Sending notification to PHONE_NUMBER, with message: MESSAGE`.
    + Create a queue with Kue that will process job of the queue `push_notification_code_2` with two jobs at a time.
  + **Requirements:**
    + You only need one Redis server to execute the program.
    + You will need to have two node processes to run each script at the same time.
    + You must use `Kue` to set up the queue.
    + Executing the jobs list should log to the console the following:
      + Terminal 2:
      ```powershell
      bob@dylan:~$ yarn dev 7-job_processor.js

      > queuing_system_in_js@1.0.0 dev /root
      > nodemon --exec babel-node --presets @babel/preset-env "7-job_processor.js"

      [nodemon] 2.0.4
      [nodemon] to restart at any time, enter `rs`
      [nodemon] watching path(s): *.*
      [nodemon] watching extensions: js,mjs,json
      [nodemon] starting `babel-node --presets @babel/preset-env 7-job_processor.js`
      Sending notification to 4153518743, with message: This is the code 4321 to verify your account
      Sending notification to 4153538781, with message: This is the code 4562 to verify your account
      Sending notification to 4153118782, with message: This is the code 4321 to verify your account
      Sending notification to 4153718781, with message: This is the code 4562 to verify your account
      Sending notification to 4159518782, with message: This is the code 4321 to verify your account
      Sending notification to 4158718781, with message: This is the code 4562 to verify your account
      Sending notification to 4153818782, with message: This is the code 4321 to verify your account
      Sending notification to 4154318781, with message: This is the code 4562 to verify your account
      Sending notification to 4151218782, with message: This is the code 4321 to verify your account
      ```
      + And in the same time in terminal 1:
        ```powershell
        ...
        Notification job #39 0% complete
        Notification job #40 0% complete
        Notification job #39 failed: Phone number 4153518780 is blacklisted
        Notification job #40 failed: Phone number 4153518781 is blacklisted
        Notification job #41 0% complete
        Notification job #41 50% complete
        Notification job #42 0% complete
        Notification job #42 50% complete
        Notification job #41 completed
        Notification job #42 completed
        Notification job #43 0% complete
        Notification job #43 50% complete
        Notification job #44 0% complete
        Notification job #44 50% complete
        Notification job #43 completed
        Notification job #44 completed
        Notification job #45 0% complete
        Notification job #45 50% complete
        Notification job #46 0% complete
        Notification job #46 50% complete
        Notification job #45 completed
        Notification job #46 completed
        Notification job #47 0% complete
        Notification job #47 50% complete
        Notification job #48 0% complete
        Notification job #48 50% complete
        Notification job #47 completed
        Notification job #48 completed
        Notification job #49 0% complete
        Notification job #49 50% complete
        Notification job #49 completed
        ```

+ [x] 10. **Writing the job creation function**
  + In a file named [`8-job.js`](8-job.js), create a function named `createPushNotificationsJobs`:
    + It takes in two arguments: `jobs` (array of objects) and `queue` (`Kue` queue).
    + If `jobs` is not an array, it should throw an `Error` with message: `Jobs is not an array`.
    + For each job in `jobs`, create a job in the queue `push_notification_code_3`.
    + When a job is created, it should log to the console `Notification job created: JOB_ID`.
    + When a job is complete, it should log to the console `Notification job JOB_ID completed`.
    + When a job is failed, it should log to the console `Notification job JOB_ID failed: ERROR`.
    + When a job is making progress, it should log to the console `Notification job JOB_ID PERCENT% complete`.

+ [x] 11. **Writing the test for job creation**
  + Now that you have created a job creator, let's add tests:
    + Import the function `createPushNotificationsJobs`.
    + Create a queue with `Kue`.
    + Write a test suite for the `createPushNotificationsJobs` function:
      + Use `queue.testMode` to validate which jobs are inside the queue.
      + etc.
  + **Requirements:**
    + Make sure to enter the test mode without processing the jobs before executing the tests.
    + Make sure to clear the queue and exit the test mode after executing the tests.

+ [x] 12. **In stock?**
  + **Data**
    + Create an array `listProducts` containing the list of the following products:
      + Id: 1, name: `Suitcase 250`, price: 50, stock: 4.
      + Id: 2, name: `Suitcase 450`, price: 100, stock: 10.
      + Id: 3, name: `Suitcase 650`, price: 350, stock: 2.
      + Id: 4, name: `Suitcase 1050`, price: 550, stock: 5.
  + **Data access**
    + Create a function named `getItemById`:
      + It will take `id` as argument.
      + It will return the item from `listProducts` with the same id.
  + **Server**
    + Create an `express` server listening on the port 1245. (You will start it via: `yarn dev 9-stock.js`).
  + **Products**
    + Create the route `GET /list_products` that will return the list of every available product with the following JSON format:
      ```powershell
      bob@dylan:~$ curl localhost:1245/list_products ; echo ""
      [{"itemId":1,"itemName":"Suitcase 250","price":50,"initialAvailableQuantity":4},{"itemId":2,"itemName":"Suitcase 450","price":100,"initialAvailableQuantity":10},{"itemId":3,"itemName":"Suitcase 650","price":350,"initialAvailableQuantity":2},{"itemId":4,"itemName":"Suitcase 1050","price":550,"initialAvailableQuantity":5}]
      bob@dylan:~$
      ```
  + **In stock in Redis**
    + Create a client to connect to the Redis server:
      + Write a function `reserveStockById` that will take `itemId` and `stock` as arguments:
        + It will set in Redis the stock for the key `item.ITEM_ID`.
      + Write an async function `getCurrentReservedStockById`, that will take `itemId` as an argument:
        + It will return the reserved stock for a specific item.
  + **Product detail**
    + Create the route `GET /list_products/:itemId`, that will return the current product and the current available stock (by using `getCurrentReservedStockById`) with the following JSON format:
      ```powershell
      bob@dylan:~$ curl localhost:1245/list_products/1 ; echo ""
      {"itemId":1,"itemName":"Suitcase 250","price":50,"initialAvailableQuantity":4,"currentQuantity":4}
      bob@dylan:~$
      ```
    + If the item does not exist, it should return:
      ```powershell
      bob@dylan:~$ curl localhost:1245/list_products/12 ; echo ""
      {"status":"Product not found"}
      bob@dylan:~$
      ```
  + **Reserve a product**
    + Create the route `GET /reserve_product/:itemId`:
      + If the item does not exist, it should return:
        ```powershell
        bob@dylan:~$ curl localhost:1245/reserve_product/12 ; echo ""
        {"status":"Product not found"}
        bob@dylan:~$
        ```
      + If the item exists, it should check that there is at least one stock available. If not it should return:
        ```powershell
        bob@dylan:~$ curl localhost:1245/reserve_product/1 ; echo ""
        {"status":"Not enough stock available","itemId":1}
        bob@dylan:~$
        ```
      + If there is enough stock available, it should reserve one item (by using `reserveStockById`), and return:
        ```powershell
        bob@dylan:~$ curl localhost:1245/reserve_product/1 ; echo ""
        {"status":"Reservation confirmed","itemId":1}
        bob@dylan:~$
        ```
  + **Requirements:**
    + Make sure to use `promisify` with Redis.
    + Make sure to use the `await`/`async` keyword to get the value from Redis.
    + Make sure the format returned by the web application is always JSON and not text.

+ [x] 13. **Can I have a seat?**
  + **Redis**
    + Create a Redis client:
      + Create a function `reserveSeat`, that will take the argument `number`, and set the key `available_seats` with the `number`.
      + Create a function `getCurrentAvailableSeats`, it will return the current number of available seats (by using `promisify` for Redis).
      + When launching the application, set the number of available to 50.
      + Initialize the boolean `reservationEnabled` to `true` - it will be turned to `false` when no seat will be available.
  + **`Kue` queue**
    + Create a `Kue` queue.
  + **Server**
    + Create an express server listening on port 1245. (You will start it via: `yarn dev 100-seat.js`).
    + Add the route GET /available_seats that returns the number of seats available:
      ```powershell
      bob@dylan:~$ curl localhost:1245/available_seats ; echo ""
      {"numberOfAvailableSeats":"50"}
      bob@dylan:~$
      ```
    + Add the route `GET /reserve_seat` that:
      + Returns `{ "status": "Reservation are blocked" }` if `reservationEnabled` is `false`.
      + Creates and queues a job in the queue `reserve_seat`:
        + Save the job and return:
          + `{ "status": "Reservation in process" }` if no error.
          + Otherwise: `{ "status": "Reservation failed" }`.
        + When the job is completed, print in the console: `Seat reservation job JOB_ID completed`.
        + When the job failed, print in the console: `Seat reservation job JOB_ID failed: ERROR_MESSAGE`.
    + Add the route `GET /process` that:
      + Returns `{ "status": "Queue processing" }` just after:
      + Process the queue `reserve_seat` (async):
        + Decrease the number of seat available by using `getCurrentAvailableSeats` and `reserveSeat`.
        + If the new number of available seats is equal to 0, set `reservationEnabled` to false.
        + If the new number of available seats is more or equal than 0, the job is successful.
        + Otherwise, fail the job with an `Error` with the message `Not enough seats available`.
  + **Requirements:**
    + Make sure to use `promisify` with Redis.
    + Make sure to use the `await`/`async` keyword to get the value from Redis.
    + Make sure the format returned by the web application is always JSON and not text.
    + Make sure that only the allowed amount of seats can be reserved.
    + Make sure that the main route is displaying the right number of seats.
