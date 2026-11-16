# OrbitCare-NASA-SpaceApps-2026
Predictive health monitoring dashboard for astronauts using XGBoost and synthetic analog data.

Here is the comprehensive technical documentation for **OrbitCare**. You can save this as `DOCUMENTATION.md` in your GitHub repository or use these sections directly in your final project submission page to explain your engineering and design choices to the judges.

---

# 🌌 OrbitCare: Technical & System Documentation

## 1. Research & Idea Implementation (The Core Concept)

### The Medical Challenge

During long-duration spaceflight, physiological changes do not happen in isolation. Our research identified the **"Calcium-Hormone Loop"** as a critical danger zone for astronauts:

1. Microgravity induces rapid bone density loss.
2. Bone degradation dumps excess calcium into the bloodstream and urine.
3. High urinary calcium, combined with mission-induced dehydration, spikes the risk of kidney stones.
4. Concurrently, disruptions in women's reproductive health (menstrual irregularities) lower estrogen levels, which further accelerates bone loss, amplifying the entire cycle.

### The Implementation Strategy

Existing health monitoring relies heavily on bulky diagnostic imaging (ultrasounds/X-rays) or time-consuming medical logging, which strains crew schedules.

* **Our Solution:** We translated 38 clinical symptoms and subjective biomarkers into a lightweight, 60-second daily questionnaire.
* **Data Privacy Bypass:** Because real NASA Lifetime Surveillance of Astronaut Health (LSAH) data is restricted by HIPAA and privacy laws, we built a Python-based synthetic data generator. This generator mathematically models the biological relationships described above, allowing us to train a robust Machine Learning algorithm on clinical analog data without violating privacy constraints.

---

## 2. UI/UX & Design Philosophy

The user interface was conceptualized using **Figma AI** and **Figma Make**, prioritizing extreme efficiency and cognitive ease for mentally fatigued astronauts.

### Design Principles

* **Zero-Friction Logging:** The daily assessment avoids text entry. It relies exclusively on binary toggles (Normal/Abnormal) and visual sliders (0-10 pain scales) to ensure the checklist takes less than 60 seconds to complete.
* **Space-Friendly Dark Mode:** The dashboard utilizes a high-contrast, dark-navy/black color palette to reduce eye strain in dimly lit spacecraft modules and avoid sleep-cycle disruption.
* **Traffic-Light Color Coding:** Risk scores are visualized instantly using universal urgency colors: 🟢 Green (Optimal), 🟡 Yellow (Monitoring), 🟠 Orange (Warning), and 🔴 Red (Critical Intervention).

### Core User Flow

1. **The Daily Check-In:** Astronaut opens the tablet app and completes the 38-point slider/toggle checklist.
2. **The Forecasting Dashboard:** The screen updates to show 5 risk dials spanning three timelines: Current Risk, Tomorrow's Risk, and Near-Future Risk.
3. **Explainable Insights:** If a dial turns Red/Orange, the astronaut clicks it to see the "Root Cause Breakdown" (e.g., *70% driven by dehydration and 30% by reported flank pain*).
4. **Actionable Protocols:** The UI displays a concrete next step, such as *“Hydration Protocol: Consume 800ml water immediately.”*

---

## 3. Backend & ML Architecture

The backend of OrbitCare is powered by a high-performance, tree-based machine learning architecture designed for tabular medical data.

### Machine Learning Engine

* **Algorithm:** `XGBoost` (Extreme Gradient Boosting) wrapped in a `MultiOutputRegressor` (via Scikit-learn).
* **Why XGBoost?** Biological data is highly non-linear (e.g., a drop in hydration only causes a kidney stone if calcium is simultaneously high). XGBoost excels at finding these hidden, multi-variable edge cases.
* **Multi-Output Advantage:** Instead of training 5 separate models for 5 different health conditions, our single Multi-Output Regressor predicts the risk for Kidney Stones (Now/Future), Bone Loss, Spinal Pain, and Reproductive Disruption simultaneously, saving massive computational overhead.

### Explainable AI (XAI)

Medical software cannot operate as a "black box." Our backend leverages XGBoost's native `feature_importances_` attribute. When a high risk is predicted, the backend dynamically calculates exactly which of the 38 questionnaire inputs mathematically contributed the most to that specific score. This allows the system to justify its predictions to both astronauts and flight surgeons.

---

## 4. System Architecture Workflow

If deployed in a real mission architecture, OrbitCare follows this data pipeline:

1. **Client (Frontend):** Astronaut submits the daily JSON payload containing 38 integer values via the React/HTML interface.
2. **API Layer (Backend):** A lightweight Python API (FastAPI or Flask) receives the payload.
3. **Inference (ML Model):** The API passes the payload to the serialized model (`astronaut_questionnaire_model.pkl`), which runs the inference in milliseconds.
4. **Analysis Engine:** The backend extracts the risk percentages and the top 3 contributing feature importances.
5. **Response:** The API returns the structured data back to the frontend, which dynamically renders the warning dials and medical countermeasure text.

---

## 5. Future Roadmap & Scalability

* **Wearable Integration:** Future iterations could passively ingest data from astronaut wearables (e.g., heart rate variability, sleep stages, skin temperature) to reduce the 38-question manual input down to just 10 subjective pain questions.
* **Long-Term Trend Analysis:** Integrating Facebook's `Prophet` library to analyze how an astronaut's risk scores fluctuate over a 2-year Mars transit, allowing for long-term predictive trajectory mapping.
* **Edge Computing:** Ensuring the entire FastAPI backend and `.pkl` model run locally on a spacecraft server (Edge AI), guaranteeing the system functions perfectly even during total communication blackouts with Earth.


click here to see my live project:
https://serene-liter-76026895.figma.site
