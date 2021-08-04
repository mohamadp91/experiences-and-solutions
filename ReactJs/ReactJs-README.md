## mapStateToProps
First create a func `function mapStateToProps (state){ return [state]}` 
then `import {connect} from "react-redux"` and export class 
`export default connect(mapStateToProps)(className)`
then state wil be return in `this.props[0]` on changing state in Redux.
(state should be initializing)

## useEffect()
 It is a combine of `componentDidMount` and `componentDidUpdate`method.
`useEffect( () => {},[])` second argument is an array of dependencies that if they have change,
 useEffect will be called. if array be empty useEffect call only first time.

## React.memo()
`export default React.memo(funcName)` if state doesn't change the component will not be rendered.
it is such as 'shouldComponentsUpdate'.
```
const MyFunc =()=> {
 return (<div></div>)
}

React.memo(MyFunc, (props, nextProps) => {
   // if return true , the component will not be updated.    
 }
```

## React.Fragment
It is used to render to jsx adjacent.

## Context in React 
it is used to access data from all component that wrapped by provider.
(it's like redux).
first `const blogInfoContext = React.createContext(blogInfo)` blogInfo is data that you want access them.
then `const value = useContext(MyContex)` now you can access blogInfo with `value`.
