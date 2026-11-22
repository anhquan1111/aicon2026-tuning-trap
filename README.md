# A Tuning Trap in Quantum-Kernel Intrusion Detection

Code, data and audit checks for the paper

> Quan Tran Anh Vo, Minh Tuan Pham, Quang Anh Nguyen, Nguyen Nang Hung Van.
> *When Model Selection Manufactures a Finding: A Tuning Trap in
> Quantum-Kernel Intrusion Detection.* EAI AICON 2026 (to appear in the
> LNICST series, Springer).

Every figure, table and number in the paper can be recomputed from this
repository. The quantum kernel is simulated exactly (statevector); no quantum
hardware and no GPU is needed.

## Contents

```
runners/   the scripts that produce every figure, table and number
results/   the stored measurements the paper cites
models/    cached quantum Gram matrices (5 runs, 100x100, exact statevector)
data/      the preprocessed UNSW-NB15 records and the sampled subsets
paper/     the figures, tables and LaTeX source of the paper
```

## Reproducing the paper

```bash
pip install -r requirements.txt

python runners/run_paper3_selection.py     # selection on the original grid
python runners/run_paper3_camera_ready.py  # remedies, objectives, N = 500
python runners/make_paper3_figures.py      # Figures 1-3
python runners/make_paper3_tables.py       # tables and the number macros
python runners/audit_paper3.py             # every check must pass
```

Tested with Python 3.12 and the versions pinned in `requirements.txt`.

## Reproduction gates

`run_paper3_selection.py` refuses to write a new number unless it first
reproduces already-published ones from the same code path: the
cross-validated F1 at the five original grid points, and the test-set means
at the tuned and at the neutral regularisation constant. It prints the
deviations and exits non-zero if any exceeds 1e-9.

`run_paper3_camera_ready.py` re-draws the five N = 100 subsets from the
cleaned data and requires them to match the stored ones exactly, and
requires its quantum Gram matrices to match the cached ones, before it
measures anything at N = 500.

Small deviations on a different BLAS or scikit-learn build would not change
the argument, but they should be visible rather than silent, which is why
the gates print them.

## The audit

`audit_paper3.py` is the check described in the paper. It asserts that

1. every number in the paper recomputes from the stored artifacts, and no
   literal decimal appears in the prose;
2. the invariants the argument rests on still hold, for instance that on
   the original grid the quantum kernel's best non-degenerate score lies
   below the degenerate floor while each classical kernel's lies above it;
3. the degeneracy census scans all models and all rows, not only the ones
   the finding was expected in.

It also fails if any audit function in the file is never called.

## Data

`data/unsw/processed_data/` holds cleaned records derived from the public
UNSW-NB15 dataset (N. Moustafa and J. Slay, MilCIS 2015). The original
dataset and its terms of use are at
<https://research.unsw.edu.au/projects/unsw-nb15-dataset>.

`results/unsw/c4_revision/` holds measurements from a companion study by the
same authors. The paper recomputes one quantity from them that the companion
does not report.

## Notes

Comments inside the scripts are partly in Vietnamese. The code (names,
functions, outputs) is in English.

## License

Code: MIT (see `LICENSE`). UNSW-NB15 remains under its original terms.
