# uke-admin-web-scaffold

uke 管理后台脚手架引擎，快速打造用户体验一致的管理系统

- [在线文档](https://scaffold.ukelli.com/)

## 使用方式

### 安装

```shell
npm i uke-admin-web-scaffold --save
```

### 使用

```js
import { AdminWevScaffold } from 'uke-admin-web-scaffold';

class LoginFilter extends React.Component {
  componentDidMount() {
    // this.props.autoLogin();
    Call(window.OnLuanched);
  }
  render() {
    const { isLogin, userInfo } = this.props;
    return (
      <LoginSelector {...this.props}>
        {
          isLogin ? (
            <AdminWevScaffold
              {...this.props}
              // DashBoard 组件
              DashBoard={DashBoard}
              // 必须填写的
              username={userInfo.username}
              versionInfo={VersionInfo}
              userInfo={userInfo}
              menuMappers={{
                child: 'child',
                code: 'code',
                title: 'title',
                icon: 'icon',
              }}
              i18nConfig={i18nConfig}
              pluginComponent={{
                Statusbar: <Status/>
              }}
              pageComponents={pageComponents}/>
          ): null
        }
      </LoginSelector>
    );
  }
}

ReactDOM.render(LoginFilter, document.querySelector('#Main'));
```

[详情请看 uke admin seed 项目](https://github.com/SANGET/uke-admin-seed.git)

## 提供三个默认数据渲染模版

```js
import { FormRender, ReportTemplate, GeneralReportRender } from 'uke-admin-web-scaffold/template-engine';
```

## 前端资源管理模块

启动时需要通过 setApiUrl 接口设置前端管理服务的地址

```js
import { setApiUrl } from 'uke-admin-web-scaffold/fe-deploy';
setApiUrl('http://127.0.0.6550');
```

### 发布机制说明

1. 功能介绍、名词解释, 「开发(Dev)须知」
    - 创建「项目 Project」
    - 在「项目」中上传「资源 Asset」
    - 「项目」允许编辑, 但是「项目编号」是唯一标识, 用作部署标记, 不可更改
    - 「项目」允许删除, 删除会清理已上传的所有资源, 除了审计记录
    - 「资源列表」列出所有已上传的资源
    - 「资源列表」可以「发布、回滚、下载、删除」对应资源
    - 「项目创建者」对自己创建的项目有绝对控制权, 为自身项目负责, 其他人可以申请作为「协作者」加入到项目
    - 「操作审计」用于记录项目的所有操作, 由系统自动产生, 不可删除
2. 关于创建者与协作者机制, 「开发(Dev)运维(Ops)须知」
    - 「项目创建者」对自身创建的项目有绝对控制权, 可以允许其他人的协作申请, 让协作者加入
    - 「协作者」有一定的限制, 只能做「项目创建者」允许的操作, 同时不允许删除项目
3. 发布机制, 「开发(Dev)运维(Ops)须知」
    - 资源格式: 统一使用 zip 的压缩格式, 「部署服务器」需要有 unzip 功能
    - 部署路径: 根据「项目编码 projCode」标记 web server 的运行路径的部署目录, 例如 web-server/assets/public/[projCode]
    - 部署地址: 静态资源在端口 28101, 所以发布后的静态地址为 ip:28101/pb/[projCode]/xx || ip:28101/public/[projCode]/xx
4. 中转站发布机制以及 SCP 路径说明, 「运维(Ops)须知」
    - 中转服务: 部署 uke-web-server 到一台中转服务器上, 在该服务器上配置对应的目标服务器 ssh 配置, 配置路径为 ~/.ssh/config
    - scp 机制: 从中转服务器 scp 资源压缩包 -> 目标服务器(存放压缩包路径为 /var/front-end-zip/), 在目标服务器进行 unzip 解压到部署路径(例如 /var/www/deploy/[projCode]/)
    - SSH 配置说明: 根据 Host 字段获取 scp 目标名，Host 后可以用 # 写中文名词(只用于显示)，格式严格验证，例如 Host demoHost #测试地址
5. 关于 webhook 机制「开发(Dev)须知」
    - 资源发布成功后，可以触发指定的 webhook，具体 webhook 的功能由对应的服务提供，uke-web-server 有提供对应的 webhook 机制
    - webhook 可以触发 telegram 机器人，或者发送邮件

## TODO 完善文档说明
