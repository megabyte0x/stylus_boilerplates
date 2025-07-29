DEFAULT_RPC := http://localhost:8547

DEFAULT_PRIVATE_KEY := 0xb6b15c8cb491557369f3c7d2c287b053eb229daa9c22138887752191c9520659

# Read contract address from address.json if it exists
CONTRACT_ADDRESS := $(shell if [ -f address.json ]; then jq -r '.address' address.json 2>/dev/null || echo ""; else echo ""; fi)

# Check if contract is deployed
check-deployed:
	@if [ -f address.json ] && [ "$$(jq -r '.address' address.json 2>/dev/null)" != "" ] && [ "$$(jq -r '.address' address.json 2>/dev/null)" != "null" ]; then \
		echo "Contract deployed at: $$(jq -r '.address' address.json)"; \
	else \
		echo "contract not deployed"; \
	fi

# Deploy the contract and save the address to a file
deploy:
	cargo stylus deploy -e $(DEFAULT_RPC) --no-verify --private-key $(DEFAULT_PRIVATE_KEY) | tee deploy_output.txt
	@if grep -q "deployed code at address:" deploy_output.txt; then \
		ADDRESS=$$(grep "deployed code at address:" deploy_output.txt | sed 's/.*deployed code at address: //' | sed 's/\x1b\[[0-9;]*[a-zA-Z]//g'); \
		echo '{"address": "'$$ADDRESS'"}' > address.json; \
		echo "Address saved to address.json: $$ADDRESS"; \
	else \
		echo "Deployment failed or address not found in output"; \
		exit 1; \
	fi


run: 
	cast call --rpc-url $(DEFAULT_RPC) --private-key $(DEFAULT_PRIVATE_KEY) $(CONTRACT_ADDRESS) "user_main(uint256)()" 1





