# crypto-prices

## Resources
* Cowsay: https://github.com/tnalpgge/rank-amateur-cowsay
* Cowsay Art: https://github.com/tnalpgge/rank-amateur-cowsay/tree/master/cows
* Get key, value w/ jq: https://stackoverflow.com/questions/34226370/jq-print-key-and-value-for-each-entry-in-an-object
* Run from anywhere: https://shapeshed.com/using-custom-shell-scripts-on-osx-or-linux/

## Installation (MacOS)
```
brew install cowsay
brew install jq
```

## Usage
* 3 examples
```
cowsay hello
echo goodbye | cowsay -f tux
cowsay -f dragon-and-cow Bitcoin 17,800 \[+5%\]
```

## Crypto price Usage
* To get coin ids from Coingecko
```
curl -X 'GET' \
  'https://api.coingecko.com/api/v3/coins/list' \
  -H 'accept: application/json'
```

* To get price from id in usd
```
curl -X 'GET' \
  'https://api.coingecko.com/api/v3/simple/price?ids=bitcoin&vs_currencies=usd' \
  -H 'accept: application/json'
```
* To get price from various ids in usd
```
curl -X 'GET' \
  
'https://api.coingecko.com/api/v3/simple/price?ids=bitcoin,ethereum,binancecoin,cardano,solana,polkadot,matic-network,monero,cosmos,oasis-network&vs_currencies=usd' 
\
  -H 'accept: application/json'
```

* To get keys and values from Coingecko json w/ jq
```
curl -X 'GET' \
  'https://api.coingecko.com/api/v3/simple/price?ids=bitcoin,ethereum,binancecoin,cardano,solana,polkadot,matic-network,monero,cosmos,oasis-network&vs_currencies=usd' \
  -H 'accept: application/json' | jq -r 'keys[] as $k | "\($k): \(.[$k] | .usd) USD\n"'
```

* Putting everything together (w/ silent curl for data transfer)
```
curl -s -X 'GET' \
  'https://api.coingecko.com/api/v3/simple/price?ids=bitcoin,ethereum,binancecoin,cardano,solana,polkadot,matic-network,monero,cosmos,oasis-network&vs_currencies=usd' \
  -H 'accept: application/json' | jq -r 'keys[] as $k | "\($k): \(.[$k] | .usd) USD\n"' | cowsay -f dragon-and-cow
```

## Making a Script
```
nano crypto_prices
curl -s -X 'GET' 'https://api.coingecko.com/api/v3/simple/price?ids=bitcoin,ethereum,binancecoin,cardano,solana,polkadot,matic-network,monero,cosmos,oasis-network&vs_currencies=usd'  -H 'accept: application/json' | jq -r 'keys[] as $k | "\($k): \(.[$k] | .usd) USD\n"' | cowsay -f dragon-and-cow
chmod 744 crypto_prices
```

## Execution (for current directory)
```
./crypto_prices
```

## Making the Script available from anywhere (w/ Z shell)
```
mkdir $HOME/bin
nano $HOME/.zshrc 
export PATH="$PATH:$HOME/bin"
```

## Execution (from anywhere)
```
crypto_prices
```
