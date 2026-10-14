# Brightest Group Galaxies in AMICO-COSMOS

[![Paper](https://img.shields.io/badge/A%26A-713%2C%20A333%20(2026)-blue)](https://doi.org/10.1051/0004-6361/202659640)
[![DOI](https://img.shields.io/badge/DOI-10.1051%2F0004--6361%2F202659640-informational)](https://doi.org/10.1051/0004-6361/202659640)

Catalogues of **brightest group/cluster galaxies (BGGs)** for the groups and clusters of the
**AMICO-COSMOS** catalogue. The release has one catalogue for each AMICO detection band (*r*, *Y*
and *H*) and a **merged catalogue** for the 490 systems detected in all three runs.

This release accompanies:

> **Toni, G., Castignani, G., Combes, F., Salomé, P., Bongiovanni, A., Moscardini, L., Maturi, M. (2026)**
> *Molecular gas properties of star-forming brightest group galaxies at z ∼ 0.3*,
> Astronomy & Astrophysics, 713, A333.
> [doi:10.1051/0004-6361/202659640](https://doi.org/10.1051/0004-6361/202659640) ·
> [PDF](https://www.aanda.org/articles/aa/pdf/2026/09/aa59640-26.pdf)

If you use these catalogues, please cite the paper above (see [Citation](#citation)).

---

## Contents

| File | Detection band | Description |
|---|---|---|
| `<BGG_catalogue_r>` | *r* | BGGs of the systems detected with the *r* band as reference |
| `<BGG_catalogue_Y>` | *Y* | BGGs of the systems detected with the *Y* band as reference |
| `<BGG_catalogue_H>` | *H* | BGGs of the systems detected with the *H* band as reference |
| `<BGG_catalogue_merged>` | *r*, *Y*, *H* | Cross-match of the 490 systems detected in all three runs, with their BGGs in each band |
| `README.md` | – | This file |

Each single-band catalogue has one row per AMICO-COSMOS system detected in that band. The merged
catalogue has one row per system detected in all three bands.

## Parent sample

The groups and clusters come from **AMICO-COSMOS** (Toni et al. 2024). It was obtained by running
the AMICO detection algorithm on the COSMOS2020 and COSMOS2015 galaxy catalogues and contains
1269 systems over an effective area of about 1.69 deg². AMICO was run three times, with the *r*, *Y*
and *H* bands as reference. The BGG selection was applied to each run separately, which gives the
three single-band catalogues. The 490 systems recovered in all three runs were then matched to
build the merged catalogue.

## Selection method

For each system, the BGG is chosen among the galaxies that AMICO associates with it:

1. **Spatial cut:** projected separation from the AMICO centre **< R200 / 3**.
2. **Redshift cut:** |z<sub>gal</sub> − z<sub>sys</sub>| **≤ 0.05 (1 + z<sub>sys</sub>)**.
3. **Brightness:** candidates are ranked by apparent magnitude in the detection band of that
   catalogue. The **brightest** is the provisional BGG and the next is the second-ranked galaxy.
4. **Magnitude-gap tie-break:** if Δm<sub>12</sub> = m<sub>2</sub> − m<sub>1</sub> **< 0.5 mag**,
   the BGG is whichever of the two has the **higher membership probability**.

### Quality scores

Besides the magnitude gap, each BGG has two scores that indicate how reliable the choice is:

| Score | Definition | Interpretation |
|---|---|---|
| **Probability score** | P<sub>mem</sub>(BGG) / max P<sub>mem</sub> among all members of the system | 1 = the BGG is the most probable member |
| **Distance score** | projected distance to the system centre / R200(z) | 0 = at the centre; ≤ 1/3 by construction |

## Catalogue columns

### Single-band catalogues (*r*, *Y*, *H*)

All three catalogues have the same columns. Distances are in **degrees** and magnitudes refer to
the detection band of the catalogue. Missing values are `NaN`.

| Column | Unit | Description |
|---|---|---|
| `cluster_id` | – | AMICO-COSMOS system ID |
| `bcg1_prob_score` | – | Probability score of the BGG |
| `bcg1_dist_score` | R200 | Distance score of the BGG |
| `bcg1_galaxy_id` | – | ID of the selected BGG |
| `bcg1_magnitude` | mag | Apparent magnitude of the BGG |
| `bcg1_distance` | deg | Projected distance of the BGG from the centre |
| `bcg1_membership_probability` | – | AMICO membership probability of the BGG |
| `bcg2_*` | | Same quantities for the second-ranked galaxy |
| `magnitude_gap` | mag | m(2nd brightest) − m(brightest), always ≥ 0 |
| `swapped` | bool | `True` if the tie-break replaced the brightest galaxy |

### Merged catalogue (490 systems)

The merged catalogue links each system across the three runs and lists its BGG and second-ranked
galaxy in every band. To get the full properties of a galaxy (magnitudes, scores, distances), join
on the corresponding `cluster_id_*` column with the single-band catalogue.

| Column | Description |
|---|---|
| `cluster_id_r` | System ID in the *r*-band catalogue |
| `cluster_id_y` | System ID in the *Y*-band catalogue |
| `cluster_id_h` | System ID in the *H*-band catalogue |
| `r_bcg1_galaxy_id` | ID of the BGG selected in the *r* band |
| `r_bcg2_galaxy_id` | ID of the second-ranked galaxy in the *r* band |
| `y_bcg1_galaxy_id` | ID of the BGG selected in the *Y* band |
| `y_bcg2_galaxy_id` | ID of the second-ranked galaxy in the *Y* band |
| `h_bcg1_galaxy_id` | ID of the BGG selected in the *H* band |
| `h_bcg2_galaxy_id` | ID of the second-ranked galaxy in the *H* band |

### Notes

- A system without a second-ranked BGG entry is a system where no other galaxy passed the cuts.
- Systems detected in more than one band appear in more than one single-band catalogue, and the BGG
  chosen may differ between bands. The merged catalogue makes this easy to check: a system whose
  `r_bcg1_galaxy_id`, `y_bcg1_galaxy_id` and `h_bcg1_galaxy_id` all agree has a BGG that is robust
  against the choice of detection band.
- **Secure BGGs** can also be identified by a large magnitude gap, a probability score
  close to 1 and a small distance score.

## Citation

```bibtex
@ARTICLE{Toni2026,
       author = {{Toni}, Greta and {Castignani}, Gianluca and {Combes}, Fran{\c{c}}oise and
                 {Salom{\'e}}, Philippe and {Bongiovanni}, Angel and {Moscardini}, Lauro and
                 {Maturi}, Matteo},
        title = "{Molecular gas properties of star-forming brightest group galaxies at z $\sim$ 0.3}",
      journal = {Astronomy \& Astrophysics},
         year = 2026,
       volume = {713},
          eid = {A333},
          doi = {10.1051/0004-6361/202659640}
}
```

If you use the parent sample, please also cite the AMICO-COSMOS catalogue paper (Toni et al. 2024)
and the galaxy catalogues COSMOS2020 (Weaver et al. 2022) and COSMOS2015 (Laigle et al. 2016).

## Contact

The BGG selection depends on a few parameters, such as the search radius, the redshift window
and the magnitude-gap threshold. The values used here suit the science goals of the paper, but
other applications may work better with different choices. If you plan to use these catalogues,
please get in touch first: I'm glad to help adapt the selection to your science case and to share
practical caveats about the sample.

Write me an email at `greta.toni4@unibo.it`.
