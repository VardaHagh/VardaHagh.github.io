# My Personal Projects
## Duke Visualization Challenge

Recently, Scholars@Duke and The Graduate School of Duke University published datasets describing their faculty, publications, and PhD dissertation committees. They invited researchers and data scientists to participate in a [visualization challenge](https://rc.duke.edu/scholars-vis-challenge/) to explore questions like:

- Can faculty publications and PhD dissertation committees show how disciplines overlap?
- What can the makeup of dissertation committees tell us about trends in research?
- How many Duke faculty cross departmental and school boundaries in their publishing and teaching activities, and can these activities be visualized?
 
[Aghil](https://github.com/AghilZadeh) (my collaborator who is a graduate student at Duke) and I put together a poster that he presented at Duke Research Computing Symposium, and we received the third prize for this visualization. Here’s the poster we presented at the symposium (click on the picture to see in full size): 

<div align="center">
<img src="https://vgy.me/3LSkgv.png" alt="3LSkgv.png" height="500px">
</div> 
 Our Poster is now in Duke's Library and can be cited [here]( https://dukespace.lib.duke.edu/dspace/handle/10161/16026) .
 
There were three data files included: The first one has the entire faculty list, their unique Duke IDs, their appointment types, and the school or organizations they’re affiliated with. The next dataset includes all the publications, their authors, venues, and a few columns that could be used to identify and index them (like their SSN, Google Scholar URL, etc.). The third dataset includes information about Ph.D. dissertation committees. In this dataset, each row identifies a student with a computer generated unique ID. The columns include the school and department they’re in, their graduation year, and the Duke ID for all faculty members on their committees. In order to get insight into Duke’s collaborations and interdisciplinary research, we needed to combine the three datasets.
 
