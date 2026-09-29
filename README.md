# HRFEST 2026-139 — Multi-Node Hospital Cold-Chain Monitoring Dataset and Analysis

This repository contains the experimental dataset, reproducible Google Colab analysis notebook, derived results, and figures associated with the paper:

> **Experimental Evaluation of a Multi-Node IoT Architecture for Real-Time Cold-Chain Monitoring in Hospital Environments**

The study evaluates the data-persistence continuity of three heterogeneous IoT monitoring nodes operating concurrently in a hospital cold-chain environment.

---

## 1. Study Overview

Three physical IoT monitoring nodes were evaluated concurrently in different hospital cold-chain applications:

| Node | Application |
|---|---|
| ULT26-001 | Plasma freezer |
| MHT2025-001 | Blood-storage refrigerator |
| BLD26-001 | Reagent/sample refrigerator |

The primary experimental analysis was performed over a common observation window from:

- **Start:** August 21, 2026, 19:07 UTC
- **End:** September 7, 2026, 15:35 UTC
- **Duration:** approximately 16.85 days
- **Nominal database-persistence interval:** 1 minute
- **Expected persistence slots per node:** 24,269
- **Total expected node-minute opportunities:** 72,807

The raw dataset also contains records generated during the initial physical installation and configuration of the monitoring nodes.

These records are preserved in the raw dataset for transparency but are excluded from the primary continuity analysis because the final one-minute database-persistence configuration had not yet been established during this preliminary phase.

---

## 2. Repository Structure

```text
hrfest2026-139/
│
├── CSV-raw-data/
│   └── bio_data_sub139_v2.csv
│
├── G-Colab/
│   ├── HRFEST_139.ipynb
│   │
│   └── Results/
│       ├── figures/
│       │
│       └── ...
│       │
│       ├── analysis_methodology.txt
│       ├── csv_structure_summary.csv
│       ├── daily_availability.csv
│       ├── daily_simultaneous_availability.csv
│       ├── derived_experimental_dataset.csv
│       ├── duplicate_slot_groups.csv
│       ├── duplicate_slot_rows.csv
│       ├── experimental_window_summary.csv
│       ├── gap_events.csv
│       ├── initial_configuration_phase.csv
│       ├── initial_configuration_summary.csv
│       ├── initial_phase_intervals.csv
│       ├── node_summary.csv
│       ├── normalized_raw_dataset_with_extra_fields.csv
│       ├── one_minute_availability_matrix.csv
│       ├── raw_temporal_ranges.csv
│       └── reviewer_ready_summary.txt
│
├── index.js
├── package.json
└── README.md
```

The `.idea/` directory contains local IDE configuration files and is not required for reproducing the experimental analysis.

---

## 3. Raw Experimental Dataset

The original experimental dataset used for the analysis is available at:

```text
CSV-raw-data/bio_data_sub139_v2.csv
```

The CSV contains records collected from the three monitoring nodes:

- `ULT26-001`
- `MHT2025-001`
- `BLD26-001`

The raw dataset is preserved in its exported form to support independent inspection and reproducibility.

Records outside the experimental observation window remain in the raw dataset but are not included in the primary persistence and availability calculations reported in the paper.

---

## 4. Experimental Observation Window

The primary analysis is restricted to the common three-node experimental observation window:

```text
2026-08-21 19:07 UTC
to
2026-09-07 15:35 UTC
```

This corresponds to approximately:

```text
16.85 days
```

Under the nominal one-minute database-persistence configuration, the observation window contains:

```text
24,269 expected one-minute slots per node
```

For three monitoring nodes, this corresponds to:

```text
72,807 expected node-minute opportunities
```

Records preceding the defined experimental start time correspond to the initial physical installation and configuration phase.

During this preliminary phase, the final one-minute persistence configuration had not yet been established. These records are retained in the raw dataset for transparency but are excluded from the primary continuity analysis.

---

## 5. Reproducible Analysis Notebook

The complete analysis workflow is provided in:

```text
G-Colab/HRFEST_139.ipynb
```

The notebook was developed for execution in **Google Colab**.

It performs the main processing and analysis steps required to reproduce the quantitative results reported in the paper.

The workflow includes:

