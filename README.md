

# Olympics Data Scraper

This project is a Python script designed to scrape data from [Olympedia](https://www.olympedia.org/) for various Olympic editions. The script retrieves a list of disciplines for each Olympic edition and then collects detailed data about athletes and their results for each discipline. The extracted data is organized into structured Excel files, with each file containing results for a specific Olympic edition.

## Features
- Scrapes Olympic event results from multiple editions listed in `BASE_URLS`.
- Fetches a list of disciplines for each Olympic edition.
- Collects athlete data and results for each discipline, including rankings and event details.
- Saves results as Excel files with separate sheets for each event and a summary sheet with event links.
- Uses progress bars (tqdm) for a user-friendly experience.
- Handles errors gracefully with detailed logging.
- Respects Olympedia’s robots.txt with a configurable crawl-delay (default: 10 seconds for edition pages).


## Prerequisites
- Python 3.6 or higher
- Google Colab environment (for automatic file downloads)
- Required Python packages:
  - `requests`
  - `beautifulsoup4`
  - `pandas`
  - `openpyxl`
  - `tqdm`
  - `IPython` (Colab-specific)

## Installation
1. Clone the repository:
   ```bash
   git clone https://github.com/your-username/olympics-data-scraper.git
   cd olympics-data-scraper
   ```
2. Install the required packages:
   ```bash
   pip install requests beautifulsoup4 pandas openpyxl tqdm
   ```
3. If running in Google Colab, ensure the notebook has access to the internet for HTTP requests.

## Usage
1. Open the script (`main_script.py`) in your preferred environment (e.g., Google Colab, Jupyter Notebook, or local Python).
2. Update the `BASE_URLS` list in the script with the Olympedia edition URLs you want to scrape.
3. Run the script:
   ```bash
   python main_script.py
   ```
   Or, in Google Colab, execute the cell containing the script.
4. The script will:
   - Fetch the list of disciplines for each Olympic edition.
   - Scrape athlete data and results for each discipline.
   - Save results as Excel files (e.g., `Olympic_Games_2020.xlsx`).
   - Automatically download each file after finishing code processing.

## How It Works
1. Fetching Disciplines: The script retrieves the list of disciplines for each Olympic edition by parsing the edition’s main page and extracting links to discipline pages.
2. Scraping Athlete Data: For each discipline, it navigates to result pages, extracts tables containing athlete data (e.g., names, rankings), and organizes them into pandas DataFrames.
3. Saving Results: The data is saved into Excel files, with one sheet per event containing athlete results and a "Links" sheet listing event names and their URLs.

## Example Output
For each Olympic edition, the script generates an Excel file with:
- A sheet for each event containing athlete data (e.g., names, rankings, times, or scores).
- A "Links" sheet listing event names and their corresponding URLs.

Example file: `Olympic_Games_2020.xlsx`

## Notes
- The script respects Olympedia’s `robots.txt` with a 10-second delay between requests.
- Ensure your environment supports google.colab.files.download for automatic downloads in Colab. For non-Colab environments, you may need to modify the download mechanism.
- The script handles cases where tables or headers are missing, logging errors for transparency.

## Contributing
Contributions are welcome! Please:
1. Fork the repository.
2. Create a new branch (`git checkout -b feature-branch`).
3. Make your changes and commit (`git commit -m "Add feature"`).
4. Push to the branch (`git push origin feature-branch`).
5. Open a pull request.

## License
This project is licensed under the MIT License. See the [LICENSE](LICENSE) file for details.

## Acknowledgments
- [Olympedia](https://www.olympedia.org/) for providing the data.
- [BeautifulSoup](https://www.crummy.com/software/BeautifulSoup/) for HTML parsing.
- [Pandas](https://pandas.pydata.org/) for data manipulation.
- [tqdm](https://tqdm.github.io/) for progress bars.

