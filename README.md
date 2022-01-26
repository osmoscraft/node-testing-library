# Node Testing Library

## Get started

```
npm i -D @osmoscraft/node-testing-library
```

Create `run-test.ts` in your project root with the following content:

```typescript
import { getTests, runTests } from "@osmoscraft/node-testing-library";

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
import { describe, expect, it } from "@osmoscraft/node-testing-library";

describe("hello world", () => {
  it("should pass", async () => {
    await expect("hello").toEqual("hello");
  });
});
```

Please make sure to use `await` in frontend of each `expect` or the error reporting may not be able to surface all failed tests

## API
```TypeScript
import { describe, expect, it, beforeEach, afterEach, beforeAll, afterAll } from "@osmoscraft/node-testing-library";

describe("hello world", () => {
  beforeAll(async () => {
    // do something before all specs
  });
  beforeEach(async () => {
    // do something before each spec
  });
  it("spec name", async () => {
    // simple assertion
    await expect("hello").toEqual("hello");
    
    // simple call
    await expect(() => throw new Error()).toThrow();
    
    // negation
    await expect(true).not.toEqual(false);
    await expect(() => {}).not.toThrow();
  });
  afterEach(async () => {
    // do something after each specs
  });
  afterAll(async () => {
    // do something after all specs
  });
});
```

## TypeScript configuration

This library relies on `ts-node-dev` to compile TypeScript to JavavScript only the fly. Since the JavaScript output will be executed in a Node.js environment, your `tsconfig.json` should produce code that is compatible with Node.js. If your existing `tsconfig.json` produces code using `ESNext` module, you will need to specific a separate `tsconfig.test.json` like this:

```json
{
  "compilerOptions": {
    "target": "es2019",
    "moduleResolution": "node",
    "module": "CommonJS",
    "importsNotUsedAsValues": "error",
    "strict": true,
    "esModuleInterop": true,
    "outDir": "dist"
  }
}

```
and consume it in your `package.json` test script:
```json
  "scripts": {
    "test": "ts-node-dev --project tsconfig.test.json --quiet --transpile-only run-test.ts",
    "test:watch": "ts-node-dev --project tsconfig.test.json --respawn --watch --transpile-only run-test.ts",
  },
```
