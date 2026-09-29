# Measures of Dispersion

Central tendency (mean, median, mode) tells us where the center of our data sits. But that's only half the picture. Two datasets can have the exact same center and still behave completely differently. To really understand data, especially before feeding it into a model, we need to know how spread out it is. That's what measures of dispersion tell us.

---

## Why Central Tendency Isn't Enough

Let's look at two small datasets that share the same center:

* **Dataset A:** `[-5, 0, 5]`
* **Dataset B:** `[-10, 0, 10]`

If we take the mean of both:

$$\text{Mean}_A = \frac{-5 + 0 + 5}{3} = 0$$

$$\text{Mean}_B = \frac{-10 + 0 + 10}{3} = 0$$

Same average, zero for both. But the values in Dataset B sit twice as far from the center as the values in Dataset A. If we only looked at the mean, we'd think these two datasets are the same. In reality, B is far more volatile. This is exactly the kind of thing a model can miss if we only scale by mean and ignore spread.

---

## 1. Range

The range is the easiest way to check how spread out data is. Just subtract the smallest value from the largest.

$$\text{Range} = \text{Maximum Value} - \text{Minimum Value}$$

**Example:** for `[4, 8, 15, 16, 23, 42]`, the range is $42 - 4 = 38$.

It's quick to compute, but it's also fragile. It only looks at two numbers and ignores everything in between. One weird outlier and your range suddenly looks huge, even if the rest of the data is tightly packed.

---

## 2. Variance

Variance looks at every point in the dataset, not just the extremes. It measures the average squared distance between each value and the mean. We square the distances so that negative and positive differences don't cancel each other out.

There are two versions depending on whether you have the full population or just a sample:

* **Population Variance ($\sigma^2$):** used when you have every data point in the group you care about.

$$\sigma^2 = \frac{\sum (x_i - \mu)^2}{N}$$

* **Sample Variance ($s^2$):** used when you only have a subset and you're trying to estimate the bigger population. We divide by $n - 1$ instead of $n$, since a small sample tends to underestimate the real spread otherwise.

$$s^2 = \frac{\sum (x_i - \bar{x})^2}{n - 1}$$

**Worked example:** let's find the population variance of `[3, 2, 1, 5, 4]`.

1. Find the mean:
   $$\mu = \frac{3 + 2 + 1 + 5 + 4}{5} = 3$$

2. Find how far each value is from the mean, then square it:

| Value ($x$) | Deviation ($x - \mu$) | Squared |
| :--- | :--- | :--- |
| 3 | 0 | 0 |
| 2 | -1 | 1 |
| 1 | -2 | 4 |
| 5 | 2 | 4 |
| 4 | 1 | 1 |

3. Add up the squared deviations: $0 + 1 + 4 + 4 + 1 = 10$

4. Divide by $N = 5$:
   $$\sigma^2 = \frac{10}{5} = 2$$

So the variance is 2.

---

## 3. Standard Deviation

There's one awkward thing about variance. Since we squared the values, the result is in squared units. If your original data was in dollars, variance is now in "dollars squared," which doesn't mean anything to a normal person. Taking the square root of variance brings it back to the original scale. That's standard deviation.

* **Population:** $\sigma = \sqrt{\sigma^2}$
* **Sample:** $s = \sqrt{s^2}$

Using the variance we just found (2):

$$\sigma = \sqrt{2} \approx 1.414$$

This is why standard deviation gets used far more often than variance when explaining spread to people. It's on the same scale as the actual data. It's also the number behind standardization (Z-score scaling), where we shrink every feature so it has a mean of 0 and a standard deviation of 1 before training a model.

---

## 4. Coefficient of Variation (CV)

Standard deviation tells you the spread, but it doesn't tell you if that spread is "a lot" or "a little" without context. A standard deviation of 5 means something very different for a dataset with a mean of 10 versus a mean of 10,000. CV fixes this by dividing standard deviation by the mean, turning it into a percentage.

$$\text{CV} = \left( \frac{\sigma}{\mu} \right) \times 100\%$$

**Worked example:** say we're comparing the daily price swings of two assets.

* **Stock A:** mean price $50, standard deviation $5
  $$\text{CV}_A = \frac{5}{50} \times 100\% = 10\%$$

* **Crypto B:** mean price $50{,}000, standard deviation $2{,}500
  $$\text{CV}_B = \frac{2500}{50000} \times 100\% = 5\%$$

Even though Crypto B's standard deviation ($2,500) looks massive compared to Stock A's ($5), CV shows that Stock A is actually relatively more volatile, 10% of its price versus 5% for B. This is the whole point of CV. It lets you compare spread across things with completely different scales or units, like comparing weight in kilograms to weight in pounds.

---

## The Outlier Problem: Variance vs. Mean Absolute Deviation (MAD)

Variance and standard deviation share one weakness. Because they square the distances, one extreme outlier can throw the whole calculation off. A single freakishly large value gets squared into an even more freakishly large number, and it drags the entire measure with it.

Mean Absolute Deviation (MAD) avoids this by using absolute value instead of squaring:

$$\text{MAD} = \frac{\sum |x_i - \bar{x}|}{n}$$

Since nothing gets squared, one outlier doesn't blow everything out of proportion the same way.

**So why don't we just use MAD everywhere instead of variance?**

It comes down to calculus.

1. **Differentiability:** absolute value creates a sharp corner on a graph, like a "V". You can't find a smooth slope at a sharp corner. Squaring creates a smooth curve (a parabola) that you can differentiate anywhere, which matters a lot for optimization.
2. **Gradient descent:** most ML training relies on gradient descent to minimize error, and gradient descent needs smooth, differentiable functions to work well. That's why squared error shows up everywhere in ML, not just in variance.
3. **Theory:** a lot of statistics, including linear regression (Ordinary Least Squares) and the Central Limit Theorem, is built on squared distances. So even though MAD is more robust to outliers, variance and standard deviation stay the default because the math around them is better developed and plays nicer with the tools we use to train models.