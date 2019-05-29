# expect-openapi

> expect objects to match OpenAPI documents

## Setup

```sh
yarn add -D @nrk/expect-openapi
# or
npm add --dev @nrk/expect-openapi
```

```ts
import { toMatchApiResponse, toMatchRef$ } from "@nrk/expect-openapi";
import expect from "expect";
expect.extend({ toMatchApiResponse, toMatchRef$ });
```

update the jest config:

```json
setupFiles: ["<rootDir>/jest.setup.ts"],
```

## Expect response to match OpenAPI path

```ts
import superagent from "superagent";
import openapi from "./openapi.json"; // OpenAPI 3 schema

describe("my api", () => {
  it("should have valid /fantastic response, according to spec", async () => {
    const url = `http://paspi.nrk.no/some/fantastic/path`;
    const response = await superagent.get(url).accept("application/json");
    // Asserts that:
    // - mime-type matches
    // - response code exists (or default)
    // - the schema matches the response body (assumes it is JSON)
    await expect(response).toMatchApiResponse(openapi, "get", "/fantastic");
  });
});
```

## testing object against a part of a openapi 3 spec

```ts
import openapi from "./openapi.json"; // OpenAPI 3 schema

describe("my complex api", () => {
  it("should match SimpleLink", async () => {
    const myLink = { href: "/my/fantastic/link" };
    // Asserts that:
    // - the reference can be resolved
    // - the resolved schema matches expected value
    await expect(myLink).toMatchRef$(
      openapi,
      "#/components/schemas/SimpleLink"
    );
  });
});
```

## Development

We use **yarn**

scripts:

- **test**
- **lint**
- **lint-fix**
