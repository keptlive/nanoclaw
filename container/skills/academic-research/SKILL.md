---
name: academic-research
description: Search academic papers, citations, and scholarly sources across multiple databases. Use when user needs research papers, literature reviews, citation analysis, author lookups, or scientific evidence. Triggers on "papers on", "find research about", "literature review", "academic sources", "citations for", "who published".
allowed-tools: Bash(python3:*), Bash(pip3:*)
---

# Academic Research

Search and analyze scholarly literature across multiple free academic databases using Python APIs.

## Available Databases

| Database | Coverage | API | Auth |
|----------|----------|-----|------|
| **Semantic Scholar** | 200M+ papers, all disciplines | `semanticscholar` Python pkg | Optional key (better rate limits) |
| **arXiv** | 2M+ preprints (STEM) | `arxiv` Python pkg | None |
| **CrossRef** | 150M+ DOIs, journal metadata | `crossrefapi` Python pkg | None (add email for polite pool) |
| **OpenAlex** | 250M+ works, bibliometrics | REST API | None (add email) |
| **PubMed** | 36M+ biomedical citations | E-utilities REST | Optional NCBI key |
| **CORE** | 46M full-text open access | REST API | Free key required |

## Quick Usage

### Search for papers on a topic
```python
from semanticscholar import SemanticScholar
sch = SemanticScholar()
results = sch.search_paper("transformer attention mechanisms", limit=10)
for paper in results:
    print(f"[{paper.year}] {paper.title}")
    print(f"  Citations: {paper.citationCount}")
    print(f"  URL: {paper.url}")
```

### Search arXiv preprints
```python
import arxiv
search = arxiv.Search(query="large language models safety", max_results=10, sort_by=arxiv.SortCriterion.SubmittedDate)
for result in arxiv.Client().results(search):
    print(f"[{result.published.strftime('%Y-%m-%d')}] {result.title}")
    print(f"  {result.summary[:200]}...")
    print(f"  PDF: {result.pdf_url}")
```

### Look up citations for a DOI
```python
from crossref.restful import Works
works = Works()
paper = works.doi("10.1038/s41586-024-07421-0")
print(f"Title: {paper['title'][0]}")
print(f"Citations: {paper.get('is-referenced-by-count', 'N/A')}")
```

### Search OpenAlex for bibliometrics
```python
import requests
resp = requests.get("https://api.openalex.org/works", params={
    "search": "CRISPR gene editing therapeutic",
    "sort": "cited_by_count:desc",
    "per_page": 10,
    "mailto": "research@agentwire.email"
})
for work in resp.json()["results"]:
    print(f"[{work['publication_year']}] {work['title']} (cited {work['cited_by_count']}x)")
```

### Search PubMed
```python
import requests
resp = requests.get("https://eutils.ncbi.nlm.nih.gov/entrez/eutils/esearch.fcgi", params={
    "db": "pubmed", "term": "mRNA vaccine efficacy 2024", "retmax": 10, "retmode": "json"
})
ids = resp.json()["esearchresult"]["idlist"]
# Fetch details
details = requests.get("https://eutils.ncbi.nlm.nih.gov/entrez/eutils/esummary.fcgi", params={
    "db": "pubmed", "id": ",".join(ids), "retmode": "json"
})
for uid, info in details.json()["result"].items():
    if uid == "uids": continue
    print(f"[{info.get('pubdate','')}] {info.get('title','')}")
    print(f"  {info.get('source','')} | PMID: {uid}")
```

## Research Workflow

1. **Identify databases**: Match topic to best databases (biomedical→PubMed, CS→arXiv+Semantic Scholar, general→OpenAlex+CrossRef)
2. **Search broadly**: Run 2-3 queries per database with different keywords
3. **Filter and rank**: Sort by citation count, recency, or relevance
4. **Get details**: Fetch abstracts, citation counts, author info
5. **Build bibliography**: Format citations with DOIs and URLs
6. **Analyze patterns**: Citation networks, publication trends, key authors

## Evidence Hierarchy

| Level | Source Type | Weight |
|-------|-----------|--------|
| 1 | Systematic reviews, meta-analyses | Highest |
| 2 | RCTs, large cohort studies | High |
| 3 | Observational studies, case series | Medium |
| 4 | Expert opinion, editorials | Lower |
| 5 | Preprints (not peer-reviewed) | Lowest (flag clearly) |

## Citation Format

Use consistent format:
```
Author et al. (Year). "Title". Journal/Venue. DOI/URL
```

Always note if a source is a preprint (not peer-reviewed).

## Tips

- Semantic Scholar is best for citation graphs and related papers
- arXiv is best for cutting-edge CS/physics/math (but not peer-reviewed)
- CrossRef is best for DOI resolution and journal metadata
- OpenAlex is best for large-scale bibliometric analysis
- PubMed is essential for anything biomedical
- Use agent-browser for paywalled papers (check institutional access, Sci-Hub alternatives)
