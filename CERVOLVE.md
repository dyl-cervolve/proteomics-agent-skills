# Cervolve / Jetty profile

This fork retains the upstream Apache-2.0 license and all upstream skills. Its
consumer is `dyl-cervolve/brain-omics-ingestion`; that repository pins this fork
to a reviewed commit in `config/skills.lock.json`.

For post-mortem brain ingestion, begin with `reading-proteomics-data` and
`performing-proteomics-quality-control`. Read their referenced column maps.
Assess the normalization, imputation, batch and statistical skills against the
actual measurements before applying them.

The ingestion contract takes precedence over analysis defaults:

* Keep deposited values, zeros, missing values, source identifiers and protein
  groups in the canonical matrix. Decoys, contaminants, low-quality features,
  transformations and imputations belong in documented derived views or QC flags.
* A protein-group matrix must contain sample-level measurements. RNA counts and
  differential-expression summary tables cannot stand in for proteomics.
* Preserve one-to-many protein/gene mapping and record the reference version.
* Keep technical replicates and samples from one donor in the same ML partition.
  Fit imputation, feature selection and scaling on training donors only.
* Follow Jetty's 8 GB memory, 4 GB source-set and 60-minute limits. Prefer
  processed tables; do not start a raw-spectrum search workflow by default.
* Produce evidence-backed skill decisions and typed `checks[]` entries in
  `validation_report.json`. A successful upload alone does not make a run pass.

Use the locked Python environment and bootstrap instructions in
`https://github.com/dyl-cervolve/brain-omics-ingestion`. These Markdown skills
are guidance, not packages to install with pip. Inspect helper code before use.

Sync policy: fetch upstream into a review branch, inspect changes and licenses,
test the ingestion environment, then update the consumer's commit pin. Jetty
runs must never select a moving branch tip automatically.
