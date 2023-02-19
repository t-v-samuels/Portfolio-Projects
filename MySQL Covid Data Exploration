SELECT * FROM covid_deaths;
SELECT * FROM covid_vaccinations;

SELECT location, date, total_cases, new_cases, total_deaths, population
FROM covid_deaths
WHERE continent IS NOT NULL
ORDER BY 1,2;

#total cases v total deaths
#shows likelihood of dying from covid by country

SELECT location, date, total_cases, total_deaths, (total_deaths/total_cases)*100 AS Death_Percentage
FROM covid_deaths
WHERE location = 'United States'
ORDER BY 1,2;

#total cases v population
#what percentage of population has gotten covid

SELECT location, date, total_cases, population, (total_cases/population)*100 AS Covid_Percentage
FROM covid_deaths
#WHERE location = 'United States'
WHERE continent IS NOT NULL 
ORDER BY 1,2;

#countries with highest infection rate compared to population

SELECT location, population, MAX(total_cases) AS highest_infection, MAX((total_cases/population))*100 AS Percent_pop_infected
FROM covid_deaths
#WHERE location = 'United States'
WHERE continent IS NOT NULL
GROUP BY population, location
ORDER BY Percent_pop_infected DESC;

#countries with highest death count per population

SELECT location, MAX(total_deaths) AS total_death_count
FROM covid_deaths
#WHERE location = 'United States
WHERE continent IS NOT NULL
GROUP BY location
ORDER BY total_death_count DESC;

#analysis by continent

SELECT continent, MAX(total_deaths) AS total_death_count
FROM covid_deaths
#WHERE location = 'United States
WHERE continent IS NOT NULL
GROUP BY continent
ORDER BY total_death_count DESC;


#global numbers

SELECT date, SUM(new_cases) AS total_cases, SUM(new_deaths) AS total_deaths, SUM(new_deaths)/SUM(new_cases)*100 as global_death_percentage
FROM covid_deaths
#WHERE location = 'United States
WHERE continent IS NOT NULL
GROUP BY date
ORDER BY 1,2;

#total population v vaccination

#USE cte
WITH PopvVac (continent, location, date, population, new_vaccinations, rolling_ppl_vaccinated) AS
(
SELECT dea.continent, dea.location, dea.date, dea.population, vac.new_vaccinations, SUM(vac.new_vaccinations) 
OVER (PARTITION BY dea.location ORDER BY dea.location, dea.date) AS rolling_ppl_vaccinated
FROM covid_deaths dea
JOIN covid_vaccinations vac
ON dea.location = vac.location
AND dea.date = vac.date
WHERE dea.continent IS NOT NULL
#ORDER BY 2,3
)
SELECT * , (rolling_ppl_vaccinated/population)*100
FROM PopvVac;

#temp table

CREATE TEMPORARY TABLE percentpopulationvaccinated
(
Continent VARCHAR(250),
Location VARCHAR(250),
Date DATETIME,
Population INT,
New_vaccinations INT,
rolling_ppl_vaccinated BIGINT
);

INSERT INTO percentpopulationvaccinated
(
SELECT dea.continent, dea.location, dea.date, dea.population, vac.new_vaccinations, SUM(vac.new_vaccinations) 
OVER (PARTITION BY dea.location ORDER BY dea.location, dea.date) AS rolling_ppl_vaccinated
FROM covid_deaths dea
JOIN covid_vaccinations vac
ON dea.location = vac.location
AND dea.date = vac.date
WHERE dea.continent IS NOT NULL
#ORDER BY 2,3
);

SELECT *, (rolling_ppl_vaccinated/population)*100
FROM percentpopulationvaccinated;


CREATE TEMPORARY TABLE percentpopulationvaccinated
(
Continent VARCHAR(250),
Location VARCHAR(250),
Date DATETIME,
Population INT,
New_vaccinations INT,
rolling_ppl_vaccinated BIGINT
);

#temp table

INSERT INTO percentpopulationvaccinated
(
SELECT dea.continent, dea.location, dea.date, dea.population, vac.new_vaccinations, SUM(vac.new_vaccinations) 
OVER (PARTITION BY dea.location ORDER BY dea.location, dea.date) AS rolling_ppl_vaccinated
FROM covid_deaths dea
JOIN covid_vaccinations vac
ON dea.location = vac.location
AND dea.date = vac.date
WHERE dea.continent IS NOT NULL
#ORDER BY 2,3
);

SELECT *, (rolling_ppl_vaccinated/population)*100
FROM percentpopulationvaccinated;


#creating views for visualizations

CREATE VIEW percentpopulationvaccinated
AS
SELECT dea.continent, dea.location, dea.date, dea.population, vac.new_vaccinations, SUM(vac.new_vaccinations) 
OVER (PARTITION BY dea.location ORDER BY dea.location, dea.date) AS rolling_ppl_vaccinated
FROM covid_deaths dea
JOIN covid_vaccinations vac
ON dea.location = vac.location
AND dea.date = vac.date
WHERE dea.continent IS NOT NULL;
#ORDER BY 2,3

CREATE VIEW percent_pop_gotten_covid
AS
SELECT location, date, total_cases, population, (total_cases/population)*100 AS Covid_Percentage
FROM covid_deaths
#WHERE location = 'United States'
WHERE continent IS NOT NULL 
ORDER BY 1,2;

CREATE VIEW continent_total_death_count
AS
SELECT continent, MAX(total_deaths) AS total_death_count
FROM covid_deaths
#WHERE location = 'United States
WHERE continent IS NOT NULL
GROUP BY continent
ORDER BY total_death_count DESC;

CREATE VIEW infection_rate_v_population
AS
SELECT location, population, MAX(total_cases) AS highest_infection, MAX((total_cases/population))*100 AS Percent_pop_infected
FROM covid_deaths
#WHERE location = 'United States'
WHERE continent IS NOT NULL
GROUP BY population, location
ORDER BY Percent_pop_infected DESC;



