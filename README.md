# Copula Lab

**Two interactive pages for learning and building bivariate models with copulas.**

| Page | What it does | Link |
|---|---|---|
| **Copula Lab** | Learn the ideas step by step, from correlation to copulas | ▶ [Open the Copula Lab](https://juanciro-commits.github.io/copula-lab/) |
| **Model Builder** | Complete a two-variable model from the pieces you know | ▶ [Open the Model Builder](https://juanciro-commits.github.io/copula-lab/model-builder.html) |

No installation and no account needed: both run entirely in the browser, on a computer or a phone.

---

## What it is for

Copulas answer a simple question: *how do two random variables depend on each other, regardless of what each one looks like on its own?*

The key result is **Sklar's theorem**: any joint distribution can be split into its marginals and a copula,

$$H(x, y) = C\big[F(x),\, G(y)\big]$$

where $F$ and $G$ describe each variable separately and $C$ describes only how they move together.

The lab builds up to that idea in four steps. Each step uses the tool from the step before it.

It was built for **Stochastic Methods for Business Analytics** (MSBA, University of Central Florida) to answer three questions from class:

1. What are the copula families and what do they look like?
2. What are the correlation structures of copulas (with two variables)?
3. What do the level sets of different patterns look like (lognormal, normal)?

---

## Start here: why go beyond correlation

The Pearson correlation and its regression line are the most basic tools for describing how two variables move together. The regression line uses the correlation to predict $Y$ from $X$:

$$\hat{y} = b_0 + b_1 x, \qquad b_1 = r\,\frac{s_y}{s_x}$$

They work well when the relationship is linear, but they only see the linear part.

**On the page:** three scenarios, each with its regression line, Pearson $r$, $r^2$ and Kendall's $\tau$.

- **Linear:** the line fits and Pearson describes the dependence well.
- **Curved** ($Y = e^X$): with no noise the relationship is perfect, so Kendall's $\tau = 1$, yet Pearson stays near 0.6 and the line misses the curve.
- **Heavy tails:** a few extreme points make Pearson and the line jump from sample to sample, while Kendall stays steady.

---

## Two reviews before the steps

### Review A · Density, probability and notation

A pdf gives **heights**, probabilities are **areas**, and the cdf **accumulates** those areas:

$$P(a \le Y \le b) = \int_a^b f(y)\,dy = F(b) - F(a)$$

**On the page:** drag the ends of an interval and watch the shaded area under the pdf match the gap on the cdf. A Uniform(0, 0.25) shows that a density can be larger than 1. A notation table explains each symbol used later ($Y$ vs. $y$, $f$ vs. $F$, $\sim$, $\Phi$, the conditional bar $\mid$, iid).

### Review B · Joint, marginal, conditional and independence

The ideas behind every multivariate model, first with a table you can count: an ice-cream shop's 100 days by weather and sales.

- **Marginal:** add up a row or a column.
- **Conditional:** divide a row by its total.
- **Independence:** every cell equals the product of its row and column totals.

**On the page:** a slider changes how much the weather affects sales. The totals at the edges never move, only the inside of the table: keeping the marginals fixed while changing the dependence is exactly what a copula does. A side-by-side table links each discrete formula to its continuous version.

---

## The four steps

### 1 · Transformations: any distribution into a uniform

The foundation of every copula is the **probability integral transform**:

$$U = F(Y) \sim \text{Uniform}(0, 1)$$

Passing a variable through its own cdf turns it into its percentile, and percentiles are always spread evenly.

**On the page:** choose a distribution and follow one value from the pdf (top), through the cdf (center), to the uniform (right). Five colored bands each hold 20% of the probability: narrow where the pdf is tall, wide where it is low, and all the same width after the transform. The **Animate** button sends 400 samples through the cdf and fills the uniform histogram.

### 2 · Level sets: two variables at once

The model from the board builds a joint density in two layers, a marginal and a conditional:

$$f_{XY}(x, y) = f_{Y\mid X}(y \mid x)\, f_X(x)$$

with $X$ lognormal and $Y \mid X$ normal around a line $\mu(x) = \alpha + \beta x$:

$$f_X(x) = \frac{1}{x\sqrt{2\pi\tau^2}} \exp\!\left[-\frac{(\ln x - \lambda)^2}{2\tau^2}\right], \qquad f_{Y\mid X}(y \mid x) = \frac{1}{\sqrt{2\pi\sigma^2}} \exp\!\left[-\frac{(y - \mu(x))^2}{2\sigma^2}\right]$$

A **level set** is every point where the joint density takes the same value, like a contour line on a topographic map.

**On the page:** move the sliders for $\lambda$, $\tau$, $\alpha$, $\beta$ and $\sigma$, and slide the conditional "slice" along $x$. Switch $X$ between **lognormal** and **normal** (same mean and variance): normal gives ellipses, lognormal gives teardrops. The Python code below the plot updates with your settings and can be copied.

**Business example:** $X$ = daily demand (lognormal, known from history) and $Y$ = daily revenue, which sits around $1.5x$ thousand dollars once demand is known. Revenue is never modeled directly, yet its distribution follows from the two layers, so the page can answer questions like $P(\text{revenue} > \$3{,}000) \approx 8.9\%$.

### 3 · Copulas: separating the marginals from the dependence

Choose a **copula family**, a **strength**, and a distribution for **X** and **Y**. Three linked plots show the same 2,000 pairs:

| Plot | What it shows |
|---|---|
| 1 · Original scale | The data as observed, with the chosen marginals |
| 2 · Uniforms | Each variable passed through its own cdf: this cloud **is** the copula |
| 3 · Normal scale | The same copula with $z = \Phi^{-1}(u)$, where the shapes are easiest to compare |

Change the distributions and only plot 1 changes. Change the family or the strength and all three change.

| Family | Tail dependence | Shape in plot 3 |
|---|---|---|
| Gaussian | None | Clean ellipse |
| Student t ($\nu = 3$) | Both tails | Ellipse with pinched corners |
| Clayton | Lower tail | Teardrop pointing to the lower left |
| Gumbel | Upper tail | Teardrop pointing to the upper right |
| Frank | None | Symmetric band, fat at the ends |

Options include level curves, the least-squares line, and **15 real-world examples** (3 per family), each with a button that loads its settings: ice-cream sales and temperature, flood losses, delivery delays, stock returns, and more.

### 4 · Kendall's τ: measuring dependence

Every family has its own parameter on its own scale, so the strength slider uses one common measure, **Kendall's τ**:

$$P(\text{two observations move the same way}) = \frac{1 + \tau}{2}$$

It depends only on the order of the data, so it does not change when the marginals change. The page converts it to each family's parameter:

$$\text{Gaussian: } \rho = \sin\!\left(\tfrac{\pi\tau}{2}\right) \qquad \text{Clayton: } \theta = \frac{2\tau}{1 - \tau} \qquad \text{Gumbel: } \theta = \frac{1}{1 - \tau}$$

A table shows how to read values from −0.5 to 0.9 and the equivalent parameter of each family.

---

## Three numbers, three meanings

| Measure | Computed on | Depends on the marginals? |
|---|---|---|
| Classic correlation (Pearson) | Plot 1, raw data | Yes |
| Pearson on the normal scale | Plot 3 | No |
| Kendall's τ | The order of the data | No |

For a copula model, Kendall's τ is the measure that belongs to the dependence itself.

---

## The six distributions

| Distribution | Why it is in the lab |
|---|---|
| Normal | The reference: ellipses, $\Phi$, the normal scale |
| Uniform (0, 1) | The destination of the transform |
| Exponential | Closed-form cdf and inverse |
| Lognormal | The board's model; money-like skew |
| Bimodal | Two peaks, same copula |
| Student t | Heavy tails; no mgf |

The **Reference** section at the end of the page lists the support, pdf, cdf, mean, variance, mgf, a business example and the `scipy.stats` command for each one.

---

## Using it in class

- **Hover over or tap any dotted label** to see what it measures and its formula.
- **Each step opens and closes with questions.** "Before you start" checks intuition; "Check your understanding" checks what was learned. Answers stay hidden until you click a question.
- A suggested order for a presentation:
  1. Step 1 with the exponential, then the bimodal.
  2. Step 2 with the board example, switching X between lognormal and normal (question 3).
  3. Step 3 with normal marginals and τ = 0.5, cycling through the five families (question 1).
  4. Step 3 again, changing the marginals to show that Pearson moves and Kendall's τ does not (question 2).
  5. Step 4 to close with the strength table.

---

## Model Builder: complete a model from what you know

### ▶ [Open the Model Builder](https://juanciro-commits.github.io/copula-lab/model-builder.html)

The companion tool. Give it the pieces of a two-variable model you know, and it completes the rest. Every parameter is editable, for example the mean and variance of a normal.

| What you know | What the tool completes | Mode |
|---|---|---|
| Both marginals and the copula (family and strength) | The joint density, the conditional of $Y$ given $X$, and $E[Y \mid X = x]$ | 1 |
| The marginal of $X$ and how $Y$ behaves given $X$ | The marginal of $Y$, the implied copula and its closest family | 2 |
| Paired data on $X$ and $Y$ | Both marginals and the copula, each family chosen by AIC | 3 |
| Only the two marginals, no data | Cannot be completed: the dependence must be chosen or estimated from data | – |
| The marginal of $Y$ and $Y$ given $X$ | Cannot recover the marginal of $X$ in general | – |

- **Mode 1 · Marginals + copula:** uses $f_{XY}(x,y) = c\big(F(x), G(y)\big)\, f(x)\, g(y)$ and $f(y \mid x) = c\big(F(x), G(y)\big)\, g(y)$.
- **Mode 2 · Marginal of X + Y given X:** with $Y \mid X = x \sim N(\alpha + \beta x, \sigma^2)$, it derives $g(y) = \int f(y \mid x)\, f(x)\, dx$, its mean and variance, and the copula the model implies. The demand-and-revenue example shows an implied copula closest to **Gumbel**: busy days pull revenue up together.
- **Mode 3 · From data:** paste two columns. It fits each marginal by maximum likelihood (normal, lognormal, exponential, uniform, Student t, bimodal) and each copula family from Kendall's $\tau$, then picks the lowest AIC $= 2k - 2\log L$. You can also force a family. Includes an example dataset of temperature and ice-cream sales.

### Example datasets for mode 3

Open a link, select all the text, copy it, and paste it into **3 · From data**. Each file has two columns, X and Y, with a header line.

| Dataset | Pairs | What it is | What the Model Builder should find |
|---|---|---|---|
| [Fathers' and sons' heights](https://raw.githubusercontent.com/juanciro-commits/copula-lab/main/data/galton_fathers_sons.csv) | 481 | **Real data**, Galton's 1886 families (inches) | τ ≈ 0.31; Frank and Gaussian almost tied, as real data often are |
| [Ice-cream sales and temperature](https://raw.githubusercontent.com/juanciro-commits/copula-lab/main/data/icecream_temperature_sales.csv) | 300 | Simulated: X normal, Y lognormal, Clayton τ = 0.5 | Clayton: low temperatures and low sales together |
| [Flood losses in two cities](https://raw.githubusercontent.com/juanciro-commits/copula-lab/main/data/flood_losses_two_cities.csv) | 800 | Simulated: both lognormal, Gumbel τ = 0.5 | Gumbel: big losses happen together |
| [Daily returns of two banks](https://raw.githubusercontent.com/juanciro-commits/copula-lab/main/data/bank_daily_returns.csv) | 500 | Simulated: both Student t, t copula τ = 0.5 | Student t marginals and a t copula: joint crashes and rallies |
| [Ad spend and store visits](https://raw.githubusercontent.com/juanciro-commits/copula-lab/main/data/ad_spend_store_visits.csv) | 300 | Simulated: X lognormal, Y normal, Frank τ = 0.4 | Frank, narrowly ahead of the Gaussian |

The simulated files have a known answer, so you can check whether the tool recovers the family that generated them. In the Galton data, the families are within one or two AIC points of each other: with real data, several models can describe the same relationship about equally well. The Galton data comes from the `GaltonFamilies` table in the R package HistData, via [Rdatasets](https://vincentarelbundock.github.io/Rdatasets/).

The **"Your completed model"** table labels every piece as **Chosen**, **Derived** or **Estimated**, and two linked plots show the joint density (level sets enclosing 25%, 50%, 75% and 90% of the probability, the marginals, $E[Y \mid X = x]$ and a conditional slice) and the copula in the unit square.

---

## Files

| File | Content |
|---|---|
| `index.html` | The Copula Lab: one self-contained page |
| `model-builder.html` | The Model Builder: one self-contained page |
| `data/` | Example datasets for the Model Builder (CSV) |
| `README.md` | This guide |

To run them locally, download the `.html` files and open them in any browser. Formulas are rendered with [MathJax](https://www.mathjax.org/), loaded from a CDN, so they need an internet connection.

---

## Credits

Made by **Juan Salazar** (MSBA, UCF, Stochastic Methods for Business Analytics). Built in collaboration with **Claude** (Anthropic), an AI assistant.
