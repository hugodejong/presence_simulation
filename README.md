*Please :star: this repo if you find it useful*

# Presence Simulation

[![hacs_badge](https://img.shields.io/badge/HACS-Default-orange.svg)](https://github.com/custom-components/hacs)

This Home Assistant component aim to provide a presence simulation in your home while you are away. It will turn on & off lights, switches, covers... based on your history.

# How it works
It will look in the DB for the states history of all the entities configured in the component for a period corresponding to a `delta` variable defined in the component.
It will apply to the entites the same state (and some attributes like brightness and rgb_color) as it was `delta` days ago, in order to simulate your presence.
If the service is running longer than the number of days defined as the `delta`, the component will simply be reset and start over, until the stop service is called.

# Pre-requisit
The `history` integration must be activated - [which it is by default](https://www.home-assistant.io/integrations/history/). The period kept in the DB should be bigger than the delta used in the simulation. The default number of days kept is 10 and this [can be configured](https://www.home-assistant.io/integrations/recorder/) with the `recorder` integration.

# Installation
## Option 1
- In your Home Assistant configuration directory (`~/.homeassistant` for instance), create a directory `custom_components/presence_simulation` and put the code in it.
- Restart Home Assistant
## Option 2
- Go in your Home Assistant configuration directory (`~/.homeassistant` for instance)
- `git clone https://github.com/slashback100/presence_simulation.git`. It will create the directory `custom_components/presence_simulation`
- Restart Home Assistant
## Option 3 (recommended)
- Have [HACS](https://hacs.xyz/) installed, this will allow you to easily manage and track updates.
- Search for "Presence Simulation".
- Click Install below the found integration.
- Restart Home Assistant

# Configuration
* In the UI, go in Configuration > Integration
* Click on the '+' button
* Search for "Presence Simulation"
* Confirm:

<p align="center">
  <img src="/custom_components/presence_simulation/images/configFlow..png" width="400" alt="accessibility text">
</p>

* Set the group of entities to be used in the simulation. It can be a group of lights, switches, covers, light groups or of any component that can be turned on and off with the service `homeassistant.turn_on` and `homeassistant.turn_off`
* Set the number of days of history the simulation will use (the delta)
* Set the number for a scan interval used to switch entities in seconds. Default is 30 seconds. Warning, the smaller the number you choose, the more computing process the component will take.
* Choose to restore the states as they were before the end of ths simulation

You can edit these configurations afterwards by clicking on Options in the integration screen.

# Use it

The component will create an entity called `switch.presence_simulation`. This entity will be set to `on` when the simulation is running. `off` otherwise.
You have 2 ways of launching the simulation:
## With the switch
Toggling the `switch.presence_simulation` will toggle the presence simulation.
## With the services
Three services are available:
### Start the simulation
The service `presence_simulation.start` will start the simulation and set the `switch.presence_simulation` entity to `on`.
Optionally, you can reference a list of entities, a delta or choose to restore the states if you want to override the component configuration:
```
entity_id:
  - group.outside_lights
  - light.living_room
  - light.hall
delta: 5
restore_states: True
```
### Stop the simulation
The service `presence_simulation.stop` will stop the simulation and set the `switch.presence_simulation` entity to `off`.
### Toggle the simulation
The service `presence_simulation.toggle` will start or stop the simulation, depending on the current state of the `switch.presence_simulation` entity.
