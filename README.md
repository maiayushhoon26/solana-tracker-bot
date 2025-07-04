# Solana Discord Trading Bot

This bot is a revolutionary trading bot designed specifically for the Solana ecosystem, bringing a new level of convenience and efficiency to crypto trading on Discord.

## Environment Variables

To run this project, you will need to add the following environment variables to your .env file

```
MONGODB_URI= "mongodb+srv:// ..."
DISCORD_TOKEN = "MTI2 ..."
DEX_TOOL_TOKEN = "W1Gn ..."
QUIKNODE_RPC = "https://example.solana.quiknode.pro/000000/"
```

## Run Locally

Clone the project

```bash
  git clone https://github.com/axioris/discord-solana-trading-bot.git
```

Go to the project directory

```bash
  cd discord-solana-trading-bot
```

Install dependencies

```bash
  npm install
```

Start the server

```bash
  npm run dev

  or

  yarn dev

```

## Features

### Private DM interactions

- /wallet show (shows your wallet)
- /wallet new (creates new wallet)
- /wallet export (shows private key)
- /wallet withdraw <enter solana wallet> <solana amount>
- /fees <allows you to change fee priority>

### Server interactions

- Monitors messages for contract addresses
- /buy <enter contract address> <solana amount>
  Example: /buy ED5nyyWEzpPPiWimP8vYm7sD7TD3LAt3Q3gRTWHzPJBY 5 sol
- /sell <enter contract address> <solana amount>
  Example: /sell ED5nyyWEzpPPiWimP8vYm7sD7TD3LAt3Q3gRTWHzPJBY 5 sol
- /portfolio (shows portfolio)

## Tech Stack

Node, Express, MongoDB, discord.js, @solana/web3
### Core Logic
```typescript
import { Client, Intents } from 'discord.js';
import { Connection, PublicKey, clusterApiUrl, Keypair, LAMPORTS_PER_SOL } from '@solana/web3.js';
import dotenv from 'dotenv';

dotenv.config();

const client = new Client({ intents: [Intents.FLAGS.GUILDS, Intents.FLAGS.GUILD_MESSAGES] });
const connection = new Connection(clusterApiUrl('mainnet-beta'));

client.once('ready', () => {
  console.log(`Logged in as ${client.user?.tag}!`);
});

client.on('messageCreate', async (message) => {
  if (message.author.bot) return;

  const args = message.content.split(' ');
  const command = args.shift()?.toLowerCase();

  if (command === '/wallet') {
    const subCommand = args.shift()?.toLowerCase();
    if (subCommand === 'show') {
      const publicKey = new PublicKey(process.env.WALLET_PUBLIC_KEY!);
      const balance = await connection.getBalance(publicKey);
      message.reply(`Your wallet balance is ${balance / LAMPORTS_PER_SOL} SOL`);
    } else if (subCommand === 'new') {
      const newWallet = Keypair.generate();
      message.reply(`New wallet created. Public Key: ${newWallet.publicKey.toBase58()}`);
    } else if (subCommand === 'export') {
      message.reply(`Your private key is ${process.env.WALLET_PRIVATE_KEY}`);
    } else if (subCommand === 'withdraw') {
      const walletAddress = args[0];
      const amount = parseFloat(args[1]) * LAMPORTS_PER_SOL;
      const transaction = await connection.requestAirdrop(new PublicKey(walletAddress), amount);
      message.reply(`Withdrawal of ${amount / LAMPORTS_PER_SOL} SOL to ${walletAddress} is successful. Transaction: ${transaction}`);
    }
  } else if (command === '/buy') {
    const contractAddress = args[0];
    const amount = parseFloat(args[1]);
    // Logic to buy using contractAddress and amount
    message.reply(`Bought ${amount} SOL of ${contractAddress}`);
  } else if (command === '/sell') {
    const contractAddress = args[0];
    const amount = parseFloat(args[1]);
    // Logic to sell using contractAddress and amount
    message.reply(`Sold ${amount} SOL of ${contractAddress}`);
  } else if (command === '/portfolio') {
    // Logic to show portfolio
    message.reply(`Your portfolio details...`);
  }
});

client.login(process.env.DISCORD_TOKEN);
```
## Feedback

For any questions or support, please open an issue or contact me.
<p align="left">
 <a href="mailto:mynameisayush2610@gmail.com"><img src="https://img.shields.io/badge/Email-D14836?style=for-the-badge&logo=gmail&logoColor=white" alt="Email" /></a>
 <a href="https://www.linkedin.com/in/kayushh/"><img src="https://img.shields.io/badge/LinkedIn-0077B5?style=for-the-badge&logo=linkedin&logoColor=white" alt="LinkedIn" /></a>
 <a href="https://t.me/maiayushhoon"><img src="https://img.shields.io/badge/Telegram-26A5E4?style=for-the-badge&logo=telegram&logoColor=white" alt="Telegram" /></a>
 <a href="https://discord.com/users/ayuvee"><img src="https://img.shields.io/badge/Discord-7289DA?style=for-the-badge&logo=discord&logoColor=white" alt="Discord" /></a>
</p>
