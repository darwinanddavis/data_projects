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

* Built a keyword search term bot for scraping data from literature search terms based on Web of Science results    
* Built a keyword search term bot for scraping PDF files for user-defined terms    
* Developed a host-parasite disease transmission model with biomass conservation and nutrient cycling to capture macroparasite burden on terrestrial ungulates (wild and livestock)   

**Outcomes**  

* Keyword query bot      
* Data scraper bot      
* NPSI model    

### Example outputs  
<br>    

******  

#### Keyword query bot 

The bot reads a `.txt` file containing literature entries resulting from a keyword search term query similar to a Web of Science database search. It then converts this file into a readable `.csv` file with each row as separate data, in this case, research articles. Using user-defined keyword search terms, it then scrapes the `.csv` file and returns a new file saved to the user's local hard drive with the final data entries containing the user-defined search terms. Users can define what part of the article they want to search, e.g. _Title_, _Author_, _Abstract_, etc.     

1. [Download the instructions for running the bot in `R`](https://github.com/darwinanddavis/LECWorkingGroup/raw/master/keyword_scrape/lec_keyword_search.pdf)    
2. [Download the model file (right click here and 'Save link as')](https://github.com/darwinanddavis/LECWorkingGroup/raw/master/keyword_scrape/lec_keyword_search.R?raw=true)      
3. Run the model in `R`. The final outputs will save to your local directory.      

> _keyword_final.csv_  
> _keyword_final_isolated.csv_


Keyword query bot `R` code  

```r
# required files
# LEC100testrecords.txt
# search_term_inputs.txt
# article_col_names.txt
# lec_keyword_search.R   

##########################################################################
##########################################################################
##########################################################################

# load packages (run once) ------------------------------------------------

install.packages("pacman")
require(pacman)
p_load(dplyr,purrr,readr)

# user inputs -------------------------------------------------------------

# set working dir
setwd("/Users/malishev/Documents/Emory/research/workshops/stl/journal_data")

# Step 1 ----------------------------------------------------------------------

# Enter either Title or Abstract to search for the keywords
extract1 <- "Title" 

# Step 2 ------------------------------------------------------------------

# Now enter what data you want to get out of the final results
# For example, if you want to know what year in which the resulting papers were published,
# type in "Year"
# use any search term you specified in the search_terms_input file
extract2 <- "Year"

##########################################################################
##########################################################################
##########################################################################

# run rest of code from here ----------------------------------------------
# read in file ----------------------------------------------------------

fh <- "LEC100testrecords.txt"
anomalies <- 1 # save csv file that   

ww <- readLines("search_term_inputs.txt") # set working dir
wd <- setwd(ww[1]) 
tt <- read.delim(paste0(wd,"/",fh),header=T,sep="\t")
tt %>% str
# set column names for df from file
df_colnames <- read.delim("article_col_names.txt",header=F,strip.white = T,sep=",",colClasses = "character")[1,] %>% as.character
colnames(tt) <- df_colnames

# view structure of data frame
tt %>% str
# randomly sample entries to see if contents align with the above col names
tt[sample(126,1),]

# key terms ---------------------------------------------------------------

keyterms <- read.delim("search_term_inputs.txt",header=F,skip=1,strip.white = T,sep=",",colClasses = "character")[1,] %>% as.character
keyterms <- keyterms %>% as.character ; keyterms_neat <- keyterms
keyterms <- paste(keyterms,collapse="|"); keyterms_neat # combine all the search terms 

final <- tt[grep(keyterms, tt[,extract1], ignore.case = T),] #
length(final[,extract1]) # get number of results
tt[final[,extract1],extract1] # show raw outputs 

### Remove NA columns  
rm_na <- grep("NA", names(tt), ignore.case = F)
final[,colnames(final[,rm_na])] <- list(NULL)

# extract isolated data col
ww <- as.list(ww); names(ww) <- ww
col_final <- ww[extract2] %>% as.character()
if(col_final!=""){final[,col_final]} # return as vector

# remove duplicate entries
final <- unique(final)

## Save output to file  
fho <- "keyword_final.csv"
write_csv(final,paste0(wd,"/",fho))

## Save isolated results data to file  
# final[col_final] # return as data frame column    
if(col_final!=""){
  final_isolated <- final[,col_final] # return as vector
  final_isolated <- as.data.frame(final_isolated); colnames(final_isolated) <- col_final
  fho_iso <- "keyword_final_isolated.csv"
  write.csv(final_isolated,paste0(wd,"/",fho_iso))
  cat(rep("\n",2),"Your results are saved as\n\n",fho_iso,"\n\n in","\"",wd,"\"","\n\n showing just",col_final,"data",rep("\n",2))
}

## Anomalies
### Here are the entries that contain the default term "<Go to ISI>" in the title column from the original data 
if(anomalies==1){
  gti <- "<Go to ISI>"
  search_func_gti <- grep(gti, tt[,"Title"], ignore.case = T)
  final_gti <- tt[search_func_gti,] #
  length(final_gti[,"Title"]) # get number of results
  tt[final_gti[,"Title"],"Title"] # confirm against raw data
  cat("Rows from original data where entries occur:\n",search_func_gti) # rows from original data where entries occur

  ### Remove NA columns  
  rm_na <- grep("NA", names(tt), ignore.case = F)
  final_gti[,colnames(final_gti[,rm_na])] <- list(NULL)
  
  ### Save anomalies to file
  fho_isi <- "keyword_final_anomalies.csv" # save these anomalies to local dir
  write.csv(final_gti,paste0(wd,"/",fho_isi))
  cat(rep("\n",2),"Your results are saved as\n\n",fho_isi,"\n\n in","\"",wd,"\"",rep("\n",2))
}

# final 
cat(rep("\n",2),"Your results are saved as\n\n",fho,"\n\n in","\"",wd,"\"","\n\n using the following search terms:\n\n",keyterms_neat,rep("\n",2),
"Total papers when searching",col2search,":",length(final[,col2search]))

```

**Results**

Raw data input: default Web of Science keyword search query (scroll right).       

![](meta1.jpg)

Output: Quiered data based on user search terms.  

Title 1               | Title 2               | Title 3               | Title 4
--------------------- | --------------------- | --------------------- | ---------------------
lorem                 | lorem ipsum           | lorem ipsum dolor     | lorem ipsum dolor sit
lorem ipsum dolor sit | lorem ipsum dolor sit | lorem ipsum dolor sit | lorem ipsum dolor sit

<br>  
******  
  
#### Data scraper bot  

The data scraper bot gleans data from PDF articles (`.pdf`) based on user defined search terms and returns a local file of meta-analysis data.  
  
Example search terms:  

> mortality, surviv*, fecund*, body condit*, body, body mass, feed*, feeding rate, feeding amount, waste, faec*, fece*, urin*, ecosystem, plant, soil, nutrie*  


Data scraper bot `R` code 
```r
# extract data from pdfs

### before running ###
# 1. delete first summary page of PDF if present
# 2. set search terms for each output 

#################################### load packages
packages <- c("pdftools","tidyverse","stringr") 
if (require(packages)) {
  install.packages(packages,dependencies = T)
  require(packages)
}
ppp <- lapply(packages,require,character.only=T); if(any(ppp==F)){
	cat("\n\n\n ---> Check packages are loaded properly <--- \n\n\n")
	}

#################################### set wd
wd <- "your working dir"
pdf_folder <- "folder where pdf files live"
setwd(wd)

colC <- list() # create empty list for relevance data (colC in LEC data extraction sheet)
unreadable_pdf <- list() # list for storing unreadable pdfs
colC_names <- c("PaperID","C")

# set col names for output 
rv_names <- paste(
  "Relevance",
  "Parasite type",
  "Response variable",
  "Effect variance",
  "Sample size",
  "P val",sep=",Line number,"
)

#################################### read in pdfs from dir  
f <- 1
file_list<-list.files(paste0(wd,"/",pdf_folder,"/")) # list files
file_list
pdf_list<-as.list(rep(1,length(file_list))) # empty list
for (f in 1:length(pdf_list)){ # read in pdfs
  p <- pdf_text(paste0(wd,"/",pdf_folder,"/",file_list[f])) # read pdf
  p <- read_lines(p) # convert to txt file
  pdf_list[[f]]<-p# save to master pdf list  
}
names(pdf_list) <- file_list # name list elements as paprer IDs
file_list # show files in dir
if(length(pdf_list)!=length(file_list)){cat("\n\n Check all pdfs in directory have been loaded \n\n")}

######################################## choose paper to scrape 
# loop through files in pdf dir 
nn <- 1

for(nn in 1:length(file_list)){ 
  fh <- file_list[nn]; fh 
  # only scrape PDFs with content that can be read
   if(pdf_list[fh][[fh]][1]==""){ # if the first line is empty
     nn <- nn + 1
     fh <- file_list[nn]
     unreadable_pdf[nn] <- fh # add troublesome pdfs to list to remove 
     unreadable_pdf <- unlist(unreadable_pdf); unreadable_pdf <- unreadable_pdf[!is.na(unreadable_pdf)]
   }
   cat(fh,"\n",nn,"\n")
   
  #################################### read in title or abstract terms
  title_abstract_terms <- as.character(read.delim("title_abstract_terms.txt",header=F,strip.white = T,sep=",",colClasses = "character")[1,])
  title_abstract_terms <- as.character(title_abstract_terms);title_abstract_terms_neat <- title_abstract_terms
  title_abstract_terms <- paste(title_abstract_terms,collapse="|");title_abstract_terms_neat # combine all the search terms 
  
  single_paper <- 1 # read in files from dir or individual papers
  if(single_paper==1){
    p1 <- pdf_list[[fh]] # read in individual paper 
  
    # ID paper, Nth line
    line_number <- 100
    p1[line_number] 
  }
  
  # isolate title and abstract in paper
  ta_length <- 1:50 # set number of lines for title and abstract in pdf
  p1_ta <- p1[ta_length] # first 80 lines of pdf 
  
  # remove whitespace
  str_squish(p1)
  
  ########################################    1. check whether title and abstract has key terms
  if(single_paper==0){
    for(kt in file_list){
      # this is for the master pdf
      relevance_return <- grep(title_abstract_terms,pdf_list[[kt]][ta_length],ignore.case = T) # scrape title and abstract for terms in protocol doc
      }
    }else{
      # this is for reading individual files
      relevance_return <- grep(title_abstract_terms,p1_ta,ignore.case = T) # scrape title and abstract for terms in protocol doc
    } # end single_paper == 0
    
  if(length(relevance_return)>1){ # if search terms return nothing, set data for that paper to NAs 
    relevance_final <- "YES"
  }else{
    relevance_final <- "MAYBE"}
  
  ########################################    2. scrape files for data  ########################################
  
  ######################################### parasite type
  nematode_terms <- as.character(c("nemat*","Nemat*","helmin*","Helmin*","cestod*","Cestod*","tremat*","Tremat*"));nematode_terms_neat <- nematode_terms
  nematode_terms <- paste(nematode_terms,collapse="|");nematode_terms_neat # combine all the search terms 
  
  #### choose paper
  p1_nematode <- pdf_list[[fh]] # read in individual paper 
  
  # this is for reading individual files
  nematode_return <- grep(nematode_terms,p1_nematode,ignore.case = T) # scrape title and abstract for terms in protocol doc
  nematode <- p1_nematode[nematode_return] # return terms in paper
  
  ######################################### response variable
  response_terms <- as.character(c("BCI","bci","body mass","bodymass","weig*","feedi*","feeding rate","uptak* rat*","mortal*","surviv*","defeca*","fecun*","urinat*","nutrie*","soil","plant biomass"));response_terms_neat <- response_terms
  response_terms <- paste(response_terms,collapse="|");response_terms_neat # combine all the search terms 
  
  #### choose paper
  p1_response_var <- pdf_list[[fh]] # read in individual paper 
  
  # this is for reading individual files
  response_var_return <- grep(response_terms,p1_response_var,ignore.case = T) # scrape title and abstract for terms in protocol doc
  response <- p1_response_var[response_var_return] # return terms in paper
  
  ######################################### effect size variance 
  effect_var_terms <- c("SE", "SD", "CI"); effect_var_terms_neat <- effect_var_terms
  effect_var_terms <- paste(effect_var_terms,collapse="|")
  
  p1_effect <- pdf_list[[fh]] # select paper
  
  effect_var_return <- grep(effect_var_terms,p1_effect,ignore.case = F) # scrape title and abstract for terms in protocol doc
  effect_var <- p1_effect[effect_var_return] # return all outputs
  # effect_var_final <- paste(unlist(effect_var), collapse='')# turn into one character string
  
  ######################################### sample size 
  n_val_terms <- c("n =", "n=", "N =", "N="); n_val_terms_neat <- n_val_terms
  n_val_terms <- paste(n_val_terms,collapse="|")
  
  p1_nval <- pdf_list[[fh]] # select paper
  
  nval_return <- grep(n_val_terms,p1_nval,ignore.case = F) # scrape title and abstract for terms in protocol doc
  nval <- p1_nval[nval_return] # return all outputs
  # pval_final <- paste(unlist(pval), collapse='')# turn into one character string
  
  
  ######################################### p value 
  p_val_terms <- c("p =", "p=", "p <", "p<", "p >", "p>","P =", "P=", "P <", "P<", "P >", "P>"); p_val_terms_neat <- p_val_terms
  p_val_terms <- paste(p_val_terms,collapse="|")
  
  p1_pval <- pdf_list[[fh]] # select paper
  
  pval_return <- grep(p_val_terms,p1_pval,ignore.case = F) # scrape title and abstract for terms in protocol doc
  pval <- p1_pval[pval_return] # return all outputs
  # pval_final <- paste(unlist(pval), collapse='')# turn into one character string
  
  ######################################## save output
  
  out_list <- list(relevance_final,relevance_return, nematode, nematode_return, response, response_var_return, effect_var, effect_var_return, nval, nval_return, pval, pval_return) # merge results into list
  n.obs <- sapply(out_list, length) # get number of obs for each element
  seq.max <- seq_len(max(n.obs)) # fill in missing number of cols to make below matrix the same length
  rv <- sapply(out_list, "[", i = seq.max) # turn into matrix
  rv <- as.data.frame(rv) # convert to df
  colnames(rv) <- rv_names # name cols 
  
  # write out to dir
  fout <- paste0(as.character(strsplit(fh,".pdf")),".csv") # create file name 
  write.csv(rv,fout) # save to dir 

} # end loop
unreadable_pdf # pdfs that can't be read 
```

**Results**    

Title 1               | Title 2               | Title 3               | Title 4
--------------------- | --------------------- | --------------------- | ---------------------
lorem                 | lorem ipsum           | lorem ipsum dolor     | lorem ipsum dolor sit
lorem ipsum dolor sit | lorem ipsum dolor sit | lorem ipsum dolor sit | lorem ipsum dolor sit

### Links    

[Project page on Github.](https://github.com/darwinanddavis/LECWorkingGroup)          

<br>  
<br>  

******  

[Back to top](#top)|[Home page](./index.md)
