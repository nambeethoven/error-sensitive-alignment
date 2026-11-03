# Error-Sensitive Alignment Bounds for Affine-Line Decoding (Lean-verified)

Nam Khanh Ngo · Independent Researcher, Beethoven Capital (independent research initiative) ·
ORCID [0009-0003-6013-7308](https://orcid.org/0009-0003-6013-7308)

Paper: [`paper/e2_note.pdf`](paper/e2_note.pdf) · Version 1.0.1 (3 November 2026) · DOI
[10.5281/zenodo.23930694](https://doi.org/10.5281/zenodo.23930694) (v1.0.0:
[10.5281/zenodo.23920280](https://doi.org/10.5281/zenodo.23920280)) · [Changelog](CHANGELOG.md)

Cite: Nam Khanh Ngo, *Error-Sensitive Alignment Bounds for Affine-Line Decoding: A Lean-Verified
Refinement*, preprint (not peer reviewed), v1.0.1, 2026, doi:10.5281/zenodo.23930694.

## Result

Goyal–Guruswami (GG25, Thm 3.5) derive affine-line mutual correlated agreement (MCA) from
`(δ, a, n+1)` line decodability. This work shows that `b ≥ max(⌊δn⌋ + 2, 3)` aligned challenges
suffice when `n < |F|`, and propagates the change through the boosting step. For folded
Reed–Solomon codes in the capacity regime (GG25, Cor 4.10), with `δ = 1 − ρ − 2/t`:

    ε_mca(C, δ) ≤ ((δn + 2)·t + 3t³) / |F|      (previously (n·t + 3t³) / |F|)

This is an upper bound on the MCA error; the gain is a factor of about `1/δ` on the main term.

## Lean declarations (namespace `CodingTheory`)

| Declaration | Content |
|---|---|
| `card_le_floor_add_one_of_pencil_mismatch` | counting lemma, at most `e + 1` aligned challenges |
| `IsLineDecodable.mcaError_le_of_floor_add_two` | MCA from `(δ, a, b)` line decodability, `b ≥ max(⌊δn⌋+2, 3)`, `n < |F|` |
| `FrsInternal.strongLineDecodable_boost_of_lambda_le_target` | boost to any target `b'` at cost `(b' − 1)·L` |
| `mcaError_le_of_strongLineDecodable_of_floor_add_two` | composition |
| `frs_mcaError_le_floor_add_two`, `frs_mcaError_le_min` | folded Reed–Solomon bound, and the better of old and new |

The existing ArkLib theorems keep their statements; `IsLineDecodable.mcaError_le` and
`strongLineDecodable_boost_of_lambda_le` are derived from the same parametrised proofs.

## Reproduce

Base: ArkLib `2203b01cede353c4cb8e6524416dbf106e2c5cfb`, Lean `v4.35.0`. The Lean sources and
patch are those checked by this repository's CI at commit `e1cdaba6a1400424ccd17bfb782332511a8de3f1`
(unchanged since; later commits touch only documentation).

```sh
git clone https://github.com/Verified-zkEVM/ArkLib.git && cd ArkLib
git checkout 2203b01cede353c4cb8e6524416dbf106e2c5cfb
git apply ../e2.patch            # or copy ArkLib/ from this repository over the checkout
lake exe cache get
lake build ArkLib.Data.CodingTheory.ProximityGap.CapacityBounds.Frs
lake env lean ../AxiomCheck.lean # every declaration: propext, Classical.choice, Quot.sound
./scripts/validate.sh --axioms   # full ArkLib validation
```

## Provenance and credit

- The four files under `ArkLib/` are modified copies of ArkLib files (Apache-2.0, © ArkLib
  Contributors). Changes are listed in `e2.patch`.
- The counting argument is classical: Ligero (Ames, Hazay, Ishai, Venkitasubramaniam, CCS 2017)
  and Ben-Sasson, Kopparty and Saraf (CCC 2018); Haböck, IACR ePrint 2025/2110, Lemma 1, states it
  for correlated agreement. This work applies it in the proof of GG25 Theorem 3.5.
- The Lean proof of the counting lemma follows `exists_common_affine_set` of Proximity Prize submission
  `016274d1-9b09-4ce1-add3-4c632dbbc283` (68.15 bits; the commit proves `ProtocolClaim 6815`),
  `ProximityPrize/SubmissionLower` in github.com/proximity-prize/proximity-prize, Apache-2.0.
- AI assistance: Claude (Anthropic) wrote most of the Lean code and numerical checks; ChatGPT
  (OpenAI) reviewed statements and literature. The author takes responsibility for the content and
  presentation; the formalized theorems are checked by the Lean kernel, while the exposition and
  formalization were developed with AI assistance.

## License

Code: Apache-2.0 (see `LICENSE`). The paper in `paper/` is also released under CC BY 4.0
(the license of the Zenodo record).
