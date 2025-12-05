# Tibber Dashboard Plugin

A comprehensive energy monitoring plugin for TRMNL that displays your Tibber electricity data, including live prices, consumption, costs, and optional solar production statistics.

## Overview

Transform your Tibber energy data into a clear, at-a-glance dashboard on your TRMNL device. Monitor current electricity prices, track daily and monthly consumption, view costs, and if you have solar panels, see your production and earnings. Perfect for staying informed about your energy usage and costs.

## Features

- **Live electricity prices** - Current price per kWh
- **Daily consumption tracking** - See today's energy usage
- **Cost monitoring** - Track daily and monthly costs
- **Solar panel support** - Optional production and earnings display
- **Price charts** - Visual representation of today's and tomorrow's prices
- **Multi-currency support** - Works with € (EUR) and kr (SEK/NOK)
- **Customizable titles** - Personalize all dashboard card titles
- **Location display** - Show your location name

## Supported Countries

- 🇳🇱 The Netherlands (€)
- 🇩🇪 Germany (€) - Possible support
- 🇸🇪 Sweden (kr) - Possible support
- 🇳🇴 Norway (kr) - Possible support

## Settings

### API Key
- **Type:** Text input
- **Required:** Yes
- **Description:** Your Tibber API access token
- **How to get:** Generate at the [Tibber Developer Dashboard](https://developer.tibber.com/settings/access-token)

### Do you have Solar panels?
- **Type:** Dropdown
- **Options:**
  - Yes, I have them
  - No, I don't have them
- **Description:** When you select yes, you get 2 extra cards with information about your solar panels (Production & Reward)
- **Note:** Solar cards only appear when production data is available

### Location name
- **Type:** Text input
- **Default:** "New York"
- **Description:** Display name for your location

### Customizable Card Titles

All dashboard cards have customizable titles:

#### Title: Current price
- **Default:** "Current Price"

#### Title: Consumption
- **Default:** "Consumption"

#### Title: Cost
- **Default:** "Cost"

#### Title: Production (Solar)
- **Default:** "Production (Solar)"
- **Optional:** Only shown when solar panels are enabled

#### Title: Sold for (Solar)
- **Default:** "Sold for"
- **Optional:** Only shown when solar panels are enabled

#### Title: Monthly Consumption
- **Default:** "Monthly Consumption"

#### Title: Monthly Cost
- **Default:** "Monthly Cost"

## Technical Details

- **Strategy:** Polling
- **Polling Method:** POST
- **API Endpoint:** `https://api.tibber.com/v1-beta/gql`
- **Refresh Interval:** 15 minutes
- **Screen Padding:** Yes
- **Dark Mode Support:** No

### API Integration

The plugin uses Tibber's GraphQL API to fetch:
- Current, today's, and tomorrow's electricity prices
- Hourly consumption data (last 48 hours)
- Hourly production data (last 48 hours, if solar panels)
- Daily consumption data (last 62 days for monthly totals)

### Data Polling

The plugin sends a GraphQL query to Tibber's API:

```graphql
query EnergyDashboard {
  viewer {
    homes {
      id
      currentSubscription {
        priceInfo {
          current { total currency }
          today { startsAt total }
          tomorrow { startsAt total }
        }
      }
      todayConsumption: consumption(resolution: HOURLY, last: 48) {
        nodes { from to consumption cost }
      }
      todayProduction: production(resolution: HOURLY, last: 48) {
        nodes { from to production profit }
      }
      monthConsumption: consumption(resolution: DAILY, last: 62) {
        nodes { from to consumption cost }
      }
    }
  }
}
```

## How It Works

1. The plugin authenticates with Tibber's API using your API key
2. Every 15 minutes, it fetches your energy data via GraphQL
3. Data is processed to calculate daily and monthly totals
4. The dashboard displays current prices, consumption, and costs
5. If solar panels are enabled and data exists, production stats are shown
6. Charts visualize price trends for today and tomorrow

### Data Accuracy

⚠️ **Note:** All data depends on Tibber's API and may be slightly delayed (+/- 1 hour). This is normal and due to how smart meter data is reported.

## Dashboard Cards

### Without Solar Panels (4 cards)
1. Current Price
2. Today's Consumption
3. Today's Cost
4. Monthly Consumption & Cost

### With Solar Panels (6 cards)
1. Current Price
2. Today's Consumption
3. Today's Cost
4. Today's Production (Solar)
5. Today's Earnings (Sold for)
6. Monthly Consumption & Cost

## Layout Support

This plugin supports all TRMNL layout sizes:
- Full screen (recommended for complete dashboard view)
- Half horizontal
- Half vertical
- Quadrant

## Setup Guide

### 1. Get Your Tibber API Key

1. Visit [Tibber Developer Dashboard](https://developer.tibber.com/settings/access-token)
2. Log in with your Tibber account
3. Generate a new access token
4. Copy the token (you'll need it for the plugin settings)

### 2. Configure the Plugin

1. Add the Tibber Dashboard plugin to your TRMNL device
2. Paste your API key in the "API Key" field
3. Select whether you have solar panels
4. Set your location name
5. (Optional) Customize the card titles to your preference
6. Save and enjoy your energy dashboard!

## Privacy & Security

- Your API key is stored securely by TRMNL
- Data is fetched directly from Tibber's official API
- No third-party services access your energy data
- API calls are made server-side for security

## Troubleshooting

**No data appearing?**
- Verify your API key is correct
- Ensure you have an active Tibber subscription
- Check that your Tibber account has consumption data

**Solar cards not showing?**
- Verify "Do you have Solar panels?" is set to "Yes"
- Ensure your Tibber account has production data
- Solar cards only appear when production data exists

**Data seems delayed?**
- This is normal - Tibber data can be delayed by up to 1 hour
- The plugin shows the last update timestamp for reference

## Resources

- [Tibber Website](https://tibber.com)
- [Tibber Developer Portal](https://developer.tibber.com)
- [Tibber API Documentation](https://developer.tibber.com/docs/overview)
