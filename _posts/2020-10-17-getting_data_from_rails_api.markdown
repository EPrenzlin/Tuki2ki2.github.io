---
layout: post
title:      "Getting Data from Rails API"
date:       2020-10-17 21:17:09 +0000
permalink:  getting_data_from_rails_api
---


The content of your blog post goes here.

To start things off, we need to refresh our memories with Rails routes. For the purposes of this project, I use the standard resources : ( model_name) configuration. 

```
Rails.application.routes.draw do
  resources :tasks 
  resources :employees

```

The routes are important because it paves the path latter for when we are using fetch. To summarise it briefly, on any specific rails api page, there will be information stored. For instance: 

```
http://localhost:3000/employees/7
```

This page will store the information we have configured within the Employee show route rendered it in a json manner for our Fetch request later. 

This is step-one of the two step process to get the information we want displayed on our JS SPA. 

Step two involves using Fetch. 

One example of Fetch synxtax would be as follows: 
```
    allTasks(){
        return fetch(http://localhost:3000/tasks)
        .then(response => response.json())
    }
    }
```

To briefly exaplin the above, fetch will go to the URL taskLink, take all the information presented on the page ( which we have full control over in our respective ModelController) ; upon the fetch having "gotten" our info, we use response => response.json() to return the values initially presented on the page. 

Once we have that, the below code is used: 
```
                .then(obj => obj.forEach(task => Task.renderTask(task)))

```

Because the return value from the response.json was an array, we are iterating over each "task" in the array, and calling Task.renderTask(task) onto the element of the array passed into the argument. 

Upon completion, we simply append the div, or container we want onto the main page, and we would have successfuly displayed all the information we want from the server to our SPA page. 
