## What is React?
- React is a JavaScript library for building user interfaces, particularly single-page applications. Developed and maintained by Facebook (now Meta), React was first released in 2013 and has become one of the most popular frontend development libraries.

### Key Features of React
- Component-based architecture: UI is broken down into reusable, self-contained components
- Virtual DOM: Efficiently updates the browser DOM by only rendering changed components
- JSX: Syntax extension that allows you to write HTML-like code in JavaScript
- Unidirectional data flow: Data flows in one direction, making the code more predictable
### Why React is Used
- React is widely adopted for several reasons:
    - Performance: The virtual DOM provides efficient rendering and updates
    - Reusability: Components can be reused throughout applications, saving development time
    - Developer experience: React's declarative approach makes UI code more predictable and easier to debug
    - Strong ecosystem: Large community with extensive libraries, tools, and extensions
    - Cross-platform development: Can be used for web (React), mobile (React Native), and desktop applications
    - Industry adoption: Used by many major companies like Facebook, Instagram, Netflix, Airbnb, and more

## What is Component-Based Architecture?
- Component-based architecture is a design pattern where UIs are broken down into smaller, reusable pieces called components. Each component is:
    - Self-contained
    - Reusable
    - Independent
    - Maintains its own state and behavior
### Key Concepts
- Components
    - Definition: Building Blocks of UI
    - Types
        - classComponent
        - FunctionalComponents with Hooks
    ```js
        // Basic functional component
        function Welcome(props) {
          return <h1>Hello, {props.name}</h1>;
        }

        // Using the component
        const element = <Welcome name="John" />;
    ```    
2. Component Hierarchy
- Components can be nested to create complex UIs
```js
    function App() {
      return (
        <div>
          <Header />
          <MainContent>
            <Sidebar />
            <ArticleList />
          </MainContent>
          <Footer />
        </div>
      );
    }
```
### props & state
- Props: Input parameters passed to components
- State: Internal component data that can change over time
```js
    function UserCard({ name, email }) {  // Props
      const [isExpanded, setIsExpanded] = useState(false);  // State

      return (
        <div>
          <h2>{name}</h2>
          {isExpanded && <p>{email}</p>}
          <button onClick={() => setIsExpanded(!isExpanded)}>
            {isExpanded ? 'Show Less' : 'Show More'}
          </button>
        </div>
      );
    }
```
### Benefits
- Reusability: Components can be reused across different parts of an application
- Maintainability: Easier to update and debug isolated components
- Separation of Concerns: Each component handles specific functionality
- Testing: Components can be tested in isolation
- Scalability: Easy to add new features by creating new components

## React's Virtual DOM Explained
### What is Virtual DOM?
- The Virtual DOM is a lightweight copy of the actual DOM (Document Object Model) that React keeps in memory. It's a programming concept where an ideal, or "virtual", representation of a UI is kept in memory and synced with the "real" DOM by a library such as ReactDOM.
- The Virtual DOM is one of React's key features that makes it efficient and popular for building modern web applications.

### How Virtual DOM Works
- Initial Render
    - React creates a virtual DOM tree
    - Renders actual DOM based on virtual DOM
- State/Props Update
    ```js
        function Example() {
          const [count, setCount] = useState(0);

          return (
            <div>
              <p>Count: {count}</p>
              <button onClick={() => setCount(count + 1)}>
                Increment
              </button>
            </div>
          );
        }
    ```
- Diffing Process
    - creates new virtua DOM tree
    - Compares(diffs) with previous virtual DOM
    - Identifies minimum changes needed
### Benefits
1. Performance Optimization
    - Reduces actual DOM manipulation
    - Batches multiple changes
    - Updates only what's necessary
2. Cross-Platform Capability
    - Abstracts DOM manipulation
    - Enables React Native development
    - Works with different rendering targets
3. Developer Experience
    - Declarative programming model
    - Predictable state updates
    - Easier debugging
4. Efficiency
    - Minimizes expensive DOM operations
    - Reduces browser reflows/repaints
    - Optimizes rendering cycles

Example:
```js
function TodoList() {
  const [todos, setTodos] = useState(['Task 1', 'Task 2']);

  const addTodo = () => {
    // React will efficiently update only the changed parts
    setTodos([...todos, 'New Task']);
  };

  return (
    <ul>
      {todos.map((todo, index) => (
        <li key={index}>{todo}</li>
      ))}
    </ul>
  );
}
```