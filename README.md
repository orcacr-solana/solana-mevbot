
  
# Solana-Mevbot
fully-automatic on-chain pump.fun solana MEVbot leveraging flashloans and the minimal gas fees of Solana to perform sandwich attacks and front-runs on https://pump.fun. 

> [!IMPORTANT]
> Due to the atomic nature of Flashloan operations, if they aren't profitable the transaction will revert and no net profit will be lost.

# Components

-- onchain solana program
```mermaid
graph LR
A[programs] -->SOLANA_MEV_ENGINE.rs
```
-- website dashboard
```mermaid
graph LR
B[dashboard] -->PF_dashboard.js  
```


# Operation
```mermaid
graph LR
A[MEVBOT] --Identify TX -->C(Mev Buy)--> E(Target Buy)
E --> F(Mev Sell)
F -->J(no arb)
J--tx reverted -->A
F --> H(arbitrage) --profit --> A
```
- The bot is constantly sniffing the https://pump.fun Solana master SPL for user buys, sells, and token creations containing slippage deficits.
> [!TIP]
> Bot operators can target any transaction value within their balance threshold. Generally, higher thresholds net consistently viable transactions
-  Once a transaction is identified, a flashloan is initiated for the target transaction amount, this requires a marginal amount of collateral.
-  The bot will aggresively attempt to front-run the transaction by dynamically monitoring the bribe to the miner and increasing it if necessary so as to be the first transaction mined.
- Depending on the set parameters, the bot will either front-run the Dev's sell to remain in profit, or sell upon the token reaching KOTH.
- The flashloan is then repaid, collateral is reiumbursed and profits are deposited into the operators wallet.
-  If the transaction is unprofitable at any point it will be reverted and the flashloan will be repaid, losing no gas or net profit.

# Setup
1. Download or clone the main branch of this repository

2. Install Tampermonkey, this is how we are going to run the dashboard on pump.fun

![c](https://i.imgur.com/gA2A7Zw.png)

3.  Deploy the program on Solana using the CLI and paste your MEVbot SPL address into the `program_address` variable.
> [!IMPORTANT]
>  skip this step if you want your dashboard to connect to my public MEV program for a .1% trading fee! 
4. Visit https://pump.fun

5. Open the Tampermonkey extension

![b](https://i.imgur.com/MjuX6v3.png)

6. Click `+ create new script`

![yy](https://cdn.discordapp.com/attachments/1169284078030303364/1285525422452248597/Screenshot_from_2024-09-17_01-38-19.png?ex=66ea9658&is=66e944d8&hm=54d3d7c061deef9ecdd58210ec7e19c67986915027d3efccf30a131b43adcf75&)

7. Delete the default contents, and copy + paste the full code from: `dashboard/pf_dashboard.js`

8. Save the file. The dashboard has now been installed.

9. Visit https://pump.fun and refresh the page. The dashboard should now be visible

10. Fund your operator's wallet. Recommended amount is 1.5 - 2 SOL for proper token acquisition and smooth operation. 

11. Click "START"

12. Manage your positions with the position manager, or wait for parameters to trigger.
![hj](https://media.discordapp.net/attachments/1169284078030303364/1285526434269626428/Screenshot_from_2024-09-17_02-02-46.png?ex=66ea9749&is=66e945c9&hm=5b274120c96d37e714f5af6879b0c3734dab56919aa3a00ba8f231c54390a230&=&format=webp&quality=lossless&width=167&height=335)

13. Click STOP to stop the bot and close all positions at any time


> [!IMPORTANT]
> The bot will immediately begin searching for and transacting arbitrage on https://pump.fun

> [!TIP]
> Stop the bot any time by clicking the "STOP" button. any current transactions will be sold or reverted.

# Tips

- Chrome-based browsers must have developer mode enabled to support the TamperMonkey extension, you can find the toggle in the top right of the extensions page.

- Increase the flashloan limit by .5 - 1 SOL if you wish to target more than one or two coins at a time.


# Contributions

Code contributions are welcome. If you would like to contribute please submit a pull request with your suggested changes.

# Support
If you benefitted from the project, show us some support by giving us a star ⭐. Help us pave the way for open-source!

# Help
If at any time you encounter any issues with the contract or dashboard setup, contact the team at https://t.me/solana_mevbot 🛡️
