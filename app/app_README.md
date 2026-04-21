This folder contains the Streamlit web application for the FEMA Disaster Assistance Dashboard.

## File

| File | Description |
|------|-------------|
| `app.py` | Main Streamlit application script |

## How to Run

Make sure you have run the model training script first to generate the required model and data files:

```bash
python models/train_models.py
```

Then launch the app:

```bash
streamlit run app/app.py
```

The app will open in your browser at `http://localhost:8501`.

## Dependencies

Install all required packages before running:

```bash
pip install -r requirements.txt
```

## Dashboard Tabs

| Tab | What it shows |
|-----|---------------|
| Geographic Analysis | US choropleth map, top states and counties |
| Trends Over Time | Year-over-year assistance and applicant trends |
| Socioeconomic Relationships | Interactive scatter plots and correlation analysis |
| Model Results | Model performance charts and feature importance |
| Predict Assistance | Interactive prediction tool using the trained Random Forest model |
