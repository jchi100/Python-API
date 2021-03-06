# Python-API

The project WeatherPy is to answer a fundamental question: "What's the weather like as we approach the equator?" 

With Jupiter Notebook, WeatherPy visualizes the weather of 500+ cities across the world of varying distance from the equator.  A Python library citypy is used for getting the name list of 500 cities based on randomly generated latitude and longitude pairs.  The OpenWeatherMap API is called to retrieve weather data of the selected world cities.

A series of scatter plots generated to showcase the following relationships:
•	Temperature (F) vs. Latitude
•	Humidity (%) vs. Latitude
•	Cloudiness (%) vs. Latitude
•	Wind Speed (mph) vs. Latitude

Steps:
•	Generate a dataset of latitude and longitude of at least 500 unique (non-repeat) cities.
•	Perform a weather check on each of the cities using a series of successive API calls.
•	Include a print log of each city as it's being processed with the city number, city name, and requested URL.
•	A CSV of all data retrieved and png images for each scatter plot are saved.

WeatherPyReport.docx describes of three observable trends based on the charts.

Tools:  Python, Pandas, Matplotlib and Seaborn libraries.


