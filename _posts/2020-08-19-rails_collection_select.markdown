---
layout: post
title:      "Rails Collection Select "
date:       2020-08-19 21:31:52 +0000
permalink:  rails_collection_select
---


Prior to the commencement of the project, collection_select seeemd like this mythical entity. The labs, in my opinion, didn't particularly explain the concept well, nor, for the first time,  was the Rails docmentation that enlighening either. 

I hope this blog post is able to shed the light for future students on collection_select, how it works, and how awesome it is! 

Within my various forms, I used the collection_select. Typically, you dont want to use it in isolation ; as part of a form, it makes it somewhat easier to handle the user's selection. 

Within my code, the syntax was as follows: 

```
<%=form_for @share do |f| %>
<%=f.label :Notional_Price %>
<%=f.text_field :price %><br> 
<%=f.label :Preference_Share?%>
<%=f.select :preference, ['Yes', 'No']%><br>
Which Stock Exchange will the share be listed on?
<%=f.collection_select :stock_exchange_id, StockExchange.all, :id, :name %><br>
<%=f.hidden_field :company_id, value: @company.id%>
<%=f.label :Pays_Dividend? %>
<%=f.select :dividend, ['Yes', 'No'] %><br>
<%=f.submit "Submit"%>
<%end%>
```

Encapsulating the collection_select within the creation of a form_for, guarantees us that the params that get's sent through to the controller is pre-formated, and that we only need to idenfity the approporirate params[:key] to be able to utilize it. 

Anyway, demyistifying the syntax: 
```
<%=f.collection_select :stock_exchange_id, StockExchange.all, :id, :name %><br>

```

stock_exchange_id => the identifier we get when we access the params in the controller. Think of this as the key. e.g. params[:stock_exchange_id]. 

StockExchange.all =>  We can imagine this as an StockExchange.all.each do |something|, and iterating through every example of an exisitng stock-exchange, and listing them out in a drop-down fashion. 

:id => this is the argument that get's passed within the params. So, in our controller, we will be able to use it by typing in params[:stock_exchange_id] ; the value we get is in the form of :id. 

:name => this is what the user will see on the front-end. 


