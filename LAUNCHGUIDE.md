# Grandmaster MCP

## Tagline
16 Shopify commerce tools for MCP agents, from product discovery to checkout.

## Description
Grandmaster MCP is a remote MCP server running on a Cloudflare Worker. It exposes 16 tools for Shopify product discovery, merchant storefront catalogs, carts, checkouts, and store policies and FAQs. Global catalog tools call Shopify's global catalog; merchant-specific tools forward requests to the domain supplied for that merchant. The server passes the upstream response back to the MCP client.

The tools are grouped into global catalog (3), storefront catalog (3), cart (4), checkout (5), and policies/FAQs (1). Cart and checkout actions operate on the merchant specified in each call. Completing a checkout can place an order and requires the buyer's authorization.

## Setup Requirements
- Remote MCP endpoint: Connect your MCP client to `https://grand-master-mcp.anigok.com/mcp`.
- Merchant-specific catalog, cart, and checkout calls: Supply `shop_domain` with the domain of the merchant you want to use. The server forwards the call to `https://<shop_domain>/api/ucp/mcp`. The domain is not limited to `myshopify.com`; it must be a domain that actually serves that merchant's UCP MCP endpoint. Global catalog tools do not use `shop_domain`.
- UCP agent profile: Merchant-specific catalog, cart, and checkout calls require `_meta["ucp-agent"].profile`, a URL for the calling agent's UCP profile. You can use your own profile or this project-provided profile: `https://ucp-agent-profile.facetimefy.com/ucp/agent-profiles/2026-08-25/valid-with-capabilities.json`. The global catalog tools accept this metadata but do not require it. Supply the URL in the tool arguments; it is not a server environment variable.
- Store policies and FAQs: Supply `store_domain` to `search_shop_policies_and_faqs`. The server forwards the call to `https://<store_domain>/api/mcp`.
- Idempotency: `cancel_cart`, `complete_checkout`, and `cancel_checkout` require a UUID in `_meta["idempotency-key"]`.
- Server-side configuration: The provided server code does not read environment variables or require users to install an npm or PyPI package.

## Category
Business Tools

## Use Cases
Shopping assistants, Product discovery, Storefront search, Cart management, Checkout workflows, Store policy answers

## Features
- Search Shopify's global catalog and retrieve product or variant details.
- Search and inspect products from a selected merchant's storefront.
- Create, retrieve, update, and cancel merchant carts.
- Create, retrieve, update, complete, and cancel merchant checkouts.
- Search a selected store's policies and FAQs.

## Getting Started
1. Add `https://grand-master-mcp.anigok.com/mcp` as a remote MCP server in your client.
2. For global discovery, ask: "Search the global Shopify catalog for trail running shoes." This uses `global_search_catalog` and does not require a merchant domain.
3. For a particular store, provide its `shop_domain` and a UCP agent profile URL, then ask: "Search this store for a blue jacket." This uses `search_catalog`. If you do not have your own profile, use `https://ucp-agent-profile.facetimefy.com/ucp/agent-profiles/2026-08-25/valid-with-capabilities.json` for `_meta["ucp-agent"].profile`.
4. Review cart and checkout results before changing them. Use `complete_checkout` only after the buyer authorizes the purchase and payment.

## Tags
shopify, mcp, ucp, ecommerce, commerce, shopping, products, catalog, cart, checkout, cloudflare-workers

## Documentation URL
https://github.com/Tattzy25/grandmaster-mcp
