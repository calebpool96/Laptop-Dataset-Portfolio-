# Laptop Dataset Overview
I sought out data on laptops and their main performance components because I was considering buying a high end gaming laptop for when I travel

### [Repsoitory](https://github.com/calebpool96/Laptop-Dataset-Portfolio-)
Contains all files involved in the project

## Collect:

### Gathered Data From Kaggle
Laptop Data, GPU Specs, CPU Specs, & Video Game Requirements

(Source links)

[Laptop Data](https://www.kaggle.com/datasets/ehtishamsadiq/uncleaned-laptop-price-dataset)

[GPU Specs](https://www.kaggle.com/datasets/alanjo/graphics-card-full-specs)

[CPU Specs](https://www.kaggle.com/datasets/baraazaid/cpu-and-gpu-stats)

[Video Game Requirements](https://www.kaggle.com/datasets/baraazaid/pc-video-game-requirements-v2)

## Clean:

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

## Analyze:

Started in PowerBI 

### PowerBI
[(Link to PowerBI report)](https://github.com/calebpool96/Laptop-Dataset-Portfolio-/blob/main/Laptop%20data%20POWER%20BI.pbix)

### MySQL
Made a local MySql server then linked it to PopSQL then uploaded the cleaned tables using MySQL Workbench then wrote queries to explore the dataset

```
SELECT 
    Resolution,
    Screen
FROM 
    laptops.laptop_data
ORDER BY 
    Resolution DESC
LIMIT
  10;
  
```
I started by looking for the best screen found in the dataset (The higher the resolution the better)

| Resolution | Screen                              |
|:----------:|:-----------------------------------:|
| 3840x2160  | IPS Panel 4K Ultra HD / Touchscreen |
| 3840x2160  | 4K Ultra HD / Touchscreen           |
| 3840x2160  | IPS Panel 4K Ultra HD               | 
| 3840x2160  | IPS Panel 4K Ultra HD               |
| 3840x2160  | IPS Panel Touchscreen / 4K Ultra HD |
| 3840x2160  | IPS Panel 4K Ultra HD               |
| 3840x2160  | IPS Panel 4K Ultra HD / Touchscreen |
| 3840x2160  | IPS Panel 4K Ultra HD / Touchscreen |
| 3840x2160  | 4K Ultra HD / Touchscreen           |
| 3840x2160  | 4K Ultra HD                         |

After a [Google Search](https://www.google.com) If IPS Panel is a good screen and the [top result](https://www.benq.com/en-us/knowledge-center/knowledge/how-to-choose-between-tn-va-and-ips-panels-for-the-games-you-play.html) States "IPS: the Finest Colors and Viewing Angles, Less Speed and Black Level Detail" so the best screen is the "IPS Panel 4K Ultra HD / Touchscreen" and the best resolution is "3840x2160"

```
SELECT
    CPU,
    Max_Cores,
    `Max_Clock(Mhz)`,
    `Process(NM)`
FROM
    laptops.cpu_specs
ORDER BY
    `Max_Clock(MHz)` DESC
LIMIT
    10;
```
I wanted to explore the CPU table to find the best CPU in the data set

| CPU             | Max_Cores | Max_Clock(Mhz) | Process(NM) |
|:---------------:|:---------:|:--------------:|:-----------:|
| Core i9-13900KS | 32        | 6000           | 10          |
| Core i9-13900KF | 32        | 5800           | 10          |
| Core i9-13900K  | 32        | 5800           | 10          |
| Ryzen 9 7950X3D | 32        | 5700           | 5           |
| Ryzen 9 7950X   | 32        | 5700           | 5           |
| Core i9-13980HX | 32        | 5600           | 10          |
| Ryzen 9 7900X   | 24        | 5600           | 5           |
| Core i9-13900   | 32        | 5600           | 10          |
| Core i9-13900F  | 32        | 5600           | 10          |
| Ryzen 9 7900X3D | 24        | 5600           | 5           |

I found that the "Core i9-13900KS" was the best in the CPU table; so my next step was writing a query to search for any laptops with the top CPU but it didnt return anything so I decided not to include it. 

```
SELECT
    LaptopID,
    GPU,
    `Storage(GB)`
FROM
    laptops.laptop_data
WHERE 
    Type LIKE '%Gaming%'
ORDER BY 
    `Storage(GB)` DESC
LIMIT 
    10;
```
I wanted to find the GPUs in the "Gaming" type laptop with the most Storage

| LaptopID | GPU                        | Storage(GB) |
|:--------:|:--------------------------:|:-----------:|
| 1205     | Nvidia GeForce GTX 1080    | 512         |
| 389      | Nvidia GeForce GTX 1050 Ti | 512         |
| 539      | Nvidia GeForce GTX 1050    | 512         |
| 897      | Nvidia GeForce GTX 1050 Ti | 512         |
| 1108     | Nvidia GeForce GTX 1060    | 512         |
| 1207     | Nvidia GeForce GTX 1060    | 512         |
| 205      | Nvidia GeForce GTX 1080    | 512         |
| 163      | Nvidia GeForce GTX 980M    | 512         |
| 190      | Nvidia GeForce GTX 1080    | 512         |
| 874      | Nvidia GeForce GTX 960     | 512         |

Nvidia (Like most companies in tech) usually get better performance the higher the number; but in some cases a lower "ti" can be more powerful than a higher number so I decided to make sure the "1080" was better than the "1050 Ti"

```
SELECT
    *
FROM
    laptops.gpu_specs
WHERE
    GPU LIKE '%Nvidia GeForce GTX 1080'
    OR 
    GPU LIKE '%Nvidia GeForce GTX 1050 Ti';
```

| GPU                        | Year_Released | Memory(MB) | Memory_bus(BIT) | Memory_Clock(MHz) | BUS          | Memory_Type | Chip  |
|:--------------------------:|:-------------:|:----------:|:---------------:|:-----------------:|:------------:|:-----------:|:-----:|
| NVIDIA GeForce GTX 1050 Ti | 2016          | 4000       | 128             | 1752              | PCIe 3.0 x16 | GDDR5       | GP107 |
| NVIDIA GeForce GTX 1080    | 2016          | 8000       | 256             | 1251              | PCIe 3.0 x16 | GDDR5X      | GP104 |

The "1080" has much better specs according to the table 

I wondered What percent of games can you play in the "Videogame_Requirements" table with a "Nvidia GeForce GTX 1080" GPU on the min settings.

```
SELECT
    Count(Name)/119*100
FROM
    laptops.Videogame_Requirements
WHERE
    `Min_GPU_Memory(MB)` < 8000
    AND
    `Min_GPU_Memory_Clock(Mhz)` < 1251;
```

It returned, 70% (ROUNDED from 69.7479)
(119 Games in table, divided returned value by total games then multiplied by 100 to get a percent)
(The values in the WHERE clause where pulled from the "1080"s specs on the GPU table)

This is when I realized that the CPU and GPU tables contain mostly desktop products so I looked up the specs of both returns in CPU/GPU queries to CPUs/GPUs in the Laptop Data table

Best CPU in a laptop with a "Nvidia GeForce GTX 1080" (The best GPU in the Laptop_Data Table)

```
SELECT
    CPU
FROM
    laptops.laptop_data
WHERE
    GPU LIKE '%Nvidia GeForce GTX 1080'
GROUP BY
    CPU;
```
"Core i7 7820HK" and "Core i7 6820HK"

I Compared both chips Specs on [Intels Website](https://www.intel.com/content/www/us/en/products/compare.html?productIds=97464,88969) and downloaded the table from the page then cleaned it in sheets, added it to the MySql database, and inserted the data into the cpu specs table.

```
INSERT INTO laptops.CPU_Specs
    (CPU, 
    Codename, 
    Min_Cores, 
    Max_Cores, 
    `Min_Clock(MHz)`,
    `Max_Clock(MHz)`, 
    Socket, 
    `Process(NM)`, 
    `TDP(Watts)`, 
    `Release`)
SELECT
    Processor_Number,
    Codename,
    Total_Cores,
    `Total_Threads(Max_Cores)`,
    `Processor_Base_Frequency(Clock_MHz)`,
    `Max_Turbo_Frequency(Max_Clock_MHz)`,
    Sockets_Suported,
    `Lithography(NM)`,
    `TDP(Watts)`,
    Launch_Date 
FROM
    laptops.cleaned_intel_comparison;
```
What percent of games can the "i7-7820HK" Play?

```
SELECT
    Count(Name)/119*100
FROM
    laptops.Videogame_Requirements
WHERE
    Min_CPU_Cores <= 4
    AND
    Min_CPU_TDP <= 45;
```
0.9% (Rounded from 0.8403)

The TDP in this cpu is too low for 99.1% of games according to the video game requirments table.
According to [Ace Cloud Hosting](https://www.acecloudhosting.com/blog/what-is-tdp-for-cpus-and-gpus/) "For a CPU, TDP is a measure of the maximum amount of heat that a CPU can generate and still perform within its specified temperature range." so I it is important to the laptop.
I want to find a CPU that can run atleast 50% of the games in the video game requirments table.

What are the Best CPUs in "Gaming" Type Laptops

```
SELECT  
    CPU,
    LaptopID
FROM
    laptops.laptop_data
WHERE 
    Type LIKE '%Gaming%'
ORDER BY  
    CPU DESC;
```
"Ryzen 1700", "FX 9830", 'Core i7 7820HK"

So I added these to a [benchmark](https://www.cpubenchmark.net/compare/2970vs2911vs2938/AMD-Ryzen-7-1700-vs-AMD-FX-9830P-vs-Intel-i7-7820HK) to see what was the best out of the 3 CPU and the "Ryzen 1700" Had the best mark but it did not exist in the CPU table so I added it in SQL by hand

```
INSERT INTO 
    laptops.cpu_specs (
        CPU, 
        Codename, 
        Min_Cores,
        Max_Cores, 
        `Min_Clock(MHz)`,
        `Max_Clock(MHz)`,
        Socket,
        `Process(NM)`,
        `TDP(Watts)`,
        `Release` 
)
VALUES(
    'Ryzen 7 1700',
    'Zen',
    '8',
    '16',
    '3000',
    '3700',
    'AM4',
    '14',
    '65',
    'Mar 2nd, 2017'
);
```

What "LaptopIDs" are "Gaming" type laptops and have an "Ryzen 1700" CPU and what their GPU(s)/RAM(GB)?
```
SELECT
    LaptopID,
    GPU,
    `RAM(GB)`
FROM
    laptops.laptop_data
WHERE
    CPU LIKE '%Ryzen 170%';
```

| LaptopID | GPU               | RAM(GB) |
|:--------:|:-----------------:|:-------:|
| 242      | AMD Radeon RX 580 | 16      |
| 226      | AMD Radeon RX 580 | 16      |
| 276      | AMD Radeon RX 580 | 8       |

Then I verified that the "AMD Radeon RX 580" exist in the GPU table

What percent of games can the "242" and "226" Laptops play

```
SELECT 
    COUNT(Name)/119*100
FROM
    laptops.videogame_requirements
WHERE
    Min_CPU_Cores <= (
        SELECT
            Max_Cores 
        FROM 
            laptops.cpu_specs
        WHERE
            CPU LIKE '%Ryzen 7 1700'
    ) AND
    Min_CPU_TDP <= (
        SELECT
            `TDP(Watts)` 
        FROM 
            laptops.cpu_specs
        WHERE
            CPU LIKE '%Ryzen 7 1700'
    ) AND   
    `Min_GPU_Memory(MB)` <= (
        SELECT 
            `Memory(MB)`
        FROM   
            laptops.gpu_specs
        WHERE
            GPU LIKE '%AMD Radeon RX 580'
     ) AND 
     `Min_GPU_Memory_Clock(Mhz)` <= (
        SELECT 
            `Memory_Clock(MHz)`
        FROM   
            laptops.gpu_specs
        WHERE
            GPU LIKE '%AMD Radeon RX 580'
     ) AND  
     `Min_RAM(GB)` <= 16;
 ```
 
 51% (Rounded from 51.2605)
 This reaches my Goal from earlier so we'll look at the other features of the laptops
 
| LaptopID | Company | Type   | Inches | Screen            | Resolution | CPU       | Ram(GB) | Storage(GB) | Gpu               | OS         |
|:--------:|:-------:|:------:|:------:|:-----------------:|:----------:|:---------:|:-------:|:-----------:|:-----------------:|:----------:|
| 242      | Asus    | Gaming | 17.3   | Full HD           | 1920x1080  | Ryzen 170 | 16      | 1,256       | AMD Radeon RX 580 | Windows 10 |
| 226      | Asus    | Gaming | 17.3   | IPS Panel Full HD | 1920x1080  | Ryzen 170 | 16      | 1,256       | AMD Radeon RX 580 | Windows 10 |

Between the two laptops the "226" has the better screen so that is the laptop I will be considering buying



















