Jumbotail Data Engineering Hiring Assignment Solution

The problem statement requires the implementation of a system for tracking user journeys within an e-commerce application. The solution involves several components: Event Producer, Webhook, In-memory Queue, Queue Consumer, and Database. Here's an outline of the solution:
1. Event Producer:
   - Implement a DRIVER class that generates events for each user.
   - Simulate user behavior and generate events based on the user's journey within the application.
   - Implement batching to optimize event sending.
   - Monitor the API's performance by logging response times.
2. Webhook:
   - Create a server that acts as a receiver for events, implemented as a REST API.
   - The server should listen for incoming events at "localhost:8888/webhook".
   - Process requests asynchronously and handle errors by implementing a retrial mechanism.
   - Push events to the in-memory queue.
3. In-memory Queue:
   - Select an in-memory queue system of your choice.
   - Use the queue to enable asynchronous behavior for event processing.
   - Events pushed to the queue by the webhook will be consumed by the Queue Consumer.
4. Queue Consumer:
   - Develop a consumer that retrieves events from the in-memory queue in batches.
   - Insert the retrieved events into the chosen database.
   - Implement a retrial mechanism to handle errors during the insertion process.
   - Handle duplicate events coming from the client.


5. Database:
   - Select a suitable database for storing the events.
   - Design the Event entity to facilitate the extraction of necessary information for generating the desired output.
   - Handle multiple asynchronous write requests to the database efficiently.
   
Final Output:
1. Calculate the percentage of users at each stage of the user journey.
2. Evaluate the performance of different cities based on the percentage of users from each city.

