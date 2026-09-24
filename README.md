# Daikibo-Telemetry-Down-Time-Analysis
# Daikibo Telemetry — Down Time Analysis

## Project description
Daikibo operates 4 factories — Daikibo Factory Meiyo (Tokyo, Japan), Daikibo Factory Seiko (Osaka, Japan), Daikibo Berlin (Germany), and Daikibo Shenzhen (China) — each running 9 types of machines that report a telemetry reading every 10 minutes. This project analyzes one month (May 2021) of that telemetry to answer two business questions for the client:

1. In which location did machines break the most?
2. What are the machines that broke most often in that location?

Each `unhealthy` reading is treated as 10 minutes of potential down time (one reporting interval). Down time is aggregated first by factory, then by device type within the worst-performing factory, using a calculated `Unhealthy` field (`10` when status is unhealthy, else `0`) — the same logic used to build the companion Tableau dashboard for this project.

## Dataset
- File: `daikibo-telemetry-data.json`
- Records: 160,704 telemetry messages
- Fields per message: `deviceID`, `deviceType`, `timestamp`, `location` (country, city, area, factory, section), `data` (status, temperature)
- Provided directly by the client as a single JSON export; not publicly hosted.

## Technologies used
- Python 3
- pandas — data loading, flattening, and aggregation
- matplotlib — down-time bar charts
- Jupyter Notebook

## Setup / run instructions
1. Create and activate a virtual environment (optional but recommended).
2. Install dependencies:
   ```
   pip install -r requirements.txt
   ```
3. Place `daikibo-telemetry-data.json` in the same folder as the notebook.
4. Launch Jupyter and run all cells:
   ```
   jupyter notebook Abhinav_Singh_Baghel_Daikibo_DownTime.ipynb
   ```
5. The notebook prints the down-time totals per factory and per device type, and saves two charts (`down_time_per_factory.png`, `down_time_per_device_type.png`) to the working directory.

## Key findings
- **Most down time:** Daikibo Factory Seiko (Osaka) — 480 minutes of unhealthy readings in May 2021.
- **Machine responsible:** LaserWelder — every minute of Seiko's down time came from this single device type; all other device types at Seiko were healthy all month.
- Runner-up factory: Daikibo Shenzhen (420 minutes), driven mainly by LaserCutter.

## Key information
- Only 103 of 160,704 readings (0.06%) were unhealthy — down time is a rare-event problem, not a systemic one.
- This notebook mirrors the Tableau dashboard built for the same dataset (factory chart used as a filter into the device-type chart).
