import numpy as np
import pandas as pd
import plotly.express as px
import streamlit as st

st.set_page_config(
    page_title="U.S. College Yield Rates (2001–2023)",
    page_icon="🎓",
    layout="wide",
)

st.title("🎓 Yield Rate at U.S. Colleges, by Selectivity")
st.caption(
    "The yield rate is the percentage of admitted students choosing to enroll."
)

# Sidebar Filters
st.sidebar.header("Filter Options")

year_range = st.sidebar.slider(
    "Select Year Range",
    min_value=2001,
    max_value=2023,
    value=(2001, 2023),
    step=1,
)

tiers = [
    "Elite (<20% admissions rate)",
    "Selective (20%-40% admissions rate)",
    "Somewhat selective (40%-60% admissions rate)",
    "Less selective (60%-80% admissions rate)",
    "Much less selective (>80% admissions rate)",
]

selected_tiers = st.sidebar.multiselect(
    "Select Selectivity Tiers", options=tiers, default=tiers
)

# Dataset Generation
years = np.arange(2001, 2024)

raw_data = {
    "Year": years,
    "Elite (<20% admissions rate)": [
        38.0,
        39.5,
        39.5,
        39.5,
        38.0,
        37.0,
        38.0,
        36.5,
        35.5,
        35.5,
        36.5,
        37.0,
        37.5,
        37.5,
        38.5,
        40.0,
        42.5,
        44.0,
        40.5,
        47.5,
        48.0,
        49.0,
        50.0,
    ],
    "Selective (20%-40% admissions rate)": [
        37.5,
        37.8,
        36.0,
        36.0,
        36.0,
        35.0,
        33.5,
        33.5,
        33.2,
        32.0,
        31.0,
        30.0,
        29.5,
        29.0,
        28.0,
        28.5,
        28.5,
        28.5,
        25.5,
        27.5,
        27.2,
        26.8,
        26.0,
    ],
    "Somewhat selective (40%-60% admissions rate)": [
        40.0,
        38.5,
        38.5,
        37.5,
        36.5,
        35.5,
        34.0,
        33.0,
        31.5,
        31.0,
        29.5,
        28.0,
        27.5,
        27.5,
        26.5,
        26.0,
        25.5,
        24.8,
        22.0,
        22.0,
        21.8,
        21.0,
        19.8,
    ],
    "Less selective (60%-80% admissions rate)": [
        41.2,
        39.5,
        38.5,
        37.5,
        36.8,
        35.5,
        34.0,
        32.2,
        31.0,
        30.0,
        28.5,
        27.5,
        26.8,
        26.2,
        25.8,
        25.0,
        24.0,
        23.0,
        20.5,
        20.0,
        19.5,
        18.5,
        18.0,
    ],
    "Much less selective (>80% admissions rate)": [
        42.0,
        42.5,
        41.0,
        39.8,
        38.8,
        38.2,
        37.0,
        35.0,
        33.0,
        31.2,
        29.8,
        29.0,
        28.5,
        27.5,
        25.5,
        24.5,
        23.8,
        22.5,
        20.5,
        20.2,
        19.8,
        19.2,
        18.5,
    ],
}

df = pd.DataFrame(raw_data)
df_long = df.melt(
    id_vars=["Year"], var_name="Selectivity Tier", value_name="Yield Rate (%)"
)

# Apply Filters
df_filtered = df_long[
    (df_long["Year"] >= year_range[0])
    & (df_long["Year"] <= year_range[1])
    & (df_long["Selectivity Tier"].isin(selected_tiers))
]

color_map = {
    "Elite (<20% admissions rate)": "#2085c7",
    "Selective (20%-40% admissions rate)": "#23a98d",
    "Somewhat selective (40%-60% admissions rate)": "#f07d32",
    "Less selective (60%-80% admissions rate)": "#ea698b",
    "Much less selective (>80% admissions rate)": "#e63946",
}

# Interactive Chart
fig = px.line(
    df_filtered,
    x="Year",
    y="Yield Rate (%)",
    color="Selectivity Tier",
    color_discrete_map=color_map,
    markers=True,
    title="College Yield Rates Over Time",
)

fig.update_traces(
    line=dict(width=3),
    hovertemplate="<b>%{fullData.name}</b><br>Year: %{x}<br>Yield Rate: %{y:.1f}%<extra></extra>",
)

fig.update_layout(
    xaxis_title="Year",
    yaxis_title="Yield Rate (%)",
    yaxis=dict(range=[10, 55], ticksuffix="%"),
    hovermode="x unified",
    legend_title_text="Selectivity Tier",
    height=600,
    template="plotly_white",
)

st.plotly_chart(fig, use_container_width=True)

# Data Table Display
with st.expander("📊 View Underlying Data"):
    st.dataframe(df_filtered.pivot(index="Year", columns="Selectivity Tier", values="Yield Rate (%)"))

st.caption(
    "Source: U.S. Department of Education, National Center for Education Statistics • Based on 2021 admission rates."
)
