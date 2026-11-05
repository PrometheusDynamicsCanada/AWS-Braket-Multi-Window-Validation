# Public Disclosure & Intellectual Property Boundary

## 1. Purpose of This Public Evidence Repository

This repository provides a complete, cryptographically verifiable, offline-auditable hardware validation package for the **AWS Braket Multi-Window & 4-Pillar Quantum Compilation Campaign** (`450` compiled circuit executions, `460,800` physical shots, `10` AWS Braket `ProgramSet` tasks, `265.73 seconds` total recorded task turnaround time, `USD 198.8400` total QPU charges) executed on **`Rigetti Cepheus-1-108Q`** (`arn:aws:braket:us-west-1::device/qpu/rigetti/Cepheus-1-108Q`), alongside a direct `10`-workload cross-provider comparison against **`IBM Quantum Heron`** (`ibm_kingston`, `ibm_fez`, `ibm_marrakesh`).

Every hardware claim in the accompanying report can be verified offline without cloud credentials using [`scripts/verify_braket_campaign.py`](scripts/verify_braket_campaign.py).

## 2. Included Empirical Provenance (100% Public & Auditable)

- **AWS Braket Hardware Task ARNs:** All `10` AWS Braket `ProgramSet` Task ARNs (`arn:aws:braket:us-west-1:893688540163:quantum-task/...`), task submission and completion timestamps, turnaround durations (`265.73s` total), and raw per-circuit measurement bitstring histograms (`data/jobs/*.json`).
- **Cryptographic Commitments & Timestamp Chronology:** All `6` SHA-256 commitment ledgers (`data/commitments/*.json`) locking the exact compiled physical circuit hashes (`circuit_sha256`), native two-qubit gate counts (`CZ`), and circuit depths across all `450` executed circuits. For Tasks 3-10 (`Pillar 3 QAOA`, `Pillar 4 Tasks 4-8`, `Window 2 Task 9`, and `Window 3 Task 10`), `commitment_timestamp_utc` in `data/commitments/*.json` strictly precedes `submission_timestamp_utc` in `data/jobs/*.json`. For Tasks 1 and 2 (`Window 1 Pillar 1` submitted at `00:07:49.173806Z` and `Pillar 2 Mirrors` submitted at `00:11:06.678697Z`), the JSON `commitment_timestamp_utc` values (`00:56:22.599515Z` and `00:56:23.836577Z`) reflect the `00:56Z` runner resume (`--phase w1_all`) that loaded the unmodified `00:07:49.124038Z` and `00:11:06.595870Z` pre-submission compiled circuit caches and re-wrote the JSON wrapper before skipping already-completed Tasks 1 and 2.
- **Multi-Window Device Calibration Telemetry:** Full AWS Braket calibration snapshots (`data/calibration_snapshots/*.json`) across **Calibration Window 1** (`00:07-01:03 UTC` baseline), **Calibration Window 2** (`09:24 UTC`, `+9.3 hours` temporal drift), and **Calibration Window 3** (`19:12 UTC`, following the `14:00-16:00 UTC` daily hardware recalibration).
- **Unrouted Logical Benchmark Circuits & Ideal Reference Distributions:** All `60` unrouted logical circuits in OpenQASM 2.0 format (`data/logical_circuits/*.qasm`), exact statevector reference distributions (`data/logical_circuits/*.npz`), Clifford mirror target bitstrings (`clifford_mirror_targets.json`), and QAOA MaxCut graph specifications (`qaoa_trajectory_graphs.json`).
- **Complete Scored Ledgers & Master CSVs:** All `450` scored circuit execution records (`data/results/*.json` and `data/ALL_450_RIGETTI_CIRCUITS_RESULTS.csv`) and the `10`-workload cross-provider comparison table (`data/CROSS_PROVIDER_RIGETTI_VS_IBM_HERON_10WORKLOADS.csv`).

## 3. Withheld Proprietary Compiler Internals

In accordance with Prometheus Dynamics intellectual property protection standards:
- Internal compiler routing heuristics, multi-objective scoring equations, candidate selection rules, and proprietary synthesis pass implementations are withheld from this public repository.
- Compiled candidates are identified strictly by their public pipeline designations (`sabre_o3`, `tket`, `prometheus_fresh_candidate`, `prometheus_control_candidate`, `prometheus_candidate`, `sabre_o3_default`, `sabre_o3_matched_budget`, and `tket_matched_budget`) and their verifiable SHA-256 cryptographic commitments.
