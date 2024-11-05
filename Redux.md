# Redux

Содержание

1. Установка
2. Пример кода с Redux

- Установка
    
    ```jsx
    yarn add redux react-redux
    npm install redux-thunk
    
    ```
    
    в проекте вставить
    
    ```jsx
    import {createStore, applyMiddleware} from 'redux'
    import thunk from 'redux-thunk';
    ```
    
- Пример кода с Redux
    
    ```jsx
    import { createStore, compose } from 'redux';
    import { render } from 'react-dom'
    import {Provider} from 'react-redux';
    import { rootReducer } from './redux/rootReducer.js';
    
    // const store = createStore(rootReducer, applyMiddleware(thunk))
    const store = createStore(rootReducer, compose(
      window.REDUX_DEVTOOLS_EXTENSION && window.REDUX_DEVTOOLS_EXTENSION()
     ));
    
    render(
      <Provider store={store}>
        <App />
      </Provider>,
      document.getElementById('root')
    )
    ```