Project Title
MyERC20Token

Simple Overview
A simple ERC20 token implementation using HardHat. The token supports minting, burning, and transferring functionalities.

Description
MyERC20Token is a project that demonstrates how to create and deploy an ERC20 token on the Ethereum blockchain using HardHat. The project includes functionalities for minting tokens by the contract owner, burning tokens by any user, and transferring tokens between users. This project is ideal for learning about smart contracts and the ERC20 token standard.

Getting Started
Installing
Clone the repository:

bash
Copy code
git clone <repository-url>
cd my-erc20-token
Install dependencies:

bash
Copy code
npm install
Executing Program
1. Compile the contract:

bash
Copy code
npx hardhat compile
2. Deploy the contract:

bash
Copy code
npx hardhat deploy --network hardhat
3. Interact with the contract:

bash
Copy code
npx hardhat run scripts/interact.js --network hardhat
Help
If you encounter any issues, try the following steps:

Ensure all dependencies are installed correctly.
Verify your Node.js version is compatible with HardHat.
Check if the HardHat network is running correctly.
For additional help, you can use the HardHat documentation:

bash
Copy code
npx hardhat help
Authors
Your Name
Contact: [Your Contact Information]
License
This project is licensed under the MIT License - see the LICENSE.md file for details.

Project Structure
Project Setup
Create a new directory for your project and navigate into it:

bash
Copy code
mkdir my-erc20-token
cd my-erc20-token
Initialize a new Node.js project:

bash
Copy code
npm init -y
Install HardHat and necessary dependencies:

bash
Copy code
npm install --save-dev hardhat @openzeppelin/contracts ethers hardhat-deploy
Create a HardHat project:

bash
Copy code
npx hardhat
Choose "Create a basic sample project".

ERC20 Smart Contract
Create a file contracts/MyToken.sol:

solidity
Copy code
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.0;

import "@openzeppelin/contracts/token/ERC20/ERC20.sol";
import "@openzeppelin/contracts/access/Ownable.sol";

contract MyToken is ERC20, Ownable {
    constructor() ERC20("MyToken", "MTK") {}

    function mint(address to, uint256 amount) external onlyOwner {
        _mint(to, amount);
    }

    function burn(uint256 amount) external {
        _burn(msg.sender, amount);
    }
}
HardHat Configuration
Update hardhat.config.js:

javascript
Copy code
require("@nomiclabs/hardhat-waffle");
require("@nomiclabs/hardhat-ethers");
require("hardhat-deploy");

module.exports = {
  solidity: "0.8.0",
  networks: {
    hardhat: {},
  },
  namedAccounts: {
    deployer: {
      default: 0,
    },
  },
};
Deployment Script
Create a file deploy/01_deploy_my_token.js:

javascript
Copy code
module.exports = async ({ getNamedAccounts, deployments }) => {
  const { deploy } = deployments;
  const { deployer } = await getNamedAccounts();

  await deploy("MyToken", {
    from: deployer,
    log: true,
  });
};

module.exports.tags = ["MyToken"];
Interaction Script
Create a file scripts/interact.js:

javascript
Copy code
const { ethers } = require("hardhat");

async function main() {
  const [deployer, user] = await ethers.getSigners();
  const MyToken = await ethers.getContract("MyToken", deployer);

  console.log("Minting tokens to user...");
  await MyToken.mint(user.address, ethers.utils.parseUnits("100", 18));
  console.log("User balance:", (await MyToken.balanceOf(user.address)).toString());

  console.log("Burning tokens from user...");
  await MyToken.connect(user).burn(ethers.utils.parseUnits("50", 18));
  console.log("User balance after burn:", (await MyToken.balanceOf(user.address)).toString());
}

main()
  .then(() => process.exit(0))
  .catch(error => {
    console.error(error);
    process.exit(1);
  });
By following these steps and using the provided code, you'll be able to create, deploy, and interact with an ERC20 token on the Ethereum blockchain using HardHat
