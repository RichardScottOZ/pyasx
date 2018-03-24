
# PyASX

Python library to pull ASX stock information via the undocumented API used on
[www.ASX.com.au](https://www.asx.com.au).

Main features are;

 - Pulling the full list of companies & securities listed on the ASX
 - Pulling detailed company information
 - Pulling company regulatory annoucements

While PyASX provides pricing information, using
[AlphaVantage](https://github.com/RomelTorres/alpha_vantage) is a better
option for up to date & historical price data.

_NOTE: This library uses a bunch of undocumented APIs from the ASX.com.au
website and thus could go the way of Google & Yahoo finance with the API
unceremoniously being killed off._

## API

### `pyasx.data.companies.get_listed_companies()`

Pulls a list of all companies listed on the ASX.  This will not include
anything other than companies, i.e. no EFT/ETPs, options, warrants etc.

**Example**

    >>> import pyasx.data.companies
    >>> results = pyasx.data.companies.get_company_info('CBA')
    >>> json.dumps(results, indent=4)
    [
        {
            "ticker": "MOQ",
            "name": "MOQ LIMITED"
            "gics": "Software & Services",
        },
        {
            "ticker": "1PG",
            "name": "1-PAGE LIMITED"
            "gics": "Software & Services",
        },
        {
            "ticker": "ONT",
            "name": "1300 SMILES LIMITED"
            "gics": "Health Care Equipment & Services",
        },
        ...
        {
            "ticker": "ZYB",
            "name": "ZYBER HOLDINGS LTD"
            "gics": "Software & Services",
        }
    ]

### `pyasx.data.companies.get_company_info(ticker)`

Pull information on the company with the given ticker symbol. This also
includes all of the pricing information returned by
`pyasx.data.securities.get_security_info()`.

This will only work for a company, it will not return information on, ETFs,
warrants, indices etc. For that please use
`pyasx.data.securities.get_security_info()`

**Example**

    >>> import pyasx.data.companies
    >>> results = pyasx.data.companies.get_company_info('CBA')
    >>> json.dumps(results, indent=4)
    {
        "ticker": "CBA",
        "name": "COMMONWEALTH BANK OF AUSTRALIA.",
        "name_short": "COMMONWEALTH BANK.",
        "sector": "Financials",
        "gics": "Banks",
        "principal_activities": "Banking, financial and related services.",
        "website": "http://www.commbank.com.au/",
        "mailing_address": "Ground Floor, Tower 1, 201 Sussex Street, SYDNEY, NSW, AUSTRALIA, 2000",
        "phone_number": "(02) 9378 2000",
        "fax_number": "(02) 9118 7192",
        "registry_name": "LINK MARKET SERVICES LTD",
        "registry_phone_number": "1800 022 440",
        "listing_date": "1991-09-12T00:00:00+1000",
        "delisting_date": null,
        "foreign_exempt": false,
        "products": [
            "shares",
            "hybrid-securities",
            "options",
            "warrants"
        ],
        "primary_share": {
            "ticker": "CBA",
            "type": "Ordinary Fully Paid",
            "isin": "AU000000CBA7",
            "open_price": 74.1,
            "last_price": 72.81,
            "offer_price": 72.92,
            "bid_price": 72.8,
            "last_trade_date": "2018-03-23T00:00:00+1100",
            "average_daily_volume": 2610218,
            "day_high_price": 74.16
            "day_low_price": 72.81,
            "day_change_price": -2.06,
            "day_change_percent": "-2.751%",
            "day_volume": 3617914,
            "prev_day_close_price": 74.87,
            "prev_day_change_percent": "-1.201%",
            "year_open_price": 75.27,
            "year_high_price": 87.74,
            "year_high_date": "2017-04-28T00:00:00+1000",
            "year_low_price": 72.81,
            "year_low_date": "2018-03-23T00:00:00+1100",
            "year_change_price": -2.46,
            "year_change_percent": "-3.268%",
            "annual_dividend_yield": 5.91,
            "securities_outstanding": 1752728198,
            "market_cap": 131226760184,
            "pe": 12.68,
            "eps": 5.7442,
            "is_suspended": false,
            "indices": [
                {
                    "code": "XTL",
                    "name": "S&P/ASX 20"
                },
                ...
            ]
        },
        "last_dividend": {
            "type": "",
            "amount_aud": "",
            "franked_percent": "",
            "created_date": "",
            "ex_date": "",
            "record_date": "",
            "books_close_date": "",
            "payable_date": "",
            "comments": ""
        },

    }

### `pyasx.data.companies.get_company_annoucements(ticker)`

Pull the latest company annoucements for the company with the given ticker
symbol. This will only work for companies, it won't work for other securities.

_NOTE_ This currently only pulls the 20 latest _market sensitive_ annoucements.

**Example**

    >>> import pyasx.data.companies
    >>> results = pyasx.data.companies.get_company_annoucements('CBA')
    >>> json.dumps(results, indent=4)
    [
        {
            "num_pages": 106,
            "title": "CommBank PERLS X Capital Notes - Replacement Prospectus",
            "url": "http://www.asx.com.au/asxpdf/20180315/pdf/43sg1vw9rn1yl1.pdf",
            "release_date": "2018-03-15T08:51:00+1100",
            "size": "4.7MB",
            "document_date": "2018-03-15T08:46:07+1100"
        },
        {
            "num_pages": 185,
            "title": "2018 Half Year Results Profit Announcement",
            "url": "http://www.asx.com.au/asxpdf/20180207/pdf/43rd1t86s7g1ll.pdf",
            "release_date": "2018-02-07T07:30:03+1100",
            "size": "19.9MB",
            "document_date": "2018-02-07T07:01:07+1100"
        },
        ...
    ]

### `pyasx.data.securities.get_listed_securities()`

Pull full list of ASX listed securities.

**Example**

    >>> TODO

### `pyasx.data.securities.get_security_info()`

Pull pricing information on the security with the given ticker symbol. This
can be for any type of listed security, such as company stock, bonds, ETFs
etc.

**Example**

    >>> import pyasx.data.companies
    >>> results = pyasx.data.securities.get_security_info('CBAPC')
    >>> json.dumps(results, indent=4)
    {
        "ticker": "CBAPC",
        "type": "Cap Note 3-bbsw+3.80% Perp Non-cum Red T-12-20",
        "isin": "AU0000CBAPC9",
        "open_price": 100.4,
        "last_price": 100.61,
        "offer_price": 100.66,
        "bid_price": 100.6,
        "last_trade_date": "2018-03-23T00:00:00+1100",
        "average_daily_volume": 12781,
        "day_low_price": 100.4,
        "day_high_price": 100.66,
        "day_change_price": 0.18,
        "day_change_percent": "0.179%",
        "day_volume": 18446,
        "prev_day_close_price": 100.43,
        "prev_day_change_percent": "-0.366%",
        "year_open_price": 104.18,
        "year_high_price": 100.66,
        "year_high_date": "2018-03-23T00:00:00+1100",
        "year_low_price": 0,
        "year_low_date": "2018-03-22T00:00:00+1100",
        "year_change_price": -3.57,
        "year_change_percent": "-3.427%",
        "annual_dividend_yield": 3.89,
        "securities_outstanding": 1752728198,
        "market_cap": 131226760184,
        "pe": 0,
        "eps": 0,
        "is_suspended": false,
        "indices": []
    }

## Unit tests

The unit tests can be run by executing the test.py file, like so;

    python3 test.py
