# 🌍 Agri-Urban Thermal Recycling & Microclimate Resilience Platform

**A FortyGuard Hackathon'26 Submission**  
**Track:** Resilient Cities & Predictive Models  
**Target Spatial Zone:** Phoenix, Arizona (Peri-Urban Agri-Industrial Corridors)

---

## 📑 Executive Abstract
Intensive peri-urban agricultural facilities (e.g., high-density poultry and livestock sheds) emit massive quantities of uncaptured metabolic heat. Concurrently, expanding urban borders face severe Urban Heat Island (UHI) effects. 

This platform leverages **FortyGuard’s 2-meter resolution Hyperlocal Temperature API** integrated with an advanced **Psychrometric & Bio-Thermal Physics Engine**. By moving beyond simple sensible heat calculations, this system incorporates the Ideal Gas Law for dynamic air density and applies avian latent enthalpy multipliers to accurately map saturated agri-industrial heat plumes. This decision-support tool closes the energy loop, modeling how recovering biological waste heat can offset fossil-fuel demands and mitigate localized microclimate extremes.

---

## 🏗️ System Architecture

The repository is modularly designed into three core computational pipelines:

1. **`api_client.py` (Geospatial Data Ingestion)**
   * Connects to the FortyGuard REST API to retrieve high-fidelity thermal grids.
   * Transforms JSON payloads into robust `GeoPandas` DataFrames.
   * Reprojects spherical coordinates (`EPSG:4326`) to planar Web Mercator (`EPSG:3857`) for accurate physical distance and area calculations.

2. **`physics_engine.py` (Advanced Psychrometric Model)**
   * Computes dynamic air mass flow by factoring in local barometric pressure and temperature via the Ideal Gas Law.
   * Differentiates between dry industrial exhaust and saturated biological exhaust, applying empirical latent heat multipliers to capture the massive enthalpy of avian macroscopic thermogenesis.
   * Scales operational hours dynamically based on seasonal brooding cycles.

3. **`app.py` (Interactive Geospatial Dashboard)**
   * A `Streamlit` and `Pydeck` (Deck.gl) 3D visualization frontend.
   * Features dynamic "What-If" sliders to simulate district-level mitigation interventions and active psychrometric tracking.

---

## 🧮 Scientific Methodology & Mathematical Models

To ensure academic and engineering validity, the thermal recovery potential abandons static air density assumptions. 

### 1. Dynamic Mass Airflow (Ideal Gas Law)
Air density ($\rho$) is dynamically calculated based on the ambient exhaust temperature ($T$ in Kelvin) and the specific barometric pressure of the target region ($P$), adjusting for elevations such as the Phoenix metropolitan basin:
$$\rho = \frac{P}{R_{\text{specific}} \cdot T}$$
Where $R_{\text{specific}}$ is the specific gas constant for dry air ($287.058\text{ J/kg}\cdot\text{K}$). The mass flow rate ($\dot{m}$) is then derived from the volumetric ventilation rate.

### 2. Psychrometric Bio-Enthalpy Recovery
Standard industrial heat recovery models only calculate sensible heat. Because high-density avian environments are heavily saturated with respiratory and evaporative moisture, this engine incorporates a Latent Multiplier ($\text{LM}$) to capture the energy of condensation within the heat exchanger matrix. Total instantaneous recovery ($Q_{\text{total}}$) is defined as:
$$Q_{\text{total}} = \dot{m} \cdot C_p \cdot (T_{\text{exhaust}} - T_{\text{ambient}}) \cdot \text{LM} \cdot \eta_{\text{thermal}}$$
*   $C_p$: Specific heat capacity of air ($\approx 1.005\text{ kJ/kg}\cdot\text{K}$)
*   $\text{LM}$: Latent Multiplier ($1.35$ for saturated avian bio-plumes; $1.0$ for dry commercial exhaust)
*   $\eta_{\text{thermal}}$: Heat exchanger operational efficiency

Equivalent carbon offsets are calculated by displacing Liquefied Petroleum Gas (LPG) heating for brooding cycles, utilizing an energy density of $25.3\text{ kWh/liter}$ and an emission factor of $1.51\text{ kg CO}_2\text{e per liter}$.

---

## 🚀 Installation & Local Deployment

### 1. Environment Setup
Ensure you have Python 3.10+ installed. A virtual environment is recommended.
```bash
git clone [https://github.com/your-username/fortyguard-thermal-loop.git](https://github.com/your-username/fortyguard-thermal-loop.git)
cd fortyguard-thermal-loop
python -m venv venv
source venv/bin/activate  # On Windows use: venv\Scripts\activate

