<div align="center">
    <h1 align="center">Serve an XGBoost Model with BentoML</h1>
</div>

This is a BentoML example project, which demonstrates how to serve and deploy an [XGBoost](https://xgboost.readthedocs.io/en/stable/) model with BentoML.

## Install dependencies

```bash
git clone https://github.com/bentoml/BentoXGBoost.git
cd BentoXGBoost

# Recommend Python 3.11
# Install BentoML and the required dependencies for the project
pip install bentoml xgboost scikit-learn
```

## Train and save a model

Save the model to the BentoML Model Store:

```bash
python3 save_model.py
```

## Run the BentoML Service

We have defined a BentoML Service in `service.py`. Run `bentoml serve` in your project directory to start the Service.

```python
$ bentoml serve .

2024-06-19T08:37:31+0000 [INFO] [cli] Starting production HTTP BentoServer from "service:CancerClassifier" listening on http://localhost:3000 (Press CTRL+C to quit)
```

The server is now active at [http://localhost:3000](http://localhost:3000/). You can interact with it using the Swagger UI or in other different ways.

<details>

<summary>CURL</summary>

```bash
curl -X 'POST' \
    'http://localhost:3000/predict' \
    -H 'accept: application/json' \
    -H 'Content-Type: application/json' \
    -d '{
    "data": [
        [1.308e+01, 1.571e+01, 8.563e+01, 5.200e+02, 1.075e-01, 1.270e-01,
        4.568e-02, 3.110e-02, 1.967e-01, 6.811e-02, 1.852e-01, 7.477e-01,
        1.383e+00, 1.467e+01, 4.097e-03, 1.898e-02, 1.698e-02, 6.490e-03,
        1.678e-02, 2.425e-03, 1.450e+01, 2.049e+01, 9.609e+01, 6.305e+02,
        1.312e-01, 2.776e-01, 1.890e-01, 7.283e-02, 3.184e-01, 8.183e-02]
      ]
    }'
```

</details>

<details>

<summary>Python client</summary>

```python
import bentoml

with bentoml.SyncHTTPClient("http://localhost:3000") as client:
    result = client.predict(
        data=[
            [1.308e+01, 1.571e+01, 8.563e+01, 5.200e+02, 1.075e-01, 1.270e-01,
            4.568e-02, 3.110e-02, 1.967e-01, 6.811e-02, 1.852e-01, 7.477e-01,
            1.383e+00, 1.467e+01, 4.097e-03, 1.898e-02, 1.698e-02, 6.490e-03,
            1.678e-02, 2.425e-03, 1.450e+01, 2.049e+01, 9.609e+01, 6.305e+02,
            1.312e-01, 2.776e-01, 1.890e-01, 7.283e-02, 3.184e-01, 8.183e-02]
        ],
    )
    print(result)
```

</details>

For detailed explanations, see [the BentoML documentation](https://docs.bentoml.com/en/latest/examples/xgboost.html).

For custom deployment in your own infrastructure, use [BentoML to generate an OCI-compliant image](https://docs.bentoml.com/en/latest/get-started/packaging-for-deployment.html).
