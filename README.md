
# Web-Policy Inspector

## Overview :

Web-Policy Inspector is a tool built for researchers working in the field of Governance, Risk, and Compliance (GRC). This project automates the process of crawling websites, identifying subdomains, extracting privacy policy information, and analyzing those policies to detect potential unethical practices, such as data exploitation or breaches in user privacy. The tool uses extensive web crawling, subdomain enumeration, and advanced NLP techniques like BERT-based models for sentiment analysis and summarization.

The theme behind the project is addressing the issue that most users don’t read privacy policies due to their complexity and length. This project aims to simplify the process by automatically summarizing policies and identifying whether a website respects user privacy or exploits data.


## Features :

- Subdomain Enumeration: Using Sublist3r, this tool finds all the subdomains associated with a domain.
- Web Crawling: Selenium is used to scrape content from websites to gather the privacy policy.
- BERT-based NLP Analysis: The tool analyzes the privacy policies by summarizing them and performing sentiment analysis to check for user-friendly or exploitative language.

## Project Workflow :


1.Subdomain Enumeration:
The tool starts by enumerating subdomains of a domain using Sublist3r. This helps identify where the privacy policy page might be located across subdomains.

2.Web Crawling:
After identifying potential subdomains, the tool uses Selenium to crawl and extract the privacy policy text from the relevant pages.

3.BERT-Based Model:
The extracted policy text is then analyzed using a BERT-based model, specifically designed to summarize the text and perform sentiment analysis. It detects whether the website is focused on user protection or data exploitation.

4.Summary & Sentiment:
A summary of the privacy policy is generated, along with sentiment analysis, to give a clear picture of the website's intent regarding user privacy.
## Project Setup :

1.Clone the repository

    git clone https://github.com/yourusername/web-policy-inspector.git
    cd web-policy-inspector

2.Install Required Dependencies

You will need the following dependencies:

- Selenium: For web crawling.
- Transformers: For BERT and sentiment analysis.
- Torch: Required by the transformer models.
- Sublist3r: For subdomain enumeration.

Install the dependencies using the following command:

    pip install -r requirements.txt

Additionally, you will need to install Chromium and ChromeDriver for Selenium to work:

    apt-get update
    apt-get install -y chromium-browser
    apt-get install -y chromium-chromedriver

3.Download the Sublist3r repository

The tool uses Sublist3r for subdomain enumeration. Clone the Sublist3r repository:

    git clone https://github.com/aboul3la/Sublist3r.git
    cd Sublist3r
    pip install -r requirements.txt

4.Run the Script

Once all the dependencies are installed, you can run the script to analyze a domain.

    python webpolicy_inspector.py

5. Input the Domain

- In the script, the domains you want to analyze are taken from the top 1 million domains (top-1m.csv). You can replace it with your own list of domains.
- Adjust path of the custom Domains csv file in the working code of webpolicy_inspector.py file accordingly .
## Code Explaination :

1.Subdomain Enumeration

- The script uses the Sublist3r tool to enumerate subdomains for a given domain. This is essential for finding various endpoints that might host privacy policies.

- def enumerate_subdomains(self, domain):
    !python sublist3r.py -d {domain_name} -o output.txt

2.Web Crawling

- Once the subdomains are identified, Selenium is used to extract the privacy policy text from the pages:

- def scrape_terms(self, url):
    self.get(url)
    policy_text = self.find_element(By.TAG_NAME, "body").text
    return policy_text

3.BERT-Based Analysis

- The extracted policy text is analyzed for intent and summarized using a BERT-based model:

- def analyze_policy(self, text):
    preprocessed_text = self.preprocess_text(text)
    sentiment = self.analyze_sentiment(preprocessed_text)
    intent = self.classify_intent(preprocessed_text)
    summary = self.summarize_policy(preprocessed_text)
    return {
        "Sentiment": sentiment,
        "Intent": intent,
        "Summary": summary,
    }

4.Sentiment and Intent Classification

- The sentiment and intent classification is done using the HuggingFace Transformers library, leveraging a pre-trained BERT model for intent classification and sentiment analysis.

- def classify_intent(self, text):
    inputs = self.tokenizer(text, return_tensors="pt", truncation=True, padding=True, max_length=512)
    with torch.no_grad():
        outputs = self.model(**inputs)
    logits = outputs.logits
    predicted_class = torch.argmax(logits, dim=1).item()
    return predicted_class

5.Output

- The output consists of a summary of the privacy policy and the sentiment and intent classification, which tells whether the site is user-friendly or potentially exploiting user data.

6.Example Output

- Enumerating subdomains for example.com...
- Found 5 subdomains.

        https://terms.example.com/privacy-policy
        Extracted Policy: [Privacy Policy Text Here]
        Sentiment: POSITIVE
        Intent: User Protection
        Summary: The privacy policy ensures data protection and user consent...


## Contribution :

- If you'd like to contribute to this project, feel free to fork the repository, create a new branch, and submit a pull request. We welcome any improvements, bug fixes, or suggestions.

## Author :

- [@Xclusive-Ishan](https://github.com/Xclusive-Ishan)

##Visuals :
![Policy_text_extracted](https://github.com/Xclusive-Ishan/WebPolicy-Inspector/blob/main/Policy_text_extracted.png)

![Summary Generated](https://github.com/Xclusive-Ishan/WebPolicy-Inspector/blob/main/Summary_visible.png)

![Results](https://github.com/Xclusive-Ishan/WebPolicy-Inspector/blob/main/Model_Results.png)

