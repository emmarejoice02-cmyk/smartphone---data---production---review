# Smartphone Data Preparation, Visualization & Unit Testing

## Project Overview

This project reviews and improves a data-preparation workflow for a university procurement team that is evaluating smartphone data before using it for analysis and visualization.

The work is implemented in a Jupyter Notebook and focuses on three practical data-science tasks:

1. Cleaning and reducing a raw smartphone dataset to the fields required for analysis.
2. Building a reusable visualization function that compares a selected smartphone attribute against price.
3. Testing the cleaning logic with `pytest`/`ipytest` to make sure missing values are handled correctly.

The project is particularly useful as a small example of **production-oriented Python data work** because it combines data ingestion, transformation, reusable functions, visualization, and automated testing.

## Project Objectives

- Load smartphone data safely from a CSV file.
- Keep only the columns required for downstream analysis.
- Remove records with missing `battery_capacity` or `os`.
- Convert the original price values into dollar amounts by dividing by 100.
- Create reusable, readable chart labels from DataFrame column names.
- Visualize a selected variable against smartphone price, with operating system used as a grouping variable.
- Verify the cleaning transformation with an automated unit test.

## Repository Structure

```text
smartphone-data-production-review/
├── notebook.ipynb
├── data/
│   └── smartphones.csv
├── README.md
└── requirements.txt
```

## Data Preparation Workflow

The notebook defines `prepare_smartphone_data(file_path)`.

The function:

1. Checks whether the supplied CSV path exists.
2. Reads the raw data with pandas.
3. Selects these analysis columns:
   - `brand_name`
   - `os`
   - `price`
   - `avg_rating`
   - `processor_speed`
   - `battery_capacity`
   - `screen_size`
4. Drops records where `battery_capacity` or `os` is missing.
5. Converts the price field to dollar values by dividing it by 100.
6. Returns the cleaned DataFrame.

This keeps the transformation logic in one reusable function instead of repeating cleaning steps throughout the notebook.

## Visualization

The notebook defines `column_to_label()` to turn technical column names such as:

```text
processor_speed
```

into readable plot labels such as:

```text
Processor Speed
```

`visualize_versus_price(clean_data, x)` then creates a Seaborn scatter plot with:

- the selected variable on the x-axis,
- `price` on the y-axis,
- smartphone operating system represented by `hue`,
- automatically generated axis labels and chart title.

The demonstrated visualization compares **processor speed with smartphone price**.

## Testing

The notebook uses `pytest` and `ipytest`.

The test fixture prepares a clean smartphone DataFrame using the same production cleaning function. The `test_nan_values` test then verifies that the resulting `battery_capacity` and `os` columns contain no missing values.

A successful run ends with:

```text
ExitCode.OK
```

This provides a simple regression check: if someone later changes the cleaning function and accidentally allows missing values through, the test can expose the problem.

## Key Skills Demonstrated

- Python
- pandas
- Seaborn
- Matplotlib
- Jupyter Notebook
- Data cleaning
- Reusable functions
- Data visualization
- Unit testing with pytest
- Notebook-based data workflows
- Production-readiness/code review thinking

## How to Run

Install the dependencies:

```bash
pip install -r requirements.txt
```

Start Jupyter:

```bash
jupyter notebook
```

Open `notebook.ipynb` and run the cells from top to bottom.

The notebook expects the dataset at:

```text
./data/smartphones.csv
```

## What I Learned / Why This Project Matters

This project demonstrates that data analysis is not only about producing a chart. A reliable workflow should also validate file inputs, make transformations explicit, avoid unnecessary repetition, and include tests around important assumptions.

The testing section is especially important because the cleaned dataset is intended to support a downstream procurement decision. A visualization built from inconsistent or incomplete data could lead to misleading conclusions.

## Possible Improvements

For a production application, this notebook could be extended by:

- Moving reusable functions into a `.py` module.
- Adding more unit tests for price conversion and required columns.
- Replacing generic exceptions with specific exception types.
- Adding type hints.
- Avoiding the temporary `print(raw_data.head())` diagnostic statement.
- Adding a command-line or pipeline entry point.
- Saving generated charts to an output directory.
- Adding a formal test suite outside the notebook.
