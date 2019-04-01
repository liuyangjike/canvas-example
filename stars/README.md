### stars
实现`stars`效果
### 实现
#### star
固定的星星叫`star`,
```js
function Star (id, x, y, useCache) {
  this.id = id
  this.x = x
  this.y = y
  this.useCache = useCache
  this.cacheCanvas = document.createElement('canvas')
  this.cacheCtx = this.cacheCanvas.getContext('2d')
  this.r = Math.floor(Math.random() * config.star_r) + 1
  this.cacheCtx.width = 6 * this.r
  this.cacheCtx.height = 6 * this.r
  var alpha = (Math.floor(Math.random() * 10) + 1) / config.star_alpha
  this.color = 'rgba(255, 255, 255,' + alpha + ')'
  if (useCache) {  // 初始化使用缓存
    this.cache()
  }
}
```

#### dot
运动的星星叫`dot`
```js
function Dot(id, x, y, useCache) {
  this.id = id
  this.x = x
  this.y = y
  this.r = Math.floor(Math.random() * config.dot_r) + 1
  this.speed = config.dot_speeds
  this.a = config.dot_alpha
  this.aReduction = config.dot_aReduction
  this.useCache = useCache
  this.dotCanvas = document.createElement('canvas')   // 缓存用的
  this.dotCtx = this.dotCanvas.getContext('2d')
  this.dotCtx.width = 6 * this.r
  this.dotCtx.height = 6 * this.r
  this.dotCtx.a = 0.5
  this.color = "rgba(255,255,255," + this.a + ")"
  this.dotCtx.color = "rgba(255,255,255," + this.dotCtx.a + ")"
  this.linkColor = "rgba(255,255,255," + this.a/4 + ")"
  this.dir = Math.floor(Math.random() * 140) + 200
  if (useCache) {
    this.cache()
  }
}
```

### 缓存
实例化星星的时候会使用缓存, 利用新建一个画布, 保存信息, 使用的时候直接使用`ctx.drawImage(this.cacheCanvas, this.x - this.r, this.y - this.r) // 左上角的坐标`
```js
cache: function() {
  this.cacheCtx.save()
  this.cacheCtx.fillStyle = this.color
  this.cacheCtx.shadowColor = 'white'
  this.cacheCtx.shadowBlur = this.r * 2
  this.cacheCtx.beginPath()
  this.cacheCtx.arc(this.r * 3, this.r * 3, this.r, 0, 2 * Math.PI)
  this.cacheCtx.closePath()
  this.cacheCtx.fill()
  this.cacheCtx.restore()
},
```