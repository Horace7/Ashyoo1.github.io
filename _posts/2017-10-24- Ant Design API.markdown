---
title:  "Ant Design API"
categories: [react]
tags: [react]
date: 2017/10/24
---

又一期项目结束了,总结一下antd的api,熟练使用的话的确能提升开发效率

## Form 表单

* antd Form.Item
```javascript
  const formItemLayout = {
    labelCol: { span: 4 },
    wrapperCol: { span: 20 }
  }

  <FormItem
    label={'退款金额：'}
    { ...formItemLayout }
  >
    <Input />
  </Form>
```
***
* antd Form.create()

    > 使用方式如下：<br />
    > class CustomizedForm extends React.Component {}<br />
    > CustomizedForm = Form.create({})(CustomizedForm)

经过 Form.create 包装的组件将会自带 this.props.form 属性，this.props.form 提供api

参数| 主要内容
-------|----------
getFieldsValue | 获取一组输入控件的值，如不传入参数，则获取全部组件的值
getFieldValue | 获取一个输入控件的值
setFieldsValue | 设置一组输入控件的值（注意：不要在 componentWillReceiveProps 内使用，否则会导致死循环）
validateFields | 校验并获取一组输入域的值与 Error，若 fieldNames 参数为空，则校验全部组件
getFieldsError | 获取一组输入控件的 Error ，如不传入参数，则获取全部组件的 Error

`getFieldsValue`获取一组输入控件的值，如不传入参数，则获取全部组件的值

`getFieldValue`获取一个输入控件的值

```javascript
  _getOrderList = () => {
    const filterData = this.props.form.getFieldsValue()
    const { dispatch } = this.props
    dispatch(action.getOrderInfo({
      orderNo: filterData.orderNo || '',
      status: filterData.orderStatus || '',
    }))
  }
```

`getFieldDecorator` 表单的双向绑定 this.props.form.getFieldDecorator(id, options)

>经过 getFieldDecorator 包装的控件，表单控件会自动添加 value（或 valuePropName 指定的其他属性） onChange（或 trigger 指定的其他属性），数据同步将被 Form 接管，这会导致以下结果：<br />
>1. 你不再需要也不应该用 onChange 来做同步，但还是可以继续监听 onChange 等事件
>2. 你不能用控件的 value defaultValue 等属性来设置表单域的值，默认值可以用 getFieldDecorator 里的 initialValue。
>3. 你不应该用 setState，可以使用 this.props.form.setFieldsValue 来动态改变表单值。
```javascript
// bad
      this.state = {
        password = ''
      }
      _getPwd = e => {
        this.setState({
          password: e.target.value
        })
      }
      <Input onChange={this._getPwd}/>
// bad
      <FormItem className={style['jc-order-row']}>
       <p className={style['jc-search-item']}>订单编号:</p>
       {getFieldDecorator('orderNo')(
         <Input className={style['jc-search-input']} placeholder='订单编号'/>
       )}
     </FormItem>
// good
      <FormItem label='密码'>
        {getFieldDecorator('password', {
          initialValue: ''
        })(
          <Input />
        )}
      </FormItem>
```

***
* antd Form 校验

antd的Form表单中的属性 pattern 可以直接写正则表达式校验
```javascript
  const { getFieldDecorator } = this.props.form
  <FormItem { ...formItemLayout } label='账号'>
    {getFieldDecorator('userName', {
      rules: [{
        required: true,  //是否需要校验
        min: 6,    //最小长度
        max: 18,    //最大长度
        whitespace: true, // true时，不可以出现空格
        pattern: /^[0-9A-Za-z]{1,15}$/,   // 正则表达式校验
        message: '账号格式错误：只能数字和字母，最多不能超过15位' // 错误信息
      }],
      initialValue: '默认值'
    })(
      <Input placeholder='数字、字母、数字和字母组合，最多不能超过15位'/>
    )}
  </FormItem>
```

## Table 表格
* Table

