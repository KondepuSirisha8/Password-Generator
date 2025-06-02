import streamlit as st
import random
import string
def generate_password(length, use_upper, use_lower, use_digits, use_special):
    if not (use_upper or use_lower or use_digits or use_special):
        return "⚠️ Select at least one character type."

    character_pool = ""
    if use_upper:
        character_pool += string.ascii_uppercase
    if use_lower:
        character_pool += string.ascii_lowercase
    if use_digits:
        character_pool += string.digits
    if use_special:
        character_pool += string.punctuation

    password = []
    if use_upper:
        password.append(random.choice(string.ascii_uppercase))
    if use_lower:
        password.append(random.choice(string.ascii_lowercase))
    if use_digits:
        password.append(random.choice(string.digits))
    if use_special:
        password.append(random.choice(string.punctuation))

    while len(password) < length:
        password.append(random.choice(character_pool))

    random.shuffle(password)
    return ''.join(password)

st.set_page_config(page_title="Strong Password Generator", page_icon="🔐")
st.title("🔐 Strong Password Generator")

length = st.slider("Select password length", 4, 64, 12)
use_upper = st.checkbox("Include Uppercase Letters (A-Z)", value=True)
use_lower = st.checkbox("Include Lowercase Letters (a-z)", value=True)
use_digits = st.checkbox("Include Digits (0-9)", value=True)
use_special = st.checkbox("Include Special Characters (!@#...)", value=True)

if st.button("Generate Password"):
    result = generate_password(length, use_upper, use_lower, use_digits, use_special)
    if "⚠️" in result:
        st.warning(result)
    else:
        st.success("✅ Password Generated!")
        st.code(result, language='text')
        st.button("Copy", on_click=st.experimental_set_query_params, args=({'password': result},))

