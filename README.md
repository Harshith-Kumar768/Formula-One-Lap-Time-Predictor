# Formula One Lap Time Predictor

**Python · pandas · scikit-learn · matplotlib**

## 1. Project Overview

This project predicts Formula 1 lap times within a single race using tire age as a feature.

The project compares:
- A **baseline model** without tire age.
- A **tire-age-enhanced model** that includes tire age to capture tire degradation effects.

The models are evaluated using **RMSE** and **MAE**, and predicted versus actual lap times are visualized for a complete stint.

## 2. Dataset

The project uses the Formula 1 World Championship dataset and these CSV files:

- `lap_times.csv`
- `pitstops.csv`
- `race_results.csv`
- `races.csv`

The files are included in the `data/` folder.

## 3. Race Selection

A single race was selected to keep track and race conditions consistent:

**2019 British Grand Prix — Round 10**

Eight classified finishers were used:

1. Lewis Hamilton
2. Valtteri Bottas
3. Charles Leclerc
4. Pierre Gasly
5. Max Verstappen
6. Carlos Sainz
7. Daniel Ricciardo
8. Kimi Räikkönen

## 4. Data Cleaning

The lap-time data was cleaned before modelling.

Cleaning included:

- Removing laps corresponding to pit stops.
- Removing the lap immediately following a pit stop.
- Removing abnormal lap times more than **5 seconds above the driver's median lap time**.
- Removing missing lap-time values.

### Cleaning Results

| Measurement | Count |
|---|---:|
| Original laps | 416 |
| Pit/post-pit laps removed | 24 |
| Slow abnormal laps removed | 47 |
| Missing-time laps removed | 0 |
| Total unique laps removed | **54** |
| Clean laps | **362** |

## 5. Feature Engineering

The following features were created:

- `grid_position` — driver's starting grid position.
- `lapNumber` — race lap number.
- `stint` — tire stint number.
- `tire_age` — number of laps completed on the current tire stint.

## 6. Train/Test Split

A **stint-based split** was used instead of a random split.

For each driver:
- Earlier stints → training data
- Final stint → testing data

Results:
- **Training rows:** 147
- **Testing rows:** 215
- **Training stints:** 2
- **Testing stints:** 2

## 7. Machine Learning Models

Two regression algorithms were compared:

### Random Forest Regressor

Trained with both baseline and tire-age-enhanced features.

### Gradient Boosting Regressor

Trained with both baseline and tire-age-enhanced features.

### Baseline Features

```text
grid_position + lapNumber
```

### Tire-Age Features

```text
grid_position + lapNumber + tire_age
```

## 8. Model Results

Lower RMSE and MAE indicate better prediction performance.

| Model | Feature Set | RMSE (seconds) | MAE (seconds) |
|---|---|---:|---:|
| Random Forest | Baseline | 0.8683 | 0.5528 |
| Random Forest | Tire-age enhanced | **0.8569** | **0.5502** |
| Gradient Boosting | Baseline | 0.7650 | 0.5839 |
| Gradient Boosting | Tire-age enhanced | **0.7649** | 0.5922 |

### Interpretation

- **Random Forest:** adding tire age improved both RMSE and MAE.
- **Gradient Boosting:** adding tire age slightly improved RMSE, but MAE increased slightly.
- **Best RMSE:** Gradient Boosting + tire-age enhanced features = **0.7649 seconds**.
- **Best MAE:** Random Forest + tire-age enhanced features = **0.5502 seconds**.

Overall, tire age provides useful information for lap-time prediction, although its effect differs between models and metrics.

## 9. Predicted vs Actual Visualization

The project includes a full-stint predicted-versus-actual plot.

The visualization compares:

- **Actual:** recorded lap time.
- **Predicted:** lap time predicted by the Gradient Boosting tire-age-enhanced model.

The plot is used to visually assess whether the model follows actual lap-time behaviour across a complete stint.

## 10. Project Outputs

Running the notebook produces:

- `model_comparison.csv`
- `cleaned_lap_data.csv`

The notebook also displays:
- Dataset sizes
- Selected race and drivers
- Cleaning statistics
- Tire-age and stint features
- Train/test sizes
- Model comparison metrics
- Full-stint predicted vs actual visualization
- Automatic interpretation of model results

## 11. How to Run

1. Open **Jupyter Notebook** or **JupyterLab**.
2. Open `F1_Lap_Time_Predictor_FIXED_V2.ipynb`.
3. Select **Kernel → Restart Kernel and Run All Cells**.

The notebook executes the complete data-cleaning, feature-engineering, modelling, evaluation, and visualization pipeline.

## 12. Project Conclusion

This project demonstrates a complete machine-learning workflow for Formula 1 lap-time prediction:

**Data Loading → Cleaning → Feature Engineering → Stint-based Train/Test Split → Model Training → RMSE/MAE Evaluation → Visualization**

The comparison shows that incorporating tire age can improve lap-time prediction performance, particularly for the Random Forest model and slightly for Gradient Boosting in terms of RMSE.
