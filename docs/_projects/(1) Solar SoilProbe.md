---
name: Solar SoilProbe
tools: [HomeAssistant, MySensorsNetwork, NRF24, cpp, circuitboard]
image: https://github.com/wesleygas/MYSNetwork_Nodes/blob/main/PlantStation/pictures/closed.jpeg?raw=true
description: Relay the soil humidity, temperature and the ligth intensity of your plants
---

# Solar SoilProbe

I've already lost count of how many times i've forgotten to water my plants and this is the main reason
this project was born. I needed some way of knowing how humid the soil was and how often I would need to
water them in order to have the perfect balance, no matter the weather.

## Sensors

The first sensor that came in mind was of course a soil sensor. To be precise, a capacitor soil sensor
to avoid dealing with galvanic corrosion.

![capacitive_soil_sensor](https://images.squarespace-cdn.com/content/v1/59b037304c0dbfb092fbe894/1592155129080-MCHMP39HUEW2QOZ5VJ7N/ke17ZwdGBToddI8pDm48kLkXF2pIyv_F2eUT9F60jBl7gQa3H78H3Y0txjaiv_0fDoOvxcdMmMKkDsyUqMSsMWxHk725yiiHCCLfrh8O1z4YTzHvnKhyp6Da-NYroOW3ZGjoBKy3azqku80C789l0iyqMbMesKd95J-X4EagrgU9L3Sa3U8cogeb0tjXbfawd0urKshkc5MgdBeJmALQKw/cap_soil_moisture_sensor_in_soil.JPG?format=1500w)

But that's the only precise thing about this sensor. Pretty
much anything **will** change the capacitance between it's plates and, consequently, it's output.


To counter at least one of the variables, I picked up an already encapsulated 10K NTC thermistor.
![ntc_thermistor](https://cdn.awsli.com.br/1000x1000/468/468162/produto/40134109/74419896e6.jpg)

The output of this specific thermistor, the MF58 is already well known, but in order to validate
I was really working with one of those, I tried to do some calibration using a thermocouple as
a control. The values and the linear regression done to get the approximated parameters are available
on a [jupyter notebook](https://github.com/wesleygas/MYSNetwork_Nodes/blob/main/PlantStation/NTCCalibration.ipynb) at the [MYSNetwork_Nodes](https://github.com/wesleygas/MYSNetwork_Nodes/blob/main/PlantStation) repository.

After calibrating the NTC, the only sensor missing is a LDR to measure the light intensity hitting the plant and, most importantly, the solar panel that is going to power all of this.

## Power 

I didn't want to be going out to charge this node every now and then, so I decided to use the case of an old solar powered LED for this job. 


To save as much power as possible, the microcontroller in use and the circuitry around it(in this case
an atmega328 on a arduino pro mini) needs to go through some modifications:

1. Desolder the green power LED and the voltage regulator.
2. Make sure to put the uC on Power-Save Mode. This is already taken care of by the MySGW library through
a ``sleep(time_ms, false)`` call. 

But still after this, the other sensors would still use power. To prevent that from happening, I joined all of them on a separate power rail in which the GND is controlled via an AO3401a N-Channel Mosfet that 
only gets turned on only at the time of sampling,

After that, I had to cram I all in the case and the board ended up looking like this

![final_board](https://github.com/wesleygas/MYSNetwork_Nodes/blob/main/PlantStation/pictures/overall.jpg?raw=true)


Now i can have all the data relayed back to my HomeAssistant dashboard:

![hass_dashboard](https://github.com/wesleygas/MYSNetwork_Nodes/blob/main/PlantStation/pictures/hass_dash.jpg?raw=true)