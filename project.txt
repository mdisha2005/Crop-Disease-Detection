import streamlit as st
import numpy as np
import cv2
import random

# ---------------- PAGE CONFIG ----------------
st.set_page_config(
    page_title="Crop Disease Detector",
    page_icon="🌱",
    layout="centered"
)

# ---------------- CLASS LABELS ----------------
CLASS_NAMES = [
    "Apple Scab",
    "Apple Black Rot",
    "Healthy Leaf",
    "Tomato Late Blight",
    "Tomato Leaf Mold"
]

# ---------------- TITLE ----------------
st.title("🌱 Crop Disease Detector AI")
st.write("Upload a plant leaf image to detect disease")

# ---------------- FILE UPLOAD ----------------
uploaded_file = st.file_uploader(
    "📤 Upload Leaf Image",
    type=["jpg", "jpeg", "png"]
)

if uploaded_file is not None:
    file_bytes = np.asarray(bytearray(uploaded_file.read()), dtype=np.uint8)
    image = cv2.imdecode(file_bytes, cv2.IMREAD_COLOR)

    st.image(image, caption="Uploaded Image", use_column_width=True)

    if st.button("🔍 Predict Disease"):
        with st.spinner("Analyzing image..."):

            # 🔴 FAKE PREDICTION (Demo)
            predicted_class = random.choice(CLASS_NAMES)
            confidence = round(random.uniform(0.80, 0.99), 2)

        # ---------------- RESULTS ----------------
        st.success(f"🌿 Prediction: {predicted_class}")
        st.info(f"📊 Confidence: {confidence}")

        # ---------------- RECOMMENDATIONS ----------------
        st.subheader("💡 Recommendation")

        if "blight" in predicted_class.lower():
            st.warning("Use fungicide spray and remove affected leaves.")
        elif "rot" in predicted_class.lower():
            st.warning("Improve drainage and avoid overwatering.")
        elif "scab" in predicted_class.lower():
            st.warning("Apply fungicide and prune infected areas.")
        elif "healthy" in predicted_class.lower():
            st.success("Plant is healthy. Maintain proper care!")
        else:
            st.info("Consult agricultural expert for treatment.")

# ---------------- FOOTER ----------------
st.markdown("---")
st.caption("🚀 AI Crop Disease Detection | Streamlit App (Demo Version)")