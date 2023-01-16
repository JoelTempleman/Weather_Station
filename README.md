# Weather_Station
Cabin project

This script adds code for a wind speed indicator to the previous script. It sets up an additional GPIO pin for the wind speed indicator on a Raspberry Pi using the RPi.GPIO library. It also initializes new variables to keep track of the wind speed (wind_speed_count, wind_speed_sum). The script enters an infinite loop, constantly checking for a change in the wind speed input and summing the wind_speed. It also checks if 24 hours have passed since the last reset, and if so, it resets the rain gauge, calculates the average wind direction and average wind speed by dividing wind_dir_sum and wind_speed_sum by their respective counts and prints them out, then resets the wind_dir_count, wind_dir_


import RPi.GPIO as GPIO
import time

# Set up the GPIO pins for the rain gauge, wind direction sensor and wind speed indicator
rain_pin = 17
wind_dir_pin = 18
wind_speed_pin = 22
GPIO.setmode(GPIO.BCM)
GPIO.setup(rain_pin, GPIO.IN, pull_up_down=GPIO.PUD_UP)
GPIO.setup(wind_dir_pin, GPIO.IN, pull_up_down=GPIO.PUD_UP)
GPIO.setup(wind_speed_pin, GPIO.IN, pull_up_down=GPIO.PUD_UP)

# Initialize variables to keep track of the rain collected, wind direction, and wind speed
rain_collected = 0
wind_dir_count = 0
wind_dir_sum = 0
wind_speed_count = 0
wind_speed_sum = 0

# Set the time for the reset (24 hours)
reset_time = 86400

# Get the current time
start_time = time.time()

while True:
    # Check for a change in the rain gauge input
    if GPIO.input(rain_pin) == 0:
        rain_collected += 0.2794
        print("Rain collected: ", rain_collected, "mm")
    
    # Check for a change in the wind direction input
    wind_direction = GPIO.input(wind_dir_pin)
    wind_dir_sum += wind_direction
    wind_dir_count += 1
    
    # Check for a change in the wind speed input
    wind_speed = GPIO.input(wind_speed_pin)
    wind_speed_sum += wind_speed
    wind_speed_count += 1
    
    # Check if 24 hours have passed
    current_time = time.time()
    if current_time - start_time >= reset_time:
        # Reset the rain gauge, calculate the average wind direction and average wind speed
        rain_collected = 0
        average_wind_dir = wind_dir_sum / wind_dir_count
        average_wind_speed = wind_speed_sum / wind_speed_count
        wind_dir_count = 0
        wind_dir_sum = 0
        wind_speed_count = 0
        wind_speed_sum = 0
        print("Rain gauge reset")
        print("Average wind direction: ", average_wind_dir)
        print("Average wind speed: ", average_wind_speed)
        start_time = current_time
    time.sleep(0.01))
![image](https://user-images.githubusercontent.com/59157736/212748075-9cdd1cbe-5a8d-45f5-b578-e057ff8b23a5.png)
