import streamlit as st
import pandas as pd

# ================= LAGRANGE =================
def lagrange(x, y, x0):
    y0 = 0
    n = len(x)

    for i in range(n):
        term = y[i]
        for j in range(n):
            if i != j:
                term *= (x0 - x[j]) / (x[i] - x[j])
        y0 += term

    return y0


# ================= NEWTON TABLE =================
def divided_difference_table(x, y):
    n = len(x)
    dd = [[0 for _ in range(n)] for _ in range(n)]

    for i in range(n):
        dd[i][0] = y[i]

    for j in range(1, n):
        for i in range(n - j):
            dd[i][j] = (dd[i+1][j-1] - dd[i][j-1]) / (x[i+j] - x[i])

    return dd


# ================= NEWTON =================
def newton(x, dd, x0):
    n = len(x)
    result = dd[0][0]
    product = 1

    for i in range(1, n):
        product *= (x0 - x[i-1])
        result += product * dd[0][i]

    return result


# ================= PAGE CONFIG =================
st.set_page_config(page_title="Interpolation Tool", layout="centered")

st.title("Interpolation Methods")
st.markdown("Lagrange and Newton Divided Difference Implementation")
st.markdown("---")


# ================= INPUT SECTION =================
with st.form("input_form"):

    n = st.number_input("Number of data points", min_value=2, step=1)
    n = int(n)

    st.markdown("Input Data Points")

    x = []
    y = []

    for i in range(n):
        col1, col2 = st.columns(2)

        with col1:
            xi = st.number_input(f"x[{i}]", key=f"x{i}")

        with col2:
            yi = st.number_input(f"y[{i}]", key=f"y{i}")

        x.append(xi)
        y.append(yi)

    x0 = st.number_input("Interpolation value (x0)")

    method = st.selectbox("Select Method", ["Lagrange", "Newton", "Both"])

    submit = st.form_submit_button("Calculate")


# ================= OUTPUT =================
if submit:

    st.markdown("---")

    # validation
    if len(set(x)) != len(x):
        st.error("Duplicate x values are not allowed.")
    else:

        try:

            # ================= LAGRANGE =================
            if method == "Lagrange":
                result = lagrange(x, y, x0)

                st.subheader("Lagrange Result")
                st.markdown(f"### {result}")

            # ================= NEWTON =================
            elif method == "Newton":
                dd = divided_difference_table(x, y)
                result = newton(x, dd, x0)

                st.subheader("Newton Result")
                st.markdown(f"### {result}")

                st.subheader("Divided Difference Table")
                df = pd.DataFrame(dd)

                st.dataframe(df, use_container_width=True)

            # ================= BOTH =================
            else:
                st.subheader("Lagrange Result")
                st.markdown(f"### {lagrange(x, y, x0)}")

                dd = divided_difference_table(x, y)
                result = newton(x, dd, x0)

                st.subheader("Newton Result")
                st.markdown(f"### {result}")

                st.subheader("Divided Difference Table")
                df = pd.DataFrame(dd)

                st.dataframe(df, use_container_width=True)

        except Exception as e:
            st.error(f"Error occurred: {e}")
