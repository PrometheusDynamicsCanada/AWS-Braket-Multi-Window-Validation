# Data Dictionary & Schema Reference

## 1. Master Circuit Ledger: `data/ALL_450_RIGETTI_CIRCUITS_RESULTS.csv`

Contains `450` rows (one per compiled circuit execution on `Rigetti Cepheus-1-108Q`, totaling `460,800` physical shots across `10` AWS Braket `ProgramSet` tasks). Zero cells contain placeholder values (`-` or `N/A`).

| Column Name | Type | Direction | Description |
| :--- | :--- | :---: | :--- |
| `pillar` | `string` | Info | Validation pillar identifier (`Pillar1_Window1`, `Pillar1_Window2`, `Pillar1_Window3`, `Pillar2_Clifford_Mirror`, `Pillar2_SPAM_Control`, `Pillar3_QAOA_Trajectory`, `Pillar4_Fresh_Block1_packed_randomized_block1`, `Pillar4_Fresh_Block2_pipeline_isolated_block2`). |
| `window_id` | `int` | Info | Calibration window index (`1`, `2`, or `3`). |
| `task_arn` | `string` | Info | Verifiable AWS Braket `ProgramSet` Quantum Task ARN (`arn:aws:braket:us-west-1:893688540163:quantum-task/...`). |
| `backend` | `string` | Info | Target QPU identifier (`Cepheus-1-108Q`). |
| `workload_id` | `string` | Info | Unique logical workload, mirror, SPAM control, or QAOA stage identifier. |
| `family_or_stage` | `string` | Info | Benchmark circuit family (`dense`, `bipartite`, `qft_phase`, `qaoa`, `qaoa_maxcut`, `ising`, `mirror_d1`, `mirror_d2`, `spam_state_prep_readout`) or QAOA optimization stage (`initial`, `intermediate`, `final_cobyla_solution`). |
| `width` | `int` | Info | Logical circuit width (number of active qubits, `4` to `36`). |
| `pipeline` | `string` | Info | Compiler pipeline identifier (`sabre_o3`, `tket`, `prometheus_fresh_candidate`, `prometheus_control_candidate`, `prometheus_candidate`, `sabre_o3_default`, `sabre_o3_matched_budget`, `tket_matched_budget`). |
| `shots` | `int` | Info | Physical measurement shots executed (`1,024` per circuit). |
| `native_2q` | `int` | `(↓)` | Compiled native two-qubit gate count (`CZ` on `Rigetti Cepheus-1-108Q`). Lower is better. |
| `depth` | `int` | `(↓)` | Compiled physical circuit depth in native gates. Lower is better. |
| `circuit_sha256` | `string` | Info | Pre-execution SHA-256 hash of the canonical native physical circuit locked in `data/commitments/*.json`. |
| `bhattacharyya_overlap_fb` | `float` | `(↑)` | Classical Bhattacharyya Distribution Overlap `F_B = Σ_x √(P_ideal(x) · P_meas(x))` for Pillars 1 & 4; Mean Per-Qubit Bit Accuracy `p_bit` for Pillar 2; High-Quality Cut Probability `P(Cut ≥ 0.90 · C_max)` for Pillar 3. Higher is better. |
| `squared_hellinger_fh` | `float` | `(↑)` | Squared Hellinger Distribution Fidelity `F_H = (F_B)^2` for Pillars 1 & 4; Exact Target Bitstring Survival for Pillar 2; QAOA Approximation Ratio `r = E[Cut] / C_max` for Pillar 3. Higher is better. |
| `hop` | `float` | `(↑)` | Heavy-Output Probability (`HOP`) for Pillars 1 & 4; Zero-Centered Polarization `P = 2·p_bit - 1` for Pillar 2; Ideal Gain Retention `η` for Pillar 3. Higher is better. |
| `tvd` | `float` | `(↓)` | Total Variation Distance `TVD = 0.5 · Σ_x |P_ideal(x) - P_meas(x)|` for Pillars 1 & 4; Mean Hamming Fraction for Pillar 2; Absolute Approximation Ratio Gap `|r - r_ideal|` for Pillar 3. Lower is better. |
| `observed_entropy_bits` | `float` | Info | Observed Shannon Entropy `H = -Σ_x P_meas(x) log2 P_meas(x)` (bits) for Pillars 1 & 4; Mean Hamming Distance (bits) for Pillar 2; Measured Expected Cut `E[Cut]` for Pillar 3. |
| `primary_metric_name` | `string` | Info | Pre-registered primary evaluation metric name for the workload. |
| `primary_metric_value` | `float` | `(↑)` | Pre-registered primary evaluation metric value. Higher is better. |

