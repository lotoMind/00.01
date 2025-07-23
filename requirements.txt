import streamlit as st
import pandas as pd
import altair as alt

st.set_page_config(page_title="לוח מחוונים - ניתוח לוטו", layout="wide")

st.title("📊 אינדיקטור לוטו - גרסת בטא")

# טען נתונים מדומים (אפשר להחליף בקובץ אמיתי)
@st.cache_data
def load_data():
    data = {
        "תאריך": pd.date_range(start="2024-01-01", periods=20, freq="W"),
        "מספרים": [
            [1, 5, 12, 18, 25, 31],
            [3, 11, 19, 27, 33, 36],
            [4, 9, 15, 21, 28, 37],
            [2, 6, 14, 22, 30, 35],
            [7, 10, 17, 20, 29, 34],
            [8, 13, 16, 23, 26, 32],
            [1, 5, 12, 18, 25, 31],
            [3, 11, 19, 27, 33, 36],
            [4, 9, 15, 21, 28, 37],
            [2, 6, 14, 22, 30, 35],
            [7, 10, 17, 20, 29, 34],
            [8, 13, 16, 23, 26, 32],
            [1, 5, 12, 18, 25, 31],
            [3, 11, 19, 27, 33, 36],
            [4, 9, 15, 21, 28, 37],
            [2, 6, 14, 22, 30, 35],
            [7, 10, 17, 20, 29, 34],
            [8, 13, 16, 23, 26, 32],
            [4, 7, 13, 19, 26, 33],
            [2, 8, 14, 20, 27, 34],
        ]
    }
    return pd.DataFrame(data)

df = load_data()

# הצגת טבלה
st.subheader("📅 הגרלות אחרונות")
st.dataframe(df, use_container_width=True)

# ניתוח שכיחות מספרים
st.subheader("🔢 שכיחות מספרים")

all_numbers = sum(df["מספרים"].tolist(), [])
freq = pd.Series(all_numbers).value_counts().sort_index()
freq_df = pd.DataFrame({
    "מספר": freq.index,
    "כמות הופעות": freq.values
})

bar_chart = alt.Chart(freq_df).mark_bar().encode(
    x=alt.X('מספר:O', sort=None),
    y='כמות הופעות:Q',
    tooltip=['מספר', 'כמות הופעות']
).properties(width=700, height=400)

st.altair_chart(bar_chart, use_container_width=True)

# חזוי פשוט - טופ 6 מספרים
st.subheader("📈 חיזוי פשוט (Top 6 מספרים נפוצים):")
top_numbers = freq_df.sort_values("כמות הופעות", ascending=False).head(6)
st.write("🔮 התחזית להגרלה הבאה:")
st.write("🎯", sorted(top_numbers["מספר"].tolist()))
