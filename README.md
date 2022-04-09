# WeatherLinkLiveUDP
Weewx Driver for The WeatherLink Live (WLL). It implements a HTTP interface for getting current weather data and can support continuous requests as often as every 10 seconds.
Also it collects a real-time 2.5 sec broadcast for wind speed and rain over UDP port 22222.
More information on the Davis Intruments website: [See Davis weatherlink-live-local-api](https://weatherlink.github.io/weatherlink-live-local-api/)

To see a live demo of this plugin in vist [meteo-otterlo.nl](https://meteo-otterlo.nl), it features the [Belchertown weewx skin](https://github.com/poblabs/weewx-belchertown#belchertown-weewx-skin) from [Pat O'Brien](https://github.com/poblabs) with a MQTT broker to display the 2.5 seconds wind and rain data.

### Installation


1) Download the driver

```
wget -O weatherlinkliveudp.zip https://github.com/grebleem/WeatherLinkLiveUDP/archive/master.zip
```

2) Install the driver

```
sudo wee_extension --install weatherlinkliveudp.zip
``` 

4) Set the `station_type` to `WeatherLinkLiveUDP` and modify the `[WeatherLinkLiveUDP]` stanza in `weewx.conf`.
```
[Station]

    # Set the type of station.
    station_type = WeatherLinkLiveUDP
```
If you have a separate wind transmitter, set up according to Davis Instruments recommendations: [How do I setup the weather link live to use a separate wind transmitter](https://support.davisinstruments.com/article/88ogxjf2mm-how-do-i-setup-the-weather-link-live-to-use-a-separate-wind-transmitter).
Where `wll_ip = 1.2.3.4` is the ip address of the WLL.
For logging extra senors just add the transition id to the stanza by adding `extra_id = x`, where x is the id. Just one sensor recording temperature and humidity is supported for now.
```
# The WLL can get dat from up to eight transmitters. If multiple transmitters e.g. extra ISS for wind, extra temp sensor, requires the lsid_iss
[WeatherLinkLiveUDP]
    wll_ip = 1.2.3.4
    poll_interval = 10              # number of seconds [minimal 10 sec.]
    driver = user.weatherlinkliveudp
```

4) Restart WeeWX

```
sudo systemctl stop weewx
sudo systemctl start weewx
```

Note: The driver requires the Python `requests` library. To install it:

```
sudo apt-get update 
sudo apt-get install python-requests
```
or us pip
```
pip install requests
```

### Extra Configuration

```
# The WLL can get dat from up to eight transmitters. If multiple transmitters e.g. extra ISS for wind, extra temp sensor, requires the lsid_iss
[WeatherLinkLiveUDP]
    wll_ip = 1.2.3.4
    poll_interval = 10              # number of seconds [minimal 10 sec.]
    driver = user.weatherlinkliveudp
    extra_id = 3                    # ID of the transmitter box
    txid_instromet_sun = 4          # ID of the transmitter box
```

* `extra_id`: ID of an extra transmitter box measuring temperature
  and humidity. Sets the observation types `extraTemp1` and
  `extraHumid1`.
* `txid_instromet_sun`: ID of an transmitter box connected to a
  sunshine duration sensor of Instromet Ltd. The sensor is
  connected to the rain input of the transmitter box and sends
  one tick every 36 seconds of sunshine. Sets the observation
  types `sunShineDur` and `daySunshineDur` (and `sunshine_time`
  for compatibility reasons)
