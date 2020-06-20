---
layout: post
title:      "Controlling User Inputs and a Search Bar"
date:       2020-06-20 14:38:24 +0000
permalink:  controlling_user_inputs_and_a_search_bar
---


As part of any Employee database, it would be logical to have a search function ; being to figure out the division or the titile the colleague has is a useful and functional addition to any internal Employee site. 

There are several considerations in having a search bar : 

* How can we ensure consistency in the returned items from the search bar? How can I control if the user has typo for example or inputs data badly from the sign-up stage? 
* How to manage having the user being to search by Titles and by Division, without have two seperate search bars? 
* Refactor how the Employee would be able to sign-up to the site and controlling how they assigned their own titles and divisions. 

Starting from the moment the user signs-up, I refactored the code into drop-downs as shown below : 

```
<h1> Create log-in deatails </h1> 
<div>
<form action="/signup" method="post"> 
<input type="text" name="name" placeholder="username"><br>
<p> </p>
<input type="password" name="password" placeholder="Password"> <br>
<p> </p>
<select name="title" value="title">
<option value=""disabled selected>Title</option>
<option value="Managing Director"> Managing Director </option>
<option value="Executive Director"> Executive Director </option>
<option value="Vice-President"> Vice President </option> 
<option value="Associate"> Associate </option>
<option value="Analyst"> Analyst </option>
<option value="Intern"> Intern </option>
</select> <br>
<p> </p>

<select name="division" value="division">
<option value=""disabled selected>Division</option>
<option value="sales">Sales </option>
<option value="trading">Trading </option>
<option value="m&a">M&A </option>
<option value="developer">Developer </option>
</select><br>
<p> </p>
<input type="submit" value="create employee login details">
</form>
<div>

<div>
<p><%="Must include all the above information to be valid. Username also has to be unique." if @user && @user.errors.any?%></p>
</div>
```

Refactoring the code from `input type ="text" name = "division"` into the above ensures that whatever the user selects, from the back-end of my code, the correct assignment is always given.

This would be a momentual switch in perspective for me - having amended the way the user signs-ups to something that makes the search function consistently functional was huge. 

Having accomplished the above, and having ensured that every user would have the correctly spelled/ formatted title and division, the next challenge was to be able combine the search bar into one form, and not in two seperate forms. 

I approved this in the below manner: 

```
<form action="/search" method="post">
<select name="search" value="search">
<option value=""disabled selected>Search by the following</option>
<option value="Managing Director">Managing Director</option>
<option value="Executive Director"> Executive Director </option>
<option value="Vice-President"> Vice President </option> 
<option value="Associate"> Associate </option>
<option value="Analyst"> Analyst </option>
<option value="Intern"> Intern </option>
<option value="sales">Sales </option>
<option value="trading">Trading </option>
<option value="m&a">M&A </option>
<option value="developer">Developer </option>
</select>
<input type="submit" value="search"> <br>
</form>

```

The solution was to combine all the various elements that I wanted to search by, and call the `params[:search]` within my dashboard controller. 

Now that this was accomplished, the task of combining the searching parameters and being able to display it wtihin the erb page. 

I settled on the below code: 

```
post '/search' do 
    authenticate
    search_title = Employee.all.select{|a|a.title == params[:search]}
    search_division = Employee.all.select{|a|a.division == params[:search]}
    @user = search_title
    if @user != [] 
        @user = search_title 
    else
        @user = search_division
    end
    
    erb :"/user_page/search" 
    end 


```

		
This would recieve the post request from the search bar ; assigned different variables to the search_title and search_division, each one selecting the appropriate title or division ( if applicable) ; logically, both search_title and search_division cannot both return true because I only allowed the user to search by one variable. Using this, and usign the IF statement logic to throwaway if the returned value was nil, I was able to accomplish my goal. 
		
	
	
		
