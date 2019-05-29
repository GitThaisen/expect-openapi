# expect-openapi

## jest.setup.ts

```ts
import { toMatchApiResponse, toMatchRef$ } from "@nrk/expect-openapi";
import expect from "expect";
expect.extend({ toMatchApiResponse, toMatchRef$ });
```

add setupfile to jest config

```json
setupFiles: ["<rootDir>/jest.setup.ts"],
```

## testing openapi spec against a server

```ts
import superagent from "superagent";
// 1. import the openapi spec
import openapi from "../public/documentation/openapi/fantastic/openapi.json";

describe("my api", () => {
  it("should have valid /fantastic response, according to spec", async () => {
    const url = `http://paspi.nrk.no/some/fantastic/path`;
    // 2. Make a request to the API endpoint
    const response = await superagent.get(url).accept("application/json");
    // 3. Expect that the response is valid according to spec
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
// 1. import the openapi spec
import openapi from "../public/documentation/openapi/fantastic/openapi.json";

describe("my complex api", () => {
  it("should match SimpleLink", async () => {
    const myLink = { href: "/my/fantastic/link" };
    // 2. expect that object matches the reference
    // Asserts that:
    // - the reference can be resolved
    // - the resolved schema matches expected value
    await expect(myLink).toMatchRef$(
      openapi, // openapi3 schema
      "#/components/schemas/SimpleLink" // ref$ to type in schema
    );
  });
});
```