# s3p3
stock screener system / portfolio

Simple Stock Screener (S3) / Permanent Portfolio Projector (P3)

How is S3 / P3 different from all the other investing apps out there?
 
Goal
 
My goal with S3/P3 is to help an private, market investor maximize the performance of their portfolio by openly considering multiple investment recommendations.  S3/P3 will determine when a financial product -- whether common stock in a company or various bonds -- meets investment criteria for an ideal portfolio.  The screener will perform time-lapse analysis based on past indicators, identify the present context of these indicators, and verify a strategy that maximizes investments until future conditions change.  A well known disclaimer in this methodology is that past performance does not cause future performance, however if you go down that path of pessimism then a private “speculator” should trust sheer chance over any professional investor’s recommendation.  I’ve found little analytical -- or computational -- justification for preferring a professionally recommended investment strategy besides the proverbial “it worked for me”.  By approaching the investment professionals recommendations via a analytical method of general evaluation, S3/P3 can systematically determine the validity of any given set of investment recommendations by computing its results from public market records.  Like any good, personal finance application, S3/P3 goal is both generally and specifically to assist the user in re-targeting their screening and re-adjusting their portfolio based on new data and approaches.

S3 will use historical market and business data to determine when a financial product had met some investment criteria, then simulate trades of the product in time-lapse, compare the return(s) of the investment styles, and provide contextual reports that a trader will use to re-adjust their portfolio.  S3/P3 will compare returns based on general criteria used by major schools of thought in investing and, obviously, only for financial products that have enough data to actually make such comparisons.  It will sample the major investment schools of thought including: value, growth, GARP, and technical.  Rather than being too technical (by essentially only looking at charts), the P3 part of this simulation will attempt to include correlations to non-financial data -- including timing and periodicity, and external influences like news tickers and weather -- in an attempt to reveal new criteria or conditions for maximizing portfolios existing in similar contexts.

Bias
 
Like any decent human, I must admit personal bias before attempting to make something that claims to be different.  My bias is that recent developments in computer driven trading systems and automated technical trading, aka. algorithmic trading firms, have undermined the performance previously seen by private, individual traders who have been using stock screening tools based on traditional schools of thought.  Modern trading firms have also limited the individual trader’s ability to quickly react to changing markets because, to put it simply: a computer trading system may errant trades just like a human trader but a computer can always make a trade faster than any human.  I suspect S3/P3 could be used to expose the impact of high volume, algorithmic trading of to the private investor.  To focus on how a human trader can still make returns in the 21st century, I’ll pay special attention S3 simulations during the years when algorithmic computer systems began executing substantial volumes of trades in equity markets.  This appears to be 2007 and after.

Baseline / Verification

Both Investopedia Simulator and YCharts Screener can be used to compare the S3 results for accuracy, although the updated simulator from Investopedia only does forward hypothetical trades, not historical.
 
Framework
 
S3/P3 should be kept as simple as possible given my nature to complicate things and prevent scalability.  There are just a few key components to the application:
 
1.       Market data (S3) – EPS, PEG, etc.
2.       Business data (S3) – cash flow, debt, etc.
3.       Non-financial data (P3) – news headlines, climate, etc.
4.       Investment conditions (S3)
5.       Observed correlations (P3)
 
Components #1 through #3 will be data repositories that the application must parse, manipulate, compute, and possibly store for long term simulations and correlations.  #4 will be the algorithmic expression of each investment style for a financial prodcut.  I will pseudo code this part first so that types of data needed are known beforehand.  #5 will be done last, and I don’t know how.  Hopefully there is some generalized way to compare datasets and extract relationships.

Technical Components

Component #1 is available from Yahoo! Finance API, either daily data with full financial metrics or historical data with only price and volume metrics.  I will use CSV payloads to extract these data.  #2 is available from Google’s Finance API and the SEC as XBRL; but the latter has more historical data and requires much a new parser.  #3 can be gotten from sources like YQL weather and Google Domestic Trends.  #4 is the heart of the application.  I originally aimed at coding it in Python incase this application needs to scale to something as big as Google Application Engine or AWS, but after changing jobs I’ve settled on writing the core logic in Ruby (and possibly a DSL for a rules engine, like BRMS / RETE).   #5 is unknown but perhaps some “intelligent” computing will actually drive #3.

