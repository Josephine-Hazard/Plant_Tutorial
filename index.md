<center><img src="{{ site.baseurl }}/tutheaderbl.png" alt="Img"></center>

To add images, replace `tutheaderbl1.png` with the file name of any image you upload to your GitHub repository.

### Tutorial Aims

#### <a href="#Long format"> 1. The first section</a>

#### <a href="#Summarize"> 2. The second section</a>

#### <a href="#Making a Graph"> 3. The third section</a>

You can read this text, then delete it and replace it with your text about your tutorial: what are the aims, what code do you need to achieve them?
---------------------------
We are using `<a href="#section_number">text</a>` to create anchors within our text. For example, when you click on section one, the page will automatically go to where you have put `<a name="section_number"></a>`.

To create subheadings, you can use `#`, e.g. `# Subheading 1` creates a subheading with a large font size. The more hashtags you add, the smaller the text becomes. If you want to make text bold, you can surround it with `__text__`, which creates __text__. For italics, use only one understore around the text, e.g. `_text_`, _text_.

# Subheading 1
## Subheading 2
### Subheading 3

Transform and summarize data frames 

GOAL: To transform a dataframe from wide to long format in order to summarize and graph the data
Raw data: Measurements of several plant functional traits (SLA, Leaf area, LDMC, Leaf fresh mass, and Leaf dry mass) on multiple individuals of four different species

You can get all of the resources for this tutorial from <a href="https://github.com/ourcodingclub/CC-EAB-tut-ideas" target="_blank">this GitHub repository</a>. Clone and download the repo as a zip file, then unzip it.

<a name="Long format"></a>

## Getting started 


First, open `RStudio`, create a new script by clicking on `File/ New File/ R Script` 
Then set the working directory and load some packages, for example `ggplot2` and `dplyr`. 

You can surround package names, functions, actions ("File/ New...") and small chunks of code with backticks, which defines them as inline code blocks and makes them stand out among the text, e.g. `ggplot2`.

```r
# Set the working directory
setwd("your_filepath")

# Load packages
library(ggplot2)
library(dplyr)
```

<a name="Summarize"></a>


## 1. Putting into long format 

```r
load("TraitData_CodingClub.RData")
```

Here you can add some more text if you wish.

```r
library(corrplot)
(correlation <- corrplot(cor(data[,2:6], use = "pairwise.complete.obs")))
```

And finally, plot the data:

```r
png(filename = "trait_correlation.png", width = 600, height = 600)
(correlation <- corrplot(cor(data[,2:6], use = "pairwise.complete.obs")))
dev.off()
```
## 2. Summarize the data 
Convert wide data to long for summarizing and graphing

```r
library(reshape2)

dlong <- melt(data, id = "SpeciesName", variable.name = "Trait")
```
# Summarize trait data to get mean, max, min, range and quantiles

```r
library(plyr)

dsumm <- ddply(dlong, c("SpeciesName","Trait"), summarise,
               mean = mean(value, na.rm = T),
               max = max(value, na.rm = T),
               min = min(value, na.rm = T),
               q2.5 = quantile(value, 0.025),
               q97.5 = quantile(value, 0.975),
               range = max-min)
```

## 3. Making a Graph

Graph raw trait data behind mean +/- 95% CI's and save the file
```r
(trait.plot <- ggplot()+
  geom_point(data = dlong, mapping = aes(x = SpeciesName, y = value, colour = Trait), alpha = 0.1) +
  geom_errorbar(data = dsumm, mapping = aes(x = SpeciesName, ymin = q2.5, ymax = q97.5, group = Trait), width = 0.3) +
  geom_point(data = dsumm, mapping = aes(x = SpeciesName, y = mean, group = Trait), size = 4, colour = "black") +
  geom_point(data = dsumm, mapping = aes(x = SpeciesName, y = mean, colour = Trait), size = 3) +
  facet_wrap(~Trait, scales = "free_y")+
  theme_classic() +
  scale_x_discrete(labels = c("Dryas", "Eriophorum", "Oxyria", "Salix")) +
  ylab("Trait Value") +
  xlab("Species"))
  ```
  We can save plots made using ggplot2 with ggsave, which is just one line of code
```r
ggsave(trait.plot, filename = "traits.png", height = 5, width = 10)
```

At this point it would be a good idea to include an image of what the plot is meant to look like so students can check they've done it right. Replace `traits.png` with your own image file:

<center> <img src="{{ site.baseurl }}/traits.png" alt="Img" style="width: 800px;"/> </center>

<a name="section1"></a>

## End 

This is the end of the tutorial. Summarise what the student has learned, possibly even with a list of learning outcomes. In this tutorial we learned:

##### - how to generate fake bivariate data
##### - how to create a scatterplot in ggplot2
##### - some of the different plot methods in ggplot2

We can also provide some useful links, include a contact form and a way to send feedback.

For more on `ggplot2`, read the official <a href="https://www.rstudio.com/wp-content/uploads/2015/03/ggplot2-cheatsheet.pdf" target="_blank">ggplot2 cheatsheet</a>.

Everything below this is footer material - text and links that appears at the end of all of your tutorials.

<hr>
<hr>

#### Check out our <a href="https://ourcodingclub.github.io/links/" target="_blank">Useful links</a> page where you can find loads of guides and cheatsheets.

#### If you have any questions about completing this tutorial, please contact us on ourcodingclub@gmail.com

#### <a href="INSERT_SURVEY_LINK" target="_blank">We would love to hear your feedback on the tutorial, whether you did it in the classroom or online!</a>

<ul class="social-icons">
	<li>
		<h3>
			<a href="https://twitter.com/our_codingclub" target="_blank">&nbsp;Follow our coding adventures on Twitter! <i class="fa fa-twitter"></i></a>
		</h3>
	</li>
</ul>

### &nbsp;&nbsp;Subscribe to our mailing list:
<div class="container">
	<div class="block">
        <!-- subscribe form start -->
		<div class="form-group">
			<form action="https://getsimpleform.com/messages?form_api_token=de1ba2f2f947822946fb6e835437ec78" method="post">
			<div class="form-group">
				<input type='text' class="form-control" name='Email' placeholder="Email" required/>
			</div>
			<div>
                        	<button class="btn btn-default" type='submit'>Subscribe</button>
                    	</div>
                	</form>
		</div>
	</div>
</div>
