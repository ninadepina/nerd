# Web Scraping: Empowering Data Acquisition for Your Projects

In the world of web development, it's not uncommon to encounter situations where you need to gather data from external sources. While APIs are often the preferred method for accessing data, there are instances when obtaining information through web scraping becomes necessary. In this article, I'll share my experience with web scraping using JavaScript and explain how I applied it to a project.

## The Quest for Data

Recently, [our team](https://github.com/ChrisvanHvA/Appclusive) found ourselves in a situation where we needed content for our application ('Appclusive', app for designers/developers to incorporate accessibility into their project) but had received the API link much later than anticipated. To overcome this hurdle, we took matters into our own hands. After some searching, we stumbled upon a website that had conveniently rewritten the Web Content Accessibility Guidelines (WCAG) into a more user-friendly format. Eager to acquire this valuable content, I decided to scrape the website.

## Getting Started with Web Scraping

As someone who had never attempted web scraping before, I was intrigued by the concept and decided to give it a try. I came across a tutorial called "Web Scraping with Puppeteer & Node.js" on YouTube, which served as an excellent starting point. Surprisingly, I discovered that web scraping wasn't as daunting as I initially thought. Equipped with the knowledge gained from the tutorial, I was ready to dive into the world of web scraping.

## Tackling Hidden Content

During the scraping process, I encountered a challenge. Some items of interest were not immediately visible on the website since they were dynamically loaded upon clicking. These items were not part of the Document Object Model (DOM) and couldn't be scraped directly. However, I discovered a solution: instructing the web scraper to simulate a click on each item, thus revealing the desired information. This enabled me to successfully retrieve the necessary data.

## Scraping and JSON

After overcoming the hurdle of hidden content, I proceeded to scrape the required information and store it in a structured format. I chose to use JSON as it offers simplicity and compatibility with various programming languages. By organizing the scraped data into a JSON file, I ensured its usability across different components of our project.

Now, let's explore the code I utilized for web scraping (please not that variables like `wcagCategoryId` and `wcagItemId` are added for storing the items in a database, they have nothing to do with the actual scraping). Keep in mind that this code assumes you have Node.js and Puppeteer (a Node.js library that provides a high-level API for controlling headless Chrome or Chromium) set up in your development environment.

```js
// Importing necessary libraries
import puppeteer from 'puppeteer';
import { promises as fs } from 'fs';

// Function to initiate web scraping
async function start() {
	const browser = await puppeteer.launch({ headless: 'new' });
	const page = await browser.newPage();
	await page.goto('https://www.a11yproject.com/checklist/');

	// Scraping data using page evaluation
	const data = await page.evaluate(() => {
		// Extracting categories
		const categories = Array.from(document.querySelectorAll('[data-checklist-category]'));
		let wcagCategoryId = 1;
		let wcagItemId = 1;

		const extractItemData = async (item, wcagId, wcagCategoryId) => {
			// Extracting item information
			const description = item.querySelector('.c-checklist__title').innerHTML;
			const detailButton = item.querySelector('.c-checklist__summary');
			await detailButton.click();
			const wcagLink = item.querySelector('.c-checklist__link');
			const title = wcagLink ? wcagLink.textContent.replace(/^\d+\.\d+\.\d+\s*/, '') : null;
			const tip_description = wcagLink ? item.querySelector('.c-checklist__description').innerHTML : null;
			const ref_link = wcagLink ? wcagLink.href : null;

			return {
				wcag_item_id: wcagItemId++,
				title: title,
				description: description,
				wcag_level: '',
				ref_link: ref_link,
				tip_description: tip_description,
				parent_id: wcagCategoryId
			};
		};

		const extractCategoryData = async (category) => {
			// Extracting category information
			const title = category.querySelector('.c-heading-large').textContent;
			const descriptionElement = category.querySelector('.c-preface');
			const description = descriptionElement ? descriptionElement.textContent : null;
			const checklistItems = Array.from(category.querySelectorAll('.c-checklist'));

			// Extracting item data within each category
			const items = await Promise.all(
				checklistItems.map((item) => extractItemData(item, wcagCategoryId, wcagCategoryId))
			);

			return {
				wcag_id: wcagCategoryId++,
				title: title,
				description: description,
				items: items
			};
		};

		return Promise.all(categories.map(extractCategoryData));
	});

	// Updating item and category IDs
	let wcagItemId = 1;
	let wcagCategoryId = 1;

	data.forEach((category) => {
		category.wcag_id = wcagCategoryId++;

		category.items.forEach((item) => {
			item.wcag_item_id = wcagItemId++;
			item.parent_id = category.wcag_id;
		});
	});

	// Writing scraped data to a JSON file
	const dataJSON = JSON.stringify(data, null, 2);
	await fs.writeFile('wcag.json', dataJSON);

	await browser.close();
}

// Initiating the scraping process
start();
```

## The Missing Piece: WCAG Levels

While the website provided valuable content, it lacked a crucial element—the WCAG levels. These levels are essential for assessing website accessibility. However, the website did include links to the official WCAG guidelines. Recognizing an opportunity, I decided to scrape those pages as well. By extracting the titles and levels from each page, I aimed to match them with the corresponding items in the original JSON file.

```js
// Importing necessary libraries
import puppeteer from 'puppeteer';
import fs from 'fs';

// Reading the previously scraped WCAG data from a file
function readWCAGFile() {
	return new Promise((resolve, reject) => {
		fs.readFile('wcag.json', 'utf8', (err, data) => {
			if (err) {
				reject(err);
				return;
			}

			const jsonData = JSON.parse(data);
			const wcagHrefs = [];

			// Extracting WCAG links from the data
			for (const element of jsonData) {
				if (element.items) {
					for (const item of element.items) {
						if (item.ref_link) {
							wcagHrefs.push(item.ref_link);
						}
					}
				}
			}

			resolve(wcagHrefs);
		});
	});
}

// Function to scrape text from a given URL
async function scrapeTextFromUrl(url) {
	const browser = await puppeteer.launch({ headless: 'new' });
	const page = await browser.newPage();
	await page.goto(url);

	// Extracting relevant text content from the page
	const scrapedData = await page.evaluate(() => {
		let elements = Array.from(document.querySelectorAll('.sctxt'));
		if (elements.length === 0) {
			const h1Element = document.querySelector('#main h1');
			if (h1Element) {
				const textContent = h1Element.textContent.trim();
				const colonIndex = textContent.indexOf(':');
				const titleWithLevel = textContent.substring(colonIndex + 1).trim();
				const title = titleWithLevel.split('(')[0].trim();
				const level = titleWithLevel
					.match(/\(Level [A-Z]+\)/)[0]
					.replace(/[()]/g, '')
					.replace('Level ', '');
				return [{ title, level }];
			}
			return [];
		}

		return elements.map((element) => {
			const title = element.textContent
				.split(':')[0]
				.trim()
				.replace(/^\d+\.\d+\.\d+ /, '');
			const level = element.textContent
				.match(/\(Level [A-Z]+\)/)[0]
				.replace(/[()]/g, '')
				.replace('Level ', '');
			return { title, level };
		});
	});

	await browser.close();
	return scrapedData;
}

// Function to scrape text from multiple URLs
async function scrapeTextFromUrls(wcagHrefs) {
	const scrapedDataList = [];

	for (const url of wcagHrefs) {
		const scrapedData = await scrapeTextFromUrl(url);
		scrapedDataList.push(scrapedData);
	}
	return scrapedDataList;
}

// Starting the scraping process
readWCAGFile()
	.then((wcagHrefs) => {
		scrapeTextFromUrls(wcagHrefs)
			.then((scrapedDataList) => {
				// Writing scraped level data to a JSON file
				const jsonContent = JSON.stringify(scrapedDataList);
				fs.writeFile('levels.json', jsonContent, 'utf8', (err) => {
					if (err) {
						console.error('Error writing to file:', err);
					}
				});
			})
			.catch((error) => {
				console.error('Error:', error);
			});
	})
	.catch((error) => {
		console.error(error);
	});
```

## Matching Scraped Levels with WCAG Items

Now that I had the scraped levels stored in a JSON file named `levels.json`, I needed to match them with the corresponding WCAG items. To accomplish this, I utilized the `fs` module to read the necessary files and performed a matching operation. The code snippet below demonstrates how I achieved this:

```js
// Importing necessary libraries
import fs from 'fs';

// Reading the previously scraped data from files
const dataFile = fs.readFileSync('wcag.json');
const levelFile = fs.readFileSync('levels.json');

const data = JSON.parse(dataFile);
const levels = JSON.parse(levelFile);

// Creating a map to match titles with levels
const titleToLevelMap = {};
levels.forEach((item) => {
	if (item.length > 0) {
		const { title, level } = item[0];
		titleToLevelMap[title] = level;
	}
});

// Updating the WCAG items with their respective levels
data.forEach((section) => {
	section.items.forEach((item) => {
		const { title } = item;
		const level = titleToLevelMap[title];
		item.wcag_level = level || 'A';
	});
});

// Writing the updated data to a JSON file
const updatedData = JSON.stringify(data, null, 2);
fs.writeFileSync('data.json', updatedData);
```

That's it! Web scraping proved to be an invaluable tool in extracting WCAG guidelines from a user-friendly website when faced with API delays. Through the use of Puppeteer and Node.js, we were able to navigate the DOM, interact with hidden elements, and scrape the required data. Additionally, we leveraged scraping techniques to retrieve missing information and matched it with the relevant WCAG items. By incorporating our scraping code into our project, we successfully obtained a comprehensive dataset that facilitated the accessibility implementation of our website.

Web scraping opens up a world of possibilities for obtaining data from various sources. However, it's important to exercise caution and adhere to legal and ethical guidelines when scraping websites. Always ensure that you have proper authorization or are scraping from publicly available and non-sensitive information. Happy scraping!
