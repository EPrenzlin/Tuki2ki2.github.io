---
layout: post
title:      "A React Search Bar"
date:       2020-12-17 17:30:09 +0000
permalink:  a_react_search_bar
---


As with almost all my other Flatiron projects, I took on the mantle of implementing a search function. 

To get the ball rolling, there are several components that need to be present ; the component that provides the form in which the user can input information into, and a seperate component that will render the results back onto the page. 

Encapsulting this within a specific route let me to use the below code: 
```
    <div> 
    <SearchForm searchItem={this.props.searchItem}/>
    <SearchResult result = {this.props.result} refreshExemption={this.props.refreshExemption} deleteExemption={this.props.deleteExemption} /> 
    </div>
```

The searchForm component will handle the user inputting the information into our form. We are able to save this in state  using the following code inside the search form component: 

```
#within the searchform component ;

    state = {
        search:""
    }

handleChange = event => {
this.setState({search: event.target.value})
}

}
```

From the outset, we are setting state to an empty string ( " "). What the above code does is save into state every change that occurs. In other words, every letter or key that the user input into the form itself will be saved into state. 

```
onSearch = (event) => {
    event.preventDefault() 
    console.log("in the search") 
    this.props.searchItem(this.state.search) 
    this.setState({
        search:""
    })
		
		#within render/ return
		<form className ="text-center" onSubmit={this.onSearch}>

```

 
The above code fires out the search. Within the App component, we have various arrow functions that are able to handle the passing of the data between the components. Upon the form being submitted, we are firing off the this.props.searchItems arrow function, sending across our state as the argument. 

The actual arrow function itself looks like below: 
```
  export const searchItem = (search) =>{ 
  console.log("In the Search Item action", search) 
  return dispatch =>{
    return(dispatch({type:"SEARCH_FORM_STATE", search}))
  }
	
```

What this means in practise is that, upon the invokation of  searchItem, we are also hitting our dispatch, which sends plain objects to our reducer function. 

The SEARCH_FORM_STATE reducer looks as follows: 

```
    case 'SEARCH_FORM_STATE':
            console.log("in the search form reducer")
            const searchValue = action.search.toString().toUpperCase().replace(/ /g, "") 
            const foundItem = state.exemptions.filter(exemption =>  
                {
                const allValues = Object.values(exemption).map(exemption => exemption.toString().toUpperCase().replace(/ /g, ""))
                if(allValues.includes(searchValue))
                { return exemption }}
                )

            return{
                ...state, 
                exemptions:[...state.exemptions], 
                newExemptions:[...state.newExemptions], 
                searchResult: foundItem
            }
```

Within the reducer, we are changing the nature of the form input, making it into all caps, removing all spaces ; the purpose behind this is to make it easier to compare. 

The compared values are all held within state.exemptions, which hold all the current exemption information such as name, isin, and the stock market. Now, if you recall, the search form itself did not categorise what the user is searching for. Whilst this provides a cleaner look for the site, it requires some additional thinking on the coding front. 

To access all the values of an object, `Object.values(theValues)` needs to be invoked. However, within our example, we are searching for a specific value/ values within all the exemptions that we hold. Therefore, in order to be able to compare our searched item within all the exemptions, we have to create a new array of matched values, by iterating through all exemptions, accessing the values within one exemption, compare the values of the exemption( after removing all the spaces, and making it all caps) vs our searched item, it they are a match, we return the exemption value. 

We then return that in our searchResult key, which we can access within our app class through the below code: 
```
const mapStateToProps = state =>{
  return {
    exemptions: state.exemptions, 
    newExemptions: state.newExemptions,
    result: state.searchResult
  }
}
<SearchForm searchItem={this.props.searchItem}/>
 <SearchResult result = {this.props.result} refreshExemption={this.props.refreshExemption} deleteExemption={this.props.deleteExemption} /> 
	 

export default connect(mapStateToProps,{getExemptions, addExemption, deleteExemption, refreshExemption,searchItem})(App)


```

The return value of the reducer is set as result; we are passing that as a prop to our SearchResult container as `result ={this.props.result}` ; further, without our export connect, we are able to call the searchItem arrow function, pass that down as a prop to our searchForm to be invoked. 


