# RandomWisdomDistributor
Project: Random Wisdom Distributor Concept: Users can call a function on the smart contract to receive a random piece of wisdom from a predefined list. It’s a playful project that’s simple yet interactive, showcasing smart contract deployment and random number generation on the Scroll network.

1.Smart Contract: Solidity (Version 0.8.x)

// SPDX-License-Identifier: MIT
pragma solidity ^0.8.0;

contract RandomWisdomDistributor {
    
    string[] private wisdoms = [
        "The early bird catches the worm.",
        "A journey of a thousand miles begins with a single step.",
        "Fortune favors the bold.",
        "Knowledge is power.",
        "Patience is a virtue.",
        "Every cloud has a silver lining."
    ];

    // A simple function to get a random wisdom from the list
    function getWisdom() public view returns (string memory) {
        uint256 randomIndex = getRandomNumber() % wisdoms.length;
        return wisdoms[randomIndex];
    }

    // Generates a pseudo-random number using block information
    function getRandomNumber() internal view returns (uint256) {
        return uint256(keccak256(abi.encodePacked(block.timestamp, block.difficulty, msg.sender)));
    }
}

2. Setup on Hardhat
   
Install Dependencies:
npm init -y
npm install --save-dev hardhat ethers @alchemy/sdk
Configure Hardhat for Scroll: In hardhat.config.js, add the Alchemy RPC URL for Scroll:
require('@nomiclabs/hardhat-ethers');

module.exports = {
  solidity: "0.8.18",
  networks: {
    scroll: {
      url: process.env.ALCHEMY_SCROLL_URL, // Set your Alchemy URL here
      accounts: [process.env.PRIVATE_KEY]   // Add your wallet private key
    }
  }
};

3. Deploy the Contract: Write a deployment script in scripts/deploy.js:
   
async function main() {
  const RandomWisdomDistributor = await ethers.getContractFactory("RandomWisdomDistributor");
  const wisdomContract = await RandomWisdomDistributor.deploy();
  await wisdomContract.deployed();
  console.log("Contract deployed to:", wisdomContract.address);
}

main().catch((error) => {
  console.error(error);
  process.exitCode = 1;
});

4. Run the Deployment:
npx hardhat run scripts/deploy.js --network scroll

