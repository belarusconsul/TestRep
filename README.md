# Pure Python command-line RSS reader


### Homework for EPAM Python Training 2021.09


### Author: Nickolai Lychagin, belarusconsul@gmail.com
### Version 1.3, 20.10.2021

> This program:
> 
> - downloads information from RSS news feed
> - processes various data from news items (title, description, date, image, link)
> - prints data to console in text or JSON format 
> - stores downloaded data to local cache file in SQL format
> - retrieves information from cache for a particular date
> - converts data to HTML and PDF formats and saves it to a local file
>
> It supports all news feeds that fully comply with RSS 2.0 Specification. Correct operation on other RSS channels is not guaranteed. 

---
## INSTALLATION

1. Clone repository to your local drive:<br>
   `git clone https://github.com/belarusconsul/Homework.git`

2. You can run this program without installation. The executable script is in the master branch:<br>
   *<path to /Homework/NickolaiLychagin/rss_reader/rss_reader.py>*
   
3. Install prebuilt package to your computer:<br>
   **Windows:**<br>
   `py -m pip install <path to /Homework/NickolaiLychagin/dist/rss_reader-1.3-py3-none-any.whl>`<br>
   **Unix/MacOS:**<br>
   `python3 -m pip install <path to /Homework/NickolaiLychagin/dist/rss_reader-1.3-py3-none-any.whl>`<br>

   This installs program from a Wheel file (built distribution). If you prefer to use a Source Distribution file (source archive) run:<br>
   **Windows:**<br>
   `py -m pip install <path to /Homework/NickolaiLychagin/dist/rss_reader-1.3.tar.gz>`<br>
   **Unix/MacOS:**<br>
   `python3 -m pip install <path to /Homework/NickolaiLychagin/dist/rss_reader-1.3.tar.gz>`<br>

   After that you can run program through CLI utility rss_reader.

4. If you want to build the distribution package yourself, go to *<path to /Homework/NickolaiLychagin/>* in the master branch and run:<br>
   **Windows:**<br>
   `py -m build`<br>
   **Unix/MacOS:**<br>
   `python3 -m build`

5. Most of the program has been written using Python's 3.9 standard library. Installation of additional modules for basic functionality is not required. In order to convert news to PDF format xhtml2pdf should be installed. If current program is installed through a Wheel file or a Source Distribution file, xhtml2pdf module is installed automatically. Otherwise you have to install it with its dependencies:<br>
   **Windows:**<br>
   `py -m pip install xhtml2pdf`<br>
   **Unix/MacOS:**<br>
   `python3 -m pip install xhtml2pdf`

---
## USAGE

1. Without installation:<br>
	**Windows:**<br>
	`rss_reader.py <arguments>` (run from directory with rss_reader.py) or<br>
	`py -m rss_reader <arguments>` (if directory with rss_reader.py is in sys.path)<br>
	**Unix/MacOS:**<br>
	`python3 rss_reader.py <arguments>`(run from directory with rss_reader.py) or<br>
	`python3 -m rss_reader <arguments>`(if directory with rss_reader.py is in sys.path)<br>

2. With installation of CLI utility:<br>
	**Windows/Unix/MacOS:**<br>
	`rss_reader <arguments>`

```
    usage: rss_reader.py [-h] [--version] [--json] [--verbose] [--limit] [--date] [--clean]
    [--to-html] [--to-pdf] [source]

    positional arguments:
      source         RSS URL

    optional arguments:
      -h, --help  show this help message and exit
      --version   Print version info
      --json      Print result as JSON in stdout
      --verbose   Outputs verbose status messages
      --limit     Limit news topics if this parameter provided
      --date      Date formatted as YYYYMMDD
      --clean     Clean all data from cache file
      --to-html   Convert news to HTML format (provide path to folder or file *.html)
      --to-pdf    Convert news to PDF format (provide path to folder or file *.pdf)
```

---
## FOLDER STRUCTURE

