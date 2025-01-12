# Stock Predictor API

## LEGAL DISCLAIMER

This project is for education purpose only and the available API's is for personal use only. The data collected using [yfinance](https://pypi.org/project/yfinance/) is under Yahoo!'s terms and conditions.

## Introduction

Personal project that enables users to get stock prediction data for the next 30 days. The project is based on Meta's [Prophet](https://facebook.github.io/prophet/) project. The project uses upto 5 year's worth of opening price data for a stock and predicts the opening price for the next 30 days. A frontend website is developed to visualize the data, [Link](https://master--stock-predictor-react.netlify.app/), [Repo](https://github.com/vasup86/Stock-Prediction-Website).

## Project Support Features

REST API end point returns last 6 months opening price and projected opening price for the next 30 days.

## Installation Guide

- Clone the repository
- The master branch is the most stable branch at any given time, ensure you're working from it.
- Create a virtual environment using `python -m venv name_of_your_env`
- Run `pip install -r requirements.txt` all dependencies
  - Windows user might also need to add `pywin32==306` to the `requirements.txt` file.
- Alternatively you can access the API which is hosted on AWS EC2, [here](https://vasu-patel.me/).

### Project Structure

- `app.py` contains the endpoints and handle's the requests.
- `forecast.py` machine learning script to predict the price.
- `requirements.txt` contains the packages used for this project.

## Deployment

The API is hosted using an ubuntu instance on [AWS EC2](https://aws.amazon.com/ec2/).

URL: `https://vasu-patel.me/`

## REST API Endpoints

The REST API endpoints are described below. The body and the response are in JSON format.

### Homepage

Use this endpoint to ping the server to start the application.

#### Request

`GET /`

```
curl https://vasu-patel.me/
```

#### Response

```
{"result":"connected"}
```

### Predict

#### Request

`POST /predict`

##### Body

The endpoint accepts a json body with the value of the `ticker` key to be the stock ticker. For example, 'aapl' for Apple Inc.

Example:

```
{
"ticker": "aapl"
}
```

```
curl --header "Content-Type: application/json" \
  --request POST \
  --data '{ "ticker": "aapl" }' \
  https://vasu-patel.me/predict
```

#### Response

For a valid ticker value, the response will contain a `result` key which holds the response array. The response will contain up to past 6 months of opening price, under `date` and `past`, `forecast` will be null for this period. After that data, the forecast data will be under `date` and `forecast`, `past` will be null for this period.

```
{ "result":
        [{
		"date": "Mon, 31 Jul 2023 00:00:00 GMT",
	        "forecast": null,
	        "past": 195.537995145902
	},{
		"date": "Tue, 01 Aug 2023 00:00:00 GMT",
	        "forecast": null,
	        "past": 195.7175151124968
	}, .....,
	{
		"date": "Sat, 03 Feb 2024 00:00:00 GMT",
	        "forecast": 199.13563380619226,
	        "past": null
	    }, {
	        "date": "Sun, 04 Feb 2024 00:00:00 GMT",
	        "forecast": 199.48080692540407,
	        "past": null
	    }, ....
	  ]
}
```

For an invalid ticker, the server will return an response error code `406`. This could be due to invalid ticker value or recent IPO's which might not be available.

```
{
	"errorType":  406,
	"message":  "No data"
}
```

## Technologies Used

Python 3.11.1 was used to develop this application.
|Package| Version |
|--|--|
| flask | 3.11.1 |
| yfinance | 0.2.35 |
| prophet | 1.1.5 |
| pandas | 2.2.0 |

## Limitations

The machine learning model, prophet, only utilizes on piece of data which is the stock's historical opening price. A stock price depends on multiple factors and an acurate prediction using this API is not possible. This project should not used as a factor to invest in a potential stock and is for education purpose only.
