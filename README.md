# TypeScript Testing Library

## Get started

```
npm i -D @osmoscraft/typescript-testing-library
```

Create `run-test.ts` in your project root with the following content:

```typescript
import { getTests, runTests } from "@osmoscraft/typescript-testing-library";

async function run() {
  const tests = await getTests("src", /\.test\.ts$/);
  runTests(tests);
}

run();
```

Update `package.json` with the following scripts to run tests

```json
  "scripts": {
    "test": "ts-node-dev --quiet --transpile-only run-test.ts",
    "test:watch": "ts-node-dev --respawn --watch --transpile-only run-test.ts",
  },

```

Write your first test in `src/hello-world.test.ts`

```typescript
import { describe, expect, it } from "@osmoscraft/typescript-testing-library";

describe("hello world", () => {
  it("should pass", async () => {
    await expect("hello").toEqual("hello");
  });
});
```

Please make sure to use `await` in frontend of each `expect` or the error reporting may not be able to surface all failed tests
