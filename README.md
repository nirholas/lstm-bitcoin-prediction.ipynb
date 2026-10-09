# lstm-bitcoin-prediction.ipynb

A single Jupyter notebook that trains an LSTM neural network to predict the daily closing price of Bitcoin (BTC/USD), then compares the model's predicted daily returns against the actual ones.

The notebook is self-contained: it downloads its own data at run time, trains the model, and plots the results.

## What the notebook does

1. **Fetches data** — 2000 daily BTC/USD candles from the public [CryptoCompare](https://min-api.cryptocompare.com/) `histoday` endpoint (no API key required).
2. **Splits** the series into train/test sets by time (the last 10% is the test set — no shuffling across the split point).
3. **Windows the data** — converts the series into overlapping 7-day windows, each normalised to its own first entry so the model learns relative change rather than absolute price.
4. **Builds the model** — Keras `Sequential`: one `LSTM` layer (20 units) → `Dropout` (0.25) → `Dense(1)` → linear activation, compiled with MAE loss and the Adam optimizer.
5. **Trains** for 50 epochs at batch size 4, seeded with `np.random.seed(42)`.
6. **Evaluates** with `sklearn.metrics.mean_absolute_error` and plots actual vs. predicted prices and actual vs. predicted daily returns.

## Requirements

- Python 3
- Jupyter (Notebook or Lab)

Python packages:

```bash
pip install jupyter keras tensorflow scikit-learn pandas numpy matplotlib seaborn requests
```

`keras` needs a backend; `tensorflow` provides it.

## Running it

```bash
git clone https://github.com/nirholas/lstm-bitcoin-prediction.ipynb.git
cd lstm-bitcoin-prediction.ipynb
jupyter notebook lstm-bitcoin-prediction.ipynb
```

Then run the cells top to bottom. The data-fetch cell needs an internet connection. Training on CPU takes a few minutes.

Outputs (plots and metrics) are saved in the committed notebook, so you can also just read it on GitHub without running anything.

## Tuning

All knobs live in one cell:

| Parameter | Default | Meaning |
| --- | --- | --- |
| `window_len` | 7 | Days of history per input window |
| `test_size` | 0.1 | Fraction of the series held out for testing |
| `zero_base` | `True` | Normalise each window to its first entry |
| `lstm_neurons` | 20 | LSTM layer width |
| `epochs` | 50 | Training epochs |
| `batch_size` | 4 | Training batch size |
| `dropout` | 0.25 | Dropout rate after the LSTM layer |
| `loss` | `mae` | Loss function |
| `optimizer` | `adam` | Optimizer |

Change the `fsym` query parameter in the data-fetch cell to model a different asset.

## Disclaimer

This is a machine-learning exercise, not financial advice. Predicting price from historical price alone is not a trading edge — the returns plot is included precisely so you can see how the model behaves out of sample.

## License

See [LICENSE](LICENSE). Copyright 2026 nirholas, all rights reserved.

## Star History

[![Star History Chart](https://api.star-history.com/svg?repos=nirholas/lstm-bitcoin-prediction.ipynb&type=Date)](https://www.star-history.com/#nirholas/lstm-bitcoin-prediction.ipynb&Date)
