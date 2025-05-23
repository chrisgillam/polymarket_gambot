# Gambot: Automated Sports Betting on Polymarket
## Overview
Gambot is an open-source trading bot that identifies profitable sports betting opportunities and executes them on [Polymarket](https://polymarket.com/). By pulling market data from sharp sportsbooks (like [Pinnacle](https://www.pinnacle.com/en/)), removing the house edge to determine the sportsbook's "true" view on odds and comparing against Polymarket, Gambot leverages a probabilistic modeling approach and Kelly Criterion bet sizing to pursue long-term capital growth.
This bot is essentially a replication of how [OddsJam](https://oddsjam.com/?ref=mzawnti) identifies value bets on notable sportsbooks, adapted to the peer-to-peer format of Polymarket. To learn more about the underlying methodology, [OddsJam](https://oddsjam.com/?ref=mzawnti) has a wealth or resources on their website.

### If this project saves you time and adds value, please consider sponsoring on GitHub or donating via our Crypto Wallet (details below).
[![Sponsor](https://img.shields.io/badge/sponsor-%F0%9F%92%B8-brightgreen)](https://github.com/sponsors/chrisgillam)

## Introduction
Sports betting markets often present mispriced odds. Sharp sportsbooks like Pinnacle are generally efficient at pricing, so comparing their implied probability to that of Polymarket can reveal edges. Gambot automates this comparison and calculates stake sizes through the Kelly Criterion to optimize growth over time.

## Key Features
Automated Market Scanning: Constantly checks Polymarket odds against sharp sportsbook lines to detect profitable divergences.

- Probability Modeling: Estimates true event probabilities, searching for positive expected value (EV).
- Kelly Criterion Bet Sizing: Dynamically adjusts stake sizes to maximize growth while controlling risk.
- Peer-to-Peer Adaptation: Tailored for Polymarket’s unique trading mechanics, differing from traditional sportsbook models.

## How It Works
- Data Collection: Gambot queries Pinnacle for sharp odds, then retrieves Polymarket prices in real time.
- Probability Estimation: By comparing implied probabilities, Gambot flags situations where Polymarket’s market prices differ significantly from the baseline “true” odds.
- Kelly Criterion Calculation: Once an opportunity is found, the bot calculates the optimal stake size using Kelly Criterion formulas—adjustable for risk tolerance—to aim for long-term growth.
- Orders: Gambot sends fill‑or‑kill (FOK) orders when placing trades on Polymarket. With an FOK order the position is filled immediately and in full at the quoted price, or cancelled entirely. This prevents partial fills, avoids price slippage, and guarantees that the Kelly‑calculated stake size is executed exactly as intended, keeping bankroll risk tightly controlled.

## Usage
### Configure Credentials
For Gambot to function correctly, you need to provide API credentials for both Polymarket and Pinnacle. Save them in a CSV file named gambot_keys.csv in the same directory as this notebook.
A template called gambot_keys.csv with placeholders has been provided that follows this format:

key,funder,X_RAPID_API_KEY
"YOUR PRIVATE KEY HERE","YOUR PUBLIC ADDRESS HERE","YOUR RAPIDAPI PINNACLE API KEY HERE"

When you run the notebook, Gambot will load these credentials to authenticate both market data fetching and on-chain betting transactions.

#### Where to find Polymarket Credentials
The private key can be found in your Polymarket account by navigating to Settings > Export Private Key. The public address (funder) is publicly available on your Polymarket profile.

#### Where to find Pinnacle API Credentials
You will need to subscribe to an API to pull the Pinnacle data. Gambot has been configured to us this API - [Pinncale Data API](https://bit.ly/4inkWMN). Gambot makes a lot of calls to the API, so the Mega Unlimited Plan is typically what I would advise.

### Set Trading Parameters
You can fine-tune Gambot’s behavior by adjusting parameters in the code. Below are the most critical parameters to consider:

- LOG_LEVEL_ENV: Sets the level of detail in the output (e.g., DISABLED, DEBUG, INFO). INFO ensures you capture essential updates/trade logs, but DEBUG can create a lot of spam. Start with DEBUG if you’re refining the code, then switch to INFO or DISABLED in production.
- SCRIPT_MODES: Dictates which sports or markets are in scope during a run.
- INCLUDE_LIVE: Determines if the bot will place bets on live games. Due to execution lag, live betting can be risky. It’s recommended to keep this FALSE.
- MAXIMIMUM_PINNACLE_TRUE_PROBABILITY: Defines the shortest odds (highest probability) Gambot will bet on. Avoid extremely short‐priced favorites (like 90%+). The potential edges are usually smaller, and a single upset can wipe out many small gains.
- MINIMUM_PINNACLE_TRUE_PROBABILITY: Defines the longest odds (lowest probability) Gambot will bet on. Very low‐probability underdogs are often priced with huge spreads and are more volatile. They can still be profitable if you have a big edge—but they’re also more prone to zero‐out your bet.
- MAX_PERCENTAGE_BET: Sets a guardrail on the maximum percentage of your bankroll you can place on a single bet. This is typically already managed by the kelly_multiplier, but was built into the code as a failsafe.
- MAX_DOLLAR_BET: Caps the maximum dollar amount for any individual bet. If you are starting with a big bankroll, make sure this is high enough so that trades will execute. This is typically already managed by the kelly_multiplier, but was built into the code as a failsafe.
- time_interval: The pause (in seconds) after each trading cycle.
- kelly_multiplier: Scales the Kelly optimal bet size by a factor of 0 to 1. A value of 1 is fully Kelly (aggressive), and lower values reduce the bet size to mitigate risk. A full‐Kelly (1.0) approach can be very volatile. Most traders scale Kelly down for less drawdown while still benefiting from Kelly’s growth principles.
- target_buy_ev: The minimum edge (EV) you want before placing a buy bet. For example, a value of 0.05 means that the true probability of a team winning is 5% higher than where they are priced on Polymarket. 3-8% looks like the sweet spot.
- target_sell_ev: The minimum edge (EV) you want before selling a bet. For example, a value of 0.05 means that the true probability of a team winning is 5% lower than where they are priced on Polymarket. Set to 0.99 to effectively disable selling.
- sportname_week: The specific week listing for that sport on Polymarket (e.g., NFL_week, NBA_week).

Ensure these parameters align with your risk tolerance and strategy objectives before running the bot.

### Run the Notebook
- Once credentials are set and parameters are tuned, launch Jupyter (or another IDE) and open Gambot_prod.ipynb.
- Make sure you set the week according to what week is listed on the Polymarket site for that particular sport.
- Execute each code cell in order, ensuring you see no import errors or authentication failures.

### Monitor
Keep an eye on the output logs (according to the LOG_LEVEL_ENV you’ve set). The bot will:
- Pull the latest odds from Pinnacle and Polymarket.
- Calculate potential +EV scenarios.
- Place or adjust bets based on the Kelly Criterion and your target EV thresholds.

## Donations and Sponsorship
If Gambot saves you time and helps generate positive returns, consider giving me a tip :) 

#### GitHub Sponsors
Click the “Sponsor” button here or at the top of this repository for a one-time donation.
[![Sponsor](https://img.shields.io/badge/sponsor-%F0%9F%92%B8-brightgreen)](https://github.com/sponsors/chrisgillam)

#### Crypto Donations
You can send coins to the following addresses:
- USDC: 0xD3b578af99e01A21B6303232BdE4a8B4CC66deFA
- Etherium (ETH) Classic: 0xbF2eFD75458238560c46436d2334df543eD87584
- Bitcoin (BTC): 3QHG3pq1ShFpcBdMYpzqsGtPyUVdKkfuBk

#### Thank you for choosing Gambot! Bet responsibly, manage your risk carefully, and enjoy the process of systematic sports trading.

## Appendix 1: Key Functions
- strategy_v1:  Implements the primary trading strategy, scanning for and identifying value bets.
- buying_shares:  Places bets (buys shares) once a positive EV opportunity has been detected.
- calc_kelly_optimal_and_adjusted_bet_size: Determines best bet sizes using the Kelly Criterion while allowing for a reduced multiplier to mitigate risk.
- calc_target_variables: Prepares target variables for modeling, often used in probability calculations and data analysis.
- merge_and_process_data: Cleans and merges data from multiple sources to ensure consistency and integrity.
- calc_polymarket_decimal_odds: Converts Polymarket odds to a decimal format, facilitating direct comparison with sportsbook data.
- get_mode_params: Retrieves strategy or model parameters, shaping how the bot makes trading decisions.
- strategy_v1_stage_2: A secondary phase of the primary strategy, refining or confirming signals before placing trades.
- pipeline_trading: The orchestrator function that connects data fetching, modeling, bet-sizing, and execution into a seamless pipeline.
- pull_from_pinnacle: Fetches and processes market lines from Pinnacle to serve as the “true odds” benchmark.
