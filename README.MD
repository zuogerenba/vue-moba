## 一.后端接口

### 1.1通用 CURD

模型不能通用，但是增删查改可以通用

~~~javascript
// categories=>Category、
router.post('/categories', async (req, res) => {
    const model = await Category.create(req.body)
    res.send(model)
  })
~~~

`app.use('/admin/api/rest/:resource', router)` ：将`:resource`设置为动态的替换`categories`等

#### 通用代码引用不同模型

- 先安装 `npm i inflection`

- ~~~javascript
  // 依靠动态参数可以引入不同的模型，放入增删查改当中
  const modelName = require('inflection').classify(req.params.resource)
  const Model = require(`../../models/${modelName}`)
  ~~~

- ~~~javascript
    router.get('/', async (req, res) => {
      const modelName = require('inflection').classify(req.params.resource)
      const Model = require(`../../models/${modelName}`)
      // populate表示关联取出东西，不仅仅可以取到id，可以取到完整的对象信息
      const items = await Model.find().populate('parent').limit(10) // 限制十条数据
      res.send(items)  // 直接发送给前端
    })
  // 进阶
  app.use('/admin/api/rest/:resource', async(req, res, next) => {
        const modelName = require('inflection').classify(req.params.resource)
        req.Model = require(`../../models/${modelName}`)
        next()
      }, router)
    }
    ~~~
    

### 1.2 上传文件

使用的是element-ui， 需要一个完整的url`<el-upload action="$http.defaults.baseURL + '/upload'" \>`

在后台使用 `multer` 处理 :`npm i multer`
