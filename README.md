# 🛰️ Propaganda Detection

This repository contains resources for prompting LLMs to detect propaganda in social media texts, along with training, validation, and evaluation scripts.  

---

## 📂 Repository Structure

### `/API Calls`
- Contains scripts and examples of how we prompted the models.  
- ⚠️ **Important:**  
  - `validation.ipynb` expects a **structured JSON output**.  
  - Scripts should check if LLM outputs are valid JSON.  
  - If invalid, replace with placeholders (e.g., `-1`).  
  - Exactly **200 items** are required for validation.  

**Prompting pipeline overview:**
1. Extract a JSONL line with its prompt.  
2. From the `{"messages": [{"role": "system", "content": <EXTRACT_THIS> ... }]}` field, extract the system prompt.  
3. Replace the tweet text after `"Text:"` with `"Text: {text}"`.  
4. Use this prompt to query the model.  

---

### `/Training and Validation files`
- Contains training/validation data for:  
  - **Our Schema**  
  - **Sahitaj et al.’s Schema**  
- Split by prompting strategy:  
  - **High only** → `*_highonly.jsonl`  
  - **Main + High** → `*_high_and_main.jsonl` / `*_high_and_main_updated.jsonl`  

**Example JSONL line:**
```json
{
  "messages": [
    {
      "role": "system",
      "content": "You are an expert in propaganda analysis. Always keep in mind that you are only looking for Russian propaganda, not any other type."
    },
    {
      "role": "user",
      "content": "You are an expert in propaganda analysis. Analyze the following text. 
Always keep in mind that you are only looking for russian propaganda, not any other type of propaganda. Any other type of propaganda should be ignored and labeled as \"No Propaganda\".

Text:
\"@USER HTTPURL all you need to hear about Ukraine propaganda\"

1. Identify the high-level category that best describes the text.

High-level categories:
1 — Patriotic & Catchy Appeals  
2 — Popularity Appeals  
3 — Deflections & Distractions  
4 — Emotional & Loaded Persuasion  
5 — Argument Manipulations  
6 — No Propaganda  

Now return the labels in JSON format:
{\"high\": integer}

Follow the format strictly. Return **one integer only**."
    },
    {
      "role": "assistant",
      "content": "{\"high\": 5}"
    }
  ]
}
```


 ### `/golden_test_sets`
- Contains gold-standard test sets as pickle files:  
  - `golden_test_set_our_schema.pkl`  
  - `golden_test_set_sahitaj_schema.pkl`  
- Both sets include **low, main, and high-level labels** and can be used with either prompting strategy.  

---

### `validation.ipynb`
- Requires a **JSON file with exactly 200 objects** in the schema defined under `/API Calls`.  
- Must include at least the predicted class name (`"high"` or `"main_and_high"`).  
- Column/JSON key names can be adjusted in the first cell of the notebook.  
- ⚠️ **Sanity check:**  
  - JSON file must have exactly 200 items.  
  - Order must match the corresponding `.jsonl` / `.pkl` files (already aligned).  