### dist - Distribution files:
- rss_reader-1.3-py3-none-any.whl - Wheel file (built distribution).
- rss_reader-1.3.tar.gz - Source Distribution file (source archive).

### rss_reader - Package for RSS reader.

#### css - CSS files:
- arial.ttf - Font with Cyrillic language support.
- css_html.css - CSS file for HTML pages.
- css_pdf.css - CSS file for PDF files.

#### files - Various files:
- rss_cache.db - SQL database.
- rss_man.png - Callback image for xhtml2pdf module.

#### modules - Subpackage for different functionality areas:
- \__init__.py - Subpackage initialization file.
- rss_reader_dates.py - Parse and reformat dates.
- rss_reader_files.py - Saving to files functionality.
- rss_reader_sql.py - SQL cache functionality.
- rss_reader_text.py - String processing.
- rss_reader_xml.py - Download and process XML data.

#### tests - Supbackage for testing:
- \__init__.py - Subpackage initialization file.
- tests.py - Tests for RSS reader.

#### \__init__.py - Package initialization file.

#### \__main__.py - Allow program to run by package name.

#### rss_reader.py - Main program file.

### .gitignore 
### CHANGELOG.md
### LICENSE
### pyproject.toml
### README.md
### setup.cfg  

---
## CACHE

News items retrieved from source URL are stored in a local SQL database in the 'news' table created as follows:
```
CREATE TABLE news
  (id INTEGER PRIMARY KEY AUTOINCREMENT,
   channel TEXT, 
   url TEXT, 
   title TEXT, 
   date TEXT,
   date_as_date DATE, 
   desc TEXT, 
   image TEXT, 
   link TEXT UNIQUE)
```
If a news item does not have date or date is in the wrong format, it is saved to cache with the date "19000101" (January 1, 1900).

---
## SAMPLE OUTPUT

> In text format:
1. From URL:
```
Feed: The Latest News from the UK and Around the World | Sky News
Description: Sky news delivers breaking news, headlines and top stories from business, politics, entertainment and more in the UK and worldwide.
URL: https://feeds.skynews.com/feeds/rss/home.xml

Title: Man kills five with bow and arrows in Norway before 'confrontation' with police
Date: Wed, 13 Oct 2021 19:54:00 +0100
Image: https://e3.365dm.com/21/10/70x70/skynews-kongsberg-arrow_5545741.jpg?20211013220801
Detail: Five people have been killed and others - including an off-duty police officer - were injured in a series of bow and arrow attacks in Norway, according to police.
Read more: http://news.sky.com/story/norway-bow-and-arrow-attacks-five-killed-in-kongsberg-before-suspect-in-confrontation-with-police-12433212

Title: EU offers to cut 80% of GB-Northern Ireland checks on some goods to end 'sausage war'
Date: Wed, 13 Oct 2021 14:52:00 +0100
Image: https://e3.365dm.com/21/10/70x70/skynews-comp-larne_5545586.jpg?20211013184651
Detail: The EU has offered to cut 80% of checks on some goods moving from Great Britain to Northern Ireland in an effort to avoid a post-Brexit trade clash.
Read more: http://news.sky.com/story/brexit-eu-offers-to-cut-80-of-gb-northern-ireland-checks-on-some-goods-to-end-sausage-war-12433021
```
2. From cache file:
```
RSS news for October 13, 2021 from channel 'The Latest News from the UK and Around the World | Sky News'

Title: Man kills five with bow and arrows in Norway before 'confrontation' with police
Date: Wed, 13 Oct 2021 19:54:00 +0100
Image: https://e3.365dm.com/21/10/70x70/skynews-kongsberg-arrow_5545741.jpg?20211013220801
Detail: Five people have been killed and others - including an off-duty police officer - were injured in a series of bow and arrow attacks in Norway, according to police.
Read more: http://news.sky.com/story/norway-bow-and-arrow-attacks-five-killed-in-kongsberg-before-suspect-in-confrontation-with-police-12433212

Title: EU offers to cut 80% of GB-Northern Ireland checks on some goods to end 'sausage war'
Date: Wed, 13 Oct 2021 14:52:00 +0100
Image: https://e3.365dm.com/21/10/70x70/skynews-comp-larne_5545586.jpg?20211013184651
Detail: The EU has offered to cut 80% of checks on some goods moving from Great Britain to Northern Ireland in an effort to avoid a post-Brexit trade clash.
Read more: http://news.sky.com/story/brexit-eu-offers-to-cut-80-of-gb-northern-ireland-checks-on-some-goods-to-end-sausage-war-12433021
```

