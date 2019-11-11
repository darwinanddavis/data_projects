---
layout: page
title: Time series
permalink: /time_series/
---
<a id="top"></a>

******  
<br>  

## Time series analysis of field temperature data identifies human-induced change to snail host habitats producing human schistosomes  

### Snapshot  

text

### Location  

Emory University Atlanta, USA

### People  

Seul Lee, Emory University, USA       
**Matt Malishev, Emory University, USA**    
Martina Laidemitt, NIH Fogarty International Center, USA  
David Civitello, Emory University, USA      

### Tasks   

* text 
* text       

### Outcomes    

* text    

### Example outputs 

**Method for wavelet analysis of time series data**  

Psuedocode for running wavelet
```
input parameters for wavelet function
analysis function
   [ site location
     time horizon ]
execute function
   [ read and clean temperature data
     set time horizon
     run wavelet analysis
     plot outputs ]
```
<br>

Wavelet analysis `R` code  
```r
packages <- c("WaveletComp","viridis")
if(require(packages)){install.packages(packages,dependencies = T)}
ppp <- lapply(packages,require,character.only=T)
if(any(ppp==F)){cbind(packages,ppp)
cat("\n\n\n ---> Check packages are loaded properly <--- \n\n\n")}

graphics.off()
par(las=1,bty="n",col=adjustcolor("steelblue",alpha=0.5))
# ---------------------------- constant/variable time period 
# when dt = 0.25, lower.period must equal 4 because lower period is 1/dt    
# X axis = time in temperature data
# Y axis = deconstructed time period (periodicity), e.g. days, weeks, month 

periodicity <- "constant" # constant or variable
# input data (an equal start and end period = constant time, i.e no change in periodicity)
start.period <- 1
dt <- # 4 =  
end.period <- 100
len <- 100 # length of time series
lowerPeriod <- 16 # lower period range (t) (16 (2^4) to 128 (2^7))
upperPeriod <- 128
djv <- 250 # number of octaves to determine the resolution along the period axis
ifelse(periodicity=="constant",end.period <- start.period, end.period <- end.period)

set.seed(12)
x <- periodic.series(1,100,100)
x <-periodic.series(start.period = start.period, 
	end.period = end.period, 
	length = len);x
# x <- x + 0.2*rnorm(1000) # add some noise 
par(mar=c(1,1,1,1))
plot(x,type="l")
my.data <- data.frame(x)
myw <- analyze.wavelet(my.data,"x",
                        loess.span = 0, # detrend time series
                        dt = dt, # define time limit (1 = 1/dt)
                        dj = 1/djv, # resolution (octave/suboctave)
                        lowerPeriod = lowerPeriod, # define time period
                        upperPeriod = upperPeriod,
                        make.pval = T, # show p vals
                        n.sim = 10 # region of significant periods
                        )

# plot wavelet analysis attributes
par(mfrow=c(1,2))
# period range on log scale (determined by dj)
plot(myw$Period,type="l",main="Log period range") 
# linear model against real part of complex values in Wave 
plot(myw$Wave,pch=".",col=adjustcolor("orange",0.5),main="Complex conjugate") 

# plot wavelet analysis
require(viridis)
require(sp)
n.levels <- 200 # number of levels for colour palette
colv <- sort(inferno(n.levels,0.1,0.9),decreasing=T)
par(mfrow=c(1,1))
wt.image(myw,
         color.key = "interval", # quantile
         plot.contour = F,
         plot.ridge = F,
         n.levels = n.levels, # colour levels
         color.palette = "colv",
         useRaster = T, # use raster or polygons
         legend.params = list(lab="Wavelet power levels",mar=5),
         label.time.axis = F,
         timelab = "Time",
         # date.tz = "", # timezone for dates. "" = local time
         # show.date = T,
         # date.format = "%Y-%m-%d" # format for calendar dates
         plot.coi = F
         )
```  
<br>

Other areas to apply this analysis:  

* Seasonal infection spikes    
* Physiologial reaction to drugs   
* Chemical spectral analysis  
* Geothermal activity/earthquake spikes  
* Decay rates of minerals/chemicals    

<br>       

![](img/time_series_sitelocs.png)  
###### Figure 1. Site locations for temperature probe data and _Biomphalaria_ habitats.  
<br>  

[wavelet1](time_series/time_series1.png)  
<br>  

[wavelet1](time_series/time_series2.png)          

<br>  
<br>  

******  

[Back to top](#top)|[Home page](./index.md)

