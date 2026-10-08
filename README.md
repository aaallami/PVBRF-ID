# PVBRF-ID: Private, Verifiable and Byzantine-Robust Federated Intrusion Detection for Oilfield Enterprise Networks

Files
- faithful_agg.py  core: 2-server secret sharing, Beaver-triple norm/cosine tests, Merkle inclusion, Pedersen verifiable aggregation
- test_core.py     11 correctness/attack-detection tests (all pass): `python3 test_core.py`
- demo_sanity.py   end-to-end sanity run on synthetic non-IID data + cost benchmark
- exp_ref_tau.py   reference-source x cosine-threshold x attack sweep (single seed, sanity only)

Real vs ideal (must be stated in the paper)
- Real: additive sharing over GF(2^127-1), fixed-point (16 frac bits), Beaver inner products, SHA-256 Merkle trees, Pedersen commitments + homomorphic audit
- Ideal: secure comparison (reveals only the accept bit; cost is a placeholder parameter), per-coordinate range proofs, trusted-dealer triples
- Toy: 512-bit Pedersen group (cost scaling only, not secure); seeded PRNG

Assumptions: two non-colluding servers in separate administrative domains; public parameters B (L2 bound), tau (cosine threshold);
reference direction known to both servers (previous aggregate OR clean root-set update).

Known limitations (from the sanity runs)
1. Cold start: with no reference in round 1, only the norm bound applies.
2. Previous-aggregate reference can be captured by an adaptive attacker (feedback loop) -> prefer a clean root-set reference.
3. In-bound, in-cone poisoning is not detectable by norm/cosine tests; damage is bounded by B, not eliminated.
4. Cosine filtering rejects honest non-IID sites; tau trades false rejection against attack surface.
5. Communication is ~8-16x plain FedAvg per element (16-byte field elements, two shares, one opened vector).
All numbers so far are single-seed, synthetic, logistic-regression sanity checks, not paper results.

## v3 additions
- faithful_agg.py: tolerant cosine threshold (tau < 0), 12 tests in test_core.py
- faithful_agg_edgeiiot_v3.ipynb: 15-class IDS (macro-F1), payload/port features, targeted adaptive / targeted-flip / backdoor attacks,
  30% Byzantine scenario, momentum-smoothed root reference, per-round and per-site acceptance logging, fairness and paired-CI tables.
  Profiles: tune (check learning), quick, full. Resumable; exports results_pack.zip (aggregates only).
