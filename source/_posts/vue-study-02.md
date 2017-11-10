---
title: iview组件级联选择器的校验
tags: [iview组件]
categories: [vue框架]
date: 2017-11-08 16:03:39
---
使用iview的级联选择器，添加校验时的坑、、、
<!-- more -->
#### html

```html
 <Form :label-width="100" ref="formData" :rules="ruleValidate" :model="formData" inline class="wms-form-check oneline">   

    <Form-item label="收货地址：" style="width:532px" prop="region" `@注意region`>
        <Cascader :data="provinceCity" v-model="formData.region" `@注意region` size="small" trigger="hover"></Cascader>
    </Form-item>

<Form>
```
#### javascript

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
#### 检验地址控件的方法

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
</br>
**总结：** `v-model`绑定的名字，`prop`的名字，和`validate`中的名字必须一致，`validate`中才能接收到值