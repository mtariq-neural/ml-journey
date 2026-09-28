# Measures of Central Tendency

A measure of central tendency is a statistical metric that represents a typical or central value for a dataset. It provides a clean summary of an entire distribution by identifying a single, core value that is most representative of the data as a whole. 

Instead of looking at thousands of raw numbers, these measures help pinpoint the "center" of the data. In statistics and machine learning, there are five primary types used depending on the shape of the data and the presence of anomalies.

---

## 1. Mean (Arithmetic Average)

The mean is the most common measure of central tendency. It is calculated by adding all the values in a dataset together and dividing the sum by the total number of data points.

*   **Real-World Example:** Calculating the average salary of software engineers at a tech company. If five engineers make \$80k, \$85k, \$90k, \$95k, and \$100k, the mean is \$90k.
*   **Behavior Note:** The mean works perfectly for symmetrically distributed data, but it is highly sensitive to extreme values (outliers). If a CEO making \$1 million is added to that engineer list, the average shoots up drastically, misrepresenting what a typical employee actually makes.

---

## 2. Median (The Middle Value)

The median is the exact middle number in a dataset when all the values are arranged in ascending order. It splits the dataset cleanly into two equal halves: 50% of the data lies below the median, and 50% lies above it.

*   **Real-World Example:** Analyzing housing prices in a major city. If five houses are priced at \$200k, \$250k, \$300k, \$350k, and \$3 million, the median is \$300k. 
*   **Behavior Note:** Unlike the mean, the median completely ignores how extreme the highest or lowest values are. It only cares about position, making it incredibly robust against outliers.

---

## 3. Mode (The Most Frequent Value)

The mode is the value that appears most frequently in a dataset. A dataset can have one mode (unimodal), multiple modes (bimodal/multimodal), or no mode at all if every number is unique.

*   **Real-World Example:** Tracked sales for an e-commerce inventory. If an apparel store sells 50 Medium shirts, 20 Small shirts, and 15 Large shirts, the mode is "Medium".
*   **Behavior Note:** The mode is the only measure of central tendency that works seamlessly with categorical data (like shirt sizes, user locations, or color preferences) where mathematical calculations like adding or ordering are impossible.

---

## 4. Weighted Mean

The weighted mean is an average where some data points contribute more to the final result than others. Instead of treating every value equally, each value is multiplied by a predetermined "weight" based on its importance before averaging.

*   **Real-World Example:** Calculating a student's final course grade where different assessments hold different weight percentages.
*   **Behavior Note:** This is heavily utilized in machine learning cost functions and financial portfolio analysis, where certain features or assets naturally hold higher priority or financial volume than others.

### Mathematical Calculation

The formula for the weighted mean ($\bar{x}_w$) is:

$$\bar{x}_w = \frac{\sum (w_i \cdot x_i)}{\sum w_i}$$

Where $x_i$ represents the values and $w_i$ represents their corresponding weights.

**Scenario:** A student scores **85** on assignments (20% weight), **78** on the midterm exam (30% weight), and **92** on the final exam (50% weight). 

$$\text{Weights } (w) = [0.20, 0.30, 0.50]$$
$$\text{Scores } (x) = [85, 78, 92]$$

$$\bar{x}_w = \frac{(85 \cdot 0.20) + (78 \cdot 0.30) + (92 \cdot 0.50)}{0.20 + 0.30 + 0.50}$$

$$\bar{x}_w = \frac{17.0 + 23.4 + 46.0}{1.0}$$

$$\bar{x}_w = 86.4$$

---

## 5. Trimmed Mean

A trimmed mean is a hybrid approach designed to combine the mathematical benefits of the mean with the outlier resistance of the median. It is calculated by removing a small, fixed percentage of the largest and smallest values from the dataset before finding the average of the remaining numbers.

*   **Real-World Example:** Removing extreme noise or sensor glitches from a dataset gathered by an edge device before feeding it into a machine learning model.
*   **Behavior Note:** In data science pipelines, trimming data is a highly effective preprocessing step to eliminate noise or extreme anomalies without completely losing the distribution of the central data.

### Mathematical Calculation

**Scenario:** A sensor records 10 temperature readings, but it experiences an initial startup lag error (value of 12) and a sudden power surge spike (value of 195). 

$$\text{Dataset (Sorted): } [12, 45, 48, 50, 52, 55, 58, 60, 62, 195]$$

To apply a **20% Trimmed Mean** (10% trimmed from the bottom, 10% trimmed from the top), we remove 1 value from each end ($10\% \text{ of } 10 = 1$ value):

*   Remove the lowest value: **12**
*   Remove the highest value: **195**

$$\text{Remaining Dataset: } [45, 48, 50, 52, 55, 58, 60, 62]$$

Now, we calculate the standard arithmetic mean of the remaining 8 values:

$$\text{Trimmed Mean} = \frac{45 + 48 + 50 + 52 + 55 + 58 + 60 + 62}{8}$$

$$\text{Trimmed Mean} = \frac{430}{8}$$

$$\text{Trimmed Mean} = 53.75$$

*(For comparison, the untrimmed regular mean would be **63.7**, heavily distorted upward by the single 195 outlier).*
