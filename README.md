# Bengaluru House Price Prediction

<img width="908" height="425" alt="image" src="https://github.com/user-attachments/assets/ceaa8ac5-04a4-47f7-a3c2-bb1f0c2e5568" />

Lightweight Flask backend + simple client for predicting Bengaluru house prices using saved artifacts.

## Quick overview
- Backend: [server/server.py](server/server.py) exposes endpoints implemented in functions [`get_location_names`](server/server.py) and [`predict_home_price`](server/server.py). It relies on helper functions in [`server/util.py`](server/util.py): [`util.load_saved_artifacts`](server/util.py), [`util.get_location_names`](server/util.py), and [`util.get_estimated_price`](server/util.py).
- Client: static files in `client/` for a minimal UI: [client/app.html](client/app.html), [client/app.js](client/app.js), [client/app.css](client/app.css).
- Model & artifacts: training data and saved metadata in `model/` and `server/artifacts/`: [model/bengaluru_house_prices.csv](model/bengaluru_house_prices.csv), [model/columns.json](model/columns.json), and [server/artifacts/columns.json](server/artifacts/columns.json). Notebook: [model/main.ipynb](model/main.ipynb).

## Files
- [client/app.html](client/app.html) — frontend HTML
- [client/app.js](client/app.js) — frontend JS that calls the API
- [client/app.css](client/app.css) — frontend styling
- [server/server.py](server/server.py) — Flask server and routes
- [server/util.py](server/util.py) — model loading and prediction helpers (`util.load_saved_artifacts`, `util.get_location_names`, `util.get_estimated_price`)
- [server/artifacts/columns.json](server/artifacts/columns.json) — saved columns/metadata
- [model/bengaluru_house_prices.csv](model/bengaluru_house_prices.csv) — raw dataset
- [model/columns.json](model/columns.json) — columns exported by notebook
- [model/main.ipynb](model/main.ipynb) — training notebook

## Setup

1. (Recommended) Create and activate a virtual environment:
```
python -m venv .venv
# Windows
.venv\Scripts\activate
# macOS / Linux
source .venv/bin/activate
```
2. Install dependencies (Flask plus typical data libs — adjust as needed):
```
pip install Flask pandas numpy scikit-learn
```
3.Run

Start the server (it will call `util.load_saved_artifacts` on startup):
```
python [server.py](http://_vscodecontentref_/0)
```
Default Flask address: http://127.0.0.1:5000

Open client/app.html in a browser (or point the JS to the running server).

4. API
GET /get_location_names
Returns JSON with available locations (uses util.get_location_names).
Example:
```
curl http://127.0.0.1:5000/get_location_names
```
POST /predict_home_price
Expects form fields: total_sqft, location, bhk, bath. Returns estimated price (uses util.get_estimated_price).
Example:
```
curl -X POST \  -F "total_sqft=1000" \  -F "location=Whitefield" \  -F "bhk=2" \  -F "bath=2" \  http://127.0.0.1:5000/predict_home_price
```

Notes
The server sets Access-Control-Allow-Origin: * in responses to allow the static client to call the API.
If you update the model/artifacts, re-run the notebook model/main.ipynb to export updated files into server/artifacts/.
For production use, remove debug settings and run behind a production WSGI server.
Contact
See code and helpers in server/util.py and endpoints in server/server.py for implementation details.
