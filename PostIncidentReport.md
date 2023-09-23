# Incident:

We committed a new change to our deployment repository, creating a version 2 of our URL Shortener application. After deploying the update, our application broke. There was an “Internal Server Error” when trying to complete a core function of the application - creating a shortened URL.

<img width="989" alt="error 2" src="https://github.com/nalDaniels/Dep3.1/assets/135375665/e012ed0f-c6dd-4666-bc58-a39443aba365">


# The Error:

The error was the result of an overloaded server or issue with the application code. Upon looking at the logs, it was clear that there was a problem with one of the JSON methods used in the code.

<img width="966" alt="logs" src="https://github.com/nalDaniels/Dep3.1/assets/135375665/19436e9e-7556-4a24-b2c1-dfabc4162290">


# The Resolution:

I looked at the git log —oneline to see the commit history. I noticed that there was a commit for Version 1 and for Version 2. I used git checkout 75ccfd4 and git checkout 4707109 to compare the differences in the application.py file. I noticed a difference in this line of code:

<img width="393" alt="error" src="https://github.com/nalDaniels/Dep3.1/assets/135375665/07a3e1dc-4a4a-4c59-a3e3-7a0b12f38e4b">

The json.load() is used to transform a json file to a dictionary, where as the json.loads() is expecting the value to be a string and change it to a dictionary. Since the application was not expecting a string value, it threw an error. I changed the V2 and redeployed it. The application worked successfully. 

Though the issue was resolved, it resulted in another problem, the application being down for a prolonged period of time.

This process of rolling forward took 20+ minutes, which failed to honor our Service Level Agreement that promised 20 minutes of downtime per year. One error used up all of our time allotted for the year. 

# The Ideal Resolution:

Since we are only afforded 20 minutes of downtime per year, it is imperative that we choose to rollback to the last successful deployment, in this case, V1. After deploying V2, the app was down, so it is not advisable to use up any additional time trying to identify and fix the error as this would further impact the user experience, business, and the SLA. After the rollback, we can then troubleshoot the error and redeploy V2. This will enable us to honor our SLA and conserve our time for future failures or errors.

# Additional Steps to Prevent Future Application Errors:

To avoid this reoccurring in the future, we can have senior engineers review the code, which should be in a separate branch, before it gets pulled into the main deployment branch. You  can also optimize this process by testing the code separately with a potential input (URL) before the pull request is made to the main branch  We can also run integration tests during our Jenkins (just build and test stages) pipeline to ensure that any errors will arise before the deployment of the new commit.

# Process
![Deployment 3 1 drawio](https://github.com/nalDaniels/Dep3.1/assets/135375665/89248636-51ba-4d16-9634-b29f86fdce02)