User WorkFlow

The user will request a product screening from a web front-end that triggers additional functionality.  This front-end will be a simple form page running server-side that prompts for a return target, the financial product (such as a stock), or the investment style. A mobile app version with similar functionality and remote management of the backend will also be included (AeroGear / PhoneGap).  JBoss/Tomcat Web Server will process form submissions and initiate web requests to middleware for processing a new screening.  This request for a screening will be exposed as a server-side web service running JBoss RESTEasy. S3 will validate data to determine if the request can be fulfilled given the amount of knowledge (in current datasets).  A rules engine, powered by JBoss Drools RETE, will be triggered from this web service on and run against the set of facts collected by backend datasets.  If the most recent and required data is already stored by S3/P3, then Drools will determine investment style or financial products that meets or exceeds the return.  Drools rules will represent both the investment schools and their conditions and algorithms. The return payload from the rules engine should be decoupled from the front-end incase long running processing occurs, such as a message consumed by the front-end that refreshes the web page to reveal the payload.  To optimize the return time by keeping current known ideal portfolio, P3 functionality will include refreshing the highest returns based on financial product and investment style after any market, business, or related dataset is updated.  S3 will schedule regular times to scrape and transform daily and quarterly datasets to reduce think time to the front-end user.  Apache Camel will be used to define the process flow for the interaction between the front-end and back-end.  All entities will persist data needed for future calculations via Hibernate ORM to Postgres tables.  These components is part of the batch oriented back-end. 

To simplify the development and operations of S3/P3, RedHat Openshift will be the PaaS solution.  This solution includes version control and code repository (Git), build and deploy management for delivery (Jenkins), and runtime / request containers / engines for execution (EAP, BRMS, and FUSE).

General Notes on #2

Analysis of Fundamentals
http://www.investopedia.com/university/fundamentalanalysis/
Cash Flow Indicators
http://www.investopedia.com/university/ratios/cash-flow-indicator/
Technical Valuation
http://www.investopedia.com/university/ratios/investment-valuation/

General Notes on #2 / #3

Ancillary Economic Indicators / Releases:
http://www.investopedia.com/university/releases/
 
General Notes on #4

Basics of Private Investments
http://www.investopedia.com/university/concepts/
Financial Products (for Private Investors)
http://www.investopedia.com/university/20_investments/
Traditional Investment Styles/Schools (for Stocks)
http://www.investopedia.com/university/stockpicking/
Quality Investment (for Mutual Funds)
http://www.investopedia.com/university/quality-mutual-fund/


Investment Styles (for Stocks)
I. Technical Analysis / Dow Theory - Charles Dow, John Murphy
http://www.investopedia.com/university/dowtheory/
I.b. Chart Analysis
http://www.investopedia.com/university/charts/
 
II. Value Investing - Benjamin Graham, Warren Buffett
            	best of Bears
            	good earnings, dividends, book value, cash flow and low price (aka. buy undervalued stock)
            	1. low Beta
            	2. price < 2/3 total value
            	3. P/E in subset of 10% lowest of all equities (the bottom P/E stocks)
            	3.b. P/E < avg P/E of industry
            	4. PEG < 1 (undervalued)
            	5. price < book value
            	6. Debt/Equity < 1 (less debt held than equity available)
            	7. assets / liabilities > 2 (total assets available should be twice the liabilities held)
			avail via Google Finance : Balance Sheet
            	8. dividend > 2/3 * AAA bond
            	9. earnings growth > 7% per annum
            	
III. Growth Investing - T. Rowe Price
            	best of Bulls
            	1. P/E >50
            	2. + EPS annually (increased earnings every year)
            	3. estimated earnings growth >10% / year
            	4. pre-tax profit stable or increasing ( annual revenue growth && EPS increase indicate profit margins are steady )
            	5. ROE stable or increasing
 
