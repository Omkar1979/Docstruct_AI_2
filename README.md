# Challenge 1B: Persona-Centric Section Extractor from PDFs

## 🧠 Objective
Build a fully offline, Dockerized solution that extracts the most relevant sections from a set of PDFs, personalized to a given persona and task. The solution uses both traditional ML and Sentence Transformers for semantic understanding.

---

##  Directory Structure

```
Challenge_1b/
├── Collection_1/                  # Contains challenge1b_input.json, PDFs, and stores output
├── Collection_2/
├── Collection_3/
├── MyModel.joblib                 # Trained classifier model
├── MyEncoder.joblib              # Label encoder for headings
├── all-MiniLM-L6-v2/             # SentenceTransformer model folder (saved locally)
├── process_collection.py         # Main processing script
├── requirements.txt
├── Dockerfile
└── README.md
```

---

##  How the Pipeline Works

1. Load joblib ML model and SentenceTransformer (offline)
2. For each document in a collection:
   - Extract visual features from lines
   - Predict headings (`H1`, `H2`, etc.)
   - Merge headings & paragraphs per page
3. Compute semantic similarity to the given task
4. Rank relevant sections and extract supporting text

---

##  Docker Instructions

###  Build Image

```bash
docker build --platform=linux/amd64 -t challenge1b:latest .
```

### ▶️ Run for Collection_1

```bash
docker run --rm -v ${PWD}/Collection_1:/app/Collection_1 challenge1b:latest python process_collection.py Collection_1
```

### ▶️ Run for Collection_2

```bash
docker run --rm -v ${PWD}/Collection_2:/app/Collection_2 challenge1b:latest python process_collection.py Collection_2
```

### ▶️ Run for Collection_3

```bash
docker run --rm -v ${PWD}/Collection_3:/app/Collection_3 challenge1b:latest python process_collection.py Collection_3
```

> 🔒 Each command runs fully offline and processes only the specified collection.

---

## ⚙️ Requirements

- Model: `MyModel.joblib` (scikit-learn classifier)
- Encoder: `MyEncoder.joblib`
- SBERT model: `all-MiniLM-L6-v2/` (pre-downloaded using `.save()`)

Install locally if needed:
```bash
pip install -r requirements.txt
```

---

##  Collection Format

Each collection folder (e.g., `Collection_1`, `Collection_2`, etc.) should follow this structure:

Collection_X/
├── pdfs/ # Folder containing input PDFs
├── challenge1b_input.json # Task + persona + document list
└── challenge1b_output.json # Output will be saved here


> 📝 **Note**: For each collection, the following are expected: `pdfs`, `challenge1b_input.json`, and `challenge1b_output.json`.  
> You can create your own collection folders by adding new PDFs and crafting your own `challenge1b_input.json` for custom testing.




##  Output Format

Each collection will generate a file named `challenge1b_output.json` with:
```json
{
  "metadata": {
    "input_documents": [...],
    "persona": "...",
    "job_to_be_done": "...",
    "processing_timestamp": "..."
  },
  "extracted_sections": [
    {
      "document": "file1.pdf",
      "section_title": "Section Title",
      "importance_rank": 1,
      "page_number": 3
    }
  ],
  "subsection_analysis": [
    {
      "document": "file1.pdf",
      "refined_text": "relevant paragraphs",
      "page_number": 3
    }
  ]
}


eg 

{
  "metadata": {
    "input_documents": [
      "html.pdf",
      "nodejs.pdf",
      "javascript.pdf",
      "cssnew.pdf"
    ],
    "persona": "web developer",
    "job_to_be_done": "what is html",
    "processing_timestamp": "2025-07-28T13:35:14.904093"
  },
  "extracted_sections": [
    {
      "document": "html.pdf",
      "section_title": "HTML Tags",
      "importance_rank": 1,
      "page_number": 2
    },
    {
      "document": "html.pdf",
      "section_title": "HTML  Basics",
      "importance_rank": 2,
      "page_number": 1
    },
    {
      "document": "html.pdf",
      "section_title": "Objectives: Prerequisites: What is an html File?",
      "importance_rank": 3,
      "page_number": 1
    },
    {
      "document": "html.pdf",
      "section_title": "Basic HTML Tags",
      "importance_rank": 4,
      "page_number": 4
    },
    {
      "document": "html.pdf",
      "section_title": "Other HTML Tags",
      "importance_rank": 5,
      "page_number": 6
    },
    {
      "document": "html.pdf",
      "section_title": "HTML Images",
      "importance_rank": 6,
      "page_number": 13
    },
    {
      "document": "html.pdf",
      "section_title": "HTML Links",
      "importance_rank": 7,
      "page_number": 12
    }
  ],
  "subsection_analysis": [
    {
      "document": "html.pdf",
      "refined_text": "What are HTML tags?\nHTML tags are used to mark-up HTML elements\n<html>\nA good way to learn HTML is to look at how other people have coded their html pages. To find out,\nof an HTML document. Most commonly, CSS is combined with the markup languages HTML",
      "page_number": 2
    },
    {
      "document": "html.pdf",
      "refined_text": "HTML is a format that tells a computer how to display a web page. The documents themselves are\nWhat is an html File?\nHTML stands for Hyper Text Markup Language\nHTML Tags\nAn HTML file is a text file containing small markup tags",
      "page_number": 1
    },
    {
      "document": "html.pdf",
      "refined_text": "HTML  Basics\n(HTML). HTML is the building block for web pages. You will learn to use HTML to author an HTML page\nOne thing you should avoid using is a word processor (like Microsoft Word) for authoring your HTML\nHTM or HTML Extension?\nDeveloping simple Web Pages using HTML or XHTML.",
      "page_number": 1
    },
    {
      "document": "html.pdf",
      "refined_text": "Defines an HTML document\nThe most important tags in HTML are tags that define headings, paragraphs and line breaks.\nMicrosoft FrontPage: Microsoft has developed a popular HTML editor called\nfor all the HTML tags.\nallows you to build interactivity into otherwise static HTML pages.",
      "page_number": 4
    },
    {
      "document": "html.pdf",
      "refined_text": "old html.</p>\n</html>\n</html>\nthe <script>... </script> HTML tags in a web page.\nThe most common character entity in HTML is the non-breaking space &nbsp; . Normally HTML will",
      "page_number": 6
    },
    {
      "document": "html.pdf",
      "refined_text": "writing our Embedded CSS in an HTML document. The following snippet shows how to use\n<p>This is web page body </p>\nIn the following section, we will see how we can place JavaScript in an HTML file in\none folder up from the html document that called\nFollowing is the example showing you how to import a style sheet file into an HTML document:",
      "page_number": 13
    },
    {
      "document": "html.pdf",
      "refined_text": "HTML Images\neditor and plain old html.</p>\nHTML uses the <a> anchor tag to create a link to another document or web page.\n<p>By learning html, I'll be able to create web pages like a pro. ...<br>\n├── escape-html@1.0.1",
      "page_number": 12
    }
  ]
}

```

---

##  Adobe Submission Compliance

| Constraint                            | Met? |
|--------------------------------------|------|
| Offline-only execution               | ✅   |
| CPU-only usage                       | ✅   |
| No internet during inference         | ✅   |
| Fully containerized with Docker      | ✅   |
| Collection-wise processing allowed   | ✅   |
| Inference time < 60s for 3–5 PDFs    | ✅   |

---

## 👨‍💻 Author
**Omkar Bhongale**  
Submission for **Adobe India Hackathon 2025 — Challenge 1B**
