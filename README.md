###  itfinally project
#### 一个为加快开发速度而开发的项目

#### 大家好, 人称前言的便是区区在下

这个项目的所有子模块都是出自作者在开发过程中遇到的一些重复处理的问题,
因而收集起来写成模块, 基于此模块下只需要关心自己的需求即可.

当然网上也有很多现成的 OA 类框架, 但是恕我直言, 无论对外说的多好多美, 
只要你看一眼代码就知道其实都是那副德性( 好吧我看过真的给恶心到了才这么说 ), 
而且大部分都是集成老一套的 jsp 或模版技术, 更有甚者写出各种奇奇怪怪的 javascript, 
嵌套各种 iframe, 令人陷进神一样的 iframe 间通讯问题, 难以维护, 
甚至为了填坑创造出更多的坑. ( 还有用一长串 if-elif-else 写 mvc dispatcher 的我会说? )

我觉得一个好的后台框架, 是不应该涉及到前端的, 
专心做好后台的事, 尽力最到最好.( 尤其是大部分做后台的前端能力都参差不齐, 可能还不如专修前端的毕业生, 
有时候开发起来真的是直骂娘, 当然我也是前端不怎样的家伙 ).

基于此, 萌生了以 spring-boot 为基础来编写一系列工具的想法.
后期也会编写前端, 抽象出一整套以 vue 做为基础的前端平台, 当然主要是给自己用的.

噢, 每个子模块的存在问题( 也就是作者尚未解决的问题 ), 设计思路, 以及为什么这么做的说明
都会以 README.md 的形式放在模块根目录下.

#### 各单位注意, 前言已经去领便当了, 我是正文

整个项目会以模块的形式使用 maven 辅助合并编译, 因为所有业务模块均依赖
core ( 基础工具及类库 ) & parent ( 基础pom ) 两个子模块.

项目说明:
* 这里先说说为何选用 myBatis.
  
  当然我不是拒绝 hibernate ( 其实我是拒绝的 ), 主要是这货为了兼容大部分 db,
  做了灰常多妥协, 很多 sql 并不是最优, 而且 session 直接绑定线程, 导致多线程操作成本偏高
  ( 只能用 openSession 重新绑定, 或者修改 session 策略, 其实我是觉得很麻烦的 ), 
  倒不如 mybatis 直接判断线程 id 决定是否创建新的 sessionFactory 来规避多线程问题来的聪明.
  
  当然 mybatis 直接的缺点就是自动化程度太低, 所以所有项目都改用 dao/mapper 重新封装一次, 
  除了新增接口外, 所有操作直接面向 api, 后期会另外开新模块写生成器.
  
* 选用 mysql 是因为这货在大部分开发中都是首选, 份额也是最大的, 直接选用.
  所以整个项目都是仅针对 mysql 的, 如果有其他 db ( 比如zf那群为了[安全or装逼]的人专门用 oracle )
  的需求, 那只能自己修改对应模块内的 xml 啦.( 没错, sql 都写在 xml 里面 )
  
* 所有涉及到的 url 均采用 restful 风格.( 其实没做的那么符合风格, 比如删除我就不会做成接收 DELETE 请求 )
* 所有涉及到前端请求交互的载体都继承于 core 模块的 vo.BaseVoBean 类.
* 所有涉及到数据库交互的实体类都继承于 core 模块的 repository.po.BaseEntity 类.
* 秉承 linux 那句 "没有消息就是好消息". 一般在编写时考虑到的问题都会写在文档上, 如果没有说明, 那一般是没问题. 
  如果碰到并且确认是个 bug, 记得带上砖头来 issues 找作者拍砖.
  
* 时间均采用机器时间, 连带数据库的数据类型为长整型, 存储应该和展示分离, 不受时区影响.
* 默认约定数据的状态, -1 为已删除, 1 为正常, 且不存在 0 这种特殊数字, 所有删除行为均为逻辑删除.
  ( 其实也有提供物理删除, 但是已经重写基类进行禁用, 强行调用会直接抛异常, 在 `top.itfinally.core.repository.dao.AbstractDao` 内,
   有需要的自行修改再编译打包)

整个项目开发基于以下版本/工具/框架:
* IntelliJ IDEA ( 不喜欢 eclipse 那么原始的工具, 请善待自己 )
* Java 1.8      ( 抱歉, 用完 stream 那一刻我就不想向下兼容了 )
* Guava         ( 用过就会上瘾的 google 工具库, 懒人福音, 一般人我不告诉他 )
* Spring Boot   ( 比较懒直接集成 sb 了, 尽量会跟随最新版 )
* Spring-*      ( 这个意思就是基本在 spring 全家桶上开发 )
* Mybatis       ( 嗯, 不想用 hibernate 就是因为我要自定义 sql )
* Mysql         ( 牵扯到数据库, 所以直接针对 mysql, 其他数据库请自行修改 xml 的 sql )

最后, 不要说什么代码风格不重要, 连风格都不注重的人只有两种, 
要么思维能力强到根本没人能理解却又次次都化解问题的大牛, 要么就是弱鸡.

毕竟, 代码是写给人看的, 顺便给机器执行而已.

做人还是要有点要求和追求.

That all.