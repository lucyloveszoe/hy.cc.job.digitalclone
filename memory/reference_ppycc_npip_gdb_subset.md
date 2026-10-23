---
name: reference-ppycc-npip-gdb-subset
description: The 15 npip_* tables that expose PPYCC data through the Genealogy DB Web Service (groups, keys, caps, sensitivity) and PPYCC-specific API gaps found 2026-10-12
metadata:
  type: reference
---

PPYCC (Projection/Planning/Yield/CRRC/Cost; backbone in gDB, deployed at BRCM SG DC, see [[project-nxg-pipeline-and-raptor]]) is exposed through gDB WS as the **15 `npip_*` tables**, documented at `http://wsdsgsvc03.sgn.broadcom.net:8080/GenealogyDB.DocFx/api/npip_*.html`. Source: gDB WS review, [[project-gdb-ws-api-doc-review-2026-10]] (counts reproducible from `family/gdb-ws/review-evidence/table_inventory.csv`).

**Groups and selectors (docs-verified):**
- CSR shipping (5): `npip_csr_shipping_main_display` (31 cols; `ship_date` <=90 d + customer<=10/program/product/mpn/status, or `ship_from`, or `auto_id`), `_split_display` (`main_id`|`lot_code`|`assy_lot` + `status`), `_awb_display` (`auto_id` <=100; has `attach_filename`, `attach_shared_link_url`), `_distinct`, `_split_lotcode_distinct` (empty-args dropdown feeds).
- NPI yield (6): `projection_header`/`_detail`, `reportcard_header`/`_detail` (identical column structures; detail is a cell grid `rownum,colnum,seq,process,fiscal_ww_month_quater,value`), `wafer_actual` (`wafer_part_number`+`cm`+`process`), `module_test_actual` (`flash_code`<=150 + `process_key`<=10).
- NPI setup (2): `setup_module_flash` (empty args), `staging_project_circuit` (21 cols, six optional keys <=50).
- Financial (1): `npip_financial_wafer_rate` (`config_set`<=1, `wafer_rate_id`<=1, `wafer_circuit`<=10).
- Planning (1): `npip_planning_pcb_bominput_master_view` (`flash_code`<=5).
- `flash_code` is the common NPI key in 8 of 15 tables; 6 of 15 carry `date_modified`/`modified_by`; none have the file-derived system columns (no `file_id`).

**PPYCC-specific gaps (proposal, not decided):** no `modified_since`/tombstones (full re-pull for Raptor/Spotfire/Snowflake sync); financial rates, customer shipments and `ebr_records.Receiver_Email` served under the same token as wafer data (scope per table group proposed); typos `yield_quater_fiscal`, `fiscal_ww_month_quater`; `columns:"*"` default means added columns (e.g. Y048 FTC ML cost adder, target 10/15/2026 — completed, buy-off 10/09, [[project-2026-ww37-40-weekly-review]]) change every star client; NPI yield needs 2-6 calls per `flash_code` (project-level read proposed); the Cost-of-Test input DB ([[project-ppycc-cost-of-test]]) is not in the gDB WS table list (verify how GO/finance consume it).

**How to apply:** when asked what PPYCC data is reachable by API, start from this list; before changing any npip table or column, check the `columns:"*"` and rename-batching notes above.

**Round 2 (2026-10-21) — business-resource view (inferred from column names, [verify] with owner):** shipment (`npip_csr_shipping_main_display`, key `auto_id`) has lots (`split_display.main_id` = shipment `auto_id`; `assy_lot` bridges to assembly genealogy) and an AWB/attachment record (`awb_display.auto_id` = `main.awb_id`; carries `attach_filename` + `attach_shared_link_url`, a link in a row with its own access; `modified_by`; no units on weight/box qty). NPI project keyed by `flash_code` spans setup, staging (circuit), yield projection/report-card header+detail (join flash_code, report_type, process_type, rownum), module test actual, PCB BOM input. Wafer yield actual (`wafer_part_number`+`cm`+`process`), wafer rate (financial; `wafer_circuit` ~ staging `circuit`), facets (`*_distinct`, empty args). These are portal read views, not warehouse data; a shipment screen needs ~3 calls, an NPI project 2-6. Proposed in [[project-gdb-ws-api-doc-review-2026-10]] round 2: shared typed `where` query (Option C), scopes `gdb.ppycc.npi/shipping/financial.read`, attachment as file reference, PPYCC domain reads (Option F) only if call logs justify. Open: do the PPYCC portals call gDB WS or read the DB directly?
