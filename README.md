## Role: You work at a startup that is building a new and disruptive platform called Fintech Finder.

## Goal: Fintech Finder is an application that its customers can use to find fintech professionals from among a list of candidates, hire them, and pay them.
------

# Specifically 

* You will assume the perspective of a Fintech Finder customer in order to do the following:

* Generate a new Ethereum account instance by using your mnemonic seed phrase (which you created earlier in the module).

* Fetch and display the account balance associated with your Ethereum account address.

* Calculate the total value of an Ethereum transaction, including the gas estimate, that pays a Fintech Finder candidate for their work.

* Digitally sign a transaction that pays a Fintech Finder candidate, and send this transaction to the Kovan testnet.

* Review the transaction hash code associated with the validated blockchain transaction.

------

### Overview 

* The steps for this challenge are broken out into the following sections:

* Import Ethereum Transaction Functions into the Fintech Finder Application
* Sign and Execute a Payment Transaction
* Inspect the Transaction on Etherscan
------

### Types Of Technology used
This Blockchain was built on the python platform using streamlit. 

* Streamlit is an open-source Python library that makes it easy to create and share beautiful, custom web apps for machine learning and data science. 

https://docs.streamlit.io/en/stable/


![pythonblockchain](https://media.onlinecoursebay.com/2019/07/30020503/2454264_fd7c-750x405.jpg)

![streamlitblockchain](https://blog.jcharistech.com/wp-content/uploads/2020/11/workingwithstfileuploads_streamlit_jcharistech.png)

-----

## Sample code 
```
# Imports
import os
import requests
from dotenv import load_dotenv
load_dotenv()
from bip44 import Wallet
from web3 import Account
from web3.auto.infura.kovan import w3
from web3 import middleware
from web3.gas_strategies.time_based import medium_gas_price_strategy

################################################################################
# Wallet functionality

def generate_account():
    """Create a digital wallet and Ethereum account from a mnemonic seed phrase."""
    # Fetch mnemonic from environment variable.
    
    mnemonic = os.getenv("MNEMONIC")

    # Create Wallet Object
    wallet = Wallet(mnemonic)

    # Derive Ethereum Private Key
    private, public = wallet.derive_account("eth")

    # Convert private key into an Ethereum account
    account = Account.privateKeyToAccount(private)

    return account

def get_balance(address):
    #"""Using an Ethereum account address access the balance of Ether"""
    # Get balance of address in Wei
    wei_balance = w3.eth.get_balance(address)

    # Convert Wei value to ether
    ether = w3.fromWei(wei_balance, "ether")

    # Return the value in ether
    return ether


def send_transaction(account, to, wage):
    """Send an authorized transaction to the Kovan testnet."""
    # Set gas price strategy
    w3.eth.setGasPriceStrategy(medium_gas_price_strategy)

    # Convert eth amount to Wei
    value = w3.toWei(wage, "ether")

    # Calculate gas estimate
    gasEstimate = w3.eth.estimateGas({"to": to, "from": account.address, "value": value})

    # Construct a raw transaction
    raw_tx = {
        "to": to,
        "from": account.address,
        "value": value,
        "gas": gasEstimate,
        "gasPrice": w3.eth.generateGasPrice(),
        "nonce": w3.eth.getTransactionCount(account.address)
    }

    # Sign the raw transaction with ethereum account
    signed_tx = account.signTransaction(raw_tx)

    # Send the signed transactions
    return w3.eth.sendRawTransaction(signed_tx.rawTransaction)

------
# Import the following functions from the `crypto_wallet.py` file:
# * `generate_account`
# * `get_balance`
# * `send_transaction`

from crypto_wallet import generate_account
from crypto_wallet import get_balance
from crypto_wallet import send_transaction 

#  Call the `generate_account` function and save it as the variable `account`
# YOUR CODE HERE

account = generate_account()

##########################################

# Write the client's Ethereum account address to the sidebar
st.sidebar.write(account.address)

# Call `get_balance` function and pass it your account address
# Write the returned ether balance to the sidebar
# YOUR CODE HERE

account_ = get_balance(account.address)
st.sidebar.write(account)

# Call the `send_transaction` function and pass it 3 parameters:
    # Your `account`, the `candidate_address`, and the `wage` as parameters
    # Save the returned transaction hash as a variable named `transaction_hash`
    # YOUR CODE HERE
    
    transaction_hash = send_transaction(account, candidate_address, wage)

    # Markdown for the transaction hash
    st.sidebar.markdown("#### Validated Transaction Hash")

    # Write the returned transaction hash to the screen
    st.sidebar.write(transaction_hash)

    # Celebrate your successful payment
    st.balloons()


```
# Validating pics 
Sent from the postal side

![sent](https://user-images.githubusercontent.com/73854785/118416273-bdf68780-b663-11eb-9b2e-50c816959062.png)
-----
Recived on https://kovan.etherscan.io 

![recived](https://user-images.githubusercontent.com/73854785/118416291-cc44a380-b663-11eb-97c1-eece837522f1.PNG)

