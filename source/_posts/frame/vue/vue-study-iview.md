---
title: iview组件使用的踩坑记录
tags: [iview组件]
categories: [vue框架]
date: 2017-11-08 16:03:39
---
级联选择器的校验， 导航Tabs的使用
<!-- more -->
## 级联选择起的校验
<div style="color: #808080; font-size: 12px; text-align: right">2017-11-08 16:03:39 </div>

### html

```html
 <Form :label-width="100" ref="formData" :rules="ruleValidate" :model="formData" inline class="wms-form-check oneline">   

    <Form-item label="收货地址：" style="width:532px" prop="region" `@注意region`>
        <Cascader :data="provinceCity" v-model="formData.region" `@注意region` size="small" trigger="hover"></Cascader>
    </Form-item>

<Form>
```
### javascript

```js
import wmsValidate from 'wmsValidate
data() {
    return {
        formData: {
            region: []
        },
        ruleValidate: {
            ownerName: [{required: true, message: '货主名称不能为空', trigger: 'change'}],
            type: [{required: true, message: '单据类型不能为空', trigger: 'change'}],

            contactsName: [{required: true, message: '联系人不能为空', trigger: 'blur'}],
            contacts: [{required: true, message: '联系方式不能为空', trigger: 'blur'}],
            
            region: [ --@注意--
                {
                    validator: wmsValidate.proviinceValidate,
                    required: true,
                    trigger: 'change',
                    fullField: 'address'
                }],
            address: [{required: true, message: '详细地址不能为空', trigger: 'blur'}],
            shippingMethod: [{required: true, message: '送货方式不能为空', trigger: 'change'}]
                
            },  
    }
}
```
### 检验地址控件的方法

```js
    // 校验地址控件选择 必填, 不能双向绑定 改变数据
export default class wmsValidate {

    static proviinceValidate(rule, value, callback) {
        if (_.isArray(value) && value.length === 3) {
            return callback()
        } else {
            return callback(new Error('地址不能为空'))
        }
    }
}
```
**Tisp：** `v-model`绑定的名字，`prop`的名字，和`validate`中的名字必须一致，`validate`中才能接收到值
</br>




## iview中tabs的使用
<div style="color: #808080; font-size: 12px; text-align: right">2017-11-15 09:46:55 </div>

正确使用姿势：
```html
<Tabs v-if="pageName!=='detail'" type="card" class="wms-mt10 wms-tabs">
    <Tab-pane label="出库箱信息">
    </Tab-pane>
</Tabs>
<div v-if="pageName!=='detail'"> </div>
```
**Tisp：**： 不能将`Tabs`放在 带有`v-if`属性的`div`中


</br>
## iview中Model弹窗二次点击依然会关闭问题
<div style="color: #808080; font-size: 12px; text-align: right">2017-11-16 18:39:55 </div>

 确定按钮点击 --会自动关闭`Model`, 添加属性 `:loading`后需要在`@on-ok`事件中手动去设置关闭
 Tips: `:loading`初始值为要设置为 `true`
 **问题：** 首次点击未手动设置model关闭， 再次点击依然会自动关闭？
 场景：
> html

 ```html
<Modal
    v-model="syncUploadPop"
    title="title"
    width="400"
    :mask-closable="false"
    @on-ok="accountUnload"
    :loading="uploadLoading"
    >
</Model>
 ```
 > js

 ```js
 data() {
     return {
         syncUploadPop: false,      // 弹出款默认不显示
         uploadLoading: true        // 上传中
     }
 }
methods: {
    async accountUnload() {

        this.uploadLoading = true   // 确定点击显示 加载中、、、

        let res = await request.post(InterObj.accountUnloadTo, params)
        this.uploadLoading = false  // 请求成功取消 加载中、、、

        // -------解决问题的核心代码------
        this.$nextTick(() => {
            this.uploadLoading = true
        })
        // -------解决问题的核心代码------

        if(res.result === 'success') {
            this.$Message.success(res.msg)
            this.syncUploadPop = false
        } else {
            this.$Message.error(res.msg)
        }
    },
}

 ```
 **总结：** `:loading`设置成false取消 加载中状态后，需要在线程最后依然将 他设置为`true`