# Itura

A low-cost night-time ventilation and heating controller, simulated in C. It is meant for buildings whose walls store heat during the day and release it overnight: it pulls in cooler outside air to flush that heat out, and switches on a heater if the night turns cold.

**Status:** In progress, early stage (control logic carried over from v1; new features below are planned)

*Successor to [temperature-fan-controller](https://github.com/intrusion26/temperature-fan-controller), the fan-only v1.*

## The problem

Many homes in warm climates are built with heavy materials that absorb heat all day and re-radiate it after sunset, so rooms stay hot well into the night even once the outside air has cooled. Air conditioning solves this but costs a lot to buy and to run. Itura aims at a cheaper alternative: compare indoor and outdoor temperature, and ventilate only when outside air can actually help.

Itura is meant to improve night-time thermal comfort. It makes no medical claims.

## How it works

- **Ventilation interlock:** the fan runs only when outside air is cooler than the room by a set margin. A fan moves air; it doesn't cool it, so running it against hotter outside air would make things worse.
- **Heater with a deadband:** if the night gets cold, the heater runs in levels, and a deadband around the setpoint stops the heater and fan from ever working against each other.
- **Closed-loop control carried over from v1:** setpoint with diff-based speed levels, hysteresis, and rate-of-change response.
- **Sleep mode:** a cap on fan speed with gradual ramping, to keep noise low.
- **Safety:** an over-temperature cutoff, fail-safe defaults on sensor faults, and (in hardware) a thermal fuse in series with the heater, independent of the code.

## Architecture

The program is split into four jobs, so only the output stage changes as the project moves from simulation to hardware:

```
readSensors()   indoor temp, outdoor temp, battery voltage
decide()        all control logic and safety priority
applyOutputs()  fan level, heater level (printf now, PWM/relays later)
displaySystem() status for the user
```

Planned hardware is a controller-only "brain" box that drives a fan and a heater, powered by a small solar panel and battery (controller and fan only; the heater has its own supply). See the system blueprint: [blueprint link, add once published].

## Roadmap

- [ ] Restructure into the four-function architecture
- [ ] Outside temperature input and ventilation interlock
- [ ] Heater with levels, deadband, and over-temperature cutoff
- [ ] Sleep mode with gradual fan ramping
- [ ] Low-battery load shedding and fail-safe defaults
- [ ] Simulated night scenario with CSV logging and a chart
- [ ] Bench prototype (microcontroller, DC fan with PWM, two sensors, low-voltage heater stand-in)
- [ ] Bill of materials and cost comparison
- [ ] Solar and battery integration, using measured power draw

## Build and run

```bash
gcc itura.c -o itura
./itura
```

---
*Part of my embedded systems and IoT portfolio.*
