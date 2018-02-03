# Personal Projects

## Duke Visualization Challenge

Recently, Scholars@Duke and The Graduate School of Duke University published datasets describing their faculty, publications, and PhD dissertation committees. They invited researchers and data scientists to participate in a [visualization challenge](https://rc.duke.edu/scholars-vis-challenge/) to explore questions like:

- Can faculty publications and PhD dissertation committees show how disciplines overlap?
- What can the makeup of dissertation committees tell us about trends in research?
- How many Duke faculty cross departmental and school boundaries in their publishing and teaching activities, and can these activities be visualized?
 
[Aghil](https://github.com/AghilZadeh) (my collaborator who is a graduate student at Duke) and I put together a poster that he presented at Duke Research Computing Symposium, and we received the third prize for this visualization. Here’s the poster we presented at the symposium (click on the picture to see in full size): 

<div align="center">
<img src="https://vgy.me/3LSkgv.png" alt="3LSkgv.png" height="500px">
</div> 


Our Poster is now in Duke's Library and can be cited. You can see the abstract [here](http://hdl.handle.net/10161/16026/).
 
There were three data files included: The first one has the entire faculty list, their unique Duke IDs, their appointment types, and the school or organizations they’re affiliated with. The next dataset includes all the publications, their authors, venues, and a few columns that could be used to identify and index them (like their SSN, Google Scholar URL, etc.). The third dataset includes information about Ph.D. dissertation committees. In this dataset, each row identifies a student with a computer generated unique ID. The columns include the school and department they’re in, their graduation year, and the Duke ID for all faculty members on their committees. In order to get insight into Duke’s collaborations and interdisciplinary research, we needed to combine the three datasets.

We first looked into the number of publications at each of the ten main schools per year from 2013 to 2016. Then we used polynomial regression to predict the numbers for 2017 and 2018. To our great surprise, there was a decline in the number of publication over the course of five years in 9 out of 10 cases. The only school that seemed to have an increasing number of publications per year is Duke Law School.  It turns out the publication data includes only those publications that were verified by the faculty, and of course the faculties have been spending less and less time on verifying these lists. The reason we don’t see this pattern in Duke Law School, is because they have a program manager that does the job. This was a big lesson we learned:

> Do not assume your dataset is complete, even if there are no obvious missing values!

We also wanted to show how different schools and disciplines are connected in terms of their coauthored publications and shared Ph.D. committees. 
Using the publication data and combining it with the faculty data, we assigned a publication to a Duke department or organization if at least one of the authors was primarily affiliated with that department or organization. Then by looking at the shared publications between different departments, we built an adjacency matrix where each row and column represent a school or department and the each element C[i,j] is proportional to the number of unique co-authored publications between schools i and j. 
To get an insight into interdisciplinarity of education at Duke, we built a similar matrix E.  In this matrix, the rows represent departments from which Ph.D. students have graduated since 2012, the columns represent the schools where Ph.D. committee advisors are affiliated, and each element E[i,j] is proportional to the number of Ph.D. students in department i, who have selected their committees from school j.  Both these matrices provide a pretty good measure of how different disciplines overlap.

All the analysis was conducted in Python, mainly using Pandas. After making the C and E matrices, we fed them into [Gephi](https://gephi.org) which is a great open source platform for graph visualization. The graphs can be seen in the poster, but we also have made interactive versions that can be found in the following links:

> [Interactive Graph for Interdisciplinary Collaborations at Duke](https://vfaghirh.github.io/Duke-Collaborations/)
> (see the HTML code [here](https://github.com/vfaghirh/Duke-Collaborations))

> [Interactive Graph for Interdisciplinary Education at Duke](https://vfaghirh.github.io/Duke-Education/)
> (see the HTML code [here](https://github.com/vfaghirh/Duke-Education))

[Here](https://github.com/vfaghirh/Duke-Project) you can find the IPython notebook for analyzing the collaborations and education data files plus the Gephi output files that you can download and play with. 


## Analyzing Insight Fellows

I’ve been interested in [Insight Data Science Fellowship](http://insightdatascience.com) for a while and I’m going to apply for their summer 2018 fellowship in Silicon Valley. This is a very prestigious and competitive program, so I wanted to know what my chances of getting into the program are. I did some web scraping to extract data from their [FELLOWS](http://insightdatascience.com/fellows) webpage and did some simple analysis. Here’s a simple explanation about the process:

First, I imported the needed libraries:

```
from bs4 import BeautifulSoup
from collections import OrderedDict
import json
import pandas as pd
import numpy as np
import requests
import matplotlib.pyplot as plt
%matplotlib inline
import seaborn as sns
sns.set()
```

I used BeautifulSoup for scraping the webpage:

```
url_to_scrape = 'http://insightdatascience.com/fellows'
r = requests.get(url_to_scrape)
soup = BeautifulSoup(r.text,"html.parser")
```

By inspecting the elements on the Insight webpage, I realized that fellows are stored in lists with 100 elements in each. So I took each list and extracted the info I needed out of them. I also made a dictionary with keys that I needed for collecting the information:

```
rosters = soup.findAll('div', class_="fellows_list w-dyn-list")
data = {'Name':[],'Title':[],'Company':[],'Project':[],'Background':[], 'Flag':[]}
```
The next step was to extract and assign the right values to each key. There was some missing data on fellows’ background, so I created some flags to filter out those entries that didn’t have any background info:
```
for i,roster in enumerate(rosters):
    fellows = roster.findAll('div', class_="w-clearfix w-dyn-items w-row")
    if len(fellows)!=0:
        fellows = fellows[0].findAll('div', class_='fellow_item w-dyn-item w-col w-col-2')
        for fellow in fellows[:]:

            exception = False
            
            try:
                name = fellow.find('div', class_="tooltip_name").text
                data['Name'].append(name)
            except:
                exception = True
                data['Name'].append('')
            
            title = fellow.find('div', class_="toottip_title").text
            data['Title'].append(title)
            
            try:
                company = fellow.find('div', class_="tooltip_company").text
                data['Company'].append(company)
            except: 
                exception = True
                data['Company'].append('')
            
            project = fellow.find('div', class_="tooltip_project").text
            data['Project'].append(project)
            
            background = fellow.find('div', class_="tooltip_background").text
            if len(background.split(','))==3:
                data['Background'].append(background)
            else:
                exception = True
                data['Background'].append(background)
            
            if exception:
                data['Flag'].append(1)
            else:
                data['Flag'].append(0)
```

 
