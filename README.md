Weather Prediction System (Java Project)
Project Overview

This repository contains a desktop-based Java application that predicts and displays weather information for different cities. The system integrates the OpenWeatherMap API to fetch real-time weather data and uses a MySQL database to store search history. The application provides a simple and interactive Java Swing GUI for entering city names, viewing current weather conditions, and retrieving past search data.

Key Features

Weather Data Retrieval: Fetches temperature, humidity, and wind speed using the OpenWeatherMap API.

Graphical User Interface: Built with Java Swing for an intuitive user experience.

Search History Storage: Saves search history in a MySQL database with city name, date, and weather details.

Search History Retrieval: Displays past weather queries in a formatted dialog.

Error Handling: Handles invalid city inputs, failed API requests, and database connection issues.

Dashboard Preview

Repository Contents

WeatherForecastApp.java – Main application code.

search_history.sql – SQL script for creating the database table.

dashboard_screenshot.png – Screenshot of the application GUI

README.md – Project documentation.

Usage

Clone or download this repository.

Open the project in NetBeans or any Java IDE.

Configure a MySQL database using the provided SQL schema.

Add your OpenWeatherMap API key in the code.

Run the project and fetch weather data.

Tools and Technologies

Java (JDK 11)

Java Swing (GUI)

MySQL 5.7

JDBC (Database Connectivity)

OpenWeatherMap API

NetBeans IDE

Author: Manoj Deepan M
