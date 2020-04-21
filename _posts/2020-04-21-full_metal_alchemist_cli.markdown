---
layout: post
title:      "Full Metal Alchemist CLI "
date:       2020-04-21 18:36:50 +0000
permalink:  full_metal_alchemist_cli
---


Full Metal Alchemsit is on my all-time favourite anime list - it brings out some unique storytelling with relateable characters in a grand epic adventure involving Alchemy, corruption, ethics, and actions. Given this, it was only natural that as part of my first Flatiron project  I would use this anime as the basis. 

Initially, the first difficulty came from narrowing down what exactly I wanted the user to do. Did I want something interactive or purely informative? Which areas of FMA would I be able to web-scrap and put into the CLI itself? I settled for scrapping all the known "State Alchemists" - a central tenants in the anime's story ( also because it features my favourite characters). 

Web-scraping was a challenge. Attempting to get the exact information that I wanted proved to be a battle  with my patience. I was greatly humbled and have a new appreciation for the hardships and skills required for web-scraping. 

From a coding challenge perspective, I hit a cross-road in the middle of my CLI build. My initial plan was for the user to be able to search for the"State Alchemists" via their names. However, as the code progressed, I found this to be un-user friendly. I turned to a simple number input for the user to select the respective Alchemist that they wanted some more info about. 

The initial code was akin to the below : 

```

def chapter_info(name_input) 
alchemist = Alchemist.all.detect{|a|a.name.inlcude?(name_input)
#this returns to us the, if in existence, the individual object of the Alchemist, which would input variables such as name_link, and a chapter_link
fma_wiki = open(alchemist.chapter_link) 
chapter_website = Nokogiri::HTML(fma_wiki) 
puts chapter_website.css("#content-text- p") 
end 

```

The difficlty was that the above relied upon a user to input a specific name - I wanted it to be based on a number input. 
Therefore, I had to reformat the accompying methods. I settled for the the below: 

```
def self.chapter_link(number) 
link = nil
    Alchemist.all.each_with_index do |alchemist,i|
    if i == number.to_i 
        link = alchemist.chapter_link
    end
end 
link  
end

```

This was to be used in conjuction with : 

```
def self.chapter_info(number)
input = number.to_i
link = self.chapter_link(number)
Webscraper.get_chapter_info(link)
end
```

Both working in tandem would alieviate my user-inputting a name issue. Given that, from the onset, the user would be selecting the character based on a numbered list, we could leverage off that, and, based on the users selection, utilise the inputted number to correspond to the @chapter_link variable embedded within the instance of that particular object by simply iterating over all the Alchemists, finding the inputted number (-1) to match. Then, assigning the @chapter_link to a variable aptly named link. 

The link then becomes the argument for the Webscraper class, which prints out the summary of the relevant chapter. 