---

## 2. Cross-Provider Comparison Ledger: `data/CROSS_PROVIDER_RIGETTI_VS_IBM_HERON_10WORKLOADS.csv`

Contains `10` rows corresponding to the `10` identical logical workloads (`dense_6q_s260026`, `dense_9q_s260027`, `dense_12q_s260028`, `bipartite_6q_s260126`, `bipartite_9q_s260127`, `bipartite_12q_s260128`, `qft_phase_6q_s260526`, `qft_phase_9q_s260527`, `qft_phase_12q_s260528`, `qaoa_12q_s260428`) executed both across **3 calibration windows on `Rigetti Cepheus-1-108Q`** (`4-tile planar square lattice`, native `CZ`) and across **3 QPUs on `IBM Quantum Heron`** (`ibm_kingston`, `ibm_fez`, `ibm_marrakesh`, `156Q heavy-hex lattice`).

| Column Name | Type | Direction | Description |
| :--- | :--- | :---: | :--- |
| `workload_id` | `string` | Info | Shared logical circuit workload identifier. |
| `family` | `string` | Info | Benchmark circuit family (`dense`, `bipartite`, `qft_phase`, `qaoa`). |
| `width` | `int` | Info | Logical circuit width (`6`, `9`, or `12` qubits). |
| `rigetti_3w_mean_fb_sabre` | `float` | `(↑)` | 3-window mean `F_B` on `Rigetti Cepheus-1-108Q` for `SABRE O3`. |
| `rigetti_3w_mean_fb_tket` | `float` | `(↑)` | 3-window mean `F_B` on `Rigetti Cepheus-1-108Q` for `TKET`. |
| `rigetti_3w_mean_fb_prometheus` | `float` | `(↑)` | 3-window mean `F_B` on `Rigetti Cepheus-1-108Q` for `Prometheus Fresh`. |
| `ibm_heron_3qpu_mean_fb_sabre` | `float` | `(↑)` | 3-QPU mean `F_B` on `IBM Heron` (`ibm_kingston`, `ibm_fez`, `ibm_marrakesh`) for `SABRE O3`. |
| `ibm_heron_3qpu_mean_fb_tket` | `float` | `(↑)` | 3-QPU mean `F_B` on `IBM Heron` for `TKET`. |
| `ibm_heron_3qpu_mean_fb_prometheus` | `float` | `(↑)` | 3-QPU mean `F_B` on `IBM Heron` for `Prometheus`. |
| `rigetti_3w_mean_2q_sabre` | `float` | `(↓)` | 3-window mean native `CZ` gate count on `Rigetti Cepheus-1-108Q` for `SABRE O3`. |
| `rigetti_3w_mean_2q_tket` | `float` | `(↓)` | 3-window mean native `CZ` gate count on `Rigetti Cepheus-1-108Q` for `TKET`. |
| `rigetti_3w_mean_2q_prometheus` | `float` | `(↓)` | 3-window mean native `CZ` gate count on `Rigetti Cepheus-1-108Q` for `Prometheus Fresh`. |
| `ibm_heron_3qpu_mean_2q_sabre` | `float` | `(↓)` | 3-QPU mean native two-qubit gate count on `IBM Heron` for `SABRE O3`. |
| `ibm_heron_3qpu_mean_2q_tket` | `float` | `(↓)` | 3-QPU mean native two-qubit gate count on `IBM Heron` for `TKET`. |
| `ibm_heron_3qpu_mean_2q_prometheus` | `float` | `(↓)` | 3-QPU mean native two-qubit gate count on `IBM Heron` for `Prometheus`. |
