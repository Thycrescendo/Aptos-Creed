# Aptos Creed - Web 3.0 Blockchain Application
![Aptoscreed](https://i.ibb.co/DVF4tNW/image.png)

# Aptos Creed

Aptos Creed is a decentralized application (dApp) Aptos Creed, is a cutting-edge platform designed to facilitate the seamless transfer of cryptocurrencies across the globe. Still under development is going to leverage the Aptos blockchain, our application ensures fast, secure, and low-cost transactions, empowering users to send and receive crypto with ease. It features an intuitive user interface, integration with major crypto wallets, and real-time transaction tracking.

## Table of Contents
- [Introduction](#introduction)
- [Features](#features)
- [Installation](#installation)
- [Usage](#usage)
- [Smart Contract Development](#smart-contract-development)
- [Technologies Used](#technologies-used)
- [Challenges Faced](#challenges-faced)
- [What We Learned](#what-we-learned)
- [Future Plans](#future-plans)
- [Contributing](#contributing)
- [License](#license)

## Introduction

Aptos Creed aims to provide a solution for fast, secure, and affordable cryptocurrency transfers worldwide. By harnessing the power of the Aptos blockchain and the Move language, the platform offers enhanced security features and optimized transaction speeds.

## Features

- Cryptocurrency transfers often face challenges such as high transaction fees, slow processing times, and security vulnerabilities. 

- Our app is going to address these issues by utilizing the Aptos blockchain, known for its high throughput and robust security features. 

- By optimizing our app for the Aptos ecosystem, we are going to significantly reduce transaction costs and enhance the speed and security of crypto transfers.

## Installation

To set up the project locally, follow these steps:

### Prerequisites

- Node.js and npm installed on your machine.
- A GitHub account for repository access.

### Frontend

1. **Clone the repository**:
   ```bash
   git clone https://github.com/Thycrescendo/Aptos-Creed
   cd aptos-creed/frontend
   ```

2. **Install dependencies**:
   ```bash
   npm install
   ```

3. **Run the development server**:
   ```bash
   npm run dev
   ```

### Smart Contract

1. **Navigate to the smart contract directory**:
   ```bash
   cd aptos-creed/smart-contract
   ```

2. **Install dependencies**:
   ```bash
   npm install
   ```

3. **Compile and deploy the smart contract**:
   Follow the steps for deploying Move language contracts on the Aptos blockchain as per [Aptos documentation](https://aptos.dev/move/move-on-aptos/).

## Usage

1. **Start the frontend**:
   ```bash
   npm run dev
   ```

2. **Access the application**:
   Open your browser and navigate to `http://localhost:3000`.

3. **Interact with the dApp**:
   - Connect your wallet.
   - Initiate cryptocurrency transfers.
   - Monitor transaction status and history.

## Smart Contract Development

1. **Writing the Contract**:
   - The smart contracts are still uner development to be written and dependent entirely on the Move language.
   - Refer to [Move documentation](https://aptos.dev/move/move-on-aptos/) for syntax and language features.

2. **Testing the Contract**:
   - Use Hardhat for testing with the `ethers` and `ethereum-waffle` plugins.
   - Ensure all tests pass before deploying.

## Technologies Used

- **Frontend**:
  - React
  - Vite
  - Tailwind CSS
  - Framer Motion
  - Ethers.js
  - React Icons

- **Backend**:
  - Hardhat
  - Move language for smart contracts

- **Tooling**:
  - ESLint
  - GitHub for version control

## Challenges Faced

- Blockchain Integration: Adapting our existing Ethereum-based solution to the Aptos blockchain required a complete rewrite of the smart contracts using the Move language. 

- Wallet Connection: Ensuring seamless wallet integration and authentication using Aptos technologies posed a challenge due to differences in APIs and security protocols. 

- Performance Optimization: Optimizing the app to handle a high volume of transactions without compromising on speed or security was a critical challenge. 

## What We Learned

Move Language: Gained a deep understanding of the Move programming language and its benefits for smart contract development.

Aptos Ecosystem: Learned about the unique features and advantages of the Aptos blockchain, such as its high throughput and security mechanisms.

Integration Techniques: Mastered the techniques for integrating Aptos wallets and ensuring secure user authentication. Performance Optimization: Developed strategies to optimize transaction processing and reduce latency in a blockchain environment.

## Future Plans

- Everything is tactical from here, and work in progress, to enable the smart contract to be dependent on the move language and it's frameworks, and a proper deployment and Integration to Aptos. 

- Adding more features such as multi-currency support, staking, and decentralized exchange integration. 

- Continuously improving the user interface and experience based on user feedback. 

Implementing advanced security features like multi-signature wallets and biometric authentication.

## Contributing

We welcome contributions from the community! To contribute:

1. Fork the repository.
2. Create a new branch (`git checkout -b feature/your-feature`).
3. Commit your changes (`git commit -m 'Add new feature'`).
4. Push to the branch (`git push origin feature/your-feature`).
5. Open a pull request.
