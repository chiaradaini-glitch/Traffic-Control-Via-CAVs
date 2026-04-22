# Traffic Control Via Connected Autonomous Vehicles (CAVs)

A MATLAB-based simulation framework for studying traffic flow optimization using connected autonomous vehicles. This project implements different control strategies (boundary control and model predictive control in centralized, decentralized, and partially centralized configurations) to improve traffic flow and reduce fuel consumption.

## Project Overview

This project explores the application of connected autonomous vehicles to optimize traffic flow based on macroscopic traffic models. The framework simulates vehicle dynamics, fuel consumption, and traffic density across different control architectures.

### Key Features

- **Multiple Control Strategies:**
  - Boundary Condition Control
  - Model Predictive Control (MPC) - Centralized
  - Model Predictive Control (MPC) - Decentralized
  - Model Predictive Control (MPC) - Partially Centralized

- **Simulation Components:**
  - Vehicle dynamics and positioning
  - Traffic density modeling
  - Fuel consumption calculation
  - Multi-lane traffic simulation
  - Velocity optimization

- **Analysis Tools:**
  - Average travel time computation
  - Density and velocity plots
  - Fuel consumption tracking
  - Trajectory visualization

## Project Structure

```
CAVs/
├── Boundary condition/          # Basic boundary condition control approach
│   ├── Simulator.m             # Main simulator for boundary condition scenarios
│   ├── av.m                    # Vehicle class definition
│   ├── godunov.m               # Godunov flux scheme implementation
│   ├── initial_datum.m         # Initial conditions setup
│   ├── boundaries_check.m      # Boundary conditions verification
│   └── ...                     # Supporting functions
│
├── MPC centralized/            # Centralized MPC control
│   ├── Simulator_TFC.m         # Total Fuel Consumption simulator
│   ├── MPC_TFC_duplex_centralized*.m  # MPC implementations
│   ├── Optimizer_TFC_*.m       # Optimization routines (Bayes, fmincon)
│   ├── speedOpt_TFC_*.m        # Speed optimization
│   └── ...                     # Supporting functions
│
├── MPC decentralized/          # Decentralized MPC control
│   ├── Simulator_TFC.m         # Total Fuel Consumption simulator
│   ├── MPC_TFC_duplex_decentralized*.m  # MPC implementations
│   ├── Optimizer_TFC_*.m       # Optimization routines
│   └── ...                     # Supporting functions
│
├── MPC partially centralized/  # Partially centralized MPC control
│   └── ...                     # Similar structure to centralized/decentralized
│
└── [PDF documents]             # Reference materials
    ├── partiallydec.pdf
    └── posizioneveicoli.pdf
```

## Getting Started

### Requirements

- MATLAB (version R2016b or later recommended)
- Optimization Toolbox (for MPC implementations)
- Statistics and Machine Learning Toolbox (for Bayesian optimization)

### Running Simulations

#### 1. Boundary Condition Approach

```matlab
cd('Boundary condition')
Simulator
```

This will run a traffic simulation with vehicles controlled at the boundaries. The script will:
- Initialize 4 vehicles at different positions and lanes
- Simulate traffic flow over the specified time horizon
- Generate density, velocity, and trajectory plots

#### 2. Centralized MPC

```matlab
cd('MPC centralized')
MPC_TFC_duplex_centralized_NOSTOP
```

This implements a centralized model predictive control strategy where a single controller optimizes velocities for all vehicles.

#### 3. Decentralized MPC

```matlab
cd('MPC decentralized')
MPC_TFC_duplex_decentralized_NOSTOP
```

This implements a decentralized control strategy where each vehicle optimizes its own velocity based on local information.

#### 4. Partially Centralized MPC

```matlab
cd('MPC partially centralized')
% Run the appropriate MPC script
```

## Core Components

### Vehicle Class (`av.m`)

Defines the connected autonomous vehicle object with properties:
- Position (`pos`) and velocity (`vel`)
- Lane assignment (`lane`)
- Vehicle parameters (max velocity, acceleration)
- Methods for updating vehicle state

### Traffic Model Functions

- **`godunov.m`** - Implements the Godunov flux scheme for traffic density computation
- **`flux.m`** - Computes traffic flux from density
- **`multigradient.m`** - Computes gradient for optimization
- **`initial_datum.m`** - Sets initial traffic density and vehicle positions
- **`boundaries_check.m`** - Enforces boundary conditions

### Optimization Functions

- **`Optimizer_TFC_bayes_centralized.m`** - Bayesian optimization for centralized control
- **`Optimizer_TFC_fmincon_centralized.m`** - Constrained optimization using fmincon
- **`speedOpt_TFC_*.m`** - Speed profile optimization

### Analysis Functions

- **`fuel_consumption.m`** - Calculates vehicle fuel consumption
- **`AverageTravelTime.m`** - Computes average travel time metrics
- **`position.m`** - Tracks vehicle positions
- **`velocity.m`** / **`vel.m`** - Tracks vehicle velocities

## Key Parameters

Simulation parameters are defined in the main scripts and include:

```matlab
par.V_max = 140;           % Maximum velocity (km/h)
par.Rho_max = 400;         % Maximum density (veh/km)
par.L = 50;                % Road length (km)
par.J = 250;               % Number of grid cells
par.lane_tot = 3;          % Number of lanes
par.Tf = 1;                % Final time (hours)
```

## Customization

### Changing Number of Vehicles

Edit the vehicle initialization section in the simulator (typically lines 26-36):

```matlab
v{1} = av(position, lane, type, velocity, label);
updatealpha(v{1}, par);
update(v{1}, par);
```

### Adjusting Simulation Time

Modify the `par.Tf` and time horizon parameters:

```matlab
par.Tf = 2;        % Final simulation time in hours
par.hor = 0.1;     % MPC horizon in hours
par.hor_tot = 0;   % Initial horizon time
```

### Changing Optimization Method

In the centralized/decentralized MPC scripts, switch between:
- `Optimizer_TFC_bayes_centralized.m` (Bayesian optimization)
- `Optimizer_TFC_fmincon_centralized.m` (Constrained optimization)

## Output and Visualization

The simulators generate:

1. **Density Plots** - Shows traffic density over time and space
2. **Velocity Plots** - Displays vehicle velocities and speed variations
3. **Trajectory Plots** - 2D visualization of vehicle paths colored by time
4. **Performance Metrics** - Total fuel consumption (TFC) and average travel time

Results are saved as `.mat` files containing:
- `TFC` - Total fuel consumption
- `pos` - Vehicle positions over time
- `yplot` - Vehicle y-coordinates (lanes)
- `Rhoplot` - Density distribution
- `Rho` - Traffic density matrix
- `t` - Time vector

## References

The research is based on:
- Macroscopic traffic flow models
- Model Predictive Control for traffic optimization
- Connected autonomous vehicle (CAV) coordination strategies

## Authors

- **Chiara Daini** - Main developer and researcher

## License

[Specify your license here]

## Notes

- **Boundary Condition Approach**: Suitable for scenarios with vehicles entering/exiting the network
- **Centralized MPC**: Optimal but requires complete communication and computational resources
- **Decentralized MPC**: Scalable solution with reduced communication requirements
- **Partially Centralized MPC**: Balance between optimality and scalability

## Contact

For questions or contributions, please contact the project maintainers.
