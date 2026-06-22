# Description of the Tool

Create a command line tool in Python that accepts a path to a data directory, reads the single CSV data file contained in that directory, and generates graphical plots saved as PNG images to visualise the mean, minimum, maximum, and standard deviation across each column of the data.

The tool must use NumPy for statistical analysis and Matplotlib for plot generation.

# Assumptions

- The input is a directory path passed as a command line argument.
- The directory contains exactly one CSV data file to process.
- The CSV file contains column-oriented numeric data suitable for statistical analysis.
- The tool is expected to create PNG output files for the requested statistics.
- The prompt does not specify the exact output filename(s), output directory, chart style, or command-line option names.

# User Stories

- As a user, I want to pass a data directory to the tool so that it can locate and process the CSV file inside it.
  - Acceptance criteria: The tool accepts a directory argument from the command line.
  - Acceptance criteria: The tool reads the CSV file found in that directory.

- As a user, I want the tool to calculate mean, minimum, maximum, and standard deviation values for each column so that I can understand the distribution of the data.
  - Acceptance criteria: The tool computes mean values for each column using NumPy.
  - Acceptance criteria: The tool computes minimum values for each column using NumPy.
  - Acceptance criteria: The tool computes maximum values for each column using NumPy.
  - Acceptance criteria: The tool computes standard deviation values for each column using NumPy.

- As a user, I want the tool to generate PNG plots for these statistics so that I can visually inspect the data.
  - Acceptance criteria: The tool produces graphical plots with Matplotlib.
  - Acceptance criteria: The plots are saved as PNG images.
  - Acceptance criteria: The output includes visualisations for mean, minimum, maximum, and standard deviation across each column.

- As a user, I want the tool to report a useful error when the expected CSV input is not available so that I know the input data is invalid.
  - Acceptance criteria: The tool handles a missing or unreadable CSV file with an error message.
  - Acceptance criteria: The tool handles an invalid input directory with an error message.

# Success Metrics

- The tool can be run from the command line with a directory path argument.
- The tool successfully reads the CSV file in the provided directory.
- The tool generates PNG plot files for mean, minimum, maximum, and standard deviation.
- The tool relies on NumPy for statistical calculations and Matplotlib for plotting.
- The tool gives a clear error when the expected input cannot be processed.

# Out of Scope

- Supporting multiple CSV files in the same directory.
- Supporting non-CSV input formats.
- Defining a specific plot theme, style guide, or custom visual design.
- Adding interactive charts or a graphical user interface.
- Modifying or cleaning the source data beyond what is required to compute the requested statistics.
- Defining the exact command-line interface details beyond the required directory argument.