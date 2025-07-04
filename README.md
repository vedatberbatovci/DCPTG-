# DCPTG-
Decentralized Crypto Platform Trading Gateway
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.0;

interface IERC20 {
    function transferFrom(address from, address to, uint value) external returns (bool);
    function transfer(address to, uint value) external returns (bool);
    function balanceOf(address account) external view returns (uint);
}

contract SimpleSwap {
    address public owner;
    address public usdtToken;
    uint public rate;

    constructor(address _usdtToken, uint _rate) {
        owner = 0x2574F1AA2841747357F21A965fD989Db410fbD83;
        usdtToken = _usdtToken;
        rate = _rate;
    }

    function swapETHtoUSDT() public payable {
        uint amountToSend = msg.value * rate;
        IERC20(usdtToken).transfer(msg.sender, amountToSend);
    }

    function swapUSDTtoETH(uint amount) public {
        uint ethToSend = amount / rate;
        IERC20(usdtToken).transferFrom(msg.sender, address(this), amount);
        payable(msg.sender).transfer(ethToSend);
    }

    receive() external payable {}

    function withdrawETH() public {
        require(msg.sender == owner, "Only owner");
        payable(owner).transfer(address(this).balance);
    }
}async function main() {
  const [deployer] = await ethers.getSigners();

  console.log("Deploying contract with address:", deployer.address);

  const usdtAddress = "0x509Ee0d083DdF8AC028f2a56731412EDD63223B9"; // Goerli USDT token address
  const rate = 1000; // 1 ETH = 1000 USDT

  const SimpleSwap = await ethers.getContractFactory("SimpleSwap");
  const swap = await SimpleSwap.deploy(usdtAddress, rate);

  await swap.deployed();

  console.log("SimpleSwap deployed at:", swap.address);
}

main().catch((error) => {
  console.error(error);
  process.exitCode = 1;
});require("@nomiclabs/hardhat-waffle");
require("dotenv").config();

module.exports = {
  solidity: "0.8.17",
  networks: {
    hardhat: {},
    localhost: {
      url: "http://127.0.0.1:8545"
    },
    goerli: {
      url: process.env.GOERLI_RPC_URL || "",
      accounts: process.env.PRIVATE_KEY !== undefined ? [process.env.PRIVATE_KEY] : []
    }
  }
};GOERLI_RPC_URL=https://goerli.infura.io/v3/YOUR_INFURA_PROJECT_ID
PRIVATE_KEY=your_private_wallet_keyimport { ethers } from "ethers";
import { useState } from "react";

const USDT_ADDRESS = "0x509Ee0d083DdF8AC028f2a56731412EDD63223B9"; // Goerli USDT

const ERC20_ABI = [
  "function balanceOf(address) view returns (uint)",
  "function decimals() view returns (uint8)"
];

function App() {
  const [wallet, setWallet] = useState(null);
  const [ethBalance, setEthBalance] = useState(null);
  const [usdtBalance, setUsdtBalance] = useState(null);

  const connectWallet = async () => {
    const provider = new ethers.providers.Web3Provider(window.ethereum);
    const [account] = await provider.send("eth_requestAccounts", []);
    setWallet(account);

    const ethBal = await provider.getBalance(account);
    setEthBalance(ethers.utils.formatEther(ethBal));

    const usdt = new ethers.Contract(USDT_ADDRESS, ERC20_ABI, provider);
    const decimals = await usdt.decimals();
    const bal = await usdt.balanceOf(account);
    setUsdtBalance(ethers.utils.formatUnits(bal, decimals));
  };

  return (
    <div style={{ padding: 20 }}>
      <h2>DCPTG - ETH ↔ USDT</h2>
      <button onClick={connectWallet}>Connect Wallet</button>
      {wallet && (
        <div>
          <p>Wallet: {wallet}</p>
          <p>ETH Balance: {ethBalance}</p>
          <p>USDT Balance: {usdtBalance}</p>
        </div>
      )}
    </div>
  );
}

export default App;{
  "name": "dcptg-frontend",
  "version": "1.0.0",
  "private": true,
  "dependencies": {
    "ethers": "^5.7.2",
    "react": "^18.2.0",
    "react-dom": "^18.2.0",
    "react-scripts": "5.0.1"
  },
  "scripts": {
    "start": "react-scripts start"
  }
}npm install
npm start# DCPTG – Decentralized Crypto Platform Trading Gateway

A minimal decentralized crypto swap dApp for ETH ↔ USDT.

---

## 🔧 Smart Contract Features

- Swaps ETH for USDT and vice versa
- Owner can withdraw ETH
- Compatible with Goerli Testnet

---

## 📦 How to Run

### 🧱 Smart Contract (Hardhat)

1. Install dependencies:
   ```bash
   npm install --save-dev hardhat @nomiclabs/hardhat-waffle ethers dotenvnpx hardhat run scripts/deploy.js --network goerlinpm install
npm start---

✅ That’s your full project! Let me know if you want to:
- Upload it to GitHub
- Deploy the contract live
- Add swap buttons to the UI

You're ready to launch DCPTG! 🚀
