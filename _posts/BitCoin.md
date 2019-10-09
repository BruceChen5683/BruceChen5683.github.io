1. make
```
configure: error: libdb_cxx headers missing
fix by:
sudo add-apt-repository ppa:bitcoin/bitcoin
sudo apt-get update
sudo apt-get install -y libdb4.8-dev libdb4.8++-dev
```