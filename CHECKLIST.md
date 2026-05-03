# Pre-submission checklist

Use this checklist before submitting the paper to *Briefings in Bioinformatics*. Each item, when ticked, contributes to a smoother review and a higher acceptance probability.

---

## GitHub repository

- [ ] GitHub organisation `exin-lti-upjv` created and verified (Step 1 of `SETUP_INSTRUCTIONS.md`)
- [ ] Empty repository `incrt-geo-proteins` created, public visibility (Step 2)
- [ ] Package contents uploaded: `README.md`, `LICENSE`, `CITATION.cff`, `requirements.txt`, `.gitignore`, `data/`, `notebooks/`, `scripts/`, `results/` (Step 3)
- [ ] Five pretraining notebooks uploaded to `notebooks/`, with cell outputs cleared (Step 4)
- [ ] Two evaluation scripts uploaded to `scripts/`, with cell outputs cleared (Step 4)
- [ ] Marta added as collaborator with **Maintain** role (Step 5)
- [ ] Visiting `https://github.com/exin-lti-upjv/incrt-geo-proteins` shows the README, the licence badge, and a working **Cite this repository** button (Step 6)

## Zenodo dataset

- [ ] Zenodo account linked to ORCID `0000-0002-2894-6691`
- [ ] Five pretrained checkpoints uploaded as a single Dataset record
- [ ] Optional `MANIFEST.json` included for convenience (per-checkpoint metadata)
- [ ] Metadata complete: title, authors with ORCIDs, description, license (Apache 2.0), keywords, GitHub URL as related identifier
- [ ] DOI **reserved** (not yet published) — DOI string copied into the paper
- [ ] DOI verified by visiting `https://doi.org/<your-doi>` — should resolve to the Zenodo landing page even in draft state

## Documentation cross-checks

- [ ] `README.md` placeholder `10.5281/zenodo.XXXXXXX` replaced with the real reserved DOI
- [ ] `README.md` Zenodo badge added near the top
- [ ] Paper "Code Availability" section cites:
   - GitHub URL `https://github.com/exin-lti-upjv/incrt-geo-proteins`
   - Zenodo DOI `10.5281/zenodo.<your-doi>`
- [ ] Paper "Data Availability" section cites the same Zenodo DOI for checkpoints, plus the public sources for input data (Ensembl release 115, UniProt REST API)

## Paper

- [ ] Authorship list: **Cirrincione (UPJV), Lovino (UniMoRe)** — Marta as corresponding author
- [ ] Marta's ORCID added (was missing in CITATION.cff — update before submission)
- [ ] Marta's email confirmed as corresponding author
- [ ] All five reviewers suggestions filled in the cover letter
- [ ] Conflict of interest statement filled in (or stated as "none")
- [ ] Funding statement filled in (or stated as "no specific funding received")
- [ ] Acknowledgements (optional): mention Giorgia Ghione for the underlying INCRT-geo theory framework
- [ ] iThenticate / plagiarism-self-check passed (Briefings runs this automatically; pre-submission via Paperpal Preflight is optional)

## Final integrity checks

- [ ] All numbers in the paper match the JSON files in `results/` exactly
- [ ] All figures are rendered from the JSON files in `results/` (so a reviewer can re-render with the public data)
- [ ] No personal paths (`/content/drive/MyDrive/...`) appear in the paper
- [ ] The paper PDF metadata does not leak author names if the journal uses double-blind review (Briefings does **not** use double-blind, so this is not strictly required, but good hygiene)

---

## After submission

- [ ] Save the submission confirmation email
- [ ] Track the manuscript ID on Manuscript Central
- [ ] **Wait** — Briefings averages 6 weeks for the first decision

## Upon acceptance

- [ ] Tag a GitHub release `v1.0.0` (Step 7 of `SETUP_INSTRUCTIONS.md`)
- [ ] Click **Publish** on the Zenodo draft to finalise the DOI
- [ ] Update `README.md` and the Zenodo record to reference the published paper DOI
- [ ] Optional: upload an arXiv preprint for higher visibility
