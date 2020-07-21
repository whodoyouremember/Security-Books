# drozer

# 安装
     adb forward tcp:31415 tcp:31415
     drozer console connect

# 获取包名
     run app.package.list
# 查找攻击面
    run app.package.attacksurface package_name
# activity 组件测试
    获取安装包中的组件
    run app.activity.info -a com.xxxxx
    调用某个组件
     run app.activity.start --component com.xxx.pe.xxxxwer(包名) xxxxxview.MainActivity（组件明）
# Broadcast Receivers 组件测试
## 查看Broadcast组件信息
     run app.broadcast.info -a package_name

