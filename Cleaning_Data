-- Data cleaning project #1

1. Remove duplicates
2. Standardize the data (grammar corrections, spelling, etc.)
3. Null values or blank values (fill them in where possible)
4. Remove any columns (situational)

-- CREATE a sandox to work with the RAW data. NEVER manipulate the source code.  
CREATE TABLE layoffs_staging
like layoffs;

select * from layoffs_staging;

insert layoffs_staging
select *
from layoffs;


-- 1. Remove duplicates

select *,
row_number() over(
partition by company, industry, total_laid_off, percentage_laid_off, `date`) as row_num
from layoffs_staging;


-- TEMP TABLE below
with duplicate_cte as
(
select *,
row_number() over(
partition by company, location, industry, total_laid_off, percentage_laid_off, `date`, country, funds_raised_millions) as row_num
from layoffs_staging
)
select *
from duplicate_cte 
where row_num >1;

select * 
from layoffs_staging
where company = 'Casper' or company ='Cazoo' or company='Hibob' or company='Wildlife Studios' or company='Yahoo'
order by 1;

-- Create another TABLE to delete the repeat rows.
CREATE TABLE `layoffs_staging2` (
  `company` text,
  `location` text,
  `industry` text,
  `total_laid_off` int DEFAULT NULL,
  `percentage_laid_off` text,
  `date` text,
  `stage` text,
  `country` text,
  `funds_raised_millions` int DEFAULT NULL,
  `row_num` int
) ENGINE=InnoDB DEFAULT CHARSET=utf8mb4 COLLATE=utf8mb4_0900_ai_ci;

select *
from layoffs_staging2;

insert into layoffs_staging2
select *,
row_number() over(
partition by company, location, industry, total_laid_off, percentage_laid_off, `date`, country, funds_raised_millions) as row_num
from layoffs_staging;

select *
from layoffs_staging;

select *
from layoffs_staging2
where row_num >1;

select * 
from layoffs_staging2
where company = 'Casper' or company ='Cazoo' or company='Hibob' or company='Wildlife Studios' or company='Yahoo'
order by 1;

delete
from layoffs_staging2
where row_num >1;

select *
from layoffs_staging2;

-- 2. Standardize the data (grammar corrections, spelling, etc.)

-- Removing unnecessary trim
select DISTINCT company, trim(company)
from layoffs_staging2; 

update layoffs_staging2
set company = trim(company);

select DISTINCT company, trim(company)
from layoffs_staging2; 

select distinct industry
from layoffs_staging2
order by 1;

select industry
from layoffs_staging2
where industry like 'Crypto%';

update layoffs_staging2
set industry = 'Crypto'
where industry like 'Crypto%';

select industry
from layoffs_staging2
where industry like 'Crypto%';

select DISTINCT country
from layoffs_staging2
order by 1;

update layoffs_staging2
set country = 'United States'
where country like 'United States%';

select DISTINCT country
from layoffs_staging2
where country like 'United States%'
order by 1;

-- Alternative way to clean up data ----------------------------------------------------------------------------
select DISTINCT country, trim(trailing '.' from country)
from layoffs_staging2
where country like 'United States%'
order by 1;

update layoffs_staging2
set country = trim(trailing '.' from country)
where country like 'United States%';
----------------------------------------------------------------------------------------------------------------

-- Change date column from text to date.

select `date`,
str_to_date(`date`, '%m/%d/%Y')
from layoffs_staging2;

update layoffs_staging2
set date = str_to_date(`date`, '%m/%d/%Y')
;

select date
from layoffs_staging2;

-- Permantly change the defintion of the date column from text to date. -------------------------------------
alter table layoffs_staging2
MODIFY column `date` date;
----------------------------------------------------------------------------------------------------------------

-- 3. Null values or blank values (fill them in where possible)
select *
from layoffs_staging2;

select *
from layoffs_staging2
where (total_laid_off is null or percentage_laid_off is null)
or (total_laid_off = '' or percentage_laid_off = '');

select * 
from layoffs_staging2
where total_laid_off is null 
and percentage_laid_off is null;

select *
from layoffs_staging2
where industry is null
or industry = '';

select * 
from layoffs_staging2
where company = 'Airbnb';
-- So far I have searched for null and blank values in each column of the table. Because there are so many in the two "laid_off" columns, let's begin with the "industry" column. We look over the "industry" column and then pick a specific company to see if it populated in another entry aside from the one marked null/blank value. After seeing that Airbnb's industry is populated as 'Travel' in another entry, the goal is to change the missing entry so that the data is complete and whole. First step is to change blank entries into null values. Then, using a self join command, update the table to get rid of the null values and populate those entries with the correct data.

update layoffs_staging2
set industry = null	
where industry = '';

select *
from layoffs_staging2
where industry is null;

select *
from layoffs_staging2 t1
join layoffs_staging2 t2
	on t1.company = t2.company
    and t1.location = t2.location
where (t1.industry is null)
and t2.industry is not null;

UPDATE layoffs_staging2 t1
join layoffs_staging2 t2
	on t1.company = t2.company
set t1.industry = t2.industry
where (t1.industry is null)
and t2.industry is not null;

select * 
from layoffs_staging2
where company = 'Airbnb';

select * 
from layoffs_staging2
where company like'Bally%';
-- Bally's didn't show up in the join clause because it only has one entry. There was no other entry to meet the requirements of 't2.industry is not null'. There was no other entry to be used to populate the null entry. It only has one entry, and it is null for the industry, i.e. we don't know what it's industry would be classified as. Similar to the situation with the industry of Bally's, there is too much unknown/missing information to fill in the 'laid_off' columns and 'funds_raised_millions' column. 
---------------------------------------------------------------------------------------------------------------------------

-- 4. Remove any columns (situational)
select *
from layoffs_staging2
where total_laid_off is null
and percentage_laid_off is null;

delete
from layoffs_staging2
where total_laid_off is null
and percentage_laid_off is null;

select *
from layoffs_staging2;

alter table layoffs_staging2
drop COLUMN row_num;

select *
from layoffs_staging2;





















































































