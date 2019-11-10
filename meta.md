---
layout: page
title: Meta-analysis
permalink: /meta/
---
<a id="top"></a>

******  
<br>  

## Meta-analysis and disease transmission model development   
### Living Earth Collaborative Working Group on macroparasite impact on nutrient and biomass cycling in ecosystems     

### Snapshot  



### Location   
Living Earth Collaborative Center for Biodiversity Working Group     
Washington University    
St. Louis, MO, USA    

### People   

Amanda Koltz (co-organizer), Washington University in St. Louis, USA    
Rachel Penczkowski (co-organizer), Washington University in St. Louis, USA    
Sharon Deem (co-organizer), Institute for Conservation Medicine, St. Louis Zoo, USA    
Vanessa Ezenwa (co-organizer), University of Georgia, USA    
Susan Kutz, University of Calgary, Canada    
Brandon Barton, Mississippi State University, USA    
Zoe Johnson, Mississippi State University, USA    
Aimee Classen, University of Vermont, USA    
J. Trevor Vannatta, Purdue University, USA    
**Matt Malishev, Emory University, USA**    
David Civitello, Emory University, USA      
Daniel Preston, University of Wisconsin-Madison, USA    
Maris Brenn-White, St. Louis Zoo, USA    
  
### Tasks   

* Develop a keyword search term bot for scraping data from literature search terms based on Web of Science results    
* Develop a keyword search term bot for scraping PDF files for user-defined terms    
* Develop a host-parasite disease transmission model with biomass conservation and nutrient cycling to predict macroparasite burden on terrestrial ungulates (wild and livestock)   

**Outcomes**  

* Keyword query bot      
* Data scraper bot      
* NPSI model    

******    

### Keyword query bot 

The bot reads a .txt file containing literature entries resulting from a keyword search term query similar to a Web of Science database search. It then converts this file into a readable .csv file with each row as separate data, in this case, research articles. Using user-defined keyword search terms, it then scrapes the .csv file and returns a new file saved to the user's local hard drive with the final data entries containing the user-defined search terms. Users can define what part of the article they want to search, e.g. Title, Author, Abstract, etc.     

:one: [Download the instructions for running the bot in `R`](https://github.com/darwinanddavis/LECWorkingGroup/raw/master/keyword_scrape/lec_keyword_search.pdf)    
:two: [Download the model file (right click here and 'Save link as')](https://github.com/darwinanddavis/LECWorkingGroup/raw/master/keyword_scrape/lec_keyword_search.R?raw=true)      
:three: Run the model in `R`. The final outputs will save to your local directory.    

*keyword_final.csv*       
*keyword_final_isolated.csv*       

**Results**

Raw data input: default Web of Science keyword search query (scroll right).       

![](meta1.jpg)

Output: Quiered data based on user search terms.  

Title 1               | Title 2               | Title 3               | Title 4
--------------------- | --------------------- | --------------------- | ---------------------
lorem                 | lorem ipsum           | lorem ipsum dolor     | lorem ipsum dolor sit
lorem ipsum dolor sit | lorem ipsum dolor sit | lorem ipsum dolor sit | lorem ipsum dolor sit
lorem ipsum dolor sit | lorem ipsum dolor sit | lorem ipsum dolor sit | lorem ipsum dolor sit
lorem ipsum dolor sit | lorem ipsum dolor sit | lorem ipsum dolor sit | lorem ipsum dolor sit

### Project links  

[Link to Github page.](https://github.com/darwinanddavis/LECWorkingGroup)        

<br>  
<br>  

******    

[Back to top](#top) | [Home page](index)
