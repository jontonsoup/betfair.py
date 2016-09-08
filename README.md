betfair2.py is a Python wrapper for the Betfair API. 

## Installation
```Python
>> pip install git+https://github.com/BenderV/betfair2.py.git
```

##### Requirements
-   Python &gt;= 2.7 or &gt;= 3.3

##### Application Keys

You will need an Application Key to log in to the Betfair API. You can find instructions for creating a key at <https://api.developer.betfair.com/services/webapps/docs/display/1smk3cen4v3lu3yomq5qye0ni/Application+Keys>

##### SSL Certificates

For non-interactive login, you must generate a self-signed SSL certificate and upload it to your Betfair account. Betfair.py includes tools for simplifying this process. To create a self-signed certificate, run (**you must clone this repo for this to work**) :

    invoke ssl

This will generate the following files in the `certs` directory :

    betfair.crt
    betfair.csr
    betfair.key
    betfair.pem

You can write SSL certificates to another directory by passing the `--name` parameter :

    invoke ssl --name=path/to/certs/betfair

Once you have generated the files, you can upload the `betfair.crt` file to Betfair at <https://myaccount.betfair.com/accountdetails/mysecurity?showAPI=1>.

## Examples

Create a Betfair client and log in :

 ```Python
>> from betfair import Betfair
>> client = Betfair('app_key', 'certs/betfair.pem')
>> client.login('username', 'password')
```
Refresh session token :
 ```Python
>> client.keep_alive()
```

Log out :
```Python
>> client.logout()
```

List all tennis markets :
```Python
>> from betfair.models import MarketFilter
>> event_types = client.list_event_types(
        MarketFilter(text_query='tennis')
    )
>> print(len(event_types))                 # 2
>> print(event_types[0].event_type.name)   # 'Tennis'
>> tennis_event_type = event_types[0]
>> markets = client.list_market_catalogue(
        MarketFilter(event_type_ids=[tennis_event_type.event_type.id],
                     market_projection=["EVENT", "COMPETITION", "RUNNER_METADATA",
                                        "MARKET_START_TIME", "RUNNER_DESCRIPTION", 
                                        "MARKET_DESCRIPTION", "EVENT_TYPE"],
                     max_results=100))
    )
>> markets[0].market_name                  # 'Djokovic Tournament Wins'
```
                                              
Transform model to dictionary
```Python
>> from betfair import utils
>> markets = utils.model_to_dict(markets)
```
  

#### Original author

Joshua Carp (jmcarp)

#### License
MIT licensed. See the bundled [LICENSE] file for more details

#### Testing
To run tests :

    $ py.test

  [LICENSE]: https://github.com/jmcarp/betfair.py/blob/master/LICENSE
