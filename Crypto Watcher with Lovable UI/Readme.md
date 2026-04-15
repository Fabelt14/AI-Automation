# CRYPTO WATCHER: AUTOMATED MARKET INTELLIGENCE & REPORTING

## Executive Summary
Crypto Watcher is an automated intelligence pipeline designed to monitor cryptocurrency markets, detect actionable volatility, and instantly distribute formatted reports across multiple channels. By eliminating manual market monitoring and filtering out data noise, this system ensures stakeholders receive real-time, business-critical updates exactly when significant market movements occur.

## Automation Logic & Processing Pipeline
The core engine of this system is a modular, serverless automation pipeline. Each "node" in this sequence performs a specific, automated task to acquire, process, and distribute market intelligence.

1. **The Trigger (Webhook):** Acts as the system's intake valve. It listens for a scheduled external signal to wake up the system and initiate a market scan.
2. **Data Acquisition (HTTP/API):** Instantly connects to global financial databases (CoinGecko) to fetch live, raw market data, pulling current prices, market capitalization, and 24-hour trajectories.
3. **Data Processor (Iterator):** Raw market data is massive and unstructured. This node systematically breaks down the bulk data into individual, manageable assets for precise evaluation.
4. **Business Logic Engine (Filter):** The most critical node for preventing "alert fatigue." It evaluates every asset against strict parameters. The system is programmed to only pass data forward if an asset experiences a price swing of 1% or greater. If the market is stagnant, the system stays quiet.
5. **Report Compiler (Text Aggregator):** Takes the raw, filtered data and automatically formats it into clean, readable financial figures (e.g., converting raw integers into localized Nigerian Naira with proper comma placement).
6. **Channel Router:** The final traffic controller. It simultaneously splits the finalized report into different formats tailored for specific delivery channels (Internal vs. Public).

![The complete automation pipeline showing the linear flow from data ingestion and filtering to omnichannel routing.](image.png)
*Figure 1: The complete automation pipeline showing the linear flow from data ingestion and filtering to omnichannel routing.*

## Channel Delivery System
To ensure maximum reach and utility, the system distributes intelligence across two distinct channels, automatically formatting the data to meet the requirements of each platform.

* **Internal Communications (Email):** Delivered via the Gmail API, this channel provides a detailed, structured format using HTML. It acts as the internal reporting mechanism for stakeholders to review market shifts in a clean, comprehensive layout.
* **Public Engagement (X / Twitter):** Delivered via Buffer integration, this channel is optimized for audience growth and public alerting. Because X enforces strict character limits and rejects HTML, the system automatically sanitizes the payload, stripping code and formatting it into a punchy, plain-text alert that maximizes platform engagement.

![The internal email report, cleanly formatted for rapid stakeholder review.](image.png)
*Figure 2: The internal email report, cleanly formatted for rapid stakeholder review.*

![The sanitized, plain-text alert automatically optimized and published to the X timeline.](image.png)
*Figure 3: The sanitized, plain-text alert automatically optimized and published to the X timeline.*

## Executive Command Dashboard
To complement the automated push alerts, a centralized, web-based dashboard was developed.

* **Business Impact:** While the automation pipeline alerts users to sudden changes, the dashboard provides a 24/7 visual command center. It empowers decision-makers to instantly view the top market assets, live prices, market cap, and total trading volume at a glance.
* **Functionality:** Built with a modern, dark-mode interface, it connects directly to live market feeds. It features dynamic color-coding (green for positive growth, red for decline) to allow for immediate visual assessment of market health.

![The live Executive Command Dashboard featuring dynamic metric tracking and direct market API integration.](image.png)
*Figure 4: The live Executive Command Dashboard featuring dynamic metric tracking and direct market API integration.*

## Autonomous Operations (The Scheduler)
A system is only fully automated if it operates without human intervention. To achieve this, the pipeline is governed by an external CRON Job.

* **Business Impact:** This represents true "zero-touch" operation. By outsourcing the trigger mechanism to a dedicated scheduling server, we eliminate the need for manual button-clicking or dedicated staff monitoring.
* **Functionality:** The CRON Job reliably pings the system's Webhook on a precise, continuous schedule. It ensures the market is scanned consistently, 24 hours a day, 7 days a week, guaranteeing no significant market movement is missed.

![The external scheduling configuration that ensures the system operates 24/7 with zero human intervention.](image.png)
*Figure 5: The external scheduling configuration that ensures the system operates 24/7 with zero human intervention.*
