# Non-functional requirements

## Deployment

* the result should be prepared for running in Docker
    * using docker-compose is for demonstrating


## Documentation

The project should be documented in README.md, which must contain:

* how to setup development environment (what JDK, is DBMS required, docker) 
* how to run project as a developer
* how to build app as docker
* how to configure and run app in docker / docker-compose

## Performance

All the performance tests should be measured on single 2 CPU 4 GB node (e.g. `t2.medium` instance type on [AWS](https://aws.amazon.com/ec2/instance-types/) with running docker images of application and database.

Test scenario should include following steps:

1. Before performance test is running, create the interviewer and candidate slots for a current week, both on 9:00..21:00, so 8 bookings can be placed in a row: at 9:00, 10:30, 12:00, 13:30, 15:00, 16:30, 18:00, 19:30..21:00
2. Every testing thread has some `threadId`=[1..8] should run the scenario:
    1. Log in as Coordinator, random delay 1..2 seconds
    2. Make booking for its period (e.g. threadId #3 makes booking at 12:00..13:30), random delay 1..2 seconds
    3. Request current week dashboard, random delay 1..2 seconds
    4. Drop just created time slot, random delay 1..2 seconds
    5. Go to step 2
3. Run 8 parallel threads of scenario for 10 minutes
4. Assert each response latency is **less then 500 milliseconds**

## Security

* facebook oauth2 token must not be logged at levels INFO, WARN or ERROR
* local JWT token must not be logged at levels INFO, WARN or ERROR
