

```
1. Explain what the simple List component does.

-> This List Component is used to display an unordered List with some texts as List items. WrappedListComponent creates an unordered list. It takes an array (items) of strings(text) as props. For each item in the array, it maps through all and displays their content on the screen with a background color of 'Red'. If one item is selected, then it displays it with a different background color (Green). The user can click on an item and it will get selected and will get background color of Green.

2. What problems/warnings are there with the code?

-> onClick events should have a function reference instead of a function call.
```

> ```<li style={{ backgroundColor: isSelected ? "green" : "red" }}```
>
> &emsp;&emsp; ``` onClick={onClickHandler(index)}>```
>
>&emsp;&emsp;&nbsp; ```  {text}```
>
> ```</li> ```

```
-> useState variables being misplaced.
```

>``` const [setSelectedIndex, selectedIndex] = useState();```

-> Passing a number selectedIndex to isSelected which should be a bool.

>```<ul style={{ textAlign: "left" }}>```
>
> &emsp;&emsp; ```     {items.map((item, index) => (```
>
> &emsp;&emsp; ```      <SingleListItem```
>
>&emsp;&emsp;&emsp;  ```        onClickHandler={() => handleClick(index)}```
>
> &emsp;&emsp;&emsp; ```        text={item.text}```
>
> &emsp;&emsp;&emsp; ```        index={index}```
>
>&emsp;&emsp;&emsp;  ```        isSelected={selectedIndex}```
>
> &emsp;&emsp; ```      />```
>
> &emsp;&emsp; ```   ))}```
>
>```</ul>```

-> Defining a default prop as null is not recommended.

>```WrappedListComponent.defaultProps = {```
>
>&emsp;&emsp;  ```items: null,```
>
>```};```
>
-> Unique key prop is missing for List items.

>```<ul style={{ textAlign: 'left' }}> ```
>
>&emsp;&emsp;  ```{items.map((item, index) => (```
>
> &emsp;&emsp;&emsp; ```<SingleListItem```
>
>&emsp;&emsp;&emsp;&emsp;&emsp;   ```onClickHandler={() => handleClick(index)}```
>
>&emsp;&emsp;&emsp;&emsp;&emsp;   ```text={item.text}```
>
>&emsp;&emsp;&emsp;&emsp;&emsp;   ```index={index}```
>
> &emsp;&emsp;&emsp;&emsp;&emsp;  ```isSelected={selectedIndex}```
>
> &emsp;&emsp;&emsp;   ```/>```
>
> &emsp;&emsp; ```))}```
>
> ```</ul>```

-> Syntax errors in the following code


>```WrappedListComponent.propTypes = {```
>
>&emsp;&emsp;```  items: PropTypes.array(```
>
>&emsp;&emsp;&emsp;```    PropTypes.shapeOf({```
>
>&emsp;&emsp;&emsp;&emsp;```      text: PropTypes.string.isRequired,```
>
>&emsp;&emsp;&emsp;```    })```
>
>&emsp;&emsp;```  ),```
>
>```};```


3. Please fix, optimize, and/or modify the component as much as you think is necessary.


