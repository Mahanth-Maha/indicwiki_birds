# Birds

Birds is one of the domains, which is a part of the IndicWiki Project.

## Description

The aim of this domain is to create a large number of articles (about 10000) about Birds. Hence, we are generating these data-rich articles in telugu for about 10000 Birds, and uploading them to wikipedia, so that people who can read only in their native language (here, telugu) can benefit from this information.

## Installation and setup

Create virtual environment in the project folder using the following commands.

```bash
$ pip install virtualenv
$ pip install python3.9 
$ virtualenv -p python3.9 birdEnv
```
After the successful creation of virtual environment (venv), clone the repository or download the zip folder of the project and extract it. Run the following commands from the Birds (root) directory to activate the virtual environment and install all necessary dependencies.

```bash
$ source birdEnv/bin/activate
$ pip install -r requirements.txt
```
requirements.txt comes along with the Project Directory. 

## Guide to generate XML dump, articles for different Birds

- Clone the repository into the local system.
- Enter the 'curDir' folder inside the cloned repo.
- Open the notebook file genXML.ipynb, Configure `From_n` and `To_n` variables then Execute the notebook by clicking on "Run all", where `From_n` and `To_n` correspond to the row numbers of the first and last desired articles (only serial order is possible). For example `From_n = 30 ` and `To_n = 50` would generate xml dump for Birds in rows 30-50 (inclusive). By default, dump is generated for all Birds (case where no mdifications are done).
- On executing the above Notebook xml dump is obtained in `birds.xml`. If `From_n` and `To_n` were modified then the file name would be birds_{From_n}-{To_n}.xml, names of these files would have the range passed in their filenames (such as `birds_30-50.xml` etc).


## Github Structure

- The Birds (root) folder contains files as 
	- requirements.txt 	→ python requirements file
	- birds.xml			→ the xml dump for all birds
	- report.html 		→ sweetviz report of the final dataset of birds

The folder curDir contains the entire implementation corresponding to article generation. This directory contains the codebase for the new model, converting knowledge base to intermediate structured article (dataframe of labeled article sentences) which can be modified and updated by users and then converting this data to XML page which can be imported in mediawiki.