IV. GARP (Growth at a Reasonable Price) - Peter Lynch;
            	1. undervalued company
            	2. + earnings over annual reports (positive earnings momentum)
            	3. + earnings estimates
            	4. growth estimates < 25%
            	5. growth estimate >10 & <20 = ideal
            	6. ROE increasing
            	7. ROE > industry
            	8. + cash flow
            	9. P/E >15 & <20
            	10. P/B < industry
            	11. PEG <1 & >0.5
Efficient Market Hypothesis - de-regulate; BAD

General SubNotes on #4

algorthims for stock screening

P/E
http://www.investopedia.com/university/peratio/
 
1. if p/e > avg p/e, then speculative growth in value or overpriced
2. if p/e < avg p/e, then speculative decline in value or underpriced
3. if P/E =< EPS, then buy

Additional definitions and explanations
1. P/E - ratio of price to earnings; price of a stock over EPS (earnings per share)
2. EPS - earnings per share; annual or quarterly earnings
3. ROE = Return on Equity
4. P/B = Price to Book value
5. PEG = Price to Earnings to Growth
 
Additional historical dataset not provided by public repositories:
1. avg p/e - average P/E over some period of time. calculate this for trending

Subnotes #4 Addendum: Financial Products
Mutual Funds
http://www.investopedia.com/university/quality-mutual-fund/

Source Notes on #1

Historical Data:

Two decisions about historical data: 1) create a data gather now that will record my own historical dataset of the entire set of publicly available security data, and 2) create a data parser of the subset of publicly available company data (earnings, income, balance).  (1) is done via Yahoo’s /d Quotes Webservice. (2) is done via Google q? Webservice.  Both services can reply with CSV data.
 
Yahoo Historical Price (hp?):
http://ichart.finance.yahoo.com/table.csv?s=IBM&a=00&b=1&c=2012&d=00&e=1&f=2013&g=d&ignore=.csv
http://code.google.com/p/yahoo-finance-managed/wiki/csvHistQuotesDownload

 
Yahoo Market Data - downloadable CSV:
http://finance.yahoo.com/d/quotes.csv?s=IBM&f=ee7e8
http://www.jarloo.com/yahoo_finance/
http://www.gummy-stuff.org/Yahoo-data.htm
 
Yahoo Analyst Estimates (ae?):
http://finance.yahoo.com/q/ae?s=IBM
 
Yahoo Query Language (YQL)
http://www.jarloo.com/get-yahoo-finance-api-data-via-yql/
http://developer.yahoo.com/yql/

Mathmatica FinancialData[]

FINRA Bond Market History
http://finra-markets.morningstar.com/BondCenter/

Historal International Data Reference:
until "Triumph Of The Optimists" was published, most of the available historical stock market data for the years prior to 1970 was only for the U.S. market

NSE (India stock exchange) API for Python
http://nsetools.readthedocs.io/en/latest/usage.html


Source Notes on #2

EDGAR filings as XBRL from the SEC. If no known open source parsers for XBRL are found, I must write one.
http://xbrl.sec.gov/
http://www.sec.gov/spotlight/xbrl/filings-and-feeds.shtml

EX: http://www.sec.gov/cgi-bin/srch-edgar?text=microsoft+10-K&first=2003&last=2013


Cash flow, income, balance sheet
https://gist.github.com/knoguchi/6952087

Competitive Brief

Investopedia Stockscreener

Source Notes on #3

industry / sector influence:
            	Google Domestic Trends (US only)
weather / harvest:
            	Yahoo weather WOEID

FUSE/Camel Notes:
	http://www.redhat.com/about/events-webinars/webinars/2013-10-16-training-simplify-integration
w/ on OpenShift:
https://www.openshift.com/blogs/jboss-fuse-on-openshift-how-to-connect-to-twitter


Integration SOA / ESB + BRMS + jBPM = see Eric Schabells “get rocking” webinar
	BRMS + FUSE demo
	GateIn OpenShift


Testing:

Arquillian
JUnit Tests	
Load Perf Tools 
http://bharath-marrivada.blogspot.com/2011/03/gomez-neoload-loadrunner-selenium.html
