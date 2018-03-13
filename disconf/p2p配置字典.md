



- 后台编辑value或者其他信息后，需要更新redis缓存

- 获取配置信息，是需要遍历redis中全部item，以key来选取，需要注意enabled状态。

- 如果没有这个item，则可根据参数默认去创建
  1. 不存在item去创建这种情况，统一放到ELSE组中，如果ELSE组不存在则创建ELSE组
  2. 这种创建方式，很多值都取默认值，如下
     - key=KEY
     - value=:value
     - description=:valu
     - group=ELSE
  3. ELSE不存在
     - group=ELSE
     - description=其他
  4. 后面可在后台中将item调到其他组去

-  由于要给资金管理使用，所以在访问T+0配置页面前，需要添加相关的key

   -  新建组 WITHDRAWT0

   -  投资人是否能使用T0：WITHDRAWT0_INVESTOR

   -  T0开关:WITHDRAWT0_FUNCTION

      ​