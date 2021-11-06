# nVenue_challenge
This project processes pitching data from the 2021 MLB season in order to "generate" accurate data for what is considered a "reach" in baseball.


1. Your high level approach to the problem.

   In order the answer the main problem, a direct manipulation of the data via pandas dataframes was used. Columns were added to the provided
   sample data in order to calculate the accuracy of the prediction model. For a higher level approach to the problem (automation), A distributed is used, focusing on horizontal scalability and parallelization. Specifically cron jobs. In the architecture, a client server model is used, which includes a client, entry-point server, cache, log table, data table, jobs table, scheduler, and worker pool. The cache is used to perform live sequential calculations during the game in order to decrease runtime as the cache rather the database is called for calculations during a game. The log table is used to record jobs and actions so that when an error arises, the logs can be used to backtrack and pinpoint the exact location and scope of an error. The data table is used to store data about pitches from the MLB. The jobs table is used to keep track of what jobs need to be ran. The scheduler coordinates which jobs need to be performed when multiple clients send chron jobs to the entry-point server at the same time. The worker pool runs the script for the backend system and is organized using a queue to determine the order of scripts to run.


2. How to run your solution

   Run the script using Jupyter Notebook. The packages that are used inlude pandas and numpy. Use the newest version of each software.


3. Any design decisions, trade offs, or assumptions you made.

   For the high level approach, I assumed that there were multiple clients that could be trying to access and retrieve data from the backend database that contains a large amount of data on MLB pitches. Also, the assumption was made that one database and server would be sufficient for the backend, which in real life would not suffice. This assumptions was made since the design was edited from the bottom up, meaning that problem was scaled up (as explained in the challenge_solution.ipynb notebook) from trying to model a 1 client 1 server model. Also, it was assumed that backend database would only need to be updated once a day at the most. I decided that it was not necessary to keep updating the "reach accuracy" value using all data from all games up to the current moment since pitching data from a few games during a day would not significantly change the "reach accuracy" value. This is assumed to be true by the law of large numbers.


4. Any extensions you have added or would like to add if you had more time.

   Things that I would like to add would be things that focus more on how to fix the system if the system failed. This includes issues such as when the server is down, the scheduler is down, the cron job that the client sends malfunctions (which would probably result in a bad return or a infinite loop), the worker goes down, or the worker also malfunctions.

5. What are the bottlenecks in the process? What ways could make this run faster?

   The bottlenecks in the process would be when there are multiple MLB games being played in a day. This would result in multiple different clients either trying to request or submit data to the server. The scheduler is the main component that deals with this bottleneck, ensuring that databases remain ACID complient. One way this bottleneck could be made to run faster would be to have the cron jobs run right before and after a game has been played. This way, the chances that a client is trying to retrieve or submit data to the backend database at the same time another client is would be reduced. 