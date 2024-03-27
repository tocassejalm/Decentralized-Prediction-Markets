# Decentralized-Prediction-Markets
Create a WEB3-based prediction market platform that allows users to bet on the outcome of various events using cryptocurrency.
from web3 import Web3

# Connect to Ethereum node
w3 = Web3(Web3.HTTPProvider('https://mainnet.infura.io/v3/YOUR_INFURA_PROJECT_ID'))

# The contract ABI and address
contract_abi = 'CONTRACT_ABI_HERE'
contract_address = Web3.toChecksumAddress('YOUR_CONTRACT_ADDRESS_HERE')

# Create a contract instance
contract = w3.eth.contract(address=contract_address, abi=contract_abi)

# Function to create an event
def create_event(description, outcomes_count):
    # This assumes you have an account set up and unlocked
    tx_hash = contract.functions.createEvent(description, outcomes_count).transact({'from': w3.eth.accounts[0]})
    receipt = w3.eth.wait_for_transaction_receipt(tx_hash)
    print(f'Event created with transaction receipt: {receipt}')

# Function to place a bet
def place_bet(event_id, outcome_id, bet_amount):
    # Convert bet amount to Wei
    value_in_wei = w3.toWei(bet_amount, 'ether')
    # Assuming account is unlocked and has sufficient balance
    tx_hash = contract.functions.placeBet(event_id, outcome_id).transact({'from': w3.eth.accounts[0], 'value': value_in_wei})
    receipt = w3.eth.wait_for_transaction_receipt(tx_hash)
    print(f'Bet placed with transaction receipt: {receipt}')

# Example usage
if __name__ == "__main__":
    create_event("Will it rain tomorrow?", 2)  # Example event with Yes/No outcomes
    place_bet(0, 1, 0.1)  # Example bet on "No" for 0.1 ETH
