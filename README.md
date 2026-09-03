# TPB Review Annotation / Gán nhãn cấu trúc TPB

Human gold standard for measuring Theory of Planned Behaviour constructs in
short-term-rental guest reviews. Open a browser, code reviews, download a JSON,
send it back. No account, no server, no personal data.

**→ Annotate here:** `https://<your-github-user>.github.io/<repo>/`

*Tiếng Việt: bấm nút "Tiếng Việt" ở góc trên bên phải. Toàn bộ giao diện và sổ tay
mã hóa đều có song ngữ.*

---

## Why this exists

Three automatic instruments were applied to the same 706,921 reviews — a keyword
dictionary, a fine-tuned DeBERTa-v3 encoder, and a zero-shot entailment scorer —
and they disagree. On satisfaction they correlate at only ρ = 0.18 and differ on
prevalence by a factor of four and a half. Worse, the sign of a core theoretical
path (subjective norm → revisit intention) is **negative** under the dictionary
and **positive** under the entailment scorer.

Neither instrument can be declared correct by comparing it with the other. That
requires human judgement, which is what this task collects.

## What you are asked to do

For each review, tick the psychological constructs the guest **actually
expresses**. Five constructs, binary decision, roughly 10–20 seconds per review.
The codebook is on the page; read it once.

- `batch_00` (100 reviews) — **everyone codes this one.** It measures agreement
  between coders (Krippendorff's α).
- `batch_01` … `batch_10` (50 each) — take whichever you were assigned, or any
  unclaimed one. More coders per batch is better.

Work is saved in your browser as you go. Press "Download my work" when you
finish and email the file, or open a pull request adding it to
`annotations/`.

## Sampling design (why these reviews)

599 reviews, chosen rather than drawn at random, because the gold standard has
two jobs:

| Stratum | n | Purpose |
|---|---|---|
| Random | 300 | Unbiased prevalence and overall human–machine agreement |
| Disagreement-enriched | 299 | Reviews where the dictionary and the entailment scorer disagree on ≥ 3 of 5 constructs — the items that discriminate between instruments |

Both strata are balanced across four review-length bands (15–40, 40–70, 70–120,
120+ tokens). Length matters: the dictionary's structural coefficients converge
on their theoretical predictions as reviews get longer, which is the signature
of a word-budget artefact. Human labels within each band are what test it.

No instrument output appears anywhere in the published data files, so coders
cannot be anchored by them.

## Repository layout

```
index.html                 the annotation app (single file, no dependencies)
data/manifest.json         batch list and design metadata
data/batch_00.json …       the reviews, id + text only
annotations/               completed annotation files land here
```

## Output format

```json
{
  "app_version": "1.0",
  "coder_id": "coder_03",
  "batch_id": "batch_00",
  "started_at": "2026-09-03T09:12:44.000Z",
  "finished_at": "2026-09-03T09:41:02.000Z",
  "n_items": 100,
  "constructs": ["ATT", "SN", "PBC", "SAT", "BI"],
  "annotations": [
    { "id": 8508622, "ATT": 1, "SN": 0, "PBC": 1, "SAT": 0, "BI": 1,
      "flag": 0, "ms": 11840 }
  ]
}
```

`flag: 1` marks a review that could not be coded (not English, empty, automated
message); its construct fields are then all zero and are ignored in scoring.
`ms` is time on item, used only to screen for inattentive coding.

## Deploying it yourself

GitHub Pages, no build step:

1. Push `index.html` and `data/` to a repository.
2. Settings → Pages → Source: *Deploy from a branch* → `main` / root.
3. Open the URL. That is all — the app is static and fetches its data with
   relative paths.

## Privacy

Review text comes from the [Inside Airbnb](https://insideairbnb.com/get-the-data/)
public dataset and is reproduced unchanged. Reviewer names and identifiers are
not included. The app collects nothing about you: everything stays in your
browser's local storage until you choose to download it.

## Citation

If you use this instrument or the resulting gold standard, please cite the
source study and this annotation set. Coders who complete a batch are
acknowledged by name in the paper unless they ask otherwise.

## Licence

Code MIT. Annotations released CC BY 4.0 once collection closes.
