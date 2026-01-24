# consumer_product_safety_analysis documentation!

## Description

Develop predictive models to identify which consumer product injuries are most likely to result in hospitalization and determine what factors—product categories, demographics, injury characteristics—predict severe medical outcomes among emergency department cases, producing evidence-based findings that inform CSRI's 2025 Product Safety Spotlight report and contribute to public understanding of consumer product safety priorities.

## Commands

The Makefile contains the central entry points for common tasks related to this project.

### Syncing data to cloud storage

* `make sync_data_up` will use `aws s3 sync` to recursively sync files in `data/` up to `s3://cpsa/data/`.
* `make sync_data_down` will use `aws s3 sync` to recursively sync files from `s3://cpsa/data/` to `data/`.


