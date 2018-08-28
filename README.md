# jira-ticket-export
A NodeJS solution for automatic export of tickets from Atlassian Jira

# Introduction 
This project exports several lists of tickets that you can define by JQL. You can also define the ticket attributes that you want to be exported with every ticket. 

# Technology
The idea of this project started after working with Postman for a while. If you don't know Postman yet, have a look. It's a great tool to test your Web APIs. You can create collections of calls, specify HTTP verbs, make calls reusable on different stages by using environment variables, setup authentication, setup tests that check the expected results of a call etc. 

When you have some decent experience with Postman and have built a few collections, you might start to think about automating these calls. This is where newman (the cli companion for Postman) comes into play. Newman can run Postman collections programmatically, it allows you to inject variables and to redirect collection output. 

This is what this project does: based on a Postman collection with dynamic calls, it searches for tickets in Jira and exports them to csv. 

# Setup
Setting up a working instance of this project starts by specifying the URL of your Jira instance as well as your credentials. You then define a list of JQLs that you want to have exported and the list of attributes you want to export with every  ticket. The script then injects your specified values into a Postman collection and then runs this collection using Newman (the cli companion for Postman). The returned lists are finally exported to csv files.

## Jira instance
You define the details of your specific Jira instance in _jira.json_. Please create this file in the root directory, since it's not part of the repository. Use the following skelleton: 

```json
{
    "url": "https://jira.mycompany.com:8080",
    "username": "pwhty",
    "password": "secret"
}
```

## Attribute list
You list the attributes you want to have exported in _attributes.json_. The attributes list looks like this: 

```json
{
    "csvFields": [
      {"label":"Ticket Key", "value":"key"},
      {"label":"Ticket ID", "value":"id"},
      {"label":"Description", "value":"fields.summary"},
      {"label":"Status", "value":"fields.status.name"},
      {"label":"Priority", "value":"fields.priority.name"},
      {"label":"Booked Time", "value":"fields.timespent"},
      {"label":"Story Points", "value":"row.fields.customfield_10806"}
    ]
}
```

All attributes you use in JQL can be exported. Play around with the JQL editor to find the technical names of your desired attributes. You can also find attribute candidates [here](https://confluence.atlassian.com/jirasoftwarecloud/advanced-searching-fields-reference-764478339.html).

## JQL
You list the JQLs you want to have exported in _jql.json_. Every JQL you put in a list element represents one exported list, filtered by that JQL. The exported list will be saved as _JQL-item.csv_, where _JQL-item_ represents the name of the key in the JQL object. This repository comes with three default JQLs, all regarding Tickets of the last 14 days: 
* all new tickets created during the last 14 days
* all tickets closed during the last 14 days
* all remaining tickets in the project 

# Advanced functions
## Post-request scripts
After every call to the Jira API, newman can run a so-called post-request script that can change the contents of the result set. You can code and test your post-request script in Postman, before you paste it to _post_request.js_. This is what a post request script looks like:

```javascript

```

## Variables in JQL
You can define variables for you JQLs that get asked when the export is started. For example, you can setup to export the default JQLs for the last _length_of_sprint_ days, instead of the last 14 days. The export script will then ask the user to enter the value of that variable. This is what a JQL list with variables looks like: 

