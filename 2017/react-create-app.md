# react-create-app

### 坑位1

这个脚手架的正真的`webpack`配置文件并不是项目下面的`webpack.config.js`，我今天在里面尝试了CSS Modules的写法，但是写来写去不成功。不能正常使用CSS Modules的语法
。

网上有人也发现了这个问题，使用
``` css
:local(.class){
  //do some thing
}
```
这个`:local`前缀可以使CSS Modules可以正常使用，但是每个要加这个前缀不是很合理，而且使用不方便。


后来发现react脚手架有个eject需要开启，正真的配置文件在开启后会有显示，分别是`webpack.config.dev.js`和`webpack.config.pro.js`。开发版本和生产版本的两个配置文件。

配置这两个文件就可以了。
`webpack.config.dev.js`
``` jsx
// before
{
  loader: require.resolve('css-loader'),
  options: {
    importLoaders: 1,
  },
},
// after
{
  loader: require.resolve('css-loader'),
  options: {
    importLoaders: 1,
    modules: true,
    localIdentName: "[name]__[local]___[hash:base64:5]"  
  },
},
```

`webpack.config.pro.js`
``` jsx
// before
{
  loader: require.resolve('css-loader'),
  options: {
    importLoaders: 1,
    minimize: true,
    sourceMap: true,
   },
},
// after
{
  loader: require.resolve('css-loader'),
  options: {
    importLoaders: 1,
    modules: true,
    minimize: true,
    sourceMap: true,
   },
},
```

这样就不用加上奇怪的`:local`前缀了，也可以正常使用CSS Modules了~~
