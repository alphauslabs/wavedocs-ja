# Authentication
Blue は認証に API クライアント認証情報を使用します。API 認証情報は、Wave(Pro) の「設定 > API アクセストークン」から生成できます。

## Access token endpoints
以下は、製品固有のアクセストークンを取得するためのエンドポイントです。その後、取得したトークンを `Authorization: Bearer {token}` HTTP ヘッダーに設定し、API を呼び出します。  

なお、アクセストークンは製品間で互換性がありません。Wave(Pro) のアクセストークンは、Wave(Pro) のエンドポイントでのみ有効です。

=== "Wave(Pro)"
    ```
    https://login.alphaus.cloud/access_token
    ```

アクセス トークンを取得するには、以下の形式に従ってアクセス トークン エンドポイントに POST リクエストを送信してください。

**Request**

```
POST {access-token-url} HTTP1.1
Content-Type: multipart/form-data

{body formdata}
```

| **Name** | **Value** |
|---|---|
| `grant_type` | 有効な値: `password`, `client_credentials` |
| `client_id` | Alphaus または API から受け取ったクライアント ID。 |
| `client_secret` | Alphaus または API から受け取ったクライアント シークレット。 |
| `username` | アカウントのユーザー名。`grant_type` が `password` に設定されている場合は必須。 |
| `password` | アカウントのユーザー名。`grant_type` が `password` に設定されている場合は必須。 |
| `scope` | 有効な値: `openid` |

**Response**

``` json
{
  "id_token": "eyJ0eXAiOiJKV1Q...",
  "token_type": "Bearer",
  "expires_in": 86400,
  "access_token": "eyJ0eXAiOiJKV1Q...",
  "refresh_token": "def50200..."
}
```

**Example**

``` sh
$ curl -X POST \
  -F client_id={client-id} \
  -F client_secret={client-secret} \
  -F grant_type=client_credentials \
  -F scope=openid \
  https://login.alphaus.cloud/access_token
```