It can be found [here](https://github.com/indicwiki-iiit/Birds/tree/main/curDir). All the below explained files and folders are inside this directory.



### data

> Github folder Link: https://github.com/indicwiki-iiit/Birds/tree/main/curDir/data
- This folder contains various datasets (final and intermediate), and the implementations as to how they were obtained. The python scripts in this folder are described as follows:
	- _<File>_ 		→ <Description>.
	- _<File>_ 		→ <Description>.
	- _<File>_ 		→ <Description>.
	- _Birds.csv_ → This is a csv file containing all the data related to birds.
	- _Birds.html_ → HTML code of the bird articles.
	- _Birds.pkl_ → This pickle file contains birds data.
	- _Birds.xlsx_ → Excel file containing data of all the 12000+ birds including all their attributes values.
	- _Birds\_Dataset\_English\_only.xlsx_ → Excel file containing data of all the birds only in english
	- _Dataset\_final.xlsx_ → Final Dataset of all the birds including both english and telugu translated values of attributes.
	
### scrape_new_data

> Github folder Link: https://github.com/indicwiki-iiit/Birds/tree/main/curDir/scrape_new_data
- This folder contains implementation and datasets corresponding to newly scraped data from Birds.org.in. The python scripts in this folder are described as follows:
	- _data\_cleaning.py_ → This file contains implementation which handles basic data cleaning, such as making missing values uniform, identifying edge cases and removing unwanted attributes.
	- _extract\_info.py_ → This file contains the actual scraping implementation for scraping the complete data for all Birds in Andhra Pradesh and Telangana. Similar to `zeroMain.py`, this file can be provided with command line arguments for scraping data for a particular subsegment of Birds. For example `python3 extract_info.py 30 50` would scrape data for Birds in rows 30-50 in intermediate dataframe `Birds_id_name_url_dataframe.pkl` (inclusive). By default, data is scraped for all Birds (case where no command line arguments are provided, such as `python3 extract_info.py`).
	- _extract\_urls.py_ → This file contains implementation for web crawling and obtaining, storing urls for each school from the two states, so as to minimize overhead while actually scraping content from them.
	- _notability\_check.py_ → This file filters out non-sparse rows (well populated rows) and stores them separately, as these can be highlighted while generating articles.
- The pickle files and datasets of this folder are described as follows:
	- _school\_urls\_dict.pkl_ → This file consists of a dictionary, where school UDISE codes are the keys and that school's url in the above website is the corresponding value.
	- _Birds\_id\_name\_url\_dataframe.pkl_ → This pickle file consists of an intermediate dataframe, where each row contains a school's UDISE code, its corresponding name, and its url in Birds.org.in. 
	- _Birds\_id\_name\_url\_dataframe.xlsx_ → This excel file consists of the same intermediate dataframe above, in excel format.
	- _Birds\_org\_data.xlsx_ → This excel file contains the complete dataset for all the data scraped from all the Birds.
	- _Birds\_org\_data\_part\_i.pkl_ → These 3-part pickle files contain the same information as in excel file above, but have been split to 3 parts due to size constraints.
	- _notable\_Birds\_org\_data.pkl_ → This pickle file consists of data corresponding to notable Birds alone (about 14% of the total data collected - for the states Andhra Pradesh and Telangana).
	- _notable\_Birds\_org\_data.xlsx_ → This excel file consists of the same dataframe above, in excel format.
	- _translated\_dataset\_notable\_Birds.pkl_ → This dataset consists of all attributes which require translation while rendering in articles. There are about 15 such attributes and their corresponding english and telugu versions are present here (summing up to 30). Note that these were collected only for notable Birds.
	- _translated\_dataset\_notable\_Birds.xlsx_ → This excel file consists of the same dataframe above, in excel format.

### template

> Github folder Link: https://github.com/indicwiki-iiit/Birds/tree/main/curDir/template
- This folder .
	- _new\_teluguDFtext.j2_ → This file consists of the improved template to generate wikitext provided a school's data, corresponding to latest dataset (from Birds.org.in)
	- _teluguDFtext.j2_ → This file consists of the template to generate the wikitext of a given school's data (based on old dataset - oneKB)
	- _teluguTitle.j2_ → This jinja2 template file generates the title for a given school.

### createMgnt.py

> Github file Link: https://github.com/indicwiki-iiit/Birds/blob/main/curDir/createMgnt.py
- This file contains implementation which generates a dataframe where each row contains a school's managament details in english and its corresponding translation in telugu.

### getJsData.py

> Github file Link: https://github.com/indicwiki-iiit/Birds/blob/main/curDir/getJsData.py
- This file contains implementation which stores the data for each school (its title, code) in .js format in provided path.

### help.py

> Github file Link: https://github.com/indicwiki-iiit/Birds/blob/main/curDir/help.py
- This file contains implementation of pre-processing and more convenient translation for frequently occuring words, phrases and numbers etc.

### onePage.xml

> Github file Link: https://github.com/indicwiki-iiit/Birds/blob/main/curDir/onePage.xml
- This file contains how a sample xml dump might look for a particular school.

### schoolXML5k_1.xml

> Github file Link: https://github.com/indicwiki-iiit/Birds/blob/main/curDir/schoolXML5k_1.xml
- This file contains how a sample xml dump might look for a multiple school articles in the same xml file.

### template\_functions.py

> Github file Link: https://github.com/indicwiki-iiit/Birds/blob/main/curDir/template_functions.py
- This file contains various python functions which are used in edge case handling (different sentences based on attributes available) while rendering the article from template based on data provided.

### trans.py

> Github file Link: https://github.com/indicwiki-iiit/Birds/blob/main/curDir/trans.py
- This file contains implementation which deals with translation, transliteration of attributes along with string cleaning (with the help of regular expressions and libraries like anuvaad, deeptranslit) and exception handling.

### transNNP.py

> Github file Link: https://github.com/indicwiki-iiit/Birds/blob/main/curDir/transNNP.py
- This file acts a helper file for `trans.py` and assists in translation of english tokens.

### write.py

> Github file Link: https://github.com/indicwiki-iiit/Birds/blob/main/curDir/write.py
- This file contains implementation which generates an article and its title by rendering the jinja templates, given a particular school's data.

### create\_translated\_dataset.py

> Github file Link: https://github.com/indicwiki-iiit/Birds/blob/main/curDir/create_translated_dataset.py
- This script generates a translated dataset for all notable Birds, including all attributes which require translation while rendering in articles (there are about 15 such english attributes).

### merge\_translated\_dataset.py

> Github file Link: https://github.com/indicwiki-iiit/Birds/blob/main/curDir/merge_translated_dataset.py
- This script merges the translated dataset (once its reviewed) with the original dataset, to avoid on-the-fly translation.

### zeroMain.py

> Github file Link: https://github.com/indicwiki-iiit/Birds/blob/main/curDir/zeroMain.py
- This file constructs article(s) for a given list of school code(s) and then segments and structures each article. It constructs an xml file for the given school code(s). (More details are explained in the section 'Guide to generate XML dump')


### Sample Article

You can find the sample article [here](https://docs.google.com/document/d/1zx1IG2G8MDLfH21ofY1Z8AEoV-bv2KEe8bbjNDWFsxc/edit)

