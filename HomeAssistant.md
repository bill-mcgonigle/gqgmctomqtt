Once you have **gqgmctomqtt-remote.service** running, to add to your existing MQTT infrastucture in Home Assistant:

Go into **Settings/Apps/Mosquitto Broker**.
Add a user/password.

Add those to the config file here.  If your Mosquitto is wide open to any device on your network set the credential variables blank like:

```
mqtt-username=
mqtt-password=
```

Now add this to your configuration.yaml:

```
mqtt:
  sensor:
    - name: "Geiger Counter"
      state_topic: "sensors/radiation"
```

and restart Home Assistant.

Then go into **Settings / Devices & Services / Helpers**.

Create a template helper.

Give it a name like "Radiation Level".

Set the state like:

```
{% set payload = states("sensor.geiger_counter") | from_json(default={"cpm": "unknown"}) %}
{{ payload.cpm }}
```

Set **Unit of Measurement** to CPM.

Set **Availability** like:

```
{% set payload = states("sensor.geiger_counter") | from_json(default={"cpm": "unknown"}) %}
{{ payload.cpm != "unknown" }}
```

Now you can add a tile to a dashboard with a sensor or a chart (choose 'Radiation Level' as the entity).

You could think about adding an automation with a text-to-speech notification if the level exceeds a certain threshold for a certain amount of time.
