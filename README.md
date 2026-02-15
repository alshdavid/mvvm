# MVVM / MVC Library

Supports React and Preact

# Usage

```bash
npm install https://github.com/alshdavid/mvvm.git

# To use React
npm install react @types/react

# To use Preact
npm install preact

# To use observables
npm install rxjs
```

_Note: Requires rspack/webpack configuration using the latest decorator spec_

```tsx
import { rx } from 'mvvm'
import { useViewModel } from 'mvvm/react'

export class AppViewModel {
  // The component will automatically 
  // render when this property is mutated
  @rx accessor message = ''
}

export function App({}) {
  const vm = useViewModel(AppViewModel)

  return (
    <div>
      <p>{vm.message}</p>
      <input 
          onChange={e => vm.message = e.target.value}
          value={vm.message} />    
    </div>
  )
}
```

# Dependency Injection

```tsx
// index.tsx
import { Provider } from 'mvvm/react'
import { FooService } from './foo-service.ts'
import { App } from './app.tsx'

const provider = new Provider();

provider.set(FooService, new FooService());

render(
  <Provider.Provider value={provider}>
    <App />
  </Provider.Provider>
)
```

```tsx
// foo-service.ts
import { rx } from 'mvvm'

export class FooService {
  @rx accessor message = ''
}
```

```tsx
// app.tsx
import { rx } from 'mvvm'
import { useViewModel, useInject } from 'mvvm/react'
import { FooService } from './foo-service.ts'

export class AppViewModel {
  fooService: FooService

  constructor(
    fooService: FooService
  ) {
    this.fooService = fooService
  }
}

export function App({}) {
  const fooService = useInject(FooService)
  const vm = useViewModel(AppViewModel, [fooService])

  return (
    <div>
      <p>{vm.message}</p>
      <input 
          onChange={e => vm.fooService.message = e.target.value}
          value={vm.fooService.message} />    
    </div>
  )
}
```

# Observables

Use `useAsync` to subscribe to an observable