参数| 主要内容
-------|----------
title | 表格尾部
footer | 表格标题
dataSource | 表格列的配置描述，具体项见下表
pagination | 分页器，配置项参考 pagination，设为 false 时不展示和进行分页
columns | 表格列的配置描述
rowKey | 表格行 key 的取值，可以是字符串或一个函数

 ```javascript
 // bad

 <Row>
    <Col span={12} className={styles['page-left']}>共{tableList.page.totalCount}条记录，{tableList.page.currentPage}/{tableList.page.totalPage}页</Col>
    <Col span={12}>
      <Pagination
        className={styles['pagination']}
        showQuickJumper onChange={this.handleChange}
        current={tableList.page.currentPage}
        defaultPageSize={tableList.page.pageSize}
        total={tableList.page && tableList.page.totalCount}
      ／>
    </Col>
  </Row>

// good

  <Table
    columns={this._columns}
    dataSource={tableList.result}
    rowKey={item => item.orderNo}
    pagination={false}
    bordered={true}
  />
  <Pagination
    onChange={this._handleChange}
    current={tableList.page.currentPage}  // 当前页数
    defaultPageSize={tableList.page.pageSize}  // 默认的每页条数
    showTotal={total => `总共 ${tableList.page.totalCount} 条`}  // 用于显示数据总量和当前数据顺序
    total={tableList.page && tableList.page.totalCount} // 数据总数
  />
 ```
>注意：
>按照 React 的规范，所有的组件数组必须绑定 key。在 Table 中，dataSource 和 columns 里的数据值都需要指定 key 值。对于 dataSource 默认将每列数据的 key 属性作为唯一的标识。

![](https://os.alipayobjects.com/rmsportal/luLdLvhPOiRpyss.png)
`rowKey={item => item.orderNo}`唯一的主键

***

* columns 表格列的配置描述（表头）

参数| 主要内容
-------|----------
title | 列头显示文字
key | React 需要的 key，如果已经设置了唯一的 dataIndex，可以忽略这个属性
dataIndex | 列数据在数据项中对应的 key，支持 a.b.c 的嵌套写法
render (text, record, index) | 当前行的值，当前行数据，行索引
colSpan | 表头列合并,设置为 0 时，不渲染
width | 列宽度


```javascript
  _columns = [{
    title: '活动时间',
    dataIndex: 'activityTimeFrom',
    key: 'activityTimeFrom',
    width: 230,
    render: (text, record) => {
      return (
        <div>
          <p>{record.activityTimeFrom}</p>
          -
          <p>{record.activityTimeTo}</p>
        </div>
      )
    }
  }, {
    title: '核销码状态',
    dataIndex: 'volumeCodeStatus',
    key: 'volumeCodeStatus',
    width: 100,
    render: (text, record) => (
      <span>
        {text === 'N' ? '未核销' : '已核销' }
      </span>
    )
  }]
```

## Input 输入框

参数| 主要内容
-------|----------
type | 声明 input 类型，同原生 input 标签的 type 属性。
id | 输入框的 id
value | 输入框内容
defaultValue | 输入框默认内容
disabled | 是否禁用状态，默认为 false
onPressEnter | 按下回车的回调

>如果 Input 在 Form.Item 内，并且 Form.Item 设置了 id 和 options 属性，则 value defaultValue 和 id 属性会被自动设置。

1. `Input.Search`搜索框
```javascript
onSearch  点击搜索或按下回车键时的回调
```
2. `Input.TextArea`文本域

3. `Input.TextArea`数字输入框
```javascript
min	  最小值
max	  最大值
step	 小数位数
```

## Modal 对话框
参数| 主要内容
-------|----------
visible | 对话框是否可见
title | 标题
footer | 底部内容，当不需要默认底部按钮时，可以设为 footer={null}
onOk | 点击确定回调
onCancel | 点击遮罩层或右上角叉或取消按钮的回调

> 清空旧数据
> <Modal /> 组件有标准的 React 生命周期，关闭后状态不会自动清空。 如果希望每次打开都是新内容，需要自行手动清空旧的状态。