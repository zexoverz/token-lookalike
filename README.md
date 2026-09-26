# token-lookalike

Spot ERC-20 tokens pretending to be another token.

Address poisoning and fake-token airdrops rely on one thing: a token that looks like USDC in a
wallet or a block explorer. Its symbol is `USDС` with a Cyrillic `С`, or `USDC.e` on a chain that
has none, or `USD Coin` with a zero-width space, and it often sits at an address that shares the
first and last hex characters with the real one. Wallets each build their own heuristics; there is
no small library that does it.

## Planned API

```ts
import { checkToken } from "token-lookalike";

const result = checkToken(
  { address, chainId: 8453, symbol: "USDС", name: "USD Coin", decimals: 6 },
  { known: tokenList },
);
// result.impersonates: { address: "0x8335…2913", symbol: "USDC" } | null
// result.signals: [{ code: "confusable-symbol" | "invisible-char" | "name-clone" | "vanity-address" | "decimals-mismatch", detail }]
```

- Unicode confusable folding (Cyrillic, Greek, full-width, zero-width and bidi characters).
- Compares against a caller-provided token list (Uniswap token list format), by chain.
- Address similarity: shared prefix and suffix length against every known token on that chain.
- Pure and offline: no RPC calls; pass metadata in, get signals out.

Built at ETHGlobal Tokyo 2026 as a working project of the End Credits demo: the library is written in
a Claude Code session, and End Credits pays the open-source packages that session used.

## License

MIT
