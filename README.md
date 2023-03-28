<p align="center">
  <a href="https://github.com/dream-faster/fold-models/actions/workflows/test-statsforecast.yaml"><img alt="Statsforecast Tests" src="https://github.com/dream-faster/fold-models/actions/workflows/test-statsforecast.yaml/badge.svg"/></a>
  <a href="https://github.com/dream-faster/fold-models/actions/workflows/test-statsmodels.yaml"><img alt="StatsModels Tests" src="https://github.com/dream-faster/fold-models/actions/workflows/test-statsmodels.yaml/badge.svg"/></a>
  <a href="https://github.com/dream-faster/fold-models/actions/workflows/test-xgboost.yaml"><img alt="XGBoost Tests" src="https://github.com/dream-faster/fold-models/actions/workflows/test-xgboost.yaml/badge.svg"/></a>
  <a href="https://github.com/dream-faster/fold-models/actions/workflows/test-sktime.yaml"><img alt="Sktime Tests" src="https://github.com/dream-faster/fold-models/actions/workflows/test-sktime.yaml/badge.svg"/></a>
  <a href="https://github.com/dream-faster/fold-models/actions/workflows/test-prophet.yaml"><img alt="Prophet Test" src="https://github.com/dream-faster/fold-models/actions/workflows/test-prophet.yaml/badge.svg"/></a>
  <a href="https://github.com/dream-faster/fold-models/actions/workflows/test-baselines.yaml"><img alt="Baselines Tests" src="https://github.com/dream-faster/fold-models/actions/workflows/test-baselines.yaml/badge.svg"/></a>
  <a href="https://discord.gg/EKJQgfuBpE"><img alt="Discord Community" src="https://img.shields.io/badge/Discord-%235865F2.svg?logo=discord&logoColor=white"></a>
    <a href="https://calendly.com/mark-szulyovszky/consultation"><img alt="Calendly Booking" src="https://shields.io/badge/-Speak%20with%20us-orange?logo=minutemailer&logoColor=white"></a>
</p>

<!-- PROJECT LOGO -->

<br />
<div align="center">
  <a href="https://dream-faster.github.io/fold/">
    <img src="https://raw.githubusercontent.com/dream-faster/fold-models/main/docs/images/logo.svg" alt="Logo" width="90" >
  </a>
<h3 align="center"><b>FOLD-MODELS</b><br> <i>(/fold models/)</i></h3>
  <p align="center">
    <b>Baseline models and Wrappers for 3rd party libraries.
    <br/>To be used with  <a href='https://github.com/dream-faster/fold'>Fold.</a> </b><br>
    <br/>
    <a href="https://dream-faster.github.io/fold-models/"><strong>Explore the docs »</strong></a>
  </p>
</div>
<br />

# Available models

