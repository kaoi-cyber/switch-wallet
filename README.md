# Switch Wallet

**Pay Thai PromptPay QR codes with USDT.** A mobile-style web demo: connect your crypto wallet, scan a merchant's PromptPay QR, see the THB amount converted to USDT at the live rate, and walk through the payment flow.

**Live demo:** https://kaoi-cyber.github.io/switch-wallet/

> ⚠️ **Demo only.** Wallet connections and balances are real, but **payments are simulated**. No transaction is signed, no USDT leaves your wallet, and no merchant receives THB.

---

## Features

- **Connect any wallet**
  - Searchable list of 40 popular wallets: MetaMask, Trust Wallet, Edge, Coinbase Wallet, Phantom, Rainbow, OKX, Binance, Ledger Live and more.
  - Wallet extensions installed in your browser are detected automatically and connect in one tap.
  - Phone wallets (like Edge) connect by scanning a WalletConnect QR code. "Search 500+ wallets" opens the full WalletConnect list.
  - Sessions are remembered, so your wallet reconnects when you reload.
- **Real USDT balance:** read live from Ethereum, Polygon, BNB Chain, Arbitrum or Optimism, shown in USDT and THB.
- **PromptPay QR scanning**
  - Uses the phone or laptop camera.
  - Upload a screenshot of a QR instead.
  - Paste the raw QR text as a fallback.
  - Two built-in sample QRs for demos without a real one.
- **Local QR decoding:** follows the EMVCo / Thai QR Payment standard.
  - Merchant ID from Tag 29: mobile number, National/Tax ID, e-wallet or bank account.
  - Bill-payment QRs from Tag 30.
  - Amount from Tag 54. If the QR has no amount, you type it in.
  - Rejects non-THB currencies and checks the CRC-16 checksum (Tag 63).
  - Clear message when a QR isn't a PromptPay code.
- **Live USDT/THB rate:** from CoinGecko, refreshed every minute. Falls back to 33.50 if unavailable.
- **Slide to Pay:** a payment flow with progress steps and a receipt.
- **Demo wallet:** 250 test USDT, for presenting without a real wallet.
- **Thai-inspired design:** flag colours, temple gold and a chedi motif. Works in a phone frame on desktop and full screen on mobile.

## Getting started

### 1. Open the app

Use the live demo link above, or run it locally (see below).

### 2. Get a free WalletConnect Project ID (one time)

Phone wallets like Edge connect through WalletConnect, which needs a free Project ID from Reown:

1. Sign up at [cloud.reown.com](https://cloud.reown.com).
2. Create a project and choose **AppKit**.
3. Copy the **Project ID** (32 characters).
4. Under the project's allowed domains, add the address you use the app from:
   - `https://kaoi-cyber.github.io` for the live demo
   - `http://localhost:8000` for local use

Browser-extension wallets (MetaMask, Rabby, …) connect without a Project ID.

### 3. Connect your wallet

1. Tap ⚙️ (top right). Paste your Project ID into **Reown (WalletConnect) Project ID** and tap **Save**.
   - The ID is stored only in your browser, so enter it once on each device.
2. Tap **Connect Wallet** and pick your wallet.
3. For a phone wallet (e.g. Edge), open the wallet app, tap its scan button, scan the QR code and approve.

### 4. Try a payment

Tap **Scan PromptPay QR**, then do one of these:
- scan a code with the camera
- upload a screenshot
- tap a sample button

Review the details and slide to pay.

## Running locally

The whole app is one file, `index.html`. Serve it over `localhost` so the browser allows camera access:

```bash
python3 -m http.server 8000
```

Then open http://localhost:8000.

Opening the file directly (`file://`) mostly works, but browsers often block the camera there.

## Supported networks

| Network   | USDT contract                                |
|-----------|----------------------------------------------|
| Ethereum  | `0xdAC17F958D2ee523a2206206994597C13D831ec7` |
| Polygon   | `0xc2132D05D31c914a87C6611C10748AEb04B58e8F` |
| BNB Chain | `0x55d398326f99059fF775485246999027B3197955` |
| Arbitrum  | `0xFd086bC7CD5C481DCC9C85ebE478A1C0b69FCbb9` |
| Optimism  | `0x94b008aA00579c1307B0EF2c499aD98a61f58e58` |

USDT on other networks (e.g. Tron, Solana) isn't shown.

## Tech

- Single self-contained HTML file. No build step.
- [Tailwind CSS](https://tailwindcss.com) (Play CDN) for styling.
- [Reown AppKit](https://reown.com/appkit) for WalletConnect and wallet connections.
- [EIP-6963](https://eips.ethereum.org/EIPS/eip-6963) to detect installed browser wallets.
- [html5-qrcode](https://github.com/mebjas/html5-qrcode) for camera and image QR scanning.
- [CoinGecko API](https://www.coingecko.com/en/api) for the USDT/THB rate.
- Optional: [Privy](https://privy.io) as an alternative wallet-login provider (Privy App ID in Settings).

## Troubleshooting

| Problem | Fix |
|---|---|
| Camera won't start | Allow camera access for the site, and use `https://` or `localhost`. On macOS, also allow your browser under System Settings → Privacy & Security → Camera. |
| QR scans but nothing happens / yellow warning | The code isn't a PromptPay QR. Use a Thai bank app's "receive money" QR. |
| Wallet list is empty or connection fails | Check the Project ID in Settings, and that your site's address is in the Reown project's allowed domains. |
| Balance shows 0.00 | Your USDT is on a network not listed above, or the wallet is on an unsupported network. Tap the connected-wallet button to switch networks. |
| Old version still showing | Hard-reload: **Cmd + Option + R** (Safari) or **Cmd + Shift + R** (Chrome). |

## Status and limitations

This is a product prototype, not a payment service:

- **No funds move.** Payments are simulated end to end.
- **No exchange partner is connected,** so nothing converts USDT to THB or settles to a merchant's PromptPay.
- **Not compliance-reviewed.** In Thailand, using digital assets to pay for goods and services is restricted for licensed digital-asset businesses. A production version would need Thai legal review and licensed settlement partners.
