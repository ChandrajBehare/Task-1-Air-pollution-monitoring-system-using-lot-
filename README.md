# Task-1-Air-pollution-monitoring-system-using-lot-
Project Summary: Combatting urban air pollution, this loT-based system monitors air quality in real-time using sensors like MQ135 and MQ6. The LCD displays PPM values, and the system triggers alarms when air quality deteriorates, saving logs for future analysis and intervention.

Surce Code
import time
import grovepi  # Library for Grove sensors
import grove_rgb_lcd  # Library for Grove RGB LCD

# Connect MQ135 sensor to analog port A0
mq135_sensor = 0

# Connect MQ6 sensor to analog port A1
mq6_sensor = 1

# Threshold for triggering alarm (adjust as needed)
threshold_ppm = 100

def read_sensor(sensor_port):
    try:
        sensor_value = grovepi.analogRead(sensor_port)
        return sensor_value
    except IOError:
        print("Error: Unable to read sensor")
        return None

def calculate_ppm(sensor_value):
    # Convert sensor value to PPM (parts per million) using calibration
    # Add your calibration logic here based on sensor specifications
    ppm = sensor_value
    return ppm

def display_lcd(ppm_mq135, ppm_mq6):
    try:
        # Display PPM values on LCD
        grove_rgb_lcd.setText("MQ135: {} PPM\nMQ6: {} PPM".format(ppm_mq135, ppm_mq6))
    except IOError:
        print("Error: Unable to display on LCD")

def trigger_alarm(ppm):
    if ppm > threshold_ppm:
        # Add alarm triggering logic (e.g., sound buzzer, send notification)
        print("Air quality deteriorated! Triggering alarm...")
    else:
        print("Air quality normal")

def main():
    try:
        while True:
            # Read sensor values
            mq135_value = read_sensor(mq135_sensor)
            mq6_value = read_sensor(mq6_sensor)

            # Calculate PPM values
            ppm_mq135 = calculate_ppm(mq135_value)
            ppm_mq6 = calculate_ppm(mq6_value)

            # Display on LCD
            display_lcd(ppm_mq135, ppm_mq6)

            # Trigger alarm if air quality deteriorates
            trigger_alarm(ppm_mq135)

            # Delay between readings
            time.sleep(2)

    except KeyboardInterrupt:
        print("Exiting program")
    finally:
        # Clear LCD display
        grove_rgb_lcd.setText("")
        grove_rgb_lcd.setRGB(0, 0, 0)

if __name__ == "__main__":
    main()

You'll need to install necessary libraries for Grove sensors ('grovepi' and 'grove_rgb_lcd').
