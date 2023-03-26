# Laptop Dataset Overview
I sought out data on laptops and their main performance components because I was considering buying a high end gaming laptop for when I travel

### [Repsoitory](https://github.com/calebpool96/Laptop-Dataset-Portfolio-)
Contains all files involved in the project

### Gathered Data From Kaggle
Laptop Data, GPU Specs, CPU Specs, & Video Game Requirements

(Source links)

[Laptop Data](https://www.kaggle.com/datasets/ehtishamsadiq/uncleaned-laptop-price-dataset)

[GPU Specs](https://www.kaggle.com/datasets/alanjo/graphics-card-full-specs)

[CPU Specs](https://www.kaggle.com/datasets/baraazaid/cpu-and-gpu-stats)

[Video Game Requirements](https://www.kaggle.com/datasets/baraazaid/pc-video-game-requirements-v2)

### Cleaned Tables in Google Sheets

#### [Laptop data](https://github.com/calebpool96/Laptop-Dataset-Portfolio-/blob/main/Laptops/Cleaned%20Tables/Laptop_Data.csv) 
*dropped all unnecessary, inaccurate, or empty rows and columns
*updated column names to make them easier to read
*created new column to create unique IDs for the laptop since their names weren’t included on the table 
*split ScreenResolution column in 2 (Screen, and Resolution)
*split manufacture and clock speed from cpu to match cpu specs table
*split storage columns with multiple storage drives into 2 columns

#### [GPU Specs](https://github.com/calebpool96/Laptop-Dataset-Portfolio-/blob/main/Laptops/Cleaned%20Tables/GPU_Specs.csv)
*dropped all unnecessary, inaccurate, or empty rows and columns
*updated column names to make them easier to read
*Included measurements in column names
*multiplied memory by 1000 to convert from GB to MB

#### [CPU Specs](https://github.com/calebpool96/Laptop-Dataset-Portfolio-/blob/main/Laptops/Cleaned%20Tables/CPU_Specs.csv)
*dropped all unnecessary, inaccurate, or empty rows and columns
*updated column names to make them easier to read
*split cores with 2 values into min and max cores if only 1 value present it is both min and max
*split clock with 2 values into min and max clock if only 1 value present it is both min and max
*split size ID (MHz or GHz) off clock, then sorted so I can multiply GHz by 1000 to convert it to MHz
*dropped nm from Process column and W from TDP column to change string to number

#### [Videogame Requirements](https://github.com/calebpool96/Laptop-Dataset-Portfolio-/blob/main/Laptops/Cleaned%20Tables/Videogame_Requirements.csv)
*dropped all unnecessary, inaccurate, or empty rows and columns
*updated column names to make them easier to read
*Included measurements in column names
*deleted duplicate rows
*Fixed “The Curse Of The Dead Gods” storage
*Trimmed size from columns and labeled in column name

### ERD Created in Google Docs 
After I cleaned up the columns I created an Entity Relationship Diagram by hand in google docs to help the analysis phase

<img src="https://user-images.githubusercontent.com/126192331/227752084-f903caf4-ad03-4bbd-aca6-08125d97d6ea.png" width="450" height="450">

