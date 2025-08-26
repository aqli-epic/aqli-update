# Data description
The Air Quality Life Index provides **three different types of data** about air pollution, by year, at different geographic levels:
* The most important type (prefixed `who` in our data) converts air pollution concentrations (annual PM<sub>2.5</sub> in µg/m<sup>3</sup>) into tangible terms —its impact on life expectancy. It's the years of life expectancy that communities could gain if they reduce air pollution to the **World Health Organization (WHO)'s guideline** of 5 µg/m<sup>3</sup>.
* Similarly, when countries have a **national standard** —most often significantly higher than the WHO guideline— we translate the annual PM<sub>2.5</sub> levels to years of life expectancy gained if that national standard (prefixed `nat`) were met.
* And finally, the **PM<sub>2.5</sub> levels** themselves (prefixed `pm`), which are the basis of all our calculations.

---

We provide `CSVs` and `JSONs` in **three different administrative levels**:

* `gadm0` is the highest level of administrative division in the [Global Administrative Areas database](https://gadm.org/), generally representing the country level.
* `gadm1` is the second level of administrative division, representing subdivisions within a country such as states, provinces, or regions. 
* `gadm2` is the third level of administrative division, further subdividing the gadm1 regions into smaller areas like counties, districts, or municipalities.

---

We also provide two different flavors of **table presentations —wide and narrow**, since different data libraries, utilities, and software requiere different table formats. (And we know not everyone enjoys pivoting their tables.)

* The wide format looks something like: 

| id | name | population | natstandard | pm1998 | pm1999 | pm2000 | pm2001 | pm2002 | ... | nat1998 | nat1999 | nat2000 | nat2001 | nat2002 |
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| AFG | Afghanistan | 37461801 | 35 | 10.88 | 10.7 | 11.88 | 13.89 | 13.58 | ... | 0 | 0 | 0 | 0 | 0 |
| XAD | Akrotiri and Dhekelia | 24378 | NA | 11.74 | 11.67 | 13.73 | 12.67 | 10.96 | ... | NA | NA | NA | NA | NA |
| ALB | Albania | 3082234 | 25 | 16.86 | 15.43 | 17.33 | 16.42 | 18.07 | ... | 0 | 0 | 0 | 0 | 0 |
| ... | ... | ... | ... | ... | ... | ... | ... | ... | ... | ... | ... | ... | ... | ... | ... |

* While the narrow or long format looks something like:

| id | name | population | natstandard | year | type | value |
| --- | --- | --- | --- | --- | --- | --- |
| AFG | Afghanistan | 37461801 | 35 | 1998 | pm | 10.88 |
| AFG | Afghanistan | 37461801 | 35 | 1999 | pm | 10.7 |
| AFG | Afghanistan | 37461801 | 35 | 2000 | pm | 11.88 |
| AFG | Afghanistan | 37461801 | 35 | 1998 | who | 0.58 |
| AFG | Afghanistan | 37461801 | 35 | ... | ... | ... |
| AFG | Afghanistan | 37461801 | 35 | 1998 | nat | 0.56 |
| AFG | Afghanistan | 37461801 | 35 | ... | ... | ... |
| ... | ... | ... | ... | ... | ... | ... | ... |

> **Warning**: <br> The **narrow (a.k.a. long) formats** might be too long for *Google Spreadsheet* and *Excel*. You may lose rows if you use them to open or process these files.<br>The **gadm2 wide table** is also beyond *Google Spreadsheet's* 10- million cell limit.

You can tweak these outputs, in this [ObservableHQ notebook](https://observablehq.com/@fndvit/wide-and-narrow-formats-off-the-csvs).

---
## Codebook
Below is the codebook for the Air Quality Life Index defining what each variable means and what it measures:

| Column Name | Description | Format / Units |
| --- | --- | --- |
| **id{0,1,2}** | Unique identifier for the administrative area | Unique integers for GADM1 and 2, Alphabetic for GADM0 |
| **name{0,1,2}** | `name0` Country<br/>`name1` Second level<br/>  and `name` Feature name | Text |
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

## GADM Shapefiles
The underlying shapefiles for these gadm datasets can be found [here](https://uchicago.box.com/s/w0q0kmxhc1k2q5yuqexqgbd6ssjarg76). Refer point 3 of the **Issues and limitations** section that 
talks more about this.

---

## Sources
This is a summary of the data sources for the Air Quality Life Index. For a more in-depth dive, read the [AQLI methodology and research design](https://aqli.epic.uchicago.edu/about/methodology/). 

* The data for the annual average concentrations of PM<sub>2.5</sub> comes from a satellite-derived particulate pollution (PM<sub>2.5</sub>) concentration dataset constructed by [Donkelaar et al. (2021)](https://sites.wustl.edu/acag/datasets/surface-pm2-5/).
* The population data comes from https://landscan.ornl.gov/.
* The shapefiles are sourced from [GADM](https://gadm.org/download_world.html). Refer point 3 of the **Issues and limitations** section that talks more about this.
* The potential gain in life expectancy is calculated based on a formula by [Ebenstein et al (2017)](https://www.pnas.org/doi/full/10.1073/pnas.1616784114) as explained in the methodology.

---
## Issues and limitations
* Certain countries lack gadm level 2 (district/county/prefecture) region names, resulting in maximum available pollution data being at gadm level 1. Some countries lack both gadm level 1 and gadm level 2 names, reducing available data to gadm level 0 (country). Future data sources could fill in missing region names.

* PM2.5 data for regions above ~ 67.995 degree N, or anything below ~ 54.995 degree S is not available due to inherent limitations in the underlying satellite data retrieval.

* Currently, overlapping region boundaries occur as a result of rendering gadm shapefiles on the mapbox platform. To address this issue, we are aiming to enhance alignment and eliminate overlaps by incorporating mapbox boundaries product in our future versions of our platform.

* The pollution time series is fully recalculated (by the [Atmospheric Composition Analysis Group](https://sites.wustl.edu/acag/datasets/surface-pm2-5/)) due to improved modeling methods and expanded ground-level monitoring, which enhances the accuracy of satellite-derived PM2.5 data calibration. Consequently, pollution values for a specific region in a given year, such as 1998 (or any given year), might exhibit slight differences between the latest and previous datasets. Read more about this in the Methodology section of the website.

* The PM2.5 averages presented on the AQLI platform are averaged annually and hence variation in pollution within the year cannot be determined from the AQLI datasets.

* AQLI utilizes satellite-derived PM2.5 data with a resolution of 0.01 degree longitude x 0.01 degree latitude (~1 km x 1 km), which is summarized and displayed at gadm levels 0, 1, and 2 (country, state, and district). However, the platform currently lacks the capability to present data at the finer 1x1 km resolution. There are plans to potentially integrate this higher resolution functionality into the platform in the future.

* AQLI data purposely concentrates on a specific subset of satellite derived PM2.5 pollution - one that excludes mineral dust and sea salt. This data mainly represents pollution from human activities like vehicles, power plants, and industries, rather than natural sources. This approach aligns with the particulate pollution studied in Ebenstein et al. (2017), focusing on variations attributed to coal combustion differences. This targeted subset is more amenable to policy intervention, since it is human-generated. Additional details are available in the Methodology section of our website.

* In cases where tiny regions are located near or within water bodies, both population and/or pollution data might be unavailable due to limitations in data sources or challenges in deriving such information within our current data processing system. We plan to address these issues in future versions of the map.   
 
 
