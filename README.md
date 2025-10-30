<img width="532" height="234" alt="image" src="https://github.com/user-attachments/assets/6c514f8b-1285-4248-8951-538fbf40aec1" />
3. 重新部署Workers
4. 访问 `/{你的UUID}` 即可使用图形化配置管理

#### 🔑 API快速开始
1. https://github.com/byJoey/yx-tools/releases 优选软件
2. **开启API功能**：访问 `/{UUID}` 或 `/{自定义路径}` → 找到"允许API管理"→ 选择"开启API管理"→ 保存
3. **API路径说明**：
   - 使用 UUID：`/{UUID}/api/...`
   - 使用自定义路径（d变量）：`/{自定义路径}/api/...`
4. **添加单个IP**：
```bash
curl -X POST "https://your-worker.workers.dev/{UUID}/api/preferred-ips" \
  -H "Content-Type: application/json" \
  -d '{"ip": "1.2.3.4", "port": 443, "name": "香港节点"}'
```
5. **批量添加IP**：
```bash
curl -X POST "https://your-worker.workers.dev/{UUID}/api/preferred-ips" \
  -H "Content-Type: application/json" \
  -d '[
    {"ip": "1.2.3.4", "port": 443, "name": "节点1"},
    {"ip": "5.6.7.8", "port": 8443, "name": "节点2"}
  ]'
```
6. **一键清空**：
```bash
curl -X DELETE "https://your-worker.workers.dev/{UUID}/api/preferred-ips" \
  -H "Content-Type: application/json" \
  -d '{"all": true}'
```

###  新功能

#### 🎯 图形化配置管理
- **KV存储支持**：使用Cloudflare KV存储持久化配置
- **图形化界面**：访问 `/{你的UUID}` 或 `/{自定义路径}` 即可使用配置管理界面
- **实时配置**：无需重新部署，配置立即生效
- **配置优先级**：KV配置 > 环境变量 > 默认值
- **路径灵活**：支持 UUID 模式和自定义路径模式

#### 🔐 多协议支持
- **VLESS 协议**：默认启用，基于 WebSocket 的标准协议
- **Trojan 协议**：支持 SHA224 密码认证，可自定义密码或使用 UUID
- **xhttp 协议**：基于 HTTP POST 的伪装协议，需绑定自定义域名并开启 gRPC
- **灵活切换**：可同时启用多个协议，也可只选择单一协议
- **协议配置**：通过图形化界面轻松管理协议开关

#### 🛣️ 自定义路径（d 变量）
- **自定义访问路径**：支持使用自定义路径代替 UUID
- **路径示例**：设置 `d=/mypath` 后，访问 `https://worker.dev/mypath` 即可
- **安全增强**：启用自定义路径后，UUID 路径自动禁用
- **智能识别**：首页终端自动识别当前使用的是 UUID 还是自定义路径
- **路径显示**：高级配置区实时显示当前使用的路径类型（u/d）

#### 🔄 订阅转换配置
- **自定义转换地址**：支持自定义订阅转换 API 地址
- **默认地址**：`https://url.v1.mk/sub`
- **灵活配置**：可在高级控制区修改转换服务地址
- **多客户端支持**：配合转换服务支持 Clash、Surge、Sing-box 等客户端

#### 🎛️ 内置优选类型控制
- **优选域名**：控制是否包含 CF 优选域名节点
- **优选 IP**：控制是否包含 CF 优选 IP 节点
- **GitHub 默认优选**：控制是否包含 GitHub 上的默认优选节点
- **灵活组合**：可任意组合启用，默认全部启用
- **订阅精简**：根据需要选择性生成节点，减少订阅体积

#### 🚀 API动态管理
- **API管理**：通过RESTful API动态管理优选IP，无需修改代码
- **批量上报**：支持一次性批量添加多个优选IP
- **一键清空**：支持清空所有优选IP，快速更新列表
- **安全开关**：默认关闭，需在图形界面手动开启API功能
- **自动合并**：API添加的IP与手动配置的yx变量自动合并
- **实时同步**：API添加的IP立即在配置页面显示
- **API端点**：
  - `GET /{UUID或自定义路径}/api/preferred-ips` - 查询优选IP列表
  - `POST /{UUID或自定义路径}/api/preferred-ips` - 添加优选IP（支持单个/批量）
  - `DELETE /{UUID或自定义路径}/api/preferred-ips` - 删除优选IP（支持单个/全部）

#### 🌍 手动指定地区
- **地区选择**：支持手动指定Worker地区，覆盖自动检测
- **设置方式**：`wk=SG` 或通过图形化界面选择
- **支持地区**：US、SG、JP、HK、KR、DE、SE、NL、FI、GB 
- **智能显示**：系统状态会显示"手动指定地区"而非"自动检测"

#### 🏷️ 优选节点命名
- **自定义名称**：支持为优选节点设置自定义名称
- **格式支持**：`IP:端口#节点名称` 或 `IP:端口`（使用默认名称）
- **示例**：`1.1.1.1:443#香港节点,8.8.8.8:53#Google DNS`
- **默认格式**：未设置名称时自动生成 `自定义优选-IP:端口`

#### 📊 系统状态监控
- **实时检测**：显示Worker地区、检测方式、ProxyIP状态
- **智能匹配**：同地区 → 邻近地区 → 其他地区的选择逻辑
- **状态指示**：可视化显示系统运行状态和配置信息

#### 🔧 高级控制选项
- **订阅转换**：自定义订阅转换 API 地址
- **优选类型**：控制内置优选（域名/IP/GitHub）的启用状态
- **地区匹配控制**：`rm=no` 关闭地区智能匹配
- **降级控制**：`qj=no` 启用降级模式（CF直连失败→SOCKS5→fallback）
- **TLS控制**：`dkby=yes` 只生成TLS节点，不生成非TLS节点
- **优选控制**：`yxby=yes` 关闭所有优选功能
- **路径监控**：实时显示当前使用的路径类型（UUID/自定义路径）

#### 🎨 多客户端支持
- **订阅格式**：支持Clash、Surge、Sing-box、Loon、V2Ray等
- **自动转换**：根据客户端类型自动生成对应配置
- **一键获取**：图形化界面一键生成订阅链接

#### ⚡ 性能优化
- **智能优选**：每15分钟自动优选一次，保持最佳性能
- **容错机制**：多重备用方案，确保服务稳定性
- **缓存优化**：智能缓存机制，减少重复计算

###  致谢

  * 本项目基于 [zizifn/edgetunnel](https://github.com/zizifn/edgetunnel) 修改，感谢原作者的贡献。
  * 本项目内置ProxyIP 来自CM [[cmliu](https://github.com/cmliu)) ，感谢作者的贡献。
  * 本项目反代IP来着前端独苗kejiland  [[qwer-search](https://github.com/qwer-search)) ，感谢作者的贡献。
## Star History

[![Star History Chart](https://api.star-history.com/svg?repos=byJoey/cfnew&type=Timeline)](https://www.star-history.com/#byJoey/cfnew&Timeline&LogScale)


