# Restaurant Rating Prediction

A small Streamlit app that predicts a restaurant's aggregate rating (out of 5) based on details like location, cuisine, cost, and votes. Built on top of the Zomato Restaurants dataset using a Gradient Boosting Regressor.

## What it does

You fill in details about a restaurant — country, city, cuisine type, cost for two, whether it takes table bookings, number of votes, etc. — and the app spits out a predicted rating. It's the same kind of problem Zomato itself deals with: figuring out how a new or unrated place might be perceived before it has enough reviews to go on.

Worth knowing upfront: the model leans almost entirely on **Votes** to make its prediction (it accounts for something like 98% of the model's decision-making). This isn't a bug — it reflects a real pattern in the data, where restaurants with more votes tend to have more "settled" ratings. So don't be surprised if changing the cuisine or location barely moves the number. Votes is what really drives it.

## Files

```
app.py              - the Streamlit app
gb_model.pkl         - the trained model (Gradient Boosting Regressor)
requirements.txt     - Python dependencies
```

## Running it locally

1. Clone or download this folder, and make sure `gb_model.pkl` sits in the same directory as `app.py`.

2. Install the dependencies:
   ```
   pip install -r requirements.txt
   ```

3. Run the app:
   ```
   streamlit run app.py
   ```

4. It should open automatically in your browser at `http://localhost:8501`. If not, the terminal will print the link.

## A note on the categories

The dropdowns for City, Locality, Cuisines, and Currency are built from the actual unique values in the original training dataset (not made up), so predictions stay consistent with how the model was trained. Some of these lists are long — Cuisines alone has over 1,800 entries — so the dropdowns can take a second to load or scroll through, especially on a slower connection.

One field, "Switch to order menu," is hardcoded to "No" in the app because every single row in the training data had that same value. There was nothing to learn from it, so there's no point asking the user for it.

## Model details

- **Algorithm:** Gradient Boosting Regressor (scikit-learn)
- **scikit-learn version:** 1.6.1 (pinned in requirements.txt — using a newer version will throw a version-mismatch warning, and results may not be 100% identical)
- **Target:** Aggregate rating (0–5 scale)
- **Dataset:** Zomato Restaurants dataset, ~9,500 restaurants across 15 countries

## Known limitations

- Since the model relies so heavily on Votes, it's not great at judging brand-new restaurants that don't have any votes yet — the prediction will basically default to whatever a 0-vote restaurant typically looks like in the training data, regardless of how nice the place actually is.
- Locality and Cuisines have very low importance in the model, so don't expect big swings in the prediction just from changing those.
- This was trained on a fixed snapshot of Zomato data from a few years back, so it won't reflect newer restaurants, cities, or currency changes.
