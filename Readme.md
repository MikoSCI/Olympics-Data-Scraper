

# Olympics Data Scraper

This project is a Python script designed to scrape data from [Olympedia](https://www.olympedia.org/) for various Olympic editions. It extracts results from specified Olympic events, organizes them into structured Excel files, and downloads them automatically. The script is optimized for use in Google Colab and supports concurrent file downloads to improve efficiency.

## Features
- Scrapes Olympic event results from multiple editions listed in `BASE_URLS`.
- Extracts discipline and result data using BeautifulSoup.
- Saves results as Excel files with separate sheets for each event and a summary sheet with links.
- Supports concurrent file downloads without waiting for the entire scraping process to complete.
- Includes progress bars using `tqdm` for better user experience.
- Handles errors gracefully with detailed logging.

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
1. Open the script (`scraper.py`) in your preferred environment (e.g., Google Colab, Jupyter Notebook, or local Python).
2. Update the `BASE_URLS` list in the script with the Olympedia edition URLs you want to scrape.
3. Run the script:
   ```bash
   python scraper.py
   ```
   Or, in Google Colab, execute the cell containing the script.
4. The script will:
   - Fetch data for each Olympic edition.
   - Process disciplines and results.
   - Save results as Excel files (e.g., `Olympic_Games_2020.xlsx`).
   - Automatically download each file as it’s created.

## Example Output
For each Olympic edition, the script generates an Excel file with:
- A sheet for each event containing result data (e.g., athlete names, rankings).
- A "Linki" sheet listing event names and their corresponding URLs.

Example file: `Olympic_Games_2020.xlsx`

## Notes
- The script respects Olympedia’s `robots.txt` with a 1-second delay between requests (though a 10-second delay is recommended for compliance).
- Concurrent downloads are handled using Python’s `threading` module to avoid blocking the main loop.
- Ensure your environment supports `IPython.display.Javascript` for Colab downloads. For non-Colab environments, you may need to modify the download mechanism.

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

