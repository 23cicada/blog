# JavaScript

- `Array.prototype.with(index, value)`: returns a new array with the element at the given index replaced, without modifying the original array.

# NPM

- [json-server](https://www.npmjs.com/package/json-server/v/0.17.4) - Fake REST AP
- [concurrently](https://www.npmjs.com/package/concurrently) - Run multiple commands concurrently

# React
### Error boundary
```jsx
import React from 'react'

class ErrorBoundary extends React.Component {
  constructor(props) {
    super(props)
    this.state = { hasError: false, error: null }
  }

  static getDerivedStateFromError(error) {
    return { hasError: true, error }
  }

  componentDidCatch(error, info) {
    console.error('ErrorBoundary caught an error', error, info)
  }

  render() {
    if (this.state.hasError) {
      return (
        <div>
          <h2>Something went wrong.</h2>
          <p>{this.state.error.message}</p>
          <button onClick={() => this.setState({ hasError: false, error: null })}>
            try again
          </button>
        </div>
      )
    }

    return this.props.children
  }
}

export default ErrorBoundary
```

### Organization of code in React application

- Grouping by features or routes
- Grouping by file type

> https://legacy.reactjs.org/docs/faq-structure.html
> https://fullstackopen.com/en/part7/miscellaneous


[Publish–subscribe pattern](https://en.wikipedia.org/wiki/Publish%E2%80%93subscribe_pattern)