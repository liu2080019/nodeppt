title: flutter
speaker: 刘敏
plugins:
    - echarts

<slide class="bg-black-blue aligncenter"  image="https://source.unsplash.com/C1HhAQrbykQ/ .dark">

# flutter {.text-landing.text-shadow}

By 刘敏 {.text-intro}

[:fa-github: Github](https://github.com/ksky521/nodeppt){.button.ghost}

<slide class="bg-black-blue aligncenter">

# VUE模板渲染

- 将“模板字符串”转换成 element AST （解析器）
- 对AST进行静态节点标记，做虚拟DOM的渲染优化（优化器）
- 使用element AST生成render函数代码字符串（代码生成器）

<slide class="bg-black-blue aligncenter">

## 解析器

````
<div>
  <p>{{name}}</p>
</div>
````
“模板字符串” ↓ “element AST”
````
{
  tag: "div"
  type: 1,
  staticRoot: false,
  static: false,
  plain: true,
  parent: undefined,
  attrsList: [],
  attrsMap: {},
  children: [
      {
      tag: "p"
      type: 1,
      staticRoot: false,
      static: false,
      plain: true,
      parent: {tag: "div", ...},
      attrsList: [],
      attrsMap: {},
      children: [{
          type: 2,
          text: "{{name}}",
          static: false,
          expression: "_s(name)"
      }]
    }
  ]
}
<slide class="bg-black-blue aligncenter">
````
思路：循环截取，“<”开头
````
const ncname = '[a-zA-Z_][\\w\\-\\.]*'
const qnameCapture = `((?:${ncname}\\:)?${ncname})`
const startTagOpen = new RegExp(`^<${qnameCapture}`)
const startTagClose = /^\s*(\/?)>/
let html = `<div></div>`
let index = 0
const start = html.match(startTagOpen)

const match = {
  tagName: start[1],
  attrs: [],
  start: 0
}
html = html.substring(start[0].length)
index += start[0].length
let end, attr
while (!(end = html.match(startTagClose)) && (attr = html.match(attribute))) {
  html = html.substring(attr[0].length)
  index += attr[0].length
  match.attrs.push(attr)
}
if (end) {
  match.unarySlash = end[1]
  html = html.substring(end[0].length)
  index += end[0].length
  match.end = index
}
````
<slide class="bg-black-blue aligncenter">
栈存储，方便记录节点的层级信息，特例-自闭合标签
````
// 判断是否是自闭合标签,input、img
if (!unary) {
  currentParent = element
  stack.push(element)
}
// 建立父子关系
currentParent.children.push(element)
element.parent = currentParent
````
变量处理
````
<p>{{name}}</p>

const expression = parseText(text, delimiters) // 对变量解析 {{name}} => _s(name)
children.push({
  type: 2,
  expression,
  text
})
````
<slide class="bg-black-blue aligncenter">
## 优化器

优化器的目标是找出那些静态节点并打上标记，而静态节点指的是 DOM 不需要发生变化的节点
````
<p>我是静态节点，我不需要发生变化</p>
````
- 每次重新渲染的时候不需要为静态节点创建新节点
- 在 Virtual DOM 中 patching 的过程可以被跳过
<slide class="bg-black-blue aligncenter">
````
function isStatic (node: ASTNode): boolean {
  if (node.type === 2) { // 带变量的动态文本节点
    return false
  }
  if (node.type === 3) { // 不带变量的纯文本节点
    return true
  }
  return !!(node.pre || ( // v-pre属性 跳过编译
    !node.hasBindings && // 不能有v-.. @.. :..
    !node.if && !node.for && // 不能有v-if v-for v-else
    !isBuiltInTag(node.tag) && // 不能是slot
    isPlatformReservedTag(node.tag) && // 不能是component
    !isDirectChildOfTemplateFor(node) && // 不能是Template
    Object.keys(node).every(isStaticKey) // 不能有额外属性
  ))
}

// 所有的子节点是否是静态节点
for (let i = 0, l = node.children.length; i < l; i++) {
  const child = node.children[i]
  markStatic(child)
  if (!child.static) {
    node.static = false
  }
}
````
<slide class="bg-black-blue aligncenter">
静态根节点：当前节点是静态节点，并且有子节点，并且子节点不是单个静态文本节点这种情况会将当前节点标记为静态根节点
## 代码生成器

````
with(this){
  // _c 对应的是 createElement(tagName: String, data: Object, children: Array)
  return _c( 
    'div',
    [
      _c(
        'p',
        [
          // _v 的意思是创建一个文本节点
          // _s 是返回参数中的字符串
          _v(_s(name))
        ]
      )
    ]
  )
}
````
<slide class="bg-black-blue aligncenter">
组装data
````
function genData (el: ASTElement, state: CodegenState): string {
  let data = '{'
  // key
  if (el.key) {
    data += `key:${el.key},`
  }
  // ref
  if (el.ref) {
    data += `ref:${el.ref},`
  }
  if (el.refInFor) {
    data += `refInFor:true,`
  }
  // pre
  if (el.pre) {
    data += `pre:true,`
  }
  // ... 类似的还有很多种情况
  data = data.replace(/,$/, '') + '}'
  return data
}
````
<slide class="bg-black-blue aligncenter">
组装children
````
function genChildren (
  el: ASTElement,
  state: CodegenState
): string | void {
  const children = el.children
  if (children.length) {
    return `[${children.map(c => genNode(c, state)).join(',')}]`
  }
}

function genNode (node: ASTNode, state: CodegenState): string {
  if (node.type === 1) {
    return genElement(node, state)
  } if (node.type === 3 && node.isComment) {
    return genComment(node)
  } else {
    return genText(node)
  }
}
````
<slide class="bg-black-blue aligncenter">




