# jsx 语法

### 1. 在 jsx 中使用变量

```jsx
import React from 'react';
import ReactDOM from 'react-dom'

let name = "javaliu"

// jsx 语法一般使用小括号括起来，使用大括号的方式引用变量 
let jsx = (
	<div>
		I am {name}
	</div>
)

ReactDOM.render(
    jsx,
    document.getElementById('app')
);
```

### 2. 在 jsx 中使用样式

```jsx
import React from 'react';
import ReactDOM from 'react-dom'

let myStyle = {
	color: 'red',       // 此处的值可以通过计算得来，color:'r' + 'ed'
	fontSize: '30px',   // 注意 fontSize 的写法，如果将其移到 css 文件中，则正常写，font-size
}

// 如果是行内样式也可以这样写：<div style={{color:'red', fontSize:'30px'}}>...</div>
// 在 jsx 中除了纯字符串可以使用 属性="字符串值" 之外，其他均使用大括号，如 key={2}
let jsx = (
	<div style={myStyle}>
		I am javaliu
	</div>
)

ReactDOM.render(
    jsx,
    document.getElementById('app')
);
```

### 3. 在 jsx 中条件判断

```jsx
import React from 'react';
import ReactDOM from 'react-dom'

let flag = true;

let jsx = (
	<div>
		{
			flag ? <p>I am javaliu</p> : <p>I am not javaliu</p>
		}
	</div>
)

ReactDOM.render(
    jsx,
    document.getElementById('app')
);
```

### 4. 在 jsx 中遍历数组

```jsx
import React from 'react';
import ReactDOM from 'react-dom'

let names = ['Rosen', 'Geely', 'Jimin']

let jsx = (
	<div>
		{
			names.map((name, index) => 
				<p key={index}>Hello {name}</p>
			)
		}
	</div>
)

ReactDOM.render(
    jsx,
    document.getElementById('app')
);
```
