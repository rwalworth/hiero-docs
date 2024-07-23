---
description: The web3 API provides Java-based REST APIs for the mirror node.
---

# Web3 API

## Technologies

This module uses [Spring Boot](https://spring.io/projects/spring-boot) for its application framework. To serve the APIs, [Spring WebFlux](https://docs.spring.io/spring-framework/docs/current/reference/html/web-reactive.html) is used with annotation-based controllers. [Spring Data JPA](https://spring.io/projects/spring-data-jpa) with Hibernate is used for the persistence layer.

***

## Supported Operations

<table data-full-width="false"><thead><tr><th width="109" align="center">Estimate</th><th width="69" align="center">Static</th><th width="231">Operation Type</th><th width="144" align="center">Supported, Historical, Reads</th><th align="center">Modifications</th></tr></thead><tbody><tr><td align="center">Y</td><td align="center">Y</td><td>non precompile functions</td><td align="center">Y</td><td align="center">Y</td></tr><tr><td align="center">Y</td><td align="center">N</td><td>non precompile functions with lazy account creation</td><td align="center">Y</td><td align="center">Y</td></tr><tr><td align="center">Y</td><td align="center">Y</td><td>operations for ERC precompile functions (balance, symbol, tokenURI, name, decimals, etc.)</td><td align="center">Y</td><td align="center">N</td></tr><tr><td align="center">Y</td><td align="center">Y</td><td>read-only ERC precompile functions</td><td align="center">Y</td><td align="center">N</td></tr><tr><td align="center">Y</td><td align="center">Y</td><td>modifying ERC precompile functions</td><td align="center">Y</td><td align="center">Y</td></tr><tr><td align="center">Y</td><td align="center">Y</td><td>read-only operations for HTS system contract</td><td align="center">Y</td><td align="center">N</td></tr><tr><td align="center">Y</td><td align="center">N</td><td>modifying operations for HTS system contract</td><td align="center">Y</td><td align="center">Y</td></tr></tbody></table>

{% hint style="info" %}
_**Note**:_ Gas estimation only supports the `latest` block.
{% endhint %}

***

## Acceptance Tests

The [acceptance tests](https://github.com/hashgraph/hedera-mirror-node/blob/main/hedera-mirror-test/README.md) contain a suite of tests to validate a web3 deployment. The `@web3` acceptance tag can be used to specifically target the web3 module.

`./gradlew :test:acceptance --info -Dcucumber.filter.tags=@web3`

***

## Smoke Tests

The Web3 API uses [Postman](https://www.postman.com/) tests to verify proper operation. The [Newman](https://learning.postman.com/docs/running-collections/using-newman-cli/command-line-integration-with-newman) command-line collection runner is used to execute the tests against a remote server. To use newman, either the executable binary or Docker approach can be used. With either approach, a `baseUrl` variable can be supplied to customize the target server.

To run the Postman tests, first ensure newman is installed locally using `npm`, then execute `newman`.

```bash
npm install -g newman
newman run charts/hedera-mirror-web3/postman.json --env-var baseUrl=https://previewnet.mirrornode.hedera.com
```

Alternatively, Docker can be used execute the smoke tests:

```bash
docker run --rm -v "${PWD}/charts/hedera-mirror-web3/postman.json:/tmp/postman.json" -t postman/newman run /tmp/postman.json --env-var baseUrl=https://previewnet.mirrornode.hedera.com
```

{% hint style="info" %}
_**Note**:_ To test against an instance running on the same machine as Docker use your local IP instead of 127.0.0.1.
{% endhint %}

***

## Manual Tests

Any REST client can be used to manually invoke the contract call API. In the below example, curl is used to simulate a call to the `tinycentsToTinybars(uint256)` function in the exchange rate system contract with 100 cents as input.

```
curl -X 'POST' https://testnet.mirrornode.hedera.com/api/v1/contracts/call -H 'Content-Type: application/json' -d \
'{
  "block": "latest",
  "data": "0x2e3cff6a0000000000000000000000000000000000000000000000000000000000000064",
  "estimate": false,
  "gas": 15000000,
  "gasPrice": 100000000,
  "to": "0x0000000000000000000000000000000000000168"
}'
```

