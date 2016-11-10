Front-End JSON(P) API Specification
===================================

## HTTP 接口返回格式 (Scheme)

  接口状态码 (HTTP Code): => 200

  接口输出 (Response): JSON => {error, msg, data, ver}

  ```javascript
  {
    "error": ${error_code},        // {String} api status code, defaults to '0',
    "msg": ${api_status_message},  // {String} api status message
    "data": ${api_data},           // {Mixed} Generic type for api data response, can be null, empty "", 0, {}, [] etc,.
    "ver": ${api_version}          // {String} version identify, defaults to '1.0'
  }
  ```

  **`error` 字段状态码定义**

  * 状态码为字符串类型 (String). eg. "0" (注意有双引号)
  * 0 ~ 100 为系统保留状态
  * "-100" -  session timeout (登陆验证)
  * "0"    -  api success (default value)
  * "1"    -  internal error (后端接口未知错误)
  * "2"    -  redirect url (页面跳转响应)
  * **业务状态码建议使用 100以后的数值 (>100)**. eg: 0x101F, 0x1013

  示例:

  ```javascripton
  {"error": "2", "msg": "user session timeout", "data": "http://login.yunhou.com/login.php?t", "ver": "1.0"}
  ```

## 其他

  * 本规范适用所有前后端数据接口规范，json, jsonp
  * 接口输出**必须是规范的 JSON 字符串**, 拒绝手动拼接 json 字符串返回，请参考 <http://json.org/>


## Global config specification

  页面全局 js 配置规范

  > 主要用于前后端数据交互时用到的一些配置项输出，减少一些额外的 HTTP 接口请求，
  >
  > 页面常量，首屏页面 JSON 数据，等都可考虑页面直接输出。
  >
  > 一些影响首屏页面绚染的接口尽量采用内联输出。 比如，获取服务端时间接口等 ...

  ```javascript
  window['$PAGE_DATA'] = {

      // Client time for bpm.js
      startTime: new Date,

      // Diff time with backend
      diffTime: {UNIX_TIMESTAMP} - new Date, // {UNIX_TIMESTAMP} - sync with server unix timestamp

      // Page logic config data, eg. urls etc,.
      data: {
          ....
      }
  };
  ```

  注：业务配置数据建议使用 `$PAGE_DATA.data` 字段, 前端做相应处理。

  附: 后端输出参考:

  ```html
  <script>
  // first declaration
  window['$PAGE_DATA'] || window['$PAGE_DATA'] = {};

  window['$PAGE_DATA']['xxx'] = xxx;

  // 业务全局
  window['$PAGE_DATA']['data'] = {
      ...
  };
  window['$PAGE_DATA']['data']['foo'] = {
      ...
  };
  </script>
  ```
