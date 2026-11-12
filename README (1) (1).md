# Logical Entailment Reasoning Agent
The notebook combines natural-language parsing with classical
reasoning engines and evaluates the resulting predictions on
RuleTaker-style, FOLIO-style, and CounterBench examples.


## Project overview

The notebook contains three main reasoning components:

  -----------------------------------------------------------------------
  Component               Purpose                 Techniques
  ----------------------- ----------------------- -----------------------
  Natural-language        RuleTaker-style facts,  Rule parsing, matching,
  parser +                rules, and questions    forward chaining,
  forward-chaining engine                         negation handling

  First-order logic       First-order logic       CNF conversion,
  parser + resolution     premises and            Skolemisation,
  engine                  conclusions             unification, resolution

  Structural causal model CounterBench-style      Abduction, intervention
  engine                  counterfactual          (`do`-operator),
                          questions               forward inference
  -----------------------------------------------------------------------

The evaluated reasoning paths are symbolic; they do not use a generative
language model to make the final reasoning decision.

## Requirements

-   Python 3
-   Jupyter Notebook
-   `datasets` from Hugging Face for dataset loading and evaluation

Install the optional dataset dependency:

``` bash
pip install datasets
```

The notebook also uses Python standard-library modules. If dataset
access is unavailable, dataset evaluation cells report the issue and may
skip that evaluation. FOLIO has a GitHub fallback in the notebook.

## How to run

1.  Open `AI assignment.ipynb` in Jupyter Notebook, JupyterLab, or
    Google Colab.
2.  Install `datasets` if you want to run the dataset-loading and
    benchmark cells.
3.  Run the notebook **from top to bottom**. Later cells use functions
    and variables defined earlier.
4.  Review the unit-test and regression-test output.
5.  Review the benchmark results and note which data source and
    evaluation path each result uses.

The notebook's dataset cells are designed to handle unavailable
downloads with a message rather than stopping the entire notebook.

## Evaluations

The notebook includes evaluation paths for:

-   **RuleTaker:** natural-language examples evaluated with the parser
    and forward-chaining engine.
-   **FOLIO:** natural-language input evaluated through the hand-written
    parser, plus a separate evaluation where supplied gold logical
    formulas are passed to the resolution engine.
-   **CounterBench:** counterfactual examples evaluated with the
    structural causal model engine.

Keep these results separate when reporting performance. In particular,
evaluation with supplied gold logical formulas tests the reasoning
engine after formalization; it does **not** measure the complete
English-to-logic pipeline.

## Unit and regression tests

The notebook includes examples for:

-   Rule-based entailment and non-entailment
-   Negation and open-/closed-world handling
-   Parser regression cases
-   First-order logic entailment using resolution
-   Counterfactual reasoning


## Assignment test file

The submission cell looks for a file named:

``` text
test.csv
```

Place it in the same working directory as the notebook. The notebook's
test loader also supports `.json` and `.jsonl` files if you change
`TEST_PATH` accordingly.

The loader accepts common field names:

  -----------------------------------------------------------------------
  Field                               Accepted names
  ----------------------------------- -----------------------------------
  ID                                  `id`, `ID`, `idx`, `qid`,
                                      `question_id`, `example_id`

  Context / premises                  `context`, `theory`, `premises`,
                                      `given_info`, `knowledge_base`,
                                      `kb`, `text`, `story`

  Question / conclusion               `question`, `query`, `statement`,
                                      `conclusion`, `hypothesis`

  Label (optional)                    `label`, `answer`, `gold`, `target`
  -----------------------------------------------------------------------

For JSON, the examples should be a list, or a list stored under one of
these keys: `data`, `examples`, `test`, or `rows`.

Each example must include a context/premises field and a
question/conclusion field. A label is optional and is used only for
evaluation when present.

### Output

When `test.csv` is available, the notebook:

1.  Reads and normalizes the test examples.
2.  Calls `predict_any` for each context/question pair.
3.  Converts predictions to the assignment's binary `True` / `False`
    labels.
4.  Prints the number of queries and inference time.
5.  Prints accuracy if labels are included.
6.  Writes `submission.csv` with columns:

``` csv
id,label
```

Unsupported or uncertain predictions are conservatively mapped to
`False` for the binary submission. The notebook separately reports how
many such predictions were mapped.

## Files

-   `AI assignment.ipynb` --- implementation, tests, dataset
    evaluations, and submission generation.
-   `README.md` --- project overview and instructions.
-   `Report` --- Reporting the results and other explaination.
