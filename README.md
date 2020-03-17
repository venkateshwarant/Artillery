# Artillery_Tutorial


## General overview

In this project we will load test our product helloworld. We will determine some parameters such as 
* Minimum request latency
* Maximum request latency
* Median request latency
* 99th percentile value
* 95th percentile value

## Goals

With the above parameters, we can do the following
* Analyze performance
  - How fast is it? / Is it fast enough?

* Soak testing
  - finding memory leaks and other resource leaks

* Analyze reliability
  - Does it degrade gracefully under stress?

* Capacity testing
  - Can we handle projected load with accepted performance and with architecture/ hardware we already have

* Analyze scalability
  - How does the system scale if we add more servers / upgrade the hardware? Is it linear? Where are the bottlenecks?

## Assumptions/Pre-requisites
1. Maven (v 3.6.2, or higher)
* Instructions to install here: https://maven.apache.org/download.cgi
* Check installation with the command `mvn --version`

2. Git (version 2.21.1)
* Instructions to install here: https://help.ubuntu.com/lts/serverguide/git.html
* Check installation with the command `git --version`

3. VirtualBox(v 6.0, or higher)
* Instructions to install here: https://www.virtualbox.org/wiki/Downloads 

4. Vagrant (v 2.2.5, or higher) 
* Instructions to install here: https://www.vagrantup.com/downloads.html
* (only if using Windows 10 or Windows 8 Pro) Disable Hyper-V, see instructions to disable here: https://www.poweronplatforms.com/enable-disable-hyper-v-windows-10-8/
* Check installation with the command `vagrant -v`

5. Helloworld product 
* Instruction to setup: https://github.com/acapozucca/helloworld/tree/master/product.helloworld
* This project needs this product to be deployed beforehand running any vagrant test.

6. NPM (version 6.13.4, or higher)
* Instructions to install here: https://www.npmjs.com/get-npm
* Check installation with the command `npm --version`

7. Artillery (version 1.6.0-29, or higher)
* Instruction to install here: https://artillery.io/docs/getting-started/
* Check installation with the command `artillery --version`

## Set local working environment

1- Clone this repo

2- Import the given project into your favourite IDE

3- Build the project using Maven. Either you do it from within the IDE, or from a terminal. From the terminal you must:

3.1-  Get to the directory

```
cd ~/<git_root_folder>/Artillery_Tutorial
```

3.2- Run the comand.

```
mvn clean install
```

Running this command should shown (almost at the end of the output):

```
[INFO] BUILD SUCCESS
```


**Note**

* The project is ready to be imported on the Eclipse IDE as an existen Maven project.


## Check local testing environment setup and run the test on startup


1-  Get to the directory

```
cd ~/<git_root_folder>/Artillery_Tutorial
```

2- Start the virtual environment: 
```
 vagrant up
```


3- It will automatically install all the required dependencies and start our test. You can see the test result like below.

```
Tests run: 3, Failures: 0, Errors: 0, Skipped: 0, Time elapsed: 0.186 sec

Results :

Tests run: 3, Failures: 0, Errors: 0, Skipped: 0

------------------------------------------------------------------------
[INFO] BUILD SUCCESS
------------------------------------------------------------------------
```

## Test script

Our test script may contain the following-

* One or more scenarios

* One or more load phases

## Artillery quick command

Artillery has a quick command which allows you to use it for ad-hoc testing (in a manner similar to ab). Run:

```
artillery quick --count 10 -n 20 http://192.168.33.14:8080/helloworld/helloworld.html
```

This command will create 10 "virtual users" each of which will send 20 HTTP GET requests to corresponding url.

## Run a test script

* While the quick command can be useful for very simple tests, the full power of Artillery lies in being able to
simulate realistic user behavior with scenarios. Let’s see how we’d run one of those.

```
config:

  target: ‘http://192.168.33.14:8080’

  phases:

    - duration: 60

      arrivalRate: 20

  scenarios:

    - flow:

      - get:

        url: "/helloworld/helloworld.html"
```
* Run the test with:

```
artillery run hello.yml
```

## Artillery report

when u run a test script as mentioned above, you may get a report like the below

```
Complete report @ 2019-01-02T17:32:36.653Z
Scenarios launched: 300
Scenarios completed: 300
Requests completed: 600
RPS sent: 18.86
Request latency:
    min: 52.1
    max: 11005.7
    median: 408.2
    p95: 1727.4
    p99: 3144
Scenario counts: 0: 300 (100%)
Codes:
    200: 300
    302: 300
```

## Report terminologies

* **Scenarios launched** is the number of virtual users created in the preceding 10 seconds (or in total)

* **Scenarios completed** is the number of virtual users that completed their scenarios in the preceding 10 seconds (or in the
whole test). Note: this is the number of completed sessions, not the number of sessions started and completed in a 10 second
interval.

* **Requests completed** is the number of HTTP requests and responses or WebSocket messages sent

* **RPS sent** is the average number of requests per second completed in the preceding 10 seconds (or throughout the test)

* **Request latency** is in milliseconds, and p95 and p99 values are the 95th and 99th percentile values (a request
latency p99 value of 500ms means that 99 out of 100 requests took 500ms or less to complete).

* **Codes** provides the breakdown of HTTP response codes received.

* If you see **NaN** ("not a number") reported as a value, that means not enough responses have been received to calculate the
percentile.

* If there are any **errors** (such as socket timeouts), those will be printed under Errors in the report as well.
