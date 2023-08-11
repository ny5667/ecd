# 地图后端问题

## 不显示墨迹天气的数据

http://gitee.com/z5657/map/issues/I64XM5

业务请求接口地址：

[http://10.54.4.21:8080/msService/SESGISConfig/Weather/Weather/getWeatherInfoNew?lon=120.133759063&lat=30.176265606&_t=1682044346](http://10.54.4.21:8080/msService/SESGISConfig/Weather/Weather/getWeatherInfoNew?lon=120.133759063&lat=30.176265606&_t=1682044346)

## 地图分析服务接口报错，类型传参有误，请检查参数

更新地图分析服务包为最新版本

![](https://cdn.jsdelivr.net/gh/NY5667/CDN/images/20230307112231.png)

## sqlserver锁表

http://www.cnblogs.com/mq0036/p/12916606.html

http://bap.supcon.com/faq/guide/数据库相关问题.html#新建sqlserver需要刷行级锁



![](https://cdn.jsdelivr.net/gh/NY5667/CDN/images/20230309101852.png)

设置行级锁

```
--将数据库改成单用户模式
ALTER DATABASE DBNAME SET SINGLE_USER WITH ROLLBACK IMMEDIATE;
--设置隔离级别
ALTER DATABASE DBNAME SET READ_COMMITTED_SNAPSHOT ON;
--将数据库改成多用户模式
ALTER DATABASE DBNAME SET MULTI_USER;
--sql server 数据默认的事务隔离级别是：read commited
SET TRANSACTION ISOLATION LEVEL READ COMMITTED;
```



## 地图模型加载问题

http://gitee.com/z5657/map/issues/I64WBS

## 设置用户自定义模型

console控制台输入该命令跳转到模型视野，其中0、2为第几个模型

```
app.viewer.flyTo(app.viewer.scene.primitives._primitives[0]) 
```

```
app.viewer.flyTo(app.viewer.scene.primitives._primitives[2])
```

![](https://cdn.jsdelivr.net/gh/NY5667/CDN/images/20230308172034.png)

注意：亮度需要设置为0.2。

![](https://cdn.jsdelivr.net/gh/NY5667/CDN/images/20230308172044.png)

![](https://cdn.jsdelivr.net/gh/NY5667/CDN/images/20230308172049.png)

## failed to request http://nacos.default:8848/nacos/v1/auth/users/login

1. 集群环境NACOS_ADDRESS配置为多个IP地址，如果安装后分析服务启动不了，看日志发现是nacos连不上问题，进行如下操作: 查看已启动服务的更新配置

2. 复制nacos信息到地图分析服务上对应位置

![](https://cdn.jsdelivr.net/gh/NY5667/CDN/images/20230314165536.png)

![](https://cdn.jsdelivr.net/gh/NY5667/CDN/images/20230314165553.png)

![](https://cdn.jsdelivr.net/gh/NY5667/CDN/images/20230314165559.png)

## 前端页面报错查看

- 打开开发者工具

![](https://cdn.jsdelivr.net/gh/NY5667/CDN/images/1.png)

- 点击message查看console所有日志

![](https://cdn.jsdelivr.net/gh/NY5667/CDN/images/2.png)

- 点击network，点击status排序后拉到底，找到所有404和500相关接口

![](https://cdn.jsdelivr.net/gh/NY5667/CDN/images/3.png)

- 点击对接接口地址，查看具体接口报错日志

![](https://cdn.jsdelivr.net/gh/NY5667/CDN/images/4.png)



## 无底图问题

![](https://cdn.jsdelivr.net/gh/NY5667/CDN/images/微信图片_20230526142234.png)

未配置正确精度主模型！请到地图配置中心-全局设置-基础底图设置中设置

这两项不能为空

```
SESGIS_BASE_LAYERSS
```

```
update SESGIS_BASE_LAYERSS set VISIBLE = 1,FINENESS ='SESGISConfig_ModelFineness/001';
```



![](https://cdn.jsdelivr.net/gh/NY5667/CDN/images/微信图片_20230403135725.png)

【全局配置】那面配置有问题，现场当前这个版本不支持多主模型，把【出厂默认主模型】的模型类型改成【辅助模型】就可以了

## extension "postgis" is not available

SQL 错误 [0A000]: 错误: extension "postgis" is not available
Detail: Could not open extension control file "C:/Program Files/PostgreSQL/15/share/extension/postgis.control": No such file or directory.
Hint: The extension must first be installed on the system where PostgreSQL is running.

postgis没有装

## throwPoolInitializationException

检查服务器和数据库ip可以ping通

![](https://cdn.jsdelivr.net/gh/NY5667/CDN/images/微信图片_20230412140342.png)

## 地图初始化地址

http://127.0.0.1:8080/msService/SESGISConfig/vSet/md/Map-Init.html

## nacos连接不上

调整为注释的nacos

![](https://cdn.jsdelivr.net/gh/NY5667/CDN/images/微信图片_20230420154023.png)

## java.lang.NoSuchFieldException: PLUGINID.PLUGINVIEWIMAGE_PROPERTY_CODE

需要升级到大于等于开发环境

supOS版本号：V4.00.00.04-22061815

在实体配置上把这个字段在对应页面上删除，并在该表上删除该字段

![](https://cdn.jsdelivr.net/gh/NY5667/CDN/images/Snipaste_2023-04-21_10-38-31.png)

## 地图配置中心模块上载报错,错误信息为query did not return a unique result: 2; nested exception is org.hibernate.NonUniqueResultException: query did not return a unique result: 2

![](https://cdn.jsdelivr.net/gh/NY5667/CDN/images/微信图片_20230420160753.png)

查看报错日志

查看报错服务名：supplant-configuration

```
kubectl get po | grep confi
kubectl logs -f --tail=500 supplant-configuration-7dd564fbf8-zppx8
```

```
kubectl get pods | grep supplant-configuration | awk '{print $1}' | xargs kubectl logs --tail=5000 -f 
```

删除数据条数多行一条数据

```
SELECT CODE,COUNT(*)
FROM RBAC_MENUINFO
WHERE VALID = 1
GROUP BY CODE 
HAVING COUNT(*) > 1
```

## 地图图标库上传自定义图片

https://gitee.com/z5657/map/issues/I683QD

## 空间分析服务，Common_Point 的 "roll"字段不存在

```roomsql
ALTER TABLE public."Common_Point" ADD roll double precision NULL;
ALTER TABLE public."Common_Point" ADD pitch double precision NULL;
ALTER TABLE public."Common_Point" ADD heading double precision NULL;
ALTER TABLE public."Common_Point" ADD azimuth double precision NULL;

ALTER TABLE public."Common_Line" ADD roll double precision NULL;
ALTER TABLE public."Common_Line" ADD pitch double precision NULL;
ALTER TABLE public."Common_Line" ADD heading double precision NULL;
ALTER TABLE public."Common_Line" ADD azimuth double precision NULL;

ALTER TABLE public."Common_Polygon" ADD roll double precision NULL;
ALTER TABLE public."Common_Polygon" ADD pitch double precision NULL;
ALTER TABLE public."Common_Polygon" ADD heading double precision NULL;
ALTER TABLE public."Common_Polygon" ADD azimuth double precision NULL;
```

## 空间数据库加字段

```roomsql
ALTER TABLE public."Common_Point" ADD glbsrc varchar(100) NULL;
ALTER TABLE public."Common_Point" ADD "scale" int4 NULL;
ALTER TABLE public."Common_Point" ADD busdata text NULL;

ALTER TABLE public."Common_Line" ADD glbsrc varchar(100) NULL;
ALTER TABLE public."Common_Line" ADD "scale" int4 NULL;
ALTER TABLE public."Common_Line" ADD busdata text NULL;

ALTER TABLE public."Common_Polygon" ADD glbsrc varchar(100) NULL;
ALTER TABLE public."Common_Polygon" ADD "scale" int4 NULL;
ALTER TABLE public."Common_Polygon" ADD busdata text NULL;
```

## 删除业务的generate业务包

删除应急指挥的generate业务包

linux: 

```shell
find /volumes/nfs/ -maxdepth 1 -type d -name "default-supplant-pvc-pvc-*" | awk '{print "rm -r \"" $0"/generate/SESECD_1.0.0" "\""}' | sh
```
windows:

```shell
rd /s /q %ADP_HOME%\bap-server\bap-workspace\generate\SESECD_1.0.0
```

删除应急资源的generate业务包

```shell
find /volumes/nfs/ -maxdepth 1 -type d -name "default-supplant-pvc-pvc-*" | awk '{print "rm -r \"" $0"/generate/SESWssER_1.0.0" "\""}' | sh
```
windows:

```shell
rd /s /q %ADP_HOME%\bap-server\bap-workspace\generate\SESWssER_1.0.0
```

# 数据库问题

## unable to extend lob segment HZSHIIP.SYS_LOB0000243299C00004$$ by 1024 in tablespace HZSHIIP

![](https://cdn.jsdelivr.net/gh/NY5667/CDN/images/微信图片_20230531153609.png)

## saas数据库直接用dbeaver直接连

连接saas环境：一种是可以用`kubectl get po`方式查询到，到再进入容器；另一种方式是直接用dbeaver直接连接；

# 平台问题

## 服务启动报错

com.supcon.orchid.services.BAPException: ec.msModule.service.startFailed<br>Waiting for service start for 30 seconds timeout!<br>ec.msModule.service.seeMoreInfo E:/ADP/bap-server/../logs/entityconf-logs\..\gisConfig\gisConfig.log 日志

启动服务端口占用，换端口处理

# 地图前端问题

## 天地图录坐标页面无天地图问题

http://gitee.com/z5657/map/issues/I6844M

## 查看当前视野坐标点

```
app.viewer.entities.values
```

## 默认视野模型显示

http://gitee.com/z5657/map/issues/I64WBS

## 楼层不显示

把中控的modelMeta改成现场的modelMeta文件，该文件在GISModel根目录

# 应急管理

## 部门数据删除处理

重复以下操作至页面不报缺少部门id为止

1.看页面上的报错缺失的部门id

2.在部门管理页面上新增部门

3.在数据库把新增的部门id改为缺失的部门id

数据库中部门的表为`org_department`

![](https://cdn.jsdelivr.net/gh/NY5667/CDN/images/Snipaste_2023-04-17_15-13-11.png)

![](https://cdn.jsdelivr.net/gh/NY5667/CDN/images/微信图片_20230417151323.png)

## 数据权限显示问题

调整对应的数据权限字段

![](https://cdn.jsdelivr.net/gh/NY5667/CDN/images/微信图片_20230418143456.png)

# 应急指挥

## 接警列表空白

在系统编码中增加图层`vxAlertRecordLayer`、`alertRecordLayer`、`warningLayer`，并重启启动应急指挥模块

# 安全模块常见问题——安全监控

## 实时报警数据查询

```
SELECT
 count(rcd.id) alarmCount,
 pnt.APPCODE
FROM
 AM_RECORDS rcd
LEFT JOIN AM_POINTS pnt ON
 pnt.id = rcd.ALARMID
WHERE
 pnt.id IN (SELECT app.ALARMID as ALARM_ID FROM AM_PERMISSION_POINTS app WHERE app.VALID = 1 AND app.IS_CAN_CONFIG = 1 --AND app.USERID = :currentStaffId
 UNION 
 SELECT app2.ALARM_ID as ALARM_ID FROM AM_POST_PERMISSIONS app2 WHERE app2.VALID = 1 AND app2.IS_CAN_CONFIG = 1 AND app2.POSITION_ID IN (SELECT bp.POSITION_ID FROM BASE_POSITIONWORK bp WHERE bp.VALID = 1 --AND bp.STAFF_ID = :currentStaffId
 )
 UNION 
 SELECT arp.ALARM_ID as ALARM_ID FROM AM_ROLE_PERMISSIONS arp WHERE arp.VALID = 1 AND arp.IS_CAN_CONFIG = 1 AND arp.ROLE_ID IN (SELECT br.ROLE_ID FROM BASE_ROLEUSER br LEFT JOIN BASE_STAFF bs ON bs.USER_ID = br.USER_ID --WHERE bs.ID = :currentStaffId
 )
 UNION 
 SELECT ap.ID AS ALARM_ID FROM AM_POINTS ap WHERE ap.VALID = 1 --AND ap.OWNER = :currentStaffId
 )
 AND rcd.IS_CONFIRM = 0
 --AND pnt.CID IN (:cids)
 GROUP BY pnt.APPCODE
```

## 报警分类数据问题

![](https://cdn.jsdelivr.net/gh/NY5667/CDN/images/微信图片_20230410151035.png)

```
SELECT * from AM_CLASSIFIES
```

![](https://cdn.jsdelivr.net/gh/NY5667/CDN/images/微信图片_20230410155252.png)


## 危险源、物联设备如何在地图上实现报警（弹框、光晕闪烁）

[安全模块常见问题——积分管理、安全监控.pdf](https://cdn.jsdelivr.net/gh/NY5667/CDN/docs/安全模块常见问题——积分管理、安全监控.pdf)

## 报警点来源修改
``
```roomsql
select id,ALARM_NAME ,CLASSIFY_CODE ,APPCODE  from AM_POINTS ap where valid = 1;

SELECT id,APPNAME ,APPCODE  from AM_ORIGINS ao where valid = 1;

SELECT id,CNAME ,CCODE ,APP_CODE ,MSG_CODE,VALID  from AM_CLASSIFIES ac where valid = 1 ;
```


# 软件下载

## 地图安装软件

[http://zaq1.cloud/software/postgresql-10.4-1-windows-x64.exe](http://zaq1.cloud/software/postgresql-10.4-1-windows-x64.exe)

[http://zaq1.cloud/software/postgis-bundle-pg10x64-setup-2.5.0-1.exe](http://zaq1.cloud/software/postgis-bundle-pg10x64-setup-2.5.0-1.exe)

## 地图相关

融合环境部署手册：

[http://zaq1.cloud/document/Map-Install-Linux.pdf](http://zaq1.cloud/document/Map-Install-Linux.pdf)

SAAS环境部署手册

[http://zaq1.cloud/document/Map-Install-Saas.pdf](http://zaq1.cloud/document/Map-Install-Saas.pdf)

# 数据库驱动

http://zaq1.cloud/download/dbeaver-jdbc.zip
