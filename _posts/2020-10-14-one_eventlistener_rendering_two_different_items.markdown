---
layout: post
title:      "One EventListener rendering two different items "
date:       2020-10-14 21:12:09 +0000
permalink:  one_eventlistener_rendering_two_different_items
---


When a button is pressed on a single-page-application, there may be an expectation that only one subsequent action occurs - whether it be a delete, edit, or create, the expectation is that there is only one thing happens from this event.

```
    deleteEmployee(button, div, jsonObject){
        button.addEventListener("click", (e) => {
        console.log("from deleting employee")
        e.preventDefault()
        div.remove()
        
        api.deleteEmployee(jsonObject)
        .then(json => console.log(json))
        })
```

Within my Employee class, there is a aptly named "deleteEmployee", with the usual trappings of a javascript event - we have an event listener, we have the arguments we require to sucessfully delete an item ( the button itself that is being pressed, the div where we are deleting the aforementioned employee, and the jsonObject that we are using as an argument to send the request to the rails api). 

All the above is fairly standard and universal. The challenge I faced in my project is being able to incorporate the successful delete action and render a previous succesful created task that has an employees name, into being now labelled " unassigned" without requireing the user to hit refresh. 

This requires several actions:

```
const deletedName = document.querySelectorAll(".delete") 
                deletedName.forEach(deletedName => deletedName.addEventListener("click", (e) => {
                        name.innerText = "Unassigned"
                        }))
```

First, we have to be able to select all the delete buttons ; prior planning and assigning a a "class" to the actual button makes the retrieval much easier. 

Secondly, we have to iterate over every element within the return of document.querySelectorAll(".delete"). This is because the returned value is an array of object. This will change a previously assigned variable's innerHTML into unassigned up on the delete button being pressed.

So, now that we have an event listener for the employee being deleted, and also an additional action which re-assigns the previous name's innerHTMl into "Unassigned",  there is still additional code that needs to be run when the task is re-rendered, showing the changed name. 

```

                let name = document.createElement("p") 
                name.innerText = "Unassigned"
                const returnName = api.employeeName(jsonObj)
                .then(json => name.innerText = `Assigned to: ${json.name}`) 
```


The above code is a small part of a larger function that renders a single task. We can see that we are utilizing Javascripts fetch function, rendering the name from the Employee class. Should an employee not exist, such as after the user has deleted one, the return value is none. However, because of a previous variable assingment of name.innerHTML = "Unassigned", we've created an environment where, no matter what, the user will be able to see either the name of the employee, or see that a particular task is unassigned. 


