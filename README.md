# Microworkers.com R Client #

**microworkers** provides access to the [Microworkers](https://microworkers.com/) crowdsourcing platform.

## Installation ##

[![Build Status](https://travis-ci.org/leeper/microworkers.png?branch=master)](https://travis-ci.org/leeper/microworkers)

**microworkers** is [available on GitHub](http://github.com/leeper/microworkers) and can (soon) be installed from within R from your favorite CRAN mirror:

```R
install.packages("microworkers")
library("Rmonkey")
```

And the latest development version, available here, can be installed directly using  [devtools](http://cran.r-project.org/web/packages/devtools/index.html):

```R
if(!require("devtools")) {
    install.packages("devtools")
    library("devtools")
}
install_github("leeper/microworkers")
library("microworkers")
```

## Setup ##

To use microworkers, the user must have a Microworkers account, which automatically supplies an API access key. To use the API, however, you must contact [info@microworkers.com](mailto:info@microworkers.com) to gain API access.

Once everything is registered, your API key can be loaded into R using `options`:

```R
options(microworkers_key = 'YourAPIKey')
```

To confirm that your key has been loaded correctly, you can use the `account_info()` function to retrieve basic details of your user account, including  your username, the email address attached to your account, and the balance available to pay workers.

## Code Examples ##

Below are some code examples showing how to use the package.

### Create a Campaign ###

There are two types of campaigns available on Microworkers:

 1. Basic campaigns, which are available to all workers (potentially with geographical restrictions)
 2. HireGroup campaigns, which are available only to a specific system or user-defined worker group

```R
# BASIC CAMPAIGN
cmp <- basic_campaign()

# HIREGROUP CAMPAIGN
cmp <- hiregroup_campaign()
```


### Modify a Campaign ###

Once a campaign is started, it is possible to regulate the availability of the campaign to workers:

```R
# pause a campaign
pause_campaign(cmp)

# resume a campaign
pause_campaign(cmp)

# stop a campaign
pause_campaign(cmp)
```

To extend a campaign with additional task "positions", use `add_positions`:

```R
# add twenty positions
add_positions(cmp, 20)
```

The `set_speed` function can be used to modify the "speed" of the campaign (must be an integer between 1 and 1000):

```R
# set fastest speed
set_speed(cmp, 1000)
```

If a worker's performance is unacceptable or it is otherwise necessary to prevent a worker from participating in a campaign, it is possible to enact a campaign-specific block using the worker's ID:

```R
block_worker(cmp, "1712bas23")
```

### Retrieve Tasks and Results ###

`list_campaigns("basic")` and `list_campaigns("hiregroup")` return lists of campaigns.

To monitor the status of a particular campaign, use `get_campaign_status`, which will indicate whether a campaign has finished. To retrieve a more detailed response use `get_campaign`, which returns all details of a campaign as specified in `basic_campaign` or `hiregroup_campaign`.

The `get_results` function will return a data.frame of results from a campaign.

### Approve or Reject Tasks ###

To assess individual tasks (rather than the reuslts of a campaign as a whole), there are several functions available:

```R
# list tasks from a campaign and their approval status
tasks <- list_tasks(cmp)

# list tasks from a campaign for a specific worker
list_worker_tasks(cmp, worker = "1712bas23")

# retrieve a specific task
get_task(tasks[1])

# retrieve the "proof" file for a task
get_proof_file(tasks[1])
```

If a task is satisfactory, it can be approved or rejected:

```R
# approve
approve_task(cmp, tasks[1], comment = "Well done!")

# reject
reject_task(cmp, tasks[1], comment = "Sorry but this is inadequate")
```

An alternative to manually approving or rejecting work involves [VCODE verification](http://www.blog.microworkers.com/vcode-verification/), whereby a task is automatically approved or rejected based on whether a worker supplied a valid code (as generated by your server). This has to be configured when the campaign is created.


[![cloudyr project logo](http://i.imgur.com/JHS98Y7.png)](https://github.com/cloudyr)