|                                                                                                                                                                                                                                             | Name                                   |                          Link                          | Supports<br />Online <br />updating | Wrapper Name<br />& Import Location                                            |
| :------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------:| :------------------------------------- | :----------------------------------------------------: | :---------------------------------: | ------------------------------------------------------------------------------ |
| <img alt='Statsforecast Logo' src='https://raw.githubusercontent.com/Nixtla/neuralforecast/main/nbs/imgs_indx/logo_mid.png' height=64>                                                                                                      | StatsForecast                          |   [GitHub](https://github.com/Nixtla/statsforecast)    |                 ❌                  | **WrapStatsForecast**<br />`from fold_models.prophet import WrapStatsForecast` |
| <img alt='XGBoost Logo' src='https://camo.githubusercontent.com/0ea6e7814dd771f740509bbb668d251d485a6e21f12e287be7cc2275e0eab1d1/68747470733a2f2f7867626f6f73742e61692f696d616765732f6c6f676f2f7867626f6f73742d6c6f676f2e737667' height=64> | XGBoost                                |       [GitHub](https://github.com/dmlc/xgboost)        |                 ✅                    | **WrapXGB**<br />`from fold_models.xgboost import WrapXGB`               |
| <img alt='Sktime Logo' src='https://github.com/sktime/sktime/raw/main/docs/source/images/sktime-logo.jpg?raw=true' height=64>                                                                                                               | Sktime                                 |       [GitHub](https://github.com/sktime/sktime)       |                 ✅                  | **WrapSktime**<br />`from fold_models.sktime import WrapSktime`                |
| <img alt='Statsmodels Logo' src='https://github.com/statsmodels/statsmodels/raw/main/docs/source/images/statsmodels-logo-v2-horizontal.svg' width=160>                                                                                      | Statsmodels                            |  [GitHub](https://github.com/statsmodels/statsmodels)  |                 ✅                  | **WrapStatsModels**<br />`from fold_models.statsmodels import WrapStatsModels` |
| <img alt='Prophet Logo' src='https://miro.medium.com/v2/resize:fit:964/0*tVCene42rgUTNv9Q.png' width=160>                                                                                                                                   | Prophet                                |     [GitHub](https://github.com/facebook/prophet)      |                 ✅                  | **WrapProphet**<br />`from fold_models.prophet import WrapProphet`             |
| <img alt='Scikit-Learn Logo' src='https://raw.githubusercontent.com/scikit-learn/scikit-learn/main/doc/logos/scikit-learn-logo.png' width=160>                                                                                              | Sklearn <br/>(natively available in `fold`) | [GitHub](https://github.com/scikit-learn/scikit-learn) |             🟡<br/>(some)              | Sklearn doesn't need to be wrapped,<br />just pass in the models.              |

# Installation

- Prerequisites: `python >= 3.7` and `pip`

- Install from git directly:
  ```
  pip install https://github.com/dream-faster/fold-models/archive/main.zip
  ```
- Depending on what model you'd like to wrap, you can either install the library directly or run
   ```
  pip install "git+https://github.com/dream-faster/fold-models.git#egg=fold-models[<your_library_name>]"
  ```

# Quickstart




You can quickly train your chosen models and get predictions by running:

```python
  from fold import ExpandingWindowSplitter, train_evaluate
  from fold.utils.dataset import get_preprocessed_dataset
  from statsforecast.models import ARIMA

  from fold_models import WrapStatsForecast

  X, y = get_preprocessed_dataset(
      "weather/historical_hourly_la", target_col="temperature", shorten=1000
  )
  model = WrapStatsForecast(
      model_class=ARIMA,
      init_args={"order": (1, 0, 0)},
      use_exogenous=False,
      online_mode=False,
  )
  splitter = ExpandingWindowSplitter(initial_train_window=0.2, step=50)

  scorecard, predictions, trained_pipeline = train_evaluate(model, X, y, splitter)
```

You can also wrap a model that you have initiate first:

```python
wrapped_model = WrapStatsForecast.from_model(ARIMA(order=(1, 0, 0)), use_exogenous=False, online_mode=False  )
```
## Our Open-core Time Series Toolkit

[![Krisi](https://raw.githubusercontent.com/dream-faster/fold/main/docs/images/overview_diagrams/dream_faster_suite_krisi.svg)](https://github.com/dream-faster/krisi)
[![Fold](https://raw.githubusercontent.com/dream-faster/fold/main/docs/images/overview_diagrams/dream_faster_suite_fold.svg)](https://github.com/dream-faster/fold)
[![Fold/Models](https://raw.githubusercontent.com/dream-faster/fold/main/docs/images/overview_diagrams/dream_faster_suite_fold_models.svg)](https://github.com/dream-faster/fold-models)

If you want to try them out, we'd love to hear about your use case and help, [please book a free 30-min call with us](https://calendly.com/mark-szulyovszky/consultation)!

## Contribution

Join our [Discord](https://discord.gg/EKJQgfuBpE) for live discussion!

Submit an issue or reach out to us on info at dream-faster.ai for any inquiries.


## Licence & Usage

Fold is our open-core Time Series engine. It's available under the MIT + Common Clause licence.
We want to **bring much-needed transparency, speed and rigour** to the process of building Time Series ML models. We're building multiple products with and on top of it.

It will be always free for research useage, but we are charging a fee for production use, and for extra features that are results of our own resource-intensive R&D.