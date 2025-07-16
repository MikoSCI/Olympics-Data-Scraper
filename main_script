import requests
from bs4 import BeautifulSoup
from urllib.parse import urljoin
import pandas as pd
import re
from tqdm.notebook import tqdm
import time
from google.colab import files


BASE_URLS = [
    "https://www.olympedia.org/editions/53",
    "https://www.olympedia.org/editions/54"
] # examples

def get_olympics_title_and_soup(url):
    response = requests.get(url)
    time.sleep(10)  # Crawl-delay according to robots.txt
    soup = BeautifulSoup(response.text, 'html.parser')
    h1 = soup.find('h1')
    title = h1.get_text(strip=True) if h1 else 'unknown_olympics'
    safe_title = re.sub(r'[^\w\s-]', '', title).strip().replace(' ', '_')
    return safe_title, soup

for BASE_URL in tqdm(BASE_URLS, desc="All editions"):
    try:
        olympics_name, soup = get_olympics_title_and_soup(BASE_URL)
        print(f"\n📦 Processing: {olympics_name}")

        discipline_links = [urljoin(BASE_URL, a['href']) for a in soup.select('table a[href]')]
        print(f"[+] Found {len(discipline_links)} disciplines.")

        result_rows = []

        for discipline_url in tqdm(discipline_links, desc="  Disciplines", leave=False):
            try:
                resp = requests.get(discipline_url)
                time.sleep(1)
                sub_soup = BeautifulSoup(resp.text, 'html.parser')
                result_links = [urljoin(BASE_URL, a['href']) for a in sub_soup.select('a[href*="/results/"]')]

                for link in tqdm(result_links, desc="    Results", leave=False):
                    try:
                        result_resp = requests.get(link)
                        time.sleep(1)
                        result_soup = BeautifulSoup(result_resp.text, 'html.parser')
                        header_tag = result_soup.find('h1', class_='event_title')
                        event_name = header_tag.get_text(strip=True) if header_tag else 'No header'

                        table = result_soup.find('table', class_='table table-striped')
                        if table:
                          thead = table.find('thead')
                          tbody = table.find('tbody')

                          # Headers from thead, or from the first tr if thead is missing
                          if thead:
                              headers = [th.text.strip() for th in thead.find_all('th')]
                          else:
                              first_tr = table.find('tr')
                              headers = [td.text.strip() for td in first_tr.find_all(['th', 'td'])] if first_tr else []

                          data_rows = []
                          # If tbody exists, use it; otherwise, use all tr except the first one
                          if tbody:
                              rows = tbody.find_all('tr')
                          else:
                              rows = table.find_all('tr')[1:]  # skip the first one, as it contains headers

                          for tr in rows:
                              cols = [td.text.strip() for td in tr.find_all('td')]
                              if cols:  # skip empty rows
                                  data_rows.append(cols)

                          result_df = pd.DataFrame(data_rows, columns=headers[:len(data_rows[0])])  # fix for column mismatch
                          result_rows.append({
                              'name': event_name,
                              'link': link,
                              'df': result_df
                          })
                        else:
                            print(f"[!] No table with class 'table table-striped' found at: {link}")

                    except Exception as e:
                        print(f"[!] Error at result link: {link} -> {e}")

            except Exception as e:
                print(f"[!] Error at discipline: {discipline_url} -> {e}")

        if not result_rows:
            print(f"❌ No data to save for {olympics_name}")
            continue

        output_file = f"{olympics_name}.xlsx"
        with pd.ExcelWriter(output_file, engine='openpyxl') as writer:
            for row in result_rows:
                sheet_name = re.sub(r'[\/\\\?\*\[\]\:]', '', row['name'])[:31] or "Sheet"
                try:
                    row['df'].to_excel(writer, sheet_name=sheet_name, index=False)
                except Exception as e:
                    print(f"Error saving sheet '{sheet_name}': {e}")

            link_df = pd.DataFrame([{'Event': r['name'], 'Link': r['link']} for r in result_rows])
            try:
                link_df.to_excel(writer, sheet_name='Links', index=False)
            except Exception as e:
                print(f"Error saving 'Links' sheet: {e}")

        print(f"✅ File '{output_file}' ready!")
        files.download(output_file)

    except Exception as e:
        print(f"❌ Error at edition {BASE_URL}: {e}")
