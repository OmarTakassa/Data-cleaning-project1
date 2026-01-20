-- This is a safe copy to clean a table from actual base one 

SELECT *
FROM layoffs_staging;

-- STEP 1: Now i will start working removing duplicate
SELECT *,
    ROW_NUMBER() OVER(
          PARTITION BY company, location, industry, total_laid_off, percentage_laid_off, date, stage, country, funds_raised_millions
          ORDER BY company) AS row_num
    FROM layoffs_staging;

-- here im searching for rows that has a duplicate

WITH duplicate_cte AS
(
    SELECT *,
    ROW_NUMBER() OVER(
          PARTITION BY company, location, industry, total_laid_off, percentage_laid_off, date, stage, country, funds_raised_millions
          ORDER BY company) AS row_num
    FROM layoffs_staging
)
SELECT *
FROM duplicate_cte
WHERE row_num > 1;

-- And here im removing it Thanks to SQL server making it easy 

WITH duplicate_cte AS
(
    SELECT *,
    ROW_NUMBER() OVER(
          PARTITION BY company, location, industry, total_laid_off, percentage_laid_off, date, stage, country, funds_raised_millions
          ORDER BY company) AS row_num
    FROM layoffs_staging
)
DELETE
FROM duplicate_cte
WHERE row_num > 1;


-- Standardizing data

SELECT company, TRIM(company)
FROM layoffs_staging;

-- removing unwanted spaces
UPDATE layoffs_staging
SET company = TRIM(company);

-- Searching for something weird in industry and its good to check each column ok for now let start with industry
SELECT DISTINCT industry
FROM layoffs_staging
ORDER BY 1;

-- here i found something weird in crypto
SELECT *
FROM layoffs_staging
WHERE industry LIKE 'Crypto%';

-- and i fix it here
UPDATE layoffs_staging
SET industry = 'Crypto'
WHERE industry LIKE 'Crypto%';

-- searching for something weird in location now
SELECT DISTINCT location
FROM layoffs_staging
ORDER BY 1;

-- searching for something weird in country now
SELECT DISTINCT country
FROM layoffs_staging
ORDER BY 1;

-- Ahaaaaaaa I found something in US has exra . let me fix it
SELECT *
FROM layoffs_staging
WHERE country LIKE 'United States%';

SELECT country
FROM layoffs_staging
WHERE country = 'United States.';

-- and i fix it here
UPDATE layoffs_staging
SET country = 'United States'
WHERE country LIKE 'United States%';

-- now for total_laid_off
SELECT *
FROM layoffs_staging;

-- At firt i found that total_laid_off is nvarchar so i will convert to int

UPDATE layoffs_staging
SET total_laid_off = NULL
WHERE total_laid_off = 'NULL';

ALTER TABLE layoffs_staging
ALTER COLUMN total_laid_off INT;

--time to fix now
SELECT *
FROM layoffs_staging
WHERE total_laid_off IS NULL;

SELECT *
FROM layoffs_staging
WHERE industry IS NULL
OR industry = 'Null';

UPDATE layoffs_staging
SET industry = NULL
WHERE industry = 'NULL';

UPDATE layoffs_staging
SET industry = ''
WHERE industry IS NULL;

SELECT *
FROM layoffs_staging AS t1
JOIN layoffs_staging AS t2
ON t1.company = t2.company
AND t1.location = t2.location
WHERE t1.industry = NULL 
AND t2.industry != NULL;

UPDATE t1
SET t1.industry = t2.industry
FROM layoffs_staging AS t1
JOIN layoffs_staging AS t2
    ON t1.company = t2.company
WHERE t1.industry = '' 
  AND t2.industry <> '';


UPDATE layoffs_staging
SET industry = NULL
WHERE industry = '';


SELECT *
FROM layoffs_staging
WHERE company = 'Airbnb';

-- and now like in the prev problem in total_laid_off i found the same in funds_raised_millions and i will conv to float
UPDATE layoffs_staging
SET funds_raised_millions = NULL
WHERE funds_raised_millions = 'NULL';

ALTER TABLE layoffs_staging
ALTER COLUMN funds_raised_millions float;

SELECT *
FROM layoffs_staging
WHERE funds_raised_millions = '';

UPDATE layoffs_staging
SET funds_raised_millions = NULL
WHERE funds_raised_millions = '';

-- cleaning stage column
SELECT *
FROM layoffs_staging
WHERE stage = 'NULL';

UPDATE layoffs_staging
SET stage = 'Unknown'
WHERE stage = 'NULL';


-- STEP 3 I will delete the null in total_laid_off and percentage_laid_off because doesnt make any sense 

SELECT *
FROM layoffs_staging 
WHERE total_laid_off IS NULL 
AND percentage_laid_off IS NULL;

DELETE
FROM layoffs_staging 
WHERE total_laid_off IS NULL 
AND percentage_laid_off IS NULL;

SELECT *
FROM layoffs_staging;

-- Perfect and the Data is clean