> In JSON format:
1. From URL:
```
{
    "Channel": {
        "Title": "The Latest News from the UK and Around the World | Sky News",
        "Description": "Sky news delivers breaking news, headlines and top stories from business, politics, entertainment and more in the UK and worldwide.",
        "URL": "https://feeds.skynews.com/feeds/rss/home.xml"
    },
    "News 1": {
        "Title": "Man kills five with bow and arrows in Norway before 'confrontation' with police",
        "Date": "Wed, 13 Oct 2021 19:54:00 +0100",
        "Image": "https://e3.365dm.com/21/10/70x70/skynews-kongsberg-arrow_5545741.jpg?20211013220801",
        "Description": "Five people have been killed and others - including an off-duty police officer - were injured in a series of bow and arrow attacks in Norway, according to police.",
        "Link": "http://news.sky.com/story/norway-bow-and-arrow-attacks-five-killed-in-kongsberg-before-suspect-in-confrontation-with-police-12433212"
    },
    "News 2": {
        "Title": "EU offers to cut 80% of GB-Northern Ireland checks on some goods to end 'sausage war'",
        "Date": "Wed, 13 Oct 2021 14:52:00 +0100",
        "Image": "https://e3.365dm.com/21/10/70x70/skynews-comp-larne_5545586.jpg?20211013184651",
        "Description": "The EU has offered to cut 80% of checks on some goods moving from Great Britain to Northern Ireland in an effort to avoid a post-Brexit trade clash.",
        "Link": "http://news.sky.com/story/brexit-eu-offers-to-cut-80-of-gb-northern-ireland-checks-on-some-goods-to-end-sausage-war-12433021"
    }
}
```
2. From cache file:
```
{
    "News 1": {
        "Title": "Man kills five with bow and arrows in Norway before 'confrontation' with police",
        "Channel": "The Latest News from the UK and Around the World | Sky News",
        "Channel URL": "https://feeds.skynews.com/feeds/rss/home.xml",
        "Date": "Wed, 13 Oct 2021 19:54:00 +0100",
        "Image": "https://e3.365dm.com/21/10/70x70/skynews-kongsberg-arrow_5545741.jpg?20211013220801",
        "Description": "Five people have been killed and others - including an off-duty police officer - were injured in a series of bow and arrow attacks in Norway, according to police.",
        "Link": "http://news.sky.com/story/norway-bow-and-arrow-attacks-five-killed-in-kongsberg-before-suspect-in-confrontation-with-police-12433212"
    },
    "News 2": {
        "Title": "EU offers to cut 80% of GB-Northern Ireland checks on some goods to end 'sausage war'",
        "Channel": "The Latest News from the UK and Around the World | Sky News",
        "Channel URL": "https://feeds.skynews.com/feeds/rss/home.xml",
        "Date": "Wed, 13 Oct 2021 14:52:00 +0100",
        "Image": "https://e3.365dm.com/21/10/70x70/skynews-comp-larne_5545586.jpg?20211013184651",
        "Description": "The EU has offered to cut 80% of checks on some goods moving from Great Britain to Northern Ireland in an effort to avoid a post-Brexit trade clash.",
        "Link": "http://news.sky.com/story/brexit-eu-offers-to-cut-80-of-gb-northern-ireland-checks-on-some-goods-to-end-sausage-war-12433021"
    }
}
```