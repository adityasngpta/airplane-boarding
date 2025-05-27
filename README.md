# Airplane Boarding Simulation

A Python-based simulation system to compare different airplane boarding methods, specifically focusing on the efficiency comparison between Back-to-Front (BTF) and Window-Middle-Aisle (WMA) boarding strategies.

## Project Overview

This project implements a discrete event simulation to model passengers boarding a single-aisle aircraft. It was created to study whether the Window-Middle-Aisle boarding method is significantly faster than the traditional Back-to-Front method used by many airlines.

The simulation tracks:
- Passenger movement through the aircraft
- Seat assignment and seating actions
- Seat shuffles (when seated passengers must get up)
- Total boarding time based on configurable time constants

## Files Structure

- `airplane.py` - Defines the aircraft seating arrangement and seat shuffle tracking
- `passenger.py` - Models individual passengers with assigned seats and movement
- `queues.py` - Implements different boarding strategies (BTF, WMA, etc.)
- `simulation.py` - Controls the boarding process and collects statistics
- `example_simulations.ipynb` - Jupyter notebook for running and visualizing simulations

## Simulation Parameters

The simulation uses the following default parameters:
- Aircraft configuration: 30 rows with 6 seats per row (180 total passengers)
- Time constants:
  - 10 seconds per stop (passenger sitting down)
  - 10 seconds per seat shuffle (passenger getting up to let another pass)
  - 2 seconds to walk one row

## Boarding Methods Implemented

### Back-to-Front (BTF)
The traditional method where passengers board in groups from the rear of the aircraft toward the front. Within each row group, passengers board in random order.

### Window-Middle-Aisle (WMA)
An alternative method where all window seat passengers board first, followed by middle seats, and finally aisle seats, regardless of row. This minimizes seat shuffles but may require more walking.

## Running the Simulation

To run a basic simulation:

```python
from airplane import Airplane
from simulation import Simulation
from queues import BackToFront, WindowMiddleAisle

# Create an airplane with 30 rows and 6 seats per row
airplane = Airplane(number_of_rows=30, seats_per_row=6)

# Run a Back-to-Front simulation
btf_sim = Simulation(airplane, BackToFront, max_iter=10000)
btf_results = btf_sim.run()

# Reset and run a Window-Middle-Aisle simulation
airplane = Airplane(number_of_rows=30, seats_per_row=6)
wma_sim = Simulation(airplane, WindowMiddleAisle, max_iter=10000)
wma_results = wma_sim.run()

# Calculate boarding times
TIME_PER_STOP = 10
TIME_PER_SHUFFLE = 10
TIME_PER_ROW = 2

btf_time = (btf_results['number of stops'] * TIME_PER_STOP + 
            btf_results['number of seat shuffles'] * TIME_PER_SHUFFLE +
            btf_results['number of steps'] * TIME_PER_ROW)

wma_time = (wma_results['number of stops'] * TIME_PER_STOP + 
            wma_results['number of seat shuffles'] * TIME_PER_SHUFFLE +
            wma_results['number of steps'] * TIME_PER_ROW)

print(f"BTF boarding time: {btf_time/60:.2f} minutes")
print(f"WMA boarding time: {wma_time/60:.2f} minutes")
```

For more comprehensive simulation with statistical analysis, refer to the `example_simulations.ipynb` notebook.

## Statistical Analysis

The project includes statistical testing to compare boarding methods:
- 1000 simulation trials per method
- Two-sample t-test to compare mean boarding times
- 95% confidence intervals for the difference in boarding times

Key findings from the simulation:
- WMA method is significantly faster than BTF (approximately 16.5 minutes faster per flight)
- WMA completely eliminates seat shuffles (vs. ~70 shuffles for BTF)
- The time savings is statistically significant (p < 0.0001)

## Visualizations

The example notebook generates:
- Histograms of boarding times for each method
- Boxplots for comparison
- Tables of summary statistics

## Limitations

This simulation has some simplifications compared to real-world boarding:
- No modeling of carry-on luggage stowage
- No passenger variability (walking speed, groups traveling together)
- Perfect boarding group adherence
- No priority boarding or pre-boarding
- No first-class or business-class sections

## Future Work

The simulation framework can be extended to:
- Model carry-on luggage and stowage time
- Add passenger variability
- Simulate group travel scenarios
- Compare additional boarding methods
- Incorporate priority boarding