- raw CSV structure inspection;
- timestamp parsing and normalization;
- monitoring-node identification;
- identification of the initial configuration phase;
- definition of the experimental observation window;
- generation of the nominal one-minute persistence grid;
- mapping of database records to nominal one-minute slots;
- individual-node persistence analysis;
- missing-slot identification;
- persistence gap-event detection;
- temperature-data completeness analysis;
- simultaneous three-node persistence analysis;
- simultaneous valid-temperature analysis;
- daily persistence availability;
- daily simultaneous availability;
- duplicate-slot verification;
- generation of derived experimental datasets;
- generation of summary tables;
- generation of figures used for experimental interpretation.

---

## 6. Analysis Definitions

### 6.1 Persistence Availability

Persistence availability is evaluated using the nominal one-minute database-persistence grid.

For each monitoring node:

```text
Persistence availability (%) =
    Stored one-minute slots
    ----------------------- × 100
    Expected one-minute slots
```

A nominal one-minute slot is considered **persisted** when at least one database record from the corresponding monitoring node is available within that nominal interval.

For the defined experimental window:

```text
Expected one-minute slots = 24,269 per node
```

---

### 6.2 Temperature-Data Availability

Temperature-data availability is evaluated independently from database persistence.

For each monitoring node:

```text
Temperature-data availability (%) =
    Slots containing a valid temperature value
    ------------------------------------------- × 100
             Expected one-minute slots
```

A persisted database record can therefore contribute to persistence availability even when its temperature field does not contain a valid numeric value.

This distinction allows database-persistence continuity and temperature-data completeness to be evaluated separately.

---

### 6.3 Persistence Gap Event

A **persistence gap event** is defined as one or more consecutive nominal one-minute slots for which no database record was persisted for a given monitoring node.

For each node, the analysis determines:

- total missing slots;
- number of persistence gap events;
- maximum consecutive missing-slot run;
- median inter-record interval;
- maximum inter-record interval.

---

### 6.4 Simultaneous Three-Node Persistence Availability

A nominal one-minute slot is considered simultaneously persisted when records from all three monitoring nodes are available within the corresponding interval.

Simultaneous persistence availability is calculated as:

```text
Simultaneous persistence availability (%) =
    Simultaneously persisted three-node slots
    ----------------------------------------- × 100
             Expected one-minute slots
```

---

### 6.5 Simultaneous Temperature-Data Availability

A nominal one-minute slot satisfies the simultaneous temperature-data criterion when all three monitoring nodes contain valid numeric temperature measurements for the corresponding interval.

This metric is evaluated separately from simultaneous database persistence.

---

## 7. Main Experimental Results

### 7.1 Individual-Node Persistence Availability

| Node | Expected slots | Stored slots | Missing slots | Persistence availability | Gap events | Maximum missing-slot run |
|---|---:|---:|---:|---:|---:|---:|
| ULT26-001 | 24,269 | 24,269 | 0 | 100.00% | 0 | 0 min |
| MHT2025-001 | 24,269 | 24,196 | 73 | 99.70% | 4 | 19 min |
| BLD26-001 | 24,269 | 24,170 | 99 | 99.59% | 4 | 31 min |

All three nodes achieved persistence availability above 99.5% during the defined experimental observation window.

---

## 8. Temperature-Data Availability

Temperature-data completeness was evaluated independently from database persistence.

| Node | Stored slots | Missing temperature values | Valid temperature slots | Temperature-data availability |
|---|---:|---:|---:|---:|
| ULT26-001 | 24,269 | 2 | 24,267 | 99.99% |
| MHT2025-001 | 24,196 | 52 | 24,144 | 99.48% |
| BLD26-001 | 24,170 | 215 | 23,955 | 98.71% |

These results demonstrate the importance of distinguishing a successfully persisted database record from a persisted record containing a valid temperature measurement.

---

## 9. Simultaneous Three-Node Results

Across the common observation window:

```text
Expected simultaneous slots:        24,269
Simultaneously persisted slots:     24,170
Simultaneous persistence:           99.59%
```

When valid temperature measurements from all three nodes were additionally required:

```text
Simultaneous valid-temperature slots: 23,901
Simultaneous temperature availability: 98.48%
```

