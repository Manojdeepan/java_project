package com.mycompany.weatherforecastapp;

import javax.swing.*;
import java.awt.*;
import java.awt.event.ActionEvent;
import java.awt.event.ActionListener;
import java.io.InputStreamReader;
import java.net.HttpURLConnection;
import java.net.URL;
import java.sql.Connection;
import java.sql.DriverManager;
import java.sql.PreparedStatement;
import java.sql.ResultSet;
import java.sql.SQLException;
import org.json.simple.JSONObject;
import org.json.simple.parser.JSONParser;

public class WeatherForecastApp {
    private static JTextField cityField;
    private static JLabel tempLabel, humidityLabel, windLabel, statusLabel;

    public static void main(String[] args) {
        SwingUtilities.invokeLater(WeatherForecastApp::createWeatherUI);
    }

    public static void createWeatherUI() {
        JFrame frame = new JFrame("Weather Forecast Viewer");
        frame.setDefaultCloseOperation(JFrame.EXIT_ON_CLOSE);
        frame.setSize(400, 400); 

        JPanel panel = new JPanel();
        panel.setLayout(new GridLayout(8, 2)); 

        JLabel cityLabel = new JLabel("Enter City:");
        cityField = new JTextField();
        JButton fetchButton = new JButton("Fetch Weather");
        JButton additionalInfoButton = new JButton("More Weather Info");
        JButton historyButton = new JButton("View History"); 
        tempLabel = new JLabel("Temperature:");
        humidityLabel = new JLabel("Humidity:");
        windLabel = new JLabel("Wind Speed:");
        statusLabel = new JLabel();

        fetchButton.addActionListener(e -> {
            String city = cityField.getText();
            if (!city.isEmpty()) {
                fetchWeather(city);
            } else {
                statusLabel.setText("Please enter a city name.");
            }
        });

        additionalInfoButton.addActionListener(e -> 
            JOptionPane.showMessageDialog(frame, "Additional weather info will be displayed here.",
                    "Additional Weather Info", JOptionPane.INFORMATION_MESSAGE));

        historyButton.addActionListener(e -> displaySearchHistory(frame));

        panel.add(cityLabel);
        panel.add(cityField);
        panel.add(fetchButton);
        panel.add(historyButton); 
        panel.add(tempLabel);
        panel.add(humidityLabel);
        panel.add(windLabel);
        panel.add(statusLabel);

        frame.add(panel);
        frame.setVisible(true);
    }

    private static void fetchWeather(String city) {
        try {
            String apiKey = "5ec545ec1894344681b86fadd9cf0c07"; 
            String urlString = "http://api.openweathermap.org/data/2.5/weather?q=" 
                    + city + "&appid=" + apiKey + "&units=metric";
            URL url = new URL(urlString);
            HttpURLConnection conn = (HttpURLConnection) url.openConnection();
            conn.setRequestMethod("GET");

if (conn.getResponseCode() == 200) {
    InputStreamReader reader = new InputStreamReader(conn.getInputStream());
    JSONParser parser = new JSONParser();
    JSONObject json = (JSONObject) parser.parse(reader);

    JSONObject main = (JSONObject) json.get("main");
    
    double temp = (double) main.get("temp");
    long humidity = (long) main.get("humidity");
    
    JSONObject wind = (JSONObject) json.get("wind");
    double windSpeed = (double) wind.get("speed");

    tempLabel.setText("Temperature: " + temp + "°C");
    humidityLabel.setText("Humidity: " + humidity + "%");
    windLabel.setText("Wind Speed: " + windSpeed + " m/s");
    statusLabel.setText("Weather data fetched successfully!");

    insertSearchHistory(city, String.valueOf(temp), String.valueOf(humidity), String.valueOf(windSpeed));
}
 else {
                clearWeatherData();
                statusLabel.setText("City does not exist!");
                JOptionPane.showMessageDialog(null, "City does not exist!", "Error", JOptionPane.ERROR_MESSAGE);
            }
        } catch (Exception e) {
            e.printStackTrace();
            clearWeatherData();
            statusLabel.setText("Error fetching weather data.");
        }
    }

    private static void insertSearchHistory(String city, String temperature, String humidity, String wind_speed) {
        String url = "jdbc:mysql://localhost:3306/weather_history"; 
        String user = "root"; 
        String password = ""; 

        try (Connection conn = DriverManager.getConnection(url, user, password)) {
            String sql = "INSERT INTO search_history (city_name, search_date, temperature, humidity, wind_speed) VALUES (?, NOW(), ?, ?, ?)";
            try (PreparedStatement pstmt = conn.prepareStatement(sql)) {
                pstmt.setString(1, city);
                pstmt.setString(2, temperature);
                pstmt.setString(3, humidity);
                pstmt.setString(4, wind_speed);
                pstmt.executeUpdate(); 
            }
        } catch (SQLException e) {
            e.printStackTrace();
        }
    }

    private static void clearWeatherData() {
        tempLabel.setText("Temperature:");
        humidityLabel.setText("Humidity:");
        windLabel.setText("Wind Speed:");
    }

private static void displaySearchHistory(JFrame frame) {
    StringBuilder history = new StringBuilder("Search History:\n");
    String url = "jdbc:mysql://localhost:3306/weather_history"; 
    String user = "root"; 
    String password = ""; 

    try (Connection conn = DriverManager.getConnection(url, user, password)) {
        String sql = "SELECT city_name, search_date, temperature, humidity, wind_speed FROM search_history ORDER BY search_date DESC";
        
        try (PreparedStatement pstmt = conn.prepareStatement(sql);
             ResultSet rs = pstmt.executeQuery()) {
             
            if (!rs.isBeforeFirst()) { 
                history.append("No search history available.");
            } else {
                while (rs.next()) {
                    String city = rs.getString("city_name");
                    String date = rs.getString("search_date");
                    String temperature = rs.getString("temperature");
                    String humidity = rs.getString("humidity");
                    String windSpeed = rs.getString("wind_speed");
                    
                    history.append(String.format("%s - %s | Temperature: %s°C, Humidity: %s%%, Wind Speed: %s m/s\n", 
                            city, date, temperature, humidity, windSpeed));
                }
            }
        }
    } catch (SQLException e) {
        e.printStackTrace();
    }

    JOptionPane.showMessageDialog(frame, history.toString(), "Search History", JOptionPane.INFORMATION_MESSAGE);
}

}
