MCP는 JSON-RPC 2.0 사양을 따른다.

MCP는 세 가지 유형의 메시지를 정의한다

### 요청 메시지
문자열 또는 숫자 타입의 ID를 반드시 포함해야 함
```json
{
  "jsonrpc": "2.0",
  "id": "string | number",
  "method": "string",
  "param?": {
    "key": "value"
  }
}
```
### 응답 메시지
```json
{
  "jsonrpc": "2.0",
  "id": "string | number",
  "result?": {
    "[key: string]": "unknown"
  },
  "error?": {
    "code": "number",
    "message": "string",
    "data?": "unknown"
  }
}
```
### 알림 메시지
응답이 필요없는 단방향 메시지
ID를 포함하지 않는다.
```json
{
  "jsonrpc": "2.0",
  "method": "string",
  "params?": {
    "[key: string]": "unknown"
  }
}
```