Therefore:

- **Simultaneous three-node persistence availability:** 99.59%
- **Simultaneous three-node temperature-data availability:** 98.48%

---

## 10. Persistence Gap Events

No persistence gap events were identified for:

```text
ULT26-001
```

Four persistence gap events were identified for:

```text
MHT2025-001
```

and four for:

```text
BLD26-001
```

The consecutive missing-slot runs were:

### MHT2025-001

```text
18 min
19 min
18 min
18 min
```

Maximum consecutive missing-slot run:

```text
19 min
```

### BLD26-001

```text
25 min
25 min
31 min
18 min
```

Maximum consecutive missing-slot run:

```text
31 min
```

The four persistence gap events observed for MHT2025-001 and BLD26-001 began at the same timestamps.

This temporal concurrence indicates that the interruptions occurred concurrently at the two nodes. However, it does not by itself establish a common underlying cause.

Because subsystem-level diagnostic logs were not continuously recorded during the experimental period, the persisted dataset alone cannot uniquely attribute the missing slots to:

- the embedded sensing node;
- Wi-Fi connectivity;
- WebSocket communication;
- backend processing; or
- database persistence.

---

## 11. Generated Results

The directory:

```text
G-Colab/Results/
```

contains the intermediate and final outputs generated by the analysis notebook.

### `analysis_methodology.txt`

Text summary of the methodology used for processing and analyzing the experimental dataset.

### `csv_structure_summary.csv`

Summary of the raw CSV structure and parsing characteristics.

### `daily_availability.csv`

Daily persistence-availability results for each monitoring node.

### `daily_simultaneous_availability.csv`

Daily simultaneous three-node persistence availability.

### `derived_experimental_dataset.csv`

Dataset derived from the raw records after application of the experimental-window and analysis criteria.

### `duplicate_slot_groups.csv`

Summary of duplicate nominal persistence-slot groups identified during verification.

### `duplicate_slot_rows.csv`

Records associated with duplicate-slot verification.

### `experimental_window_summary.csv`

Definition and quantitative summary of the experimental observation window.

### `gap_events.csv`

Persistence gap events identified for each monitoring node.

### `initial_configuration_phase.csv`

Records associated with the initial installation and configuration phase.

### `initial_configuration_summary.csv`

Summary of the initial configuration phase.

### `initial_phase_intervals.csv`

Temporal intervals identified during the initial configuration phase.

### `node_summary.csv`

Main node-level persistence, missing-slot, gap-event, and availability results.

### `normalized_raw_dataset_with_extra_fields.csv`

Normalized representation of the raw exported dataset used during preprocessing and structural verification.

### `one_minute_availability_matrix.csv`

One-minute availability matrix used to evaluate individual and simultaneous persistence across the three monitoring nodes.

### `raw_temporal_ranges.csv`

Temporal ranges identified in the raw dataset.

### `reviewer_ready_summary.txt`

Compact summary of the principal experimental metrics obtained from the reproducible analysis.

---

## 12. Figures

Figures generated by the analysis notebook are stored in:

```text
G-Colab/Results/figures/
```

The generated visualizations include:

- persistent-data continuity across the experimental window;
- temporal distribution of missing one-minute persistence slots;
- individual-node persistence availability;
- temperature-data availability;
- daily node-level persistence availability;
- daily simultaneous three-node persistence availability.

The manuscript uses selected figures that provide complementary temporal information to the quantitative results reported in the tables.

---

## 13. Reproducing the Analysis

To reproduce the experimental analysis:

1. Clone or download this repository.

2. Open:

```text
G-Colab/HRFEST_139.ipynb
```

in Google Colab.

3. Make the raw dataset available to the notebook:

```text
CSV-raw-data/bio_data_sub139_v2.csv
```

4. Execute the notebook cells sequentially.

5. The notebook will process the raw dataset according to the defined experimental window and analysis methodology.

6. Compare the generated outputs with the files provided in:

```text
G-Colab/Results/
```

The repository contains both the original experimental dataset and the derived analysis outputs to facilitate independent inspection and reproduction of the reported results.

---

## 14. Monitoring-System Context

The experiment involved three heterogeneous ESP32-based IoT monitoring nodes.

