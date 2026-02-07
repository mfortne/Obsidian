# Options Strategy

## Goal
Build a **rules-based**, repeatable options strategy that:
- Works in a **small account (~$5,000)**
- Uses **liquid underlyings** and **defined risk** where possible
- Avoids “blow-up risk” (earnings gaps, meme names, wide spreads)
- Can be evaluated against public benchmarks (CBOE strategy indices)

> Note: Not financial advice. This is a research + rules framework so we can test/iterate.

---

## Benchmarks worth tracking ("what good looks like")
CBOE publishes long-running strategy indices that approximate classic options portfolios. These are useful as *north stars* and sanity checks.

- **BXM** (S&P 500 BuyWrite): systematic covered-call / buy-write style.
- **PUT** (S&P 500 PutWrite): systematic short put / put-writing style.

Source overview list (CBOE “Strategy Benchmarks”):
- https://www.cboe.com/us/indices/benchmark_indices/

Why this matters:
- If our strategy is “sell premium”, the long-term question is usually: **does it beat simple buy-and-hold after drawdowns, taxes, effort, and tail risk?**

---

## Strategy families (what we can implement)

### A) The Wheel (cash-secured puts → covered calls)
**Best for:** small account, simple mechanics, clear assignment logic.
- Phase 1: Sell **cash-secured puts (CSPs)** on underlyings you’d be okay owning.
- Phase 2 (if assigned): sell **covered calls (CCs)** until called away.

Key risks:
- Concentration (one assignment can dominate a $5k account)
- Big gap downs (earnings, macro)


### B) Defined-risk credit spreads (put credit spreads)
**Best for:** $5k account risk control.
- Similar “short premium” thesis, but capped downside.

Key risks:
- Easier to get chopped up by whipsaws
- Spreads widen in stress


### C) Covered call / buy-write portfolio
**Best for:** if we intentionally want an equity + income profile.
- Can anchor to something like SPY/QQQ/sector ETFs *if* share price fits.

---

## Universal filters (the “don’t be dumb” checklist)
These are the filters I’d use before even looking at a strike:

### Liquidity / execution
- **Option volume**: prefer active strikes/expirations (not ghost chains)
- **Open interest**: meaningful OI around target strikes
- **Bid/ask spread**:
  - Prefer tight markets; avoid contracts where spread eats the edge
  - Rule of thumb: spread should be **small vs premium** (ex: <10–15% of mid)

### Event / gap risk
- **Avoid earnings week** (for single names). Rule: no short premium positions opened within ~7–10 trading days of earnings.
- Avoid major binary events (FDA decisions, merger votes, etc.)

### Underlying quality
- Avoid meme/pump names and ultra-high IV “lottery stocks” unless explicitly allowed.
- Prefer: broad ETFs, mega-caps, or boring liquid names.

### Position sizing / concentration (critical at $5k)
- Per-underlying max risk:
  - Wheel CSP: ensure **strike × 100 ≤ available cash** *and* do not allocate >50–70% of account to a single assignment unless you accept that.
  - Spreads: cap max loss per trade (e.g., **1–3%** conservative, **3–6%** aggressive).

---

## Entry rules (suggested defaults)

### DTE (days to expiration)
- For short premium, a common “sweet spot” is **~30–60 DTE** for smoother gamma.
- For our automation + simplicity: **7–21 DTE** can work, but gamma risk rises as you get close.

### Strike selection
Two practical strike rules we can implement without fancy data:
1) **Delta targeting** (preferred):
   - CSP: target ~0.20–0.35 delta (more aggressive up to 0.40)
2) **Moneyness targeting** (fallback when delta isn’t available):
   - Choose a put strike that is **slightly OTM** (e.g., ~1–3% below spot), then sanity-check premium and assignment size.

### Volatility regime (optional but helpful)
- Prefer selling premium when IV is elevated vs its recent history (IV Rank / IV Percentile), but don’t chase pure IV at the cost of quality.

---

## Management rules (exit / roll)

### Profit target
- Common approach: close at **50–60%** of max profit.
  - This reduces time in trade and tail exposure.

### Loss / defense
Pick one to keep it simple:
- **Hard stop:** close at a defined loss (e.g., 2× credit)
- **Time stop:** if trade hasn’t worked by X DTE remaining (e.g., exit at 7 DTE)
- **Roll rule:** roll out (and possibly down) only if you can do it for a credit and you still want the underlying

### Assignment plan (Wheel-specific)
- If assigned:
  - Sell CC at a strike near your cost basis (or slightly above)
  - Avoid selling CC below basis unless you explicitly want out

---

## A simple scoring system (so it’s not vibes)
For each candidate underlying, score 0–2 on:
1) Liquidity (tight spreads, strong OI)
2) No upcoming earnings/binary event
3) Assignment fits account (strike×100 ≤ cash, concentration acceptable)
4) Volatility favorable (IV elevated vs recent)
5) Underlying quality (ETF/mega-cap/boring > hype)

Only trade if total score ≥ a threshold (e.g., **7/10**).

---

## $5,000 portfolio implementation (practical)

### Constraint
With $5k, **one assignment dominates**.
- If we sell a CSP on a $40 stock, that’s $4,000 tied up.

### Two workable modes

**Mode 1: Wheel on lower-price liquid names (simple, but concentrated)**
- Only sell CSPs where strike×100 is comfortably ≤ cash
- Keep a cash buffer (ex: don’t deploy 100%)

**Mode 2: Defined-risk spreads (less capital, more diversification)**
- Put credit spreads sized to a fixed max loss per trade
- Allows multiple positions without one name consuming the account

---

## Research note: backtesting tools & “45 DTE / profit targets”
Tastytrade/tastylive discuss parameterized backtesting (e.g., DTE selection, delta targeting, profit targets). Useful for generating hypotheses, but we still need to validate with realistic fills/slippage and understand tail events.

Reference (tastylive backtesting tool overview):
- https://www.tastylive.com/news-insights/backtesting-options-trades-learn-use-tastytrade-backtesting-tool

---

## Next steps (what I suggest we do next)
1) Decide which mode we’re building first:
   - **Wheel** (simplest) vs **put credit spreads** (best for $5k risk control)
2) Create an initial **universe allowlist** (10–30 tickers/ETFs).
3) Encode the rules into `strategy.json` and a repeatable checklist.
4) Run a paper simulation for 4–8 weeks; review stats:
   - Win rate, avg win/loss, max drawdown, % time in market, concentration
5) Compare the behavior to the “idea benchmarks” (BXM/PUT conceptually).

