# Testing React apps

```bash
npm install --save-dev vitest jsdom

npm install --save-dev @testing-library/react @testing-library/dom

npm install --save-dev jsdom
```

Auto Cleanup:

```js
export default defineConfig({
  plugins: [react()],
  test: {
    environment: 'jsdom',
    globals: true,
    setupFiles: ['./vitest-cleanup-after-each.js']
  }
})
```

```js
import {cleanup} from '@testing-library/react'
import {afterEach} from 'vitest'

afterEach(() => {
  cleanup()
})
```

`environment`: The default environment in Vitest is a Node.js environment. If you are building a web application, you can use browser-like environment through either jsdom or happy-dom instead.

`globals`: With `globals: true`, there is no need to import keywords such as describe, test and expect into the tests. Note that some libraries, e.g., `@testing-library/react`, rely on globals being present to perform auto cleanup.

`setupFiles`: They will run before each test file in the same process.

> [Auto Cleanup in Vitest](https://testing-library.com/docs/react-testing-library/setup#auto-cleanup-in-vitest)