The embedded firmware was implemented in **MicroPython**.

The common remote monitoring architecture included:

```text
Monitoring node
      ↓
     Wi-Fi
      ↓
  WebSocket
      ↓
Node.js backend
      ↓
   MongoDB
      ↓
Next.js web interface
```

The monitoring nodes shared the same remote communication and data-management infrastructure while using different sensing configurations according to their applications.

### ULT26-001

The plasma-freezer monitoring node used:

- dual ESP32 architecture;
- three-wire PT100 temperature sensor;
- MAX31865 RTD interface;
- DHT22 ambient temperature and relative-humidity sensor;
- MC-38 magnetic door sensor;
- local OLED display;
- UART communication between the two ESP32 microcontrollers.

### MHT2025-001 and BLD26-001

These monitoring nodes used:

- single ESP32 architecture;
- DS18B20 digital temperature sensor;
- DHT22 ambient temperature and relative-humidity sensor;
- local display.

All three nodes communicated with the remote backend through Wi-Fi and WebSocket connections.

Automatic reconnection logic was implemented following connectivity loss.

The evaluated nodes did not implement local data buffering or store-and-forward retransmission during the experimental period.

---

## 15. Backend

The backend source files included in this repository are:

```text
index.js
package.json
```

The backend is based on a Node.js architecture and supports WebSocket communication between the monitoring nodes and the remote data-management infrastructure.

These files are included to provide additional implementation context for the experimental monitoring architecture.

The primary reproducibility materials for the quantitative analysis reported in the paper are:

```text
CSV-raw-data/bio_data_sub139_v2.csv
G-Colab/HRFEST_139.ipynb
G-Colab/Results/
```

---

## 16. Data-Analysis Scope

The quantitative results reported in the associated paper are restricted to the predefined experimental observation window:

```text
2026-08-21 19:07 UTC
to
2026-09-07 15:35 UTC
```

Records outside this window may remain in the raw dataset but are outside the scope of the primary experimental analysis.

The repository preserves these raw records to maintain transparency and allow independent inspection of the complete exported dataset.

---

## 17. Limitations

The dataset and analysis should be interpreted within the scope of the experiment.

The evaluation involved:

- three physical monitoring nodes;
- one hospital deployment;
- a 16.85-day common observation window;
- a nominal one-minute database-persistence configuration.

Subsystem-level diagnostic logs were not continuously collected during the experiment.

Therefore, persistence gaps identified from the database records cannot be uniquely attributed to individual components of the end-to-end monitoring path.

In addition, the evaluated nodes did not implement local buffering or store-and-forward retransmission.

The repository consequently supports reproducibility of the observed persistence and temperature-data metrics but does not provide evidence for causal attribution of individual communication interruptions.

---

## 18. Associated Publication

This repository accompanies the manuscript:

> **I. Lon-Kan et al., “Experimental Evaluation of a Multi-Node IoT Architecture for Real-Time Cold-Chain Monitoring in Hospital Environments,” 2026.**

Final bibliographic information will be added after publication.

---

## 19. Citation

If you use the dataset, analysis workflow, or derived results provided in this repository, please cite the associated publication and this repository.

A complete citation will be added after final publication.

---

## 20. Authors

This repository was prepared as supporting material for the experimental study:

**Experimental Evaluation of a Multi-Node IoT Architecture for Real-Time Cold-Chain Monitoring in Hospital Environments**

Author and affiliation information is available in the associated manuscript.

---

## 21. License

The dataset, source code, and associated analysis materials are provided for academic and research purposes.

Please refer to the repository `LICENSE` file for the applicable terms of reuse.

---

## 22. Research Reproducibility

The purpose of this repository is to provide transparent access to the experimental materials used in the associated study.

The reproducibility chain is:

```text
Raw experimental CSV
        ↓
Google Colab analysis notebook
        ↓
Experimental-window filtering
        ↓
One-minute persistence analysis
        ↓
Gap-event and temperature-data analysis
        ↓
Simultaneous multi-node analysis
        ↓
Derived CSV results
        ↓
Figures and manuscript metrics
```

The raw dataset, analysis notebook, derived results, and generated figures are provided together so that the quantitative findings reported in the paper can be independently inspected and reproduced.