>import React, { useState, useEffect, memo } from 'react';</br>
>import PropTypes from 'prop-types';</br>
>
>// Single List Item</br>
>const WrappedSingleListItem = ({</br>
>  &emsp;&emsp;  index,</br>
>  &emsp;&emsp;  isSelected,</br>
>  &emsp;&emsp;  onClickHandler,</br>
>  &emsp;&emsp;  text,</br>
>}) => {</br>
> &emsp;&emsp;   return (</br>
> &emsp;&emsp;&emsp;&emsp;       <li</br>
>    &emsp;&emsp;&emsp;&emsp;&emsp;&emsp;        style={{ backgroundColor: isSelected ? 'green' : 'red' }}</br>
>     &emsp;&emsp;&emsp;&emsp;&emsp;&emsp;       // onClick={onClickHandler(index)} //There should be a function reference instead of call</br>
> &emsp;&emsp;&emsp;&emsp;&emsp;&emsp;           onClick={() => onClickHandler(index)}</br>
>  &emsp;&emsp;&emsp;&emsp;      ></br>
>       &emsp;&emsp;&emsp;&emsp;&emsp;&emsp;     {text}</br>
>&emsp;&emsp;&emsp;&emsp;        </li></br>
>  &emsp;&emsp;  );</br>
>};</br>
</br>
>WrappedSingleListItem.propTypes = {</br>
>&emsp;&emsp;    index: PropTypes.number,</br>
>&emsp;&emsp;    isSelected: PropTypes.bool,</br>
>&emsp;&emsp;    onClickHandler: PropTypes.func.isRequired,</br>
>&emsp;&emsp;    text: PropTypes.string.isRequired,</br>
>};</br>
>
>const SingleListItem = memo(WrappedSingleListItem);</br>
>
>// List Component</br>
>const WrappedListComponent = ({ items, }) => {</br>
>
>&emsp;&emsp;&emsp;&emsp;    // const [setSelectedIndex, selectedIndex] = useState();the place of selectedIndex & setSelectedIndex should get interchanged.</br>
>&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;    const [selectedIndex, setSelectedIndex] = useState();</br>
>
>&emsp;&emsp;    useEffect(() => {</br>
>&emsp;&emsp; &emsp;&emsp;       setSelectedIndex(null);</br>
>&emsp;&emsp;    }, [items]);</br>
>
>&emsp;&emsp;    const handleClick = index => {</br>
> &emsp;&emsp;&emsp;&emsp;       setSelectedIndex(index);</br>
>&emsp;&emsp;    };</br>
>
>&emsp;&emsp;    return (</br>
>&emsp;&emsp;   &emsp;&emsp;     <ul style={{ textAlign: 'left' }}></br>
> &emsp;&emsp;  &emsp;&emsp;&emsp;&emsp;         {items.map((item, index) => (</br>
> &emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;               <SingleListItem</br>
>&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;                    key={index}</br>
>&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;                    onClickHandler={() => handleClick(index)}</br>
>&emsp;&emsp; &emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;                   text={item.text}</br>
>&emsp;&emsp; &emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;                   index={index}</br>
>&emsp;&emsp; &emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;                   // isSelected={selectedIndex}</br>
>&emsp;&emsp; &emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;                   isSelected={Boolean(selectedIndex)}</br>
>&emsp;&emsp; &emsp;&emsp;&emsp;&emsp;&emsp;&emsp;               /></br>
>&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;            ))}</br>
>&emsp;&emsp; &emsp;&emsp; </ul> </br>
>&emsp;&emsp;&emsp;&emsp;    )</br>
>};</br>
>
>// WrappedListComponent.propTypes = {</br>
>//&emsp;&emsp;     items: PropTypes.array(PropTypes.shapeOf({</br>
>// &emsp;&emsp;&emsp;&emsp;        text: PropTypes.string.isRequired,</br>
>//&emsp;&emsp;     })),</br>
>// };</br>
>
>//The above code contains some errors, The modified code is shown below</br>
>
>WrappedListComponent.propTypes = {</br>
>&emsp;&emsp;    items: PropTypes.arrayOf(PropTypes.shape({</br>
> &emsp;&emsp;&emsp;&emsp;       text: PropTypes.string.isRequired,</br>
>&emsp;&emsp;    }).isRequired</br>
>&emsp;&emsp;    ).isRequired</br>
>};</br>
>
>WrappedListComponent.defaultProps = {</br>
>&emsp;&emsp;    // items: null //giving null as default prop is never recommended</br>
>&emsp;&emsp;    items: undefined </br>
>};</br>
>
>const List = memo(WrappedListComponent);</br>
>
>export default List;</br>



