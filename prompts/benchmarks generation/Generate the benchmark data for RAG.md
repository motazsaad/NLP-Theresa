# Generate the benchmark data for RAG

You can generate benchmark data for KPI extraction in the ESG domain instead of collecting it manually. This is particularly useful if real ESG reports are scarce or if you want a controlled dataset for testing RAG models. Below are different approaches to generating such data.


## Approaches to Generating ESG KPI Benchmark Data
A. Synthetic Data Generation with LLMs (GPT, Llama, Claude)

* Use LLMs to create realistic ESG reports containing KPIs.
* Define company names, industries, and standard ESG KPIs.
* Ensure outputs follow real ESG reporting standards (GRI, ESRS, SASB, etc.).

Example Prompt for Generating ESG Reports

```Prompt
Generate a corporate ESG report for a technology company in 2023. 
Include sections on Environmental, Social, and Governance performance. 
Report at least five KPIs per section, specifying the metric, unit, and a brief analysis.
Ensure the report follows Global Reporting Initiative (GRI) standards.
```

Generated Output Example

```plaintext
Company: TechCorp  
Year: 2023  

**Environmental KPIs:**  
- Carbon Emissions: 4.2 metric tons CO₂ per employee  
- Energy Consumption: 12,500 MWh (100% renewable sources)  

**Social KPIs:**  
- Workforce Diversity: 40% female employees  
- Employee Training Hours: 35 hours per employee per year  

**Governance KPIs:**  
- Board Diversity: 30% women on the board  
- Data Privacy Incidents: 2 reported breaches  
```

B. ESG-Specific KPI Templates

* Create predefined templates for each industry (tech, finance, manufacturing).
* Randomly populate values to simulate real variations.


Example KPI Template for the Energy Sector

```json
{
  "company": "EnergyCorp",
  "year": 2023,
  "environmental": {
    "carbon_emissions": {
      "value": 3.5,
      "unit": "metric tons CO₂ per MWh"
    },
    "water_usage": {
      "value": 25000,
      "unit": "cubic meters"
    }
  },
  "social": {
    "workforce_diversity": {
      "value": 38,
      "unit": "percentage"
    }
  },
  "governance": {
    "board_diversity": {
      "value": 25,
      "unit": "percentage"
    }
  }
}
```

C. Data Augmentation (Expanding Existing Data)

If you have some ESG reports, apply augmentation techniques:

* Paraphrase KPI descriptions using NLP models.
* Randomly vary numerical values within realistic ranges.
* Shuffle KPI order to avoid pattern memorization

```python 
import random

kpi_templates = [
    {"name": "Carbon Emissions", "unit": "metric tons CO₂", "min": 2, "max": 10},
    {"name": "Workforce Diversity", "unit": "percentage", "min": 30, "max": 50},
    {"name": "Board Diversity", "unit": "percentage", "min": 20, "max": 40}
]

def generate_random_kpi():
    kpi = random.choice(kpi_templates)
    value = round(random.uniform(kpi["min"], kpi["max"]), 2)
    return {"kpi_name": kpi["name"], "value": value, "unit": kpi["unit"]}

# Generate 5 random KPIs
generated_kpis = [generate_random_kpi() for _ in range(5)]
print(generated_kpis)

```


D. Retrieval-Based Synthetic Generation

* Use a database of real ESG reports to retrieve sample KPI descriptions.
* Combine retrieved text + numerical variations to create a semi-synthetic dataset.

👉 This approach mimics real company reports while ensuring the dataset is varied.



## Validating the Synthetic Data
Before using generated benchmark data, validate it with: ✅ Domain Experts: Ensure realistic KPI values.
* ✅ Automated Checks: Verify format consistency.
* ✅ Diversity Checks: Cover multiple industries & KPI categories.

## Evaluating KPI Extraction on Generated Data
Once the dataset is ready, use it for RAG KPI extraction evaluation:

* Exact Match (EM): Compare extracted KPI with generated ground truth.
* F1 Score: Check if extracted text overlaps with correct KPIs.
* BLEU/ROUGE: Compare extracted vs. generated KPI text similarity.
* Hallucination Detection: Ensure extracted KPIs exist in source text.