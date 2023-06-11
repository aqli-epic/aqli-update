# Data description
The Air Quality Life Index provides **three different types of data** about air pollution, by year, at different geographic levels:
* The most important type (prefixed `who` in our data) converts air pollution concentrations (annual PM<sub>2.5</sub>) into tangible terms—its impact on life expectancy. It's the years of life expectancy that communities could gain if they reduce air pollution to the **World Health Organization (WHO)'s guideline** of 5 µg/m<sup>3</sup>.
* Similarly, when countries have a **national standard** —most often significantly higher than the WHO guideline— we translate the annual PM<sub>2.5</sub> levels to years of life expectancy gained if that national standard (prefixed `nat`) were met.
* And finally, the **PM<sub>2.5</sub> levels** themselves (prefixed `pm`), which are the basis of all our calculations.

---

We provide `CSVs` and `JSONs` in **three different administrative levels**:

* `gadm0` is the highest level of administrative division in the [Global Administrative Areas database](https://gadm.org/), generally representing the country level.
* `gadm1` is the second level of administrative division, representing subdivisions within a country such as states, provinces, or regions. 
* `gadm2` is the third level of administrative division, further subdividing the gadm1 regions into smaller areas like counties, districts, or municipalities.

---

We also provide two different flavors of **table presentations —wide and narrow**, since different data libraries, utils, and software requiere different table formats. (And we know not everyone enjoys pivoting their tables.)

* The wide format looks something like: 

| objectid_gadm0 | iso_alpha3 | name0 | population | natstandard | pm1998 | pm1999 | pm2000 | pm2001 | pm2002 | ... | nat1998 | nat1999 | nat2000 | nat2001 | nat2002 |
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| 1 | AFG | Afghanistan | 37461801 | 35 | 10.88 | 10.7 | 11.88 | 13.89 | 13.58 | ... | 0 | 0 | 0 | 0 | 0 |
| 2 | XAD | Akrotiri and Dhekelia | 24378 | NA | 11.74 | 11.67 | 13.73 | 12.67 | 10.96 | ... | NA | NA | NA | NA | NA |
| 3 | ALB | Albania | 3082234 | 25 | 16.86 | 15.43 | 17.33 | 16.42 | 18.07 | ... | 0 | 0 | 0 | 0 | 0 |
| ... | ... | ... | ... | ... | ... | ... | ... | ... | ... | ... | ... | ... | ... | ... | ... |

* While the narrow or long format looks something like:

| objectid_gadm0 | iso_alpha3 | name0 | population | natstandard | year | type | value |
| --- | --- | --- | --- | --- | --- | --- | --- |
| 1 | AFG | Afghanistan | 37461801 | 35 | 1998 | pm | 10.88 |
| 1 | AFG | Afghanistan | 37461801 | 35 | 1999 | pm | 10.7 |
| 1 | AFG | Afghanistan | 37461801 | 35 | 2000 | pm | 11.88 |
| 1 | AFG | Afghanistan | 37461801 | 35 | 1998 | who | 0.58 |
| 1 | AFG | Afghanistan | 37461801 | 35 | ... | ... | ... |
| 1 | AFG | Afghanistan | 37461801 | 35 | 1998 | nat | 0.56 |
| 1 | AFG | Afghanistan | 37461801 | 35 | ... | ... | ... |
| ... | ... | ... | ... | ... | ... | ... | ... |

> ⚠️ **Warning**: <br> Description text here

<table>
  <td>⚠️ <b>Warning:</b> <br/>The **narrow (a.k.a. long) formats** might be too long for *Google Spreadsheet* and *Excel*. You may lose rows if you use them to open or process these files.<br/>The **gadm2 wide table** is also beyond *Google Spreadsheet's* 10- million cell limit.</td>
</table>

You can tweak these outputs, in this [ObservableHQ notebook](https://observablehq.com/@fndvit/wide-and-narrow-formats-off-the-csvs).

---
## Codebook
Below is the codebook for the Air Quality Life Index defining what each variable means and what it measures:

| Column Name | Description | Format / Units |
| --- | --- | --- |
| **objectid_gadm{0,1,2}** | Unique identifier for the administrative area | Unique integers |
| **iso_alpha3** | Three-letter country codes defined in ISO 3166-1 | Alphabetic |
| **name{0,1,2}** | `name0` Country<br/>`name1` Second level<br/>  and `name1` Third level admin name | Text |
| **population** | Total population of the administrative feature | People |
| **natstandard** | National standards for annual concentrations of PM<sub>2.5</sub>, in most cases, significantly higher than the WHO guideline | PM<sub>2.5</sub> in µg/m<sup>3</sup> |

The next columns follow a prefix (`pm`, `who`, `nat`) + year (`1998-2021`) structure:
| Column Name | Description | Format / Units |
| --- | --- | --- |
| **pm{1998-2021}** | `pm` PM<sub>2.5</sub> or fine particulate matter (air pollution levels) <br/>`0000` Year the data was recorded | PM<sub>2.5</sub> in µg/m<sup>3</sup> |
| **who{1998-2021}** | `who` Gain in life expectancy if the World Health Organization (WHO)'s guideline for annual average concentrations of PM<sub>2.5</sub> (5 µg/m<sup>3</sup>) is met <br/>`0000` Year the data was recorded | Years |
| **nat{1998-2021}** | `nat` Gain in life expectancy if the country's national standard is met<br/>`0000` Year the data was recorded | Years<br/>`NA` when not available |

---

For the data in **narrow format**, `type`, `year` and `value` represent what the last three rows in the previous table represented (`pm1998`, `who1998`, `nat1998` ...)

| Column Name | Description | Format / Units |
| --- | --- | --- |
| type | Type of measurement (`pm`, `who`, `nat`)<br/> `pm` PM<sub>2.5</sub> or fine particulate matter (air pollution levels in µg/m<sup>3</sup>) <br/>`who` Gain in life expectancy if the World Health Organization (WHO)'s guideline for annual average concentrations of PM<sub>2.5</sub> (5 µg/m<sup>3</sup>) is met <br/>`nat` Gain in life expectancy if the country's national standard is met | Categorical |
| year | Year the data was recorded | Year (YYYY) |
| value | Measurement value | `pm` PM<sub>2.5</sub> in µg/m<sup>3</sup> <br/>`who` Years <br/>`nat` Years |

---
## Sources
This is a summary of the data sources for the Air Quality Life Index. 
* The data for the annual average concentrations of PM<sub>2.5</sub> comes from a satellite-derived particulate pollution (PM<sub>2.5</sub>) concentration dataset constructed by [Donkelaar et al. (2021)](https://sites.wustl.edu/acag/datasets/surface-pm2-5/).
* The population data comes from https://landscan.ornl.gov/.
* The potential gain in life expectancy is calculated based on a formula by [Ebenstein et al (2017)](https://www.pnas.org/doi/full/10.1073/pnas.1616784114) as explained in the methodology.

For a more in-depth dive, read the [AQLI methodology and research design](https://aqli.epic.uchicago.edu/about/methodology/). 

---
## Issues and limitations
* TK TK
* TK TK
* TK TK
* TK TK