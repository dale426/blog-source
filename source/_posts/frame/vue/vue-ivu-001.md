---
title: iview使用总结（二）
tags: [iview组件]
categories: [vue框架]
date: 2018-12-13 17:15:14
---
摘要: iview表单校验、vue中实现hover效果、vue Render方法使用姿势
<!-- more -->
### iview表单校验
**1. iview在校验select报错**    
***
 问题：即使选择了某一项一直报错？;
 原因: iview默认校验数据类型为**String**, 而我们再给select的 :value是`number`类型的;

**解决方法:** 加入 type: 'number'

```js

industryType: [
    { required: true, message: "请选择", trigger: "change", type: 'number' }
]

```
**2. 多条件校验， 自定义正则校验**
```js

 contractPhone: [
    { required: true, message: "请输入联系电话", trigger: "blur" },
    { type: 'string',pattern: /^1\d{10}$/, message: '联系电话格式有误', trigger: "blur" } // 使用正则表达式
]

```
**3. 校验日期，或者城市选择器**
 问题： iview默认校验的数据类型是 String，所以用默认校验，type是不符合的。
 解决： type：data
```js
province:[
    { required: true, message: '预送达时间不能为空', trigger: 'change' ,type: 'date'},
],
```
***
### vue中实现hover效果

```html
<div
    class="img-wrap-item"
    @mouseenter="showCloseIcon(item)"
    @mouseleave="showCloseIcon(item)"
</div>
```

### iview select  remote模式下，无法清空搜索输入的内容
当使用remote模式下的select弹窗时， 关闭再显示，无法清空其内容

```html
<Select
    class="change-select"
    v-model="concatCustomer"
    filterable
    remote
    :remote-method="queryCustomerList"
    :loading="customer_loading"
    placeholder="请输入客户名称"
>
```
解决方法： 
`        document.querySelector('.change-select .ivu-select-input').value = ''`


### vue Render方法使用姿势
iview框架中使用render函数方法：`render:(h,params)=>{}`
> render参数
```js
render:(h, params) => {
  return h(" 定义的元素 "，{ 元素的性质 }，" 元素的内容"/[元素的内容])
}
```
example
```js
render: (h, params) => {
    return h('p',
        {
            props: { // 组件peops
            },
            style: { // style样式属性
                height: '60px',
                border: "1px solid lightblue"
            },
            domProps: { // dom原生属性
                href: 'javascript:void(0);',
            },
            on: { // 绑定 vue事件
            },
            nativeOn: { // 绑定原生事件
                click: value => {
                    this.$Message.success('点击事件成功啦')
                },
                keydown: event => {
                    console.log('event.target.value', event.target.value);
                }
            }
        },
        [h('span', {
            style: {
                background: 'lightgreen'
            }
        }, 55),
        [h('a',{
            style: {
                border: '1px solid red'
            }
        }, '我是a标签')]
        ], )
    }
```



### 完整示例如下
html -> template
```html
    <Form :model="formItem" ref="formItem" :rules="formValidate" inline :label-width="84"> <!-- label-width作用于 所有的子formitem  inline行内模式 -->
        <FormItem label="联系电话" prop="contractPhone">
              <Input
                v-model="formItem.contractPhone"
                style="width: 280px;"
                placeholder="请输入联系电话"
              />
        </FormItem>
        <FormItem label="公司所在地" prop="provinceArr">  <!-- 此处需要用 prop 名字要与v-model的 一致 -->
              <Cascader
                v-model="formItem.provinceArr"
                :data="provinceList"
                trigger="hover"
              ></Cascader>
        </FormItem>
        <FormItem label="请选择" prop="select">
              <Select
                v-model="formItem.industryType"
              >
                <Option
                  v-for="item in categoryList"
                  :key="item.value"
                  :value="item.value"
                >{{ item.label }}</Option>
              </Select>
        </FormItem>
    </Form>
```
data中的校验：
```js
    data() {
        return {
            formValidate: {
                // 多重校验
                contractPhone: [
                    { required: true, message: "请输入联系电话", trigger: "blur" },
                    { pattern: /^1\d{10}$/, message: '联系电话格式有误', trigger: "blur" } // 使用正则表达式
                ],
                industryType: [
                    { required: true, message: "请选择行业类别", trigger: "change",  type:'number'}
                ],
                // 自定义校验  地址控件校验
                provinceArr: [
                    {
                        validator: this.proviinceValidate, // 调用自定义方法 支持异步，比如查重
                        required: true,
                        trigger: "change"
                    }
                ]
            }
        }
    },
    method: {
        // 异步或者自定义校验方法
        proviinceValidate(rule, value, callback) {
            if (Array.isArray(value) && value.length === 2) {
                return callback();
            } else {
                return callback(new Error("地址不能为空"));
            }
        },
        // 表单提交 验证方法
        submit() {
            this.$refs['formItem'].validate( async (valid) => {  // 如果有await异步操作 需要async
                if (valid) {
                    // 校验通过
                }
        }),
        // 清空方法
        clear() {
            this.$refs['formItem'].resetFields()
        }

    }
      
```